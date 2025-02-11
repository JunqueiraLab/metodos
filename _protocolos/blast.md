---
layout: default
title: "Protocolo - BLAST"
permalink: /protocolos/blast/
---

# 🔬 BLAST - Basic local alignment search tool
O BLAST é o algoritmo mais utilizado para comparação de sequências, buscando por alinhamentos locais ótimos entre uma sequência de interesse (**query**) e todas as sequências contidas em um banco de dados de referência (Altschul et al. 1990). O BLAST utiliza uma abordagem heurística para buscar similaridades na base de dados, tornando-a mais rápida e eficiente.

O algoritmo necessita de um banco de dados indexado para realizar as buscas. É possível realizar análises remotas utilizando o banco de dados já indexado com a conexão ao NCBI, ou indexar bancos de sequências a partir de arquivos FASTA utilizando o módulo `makeblastdb` (disponível na linha de comando do BLAST). Durante a indexação, um identificador único é atribuído a cada sequência no banco de dados, que pode também ser associado a um nó taxonômico (taxID). Isso é particularmente poderoso para análises em que se quer restringir os grupos taxonômicos ou sequências de referência. 

Na etapa de busca, o BLAST quebra a query em palavras curtas e cria uma tabela de pesquisa (**lookup table**), usada para procurar estas palavras no banco de dados usando uma estratégia chamada de alinhamento local. Estes alinhamentos são curtos e sem lacunas (gaps) inicialmente. Os trechos similares são escolhidos de acordo com uma matriz de pontuação e as palavras que tiverem a pontuação acima de um limite, são utilizadas como sementes (**seeds**) para estender bidirecionalmente o alinhamento entre a sequência de interesse e a sequência do banco. Este passo é realizado para obter pares de sequências com pontuações altas (ou HSPs, do inglês high scoring segment pairs - **pares de segmentos com pontuação alta**). O comprimento padrão das sementes é de 3 aminoácidos para proteínas e 11 nucleotídeos para DNA. Os pares de sequências com alta similaridade são estendidos para alinhamentos sem lacunas mais longos e, em seguida, para alinhamentos com lacunas em fases posteriores dos programas BLAST (Morgulis et al. 2008).

No entanto, a porcentagem de similaridade entre duas sequências não é suficiente para um match (correspondência) confiável. Para isso, o **e-value** (expectation value, ou valor esperado) é utilizado como um parâmetro mais informativo. O e-value pode ser entendido como o número de alinhamentos de igual ou melhor qualidade que seriam encontrados ao acaso quando uma pesquisa contra o banco de dados de referência é feita. Assim, quanto menor o valor do e-value, maior é a confiabilidade de que o match reportado pelo BLAST é significativo e menores são as chances de que a sequência alvo tenha sido alinhada com uma sequência aleatória do banco de dados. Por exemplo, um e-value de 1e-5 (ou 1x10-5) significa que a chance de o resultado do alinhamento ser aleatório é de 0,00001 (ou 1 em 100 mil). O usuário deve se atentar, no entanto, para o fato de que alinhamentos idênticos contra bancos de dados diferentes podem resultar em e-values diferentes, pois este valor é dependente do tamanho da sequência, mas também do número de sequências presente nos bancos de dados.

Uma outra medida importante para os resultados é o **bit-score**, que indica a significância estatística de um alinhamento. O bit-score leva em consideração a frequência das palavras encontradas no banco de dados e a frequência esperada de palavras aleatórias, além do tamanho e de parâmetros específicos usados para o alinhamento (como matriz de substituição e penalização de gaps). Em termos práticos, quanto maior o bit-score, mais semelhantes são as duas sequências. Os bit-scores abaixo de 50 geralmente não são confiáveis. Como os bit-scores são medidas normalizadas, elas podem ser usadas para comparar as pontuações de alinhamento contra bancos de dados de diferentes tamanhos.

Ao final da busca, o BLAST retorna uma lista de sequências do banco de dados (**subject**) que apresentam alinhamentos significativos com a sequência de interesse, classificando-as por ordem decrescente de significância estatística. Isso permite que os pesquisadores identifiquem rapidamente as sequências mais similares à **query** e possam então realizar análises adicionais, como a atribuição taxonômica, construção de árvores filogenéticas ou análise funcional.

## Algoritmos de busca do BLAST

Existem cinco módulos do BLAST que podem ser utilizados para diferentes tipos de análise:

- **blastn** - Compara a sequência de nucleotídeos contra um banco de dados de nucleotídeos (**nt-nt**).
- **blastp** - Compara sequências de aminoácidos contra um banco de dados de proteínas (**aa-aa**).
- **blastx** - Traduz uma sequência de nucleotídeos nos 6 quadros de leitura possíveis e utiliza as traduções para buscas em bancos de dados de proteínas (**nt traduzido-aa**).
- **tblastn** - Busca por similaridade entre uma sequência de aminoácidos contra a tradução em 6 quadros de leitura possíveis do banco de dados de nucleotídeos (**aa - nt traduzido**).
- **tblastx** - Traduz uma sequência de nucleotídeo nos 6 quadros de leitura possíveis e compara os resultados contra a tradução das sequências nucleotídicas do banco de dados nos 6 quadros de leitura possíveis. É o mais lento dos algoritmos e tem por objetivo encontrar similaridades entre sequências de nucleotídeos distantemente relacionadas (**nt traduzido - nt traduzido**).

---

## 🌐 **BLAST Online**

Agora vamos fazer um exercício para analisar os resultados do BLAST online, utilizando o webserver do NCBI, de uma sequência desconhecida retirada do arquivo `illumina.fastq`.

### **Passos para executar o BLAST no NCBI**

1. **Acesse a página do BLAST no NCBI:** <a href="https://blast.ncbi.nlm.nih.gov/Blast.cgi" target="_blank">Clique aqui</a>
2. **Escolha** “Nucleotide BLAST” (**blastn**).
3. **Abra o arquivo `AE001.fasta`** na pasta da `aula01`, copie e cole a sequência no campo **“Enter Query Sequence”**.
4. **Atente para os parâmetros da busca.**
5. **Analise os resultados:**
   - **A qual organismo a sequência pertence?**
   - **Qual o valor do e-value?**
   - **Qual o valor do bit-score?**
   - **Você acha este um resultado confiável?**

📌 **Dica:** O **e-value** indica a significância do alinhamento. Quanto **menor o e-value**, mais significativo é o resultado.

🔬 Analise os resultados e registre suas observações! 🙂

---

## 🖥️ **BLAST na linha de comando**

Em muitos projetos que utilizam sequenciamento massivo, é praticamente impossível fazer o BLAST via web. Isso ocorre porque existem limites impostos pelo NCBI para a análise de dados. Por exemplo, o BLAST via web tem um limite de tamanho das sequências de nucleotídeos e proteínas para serem submetidas (1 milhão de nt ou 100 mil aa) e o uso de processamento nos servidores online também é limitado. Além disso, você pode ter um volume grande de análises que quer processar de forma automatizada com scripts. Para estas ocasiões, é possível realizar sua análise via linha de comando. Para isso, você deve instalar o pacote do BLAST+ no seu computador ou servidor a partir do download do pacote do [NCBI](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/){:target="_blank"} ou via um gerenciador de pacotes e programas (por exemplo: `conda install blast`). Uma vez com o pacote instalado, você pode fazer uma análise similar à que fez via web. 

❗ **Atenção** O BLAST+ já está instalado no seu computador e pode ser ativado com o comando:

```micromamba activate metodos```

Para rodar o BLAST, a linha de comando básica do blastn é:

```blastn -db [database] -query [input_file] -remote```

- **blastn** é o comando para o algoritmo de alinhamento de sequências de nucleotídeos contra um banco de nucleotídeos
- **-db** indica qual banco será utilizado na busca (nt, para nucleotídeos ou nr para proteínas, por exemplo)
- **-query** é o input do arquivo de sequência que se quer buscar
- **-remote** indica que o comando deve fazer a busca online

Agora você pode fazer a mesma análise que fez via webserver utilizando a seguinte linha de comando:

```blastn -db nt -query AE001.fasta -remote```

ℹ️ O pacote do BLAST+ provê todos os módulos necessários para realizar as análises com nucleotídeos e aminoácidos (blastn, blastp, blastx, tblastn, tblastx), além de outros módulos como o megablast (algoritmo alterado), psiblast, rpsblast (busca por perfis em regiões delimitadas de sequências) e o makeblastdb, para indexar bancos de dados (Altschul et al. 1997).
Quer saber mais sobre o BLAST na linha de comando? 
Acesse o [BLAST® Command Line Applications User Manual](https://www.ncbi.nlm.nih.gov/books/NBK279690/){:target="_blank"}.

