# ✈️ Sistema de Estimação de Orientação com MPU6050 e OLED (STM32)

## 📌 Descrição

Este projeto implementa um sistema embarcado capaz de estimar a orientação espacial de um objeto utilizando um sensor inercial MPU6050 e exibir as informações em um display OLED SSD1306.

A estimativa dos ângulos de orientação (Roll, Pitch e Yaw) é realizada através de um **filtro complementar**, combinando dados do acelerômetro e giroscópio.

Além da exibição numérica, o sistema também apresenta uma **animação gráfica** representando a orientação de uma aeronave em tempo real.

---

## 🎯 Objetivos

* Realizar comunicação via I2C com sensores
* Ler dados triaxiais de aceleração e velocidade angular
* Converter dados brutos para unidades físicas
* Implementar filtro complementar
* Estimar ângulos de orientação (Roll, Pitch, Yaw)
* Exibir informações em display OLED
* Implementar animação baseada na orientação

---

## 🧰 Hardware Utilizado

* Placa STM32 NUCLEO-F767ZI
* Sensor inercial MPU6050
* Display OLED SSD1306 (128x64, I2C)
* Protoboard
* Jumpers

---

## 🔌 Conexões

### I2C (Compartilhado)

| Dispositivo | Pino | STM32 |
| ----------- | ---- | ----- |
| MPU6050     | SCL  | PB8   |
| MPU6050     | SDA  | PB9   |
| SSD1306     | SCL  | PB8   |
| SSD1306     | SDA  | PB9   |

### Alimentação

| Dispositivo | Conexão |
| ----------- | ------- |
| MPU6050 VCC | 3.3V    |
| MPU6050 GND | GND     |
| OLED VCC    | 3.3V    |
| OLED GND    | GND     |

---

## ⚙️ Configuração do Projeto

* IDE: STM32CubeIDE
* Microcontrolador: STM32F767ZI
* Comunicação: I2C (400 kHz)
* Biblioteca HAL

### Observação importante

Foi necessário habilitar suporte a float no `printf`:

```
-u _printf_float
```

---

## 🧠 Funcionamento

### 📊 Leitura dos Sensores

O sistema realiza leitura contínua de:

* Aceleração: ax, ay, az
* Velocidade angular: gx, gy, gz

---

### 🔄 Filtro Complementar

Os ângulos são calculados utilizando:

* Acelerômetro → referência de longo prazo
* Giroscópio → resposta rápida

---

### 📐 Cálculo dos Ângulos

* **Pitch**: inclinação frente/trás
* **Roll**: inclinação lateral
* **Yaw**: rotação horizontal (integração do giroscópio)

---

### ⚠️ Observação sobre Yaw

O ângulo de yaw é obtido apenas pela integração do giroscópio, portanto:

* Está sujeito a **drift**
* Não possui referência absoluta (ausência de magnetômetro)

---

## 🔧 Calibração

Para reduzir erros do giroscópio, foi implementado:

### ✔ Offset inicial

O sistema calcula o erro médio do sensor parado:

```c
gz_offset = média das leituras
```

---

### ✔ Zona morta (deadband)

Pequenas variações são ignoradas:

```c
if (fabs(gz) < 0.5) gz = 0;
```

---

## 🖥️ Interface (Display)

O sistema alterna entre dois modos:

### 🎮 Modo 1 — Animação

* Exibição de sprite de avião
* Direção baseada em Yaw
* Inclinação baseada em Pitch

---

### 📊 Modo 2 — Dados

Exibe:

```
R: Roll | P: Pitch | Y: Yaw
A: ax ay az
G: gx gy gz
```

---

## 🎮 Interpretação dos Movimentos

Considerando o sistema segurado como um volante:

| Movimento              | Resultado |
| ---------------------- | --------- |
| Girar esquerda/direita | Yaw       |
| Levantar/abaixar       | Pitch     |
| Inclinar lateralmente  | Roll      |

---

## 📈 Resultados

* Estimativa de orientação estável
* Boa resposta dinâmica
* Drift reduzido com calibração
* Interface gráfica funcional

---

## 🚀 Possíveis Melhorias

* Uso de magnetômetro para correção do Yaw
* Implementação de filtro de Kalman
* Interface gráfica mais elaborada
* Comunicação via UART/Bluetooth

---

## 👨‍💻 Autor

Projeto desenvolvido para a disciplina de Sistemas Embarcados.

---

## 📹 Demonstração

(https://youtu.be/6b0099xZxGs)

---

## 📚 Referências

* Datasheet MPU6050
* Documentação STM32 HAL
* Material da disciplina
