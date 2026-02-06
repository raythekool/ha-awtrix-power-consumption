# AWTRIX Power Consumption Display ⚡

[![GitHub Release](https://img.shields.io/github/v/release/Raythekool/ha-awtrix-power-consumption?style=flat-square)](https://github.com/Raythekool/ha-awtrix-power-consumption/releases)
[![GitHub License](https://img.shields.io/github/license/Raythekool/ha-awtrix-power-consumption?style=flat-square)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/Raythekool/ha-awtrix-power-consumption?style=flat-square)](https://github.com/Raythekool/ha-awtrix-power-consumption/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/Raythekool/ha-awtrix-power-consumption?style=flat-square)](https://github.com/Raythekool/ha-awtrix-power-consumption/issues)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Blueprint-41BDF5?style=flat-square&logo=home-assistant)](https://www.home-assistant.io/)
[![AWTRIX](https://img.shields.io/badge/AWTRIX-3-orange?style=flat-square)](https://blueforcer.github.io/awtrix3/)

Home Assistant Blueprint to display real-time household power consumption on your AWTRIX 3 LED matrix with dynamic color-coded display based on consumption levels.

![Power Display Example](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9qjudo87ha9ntscsjplz.gif)

## ✨ Features

- 🚦 **Dynamic color coding** - Green (low), Orange (medium), Red (high)
- ⚡ **Real-time updates** - Instant response to power changes
- 🎨 **Multiple icon styles** - Lightning bolt, plug, or home power
- 📊 **Auto unit conversion** - Displays W or kW automatically
- 🔧 **Fully configurable** - Custom thresholds and preferences
- 📱 **Multi-device support** - Send to multiple AWTRIX displays
- 🔄 **Fallback updates** - Time-based refresh every minute

---

## ⚠️ Requirements

1. **Home Assistant** 2024.6.0 or newer
2. **AWTRIX 3** device (Ulanzi TC001 or compatible) configured via MQTT
3. **Power monitoring sensor** with `device_class: power` and unit in Watts (W)

### Compatible Power Sensors

- ✅ **Shelly** (EM, 1PM, Plus 1PM, Pro 3EM)
- ✅ **Emporia VUE** energy monitor
- ✅ **Zigbee smart plugs** (SONOFF, Tuya, IKEA, Aqara)
- ✅ **Riemann Sum** integration for total consumption
- ✅ **Template sensors** combining multiple sources
- ✅ Any sensor with `device_class: power` and `unit_of_measurement: W`

---

## 📦 Installation

### Quick Import (Recommended)

Click the button below to import the blueprint directly into Home Assistant:

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FRaythekool%2Fha-awtrix-power-consumption%2Fblob%2Fmain%2Fawtrix_power_consumption.yaml)

### Manual Installation

1. Download [`awtrix_power_consumption.yaml`](https://github.com/Raythekool/ha-awtrix-power-consumption/blob/main/awtrix_power_consumption.yaml)
2. Copy to `config/blueprints/automation/` in your Home Assistant directory
3. Restart Home Assistant or reload automations
4. Go to **Settings** → **Automations & Scenes** → **Blueprints**

---

## 🚀 Quick Start

### 1. Create Automation

1. Go to **Settings** → **Automations & Scenes**
2. Click **Create Automation** → **Use a blueprint**
3. Select **AWTRIX Power Consumption Display ⚡**

### 2. Basic Configuration

| Parameter | Example Value | Description |
|-----------|---------------|-------------|
| **AWTRIX Device** | Ulanzi TC001 | Your AWTRIX display |
| **Power Sensor** | `sensor.house_power` | Entity with device_class: power |
| **App Name** | `power_home` | Unique identifier |
| **Display Unit** | Auto | W, kW, or Auto |
| **Low Threshold** | 500W | Below = green |
| **High Threshold** | 2000W | Above = red |
| **Icon Mode** | Lightning Bolt | Visual style |

### 3. Save and Enjoy!

Your AWTRIX display will now show real-time power consumption with color-coded indicators.

---

## 📊 Display Examples

### Color Coding

| Consumption | Display | Color | Meaning |
|-------------|---------|-------|----------|
| 250W | `250W` | 🟢 Green | Low / Standby |
| 1.5kW | `1.5kW` | 🟠 Orange | Normal usage |
| 4.2kW | `4.2kW` | 🔴 Red | High consumption |

### Unit Conversion (Auto Mode)

- **< 1000W** → Displays as Watts: `850W`
- **≥ 1000W** → Displays as Kilowatts: `1.85kW`

---

## 🎨 Icon Options

Choose from three icon styles (no upload needed - built into LaMetric library):

| Icon Mode | LaMetric ID | Description | Best For |
|-----------|-------------|-------------|----------|
| **Lightning Bolt** | `44432` | Animated bolt | Dynamic, eye-catching |
| **Plug** | `6747` | Static plug | Minimalist |
| **Home Power** | `49866` | House + lightning | Home context |

---

## 🔧 Configuration Options

### Display Unit

- **Auto** (default): Shows W below 1000W, kW above
- **Watts (W)**: Always displays in watts
- **Kilowatts (kW)**: Always displays in kilowatts

### Thresholds

#### Recommended Settings by Home Type

| Home Type | Low Threshold | High Threshold |
|-----------|---------------|----------------|
| Apartment | 200W | 1500W |
| Small House | 500W | 3000W |
| Large House | 800W | 5000W |
| With EV Charger | 1000W | 8000W |

#### Calibration Tips

1. Monitor your power for a few days
2. Set **Low** = typical standby + 20%
3. Set **High** = typical peak - 20%

### Lifetime

- **0 seconds** (default): Always visible
- **> 0 seconds**: Auto-cycles with other apps

---

## 📝 Example Scenarios

### Scenario 1: Single Household Monitor

```yaml
Power Sensor: sensor.shelly_em_power
App Name: power_total
Low Threshold: 500W
High Threshold: 3000W
Display Unit: Auto
Icon Mode: Lightning Bolt
```

### Scenario 2: Multi-Room Monitoring

Create separate automations for each room:

| Room | Sensor | App Name | Low | High |
|------|--------|----------|-----|------|
| Kitchen | `sensor.kitchen_power` | `power_kitchen` | 100W | 2000W |
| Office | `sensor.office_power` | `power_office` | 50W | 300W |
| Total | `sensor.house_power` | `power_total` | 500W | 3500W |

### Scenario 3: Solar System

Monitor grid consumption (positive = import, negative = export):

```yaml
Power Sensor: sensor.grid_power
App Name: power_grid
Low Threshold: 100W
High Threshold: 2000W
Display Unit: Auto
Icon Mode: Home Power
```

---

## 🐛 Troubleshooting

### Display Not Updating

**1. Check sensor:**
```yaml
# Developer Tools → States
sensor.your_power_sensor
  state: 1234  # Should be a number
  device_class: power
  unit_of_measurement: W
```

**2. Verify AWTRIX:**
- Device online in Home Assistant?
- MQTT broker running?
- Test with AWTRIX companion app

**3. Check logs:**
```yaml
Settings → System → Logs
Look for: "⚡ Power Consumption Display Updated"
```

### Sensor in kW Instead of W

Create a template sensor to convert:

```yaml
template:
  - sensor:
      - name: "Power in Watts"
        unique_id: power_watts
        unit_of_measurement: "W"
        device_class: power
        state: >
          {{ (states('sensor.your_kw_sensor')|float(0) * 1000)|round(0) }}
```

### Wrong Colors

- Check current sensor value vs your thresholds
- Adjust Low/High thresholds to match your usage
- Example: 1500W showing red? Increase High Threshold

---

## 💡 Advanced Tips

### Time-Based Thresholds

Create two automations with different thresholds:

**Night (23:00-07:00):**
- Low: 100W, High: 500W
- Add condition: `{{ now().hour >= 23 or now().hour < 7 }}`

**Day (07:00-23:00):**
- Low: 500W, High: 3000W
- Add condition: `{{ now().hour >= 7 and now().hour < 23 }}`

### Calculate Cost Per Hour

Create template sensor:

```yaml
template:
  - sensor:
      - name: "Power Cost Per Hour"
        unit_of_measurement: "€/h"
        state: >
          {{ (states('sensor.house_power')|float(0) / 1000 * 0.30)|round(2) }}
```

### Integration with Energy Dashboard

1. Configure Energy Dashboard (Settings → Energy)
2. Add your power sensor
3. Use this blueprint for real-time display
4. View historical data in Energy Dashboard

---

## 🤝 Contributing

Contributions welcome!

- 🐛 [Report bugs](https://github.com/Raythekool/ha-awtrix-power-consumption/issues)
- 💡 [Request features](https://github.com/Raythekool/ha-awtrix-power-consumption/issues)
- 🔀 [Submit pull requests](https://github.com/Raythekool/ha-awtrix-power-consumption/pulls)

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

- **AWTRIX 3**: [Blueforcer](https://github.com/Blueforcer/awtrix3)
- **Icons**: [LaMetric Icon Library](https://developer.lametric.com/icons)
- **Home Assistant**: [Home Assistant Community](https://www.home-assistant.io/)
- **Inspiration**: [Matt Zaske's blog](https://mattzaskeonline.info/blog/2025-07/configuringusing-awtrix-3-ulanzi-tc001-through-home-assistant)

---

## ⭐ Show Your Support

If you find this blueprint useful:

- ⭐ [Star this repository](https://github.com/Raythekool/ha-awtrix-power-consumption)
- ☕ [Buy me a coffee](https://buymeacoffee.com/raythekool)
- 💬 Share your setup and feedback!

---

## 🔗 Related Projects

- [AWTRIX Stock Display](https://github.com/Raythekool/ha-awtrix-yahoofinance-stock) - Display stock prices on AWTRIX
- [AWTRIX Documentation](https://blueforcer.github.io/awtrix3/) - Official AWTRIX 3 docs
- [Home Assistant Energy](https://www.home-assistant.io/home-energy-management/) - Energy management in HA
