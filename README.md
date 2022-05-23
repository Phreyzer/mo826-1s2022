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
KNN|0.888|0.978|0.988|0.988|0.988|0.800
SVM|0.494|0.944|0.971|0.944|1.000|0.000	
Random Forest|0.932|0.967|0.983|0.966|1.000|0.400
Logistic Regression|0.971|0.978|0.988|0.988|0.988|0.800

A tabela 1 é o resultado do treinamento dos respectivos modelos no cenário 1. Foi feita uma divisão de 80% dos dados para treinamento e 20% para validação, e os resultados indicam os desempenhos para o _Target class_ como a categoria "Lived", ou sobreviventes.
Inicialmente, levando em conta a métrica _AUC_ (_Area Under Curve), ou seja, a área sob a curva ROC, como principal método de comparação vemos que a ordem decrescente de desempenho é _Logistic Regression_ > _Random Forest_ > KNN > SVM. Podemos visualizar este resultado também através das curvas ROC conforme a figura 1.
	
</div>	
	
**Figura 1** - Curvas ROC (Rosa->Regressão logística; Verde->KNN; Laranja->Random Forest; Azul->SVM).
<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/assets/Imagem1.png?raw=true" >
</div>

A partir da **Tabela 1** podemos ver que a regressão logística obteve uma performance superior a todos os outros métodos por todos os parâmetros comparativos. Já os demais métodos tiveram _scores_ aproximadamente iguais para as métricas CA, F1, _Precision_ e _Recall_, diferindo significativamente apenas para AUC.

**Tabela 2** - Comparação dos modelos por AUC.
<div align="center">
	
	
 -|KNN|SVM|Random Forest|Logistic Regression
-|-|-|-|-
KNN||0.579|0.618|0.340
SVM|0.421||0.427|0.358
Random Forest|0.382|0.573||0.345
Logistic Regression|0.660|0.642|0.655||
	
	
</div>

Através da **Tabela 2** temos a probabilidade de que o _score_ do modelo na linha seja superior ao modelo na coluna, por exemplo, P(KNN > SVM) = 0.579. Assumindo as probabilidades como representativas do cenário real temos que a ordem decrescente de desempenho mais provável é:
Regressão Logística -> KNN -> Random Forest -> SVM.

Agora validamos os classificadores treinados a partir do cenário 1 com os pacientes do cenário 2. Os _scores_ obtidos estão dispostos na **Tabela 3**.

**Tabela 3** - Resultado do Teste (treinamento/validação=cenário 1; Teste=cenário 2).

<div align="center">

Modelo|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
KNN|0.989|0.980|0.971|0.961|0.980
SVM|0.998|0.980|0.971|0.961|0.980
Random Forest|1|0.980|0.971|0.961|0.980
Logistic Regression|0.993|0.990|0.985|0.980|0.990

</div>

A princípio, utilizando AUC como métrica principal de comparação vemos que o modelo _Random Forest_ aparenta obter o melhor desempenho, seguido do _SVM_, _Regressão Logística_, _KNN_. Entretanto, devido a baixa quantidade de pacientes no cenário de treinamento, os modelos estão enviesados a classificar o paciente como "Lived". Uma breve análise da predição dos modelos para cada paciente mostra que o _KNN_, _SVM_ e _Random Forest_ classificaram todos os 102 pacientes do cenário 2 como sobreviventes, enquanto apenas a _Regressão Logística_ acertou o prognóstico de um paciente que veio a falecer. Este fato pode ser observado pela coluna "_Recall_" da **Tabela 3**, mostrando que o modelo de regressão logística acertou <img src="https://render.githubusercontent.com/render/math?math=101/102\approx0.990"> enquanto os demais acertaram <img src="https://render.githubusercontent.com/render/math?math=100/102\approx0.980">, ou seja, errando apenas os 2 dos 102 pacientes que vieram a óbito.

### Treinamento/Validação no cenário 2 e Teste no cenário 1 

**Tabela 4** - Resultado das avaliações dos modelos treinados no cenário 2.

<div align="center">

Modelo|AUC|CA|F1|Precision|Recall
---|---|---|---|---|---
KNN|0.485|0.980|0.971|0.961|0.980
SVM|0.328|0.980|0.971|0.961|0.980
Random Forest|0.568|0.980|0.971|0.961|0.980
Logistic Regression|0.480|0.971|0.966|0.961|0.971

</div>
	
**Figura 2** - Curvas ROC (Rosa->Regressão logística; Verde->KNN; Laranja->Random Forest; Azul->SVM).
<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/assets/Imagem2.png?raw=true">
	</div>

**Tabela 5** - Comparação dos modelos por AUC.
<div align="center">

-|KNN|SVM|Random Forest|Logistic Regression
-|-|-|-|-
KNN||0.911|0.344|0.059
SVM|0.089||0.243|0.017
Random Forest|0.656|0.757||0.333
Logistic Regression|0.941|0.983|0.667||
	
</div>

Observando a comparação dos modelos por AUC através da **Tabela 5** é tentador concluir que a _Logistic Regression_ é novamente superior aos demais modelos para este problema. Contudo, uma avaliação das predições de cada modelo vemos que todos os modelos classificaram os pacientes no grupo de sobreviventes, com exceção da _Logistic Regression_ que classificou um sobrevivente com o prognóstico de morte em 3 semanas. Está claro que a pouca quantidade de dados e o desbalanceamento dos mesmos (apenas 2 pacientes morreram), enviesaram os modelos a classificarem qualquer exemplo como "_Lived_", os tornando precisos mas pouco específicos neste cenário.

### Treinamento no cenário 4 e validação nos cenários 1,2 e 3
**Tabela 6** - Resultado das avaliações dos modelos treinados no cenário 4.
<div align="center">
	
Modelo|AUC|CA|F1|Precision|Recall|Specificity
---|---|---|---|---|---|---
KNN|0.706|0.963|0.959|0.954|0.963|0.316
SVM|0.651|0.970|0.955|0.941|0.970|0.030
Random Forest|0.820|0.966|0.960|0.954|0.966|0.345
Logistic Regression|0.956|0.970|0.961|0.952|0.970|0.192

</div>

**Figura 3** - Curvas ROC (Rosa->Regressão logística; Verde->KNN; Laranja->Random Forest; Azul->SVM).
<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/assets/Imagem3.png?raw=true">
</div>

**Tabela 7** - Comparação dos modelos por AUC.

<div align="center">

-|KNN|SVM|Random Forest|Logistic Regression
-|-|-|-|-
KNN||0.836|0.046|0.022
SVM|0.164||0.080|0.044
Random Forest|0.954|0.920||0.002
Logistic Regression|0.978|0.956|0.998||

</div>
	
Utilizando novamente o AUC como métrica de comparação, vemos que a ordem decrescente de desempenho é: _Logistic Regression_ -> _Random Forest_ -> KNN -> SVM

Apesar dos _Scores_ _AUC_, _CA_, _F1_, _Precision_ e _Recall_ terem valores altos, uma melhor medida do desempenho dos modelos é a especificidade. Isto porque como a grande maioria dos pacientes sobreviveram à COVID-19, os modelos sempre tenderão a classificá-los como sobreviventes, sendo os casos em que o prognóstico é de morte uma melhor medida do seu desempenho. Por exemplo, suponhamos que um quinto modelo classifique todos os casos como sobreviventes. Neste caso, como 9863 dos 10168 pacientes sobreviveram, ele teria uma taxa de acerto de <img src="https://render.githubusercontent.com/render/math?math=9863/10168\approx0.9778">, provando-se superior aos modelos treinados.  

# Conclusão
O presente projeto permitiu aos seus integrantes trabalhar com dados sintéticos, aplicando técnicas tratamento de dados e também modelos de aprendizado de máquina.
O treinamento dos diversos modelos evidenciou a importância que a quantidade e a qualidade dos dados têm no seu treinamento. Entretanto, é a qualidade dos dados que exerceu um papel significativo no modelo final. Por ser uma doença com uma baixa mortalidade, os modelos treinados se mostraram enviesados ao prognóstico de recuperação, apresentando uma alta acurácia mas sacrificando sensibilidade, o que é conhecido como paradoxo da acurácia. 
Uma possível solução para esse problema seria a adição de parâmetros mais impactantes para o prognóstico, como a presença ou não de problemas respiratórios ou obesidade, o que tornaria a distinção entre casos mais evidente. Outra solução poderia ser a de utilizar ténicas como _Undersampling_, _Oversampling_ ou _SMOTE_[2] para lidar com o desbalanceamento dos dados do projeto.


# Referências Bibliográficas
\[1\] [This is Environmental Racism - Darryl Fears and Brady Dennis](https://www.washingtonpost.com/climate-environment/interactive/2021/environmental-justice-race/?itid=lk_interstitial_manual_21)

\[2\] [Dados Desbalanceados — O que são e como lidar com eles](https://medium.com/turing-talks/dados-desbalanceados-o-que-s%C3%A3o-e-como-evit%C3%A1-los-43df4f49732b)
