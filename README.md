# Sistema de Monitoramento de Água 🌊

Este repositório contém os arquivos de projeto de hardware e firmware para um **Sistema de Monitoramento de Água**, focado na leitura, condicionamento de sinal e exibição dos parâmetros de **pH** e **Condutividade Elétrica (CE)**.

---

## 🛠️ Arquitetura do Sistema

O projeto é baseado no microcontrolador **ATmega328P** e subdividido nos seguintes blocos funcionais de hardware:

1. **Fonte de Alimentação**:
   - Conector de entrada `VIN` (+5V).
   - Regulador LDO 3V
   - Gerador de tensão negativa **ICL7660** para obtenção de **-3V**, garantindo alimentação simétrica para os amplificadores operacionais.
   - LEDs de indicação de status.

2. **Microcontrolador (ATmega328P)**:
   - Operando com cristal externo de **16 MHz**.
   - Interface de gravação **AVR-ISP-6** (In-System Programming).
   - Leitura analógica dos sinais condicionados nas portas `ADC0` (pH) e `ADC1` (Condutividade Elétrica).
   - Interface com sensor digital via conector de dados (`DATA` / Pino `PD4` com *pull-up*).

3. **Condicionamento de Sinal de pH**:
   - Desenvolvido com o amplificador operacional **MCP6002**.
   - **Primeiro estágio (Offset)**: Referência de tensão ajustável de 2,4V via trimpot para deslocar o sinal do eletrodo de pH (que varia em milivolts positivos e negativos).
   - **Segundo estágio (Ganho)**: Configuração de amplificador não-inversor com ganho ajustável via trimpot (`RV3 100K`) para calibração da faixa de leitura `AD_PH`.

4. **Condicionamento de Sinal de Condutividade Elétrica (CE)**:
   - Gerador de frequência/onda usando o CI **CD4060** e alimentado em **+3V / -3V**.
   - Circuito de condicionamento de sinal baseado no Quad Op-Amp **LMV324**.
   - Retificador/Filtro ativo utilizando diodos `1N4002` e estágios de filtragem RC (`1µF` / `10kΩ`) para entregar um sinal contínuo `AD_EC` proporcional à condutividade da água.

5. **Display / Interface Homem-Máquina**:
   - Módulo **LCD 16x2 (WC1602A)** operando em modo de comunicação de 4 bits (`D0`-`D3` / `Enable` / `RS`).
   - Trimpot de 10kΩ (`RV2`) dedicado ao ajuste de contraste do LCD.

---

## 📂 Estrutura do Repositório

```text
.
├── firmware/                               # Código-fonte para o microcontrolador ATmega328P
├── hardware/                               # Arquivos do projeto de hardware
│   ├── esquematico/                        # Documentação exportada em PDF
│   │   └── monitoramento_agua.pdf
│   └── kicad_project/           # Projeto no KiCad (v10.0.5)
│       ├── Atmega328p_sch.kicad_sch
│       ├── condicionamento_ec_sch.kicad_sch
│       ├── Condicionamento_PH_sch.kicad_sch
│       ├── display_sch.kicad_sch
│       ├── fonte_sch.kicad_sch
│       ├── monitoramento_agua.kicad_pro
│       └── monitoramento_agua.kicad_sch
└── README.md                    # Documentação do projeto
