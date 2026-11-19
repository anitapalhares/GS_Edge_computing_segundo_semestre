https://wokwi.com/projects/481084461184291841

https://thingspeak.mathworks.com/channels/3586432/private_show

Channel ID: 3586432
Author: mwa0000043123166
Access: Private

# ViaTempo - Smart Alarm Hub (Edge Computing & IoT)


##  Integrantes - Dev.Ak
* **Anita Palhares** - RM571264
* **Kauã Coelho** - RM568665
* **Vitória Kereski** - RM569438

---

##  Visão Geral da Solução
O **ViaTempo Smart Alarm Hub** é um dispositivo de borda (Edge Computing) simulado com o microcontrolador **ESP32**, projetado para monitorar condições ambientais locais e níveis de tráfego urbano em tempo real. Integrado à plataforma em nuvem **ThingSpeak**, o terminal atua preventivamente, recalculando a rotina do usuário e acionando alertas visuais e sonoros com base em variáveis climáticas e de mobilidade.

---

##  Arquitetura e Componentes de Hardware
* **Microcontrolador:** ESP32 DevKit v4
* **Display Local:** LCD 16x2 com interface I2C (Endereço `0x27`, pinos GPIO 21 SDA e GPIO 22 SCL)
* **Sensor de Clima / Chuva:** DHT22 (Sensor digital de Temperatura e Umidade no GPIO 15)
* **Sensor de Nível de Tráfego:** Potenciômetro analógico (GPIO 34 - ADC)
* **Atuador Sonoro (Alarme):** Buzzer Piezoelétrico (GPIO 18 - PWM)
* **Atuador Visual (Alerta Crítico):** LED Vermelho de alto brilho com resistor de 220Ω (GPIO 19)
* **Interação Física Local:** Pushbutton com resistor interno PULL-UP (GPIO 4 - Função Soneca/Desligar)

---

##  Lógica de Borda e Amostragem (Zero `delay`)
Em conformidade com as boas práticas de sistemas embarcados e requisitos da disciplina:
* **Amostragem Temporizada:** Não foi utilizado `delay()` no loop principal. Todas as rotinas de amostragem (1s), atualização de interface gráfica (500ms) e envio para a nuvem (15s) são gerenciadas por temporizadores baseados em `millis()`.
* **Decisão Autônoma na Borda:**
  * Se o nível de trânsito ultrapassar **75%** ou a umidade relativa superar **85%** (risco iminente de temporal/alagamento), o dispositivo aciona imediatamente o LED de alerta.
  * Se o congestionamento atingir nível crítico (> **85%**), o alarme é disparado antecipadamente (`ALRM:DISPAR!`) com sinal sonoro intermitente de 1 kHz.
  * O botão físico no GPIO 4 desativa o alarme localmente através de interrupção lógica, alterando o status para Soneca.

---

##  Nuvem e Persistência de Dados (ThingSpeak)
* **Canal ID:** `3586432`
* **Transmissão:** Envio de telemetria a cada 15 segundos mapeando:
  * **Field 1:** Temperatura (°C)
  * **Field 2:** Umidade Relativa (%)
  * **Field 3:** Nível de Tráfego (0 a 100%)
  * **Field 4:** Status do Alarme (0=Soneca, 1=Armado, 2=Disparando)
  * **Field 5:** Flag de Alerta Crítico (0=Normal, 1=Crítico)

---

##  Interface Gráfica Local (Display LCD 16x2 I2C)
O display fornece feedback em tempo real cumprindo os três requisitos obrigatórios:
* **Linha 1:** Leituras dos sensores (`C:<Umidade>% Trf:<Tráfego>%`)
* **Linha 2:** Status do Atuador (`ALRM:ARMADO`, `ALRM:DISPAR!` ou `ALRM:SONECA`)
* **Diagnóstico de Conectividade:** Indicador de status no canto inferior direito (`M:OK` para conexão de nuvem ativa, `W:X` para queda de Wi-Fi e `M:--` para falha de envio).

---

## 🎬 Demonstração em Vídeo e Simulação
* **Link da Simulação no Wokwi:** [Inserir o link público do seu projeto Wokwi]
* **Vídeo de Demonstração (YouTube):** [Inserir link do vídeo não listado]
