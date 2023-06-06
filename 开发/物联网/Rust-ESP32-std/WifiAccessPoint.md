
```rust
fn main() {
    let peripherals = Peripherals::take().unwrap();
    let sysloop = EspSystemEventLoop::take()?;
    let nvs = EspDefaultNvsPartition::take().unwrap();
    let mut wifi = EspWifi::new(peripherals.modem, sysloop, Some(nvs))?;
    wifi.set_configuration(&wifi::Configuration::AccessPoint(
        AccessPointConfiguration {
            ssid: "mix_ssid".into(),
            ssid_hidden: false,
            channel: 0,
            secondary_channel: None,
            auth_method: AuthMethod::WPA2Personal,
            password: "11223344".into(),
            max_connections: 10,
            ..Default::default()
        },
    ))?;
    wifi.start()?;
}
```