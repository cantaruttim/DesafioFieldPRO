# Avaliação Técnica - Desafio FieldPRO
<hr />

<br />

O objetivo deste desafio é construir um modelo de calibração de um sensor de chuva baseado em impactos mecânicos.
O sistema de medição de chuva funciona por meio de uma placa eletrônica com um piezoelétrico, um acumulador de carga e um sensor de temperatura. Os dados são transmitidos de hora em hora

<br />

## Análise Exploratória dos Dados
<hr />

Uma análise inicial foi realizada com o intuito de identificar o tipo dos dados em cada coluna, se existiam dados faltantes (que foram tratados - 6 dados faltantes) e posteriormente um gráfico de correlação foi utilizado para tentar entender a dependência das variáveis. Descobriu-se que o número de resets realizados pela máquina era a variável mais importante para que houvesse o acumulo de energia. Porem todas as variáveis foram utilizadas devido a quantidade baixa de variáveis dependentes.

A temperatura do ambiente influenciava também na temperatura do aparelho. Porem não foi medido, apenas mostrado. Foi a única variável que se correlacionavam positivamente. As medições marjoritariamente foram realizadas em temperatura ambiente e poucas acima dos 30º. Uma outra variável que influencia normalmente nas chuvas é a pressão atmosférica, porem apresentou pouca informação quanto correlacionada ao acumulo de energia.


## Discussão Técnica do Desafio
<hr />

Por meio deste desafio estamos procurando resolver um problema de classificação. Como não possuímos os `rótulos` de cada uma das medições, podemos perceber que precisariamos procurar agrupar os dados para então podermos classificá-lo posteriormente.

Desta forma, após a realização dos tratamentos dos dados, realizei o agrupamento por meio do algoritmo `KMeans` (clusterização) para podermos colocar em dois grupos (0 e 1), que foram utilizados como variável `target`. Observamos que os dados criados ficam desbalanceados. Dessa forma apliquei a técnica SMOTE para criar dados sintéticos para manter os dados balanceados e não haver nenhum tipo de viés dentro dos modelos utilizados.

`np.unique(dataset['Groups'], return_counts=True)` <br />
`(array([0, 1], dtype=int32), array([790, 921]))`

**_Ponto interessante_**: Foi realizado a modelagem sem a correção do desbalanceamento dos dados, porem a acurácia dos dados caiu significativamente. Então não foi colocado no código. Para resolver esse problema, eu utilizei o teste de Hipótese com o teste ANOVA para comparar e avaliar qual teria sido o melhor algoritmo para resover esse problema em questão.

### Acurácia

A acurácia dos modelos não foram medidas diretamente. Utilizei a validação cruzada por meio do `KFold` onde analisei o resultado médio de 30 interações. Dividindo o dataset em 10 splits (Recomendado pela literatura da área de _machine learning_)

Por meio disso, utilizei a análise estatística para verificar a rejeitão das Hipóteses H0/H1:

<img width="585" alt="tabela Hipotese" src="https://github.com/cantaruttim/DesafioFieldPRO/assets/81988636/2ed87338-be1d-4f79-a52d-6e42723522cd">

Em todos os testes realizados as redes neurais obtiveram os melhores resultados significativamente melhores quando comparado com os outros modelos. Por mais que os outros modelos pudesse apresentar acurácias maiores, as redes neurais apresentaram maior estabilidade, melhor valor de p e métricas mais estáveis do que os outros modelos. Com uma acurácia de 99.73% [IC 95%].
