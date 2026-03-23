# Realtime Vision Pipeline with Arduino Leonardo R3 HID Bridge

Projeto experimental de **visão computacional em tempo real** integrado a um **Arduino Leonardo R3** para transporte de comandos via **HID/RawHID**, com foco em:

- baixa latência;
- processamento em janela reduzida;
- tolerância a falhas de captura e reconexão;
- agregação de entrada USB no microcontrolador;
- integração entre software Python e firmware embarcado.

O sistema é composto por três camadas técnicas:

1. **Aplicação Python**
   - captura de tela com DXCam;
   - processamento de imagem com OpenCV/NumPy;
   - seleção geométrica de referência;
   - geração de deltas incrementais;
   - envio de comandos para o microcontrolador por HID.

2. **Firmware para Arduino Leonardo R3**
   - recepção de relatórios RawHID;
   - uso do USB nativo do ATmega32U4;
   - agregação de deltas vindos do Python com deltas vindos de um dispositivo USB externo conectado via Host Shield;
   - emissão final de eventos HID para o computador host.

3. **Modificações no core USB/HID**
   - ajustes no stack USB para operação sem CDC;
   - adaptação da configuração de endpoints;
   - alterações voltadas à estabilidade e ao layout de interface desejado;
   - derivadas do ecossistema open source Arduino, com preservação de créditos e licenças.

---

## 1. Visão geral da arquitetura

O projeto foi desenvolvido para demonstrar uma cadeia completa de processamento em tempo real:

```text
Captura de tela -> Segmentação de imagem -> Extração de referência -> Geração de deltas ->
Empacotamento HID -> Arduino Leonardo R3 -> Consolidação de entrada -> HID final ao host
