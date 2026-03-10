# Ternarium
## Resumo
<p align="justify">
Este trabalho apresenta o projeto e a implementação do Ternarium, um datapath (caminho de dados) fundamentado em lógica ternária, explorando uma alternativa à arquitetura binária convencional. Enquanto a computação tradicional se limita a estados booleanos, a utilização de trits (unidades ternárias) permite uma maior densidade de informação e potencial redução na complexidade de interconexões. A pesquisa descreve a arquitetura da Unidade Lógica e Aritmética (ULA) ternária, integrando portas lógicas específicas como PNOT, NNOT e funções de consenso (AND/OR ternários), além da estruturação do fluxo de dados e registradores. Os resultados obtidos via simulação LTspice demonstram a viabilidade da execução de instruções em base 3, evidenciando os desafios e as vantagens da lógica multivalorada na eficiência de processamento. O projeto contribui para o campo de arquitetura de computadores ao propor uma infraestrutura funcional para sistemas não-binários, servindo de base para futuras implementações de processadores ternários completos.
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

### AND
<p align="justify">
A porta AND ternária opera processando dois sinais de entrada para fornecer uma saída baseada no valor mínimo entre eles, utilizando uma estrutura composta por uma porta NAND seguida de um inversor ternário do tipo SNOT em sua saída. No primeiro estágio do circuito, os transistores PMOS e NMOS realizam a operação lógica de NAND, onde os transistores de topo garantem o nível máximo quando há entradas baixas, enquanto a rede inferior puxa o sinal para o nível mínimo apenas quando ambas as entradas são elevadas.
O estágio final do circuito utiliza uma negação neutra SNOT (Simple NOT) conectada à saída da NAND para inverter o resultado, transformando a lógica negativa em uma função AND direta. Este inversor SNOT, composto por um par complementar de transistores e resistores em série, garante que a saída S reflita exatamente o menor valor detectado entre as entradas A e B, estabilizando o sinal nos níveis 0, 1 ou 2. Dessa forma, a corrente flui através do estágio de negação para retificar os níveis lógicos, permitindo que o circuito execute a função de mínimo necessária para a aritmética do sistema ternário.
</p>
<p align="center">
  <img src="Images/Circuits/AND.png" width="300">
</p>


### NOR
<p align="justify">
A porta NOR ternária implementa a inversão do valor máximo entre as duas entradas, utilizando uma configuração onde os transistores PMOS estão dispostos em série e os NMOS em paralelo. Quando ambas as entradas A e B estão no nível lógico 0, o caminho através dos transistores superiores é totalmente estabelecido, conectando a saída diretamente ao nível lógico 2. Se qualquer uma das entradas subir para o nível 2, o transistor NMOS correspondente entra em condução plena, superando a rede superior e puxando a saída para o nível lógico 0. Nos estados onde o maior valor entre as entradas é 1, o circuito atua através de um equilíbrio entre a condução parcial dos transistores e a rede de resistências internas, garantindo que a saída se estabilize no nível intermediário 1. O comportamento da corrente flui do barramento superior para o inferior com intensidades variadas conforme a combinação das entradas, permitindo que a porta execute a lógica de inversão necessária para o processamento ternário. Essa estrutura assegura que a saída seja sempre o oposto complementar do maior sinal detectado, mantendo a consistência dos níveis lógicos em todo o sistema.
</p>

<p align="center">
  <img src="Images/Circuits/NOR.png" width="300">
</p>

### OR
<p align="justify">
A porta OR ternária opera processando dois sinais de entrada para fornecer uma saída baseada no valor máximo entre eles, utilizando uma estrutura composta por uma porta NOR seguida de um inversor ternário do tipo SNOT em sua saída. No estágio inicial do circuito, a rede de transistores PMOS em série e NMOS em paralelo realiza a operação lógica NOR, onde a saída é conduzida para o nível máximo apenas se ambas as entradas forem baixas, sendo puxada para o nível mínimo caso qualquer entrada atinja o valor 2.
O estágio final do circuito utiliza uma negação neutra SNOT (Simple NOT) conectada à saída da NOR para inverter o resultado, transformando a lógica negativa em uma função OR direta que retorna o valor máximo. Este inversor SNOT, equipado com seu divisor de tensão resistivo, garante que a saída S se estabilize corretamente nos níveis 0, 1 ou 2, retificando a inversão prévia para que o sinal final corresponda à união lógica das entradas A e B.
</p>
<p align="center">
  <img src="Images/Circuits/OR.png" width="300">
</p>


### XOR
A porta XOR ternária apresentada é um circuito combinacional que processa os sinais de entrada através de três estágios lógicos distintos: uma porta NAND, uma OR e uma AND final. No primeiro nível do circuito, as entradas A e B são enviadas simultaneamente para a NAND superior e para a OR inferior, onde a NAND identifica o inverso do valor mínimo e a OR identifica o valor máximo entre as entradas.
No estágio final, os resultados dessas duas operações são processados pela porta AND à direita, que extrai o valor mínimo entre a saída da NAND e a saída da OR para gerar o sinal final S. Esse arranjo garante que a saída atinja o nível lógico máximo apenas quando houver uma disparidade entre as entradas, mantendo a coerência dos níveis 0, 1 e 2 dentro da aritmética ternária.
<p align="center">
  <img src="Images/Circuits/XOR.png" width="300">
</p>


### XNOR
A porta XNOR é um circuito que expande a lógica da XOR ao adicionar um estágio final de inversão para complementar o resultado. O processamento inicial ocorre através de uma porta NAND superior e uma OR inferior que recebem as entradas A e B em paralelo, seguido por uma porta AND que extrai o valor mínimo entre essas duas operações intermediárias.
O estágio final do circuito utiliza uma negação neutra SNOT conectada à saída da porta AND para inverter o sinal resultante e gerar a saída final S. Esta negação garante que o circuito execute a função de coincidência ternária, onde a saída atinge o nível máximo (2) quando as entradas são idênticas e níveis inferiores quando há divergência entre os sinais de entrada.
<p align="center">
  <img src="Images/Circuits/XNOR.png" width="300">
</p>


## Algebra Ternária
### AND e NAND
Durante a operação AND na lógica ternária, a saída S corresponde sempre ao menor valor entre as entradas A e B, podendo ser representada matematicamente pela função $S = min(A, B)$. Já a operação NAND é definida como a aplicação da negação neutra (SNOT) sobre o resultado da operação AND; dessa forma, a saída é obtida pela expressão  $S = ¬(min(A, B))$, ou seja, primeiro determina-se o menor valor entre A e B e, em seguida, aplica-se a negação neutra a esse resultado.
<div align="center">

| A | B | S | ¬S |
|:--------:|:--------:|:--------:|:--------:|
| 0  | 0  | 0  | 2 |
| 0  | 1  | 0  | 2 |
| 0  | 2  | 0  | 2 |
| 1  | 0  | 0  | 2 |
| 1  | 1  | 1  | 1 |
| 1  | 2  | 1  | 1 |
| 2  | 0  | 0  | 2 |
| 2  | 1  | 1  | 1 |
| 2  | 2  | 2  | 0 |
</div>

### OR e NOR
Diferentemente da operação AND, na operação OR da lógica ternária a saída S corresponde ao maior valor entre as entradas A e B, sendo representada pela expressão $S = max(A, B)$. De forma semelhante ao que ocorre com a operação NAND, a operação NOR é definida como a negação neutra do resultado da operação OR, podendo ser expressa por $S = ¬(max(A, B))$, ou seja, primeiro determina-se o maior valor entre A e B e, em seguida, aplica-se a negação neutra ao resultado obtido.
<div align="center">

| A | B | S | ¬S |
|:--------:|:--------:|:--------:|:--------:|
| 0  | 0  | 0  | 2 |
| 0  | 1  | 1  | 1 |
| 0  | 2  | 2  | 0 |
| 1  | 0  | 1  | 1 |
| 1  | 1  | 1  | 1 |
| 1  | 2  | 2  | 0 |
| 2  | 0  | 2  | 0 |
| 2  | 1  | 2  | 0 |
| 2  | 2  | 2  | 0 |
</div>

### XOR e XNOR
Na lógica ternária, a operação XOR (ou “ou exclusivo”) pode ser definida como aquela em que a saída S assume um valor diferente quando as entradas A e B são distintas, refletindo a diferença entre elas. De forma geral, essa operação pode ser representada pela expressão S = |A − B|, indicando que a saída corresponde ao valor absoluto da diferença entre as entradas. Já a operação XNOR corresponde ao complemento da XOR, representando a condição em que as entradas possuem o mesmo valor ou apresentam equivalência lógica; assim, sua saída pode ser definida como a negação da operação XOR, sendo expressa por S = ¬(|A − B|). Dessa forma, enquanto a XOR destaca a diferença entre as entradas, a XNOR indica sua equivalência.
<div align="center">

| A | B | S | ¬S |
|:--------:|:--------:|:--------:|:--------:|
| 0  | 0  | 0  | 2 |
| 0  | 1  | 2  | 0 |
| 0  | 2  | 1  | 1 |
| 1  | 0  | 2  | 0 |
| 1  | 1  | 1  | 1 |
| 1  | 2  | 0  | 2 |
| 2  | 0  | 1  | 1 |
| 2  | 1  | 0  | 2 |
| 2  | 2  | 2  | 0 |
</div>

### Comutação
A propriedade comutativa na lógica ternária estabelece que a ordem das entradas não altera o resultado da operação lógica. Em outras palavras, trocar as posições das variáveis A e B não modifica o valor da saída S.
### Associação
A propriedade associativa na lógica ternária estabelece que o agrupamento das operações não altera o resultado final, desde que a ordem das variáveis seja mantida. Em outras palavras, ao realizar operações lógicas com três ou mais entradas, é possível agrupar os operandos de diferentes maneiras sem modificar a saída S.
### Distribuição
A propriedade distributiva na lógica ternária estabelece que uma operação lógica pode ser distribuída sobre outra sem alterar o resultado final da expressão.
### Absorção
A propriedade de absorção na lógica ternária estabelece que determinadas combinações entre as operações AND e OR permitem simplificar expressões lógicas sem alterar o resultado final. Essa propriedade mostra que uma variável pode “absorver” uma expressão que a contenha.
### Idempotência
A propriedade de idempotência na lógica ternária estabelece que, ao aplicar uma mesma operação lógica entre duas entradas iguais, o resultado permanece igual ao valor dessas entradas.
## Circuitos 
### Decodificador
### Somador
#### Somador incompleto (Half-Adder)
<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{sum} = A^{2}B^{0} + A^{1}B^{1}A^{0}B^{2} + 1\cdot(A^{1}B^{0} + A^{0}B^{1} + A^{2}B^{2})
$$

<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{carry} = 0 + 1\cdot(A^{2}B^{1} + A^{1}B^{2} + A^{2}B^{2})
$$

#### Somador completo (Full-Adder)
<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{sum} = A^{2}B^{0}C^{0} + A^{1}B^{0}C^{1} + A^{0}B^{0}C^{2} + A^{1}B^{1}C^{0} + A^{0}B^{1}C^{1} + A^{2}B^{1}C^{2} + A^{0}B^{2}C^{0} + A^{2}B^{2}C^{1} + A^{1}B^{2}C^{2} + (A^{1}B^{0}C^{0} + A^{0}B^{0}C^{1} + A^{2}B^{0}C^{2} + A^{0}B^{1}C^{0} + A^{2}B^{1}C^{1} + A^{1}B^{1}C^{2} + A^{2}B^{2}C^{0} + A^{1}B^{2}C^{1} + A^{0}B^{2}C^{2})
$$

<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{carry} = A^{2}B^{2}C^{2} + (A^{2}C^{2} + A^{0}B^{2} + A^{2}B^{2} + A^{1}C^{2} + A^{2}C^{1} + B^{2}C^{1} + B^{1}C^{2} + B^{2}C^{2} + A^{1}B^{1}C^{1})
$$

### Subtrator
<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{sub} = B_{0}^{0}[X_{1}^{0}X_{2}^{1} + X_{1}^{2}X_{1}^{1}X_{2}^{2}] + B_{0}^{1}[X_{1}^{0}X_{2}^{0} + X_{1}^{1}X_{2}^{1} + X_{1}^{2}X_{2}^{2}] + B_{0}^{2}[X_{1}^{1}X_{2}^{0} + X_{1}^{0}X_{2}^{2} + X_{1}^{2}X_{2}^{1}] + [B_{0}^{0}(X_{1}^{1}X_{2}^{0} + X_{1}^{2}X_{2}^{1} + X_{1}^{0}X_{2}^{2}) + B_{0}^{1}(X_{1}^{0}X_{2}^{0} + X_{1}^{1}X_{2}^{2}) + B_{0}^{2}(X_{1}^{0}X_{2}^{0} + X_{1}^{1}X_{2}^{1} + X_{1}^{2}X_{2}^{2})]
$$

<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{borrow} = B_{0}^{2}[2(X_{1}^{1} + X_{2}^{1})] + 2[X_{1}^{0}X_{2}^{1} + X_{1}^{0}X_{2}^{2}] + 2[X_{1}^{1}X_{2}^{2}] + X_{1}^{2}X_{2}^{2}B_{0}^{1} + X_{1}^{1}X_{2}^{0}B_{0}^{2}
$$

### Multiplicador
<div align="center">

| A | B | Cin | S | Cout | 
|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  | 0  | 0  | 0 | 0 |
| 0  | 1  | 0  | 1 | 0 |
| 0  | 2  | 0  | 2 | 1 |
| 1  | 0  | 0  | 1 | 0 |
| 1  | 1  | 0  | 2 | 0 |
| 1  | 2  | 0  | 0 | 0 |
| 2  | 0  | 0  | 2 | 0 |
| 2  | 1  | 0  | 0 | 1 |
| 2  | 2  | 0  | 1 | 1 |
| 0  | 0  | 1  | 1 | 0 |
| 0  | 1  | 1  | 1 | 0 |
| 0  | 2  | 1  | 0 | 1 |
| 1  | 0  | 1  | 2 | 0 |
| 1  | 1  | 1  | 0 | 1 |
| 1  | 2  | 1  | 1 | 1 |
| 2  | 0  | 1  | 0 | 1 |
| 2  | 1  | 1  | 1 | 1 |
| 2  | 2  | 1  | 2 | 1 |
| 0  | 0  | 2  | 2 | 0 |
| 0  | 1  | 2  | 0 | 1 |
| 0  | 2  | 2  | 1 | 1 |
| 1  | 0  | 2  | 0 | 1 |
| 1  | 1  | 2  | 1 | 1 |
| 1  | 2  | 2  | 2 | 1 |
| 2  | 0  | 2  | 1 | 1 |
| 2  | 1  | 2  | 2 | 1 |
| 2  | 2  | 2  | 0 | 2 |
</div>

<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{sum} = C_{in}^{0}[A_{1}^{2}B_{1}^{0}+A_{1}^{1}B_{1}^{1}+A_{1}^{0}B_{1}^{2}] + C_{in}^{1}[A_{1}^{1}B_{1}^{0}+A_{1}^{0}B_{1}^{1}+A_{1}^{2}B_{1}^{2}] + C_{in}^{2}[A_{1}^{0}B_{1}^{0}+A_{1}^{2}B_{1}^{1}+A_{1}^{1}B_{1}^{2}] + [C_{in}^{0}(A_{1}^{1}B_{1}^{0}+A_{1}^{0}B_{1}^{1}+A_{1}^{2}B_{1}^{2}) + C_{in}^{1}(A_{1}^{0}B_{1}^{0}+A_{1}^{2}B_{1}^{1}+A_{1}^{1}B_{1}^{2}) + C_{in}^{2}(A_{1}^{2}B_{1}^{0}+A_{1}^{1}B_{1}^{1}+A_{1}^{0}B_{1}^{2})]
$$

<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
F_{carry} = A_{1}^{2}B_{1}^{2}C_{in}^{2} + [A_{1}^{2}B_{1}^{1}C_{in}^{0} + A_{1}^{2}B_{1}^{2}C_{in}^{0} + A_{1}^{1}B_{1}^{2}C_{in}^{0} + A_{1}^{1}B_{1}^{1}C_{in}^{1}] + 2[(B_{1}^{1}C_{in}^{2} + B_{1}^{2}C_{in}^{2}) + (A_{1}^{1}C_{in}^{2} + A_{1}^{2}C_{in}^{2}) + (A_{1}^{2}C_{in}^{2})]
$$

### Divisor
### Comparador
<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
A = B = (A_{0}^{1}B_{0}^{0} + A_{1}^{1}B_{1}^{1} + A_{1}^{2}B_{1}^{2})(A_{0}^{0}B_{0}^{0} + A_{0}^{2}B_{0}^{2}) + A_{1}^{0}B_{1}^{0}(A_{0}^{1}B_{1}^{0} + A_{1}^{1}B_{1}^{1} + A_{1}^{2}B_{1}^{2})
$$

<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
A < B = A_{0}^{0}A_{1}^{1}B_{1}^{0}B_{1}^{1} + A_{0}^{0}A_{1}^{2}B_{1}^{1}B_{1}^{2} + A_{1}^{1}A_{1}^{1}B_{0}^{2}B_{1}^{1} + 2A_{1}^{0}B_{1}^{1} + 2A_{1}^{0}B_{1}^{2} + 2A_{1}^{1}B_{1}^{2} + 2B_{0}^{2}B_{1}^{2}(A_{0}^{0} + A_{0}^{1}) + A_{0}^{0}A_{1}^{0}B_{1}^{0} + 2A_{0}^{0}B_{0}^{2}(A_{0}^{0} + A_{0}^{1})
$$
  
<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

$$
A > B = A_{0}^{2}A_{1}^{1}B_{0}^{0}B_{1}^{1} + A_{1}^{0}A_{1}^{1}B_{0}^{0}B_{1}^{1} + A_{0}^{2}A_{1}^{1}B_{1}^{1} + A_{0}^{2}A_{1}^{2}B_{1}^{0}B_{1}^{0} + A_{0}^{2}A_{1}^{2}B_{1}^{0} + 2B_{0}^{0}B_{1}^{0}(A_{0}^{1} + A_{0}^{2}) + 2A_{1}^{2}B_{0}^{0}(A_{0}^{1} + A_{0}^{2}) + 2A_{1}^{1}B_{1}^{0} + 2A_{1}^{2}B_{1}^{1} + 2A_{1}^{2}B_{1}^{0}
$$

### Multiplexador
<div align="center">

| A/B | 0 | 1 | 2 | \ | 0 | 1 | 2 | \ | 0 | 1 | 2 |
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 0  |   |   |   | \ |   |   |   | \ |   |   |   |
| 1  |   |   |   | \ |   |   |   | \ |   |   |   |
| 2  |   |   |   | \ |   |   |   | \ |   |   |   |
|C|C = 0|C = 0|C = 0| \ |C = 1|C = 1|C = 1| \ |C = 2|C = 2|C = 2|
</div>

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
