# Ternarium
## Resumo
<p align="justify">
Este trabalho apresenta o projeto e a implementação do Ternarium, um datapath (caminho de dados) fundamentado em lógica ternária, explorando uma alternativa à arquitetura binária convencional. Enquanto a computação tradicional se limita a estados booleanos, a utilização de trits (unidades ternárias) permite uma maior densidade de informação e potencial redução na complexidade de interconexões. A pesquisa descreve a arquitetura da Unidade Lógica e Aritmética (ULA) ternária, integrando portas lógicas específicas como PNOT, NNOT e funções de consenso (AND/OR ternários), além da estruturação do fluxo de dados e registradores. Os resultados obtidos via simulação [mencione o software, como LTspice ou MATLAB] demonstram a viabilidade da execução de instruções em base 3, evidenciando os desafios e as vantagens da lógica multivalorada na eficiência de processamento. O projeto contribui para o campo de arquitetura de computadores ao propor uma infraestrutura funcional para sistemas não-binários, servindo de base para futuras implementações de processadores ternários completos.
</p>

## Base 3
<p align="justify">
A base 3, também conhecida como sistema ternário, é um sistema de numeração posicional que utiliza apenas três algarismos distintos (geralmente 0, 1 e 2) para representar qualquer valor numérico. Diferente do sistema binário (base 2) ou decimal (base 10), cada posição de um dígito em um número ternário corresponde a uma potência de 3, o que permite uma representação de dados que, em certas implementações teóricas e de hardware, oferece uma eficiência logarítmica superior à da lógica binária tradicional. A fórmula geral para converter um número de base 3 com n dígitos para a base 10 é dada pela soma ponderada: $V = \sum_{i=0}^{n-1} d_i \cdot 3^i$
</p>

## Complemento de 3
<p align="justify">
O complemento de 3 é o análogo no sistema ternário ao que o complemento de 2 representa no sistema binário, sendo utilizado para facilitar operações de subtração e representação de números negativos em hardware. Ele é classificado como o "complemento da base", onde para um número de n algarismos, o complemento de 3 é obtido subtraindo o número original de 3n ou, de forma mais prática, somando 1 ao seu complemento de 2 ternário (o complemento de B−1). Matematicamente, se temos um número N na base 3 com n dígitos, a fórmula para o seu complemento de 3 é: $C_3(N) = 3^n - N$
</p>

$$
210210_3 -> 012012_3 +1 -> 012020_3
$$
## Componentes eletrônicos
### n-MOS
<p align="justify">
O NMOS (N-channel Metal-Oxide-Semiconductor) é um transistor de efeito de campo que utiliza elétrons (cargas negativas) como os principais portadores de corrente, sendo o tipo mais comum de MOSFET devido à sua alta velocidade de comutação. Ele é construído sobre um substrato de silício tipo P, contendo duas regiões de silício tipo N que formam o dreno e a fonte. Em circuitos digitais, o NMOS atua como uma chave de "pull-down", conectando a saída do circuito ao terra (GND) para definir o nível lógico baixo.
O funcionamento do NMOS baseia-se na aplicação de uma tensão positiva no terminal do gate em relação à fonte (VGS​). Quando essa tensão ultrapassa o valor de limiar (Vth​), o campo elétrico atrai elétrons livres para a região abaixo do gate e repele as lacunas do substrato, formando um canal de condução N entre a fonte e o dreno. Ao contrário do PMOS, o NMOS "liga" quando recebe um sinal de nível lógico alto (ex: 5V ou 3.3V) e entra em estado de corte quando o gate está em 0V, permitindo que a corrente flua rapidamente devido à alta mobilidade dos elétrons.
</p>

### p-MOS
<p align="justify">
O PMOS (P-channel Metal-Oxide-Semiconductor) é um tipo de transistor de efeito de campo que utiliza lacunas (cargas positivas) como os principais portadores de corrente. Ele é construído sobre um substrato de silício do tipo N, com duas regiões de silício tipo P que formam o dreno e a fonte. No design de circuitos digitais, como a tecnologia CMOS, o PMOS atua como uma chave de "pull-up", sendo responsável por conectar a saída do circuito à tensão de alimentação positiva (VDD​) para definir o nível lógico alto.
O funcionamento do PMOS baseia-se na aplicação de uma tensão negativa no terminal do gate em relação à fonte (VGS​). Quando essa tensão cai abaixo de um determinado valor de limiar (Vth​), o campo elétrico repele os elétrons do substrato e atrai lacunas para a região abaixo do gate, criando um canal de condução P que interliga a fonte e o dreno. Dessa forma, ao contrário do NMOS, o PMOS "liga" quando recebe um sinal de nível lógico baixo (0V) e permanece em estado de corte (desligado) quando o gate recebe uma tensão alta, funcionando como uma chave inversora em relação ao sinal de controle.
</p>

## Portas Lógicas
### PNOT
<p align="justify">
O circuito PNOT opera como um inversor de lógica ternária onde o transistor PMOS atua como uma chave controlada pela tensão no gate (A): quando a entrada está em nível baixo (0V), o transistor entra em condução, permitindo que a corrente flua da fonte de +2V em direção ao dreno, elevando o potencial da saída (S) para o nível alto e dissipando energia através do resistor conectado ao −2V.  Por outro lado, quando a entrada (A) recebe uma tensão alta (+2V), o transistor entra em estado de corte, interrompendo a passagem de corrente e permitindo que o resistor "puxe" a tensão de saída (S) para o nível negativo de 0V.
</p>


<p align="center">
  <img src="Images/Circuits/PNOT.png" width="300">
</p>

<div align="center">

| A | S | 
|:--------:|:--------:|
| 0  | 2  | 
| 1  | 2  | 
| 2  | 0  | 

</div>

### SNOT
<p align="justify">
No circuito SNOT, quando a entrada A é 0V, o transistor PMOS (topo) é acionado fortemente devido à diferença de potencial negativa entre o Gate e a Fonte de +2V, permitindo que a corrente flua da fonte superior para a saída S, elevando-a para 2V enquanto o NMOS permanece em corte.  Se a entrada sobe para 2V, o PMOS corta e o NMOS (base) passa a conduzir, mas como o dreno do NMOS está conectado ao divisor de tensão central, a corrente flui de modo a equilibrar a saída S em 0V, assumindo que o ponto médio do circuito esteja referenciado a esse potencial. No estado intermediário de 1V, o circuito entra em uma zona de equilíbrio onde ambos os transistores operam de forma limitada ou equilibrada pelos resistores internos, forçando a saída S a se manter em 1V através da divisão de tensão entre os barramentos de +2V e −2V, estabilizando o sinal.
</p>

<p align="center">
  <img src="Images/Circuits/SNOT.png" width="300">
</p>

<div align="center">

| A | S | 
|:--------:|:--------:|
| 0  | 2  | 
| 1  | 1  | 
| 2  | 0  | 

</div>

### NNOT
<p align="justify">
No circuito NNOT a saída S é determinada pelo estado de condução do transistor NMOS em relação ao divisor de tensão formado pelo resistor de carga: quando a entrada A está em 0V, o transistor permanece em corte (desligado), permitindo que o resistor de pull-up eleve o potencial da saída para +2V; no entanto, quando a entrada sobe para os níveis de 1V ou 2V, a tensão no gate torna-se positiva o suficiente para saturar o NMOS, criando um caminho de baixa impedância que puxa a corrente para o barramento inferior e estabiliza a saída S em 0V.
</p>

<p align="center">
  <img src="Images/Circuits/NNOT.png" width="300">
</p>

<div align="center">

| A | S | 
|:--------:|:--------:|
| 0  | 2  | 
| 1  | 0  | 
| 2  | 0  | 

</div>

### NAND
<p align="justify">
A porta NAND ternária opera processando dois sinais de entrada para fornecer uma saída baseada na inversão do valor mínimo entre eles, utilizando uma configuração complementar de transistores para alternar entre os níveis lógicos 0, 1 e 2. No circuito, os transistores PMOS no topo funcionam como chaves que conectam a saída ao nível lógico máximo quando qualquer uma das entradas é baixa, enquanto a rede de transistores NMOS na parte inferior atua para puxar a saída para o nível lógico mínimo apenas quando ambas as entradas atingem níveis elevados. O comportamento da corrente e dos níveis de tensão é definido pela interação entre os pares de transistores: quando as entradas A ou B estão em 0, o caminho para o barramento superior é ativado, garantindo uma saída 2; se ambas as entradas sobem para o nível 2, os transistores inferiores conduzem plenamente, drenando a tensão da saída para o nível 0. Nos estados intermediários, onde pelo menos uma entrada está em 1, o circuito se equilibra para fornecer a inversão lógica correspondente, garantindo que a saída reflita sempre o estado oposto ao menor valor detectado nas entradas, mantendo a estabilidade do sinal em todo o sistema ternário.
</p>

<p align="center">
  <img src="Images/Circuits/NAND.png" width="300">
</p>

<div align="center">

| A | B | S | 
|:--------:|:--------:|:--------:|
| 0  | 0  | 2  |
| 0  | 1  | 2  |
| 0  | 2  | 2  |
| 1  | 0  | 2  |
| 1  | 1  | 1  |
| 1  | 2  | 1  |
| 2  | 0  | 2  |
| 2  | 1  | 1  |
| 2  | 2  | 0  |
</div>


### AND
<p align="center">
  <img src="Images/Circuits/AND.png" width="300">
</p>

<div align="center">

| A | B | S | 
|:--------:|:--------:|:--------:|
| 0  | 0  | 0  |
| 0  | 1  | 0  |
| 0  | 2  | 0  |
| 1  | 0  | 0  |
| 1  | 1  | 1  |
| 1  | 2  | 1  |
| 2  | 0  | 0  |
| 2  | 1  | 1  |
| 2  | 2  | 2  |
</div>

### OR
<p align="center">
  <img src="Images/Circuits/OR.png" width="300">
</p>

<div align="center">

| A | B | S | 
|:--------:|:--------:|:--------:|
| 0  | 0  | 0  |
| 0  | 1  | 1  |
| 0  | 2  | 2  |
| 1  | 0  | 1  |
| 1  | 1  | 1  |
| 1  | 2  | 2  |
| 2  | 0  | 2  |
| 2  | 1  | 2  |
| 2  | 2  | 2  |
</div>

### NOR
<p align="center">
  <img src="Images/Circuits/NOR.png" width="300">
</p>

<div align="center">

| A | B | S | 
|:--------:|:--------:|:--------:|
| 0  | 0  | 2  |
| 0  | 1  | 1  |
| 0  | 2  | 0  |
| 1  | 0  | 1  |
| 1  | 1  | 1  |
| 1  | 2  | 0  |
| 2  | 0  | 0  |
| 2  | 1  | 0  |
| 2  | 2  | 0  |
</div>

### XOR
<p align="center">
  <img src="Images/Circuits/XOR.png" width="300">
</p>

<div align="center">

| A | B | S | 
|:--------:|:--------:|:--------:|
| 0  | 0  | 0  |
| 0  | 1  | 2  |
| 0  | 2  | 1  |
| 1  | 0  | 2  |
| 1  | 1  | 1  |
| 1  | 2  | 0  |
| 2  | 0  | 1  |
| 2  | 1  | 0  |
| 2  | 2  | 2  |
</div>

### XNOR
<p align="center">
  <img src="Images/Circuits/XNOR.png" width="300">
</p>

<div align="center">

| A | B | S | 
|:--------:|:--------:|:--------:|
| 0  | 0  | 2  |
| 0  | 1  | 0  |
| 0  | 2  | 1  |
| 1  | 0  | 0  |
| 1  | 1  | 1  |
| 1  | 2  | 2  |
| 2  | 0  | 1  |
| 2  | 1  | 2  |
| 2  | 2  | 0  |
</div>

## Algebra Ternária
### AND e NAND
Durante a operação AND, a saída S sera sempre o menor dos valores dentre as entradas A e B podendo ser definida pela formula $S = min(A,B)$. Já a operação NAND baseiase na negação neutra (SNOT) da saída S da operação AND, sendo definida por $S = ¬(min(A,B))$.
### OR e NOR
Diferente da operação AND, a saída S da operação OR é o maior valor das entradas A e B, definida pela formula $S = max(A,B)$.  Já a operação NOR de modo similar à NAND é a negação neutra da saída S $S = ¬(max(A,B))$.
### XOR e XNOR
### Comutação
### Associação
### Distribuição
### Absorção
### Idempotência
## Circuitos 
### Decodificador
### Somador
### Subtrator
### Multiplicador
### Divisor
### Comparador
### Multiplexador
### Demultiplexador
### Flip-flop
#### C-Flip-flop
#### D-Flip-flop
#### T-Flip-flop
### Registrador
## Datapath
### Memória de Instruções
### Banco de Registradores
### ULA
### Controle
### Memória
## Referências
