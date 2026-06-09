# Projeto_Eletronica_USP
Construção de uma fonte de tensão retificadora ajustável entre 3V e 12V com capacidade de 100mA. O circuito será feito a partir de uma corrente alternada de 127V (pico de 180V) de 60Hz.

# Membros do Grupo
- Kim
- Henrique Mohr
- João Paulo
- Pedro Romano

# Tabela de Componentes 
|Quantidade|Componente|Descrição|Valor Unitário|
|:---------|:---------|:--------|:-------------|
|1         |Protoboard|xxx pontos de conexão|	R$ 39,10|
|1	|Kit Jumper	|Macho-Macho + Macho-Fêmea	|R$ 28,89|
|1	|Capacitor	|470 uF|	R$ 0,44|
|1	|Potenciômetro	|10 kΩ, 1W|	R$ 7,00|
|1	|Resistor	|1,2 kΩ, 1W|	R$ 0,40|
|1	|Resistor	|5,4 kΩ|	R$ 0,07|
|4	|Resistor	|120 Ω|	R$ 0,13|
|1	|Resistor	|4,7 kΩ, 2W|	R$ 1,20|
|4	|Diodo Retificador	|1N4007|	R$ 0,20|
|1	|Diodo Zener	|13V, 1W|	R$ 0,50|
|1	|LED	5MM |Difuso 333‑2SDRD/S530‑L|	R$ 0,50|
|1	|Transistor	|NPN BC338-25|	R$ 0,45|

> Valor total gasto:



## Papel de Cada Componente
### Transformador
É o componente de entrada. Ele reduz os 127V alternados da tomada (que são perigosos) para 12V alternados. Ele também isola o restante do circuito da rede elétrica por segurança.

### Ponte Retificadora
A energia do transformador ainda é alternada (muda de direção 60 vezes por segundo). A ponte de diodos direciona essa corrente para que ela flua em um único sentido, transformando Corrente Alternada (AC) em Corrente Contínua (DC) pulsante.

### Capacitor Eletrolítico 
Funciona como um "reservatório" ou amortecedor de energia. Ele armazena carga nos momentos de pico da onda e descarrega nos momentos de queda. Isso elimina os pulsos da retificação (chamados de ripple), deixando a tensão contínua lisa e linear (cerca de 24,2V).

### Resistor de Polarização 
Ele limita a corrente que vai para o diodo Zener. Sem ele, o Zener receberia toda a potência do capacitor e queimaria instantaneamente. Ele absorve o "excesso" de tensão (11,2V) e o transforma em calor.

### Diodo Zener
É o coração da referência de tensão. Não importa se a tensão no capacitor oscilar um pouco; o Zener "trava" a tensão sobre ele em exatamente 13V estáveis. É a partir dessa barreira de 13V que o potenciômetro vai trabalhar.

### Potenciômetro Linear
Componente resistivo ajustável que funciona como a interface do usuário. Ao deslizar o cursor central (atualmente fixado em 50% de curso), ele extrai uma fração precisa de tensão da malha para controlar o nível elétrico injetado no circuito de potência.

### Resistor Inferior
Fica na base do potenciômetro, conectado ao terra. Ele impede que a tensão de controle caia para 0V. Ele segura o piso inferior em 3,7V, garantindo que o ajuste mínimo da sua fonte comece exatamente em 3V.

### Transistor NPN de Potência
Funciona como o músculo de corrente da fonte. Como a malha do potenciômetro manipula correntes muito baixas que sofreriam quedas drásticas ao alimentar qualquer dispositivo externo, o transistor recebe a tensão de controle em sua Base e a replica em seu Emissor com alta capacidade de corrente, sofrendo apenas uma perda fixa de $0,7\text{V}$ na barreira interna de silício.

### Resistor de Emissor 
Resistor de estabilização de saída que garante uma circulação de corrente mínima permanente pelo transistor. Isso mantém o componente polarizado e estável mesmo quando nenhuma carga externa estiver conectada aos terminais da fonte.

### LED Vermelho e Resistor Limitador
Conjunto indicador visual. O LED acende para confirmar que a fonte está ativa e gerando tensão na saída, enquanto o resistor de 4,7k Ohw limita a corrente para que o LED opere com segurança sem queimar quando a saída atingir o valor máximo de 12V.



## Cálculos Necessários
### 1. Cálculo do Transformador e Entrada
A tensão residencial nominal eficaz da tomada é $V_{\text{RMS}} = 127\text{ V}$. O valor de pico máximo dessa senoide alimentadora é:

$$V{\text{pico}} = V{\text{RMS}} \times \sqrt{2} = 127 \times 1{,}4142 \approx 180\text{ V}$$

No simulador, a tensão contínua perfeitamente estabilizada sobre o capacitor de filtro atingiu o pico de $25{,}959\text{ V}$ ($\approx 26\text{ V}$). A relação de transformação de espiras ($N$) necessária para ajustar o transformador é dada por:

$$\text{Relação de Espiras} = \frac{V_{\text{pico\_primario}}}{V_{\text{capacitor}}} = \frac{180\text{ V}}{26\text{ V}} \approx 6{,}92$$

### 2. Comportamento de Queda no Transistor
O transistor NPN operando como seguidor de emissor dita que a tensão final disponível na saída ($V_{\text{saida}}$) rastreia a tensão ajustada na base ($V_{\text{base}}$), subtraindo a barreira de silício da junção Base-Emissor ($V_{\text{BE}} = 0{,}7\text{ V}$):

$$V{\text{saida}} = V{\text{base}} - 0{,}7\text{ V} \implies V{\text{base}} = V{\text{saida}} + 0{,}7\text{ V}$$

Para alcançar a janela de regulação final na saída da fonte (de 3V a 12V), a base precisa receber os seguintes limites:

* **Para saída mínima de 3V:**
  $$V{\text{base\_minima}} = 3\text{ V} + 0{,}7\text{ V} = 3{,}7\text{ V}$$

* **Para saída máxima de 12V:**
  $$V{\text{base\_maxima}} = 12\text{ V} + 0{,}7\text{ V} = 12{,}7\text{ V}$$

### 3. Dimensionamento do Resistor de Proteção do Zener ($R_Z$)
O barramento bruto após a filtragem fornece $26\text{ V}$. O diodo Zener fixa firmemente a tensão em seu terminal em $13\text{ V}$ ($13{,}015\text{ V}$ no simulador). A queda de potencial que o resistor de $1{,}2\text{ k}\Omega$ precisa suportar isoladamente é:

$$V_{RZ} = V_{\text{capacitor}} - V_{\text{Zener}} = 26\text{ V} - 13\text{ V} = 13\text{ V}$$

A corrente contínua que passa por ele para alimentar o diodo Zener é de:

$$I_{RZ} = \frac{V_{RZ}}{R_Z} = \frac{13\text{ V}}{1200\text{ }\Omega} \approx 0{,}0108\text{ A}\quad(10{,}8\text{ mA})$$

**Cálculo Crítico de Aquecimento (Potência):**

$$P_{RZ} = V_{RZ} \times I_{RZ} = 13\text{ V} \times 0{,}0108\text{ A} \approx 0{,}14\text{ W}$$

Como a dissipação deu $0{,}14\text{ W}$, um resistor padrão de película de carbono comum de $1/4\text{ W}$ ($0{,}25\text{ W}$) é plenamente capaz de operar neste ponto do circuito sem risco de superaquecimento.

# Circuito Falstad
<img width="1293" height="837" alt="Image" src="https://github.com/user-attachments/assets/cff179f6-9f50-4843-80ac-7bbac707707a" />

Link para Simulação: [simulação falstad](https://falstad.com/circuit/circuitjs.html?ctz=DwYwlgTgBAZgvAIgIwKgFwM6IAwDpsEECsqYIiSRuALAOwBMtAbLQJy1JIAcTS2rqEACNEReqgAOIhNQDMqAG4RRqALaZRAUwC0nBAD4AUFCjAFUAB6Jt9bFyhdsUG3aj1q2VPGRNUAd28UWGUEJk8oVQBDCwUKRwQAeiMTYAAVS2tbe0coJ3dw73CwADtEalQISJxcJGpWeiJZWlkkWTl6LlkiWkEAe2rWQf8wCjDBbGs8Jkd6ViZZtvokenkoECRq7FoiLkoGBqImViRWOUTk0z8M5GWHJyd42Bxz41MAE2ukZigkMJ-buZeZ4RfoIN6aGCRACuABs0C8UlcrAhZPRnNR7LJsNR0VwgQhPElXsAkYgsTj8m5XPl8YSLiTruTcVA6kxcbSEZdGdi3B4WXNeQVnkSUm9VFBiqpEAAvTTFTQQXSrQKkLDIXAcORiXb1XaEeg6HpQCBq8KlBD0fCEa02uzlKBCWLIZUjZCc4DS66swX8tlMe2FNSg2XyxWtd3QZHe-2+9lPAkVCi2OnE0kIIj1Zne7QYjki0wSNDXML2GMln5jeNBKIUa2SUF4YioDAwxAAJU0GDAGDQkWKIE0EeLrl+TnLNPj4RCRA8KcRw-slO9E8D+YZyNoXBxOfsGbRO7z9LTm4pfPHHkPqc+ALZnDRj1X9IA5tzT04mR08ZP3Wnozjy-6l4pEWG48neUAnv84jflAEjmkEQiaLW4SqKC4KQrC8JQAoiHWK0+BNLYGYMNgRDYO4qw4eQBIapQ1DuPM2LYLIrA7D+1yQQKkG5t+a7HluPwdBBAmjkBpiRogkGUpBokwSEyxzlyG4iUJXA5MsX6PsSAAyACiAAi1xqU4GkOOpzAciCiDodCcLaDCmhvII8mCE+wLCMCqhCNRjYoGuEkIMZPzfEFK7AiEdAEOxyKhXyUkXrx9IgK+zIfglgZrJsBB0KQTqNtgQQYCE4QKG8iANLgrE8FsXDdHUbCsdFiDzDi4G0GByxicAHzIuBAoYhWQoJlZYIQrZ8Jrj1FDfKOUFuO4lmodZY2Ye6U03Gi9DuCy9iyRlS2jRhcJNSi+4Daig1dWmF2zQNe3Cke16bdt4FhQmfHXFkPpffd72PciP1-F8t6Vlp869bclJ5OlD1Xsp24DdxmmwykADKIC9BImjFgwbi4+wbLUDxgYXCkvRQHK5WFRIzUBogFirDTCC6KgyIAGq9HCkRPpo4rFOTADCmgwrCkQQAYrwmCkEjYcCGDUbYnJSwWssos2Pm4OI+YpAkvT0gk6OY5o9KG1j1w5r8bhqc44FE8jCak6Y5OU8NGBM4B8YM5IeGs4gABivTFGg2PglAAu9BAEBysHUAAIJwgqxSRG8kQS8r0uq+E8tkor2tS8AMtOqs2fqr4efALr+um8bxLV+bDRsrotxOMTzyO8AzvmtBbu0-iXuwT71wc1zPPilHaBgDAZDJ5EsHC7PAAKgcx6H+lgL0by9Gn6cF5n6tlIpyu70X++l0rpiV8SBsY1jJs39jvV-DYuPUDG2gPm3ktOxT5q+FAPehDpggfuTMWZD05r2UeFMeyRCEGAGEYBpQzznjCcmIBIgSEiOANAEdt5H0LnLaiF484ZxPv-DWWt26Xx1tXO+RtrhEFuM-e8b9ATfnbp3OIzZ3ZAJAYPdmEDua81gPAtAlQU4oPJgALTlAqPB+cCGuwVofBRqti4awIC0Bo58K56yvrQwwFdwAQCMEAA)

