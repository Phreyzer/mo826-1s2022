# Projeto 3 – Reproduzindo o Experimento de um Artigo Científico
# Apresentação
O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [_Ciência e Visualização de Dados em Saúde_](https://ds4h.org/), oferecida no primeiro semestre de 2022, na Unicamp.

## Integrantes

Nome | RA | Especialização
---|---|---
Caroline Gerbaudo Nakazato | 168913 | Ciência da Computação
Luan de Oliveira Silveira | 204099 | Ciência da Computação
Wilson Bagni Junior | 010097 | Ciência da Computação

# Referência bibliográfica do artigo lido
Goh, K.-I., Cusick, M. E., Valle, D., Childs, B., Vidal, M., & Barabási, A.-L. (2007). The human disease network. In Proceedings of the National Academy of Sciences (Vol. 104, Issue 21, pp. 8685–8690). Proceedings of the National Academy of Sciences. [Link](https://doi.org/10.1073/pnas.0701361104)

# Resumo
> Escreva um breve do artigo (com as suas palavras, não deve ser copiado texto do artigo).

# Breve descrição do experimento/análise do artigo que foi replicado
A OMIM (_Online Mendelian Inheritance in Men_) é uma base de dados contendo genes e fenótipos humanos de forma gratuita e atualizada constantemente. Em sua divulgação inicial, Barabási et al. contavam com 517 doenças e suas categorias, e 903 genes. Atualmente, a OMIM conta com mais de 26,453 entradas.

Em seu artigo, dentre outras coisas, Barabási et al. se propõem a criar um grafo no qual representa doenças como nódulos e as conectam através de arestas caso compartilhem um ou mais genes, sendo o tamanho do nó proporcional à quantidade de genes associados àquela condição e a espessura da linha proporcional à quantidade de genes compartilhados entre os nós. A ideia é expandida para a realização de um grafo gene-gene no qual cada nó é indicativo de um gene na base de dados, e cada aresta representa o compartilhamento de uma ou mais doenças associadas àquele gene. Neste caso o tamanho do nó é proporcional à quantidade de doenças associadas ao gene mas a largura da aresta não transmite significado.
