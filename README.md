# pointer-event

### Mouse/Touch input event detection for mpv

Low latency detection of single-click, double-click, long-click and horizontal/vertical dragging.
Each event is detected in a mutually exclusive manner, so e.g. a double-click won't also detect single-click events.

Dragging will additionally emit a start and stop event, as well as a generic dragging event that can be used for detecting more complicated gestures.

Beware that on Wayland in windowed mode with window dragging enabled (enabled by default) the left mouse button won't detect single-click and long-click events (because of [reasons](https://github.com/mpv-player/mpv/issues/9771#issuecomment-1272605271)). They will still work with touch and when in fullscreen or maximized.

## Installation

1. Save [pointer-event.lua](https://github.com/christoph-heinrich/mpv-pointer-event/raw/master/pointer-event.lua) to your [scripts directory](https://mpv.io/manual/stable/#script-location).
2. Configure the events you want to listen to in your `pointer-event.conf`

## Usage

Upon detecting an event the corresponding [command](https://mpv.io/manual/master/#list-of-input-commands) from the `pointer-event.conf` in your script-opts directory (next to the scripts directory, create if it doesn't exist) gets executed.

The command configuration follows the `<button>_<event_type>=command` scheme.

The available buttons are
```
left
right
mid
```

Each of those can listen to the following event types
```
single
double
long
drag_start
drag_end
drag
drag_horizontal
drag_vertical
```

`drag_start` appends ` horizontal` or ` vertical` to the command.
`drag`, `drag_horizontal` and `drag_vertical` append the change in pointer position to the command. ` dx dy` for `drag` and only one of those for the other two.

Additionally the options `long_click_time`, `double_click_time` and `drag_distance` are available. They all have sensible default values with `double_click_time` following [input-doubleclick-time](https://mpv.io/manual/master/#options-input-doubleclick-time). Times are interpreted as milliseconds.

Beware of conflicts with mpvs built-in key bindings as well as your key configuration in `input.conf`.

## Example

[touch.lua](https://github.com/christoph-heinrich/mpv-pointer-event/blob/master/touch.lua) and [pointer-event.conf](https://github.com/christoph-heinrich/mpv-pointer-event/blob/master/pointer-event.conf) are an example for a configuration that's useful on touchscreens.  
It is meant to be used with [uosc](https://github.com/tomasklaen/uosc) because mpv doesn't have a graphical menu available by default.

Single click/tap pauses/unpauses the video.  
Long click/tap opens the menu.  
Drag/swipe vertical to change volume.  
Drag/swipe vertical to seek.  