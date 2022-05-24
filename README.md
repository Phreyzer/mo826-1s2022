# Projeto 2 – Predizendo Prognóstico de Mortalidade com Dados Sintéticos
# Apresentação
O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [_Ciência e Visualização de Dados em Saúde_](https://ds4h.org/), oferecida no primeiro semestre de 2022, na Unicamp.

## Integrantes

<div align="center">
	
	
Nome | RA | Especialização
----|--|-----
Caroline Gerbaudo Nakazato | 168913 | Ciência da Computação
Luan de Oliveira Silveira | 204099 | Ciência da Computação
Wilson Bagni Junior | 010097 | Ciência da Computação
</div>


# Contextualização da Proposta

O [Synthea](https://synthea.mitre.org/) é um gerador de dados médicos de pacientes virtuais, ou seja, pacientes e encontros médicos sintéticos. O grande benefício de usar esse tipo de dados é seu potencial educacional, de pesquisa ou inovação sem as restrições legais de dados médicos verdadeiros, enquanto mantém uma simulação acurada de dados reais.
Para o presente trabalho contamos com 4 cenários gerados a partir do Synthea, e temos como objetivo a construção de modelos de aprendizado de máquina para a predição do tempo de vida de um paciente sintético. Cada base de dados possui aproximadamente a mesma estrutura e contém uma série de arquivos em formato csv:

- allergies.csv -> Dados de alergias à medicamentos, comidas ou condições ambientais
- careplans.csv -> Dados de tratamentos feitos pelo paciente
- claims.csv -> Dados dos custos médicos e plano de saúde
- claims_transactions.zip -> Dados de pagamento dos custos médicos
- conditions.csv -> Diagnóstico médico do paciente em uma determinada consulta
- devices.csv -> Equipamentos médicos utilizados pelo paciente
- encounters.csv -> Dados de encontros médicos
- imaging_studies.csv -> Resultados de exames médicos feitos com imagem
- immunizations.csv -> Vacinas tomadas pelo paciente
- medications.csv -> Medicamentos tomados pelo paciente
- observations.csv -> Medições médicas em um dado encontro
- organizations.csv -> Unidades de saúde que realizaram os atendimentos
- patients.csv -> Dados pessoais de cada paciente
- payer_transitions.csv -> Dados de transações financeiras
- payers.csv -> Pagadores das despesas médicas
- procedures.csv -> Dados sobre motivações de cada consulta
- providers.csv -> Dados dos médicos que realizaram as consultas
- supplies.csv -> Suprimentos médicos utilizados em cada consulta 

Para mais detalhes sobre a estrutura destes dados, um dicionário detalhado (em inglês) pode consultando neste link: https://github.com/synthetichealth/synthea/wiki/CSV-File-Data-Dictionary.

## Evolução do Projeto

A variedade de condições médicas abriu a possibilidade para diferentes ideias para a predição do tempo de vida do paciente. Dentre as propostas consideradas, 4 delas merecem destaque:
- Tempo de vida de um paciente com câncer baseado em suas despesas médicas e cobertura do plano de saúde. Neste caso consideramos que uma pessoa com câncer tem maiores chances de morrer caso não seja capaz de continuar arcando com as despesas do tratamento. Contudo, este caminho foi abandonado devido a falta de conhecimento de como os dados foram gerados, podendo ser considerado apenas o custo final após o tratamento e a cura da doença.
- Prognóstico de um paciente com depressão considerando fatores como ansiedade, estresse e vínculo empregatício. Neste caso, o objetivo era correlacionar fatores agravantes da depressão como a ansiedade e o estresse, mas também analisar o papel que o trabalho do indivíduo exerce neste cenário. Se conjecturava a possibilidade de pessoas desempregadas e ociosas como mais suscetíveis a casos graves de depressão.
- Incidência de doenças respiratórias baseado no endereço de residência e etnia do indivíduo. É comum que nos EUA pessoas negras e de baixa renda habitem regiões próximas à polos industriais e desfrutem de uma baixa qualidade do ar. Estudos sugerem afrodescendentes são expostos a cerca de 38% mais ar poluído que brancos, e que é 75% mais provável que morem em comunidades que façam borda com uma planta industrial. [This is Environmental Racism - Darryl Fears and Brady Dennis](https://www.washingtonpost.com/climate-environment/interactive/2021/environmental-justice-race/?itid=lk_interstitial_manual_21) [1]
- Prognóstico de um paciente diagnosticado com COVID-19 baseando-se tanto em fatores de risco como obesidade e problemas respiratórios, quanto em termos genéticos como sexo, raça e etnia. 

Dentre as opções possíveis, o grupo optou pela última (Prognóstico de um paciente diagnosticado com COVID-19) devido à relevância da COVID-19 no cenário atual e a hipótese de que a quantidade de dados disponíveis seria insuficiente para evidenciar uma correlação relevante nos demais casos.

Como objetivo inicial, o projeto visava a predição de morte para a COVID-19 através da correlação com diversos fatores biológicos como imunização, obesidade, hipertensão, idade, gênero, além de fatores socioeconômicos como cobertura do plano de saúde e endereço de residência. Contudo, devido a escassez destes dados, optou-se pela redução dos parâmetros apenas para o status de imunização, idade, etnia, raça e gênero, uma vez que os diferentes métodos de classificação requerem uma grande quantidade de dados para convergir a uma solução satisfatória. Essa redução foi feita sobre o pressuposto de que estas variáveis tem um papel mais significativo no prognóstico de morte de uma pessoa com COVID-19 e que a intersecção entre as diferentes condições médicas tornaria os dados ainda mais escassos, o que enviesaria o modelo de predição.

## Ferramentas Utilizadas
- Orange Software
- Jupyter Notebook

# Metodologia
Após a escolha da infecção pela COVID-19 como condição à qual seria predita a morte, deu-se inicio à etapa de mineração dos dados para os cenários um e dois, sendo seu objetivo a obtenção de um arquivo .csv com as variáveis de predição e a variável objetivo (prognóstico). Nesta etapa, foram utilizados os arquivos "patients.csv" e "condition.csv" para extrair da lista inicial de pacientes aqueles que foram diagnosticados com COVID-19, além de suas datas de nascimento, cor de pele, etnia, gênero, diagnóstico da condição e morte. Em seguida, realizou-se a busca de cada um desses pacientes no arquivo "immunizations.csv" e criou-se uma nova coluna para que fossem categorizados em imunizados (1) e não-imunizados (0). 
Visando um modelo mais assertivo, optou-se por discretizar o prognóstico de morte em 5 categorias: 

<div align="center">
	
	
Categoria | Condição 
-|-
1 | Paciente Morre <img src="https://render.githubusercontent.com/render/math?math=\leq 7"> dias (1ª semana)
2 | <img src="https://render.githubusercontent.com/render/math?math=7<"> Morre <img src="https://render.githubusercontent.com/render/math?math=\leq 14"> dias (2ª semana)
3 | <img src="https://render.githubusercontent.com/render/math?math=14<"> Morre <img src="https://render.githubusercontent.com/render/math?math=\leq 21"> dias (3ª semana)
4 | <img src="https://render.githubusercontent.com/render/math?math=21<"> Morre <img src="https://render.githubusercontent.com/render/math?math=\leq 28"> dias (4ª semana)
Lived | Paciente Sobrevive

</div>

A escolha destas 5 categorias foi feita devido à natureza da doença que tem uma duração média de 2 semanas, sendo também assumido que a partir de 1 mês do início da doença o paciente já estava fora do risco de morte.
A categorização do paciente foi feita a partir da diferença de datas entre a morte do paciente e a data de diagnóstico da doença. Caso a data de morte fosse faltante no arquivo assumiu-se que o paciente sobreviveu e o mesmo foi categorizado em "Lived". O arquivo .csv final tem a seguinte forma:

<div align="center">

PATIENT|BIRTHDATE|DEATHDATE|RACE|ETHNICITY|GENDER|START|IMMUNIZED|AGE|PROG
-|-|-|-|-|-|-|-|-|-
c87c02ef-6b7a-224c-4513-1b85e19573b9|1998-11-21|NaN|white|	nonhispanic|	F|	11/25/2020|	1|	22	|Lived
4868d84d-7a09-477a-da7c-3fbb8edf3e19|1989-10-28	|NaN|white|	nonhispanic|	M|	1/1/2021|	1|	31	|Lived
f2e5bd39-dc31-0471-1028-adee47891760|1976-05-17|NaN|white|	nonhispanic|	F|	11/26/2020|	1|	44	|Lived
77e53fde-d641-fa26-5792-7a92af4fa260|2018-10-19	|NaN|white|	nonhispanic|	M|	2/17/2021|	0|	2	|Lived
850c346b-9bed-1a8b-c452-165705841a8b|1995-01-10	|NaN|white|	nonhispanic|	M|	11/26/2020|	0|	25	|Lived
...|...|...|...|...|...|...|...|...|...

</div>


O código responsável pela extração dos dados foi elaborado em Python em um Jupyter Notebook que pode ser visualizado no arquivo "filtering_code.ipynb" no link https://github.com/Phreyzer/mo826-1s2022/tree/main/notebooks.

Uma vez obtidos os arquivos .csv com os dados filtrados, utilizamos o software Orange para realizar os seguintes modelos de classificações:

- Random Forest
- KNN
- Support Vector Machine
- Logistic Regression

Cada modelo foi treinado/validado em um dos cenários e testados em outros conforme indicado:

<div align="center">

Treinamento/Validação | Teste
-|-
Cenário 1| Cenário 2
Cenário 2| Cenário 1
Cenário 4| Cenários 1, 2 e 3 |
</div>

Os modelos foram treinados com base nos dados na caracteríticas (_features_) "RACE", "ETHNICITY", "GENDER", "IMMUNIZED e "AGE" e como rótulo foi utilizada a coluna "PROG". Todas as características são categóricas, com exceção da idade ("AGE") que é numérica.

Para cada secção de treinamento/validação foi medida a performance dos modelos através da curva ROC, matriz de confusão e as predições feitas por cada um.

## Bases Adotadas e Geradas para o Projeto

Um resumo da quantidade de pacientes nos cenários antes e depois da filtragem dos dados segue abaixo:

<div align="center">
	
| Cenário(nome da base) | Quantidade de Pacientes Iniciais | Quantidade de pacientes após filtragem
|-|-|-
scenario01 | 1175  |  87 
scenario02 | 1122  | 102 
scenario03 | 11.574 | 986 
scenario04 | 117.532 | 10.168 |

</div>

Os dados originais referentes aos arquivos "patients.csv", "condition.csv" e "immunizations.csv" dos cenários 1 e 2 podem ser acessados no link https://github.com/Phreyzer/mo826-1s2022/tree/main/data/raw. Os dados dos cenários 3 e 4, por serem grandes demais no armezanamento do GitHub não foram disponibilizados.

Os dados filtrados e os workflows do Orange para treinamento, validação e teste dos modelos estão disponíveis no link: https://github.com/Phreyzer/mo826-1s2022/tree/main/src

# Resultados e Discussão
### Treinamento/validação no cenário 1 e Teste no cenário 2 

**Tabela 1** - Resultado das avaliações treinados no cenário 1.
<div align="center">


Modelo|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
KNN|0.792|0.920|0.958|0.930|0.988|0.000
SVM|0.565|0.931|0.964|0.931|1.000|0.000	
Random Forest|0.717|0.931|0.964|0.931|1.000|0.000
Logistic Regression|0.913|0.954|0.976|0.964|0.988|0.500

</div>	

A __Tabela 1__ é o resultado do treinamento dos respectivos modelos no cenário 1. Foi feita uma divisão em apenas 2 _folds_ devido aos poucos dados, e em seguida a validação cruzada entre os mesmos. Os resultados indicam os desempenhos para o _Target class_ como a categoria "Lived", ou sobreviventes.

- _AUC_: (_Area Under Curve_) É a área sob a curva ROC para cada modelo.
- _CA_: (Classification Accuracy) É a proporção de exemplos classificados corretamente.
- _F1_: É a média harmônica entre _Precision_ e _Recall_.
- _Precision_: Proporção entre os pacientes classificados corretamente como "Lived" e o número total de classificados como "Lived".
- _Recall_: Proporção entre o número de pacientes corretamente classificados como "Lived" e os pacientes que realmente sobreviveram.
- _Specificity_: Proporção entre pessoas que foram corretamente classificadas como não-sobreviventes (categorias 1,2,3 e 4) e o total de pessoas que realmente não sobreviveram.

Inicialmente, observando somente as métricas _CA_, _F1_, _Precision_ e _Recall_, poderiamos concluir prematuramente que os modelos obteveram um bom resultado devido aos altos valores obtidos. Entretanto, precisamos levar em consideração a qualidade dos dados de treinamento. O cenário 1 consiste em 87 pacientes dos quais 81 sobreviveram, portanto mesmo um modelo que classificasse todos pacientes que recebesse como sobreviventes possuiria as métricas _Recall_= 1, _Precision_= 81/87 = 0.931, _F1_= 0.964 e _CA_ = 81/87, já que é a proporção de exemplos classificados corretamente. Portanto, as medidas descritas não seriam uma boa medida de desempenho para o problema em questão. Olhamos agora para a informação contida na medida _Specificity_.
Como o cenário 1 está desbalanceado e possui uma grande proporção de pacientes de um único grupo, podemos supor que os modelos estarão enviesados a classificar os pacientes como "Lived". Apesar das demais medidas de comparação estarem comprometidas devido a este desbalanceamento, a _Specificity_ é uma medida leva em consideração os pacientes que foram classificados corretamente como não-sobreviventes, punindo modelos que erroneamente classificam os pacientes como sobreviventes. Portanto, olhamos para a especificidade para comparar os modelos e obtemos a seguinte ordem decrescente de desempenho: _Logistic Regression_ > _KNN_ = _Random Forest_ = _SVM_, onde o _KNN_,_SVM_ e _Random Forest_ não classificaram nenhum não-sobrevivente corretamente.

A segunda métrica que podemos utilizar é a _AUC_, ou a área sob a curva ROC, uma vez que seu valor também depende da _Specificity_ e favorece modelos menos enviesados pela má qualidade dos dados. Neste caso, a partir da __Tabela 1__ obtemos a seguinte ordem decrescente de desempenho: _Logistic Regression_ > _KNN_ > _Random Forest_ > _SVM_. Podemos visualizar este resultado também através das curvas ROC conforme a __Figura 1__, onde vemos que a curva da _Logistic Regression_ (rosa) domina as demais, seguida pela _KNN_ (verde), _Random Forest_ (laranja) e finalmente _SVM_ (azul) com o pior desempenho.
	
**Figura 1** - Curvas ROC (Rosa->_Logistic Regression_; Verde->_KNN_; Laranja->_Random Forest_; Azul->_SVM_).
<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/assets/Imagem1.png?raw=true" >
</div>

Através da **Tabela 2** também podemos interpretar os valores _AUC_ de forma probabilística. Em cada entrada da tabela temos a probabilidade de que o _score_ do modelo na linha seja superior ao modelo na coluna, por exemplo, P(KNN > SVM) = 0.570. Assumindo as probabilidades como representativas do cenário real temos que a ordem decrescente de desempenho mais provável é:
Regressão Logística -> KNN -> Random Forest -> SVM. 

**Tabela 2** - Comparação dos modelos por AUC.
<div align="center">
	
 -|Logistic Regression|Random Forest|SVM|KNN
-|-|-|-|-
Logistic Regression||0.661|0.632|0.696
Random Forest|0.339||0.542|0.383
SVM|0.368|0.458||0.430
KNN|0.304|0.617|0.570||
	
</div>

Agora validamos os classificadores treinados a partir do cenário 1 com os pacientes do cenário 2. Os _scores_ obtidos estão dispostos na **Tabela 3**.

**Tabela 3** - Resultado do Teste (treinamento/validação=cenário 1; Teste=cenário 2).

<div align="center">

Modelo|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
KNN|0.989|0.980|0.971|0.961|0.980|0.020
SVM|0.998|0.980|0.971|0.961|0.980|0.020
Random Forest|1.000|0.980|0.971|0.961|0.980|0.020
Logistic Regression|0.993|0.990|0.985|0.980|0.990|0.510

</div>

A princípio, utilizando AUC como métrica principal de comparação vemos que o modelo _Random Forest_ aparenta obter o melhor desempenho, seguido do _SVM_, _Logistic Regression_ e _KNN_. Entretanto, uma breve análise da predição dos modelos para cada paciente mostra que o _KNN_, _SVM_ e _Random Forest_ classificaram todos os 102 pacientes do cenário 2 como sobreviventes, enquanto apenas a _Regressão Logística_ acertou o prognóstico de um paciente que veio a falecer. Este fato pode ser observado pela coluna "_Specificity_" da **Tabela 3**, onde o modelo de regressão logística apresenta valor significativamente maior que os demais.

### Treinamento/Validação no cenário 2 e Teste no cenário 1 

**Tabela 4** - Resultado das avaliações dos modelos treinados no cenário 2.

<div align="center">

Modelo|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
KNN|0.490|0.980|0.990|0.980|1.000|0.000
SVM|0.300|0.980|0.990|0.980|1.000|0.000
Random Forest|0.660|0.971|0.985|0.980|0.990|0.000
Logistic Regression|0.940|0.971|0.985|0.980|0.990|0.000

</div>

Assim como no primeiro caso, antes de utilizarmos todas as métricas como medida de desempenho, observamos que para um modelo completamente enviesado que classificaria todos os pacientes como sobreviventes, teriamos _Recall_= 100/100 = 1, _Precision_= 100/102 = 0.980, _F1_= 0.990 e _CA_ = 100/102 = 0.980, indicando que os altos valores observados não são boas medidas comparativas. Logo, mais uma vez recorremos à _Specificity_. Contudo, nenhum dos modelos acertou a predição de um paciente que não sobreviveu, comprometendo também a métrica _Specificity_. Portanto, nenhuma métrica em questão reflete o desempenho dos modelos, fato causado em sua maioria pela desproporção de dados para um grupo (sobreviventes) em relação aos demais (1,2,3 e 4), já que apenas 2 dos 102 pacientes morreram.

As curvas ROC para cada modelo são apresentadas na __Figura 2__.

**Figura 2** - Curvas ROC (Rosa->Regressão logística; Verde->KNN; Laranja->Random Forest; Azul->SVM).
<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/assets/Imagem2.png?raw=true">
	</div>

Interpretando os valores das respectivas áreas sob as curvas ROC como uma medida de desempenho, é feita a análise probabilística do melhor modelo conforme a __Tabela 5__.  

**Tabela 5** - Comparação dos modelos por AUC.
<div align="center">

-|KNN|SVM|Random Forest|Logistic Regression
-|-|-|-|-
KNN||0.911|0.341|0.059
SVM|0.089||0.209|0.017
Random Forest|0.659|0.791||0.227
Logistic Regression|0.941|0.983|0.773||
	
</div>

Observando a comparação dos modelos por AUC através da **Tabela 5** é tentador concluir que a _Logistic Regression_ é novamente superior aos demais modelos para este problema. Contudo, uma avaliação das predições de cada modelo vemos que o modelo _KNN_ e _SVM_ classificaram todos os pacientes no grupo de sobreviventes, enquanto que a _Logistic Regression_ e _Random Forest_ erroneamente classificaram um sobrevivente no grupo de morte na 3ª semana, e os demais como sobreviventes. Está claro que a pouca quantidade de dados e o desbalanceamento dos mesmos (apenas 2 pacientes morreram), enviesaram os modelos a classificarem qualquer exemplo como "_Lived_", os tornando precisos mas pouco específicos neste cenário.

Olhamos para o resultado do teste nos dados do cenário 1 apresentado na __Tabela 6__.

**Tabela 6** - Resultado do Teste (treinamento/validação=cenário 2; Teste=cenário 1).

<div align="center">

Modelo|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
KNN|0.600|0.931|0.898|0.867|0.931|0.069
SVM|0.937|0.931|0.898|0.867|0.931|0.069
Random Forest|0.710|0.931|0.898|0.867|0.931|0.069
Logistic Regression|0.960|0.920|0.903|0.887|0.920|0.378

</div>

É esperado que o modelo treinado com os dados do cenário 2 possua a pior performance, uma vez que possuí o maior desbalanço entre a frequência de categorias (98% é da categoria "Lived") e o modelo não aprenderá a classificar os casos adequadamente. A __Tabela 6__ sumariza esse raciocínio ao evidenciar um desempenho inferior ao apresentado pela __Tabela 3__, quando o cenário de treino é 1 e o de teste o 2.

### Treinamento no cenário 4 e validação nos cenários 1, 2 e 3

Finalmente, realizamos os mesmos procedimentos para o cenário 4, o qual possui cerca de 10000 pacientes. As métricas de desempenho, a curva ROC e a comparação probabilística pela _AUC_ se encontram na __Tabela 6__, __Figura 3__ e __Tabela 7__ respectivamente. 

**Tabela 7** - Resultado das avaliações dos modelos treinados no cenário 4.
<div align="center">
	
Modelo|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
KNN|0.810|0.971|0.985|0.979|0.991|0.328
SVM|0.662|0.970|0.985|0.971|0.999|0.036
Random Forest|0.874|0.972|0.986|0.980|0.992|0.338
Logistic Regression|0.964|0.972|0.986|0.975|0.997|0.167

</div>

**Figura 3** - Curvas ROC (Rosa->Regressão logística; Verde->KNN; Laranja->Random Forest; Azul->SVM).
<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/assets/Imagem3.png?raw=true">
</div>

**Tabela 8** - Comparação dos modelos por AUC.

<div align="center">

-|KNN|SVM|Random Forest|Logistic Regression
-|-|-|-|-
KNN||0.942|0.033|0.010
SVM|0.058||0.016|0.005
Random Forest|0.967|0.984||0.005
Logistic Regression|0.990|0.995|0.995||

</div>
	
Utilizando novamente o AUC como métrica de comparação, vemos que a ordem decrescente de desempenho é: _Logistic Regression_ > _Random Forest_ > KNN > SVM, compartilhando a tendência dos demais onde a _Logistic Regression_ obteve o melhor desempenho e _SVM_ o pior.

Por apresentar o melhor desempenho consistentemente, fazemos o teste do modelo _Logistic Regression_, obtido no cenário 4, com os dados dos três primeiros. Os resultados estão sumarizados na __Tabela 9__.

**Tabela 9** - Resultado do Teste (treinamento/validação=cenário 4; Teste=cenário 1, 2 e 3).

<div align="center">

Scenario|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
1|0.934|0.954|0.943|0.932|0.954|0.534
2|0.993|0.980|0.976|0.971|0.980|0.510
3|0.954|0.968|0.957|0.949|0.968|0.184

</div>

Como podemos observar, o modelo obteve um _score_ alto para todas as métricas exceto a _Specificity_. Apesar de possuir valor quase 3 vezes maior quando testado contra os cenários 1 e 2, é possível que devido ao baixo número de não-sobreviventes, 6 e 2 respectivamente, a especificidade tenha obtido esses valores em parte por sorte. Já o cenário 3 evidencia que para uma base de dados maior, o modelo é realmente pouco específico.

# Conclusão
O presente projeto permitiu aos seus integrantes trabalhar com dados sintéticos, aplicando técnicas tratamento de dados e também modelos de aprendizado de máquina.
O treinamento dos diversos modelos evidenciou a importância que a quantidade e a qualidade dos dados têm no seu treinamento. Entretanto, é a qualidade dos dados que exerceu um papel significativo no modelo final. Por ser uma doença com uma baixa mortalidade, os modelos treinados se mostraram enviesados ao prognóstico de recuperação, apresentando uma alta acurácia e precisão mas sacrificando especificidade, o que é conhecido como paradoxo da acurácia. Uma análise comparativa dos métodos em cada cenário mostra que a _Logistic Regression_ apresentou a melhor performance em todos os casos, potencialmente indicando que seu uso é preferível em casos onde há um alto desbalanço na frequência de alguns grupos. Já o modelo _SVM_ apresentou a pior performance em todos cenários, o que evidenciou sua dependência com a qualidade dos dados. Os modelos _Random Forest_ e _KNN_ possuíram performances semelhantes e se alternaram como segundo melhor classificador.
Conforme os cenários um e dois se mostraram enviesados, acreditou-se que o treinamento de modelos no cenário quatro pudesse amortecer esse fenômeno, o que se mostrou não ser o caso. Apesar dos 10168 paciences contidos no cenário quatro após a filtragem, 97% deles sobreviveram, o que contribuiu para o viés dos classificadores.
Uma possível solução para esse problema seria a adição de parâmetros mais impactantes para o prognóstico, como a presença ou não de problemas respiratórios ou obesidade, o que tornaria a distinção entre casos mais evidente. Outra solução poderia ser a de utilizar ténicas como _Undersampling_, _Oversampling_ ou _SMOTE_[2] para lidar com o desbalanceamento dos dados do projeto.

A partir do cenário quatro, também podemos corroborar nossa escolha em separar os pacientes em 5 classes, sendo eles os que morreram na primeira semana, segunda, terceira, quarta e os que sobreviveram. Entre os 305 pacientes que vieram a óbito, 58 morreram em menos de 7 dias, 161 na segunda semana, 83 na terceira e apenas 3 na quarta, indicando assim, que a criação de categorias para períodos de tempo maiores seriam infrutíferas. 


# Referências Bibliográficas
\[1\] [This is Environmental Racism - Darryl Fears and Brady Dennis](https://www.washingtonpost.com/climate-environment/interactive/2021/environmental-justice-race/?itid=lk_interstitial_manual_21)

\[2\] [Dados Desbalanceados — O que são e como lidar com eles](https://medium.com/turing-talks/dados-desbalanceados-o-que-s%C3%A3o-e-como-evit%C3%A1-los-43df4f49732b)
