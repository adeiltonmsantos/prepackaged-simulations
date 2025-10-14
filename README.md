# prepackaged-simulations

## Do que se trata

Aplicação desenvolvida no Google Colab que realiza Simulações de Monte Carlo para estimar a probabilidade de reprovação em exames de produtos pré-embalados


## Contextualização

Órgãos Executores do INMETRO são responsáveis pela fiscalização de produtos pré-embalados, que via de regra são quaisquer mercadorias que são medidas na ausência do consumidor. Sendo assim, são classificadas como mercadorias pré-embaladas:

- Sacos de arroz de 1kg
- Refrigerantes em garrafas de 1 litro
- Biscoitos em embalagens de 120 g
- Papel higiênico de 30 m
- Sucos de fruta em embalagem longa vida de 200 mL, etc.

O Regulamento Técnico do INMETRO dispõe de um plano de amostragem que determina o tamanho de amostras para exame a depender do tamanho do lote. Neste contexto:

- Quanto maior o tamanho do lote, maior o tamanho da amostra
- Quanto maior o tamanho da amostra, mais rígidos são os critérios para aprovação, visto que tamanhos maiores de amostras são mais representativos quanto ao lote original
- Em outras palavras, quanto menor o tamanho do lote, menos rígidos são os critérios para aprovação, dado o risco de as amostras menores não serem suficientemente representativas

## O problema

Quando amostras menores são examinadas quanto a seu conteúdo, os critérios menos rigorosos podem resultar em falsos positivos, ou seja, uma amostra pode ser aprovada quando na verdade deveria ser reprovada

## A solução

Um laudo em PDF de um exame que tenha sido aprovado como possível falso positivo é carregado pela aplicação, a qual extrai:

- As informações do produto (nome, conteúdo nominal, etc.)
- As medições realizadas no exame

Com as informações a aplicação calcula média e desvio padrão das medições e gera 10.000 valores aleatórios de medições, com distribuição normal, para cada tamanho de amostra prevista no Regulamento Técnico do INMETRO. Em outras palavras:

- 10.000 amostras de tamanho 13: total de 130.000 valores com distribuição normal
- 10.000 amostras de tamanho 20: total de 200.000 valores com distribuição normal
- 10.000 amostras de tamanho 32: total de 320.000 valores com distribuição normal
- 10.000 amostras de tamanho 80: total de 800.000 valores com distribuição normal

**Um total de 1.450.000 valores!**

Em cada amostra gerada por simulação de Monte Carlo são aplicados os critérios de avaliação do Regulamento Técnico do INMETRO, possibilitando assim estimar a probabilidade de reprovação para cada tamanho de amostra. Sendo evidenciado que na análise da amostra original pode ter havido falso positivo, o IPEM pode direcionar sua força de trabalho em busca de maiores amostras daquele produto para uma nova avaliação

Você pode fazer um teste clicando no link abaixo

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/adeiltonmsantos/prepackaged-simulations/blob/main/prepackaged_simulations.ipynb)

## Considerações finais

De acordo com o Teorema Central do Limite, muitas medições no mundo real são o resultado cumulativo ou a soma de muitos fatores aleatórios e independentes. O Teorema Central do Limite, ao afirmar que a distribuição da soma (ou média) de variáveis independentes se torna normal, justifica por que a distribuição normal é um modelo adequado para as medições dos exames de mercadorias pré-embaladas, onde o resultado final é a agregação de múltiplos efeitos aleatórios. Essa é a razão pela qual os valores aleaórios gerados por Simulação de Monte Carlo nesta aplicação devem seguir distribuição normal em conformidade com a amostra original

É considerada a possibilidade de a amostra original não seguir distribuição normal. Por essa razão, as Simulações de Monte Carlo só são realizadas pela aplicação se a amostra original efetivamente seguir uma distribuição normal, o que é avaliado por um teste de aderência adequado (Kolmogorov-Smirnov ou Shapiro-Wilk). Caso a aplicação determine que a amostra original não siga distribuição normal, uma mensagem avisa ao usuário a inviabilidade de se realizar as simulações
