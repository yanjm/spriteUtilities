Pixi Sprite Utilities
=====================

This repository contains a bunch of useful functions for creating
[Pixi](https://github.com/GoodBoyDigital/pixi.js/) sprites and making them easier to work with.

Setting up and initializing `SpriteUtilities`
-------------------------------------------

Create a new instance of `SpriteUtilities` like this:
```js
let u = new SpriteUtilities(PIXI);
```
Supply a reference to `PIXI` as the optional argument in the
constructor. (If you don't supply it, `SpriteUtilites` will look for a
global `PIXI` object and alert you with an error if it can't find it.)

You can now access the `SpriteUtilites` instance and all its
methods using the variable reference `u`.

The `sprite` function
-------------------

Use the universal `sprite` function to make any kind of Pixi sprite.
```js
let anySprite = u.sprite(frameTextures, xPosition, yPosition);
```
The first argument, `frameTextures` can be any of the following: 

-	A single PNG image string.
-	A Pixi `Texture` object.
-	An array of texture atlas frame ids.
-	An array of single PNG image strings.
-	An array of Pixi `Texture` objects.

You can essentially throw anything at it, and it will give you back a sprite that works as it should, 
depending on the kind of texture information you've supplied. That
means you can use the `sprite` function 
as your one-stop-shop for creating any kind of sprite. Forget about
using Pixi's `Sprite` and `MovieClip` 
classes to make sprites and just use the `sprite` function for everything!

If you supply the `sprite` function with an array, it will return a
`MovieClip` sprite but with a bonus 
**state player**  built into it. The state player is just a collection of 4 properties and methods 
that make it easy to control sprite animation states. Here they are:

1. `fps`: A property to set the precise animation speed, as
frames-per-second. Its default value is 12. The `fps` is not linked to
the renderer's fps, and that means you can have sprite animations
playing at speeds that are independent of the game or application
speed. `anySprite.fps = 6`.

2. `playAnimation`: A method to play the sprite's
animation.`anySprite.playAnimation()`. You can supply it with start and end frame values if you want to play a sub-set of frames. Here's how: `anySprite.playAnimation([startFrame, endFrame])` The animation will play in a loop, by default, unless you set the sprite's `loop` property value to `false`.

3. `stopAnimation`: A method that stops the sprite's animation at the
current frame. `anySprite.stopAnimation()`.

4. `show`: A method that displays a specific frame number.
`anySprite.show(frameNumber)`.

5. `animating`: A Boolean property that will be `true` if the
animation is playing and `false` if it isn't.

`filmstrip`
----------

Use the`filmstrip` function to automatically turn a tileset PNG image
into an array of textures that you can use to make a sprite.
```js
u.filmstrip("anyTilesetImage.png", frameWidth, frameHeight, optionalPadding);
```
Supply `filmstrip` with the tileset image name and the width and
height of each frame. If there's padding around each frame, supply the
padding amount, in pixels. `filmstrip` returns an array of frames that
you can use to make an animated `MovieClip` sprite. 
Here's how you could use `filmstrip` with the universal `sprite`
function to quickly make a sprite with multiple frames: 
```js
let textures = u.filmstrip("tileset.png", 32, 32);
let anySprite = u.sprite(textures);
```
The `filmstrip` function automatically loads every frame from a tileset image into the sprite.

`frames`
-------

But what if you only want to use a sub-set of frames from the tileset,
not all of them? Use another utility function called `frames`. The
`frames` function takes 4 arguments: the texture, a 2D array of x/y
frame position coordinates, and the width and height of each frame.
Here's how you could use the frames function to create a sprite.
```js
let textures = u.frames(
  "tileset.png",             //The tileset image
  [[0,0],[32,0],[64,0]],     //A 2D array of x/y frame coordianates
  32, 32                     //The width and height of each frame
);
let anySprite = u.sprite(textures);
```
Use the `frames` function whenever you need to create a sprite using
selected frames from a larger tileset PNG image.

`frame`
-------

Use the `frame` function if you just want to create a texture using a smaller
rectangular section of a larger image. The `frame` function takes
four arguments: the image, the sub-image x position, the sub-image y
position, and the sub-image's width and height.
```
u.frame("image.png", x, y, width, height)
```
Here's how you could make a sprite using a sub-image of a larger
image. 
```js
let texture = u.frame("tileset.png", 64, 128, 32, 32);
let anySprite = u.sprite(texture);
```
Use the `frame` function to
[blit](https://en.wikipedia.org/wiki/Bit_blit) a smaller image from bigger image.

`frameSeries`
------------

If you've loaded a texture atlas and want a sequence of numbered frame
ids to create an animated sprite, use the `frameSeries` function.
Imagine that you have frames in a texture atlas with the following id
names:
```js
frame0.png
frame1.png
frame2.png
```
To create a sprite in Pixi using these frames, you would ordinarily
write some code using Pixi's `MovieClip` class
(`PIXI.extras.MovieClip`) that looks something like this:
```js
let frameTextures = ["frame0.png", "frame1.png", "frame2.png"];
let anySprite = MovieClip.fromFrames(frameTextures);
```
You now have a sprite with 3 frames that you can control. That's not too painful, but what if you
had 100 animation frames? You definitely don't want to manually type in
100 frame id's into an array. Instead, use the `frameSeries` function.

`frameSeries` takes four arguments: the start frame sequence number,
the end frame sequence number, the optional base file name, and the optional file extension.
You could use the `frameSeries` function to create the sprite from the
texture atlas frame ids like this:
```
let frameTextures = u.frameSeries(0, 2, "frame", ".png");
let anySprite = u.sprite(frameTextures);
```
If you had 100 animation frames, your code might look like this:
```
let frameTextures = u.frameSeries(0, 99, "frame", ".png");
let anySprite = u.sprite(frameTextures);
```
That's much better!







