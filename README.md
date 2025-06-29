## awesome.battery-widget

Battery indicator widget for Awesome Window Manager.

![Screenshot](/screenshot.png?raw=true "Screenshot")

Displays status information from `/sys/class/power_supply`.

### Installation

Clone the repository into your Awesome config folder:

```bash
cd ~/.config/awesome
git clone https://github.com/deficient/battery-widget.git
```

Optionally, in order to receive status updates, you will also need `acpid`:

```bash
pacman -S acpid
systemctl enable --now acpid
```

## Usage


In order to add a battery widget to your wibox, you have to import the module and then instanciate a widget with the desired options like this:

```Lua
-- Import module:
local battery_widget = require("battery-widget")

-- Instantiate and add widget to the wibox:
s.mywibox:setup {
    ...,
    { -- Right widgets
        ...,
        battery_widget {
            -- pass options here
        },
    },
}
```
If you pass an adapter name using the adapter = "..." option, a widget for that specific battery adapter will be instanciated. If the adapter option is not specified, the call will return a table containing widgets for each of the battery adapters in /sys/class/power_supply. In that case if there are no batteries an empty table will be returned and no error will occur on machines without batteries.


### Options

- `adapter`: Directory entry in `/sys/class/power_supply` for the battery adapter.
- `ac`: Directory entry in `/sys/class/power_supply` for AC status.
- `ac_prefix`: Prefix for `${AC_BAT}` when on AC power (e.g., "🔌").
- `battery_prefix`: Prefix for `${AC_BAT}` when on battery (e.g., "🔋"). Can be a table for different percentages.
- `percent_colors`: Color changes for percentage levels (e.g., `{100, "green"}`).
- `listen`: Enable updates via `acpi_listen`.
- `timeout`: Update interval in seconds.
- `widget_text, tooltip_text`: Text displayed on the toolbar and tooltip.
- `widget_font`: Font for the widget text (default if unspecified).
- `alert_threshold`: Max percentage for alerts (`-1` to disable).
- `alert_timeout`: Time after which alerts expire (`0` for no timeout).
- `alert_title, alert_text, alert_icon`: Notification title, body text, and image path.
- `warn_full_battery`: Show notification when fully charged.
- `full_battery_icon`: Image path for full battery notification.

```Lua
battery_widget {
    ac = "AC",
    adapter = "BAT0",
    ac_prefix = "AC: ",
    battery_prefix = "Bat: ",
    percent_colors = {
        { 25, "red"   },
        { 50, "orange"},
        {999, "green" },
    },
    listen = true,
    timeout = 10,
    widget_text = "${AC_BAT}${color_on}${percent}%${color_off}",
    widget_font = "Deja Vu Sans Mono 16",
    tooltip_text = "Battery ${state}${time_est}\nCapacity: ${capacity_percent}%",
    alert_threshold = 5,
    alert_timeout = 0,
    alert_title = "Low battery!",
    alert_text = "${AC_BAT}${time_est}",
    alert_icon = "~/Downloads/low_battery_icon.png",
    warn_full_battery = true,
    full_battery_icon = "~/Downloads/full_battery_icon.png",
}
```
`ac_prefix`, `battery_prefix` and `widget_text` can be further customized with spans to specify colors or fonts.
example : 
```Lua
battery_widget {
    -- Use different colors for ac_prefix and battery_prefix
    ac_prefix = '<span color="red">AC: </span>',
    battery_prefix = '<span color="green">Bat: </span>',

    -- Use a bold font for both prefixes (overrides widget_font)
    widget_text = '<span font="Deja Vu Sans Bold 16">${AC_BAT}</span>${color_on}${percent}%${color_off}'
}
```


### _Requirements_
> - [awesome 4.0](http://awesome.naquadah.org).
> - `acpid` (optional)