# Forecast Lab (ESPHome + ESP32-S3 + ST7789)

**Forecast Lab** — компактний погодний дисплей на базі **ESP32-S3** та **2.0" SPI TFT (ST7789V, 240x320)**.
Пристрій отримує дані з Home Assistant і показує:

- температуру на вулиці
- вологість
- швидкість вітру
- іконку погодного стану

## Обладнання

- ESP32-S3 DevKitC-1
- 2.0" SPI TFT дисплей (ST7789V, 8 pin)
- USB-C кабель
- Dupont дроти

## Схема підключення

Поточна робоча розпайка:

| Пін дисплея | Пін ESP32-S3    |
| ----------- | --------------- |
| `BL`        | `GPIO7`         |
| `CS`        | `GPIO12`        |
| `DC`        | `GPIO11`        |
| `RST`       | `GPIO10`        |
| `SDA`       | `GPIO13` (MOSI) |
| `SCL`       | `GPIO14` (CLK)  |
| `VCC`       | `3V3`           |
| `GND`       | `GND`           |

## Структура проєкту

- `forecast-lab.yaml` — основна конфігурація ESPHome
- `secrets.yaml` — Wi-Fi, API key, OTA password
- `README.md` — документація

## Сутності Home Assistant

Зараз проєкт читає такі сутності:

- `sensor.vulitsia_t_h_a_temperature`
- `sensor.vulitsia_t_h_a_humidity`
- `binary_sensor.m_kiiv_air`
- `sensor.battery_monitor_4s_battery_level`
- `weather.forecast_home` (стан + атрибут `wind_speed`)

Якщо у вас інші `entity_id`, замініть їх у `forecast-lab.yaml` в блоках `sensor:` і `text_sensor:`.

## Налаштування

1. Заповніть `secrets.yaml`:
   - `wifi_ssid`
   - `wifi_password`
   - `forecast_api_key`
   - `forecast_ota_password`
   - `forecast_web_username`
   - `forecast_web_password`
2. Додайте пристрій у Home Assistant через інтеграцію ESPHome.
3. Скомпілюйте і прошийте.

## Команди збірки та прошивки

```bash
esphome run forecast-lab.yaml
```

Тільки OTA-завантаження:

```bash
esphome upload forecast-lab.yaml
```

Логи:

```bash
esphome logs forecast-lab.yaml
```

## Що відображається на екрані

- заголовок `Наявність тривоги та рівень батареї`
- велике значення температури
- іконка + значення вологості
- іконка + швидкість вітру (`m/s`)
- іконка стану погоди (сонце/хмари/дощ/сніг/гроза)

## Примітки

- Дисплей працює через `mipi_spi` з моделлю `ST7789V`.
- Параметри `rotation`, `dimensions`, `offset` підібрані під цей модуль.
- Якщо кольори відображаються неправильно, перевірте `invert_colors` та `color_order` у `forecast-lab.yaml`.

## Версія

Визначається в `substitutions`:

- проєкт: `forecast.lab`
- версія: `1.3.0`
