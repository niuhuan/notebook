
从github得知`400khz`才能初始化SD卡， 实测800khz也可以。这可能是信致春秋C001的问题。

core2，20_000khz也可以

```rust
use anyhow::Result;
use embedded_sdmmc::SdCard;
use embedded_svc::http::server::{HandlerError, Request};
use embedded_svc::http::Headers;
use embedded_svc::io::Write;
use esp_idf_hal::delay::{Ets, FreeRtos};
use esp_idf_hal::gpio::{Gpio0, PinDriver};
use esp_idf_hal::i2c;
use esp_idf_hal::i2c::I2cDriver;
use esp_idf_hal::prelude::{Hertz, Peripherals};
use esp_idf_hal::spi::config::{BitOrder, Duplex};
use esp_idf_hal::spi::Dma;
use esp_idf_svc::http::server::EspHttpConnection;
use std::pin::Pin;
use std::time::Duration;

use crate::define::{
    SCREEN_HEIGHT, SCREEN_HEIGHT_U32, SCREEN_WIDTH, SCREEN_WIDTH_U32, TOUCH_HEIGHT, TOUCH_WIDTH,
};


fn main() {
    run().unwrap();
}

fn run() -> Result<()> {
    // It is necessary to call this function once. Otherwise some patches to the runtime
    // implemented by esp-idf-sys might not link properly. See https://github.com/esp-rs/esp-idf-template/issues/71
    esp_idf_sys::link_patches();
    // Bind the log crate to the ESP Logging facilities
    esp_idf_svc::log::EspLogger::initialize_default();
    // log start
    info!("started!");

    let mut spi_d_config = esp_idf_hal::spi::config::DriverConfig::default();
    let spi_d = esp_idf_hal::spi::SpiDriver::new(
        peripherals.spi2,
        peripherals.pins.gpio18,       // sclk
        peripherals.pins.gpio23,       // sdo
        Some(peripherals.pins.gpio19), // sdi
        &spi_d_config,
    )
    .expect("panic spi_d");

    // sdc

    let mut spi_dd_config = esp_idf_hal::spi::SpiConfig::default();
    spi_dd_config.baudrate = Hertz(1_000_000);
    spi_dd_config.duplex = Duplex::Full;
    let sd_spi_dd = esp_idf_hal::spi::SpiDeviceDriver::new(&spi_d, None::<Gpio0>, &spi_dd_config)?;
    let sdcard = SdCard::new(
        sd_spi_dd,
        PinDriver::output(&mut peripherals.pins.gpio4)?,
        SdDelayer,
    );
    info!("sdcard init : {:?}", sdcard.get_card_type());
    let mut volume_mgr = embedded_sdmmc::VolumeManager::new(sdcard, FakeTimesource {});
    let mut volume0 = volume_mgr.get_volume(embedded_sdmmc::VolumeIdx(0)).unwrap();
    info!("volume0 init : {:?}", volume0);
    drop(volume0);
    drop(volume_mgr);

}

struct SdDelayer;

impl embedded_hal::blocking::delay::DelayUs<u8> for SdDelayer {
    fn delay_us(&mut self, us: u8) {
        Ets::delay_us(us as _)
    }
}

struct FakeTimesource();

impl embedded_sdmmc::TimeSource for FakeTimesource {
    fn get_timestamp(&self) -> embedded_sdmmc::Timestamp {
        embedded_sdmmc::Timestamp {
            year_since_1970: 0,
            zero_indexed_month: 0,
            zero_indexed_day: 0,
            hours: 0,
            minutes: 0,
            seconds: 0,
        }
    }
}

```