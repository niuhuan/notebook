
实测`20_000u32`也可以初始化SD卡

```rust
#![no_std]
#![no_main]

use embedded_sdmmc::sdcard::AcquireOpts;
use esp_backtrace as _;
use esp_println::println;
use hal::spi::SpiMode;
use hal::{
    clock::ClockControl, peripherals::Peripherals, prelude::*, timer::TimerGroup, Delay, Rtc, IO,
};

#[entry]
fn main() -> ! {
    let peripherals = Peripherals::take();
    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    let mut system = peripherals.DPORT.split();
    let clocks = ClockControl::boot_defaults(system.clock_control).freeze();
    // Disable the RTC and TIMG watchdog timers
    let mut rtc = Rtc::new(peripherals.RTC_CNTL);
    let timer_group0 = TimerGroup::new(
        peripherals.TIMG0,
        &clocks,
        &mut system.peripheral_clock_control,
    );
    let mut wdt0 = timer_group0.wdt;
    let timer_group1 = TimerGroup::new(
        peripherals.TIMG1,
        &clocks,
        &mut system.peripheral_clock_control,
    );
    let mut wdt1 = timer_group1.wdt;
    rtc.rwdt.disable();
    wdt0.disable();
    wdt1.disable();
    println!("Hello world!");
    let mut spi = hal::spi::Spi::new_no_cs(
        peripherals.SPI2,
        io.pins.gpio18,
        io.pins.gpio23,
        io.pins.gpio19,
        100u32.kHz(),
        SpiMode::Mode0,
        &mut system.peripheral_clock_control,
        &clocks,
    );
    let sdcard = embedded_sdmmc::SdCard::new_with_options(
        spi,
        io.pins.gpio4.into_push_pull_output(),
        Delay::new(&clocks),
        AcquireOpts { use_crc: true },
    );
    println!("sdcard init : {:?}", sdcard.get_card_type());
    let mut volume_mgr = embedded_sdmmc::VolumeManager::new(sdcard, FakeTimesource {});
    println!("sdcard init : {:?}", volume_mgr.has_open_handles());
    let mut volume0 = volume_mgr.get_volume(embedded_sdmmc::VolumeIdx(0)).unwrap();
    println!("volume0 init : {:?}", volume0);
    drop(volume0);
    drop(volume_mgr);
    loop {}
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