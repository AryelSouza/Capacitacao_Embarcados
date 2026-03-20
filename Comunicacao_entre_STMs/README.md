# 📡 Comunicação entre dois STM32 usando GPIO + Interrupções

## 📌 Descrição

Este projeto implementa um sistema de comunicação entre duas placas STM32 Nucleo-F446RE utilizando apenas pinos GPIO e interrupções externas (NVIC), conforme os requisitos da atividade.

Uma das placas atua como transmissora (STM A) e a outra como receptora (STM B). Foi desenvolvido um protocolo de comunicação próprio, com verificação de erro, mecanismo de confirmação (ACK/NACK) e retransmissão automática, garantindo robustez na transmissão dos dados.

---

## 🎯 Objetivos

* Implementar comunicação entre dois microcontroladores usando apenas GPIO
* Utilizar interrupções externas (NVIC) para recepção de dados
* Transmitir um array de 100 bytes
* Implementar um protocolo de comunicação confiável

---

## 🧱 Arquitetura do Sistema

### 🔌 Conexões

| STM A (Transmissor) | STM B (Receptor) | Função        |
| ------------------- | ---------------- | ------------- |
| PA0                 | PA0              | Dados         |
| PA1                 | PA1              | Clock         |
| PB0                 | PB0              | ACK (retorno) |
| GND                 | GND              | Referência    |

---

## ⚙️ Funcionamento

A comunicação é síncrona, controlada por um sinal de clock gerado pelo transmissor.

Cada dado transmitido segue o seguinte formato:

```
[START][8 bits de dados][PARIDADE][STOP]
```

### 🔁 Etapas da transmissão:

1. O transmissor envia o bit de START
2. Envia os 8 bits do dado
3. Envia o bit de paridade (para detecção de erro)
4. Envia o bit de STOP
5. Aguarda resposta do receptor (ACK/NACK)

### ✔️ Validação:

* Se a paridade estiver correta → receptor envia ACK
* Se houver erro → envia NACK
* Em caso de NACK → o transmissor retransmite o dado

---

## ⚡ Uso de Interrupções (NVIC)

A recepção dos dados no STM B é feita através de interrupção externa no pino de clock (PA1).

A cada pulso de clock:

* Um novo bit é lido
* O dado é reconstruído progressivamente
* Ao final, o byte é validado e processado

---

## 🔢 Envio de Dados

O sistema transmite continuamente um array de 100 bytes:

```
0, 1, 2, ..., 99
```

---

## 🔘 Interação com Usuário

* Um botão no STM A controla o LED do STM B
* Botão pressionado → envia comando → LED acende
* Botão solto → LED apaga

---

## 🖥️ Debug via UART

O STM B envia os dados recebidos via USART2 para o computador.

No terminal serial (Tera Term), é possível visualizar:

```
Recebido: 0
Recebido: 1
...
Recebido: 99
```

Isso facilita a validação da comunicação.

---

## 🚀 Funcionalidades Implementadas

### ✅ Requisitos mínimos

* Comunicação via GPIO
* Uso de interrupções externas (NVIC)
* Transmissão de 100 bytes
* Comunicação entre dois microcontroladores

### 🔥 Funcionalidades avançadas

* Protocolo estruturado (start, dados, paridade, stop)
* Detecção de erro por paridade
* Handshake com ACK/NACK
* Retransmissão automática
* Comunicação bidirecional (linha de ACK)

---

## 🏆 Destaque do Projeto

O projeto foi otimizado para **robustez**, garantindo que:

* Dados corrompidos sejam detectados
* Retransmissões ocorram automaticamente
* A comunicação seja confiável mesmo com falhas

---

## 🛠️ Tecnologias Utilizadas

* STM32 Nucleo-F446RE
* STM32CubeIDE
* HAL (Hardware Abstraction Layer)
* GPIO
* NVIC (Interrupções externas)
* USART (debug)

---

## ▶️ Como Executar

1. Conectar as duas placas via jumpers (conforme tabela de conexões)
2. Conectar ambas ao PC via USB
3. Gravar o código em cada placa (STM A e STM B)
4. Abrir um terminal serial (ex: Tera Term)
5. Configurar baudrate em 115200
6. Observar os dados sendo recebidos

---

## 📊 Resultado Esperado

* Valores de 0 a 99 sendo exibidos no terminal
* LED da placa receptora respondendo ao botão da transmissora
* Comunicação estável e sem perda de dados

---

## 📌 Conclusão

O projeto atende a todos os requisitos propostos, implementando uma comunicação eficiente e confiável entre dois microcontroladores utilizando apenas GPIO e interrupções externas. Além disso, incorpora técnicas avançadas de protocolos de comunicação, tornando a solução robusta e completa.
