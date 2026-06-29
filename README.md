# BGH Smart Control Retrofit

Proyecto _en progreso_ para reemplazar la PCB del BGH Smart Control con un ESP32 con ESPHome

## Instalar ESPHome

Tuve problemas al usar una version de ESPHome muy distinta del servidor de Home Assistant, asi que mejor instalar la version exacta.

Yo lo resolvi corriendo:

``` shell
uv tool install esphome==2026.1.3 --python 3.12 --force
```
(teniendo `uv` instalado)

## Hardware usado

Use una placa ESP32C3 SuperMini

Pines:

**GPIO0:** Out del receptor IR

**GPIO1:** Base transistor transmisor IR

## Detectar Protocolo IR

Para capturar e identificar la señal de un control remoto hay que agregar `dump: all` asi:

```yaml
remote_receiver:
  id: ir_receiver
  pin:
    number: GPIO00
    inverted: true
  # high 55% tolerance is recommended for some remote control units
  tolerance: 55%
  dump: all # <------
```

Subir el firmware con `esphome upload esp32c3.yaml`

Y despues monitorear los logs con `esphome logs esp32c3.yaml` mientras envias la señal del control remoto.

Si por alguna razon no funciona, se puede usar [este script](https://github.com/crankyoldgit/IRremoteESP8266/blob/master/examples/IRrecvDumpV2/IRrecvDumpV2.ino) de `IRremoteESP8266`

## Protocolos IR probados

**Control remoto:** LG AKB73756220

**Protocolo:** LG variante `LG2`

Timings que lo diferencian del protocolo LG normal:
```yaml
    header_high: 3200us
    header_low: 9900us
    bit_high: 480us # bit mark
```

Si bien el control remoto tiene botones para "JET MODE", "SLEEP" y "ENERGY SAVING", ninguna de las funciones esta implementada en `climate_ir_lg`.

## Links utiles

Timings default del protocolo LG: https://github.com/esphome/esphome/blob/release/esphome/components/remote_base/lg_protocol.cpp#L11
Timings del protocolo LG2: https://github.com/crankyoldgit/IRremoteESP8266/blob/master/src/ir_LG.cpp#L156
Todos los protocolos soportados por la libreria que usa ESPHome: https://github.com/crankyoldgit/IRremoteESP8266/blob/master/SupportedProtocols.md
Timings por default que usa ESPHome: https://github.com/esphome/esphome/blob/release/esphome/components/remote_base/lg_protocol.cpp#L11