# 💡 Atividade-IOT-Alexa

Este projeto mostra como controlar uma *lâmpada real* usando a *Alexa, com ajuda de um **ESP8266, um **módulo relé* e a plataforma *SinricPro*.

Com isso, é possível ligar e desligar a lâmpada tanto com *comando de voz pela Alexa, quanto por um **botão no aplicativo SinricPro*.

---

## ⚙ Funcionalidades

- Ligar e desligar a lâmpada via *Alexa*  
- Controle manual pelo *app SinricPro*  
- Comunicação em tempo real via *Wi-Fi*  
- Estado sincronizado entre Alexa, app e dispositivo  

---

## 🧠 Como funciona

O ESP8266 se conecta ao *Wi-Fi* e depois à *plataforma SinricPro, que faz a ponte entre a **Alexa* e o *dispositivo físico*.

Quando o usuário fala “*Alexa, ligar a lâmpada”, a Alexa envia esse comando para a **nuvem da SinricPro*, que repassa a instrução para o ESP8266.  
O ESP8266, ao receber o comando, *aciona o relé, que **liga ou desliga a lâmpada*.

---

## 🏗 Arquitetura do sistema

1. *Dispositivo físico*: ESP8266 conectado ao relé e à lâmpada.  
2. *Rede local*: ESP se conecta via Wi-Fi à internet.  
3. *Serviço em nuvem*: SinricPro recebe comandos da Alexa e envia ao ESP.  
4. *Alexa*: Interpreta comandos de voz e envia à nuvem da SinricPro.

---

## 🔄 Fluxo completo do sistema

1. *Usuário fala com Alexa*  
   - Ex.: “Alexa, ligar a lâmpada”.  

2. *Alexa interpreta o comando de voz*  
   - Converte fala em comando digital.  
   - Verifica se o dispositivo está cadastrado e ativo.  

3. *Alexa envia comando à nuvem da Amazon*  
   - Amazon processa e identifica que o dispositivo é controlado via *SinricPro*.  

4. *Amazon repassa para a API da SinricPro*  
   - SinricPro atua como middleware, recebendo e autenticando o comando.  
   - Identifica o ESP8266 correspondente.  

5. *SinricPro envia comando para o ESP8266*  
   - Comunicação via *WebSocket seguro (TLS/SSL)*, quase em tempo real.  
   - Mensagem contém ação: *ligar ou desligar*.  

6. *ESP8266 recebe o comando*  
   - Processa a mensagem e atualiza o estado interno da lâmpada.  

7. *ESP8266 aciona o relé*  
   - Ativa/desativa o pino GPIO conectado ao relé.  
   - Relé fecha/abre o circuito da lâmpada.  

8. *Lâmpada liga ou desliga*  
   - Fluxo de energia da tomada passa pelo relé, acionando a lâmpada.  

9. *Atualização de estado*  
   - ESP envia status atualizado para SinricPro.  
   - SinricPro sincroniza com Alexa e app, mostrando o estado real da lâmpada.  

10. *Usuário vê confirmação*  
    - App SinricPro mostra se a lâmpada está *ligada* ou *desligada*.  
    - Alexa confirma por voz: “Lâmpada ligada/desligada”.  

![Imagem do WhatsApp de 2025-10-28 à(s) 21 10 14_ef270727](https://github.com/user-attachments/assets/2f0b0591-fdab-420d-bc9a-23408bcfe23f)
---

## 🔌 Conexões

| ESP8266 | Relé | Descrição |
|----------|------|-----------|
| D2 (GPIO4) | IN | Controle do relé |
| GND | GND | Terra comum |
| VIN | VCC | Alimentação do relé (5V) |

> O relé é ligado em série com a lâmpada e a tomada, permitindo que o ESP8266 controle o fluxo de energia.

---

## 🧩 Componentes usados

| Componente | Quantidade | Descrição |
|-------------|-------------|-----------|
| ESP8266 (NodeMCU) | 1 | Microcontrolador com Wi-Fi |
| Relé 5V | 1 | Controla a passagem de energia da lâmpada |
| Jumpers macho-fêmea | 3 | Para conexões entre o relé e o ESP |
| Fio elétrico | 1 | Para ligação da lâmpada à tomada |
| Tomada | 1 | Alimentação da lâmpada |
| Lâmpada | 1 | Dispositivo controlado |

---

## 📜 Código

Arquivo principal: main.cpp

Definição principal:
```cpp
#define RELE_PIN 4   // Pino D2 no ESP8266
