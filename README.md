# pyscrh

A tiny, **Scratch-style** sprite library built on [pygame](https://www.pygame.org/),
made for people moving from Scratch to Python.

Every Scratch block you know has a short Python equivalent — no game loop,
no event handling, no surface juggling to remember.

## Install

```bash
pip install pyscrh
```

(Requires Python 3.8+ and pygame, which is installed automatically.)

## Quick start

```python
import pyscrh

apple = Sprite("apple.png")     # load an image (PNG with transparency)
apple.turnway = "lr"            # flip left-right when bouncing

def forever():
    apple.move(5)               # walk 5 steps in the current direction
    apple.edgeb()               # bounce off the screen edge
    # stop_all()                # call this anywhere to quit the game

clickGF()                       # start the game
```

Just `import pyscrh` and you're done — `Sprite`, `clickGF`, `stop_all`,
`WIDTH` and `HEIGHT` are available directly, just like `import pgzrun`.

That's it. The apple walks right, bounces off the right edge, walks left,
bounces off the left edge, and repeats.

## Scratch → Python cheat sheet

| Scratch block | pyscrh |
| --- | --- |
| move 10 steps | `sprite.move(10)` |
| turn 15° clockwise | `sprite.turnright(15)` |
| turn 15° counter-clockwise | `sprite.turnleft(15)` |
| go to x: y: | `sprite.gotoxy(x, y)` |
| go to random position | `sprite.goto("r")` |
| go to another sprite | `sprite.goto("other_name")` |
| glide 1 sec to x: y: | `sprite.glidexy(1, x, y)` |
| glide to another sprite | `sprite.glideto(1, "other_name")` |
| point in direction 90 | `sprite.point(90)` |
| point towards mouse | `sprite.pointtow("m")` |
| point towards another sprite | `sprite.pointtow("other_name")` |
| if on edge, bounce | `sprite.edgeb()` |

## How it works

You only write one required function:

- `forever()` — called once every frame, like Scratch's *forever* block.
  Put `sprite.move(...)`, `edgeb()`, etc. in here. No `dt` parameter
  needed; the library handles timing.

An optional `draw()` is also called every frame, *after* all sprites are
drawn, if you want extra drawing (text, backgrounds, etc.).

Call `clickGF()` to start, and `stop_all()` to close the window and quit.
pyscrh handles the window, clock, event loop and rendering for you.

## API reference

### `Sprite(image_path, name="")`

Create a sprite from an image file.

| Method | Description |
| --- | --- |
| `move(steps)` | Walk `steps` pixels in the current direction. |
| `turnright(deg)` / `turnleft(deg)` | Rotate by `deg` degrees. |
| `gotoxy(x, y)` | Jump to `(x, y)`. |
| `goto("r")` / `goto("m")` / `goto("name")` | Jump to a random spot / the mouse / another sprite. |
| `glidexy(sec, x, y)` | Glide to `(x, y)` over `sec` seconds. |
| `glideto(sec, "r")` / `glideto(sec, "m")` / `glideto(sec, "name")` | Glide to a spot / the mouse / another sprite. |
| `point(deg)` | Set direction to `deg` degrees. |
| `pointtow("r")` / `pointtow("m")` / `pointtow("name")` | Face a random direction / the mouse / another sprite. |
| `edgeb()` | Bounce off the screen edge. Works for any `turnway`. |
| `addcos(path, name)` | Add a new costume (look) to the sprite. |
| `switchcos(name)` | Switch the sprite to the costume called `name`. |
| `nextcos()` | Switch to the next costume (wraps around to the first one). |
| `cosnum` | The number (index) of the costume currently shown (first = `0`). |
| `cosname` | The name of the costume currently shown. |
| `cleareff()` | Clear all graphic effects (`color`, `fisheye`, `whirl`, `pix`, `mos`, `bri`, `ghost`) back to `0`. `size` is kept. |
| `show()` | Make the sprite visible again. |
| `hide()` | Hide the sprite (stops drawing it; it still moves and acts). |
| `gotol('front' / 'back')` | Move the sprite to the front or the back layer. |
| `gofl(step)` | Move the sprite `step` layers forward (negative = backward). |
| `gobl(step)` | Move the sprite `step` layers backward (negative = forward). |

| Attribute | Description |
| --- | --- |
| `x`, `y` | Center position (read or write). |
| `angle` / `d` | Direction in degrees (0 = right, 90 = down). |
| `size` | Scale in percent: `100` = original, `50` = half, `200` = double. |
| `color` | Colour effect: shifts the hue of the whole image. `100` = half circle, `200` = full circle (= `0` again). |
| `fisheye` | Fisheye effect: `0` = none, positive bulges like a fish-eye lens, negative pinches inward. |
| `whirl` | Whirl effect: `0` = none, positive spins clockwise, negative counter-clockwise (degrees). |
| `pix` | Pixelate effect: `0` = none, value is the pixel-block size (`5` = 5x5 blocks). |
| `mos` | Mosaic effect: tiles the image into a grid. `0` = none, `25` = 4x4, `50` = 6x6, `100` = 11x11. |
| `bri` | Brightness effect: `0` = normal, `100` = white, `-100` = black. |
| `ghost` | Ghost effect: `0` = solid, higher = more transparent, `100` = invisible. |
| `costumes` | The list of `(name, image)` costumes on this sprite. |
| `turnway` | Only controls which way the *image* faces — it never changes the real direction (`angle` / `d`). `"r"` rotates the image to match the angle, `"lr"` flips it left/right so it faces the direction disc, `"n"` never turns the image. `edgeb()` bouncing works the same for all three. |

Example — a sprite with two looks:

```python
import pyscrh

hero = pyscrh.Sprite("hero.png")        # first look, "costume1"
hero.addcos("hero_run.png", "run")
hero.addcos("hero_jump.png", "jump")

def forever():
    if hero.x > 480:                    # your own condition
        hero.switchcos("run")
    else:
        hero.switchcos("jump")

pyscrh.clickGF()
```

## License

MIT
