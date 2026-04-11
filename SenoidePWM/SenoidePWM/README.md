# Gerador de Senoide via PWM - STM32

## 📌 Descrição

Este projeto implementa um gerador de sinal senoidal utilizando o microcontrolador STM32 (placa NUCLEO-F446RE). A senoide é sintetizada a partir de um sinal PWM (Pulse Width Modulation), cujo duty cycle varia ao longo do tempo de acordo com uma tabela senoidal.

O sinal PWM é filtrado por um circuito passa-baixas RC, resultando em uma aproximação de um sinal analógico senoidal.

A frequência da senoide pode ser ajustada em tempo real via comunicação UART.

---

## 🎯 Objetivos

- Gerar uma senoide a partir de PWM
- Aplicar filtragem analógica usando filtro RC
- Controlar a frequência do sinal via UART
- Analisar o comportamento do sinal no osciloscópio

---

## ⚙️ Hardware Utilizado

- STM32 NUCLEO-F446RE
- Resistor (1 kΩ)
- Capacitor (100 nF)
- Osciloscópio
- Cabos de conexão

---

## 🔌 Montagem do Circuito


PA8 (PWM) ---- resistor (1kΩ) ----+---- saída analógica
|
100nF
|
GND


---

## 🧠 Funcionamento

### 🔹 Geração da senoide

- Uma tabela com 100 pontos senoidais é gerada no código.
- Um timer (TIM2) gera interrupções periódicas.
- A cada interrupção, o duty cycle do PWM é atualizado com o próximo valor da tabela.
- Isso faz com que o sinal PWM varie suavemente, simulando uma senoide.

---

### 🔹 PWM

- Gerado pelo TIM1 (Channel 1 - PA8)
- Resolução: 1000 níveis (ARR = 999)
- Frequência alta para facilitar filtragem

---

### 🔹 Filtro RC

- Remove componentes de alta frequência do PWM
- Mantém apenas a frequência fundamental da senoide

---

### 🔹 UART

- Interface serial para controle da frequência
- Usuário envia valores via terminal (ex: 10, 20, 50 Hz)
- O sistema ajusta a velocidade da senoide dinamicamente

---

## 📡 Como Usar

1. Conectar a placa via USB
2. Abrir um terminal serial (ex: PuTTY)
3. Configurar:
   - Baud rate: 115200
4. Enviar valores de frequência, por exemplo:

10
25
50


---

## 📊 Resultados

- Para frequências baixas, a senoide apresenta boa qualidade
- Para frequências mais altas, há distorção
- A qualidade depende do filtro RC e da frequência do PWM

---

## ⚠️ Limitações

- O filtro RC possui frequência de corte fixa
- Presença de harmônicos do PWM
- Distorção aumenta com a frequência
- THD não foi medido quantitativamente

---

## 📈 Análise

O sinal PWM contém múltiplas componentes harmônicas. O filtro RC atua atenuando essas componentes, reduzindo a distorção e aproximando o sinal de uma senoide.

A eficiência do filtro depende da relação entre:
- Frequência da senoide
- Frequência do PWM
- Constante de tempo do circuito RC

---

## 🚀 Melhorias Futuras

- Aumentar número de amostras da senoide
- Aumentar frequência do PWM
- Projetar filtro com melhor resposta
- Implementar análise de THD
- Utilizar DAC (em placas que possuem)

---



## 👨‍💻 Autor

- Aryel de Souza Silva

---

## 📚 Referência

- Mastering STM32 - Second Edition
- Documentação oficial STM32
