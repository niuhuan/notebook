RUST - JSON
===========

## FuzzyDecode 

总有那么一些不正经的接口使用模糊数据类型, javascript不识别长整形长度的数字, 导致了接口有时候会将数字加上双引号, 有时候不加, 这个时候需要去适配

```json
{
  "pageSize": "100000"
}
```
```json

{
  "pageSize": 100000
}
```

```rust
#[derive(Debug, Serialize, Deserialize)]
#[serde(rename_all = "camelCase")]
pub struct PageData<T> {
    #[serde(deserialize_with = "fuzzy_i32")]
    pub page_size: i32,
}
```

```rust
fn fuzzy_i32<'de, D>(d: D) -> std::result::Result<i32, D::Error>
    where
        D: serde::Deserializer<'de>,
{
    let value: serde_json::Value = serde::Deserialize::deserialize(d)?;
    if value.is_i64() {
        Ok(value.as_i64().unwrap() as i32)
    } else if value.is_string() {
        let str = value.as_str().unwrap();
        let from: std::result::Result<i32, ParseIntError> = std::str::FromStr::from_str(str);
        match from {
            Ok(from) => Ok(from),
            Err(_) => Err(serde::de::Error::custom("parse error")),
        }
    } else {
        Err(serde::de::Error::custom("type error"))
    }
}
```

## 自定义序列化反序列化

```rust
macro_rules! enum_str {
    ($name:ident { $($variant:ident($str:expr), )* }) => {
        #[derive(Clone, Copy, Debug, Eq, PartialEq)]
        pub enum $name {
            $($variant,)*
        }

        impl ::serde::Serialize for $name {
            fn serialize<S>(&self, serializer: S) -> Result<S::Ok, S::Error>
                where S: ::serde::Serializer,
            {
                // 将枚举序列化为字符串。
                serializer.serialize_str(match *self {
                    $( $name::$variant => $str, )*
                })
            }
        }

        impl<'de> ::serde::Deserialize<'de> for $name {
            fn deserialize<D>(deserializer: D) -> Result<Self, D::Error>
                where D: ::serde::Deserializer<'de>,
            {
                struct Visitor;

                impl<'de> ::serde::de::Visitor<'de> for Visitor {
                    type Value = $name;

                    fn expecting(&self, formatter: &mut ::std::fmt::Formatter) -> ::std::fmt::Result {
                        write!(formatter, "a string for {}", stringify!($name))
                    }

                    fn visit_str<E>(self, value: &str) -> Result<$name, E>
                        where E: ::serde::de::Error,
                    {
                        match value {
                            $( $str => Ok($name::$variant), )*
                            _ => Err(E::invalid_value(::serde::de::Unexpected::Other(
                                &format!("unknown {} variant: {}", stringify!($name), value)
                            ), &self)),
                        }
                    }
                }

                // 从字符串反序列化枚举。
                deserializer.deserialize_str(Visitor)
            }
        }
    }
}

enum_str!(Sort {
    New("mr"),
    View("mv"),
});
```

## 打印出json出错的位置

```shell
/// FROM STRING 并打印出错的位置
pub fn from_str<T: for<'de> serde::Deserialize<'de>>(json: &str) -> Result<T> {
    Ok(serde_path_to_error::deserialize(
        &mut serde_json::Deserializer::from_str(json),
    )?)
}
```
