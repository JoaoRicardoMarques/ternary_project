# Ternarium
## Resumo
<p align="justify">
Este trabalho apresenta o projeto e a implementação do Ternarium, um datapath (caminho de dados) fundamentado em lógica ternária, explorando uma alternativa à arquitetura binária convencional. Enquanto a computação tradicional se limita a estados booleanos, a utilização de trits (unidades ternárias) permite uma maior densidade de informação e potencial redução na complexidade de interconexões. A pesquisa descreve a arquitetura da Unidade Lógica e Aritmética (ULA) ternária, integrando portas lógicas específicas como PNOT, NNOT e funções de consenso (AND/OR ternários), além da estruturação do fluxo de dados e registradores. Os resultados obtidos via simulação [mencione o software, como LTspice ou MATLAB] demonstram a viabilidade da execução de instruções em base 3, evidenciando os desafios e as vantagens da lógica multivalorada na eficiência de processamento. O projeto contribui para o campo de arquitetura de computadores ao propor uma infraestrutura funcional para sistemas não-binários, servindo de base para futuras implementações de processadores ternários completos.
</p>

## Base 3
<p align="justify">
A base 3, também conhecida como sistema ternário, é um sistema de numeração posicional que utiliza apenas três algarismos distintos (geralmente 0, 1 e 2) para representar qualquer valor numérico. Diferente do sistema binário (base 2) ou decimal (base 10), cada posição de um dígito em um número ternário corresponde a uma potência de 3, o que permite uma representação de dados que, em certas implementações teóricas e de hardware, oferece uma eficiência logarítmica superior à da lógica binária tradicional. A fórmula geral para converter um número de base 3 com n dígitos para a base 10 é dada pela soma ponderada: $V = \sum_{i=0}^{n-1} d_i \cdot 3^i$
</p>


## Base de 27
## Complemento de 3
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

### NAND
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
