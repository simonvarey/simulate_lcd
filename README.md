# simulate_lcd <!-- [![](https://img.shields.io/crates/v/simulate_lcd.svg)](https://crates.io/crates/simulate_lcd) --> <!-- [![](https://docs.rs/simulate_lcd/badge.svg)](https://docs.rs/simulate_lcd) -->

A simple library to simulate dot-matrix displays, such as monochrome LCD screens.

<!-- [Documentation](https://docs.rs/simulate_lcd) -->

<!-- ## Overview -->

## Example <!-- 'Example Usage' -->

```
use std::{thread::sleep, time::Duration};

use rand::{thread_rng, Rng};
use sdl2::event::Event;
use simulate_lcd::{Bitmap, LcdScreen, LCD_DARK_GREEN, LCD_LIGHT_GREEN};

fn main() {
    let sdl_context = sdl2::init().unwrap();
    let mut screen = LcdScreen::<64, 96>::new(
        &sdl_context,
        "LCD Demo: Random",
        LCD_DARK_GREEN,
        LCD_LIGHT_GREEN,
        10,
        10,
    )
    .unwrap();

    let mut event_pump = sdl_context.event_pump().unwrap();
    'running: loop {
        for event in event_pump.poll_iter() {
            match event {
                Event::Quit { .. } => break 'running,
                _ => {}
            }
        }

        let mut rng = thread_rng();
        let random_bits: Vec<[bool; 96]> = (0..64).map(|_| rng.gen()).collect();
        screen.draw_bitmap(&random_bits.try_into().unwrap()).unwrap();

        sleep(Duration::new(0, 1_000_000_000u32 / 60));
    }
}
```

More examples can be found in the examples folder.

## Usage

`LcdScreen` is the main type provided by this crate. To create new simulated screen window with `R` rows and `C` columns of dots, use the function `LcdScreen::<R, C>::new`, with the following parameters:

- `sdl_context`: an `Sdl` context object 
- `title`: the window title
- `on_color`: the color of a dot when it is 'on'. e.g. (near) black on a backlight LCD screen
- `off_color`: the color of a dot when it is 'off'. e.g. green on a green backlight LCD screen
- `dot_width`: the width of a dot in pixels of the actual window
- `dot_height`: the height of a dot in pixels of the actual window

The screen will disappear as soon as the `LcdScreen` object is dropped, including at the end of the scope it was created. Use a loop, or some other device, to stop the screen object from being dropped. 

New images can be drawn to the screen using the `draw_bitmap` method. `draw_bitmap` takes any object which can be converted into a `[[bool; C]; R]` array. Each `true` in this row-major array represents a dot that is 'on'. simulate_lcd offers `Bitmap<C, R>` as a convenient alias for `[[bool; C]; R]`.

The 'on' and 'off' colors of the screen are [`sdl2::pixels::Color`](https://rust-sdl2.github.io/rust-sdl2/sdl2/pixels/struct.Color.html) objects. They can be created from RGB values with the `sdl2::pixels::Color::RGB` function. simulate_lcd offers the `LCD_DARK_GREEN` and `LCD_LIGHT_GREEN` constants from simulating green backlight LCD screens.

## Setup

simulate_lcd is built around the [sdl2](https://crates.io/crates/sdl2) crate. A new `LcdScreen` requires an `Sdl` context object created by the `sdl2::init()` function. Note that sdl2 may require further setup than just adding the crate. See the [sdl2 README](https://github.com/Rust-SDL2/rust-sdl2/blob/master/README.md#requirements) for details.
<!-- from the [sdl2](https://crates.io/crates/sdl2) crate.  -->

<!-- ## Examples


License

Licensed under either of

    Apache License, Version 2.0 (LICENSE-APACHE or http://www.apache.org/licenses/LICENSE-2.0)
    MIT license (LICENSE-MIT or http://opensource.org/licenses/MIT) at your option.

Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you shall be dual licensed as above, without any additional terms or conditions.
Dependencies

~0???6.5MB
~105K SLoC

    unix nix 0.26+fs+signal
    windows windows-sys 0.42+Win32_Foundation+Win32_System_Threadi???+Win32_Security+Win32_System_Windows???+Win32_System_Console


-->