# Projeto 3 – Reproduzindo o Experimento de um Artigo Científico
# Apresentação
O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [_Ciência e Visualização de Dados em Saúde_](https://ds4h.org/), oferecida no primeiro semestre de 2022, na Unicamp.

## Integrantes


<div align="center">
  
Nome | RA | Especialização
---|---|---
Caroline Gerbaudo Nakazato | 168913 | Ciência da Computação
Luan de Oliveira Silveira | 204099 | Ciência da Computação
Wilson Bagni Junior | 010097 | Ciência da Computação
  
</div>

# Referência bibliográfica do artigo lido
Goh, K.-I., Cusick, M. E., Valle, D., Childs, B., Vidal, M., & Barabási, A.-L. (2007). The human disease network. In Proceedings of the National Academy of Sciences (Vol. 104, Issue 21, pp. 8685–8690). Proceedings of the National Academy of Sciences. [Link](https://doi.org/10.1073/pnas.0701361104)

# Resumo
> Escreva um breve do artigo (com as suas palavras, não deve ser copiado texto do artigo).

# Breve descrição do experimento/análise do artigo que foi replicado
A OMIM (_Online Mendelian Inheritance in Men_) é uma base de dados contendo genes e fenótipos humanos de forma gratuita e atualizada constantemente. Em sua divulgação inicial, Barabási et al. contavam com 517 doenças e suas categorias, e 903 genes. Atualmente, a OMIM conta com mais de 26,453 entradas.

Em seu artigo, dentre outras coisas, Barabási et al. se propõem a criar um grafo no qual representa doenças como nódulos e as conectam através de arestas caso compartilhem um ou mais genes, sendo o tamanho do nó proporcional à quantidade de genes associados àquela condição e a espessura da linha proporcional à quantidade de genes compartilhados entre os nós. A ideia é expandida para a realização de um grafo gene-gene no qual cada nó é indicativo de um gene na base de dados, e cada aresta representa o compartilhamento de uma ou mais doenças associadas àquele gene. Neste caso o tamanho do nó é proporcional à quantidade de doenças associadas ao gene mas a largura da aresta não transmite significado.

## Dados usados como entrada
Dataset | Endereço na Web | Resumo descritivo
----- | ----- | -----
Diseasome | https://github.com/gephi/gephi/wiki/Datasets | Base de dados contendo pares de doenças associadas por genes em comum, assim como os genes associados a cada uma delas. Retirada originalmente da OMIM, dezembro de 2005.

# Método
> Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos. Etapas do processo reproduzido.


# Resultados

## Doença-doença
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image1.png?raw=true">
</div>

**Figura 1** - Grafo das doenças reproduzido através do Cytoscape.
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image2.png?raw=true">
</div>

**Figura 2** - Grafo das doenças do artigo.

A partir da **Figura 1** vemos o formato do grafo obtido para a rede doença-doença. As cores para cada categoria de doença foram escolhidas a fim de coincidir com o grafo original (**Figura 2**) e auxiliar na comparação.

Apesar do padrão de cores ser similar em algumas regiões dos grafos, os nós gerados pelo Cytoscape encontram-se levemente transladados e rotacionados ao grafo original. De fato, cada vez que os dados são carregados no software os nós gerados possuem posições absolutas diferentes, mas mantendo a posição relativa entre os mesmos similar. Para uma análise mais rigorosa é feita a comparação entre alguns grupos de doenças.

### Câncer
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image3.png?raw=true">
</div>

**Figura 3** - Grupo Câncer reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image4.png?raw=true">
</div>

**Figura 4** - Grupo Câncer no artigo original.

### Hematológico
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image5.png?raw=true">
</div>

**Figura 5** - Grupo Hematológico reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image6.png?raw=true">
</div>

**Figura 6** - Grupo Hematológico no artigo original.

### Oftomológico
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image7.png?raw=true">
</div>

**Figura 7** - Grupo Oftomológico reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image8.png?raw=true">
</div>

**Figura 8** - Grupo Oftomológico no artigo original.

### Ouvido, Nariz, Garganta
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image9.png?raw=true">
</div>

**Figura 9** - Grupo Ouvido, Nariz, Garganta reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image10.png?raw=true">
</div>

**Figura 10** - Grupo Ouvido, Nariz, Garganta no artigo original.

### Muscular
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image11.png?raw=true">
</div>

**Figura 11** - Grupo Muscular reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image12.png?raw=true">
</div>

**Figura 12** - Grupo Muscular no artigo original.

### Neurológico
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image13.png?raw=true">
</div>

**Figura 13** - Grupo Neurológico reproduzido através do Cytoscape (parte 1).

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image14.png?raw=true">
</div>

**Figura 14** - Grupo Neurológico reproduzido através do Cytoscape (parte 2).

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image15.png?raw=true">
</div>

**Figura 15** - Grupo Neurológico no artigo original.

## Gene-gene
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image16.png?raw=true">
</div>

**Figura 16** - Grafo dos genes reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image17.png?raw=true">
</div>

**Figura 17** - Grafo dos genes do artigo original.

Assim como para o grafo doença-doença, realizamos a comparação entre alguns grupos.

### Câncer
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image18.png?raw=true">
</div>

**Figura 18** - Grupo Câncer reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image19.png?raw=true">
</div>

**Figura 19** - Grupo Câncer no artigo original.

### Hematológico
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image20.png?raw=true">
</div>

**Figura 20** - Grupo Hematológico reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image21.png?raw=true">
</div>

**Figura 21** - Grupo Hematológico no artigo original.

### Oftomológico
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image22.png?raw=true">
</div>

**Figura 22** - Grupo Oftomológico reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image23.png?raw=true">
</div>

**Figura 23** - Grupo Oftomológico no artigo original.

### Ouvido, Nariz, Garganta
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image24.png?raw=true">
</div>

**Figura 24** - Grupo Ouvido, Nariz, Garganta reproduzido através do Cytoscape.

<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image25.png?raw=true">
</div>

**Figura 25** - Grupo Ouvido, Nariz, Garganta no artigo original.

### Muscular e Cardiovascular
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image26.png?raw=true">
</div>

**Figura 26** - Grupo Muscular e Cardiovascular reproduzido através do Cytoscape.
<div align="center">
  
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p3/assets/Image27.png?raw=true">
</div>

**Figura 27** - Grupo Muscular e Cardiovascular no artigo original.
