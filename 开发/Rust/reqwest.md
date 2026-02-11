


## 自定义解析 (1)
```rust
    let host = "a.b.c";
    let ip = Ipv4Addr::new(1, 2, 3, 4);

    let client = reqwest::Client::builder()
        .http3_prior_knowledge()
        .resolve(host, SocketAddr::new(IpAddr::V4(ip), 443))
        // ignore the certificate error
        .danger_accept_invalid_certs(true)
        .build()?;
```

## 自定义解析 (2)
```rust

/// 支持的域名列表
pub(crate) const HANIME_HOSTNAMES: &[&str] =
    &["domain.com"];

/// 内置的 Cloudflare IP
const CLOUDFLARE_IPS: &[&str] = &[
    "172.64.229.154",
    "104.25.254.167",
    "172.67.75.184",
    "104.21.7.20",
    "172.67.187.141",
];



/// 自定义 DNS 解析器
struct CustomDnsResolver;

impl Resolve for CustomDnsResolver {
    fn resolve(&self, name: Name) -> Resolving {
        let name_str = name.as_str().to_string();

        Box::pin(async move {
            let active_domain = get_active_domain();
            if active_domain.use_custom_dns
                && HANIME_HOSTNAMES
                    .iter()
                    .any(|h| name_str == *h || name_str.ends_with(&format!(".{}", h)))
            {
                tracing::info!("Using built-in IPs for {}", name_str);

                let addrs: Vec<SocketAddr> = CLOUDFLARE_IPS
                    .iter()
                    .filter_map(|ip| ip.parse::<IpAddr>().ok())
                    .map(|ip| SocketAddr::new(ip, 0))
                    .collect();

                if !addrs.is_empty() {
                    return Ok(
                        Box::new(addrs.into_iter()) as Box<dyn Iterator<Item = SocketAddr> + Send>
                    );
                }
            }

            tracing::debug!("Using system DNS for {}", name_str);

            let addrs = tokio::net::lookup_host(format!("{}:0", name_str))
                .await
                .map_err(|e| -> Box<dyn std::error::Error + Send + Sync> { Box::new(e) })?
                .collect::<Vec<_>>();

            Ok(Box::new(addrs.into_iter()) as Box<dyn Iterator<Item = SocketAddr> + Send>)
        })
    }
}



/// 获取 HTTP 客户端
pub fn get_client() -> &'static Client {
    CLIENT.get_or_init(|| {
        let jar = get_cookie_jar();

        Client::builder()
            .cookie_store(true)
            .cookie_provider(jar)
            .timeout(Duration::from_secs(30))
            .dns_resolver(Arc::new(CustomDnsResolver))
            .build()
            .expect("Failed to create HTTP client")
    })
}

```


### http3
curl -k --http3 https://host/123 -v
```rust
#![deny(warnings)]

use tracing_subscriber::FmtSubscriber;
use tracing::Level;

fn init_logger() {
    let subscriber = FmtSubscriber::builder()
        .with_max_level(Level::TRACE)
        .finish();
    tracing::subscriber::set_global_default(subscriber).expect("Failed to set subscriber");
}

// This is using the `tokio` runtime. You'll need the following dependency:
//
// `tokio = { version = "1", features = ["full"] }`
#[tokio::main]
async fn main() -> Result<(), reqwest::Error> {

    init_logger();

    let client = reqwest::Client::builder()
        .http3_prior_knowledge()
        .danger_accept_invalid_certs(true)
        .build()?;

    // Some simple CLI args requirements...
    let url = match std::env::args().nth(1) {
        Some(url) => url,
        None => {
            println!("No CLI URL provided, using default.");
            format!("https://host/123").into()
        }
    };

    eprintln!("Fetching {url:?}...");

    let res = client
        .get(url)
        .version(http::Version::HTTP_3)
        .send()
        .await?;

    eprintln!("Response: {:?} {}", res.version(), res.status());
    eprintln!("Headers: {:#?}\n", res.headers());

    let body = res.text().await?;

    println!("{body}");

    Ok(())
}

```
