# Touch Emulator

> Forked from [hammerjs/touchemulator](https://github.com/hammerjs/touchemulator), add the attribute `data-emulate-touch` to control certain DOM can trigger the touch emulation.

## Install

Download the script from this repo, via NPM:

```bash
npm install git+https://github.com/AkatQuas/touchemulator
```

## How to use

Include the javascript file, and call the `TouchEmulator()` function before any other libraries that do something with the
touch input. It will set some fake properties to spoof the touch detection of some libraries, and triggers `touchstart`, `touchmove` and `touchend` events on the mouse target.

```html
<div data-emulate-touch="on">This dom can trigger touch-related event</div>

<div data-emulate-touch="off">This dom can NOT trigger touch-related event</div>

<div>This dom can NOT trigger touch-related event</div>

<script src="touch-emulator.js"></script>
<script>
  // mannually invoke the function
  TouchEmulator();
</script>
```

```js
function log(ev) {
  console.log(ev);
}

document.body.addEventListener('touchstart', log, false);
document.body.addEventListener('touchmove', log, false);
document.body.addEventListener('touchend', log, false);
```

Also, the script includes polyfills for `document.createTouch` and `document.createTouchList`.

## How it works

It listens to the `mousedown`, `mousemove` and `mouseup` events, and translates them to touch events. If the mouseevent
has the `shiftKey` property to `true`, it enables multi-touch.

The script also prevents the following mouse events on the page:
`mousedown`, `mouseenter`, `mouseleave`, `mousemove`, `mouseout`, `mouseover` and `mouseup`.

## Web platform tests

The script has been tested with the [w3c web platform tests](/tests/web-platform-tests/touch-events) and passes all tests, except these;

- _assert_true: event is a TouchEvent event expected true got false_
  - We trigger an event of the type `Event`
- _assert_equals: touch list is of type TouchList expected "[object TouchList]" but got "[object Array]"_
- _assert_equals: touch is of type Touch expected "[object Touch]" but got "[object Object]"_

## Options

You might change some properties before invoking the function.

#### TouchEmulator.template = Function(touch)

Change the css properties of the rendered touches.

#### TouchEmulator.multiTouchOffset = 75

The distance between the two touch points when entering the _multi-touch zone_.
