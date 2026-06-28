# BGH Smart Control Retrofit

Proyecto _en progreso_ para reemplazar la PCB del BGH Smart Control con un ESP32 con ESPHome

## Instalar ESPHome

Tuve problemas al usar una version de ESPHome muy distinta del servidor de Home Assistant, asi que mejor instalar la version exacta.

Yo lo resolvi corriendo:

``` shell
uv tool install esphome==2026.1.3 --python 3.12 --force
```

## Hardware usado

Use una placa ESP32C3 SuperMini

Pines:

GPIO0: Out del receptor IR
GPIO1: Base transistor transmisor IR


## Links utiles

Timings default del protocolo LG: https://github.com/esphome/esphome/blob/release/esphome/components/remote_base/lg_protocol.cpp#L11
