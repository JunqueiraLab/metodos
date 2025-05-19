---
layout: default
title: "Protocolo - Metagenômica e Microbiomas"
permalink: /protocolos/metagenomica/
---
[⬅ Voltar para a página principal](/metodos/)

# 🧬 Metagenômica e Microbiomas - Métodos computacionais 

O termo “microbioma” geralmente é utilizado para se referir à composição complexa de genes e genomas de microrganismos associados a ambientes, hospedeiros, tecidos e superfícies. Um dos grandes desafios na análise de microbiomas gerados por sequenciamento WGS é a atribuição de uma quantidade massiva de sequências a classes taxonômicas e funcionais. A principal metodologia utilizada para a classificação das sequências baseia-se na homologia de proteínas (Huson et al. 2007). Nessa abordagem, as sequências de nucleotídeos são traduzidas e alinhadas contra um banco de dados de referência de proteínas com identidade taxonômica e funcional conhecidas. Em seguida, os alinhamentos significativos resultantes são usados para atribuir as sequências a grupos taxonômicos e funcionais baseando-se em pontuações similares aos resultados gerados pelo BLAST. Tradicionalmente, as análises de microbiomas usam os bancos de proteínas como referência (Bağcı et al. 2021), que são mais conservadas do que as sequências nucleotídicas. Isso significa que é mais provável encontrar sequências similares em proteínas do mesmo táxon, mesmo que haja variações nas sequências de DNA. Além disso, uma compreensão biológica das vias metabólicas presentes em um determinado microbioma requer conhecimento detalhado das proteínas com função conhecida, o que permite uma melhor inferência funcional. Desta forma, diversos programas que computam sequências de microbiomas geradas por WGS, tem como abordagem central o alinhamento traduzido das sequências do microbioma contra o banco de dados NR do NCBI (Benson et al. 2005). Algoritmos otimizados foram desenvolvidos para dar maior rapidez aos alinhamentos, sem perder a precisão da atribuição taxonômica. 

O programa **DIAMOND** (Buchfink et al. 2015), por exemplo, é amplamente utilizado em análises metagenômicas. Ele é um alinhador otimizado para a estratégia _seed-extend_, utilizando um alfabeto reduzido para a correspondência entre as queries e o banco de dados (Buchfink et al. 2015). É possível alterar parâmetros de especificidade e pontuação de alinhamentos como no BLAST. A seguir, a atribuição taxonômica e funcional é feita pelo script **MEGANIZER**, implementado no programa **MEGAN6** (Huson et al. 2016). 

Nesta aula, vamos usar conjuntos de dados metagenômicos de sequências curtas geradas em plataforma Illumina. As amostras foram sequenciadas com a metodologia de shotgun e, para as análises vamos utilizar os programas DIAMOND (Buchfink et al. 2015) e MEGAN6 (Huson et al. 2016). Estes programas são adequados para a análise de grandes conjuntos de dados (dezenas de milhões de reads) e permitem realizar uma abordagem comparativa dos microbiomas. A análise consiste nos seguintes passos principais:

- 1.	Alinhamento de todos os reads gerados contra o banco de dados de proteínas NR (NCBI), usando o DIAMOND (passo realizado no servidor).
- 2.	Atribuição taxonômica e funcional dos alinhamentos resultantes usando o programa MEGANIZER do MEGAN6 (passo realizado no servidor).
- 3.	Exploração dos dados interativa usando o MEGAN6 (no seu computador).

Após o alinhamento e a atribuição taxonômica e funcional feita em um servidor, é possível analisar os dados em um computador com menor poder de processamento. Devido à restrição do poder computacional dos desktops no LIG, vamos usar os dados processados em um servidor, mas todos os passos da análise serão simulados em aula. 

## 💎 1. DIAMOND - Alinhamento de sequências metagenômicas geradas por shotgun

Os conjuntos de dados que vamos utilizar foram gerados na plataforma Illumina em uma corrida paired-end. Cada arquivo analisado tem, em média, 45 milhões de reads com 251 bases de comprimento (Tabela 1).

<p style="text-align: center;">
  <em>Tabela 1. Número de reads de cada amostra.</em><br>
  <img src="{{ site.baseurl }}/images/tabela_reads.png" alt="Tabela 1 - Informações dos arquivos analisados" width="600">
</p>

Para realizar o alinhamento das sequências, é necessário dar o comando do DIAMOND juntamente com os parâmetros **obrigatórios** da análise. 
- O **comando** ```diamond``` chama o programa.
- Em seguida, precisamos especificar o **algoritmo** de alinhamento. Como temos sequências de DNA e queremos alinhá-las a um banco de referência de proteínas, usaremos o ```blastx```.
- O alinhamento deve ser feito usando um **banco de dados de referência** com sequências indexadas e classificadas, como o banco de sequências de proteínas **nr**, do NCBI. Este parâmetro obrigatório da análise é determinado com o argumento ```-d nr```.
- O arquivo de entrada (**input**), que contém as sequências, deve ser especificado com o argumento ```-q input.fastq```.
- Para os arquivos de saída (**output**), é importante indicar o a extensão do diamond alignment archive, ou DAA, que especifica que este é o arquivo resultante da análise de alinhamento do programa DIAMOND. Para isso, é preciso adicionar ao comando o parâmetro ```-o output.daa``` e usar o argumento ```-f 100```, que irá formatar o output corretamente como DAA. Este formato é necessário para o passo de conversão do alinhamento posteriormente, com o programa MEGANIZER, permitindo que ele possa ser visualizado graficamente no MEGAN6.
- Outros argumentos e parâmetros do DIAMOND podem ser observados com o comando de ajuda ```diamond help```.

Sabendo destas informações, agora podemos montar a linha de comando que será utilizada nas análises:

```diamond blastx -d nr -q amostra.fastq -o amostra.daa -f 100```

_Lembre-se que você deve usar os paths (endereços) completos dos programas e dos arquivos que não estão na variável_ `$PATH` _ou no mesmo diretório que você dispara a análise._

## 📊 2. MEGANIZER – classificação taxonômica e funcional

O programa DIAMOND gera um arquivo em formato DAA que contém as sequências alinhadas e seus respectivos alinhamentos. Estas informações são úteis para realizar a atribuição taxonômica e funcional (_binning_) das sequências. O programa MEGANIZER atua justamente neste passo, agrupando sequências e escrevendo um bloco adicional de resultados no final do arquivo DAA gerado no output do passo anterior.

Para os reads curtos, o agrupamento taxonômico é feito usando o algoritmo naïve LCA (_lowest common ancestor_), com base na taxonomia do NCBI e do Genome Taxonomy Database (GTDB) (Huson et al. 2007). A atribuição funcional é realizada pela estratégia do melhor alinhamento (_best hit_) (Mitra et al. 2011), usando a classificação de vários bancos curados de proteínas como referência (EC, eggNOG, InterPro2GO, SEED).

Para realizar a classificação taxonômica e funcional, é necessário usar o programa MEGANIZER com um arquivo de [mapeamento - MEGAN MAP](https://software-ab.cs.uni-tuebingen.de/download/megan6/welcome.html){:target="_blank"}. O arquivo DAA gerado pelo DIAMOND pode ser “meganizado” com a seguinte linha de comando:

```daa-meganizer -i nome_do_arquivo.daa -mdb path/para/megan-map-Feb2022.db -t 30```

```-i``` indica o arquivo de entrada/input (ou seja, o arquivo .daa gerado no passo anterior).

```-mdb``` indica o endereço do arquivo de mapeamento para classificação taxonômica e funcional, que inclui as informações dos bancos de dados NCBI, GTDB, EC, eggNOG, InterPro2GO, SEED.

```-t 30``` é o comando que paraleliza o processamento do arquivo em 30 CPUs (neste exemplo). O número pode variar de acordo com a disponibilidade de CPUs e o default é 8.

Ao fim do processo, o arquivo DAA terá três blocos de informação de dados: 

1. informação de identificação das proteínas de referência encontradas (headers) no seu conjunto de sequências;
2. todas as sequências alinhadas e todos os alinhamentos realizados contra as proteínas de referência;
3. um bloco de informações adicionadas pelo MEGANIZER com as classificações taxonômicas e funcionais, juntamente com um índice de todos os reads alinhados.

O novo arquivo DAA “meganizado” pode ser aberto diretamente no programa MEGAN6.

## 🦣 3. MEGAN6 

O MEGAN6 é um programa que permite a visualização e exploração interativa de conjuntos de dados de metagenomas em computadores comuns. Ao ser iniciado, o programa abre duas janelas. A janela principal mostra uma árvore com a taxonomia definida pelo banco Taxonomy do NCBI. A janela secundária exibe um log de todos os comandos executados durante as análises e eventuais erros.

Para abrir um arquivo DAA já “meganizado” ou um arquivo já exportado no formato ```.megan```, clique em ```File▸Open``` e então escolha o arquivo a ser analisado. Você deve escolher um dos arquivos que estão em formato megan no diretório **aula_03**. O programa irá mostrar a atribuição dos reads na árvore taxonômica do NCBI. Você pode expandir ou contrair a árvore nos sentidos horizontal e vertical para visualizar melhor. A barra de status na parte inferior da janela mostra as informações sobre o número de táxons sendo mostrado e dados sobre amostra, como o número de reads analisados e atribuídos. Os parâmetros utilizados na “meganização” também podem ser visualizados na barra de status e estão resumidos abaixo:

• **MinScore** - limite mínimo do bitscore (default = 50.0)  
• **MaxExpected** - limite máximo do e-value (default = 0.01)  
• **TopPercent** - filtra os alinhamentos menos relevantes e considera apenas os que têm bitscore próximo ao melhor alinhamento. Isso é útil para reduzir a quantidade de dados redundantes e focar apenas nos alinhamentos mais informativos para cada read (default = 10.0)  
• **MinSupportPercent** - limite mínimo da porcentagem de reads atribuídos a um determinado táxon (e seus descendentes) para que ele apareça na árvore taxonômica (default = 0.05)  
• **MinSupport** - limite mínimo do número de reads atribuídos a um determinado táxon (e seus descendentes) para que ele apareça na árvore taxonômica  

Os dois últimos parâmetros determinam que os reads atribuídos aos táxons que não atendem aos critérios devem ser contabilizados no nó imediatamente superior da árvore (em direção à raiz), até que um nível taxonômico seja alcançado com uma contagem de reads suficientemente alta.

Por padrão, os círculos representam os táxons e nós da árvore. O tamanho dos círculos corresponde à abundância relativa de cada táxon na amostra, ou seja, é proporcional ao número de reads atribuído a um determinado táxon ou nó. Quando você clica em cada círculo, pode visualizar o número de reads atribuídos àquele táxon específico (**assigned**) e também à soma dos reads atribuídos a todos os táxons descendentes daquele nó (**summed**). 

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/images/megan1.png" alt="Megan1" width="600">
</p>

Você pode colapsar ou expandir os nós para interagir com seus resultados, escolhendo o nível taxonômico que deseja visualizar. 

- ▶️ Clique no botão `Rank` e explore os dados, escolhendo o nível taxonômico que deseja visualizar.
  
Para facilitar a visualização das abundâncias relativas, vamos criar um gráfico no nível taxonômico que você escolher. As folhas da árvore taxonômica precisam estar selecionadas para isso. A seleção é feita automaticamente quando você escolhe o ranqueamento taxonômico com o botão `Rank`. 

- ▶️ Para gerar o gráfico, clique em `Show chart` no menu de opções e escolha `Show Bubble Chart`, ou `Show Stacked Bar Chart`. Uma nova janela vai abrir com o gráfico e você pode ordenar os táxons por ordem crescente ou decrescente do número de reads clicando no menu à esquerda.

Os gráficos abaixo estão em nível taxonômico de Filo.

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/images/megan2.png" alt="Megan1" width="900">
</p>

- ❓ Qual o filo mais abundante na sua amostra?
- ❓ Qual a espécie mais abundante na sua amostra?







---
### 📖 **Referências**
