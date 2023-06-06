
```rust
fn main() {
    let mut http_server = EspHttpServer::new(&http::server::Configuration::default())?;
    http_server.fn_handler("/", Method::Get, http_index)?;
}


fn http_index(request: Request<&mut EspHttpConnection>) -> Result<(), HandlerError> {
    let host = request.host().unwrap_or_default().to_string();
    let uri = request.uri().to_string();
    let mut response = request.into_ok_response()?;
    response.write_all("<br />HOST : ".as_bytes())?;
    response.write_all(host.as_bytes())?;
    response.write_all("<br /> URI : ".as_bytes())?;
    response.write_all(uri.as_bytes())?;
    Ok(())
}
```