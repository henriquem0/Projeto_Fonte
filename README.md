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



## Cálculo Prático do Circuito

### 1. Cálculo do Transformador e Entrada
A tensão residencial nominal eficaz da tomada é $V_{RMS} = 127\text{V}$. O valor de pico máximo dessa senoide alimentadora é dado por:

$$V_{pico} = V_{RMS} \times \sqrt{2} = 127 \times 1,4142 \approx 180\text{V}$$

No simulador, a tensão contínua perfeitamente estabilizada sobre o capacitor de filtro atingiu o pico de $25,959\text{V}$ ($\approx 26\text{V}$). A relação de transformação de espiras ($N$) necessária para ajustar o transformador é calculada pela fórmula:

$$\text{Relação de Espiras} = \frac{V_{pico}}{V_{capacitor}} = \frac{180\text{V}}{26\text{V}} \approx 6,92$$

### 2. Comportamento de Queda no Transistor
O transistor NPN operando como seguidor de emissor dita que a tensão final disponível na saída ($V_{saida}$) rastreia a tensão ajustada na base ($V_{base}$), subtraindo a barreira de silício da junção Base-Emissor ($V_{BE} = 0,7\text{V}$):

$$V_{saida} = V_{base} - 0,7\text{V} \implies V_{base} = V_{saida} + 0,7\text{V}$$

Para alcançar a janela de regulação final na saída da fonte (de 3V a 12V), a base precisa receber os seguintes limites:

* **Para saída mínima de 3V:**
  $$V_{base\_minima} = 3\text{V} + 0,7\text{V} = 3,7\text{V}$$

* **Para saída máxima de 12V:**
  $$V_{base\_maxima} = 12\text{V} + 0,7\text{V} = 12,7\text{V}$$

### 3. Dimensionamento do Resistor de Proteção do Zener ($R_Z$)
O barramento bruto após a filtragem fornece 26V. O diodo Zener fixa firmemente a tensão em seu terminal em 13V (13,015V no simulador). A queda de potencial que o resistor de $1,2\text{k}\Omega$ precisa suportar isoladamente é:

$$V_{RZ} = V_{capacitor} - V_{Zener} = 26\text{V} - 13\text{V} = 13\text{V}$$

A corrente contínua que passa por ele para alimentar o diodo Zener é de:

$$I_{RZ} = \frac{V_{RZ}}{R_Z} = \frac{13\text{V}}{1200\Omega} \approx 0,0108\text{A} \quad (10,8\text{mA})$$

**Cálculo Crítico de Aquecimento (Potência):**

$$P_{RZ} = V_{RZ} \times I_{RZ} = 13\text{V} \times 0,0108\text{A} \approx 0,14\text{W}$$

Como a dissipação resultou em $0,14\text{W}$, um resistor padrão de película de carbono comum de $1/4\text{W}$ ($0,25\text{W}$) é plenamente capaz de operar neste ponto do circuito sem nenhum risco de fadiga térmica.

# Circuito Falstad
<img width="1293" height="837" alt="Image" src="https://github.com/user-attachments/assets/cff179f6-9f50-4843-80ac-7bbac707707a" />

Link para Simulação: [simulação falstad](https://falstad.com/circuit/circuitjs.html?ctz=DwYwlgTgBAZgvAIgIwKgFwM6IAwDpsEECsqYIiSRuALAOwBMtAbLQJy1JIAcTS2rqEACNEReqgAOIhNQDMqAG4RRqALaZRAUwC0nBAD4AUFCjAFUAB6Jt9bFyhdsUG3aj1q2VPGRNUAd28UWGUEJk8oVQBDCwUKRwQAeiMTYAAVS2tbe0coJ3dw73CwADtEalQISJxcJGpWeiJZWlkkWTl6LlkiWkEAe2rWQf8wCjDBbGs8LlY26jr+em5sWk7BJGqiJnpZWVZWrj5qekZxJONTPwzkJHoHJyd42BxE5NMAEyukZigkMJ+bqCsXxPBDhVT9BBvTQwSIAVwANmgXudgJcrAhZLdtNR7LJsNRnDivM8zik0Yg8QT8m5XPliaDkWSrpTCfYPE5sVx6Z5SRdmfi3B4oOzBQUSa9gG9VFBiqpEAAvTTFTQQXTyEFBMBYZC4DhyMQHeoHQj0HQ9KAQbXhUoIej4QgOx12cpQISxZDqmAjZCM0zyq51Jii4VAqBMF2FNQQxXK1WtX3AaDowNhgkpzncioUWw8iXkhBEeqskNBjMg3MoiRoK5hezhsOuX5in0RKrIB2SCF4QZEJ1OoIYeGIABKmgwWrQkWKIE0CaTiFrPz+i7p5azBfZCfzK6FKdXkd5qKuKwJnKghaxRLXh-zJ+DO+bFaZ6M4t1Dr7umcPAHN+VShSyHRcteeYBqG9aLuGX4StW6K0AKH53q+mZQBINpBEImgUAQUaIFCMIIkiUAKJhmRILgRBcHQrQrFRXD0YwihCOQoK4Cw0yYuGHCsNgXzlDex5UYCQZ3leB6gXBQk3PYSFjCBKLzggd7UrJj7rjcT6mAAMgAogAIgGBDFiK0koeCeHQnCiLaPCmhvIIIRBCA37PFAwiuaozHVLxc6GU40nCkZ+6uSEdA4YeIB-sWgEeChLF4IQdCkO6CU+VAGAhOEChvIgDS4HiTD0Vw2zYO4TDsL4AnolsBKIQhNzQSiHwvgCoY4kuakRBC+FWUih7NRQ3xNv8tzHBGHndZZhEJgN1yje4wr2E2ZmTQRiJbsyl64rcy3yc+FI7X87W7eJKL5h+Y0jaKjX7batJClkHU3Xy6KPcNXxBid4pnZ8ALUnksV7S9iB3meonAadKQAMogL0EiaDWDBuEjFXCmJzyvCkvRQEquUDhIC7jQgFjqgTCC6Kg6IAGq9IikTfpo0rFNjADCmjwgikQQAY5wmCkEjEa5GAsbYvp86YAvuuqwvVKcmOmAkvQSgkMNw5oEqq-DVzYr8bj0c4H5zBDGO86Y2O4wy6Vk1BIIk5I1hBOiABivTFGgCNQlALO9BAEBKu7UAAIKIiqxSRG8kQ8+L-OC5bMsYqLvLi8AksUqg8fkZV8vAIryua+rKL59rDSlh+Tjowy2fmza4hW4T9J26hDuU4gNN0wz0p+2gYBeiA4eRKh7MDwACq7Aee3pYC9G8vRR9HKex+E8exUnfML1L6fxbgXKrznSsoirsPwxrR8Iy+fw2Ej1D1tojyRlXOM2sCGDW0TjdkxTVxt5OHc4xgk5CDAPCMA8p+6D3hNjPuEhIjgDQD7OeydU5xxYiveWMcN7pS3nLU2e886nxPmrK4RAASX1uDwU8QJMwPwttwdOr8G6k2bl-WmP9GawCAWgSoEdwHYwAFpKhVAgteSCl4oM0og2O0st4EBaA0MWCt94pEPmrCSZQjJnnTBXTSR5kxBV3KGYKDIzg53ABAIwQA![Uploading circuit-20260610-1932.svg…]()
)

