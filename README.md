# Sistema de Monitoramento de pH, EC e Temperatura

Este repositório contém os arquivos de hardware desenvolvidos no **KiCad** para o sistema de monitoramento de parâmetros físico-químicos da água, com foco em medições de **pH**, **Condutividade Elétrica (EC)** e **Temperatura**.

O projeto foi projetado de forma modular, dividido em **duas placas de circuito impresso (PCBs)** interconectadas para otimizar o condicionamento de sinal analógico e evitar ruídos de interferência no microcontrolador.

---

## 🛠️ Arquitetura do Sistema

A arquitetura do projeto é dividida nos seguintes módulos:

```
                  ┌──────────────────────────────────────────────┐
                  │              Placa 1: MCU                    │
                  │                                              │
┌──────────────┐  │   ┌────────────────┐     ┌──────────────┐   │
│ Sensores     │──┼──>│ ATmega328P     │────>│ Interfaces   │   │
│ Temp (DS18B20)│ │   │ (Microcontrol) │     │ (Display/UART)│  │
└──────────────┘  │   └───────▲────────┘     └──────────────┘   │
                  └───────────┼──────────────────────────────────┘
                              │ Sinal Analógico Filtrado
                  ┌───────────┴──────────────────────────────────┐
                  │ Placa 2: Condicionamento de Sinal            │
                  │                                              │
┌──────────────┐  │   ┌────────────────┐     ┌──────────────┐   │
│ Sonda pH     │──┼──>│ Amplificadores │    │ Conectores   │   │
└──────────────┘  │   │ Operacionais   │────>│ BNC / Bornes │   │
┌──────────────┐  │   │ e Filtros      │     └──────────────┘   │
│ Sonda EC     │──┼──>│                │                        │
└──────────────┘  │   └────────────────┘                        │
                  └──────────────────────────────────────────────┘
```

---

## 📐 Estrutura do Hardware

### 1. Placa MCU (`/hardware/placa_mcu`)
Placa responsável pelo processamento dos dados, leitura dos sinais analógicos e comunicação externa.

* **Microcontrolador:** ATmega328P (Encapsulamento TQFP/DIP).
* **Alimentação:** Regulador de tensão integrado e circuitos de filtragem de alimentação.
* **Comunicação & Periféricos:** 
  * Pinos de expansão para displays (LCD/OLED) ou conectores de comunicação (UART/I2C/SPI).
  * Interface para sensor digital de temperatura (ex: DS18B20).
  * Entradas analógicas protegidas para recepção dos sinais vindo da placa de condicionamento.

### 2. Placa de Condicionamento de Sinal (`/hardware/placa_condicionamento`)
Placa analógica dedicada ao tratamento de altas impedâncias e sinais de baixa amplitude provenientes dos eletrodos.

* **Condicionamento de pH:** Amplificador operacional de altíssima impedância de entrada (como TL082 ou CA3140) em configuração de ganho e offset ajustáveis para adequar o sinal da sonda (mV) à escala $0 - 5\text{V}$ do ATmega328P.
* **Condicionamento de EC:** Circuito de excitação em corrente alternada (AC) para a célula de condutividade (evitando a polarização dos eletrodos) e circuito retificador/filtro para conversão em tensão contínua (DC).
* **Conexões de Entrada:** Conectores BNC para sondas de pH e conectores de borne/jST para a célula de EC.

---

## 📂 Estrutura de Pastas do Repositório

```text
.
├── hardware/
│   ├── placa_mcu/                 # Projeto KiCad da Placa Principal (MCU)
│   │   ├── placa_mcu.kicad_pro
│   │   ├── placa_mcu.kicad_sch
│   │   └── placa_mcu.kicad_pcb
│   │
│   └── placa_condicionamento/     # Projeto KiCad da Placa Analógica
│       ├── condicionamento.kicad_pro
│       ├── condicionamento.kicad_sch
│       └── condicionamento.kicad_pcb
│
├── .gitignore                     # Arquivos temporários e backups do KiCad ignorados
└── README.md                      # Documentação do projeto
```

---

## 💻 Ferramentas Utilizadas

* **KiCad EDA** (Versão 7.0 ou superior) - Para esquemáticos e layout das placas.
* **Git / GitHub** - Versionamento do código-fonte dos esquemáticos e layouts.

---

## 🚀 Como Abrir o Projeto no KiCad

1. Clone este repositório para o seu computador:
   ```bash
   git clone https://github.com/seu-usuario/seu-repositorio.git
   ```
2. Abra o KiCad.
3. Para a placa do microcontrolador: Vá em **Arquivo > Abrir Projeto** e selecione `hardware/placa_mcu/placa_mcu.kicad_pro`.
4. Para a placa de condicionamento: Vá em **Arquivo > Abrir Projeto** e selecione `hardware/placa_condicionamento/condicionamento.kicad_pro`.

---

## 📝 Licença

Este projeto está sob a licença [MIT](LICENSE) - sinta-se à vontade para modificar e utilizar para fins acadêmicos ou comerciais.