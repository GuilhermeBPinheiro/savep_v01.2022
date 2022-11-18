SAVEP_v01.2022
Pipeline para Anotação de Arquivo VCF de Variantes Somáticas utilizando o Ensembl Variant Effect Predictor (VEP) version 105.0 via Google Colab. O SAVEP_v01.2022 é de código aberto e está disponível no GitHub.
Tempo de duração para rodar o pipeline: ~20-25 minutos (esse tempo foi estimado com base nos arquivos de exemplo, mas pode sofrer alterações principalmente devido ao tamanho do seu arquivo VCF).

1. Apresentação:
1.1. O que é Bioinformática?
1.2. O que são Variantes Somáticas?
1.3. O que é VEP?
2. Objetivo:
3. Metodologia:
3.1. Pré-requisitos obrigatórios:
3.2. Pré-requisitos opcionais:
4. Introdução:
4.1. Montar o Ambiente de Trabalho:
4.2. Instalar Programas:
4.3. Material Fornecido:
4.4. Adicionar Arquivos:
5. Aplicações:
5.0. Etapa I:
5.1. Etapa II:
5.2. Etapa III:
6. Agradecimentos:
7. Contatos:

1. Apresentação:
Somatic Annotator Ensembl Variant Effect Predictor, ou simplesmente SAVEP. É um Pipeline de Bioinformática para anotação de arquivos VCF de variantes somáticas, que utiliza tecnologia da Ensembel Variant Effect Predictor (VEP) em sua versão 105.0 via ambiente nuvem do Google Colab. 

1.1. O que é Bioinformática?
A ideia de um Pipeline (ou popularmente Pipe) pode ser definida como, uma sequências de etapas a serem executadas desde do um dado bruto até a entrega de um resultado determinado por diversos parâmetros. E, dentro da área de bioinformática, podemos separar 5 (cinco) etapas principais de um Pipe:

A.	Sequenciamento de Nucleotídeos  Sequenciador converter uma amostra de um paciente X em específicos pedaços de pequenas ou longas sequências de nucleotídeos (vai depender da metodologia do sequenciador). Tais informações serão armazenadas em um formato de arquivo em texto denominado FASTQ. Geralmente, possui dois arquivos FASTQ, cada um representa um sentido da linha de DNA.
 
Figura 1. FASTQ. Fonte: https://www.drive5.com/usearch/manual/fastq_files.html.
Observações #01: 
No arquivo FASTQ, podemos separar o arquivo em 4 linhas obrigatórias, cada um representando uma informação, sendo estas: 1. Identificador de sequência com informações sobre a execução de sequenciamento e o cluster (pode varia de acordo com o software de conversão BCL para FASTQ usado) / 2. A sequência (as chamas de bases; A, C, T, G e N) / 3. Um separador, que é simplesmente um sinal de mais (+) / 4. As pontuações básicas de qualidade da chamada – estes são codificados em Phred+33, usando caracteres ACSII para representar as pontuações de qualidade numérica. 

B.	Alinhamento de Nucleotídeos  É feito uma união de todos os sequenciamentos obtidos pelo sequenciador, e para realizar essa etapa é necessário comparar a sequência do paciente com uma sequência de um genoma referência (hg19 ou hg38) para observar seu nível de similaridade. Portanto, nessa etapa um novo formato de arquivo é gerado, um arquivo binário denominado de BAM (Binary Alignment / Map).
Observações #02: 
Nos arquivos BAM são adequados para visualização com um visualizador externo, como IGV ou o UCSC Genome Browser. Se você usar um aplicativo no BaseSpace que usa arquivos BAM como entrada, o aplicativo localizará o arquivo ao ser iniciado. Se estiver usando arquivos BAM em outras ferramentas locais, baixe o arquivo para usá-lo na ferramenta externa.
 
Figura 2. BAM. Fonte: https://en.wikipedia.org/wiki/Binary_Alignment_Map.

C.	Chamadas de Variantes  Após realizar o alinhamento das sequências, precisa de uma etapa para classificar possíveis variantes gênicas. Estas variantes podem acarretar em mutações benignas (que não causam impacto patogênico) ou malignas (que causam impacto patogênico). Vale ressalta que tem outros parâmetros para determinar o grau de impacto de cada variante individualmente e/ou conjunto dela. Essas variantes podem ser identificadas por SNPs (trocas) ou Indels (deleções ou inserções) de trechos de bases de nucleotídeos. O resultado é um arquivo que contêm apenas essas alterações, um arquivo que possui um formato especifico padrão com cabeçalho e um conjunto de diversas linhas que representa as amostras e questão, esse arquivo é denominado VCF (Variant Call Format). 
Observações #03: 
No VCF cada linha representa uma variante detectada, contendo sempre de forma fixa as informações de 8 (oito) colunas: cromossomo / posição / identificador /  alelo referência / alelo alternativo / qualidade / filtro. E pode conter 1 (um) ou mais colunas adicionas, tais como: informações / amostras.
 
Figura 3. VCF. Fonte: https://en.wikipedia.org/wiki/Variant_Call_Format.

D.	Anotação de Variantes  Após a etapa de chamada de variantes, a etapa subsequente será anotar as variantes, e essa etapa ajudar a enriquecer informações para determinar as variantes, como por exemplo: determinar sua coordenada genômica, a sequência prevista de aminoácidos (p.) e o dano à proteína, qual sua frequência alélica (VAF), com quais doenças se relaciona (OMIM), qual é a sua classificação nos bancos de dados de variantes clínicas (CLINVAR) e nos de predição in sílico (Polyphen entre outros), qual sua frequência populacional, etc. Portanto, é onde iremos aplicar os filtros que farão toda diferença na hora de ver o resultado final. Podemos iniciar essa etapa com mais de 100 variantes e terminar com apenas uma ou duas. Nesta etapa geramos um arquivo no formato de tabela denominado CSV.
 
Figura 4. CSV. Fonte: https://colab.research.google.com/drive/1n7o2NFkU5OQnB0aCmLDMVaXKGF3vY_ie#scrollTo=E9Y7CHSdDDLy.


E.	Análises de Variantes  É quando iremos classificar as variantes após filtragem e determina o impacto delas conforme as normas de classificação HGVS. É gerada uma lista com apenas algumas poucas variantes identificadas como relevantes para o quadro clínico do paciente. Elas são então novamente analisadas, revisadas e reportadas no laudo para o médico solicitante.
Observações #04: 
Ao final da análise, podemos terminar com uma tabela com uma ou duas variantes para laudo do paciente em questão. 

SIMPLIFICAÇÃO DE UM PIPELINE COMPLETO:
 
Figura 5. Pipeline Completo. Fonte: https://h600.org/wiki/Sequencing+File+Formats.

1.2.	O que são Variantes Somáticas?
O que diferente variantes somáticas de germinativas é a que estão presentes apenas nas células tumorais e/ou que possui capacidade de oncogênese. Outra diferença é a diferenciação de análises somáticas das germinativas é a forma de classificar as variantes, pelo qual variantes somáticas são classificas com base em seu nível de significância clínica. 


Baseado em seu impacto clínico, classificamos as variantes somáticas em 4 (quatro) categorias:
	TIER I: variantes com forte significância clínica (nível de evidência A e B);
	TIER II: variantes com potencial significado clínico (nível C ou D de evidência);
	TIER III: variantes com significado clínico desconhecido;
	TIER IV: variantes que são benignas ou provavelmente benignas.
Os principais bancos indicados para análise de variantes sómaticas são:
	cBioPortal for Cancer Genomics - http://www.cbioportal.org/;
	My Cancer Genome: Genetically Informed Cancer Medicine - http://www.mycancergenome.org/;
	The University of Texas: MD Anderson Cancer Center (Making Cancer History) - https://pct.mdanderson.org/;
	Varsome: The Human Genomics Community - https://varsome.com/;
	Franklin by Gennox: The Future of Genomic Medicine - https://franklin.genoox.com/clinical-db/home.

1.3.	O que é VEP?
VEP é uma ferramenta tecnológica com código aberto que é utilizado para realizar anotações e filtragem de variantes genômicas, e prevê consequências moleculares das variantes anotados usando os conjuntos de genes Ensembl/GENCODE ou RefSeq.
Possui 3 (três) tipos de interface – cada um com sua documentação e ambiente com foco especifico:
 
Figura 6. Interfaces do VEP. Fonte: https://www.ensembl.org/info/docs/tools/vep/index.html.
Observações #05: 
Usaremos nesse Pipeline a interface Command line tool do VEP.
2. Objetivo:
O script SAVEP_v01 se encontra na sua primeira versão e tem como funcionalidade pular algumas etapas de um Pipeline de Bioinformática padrão. Profissionais bioinformatas que possuam documentos pós etapa de Chamada de Variantes possa realizar a etapa de Anotação de Variantes. Esse script funciona com auxílio da ferramenta da VEP – com a interface de trabalho escolhida sendo: command line tool. Para implementar essa interface será utilizado o recurso e tecnologia do Google, denominada de Google Colab  espaço web que garante rodar códigos sem a necessidade de instalar vários pacotes e programas externos.
Observações #06: 
O ANNOVAR é a principal ferramenta utilizada aqui para converter o arquivo BAM em VCF. Mas, uma alternativa para essa ferramenta que é bem utilizada, é justamente o VEP. 
Dica #01:
Para entender as diferenças e semelhanças entre softwares de anotações de variantes, recomendo a leitura de um arquivo publicado no blog The Golden Helix sobre o assunto. Disponível em: https://blog.goldenhelix.com/the-sate-of-variant-annotation-a-comparison-of-annovar-snpeff-and-vep/.

3. Metodologia:
3.1. Pré-requisitos obrigatórios:
•	Conexão boa com a internet  toda execução necessita de uma rede instável, pois a maneira que é feita será totalmente via ambiente nuvem;
•	Bom espaço de armazenamento nuvem   como o ambiente nuvem será utilizado para execução do nosso script, então é bom ter um bom espaço para armazenar os documentos que serão obtidos pós script. Seja esse ambiente sua própria máquina ou espaço compartilhado de seu local de trabalho;
•	Paciência  dependendo do tamanho do arquivo a ser estudado, pode demorar alguns minutos a mais, então é necessário ter um pouco de paciência tanto para executar como para ler atentamente cada etapa;

•	Conhecimentos básicos das linguagens: Python (versão 3) e Bash  alguns comandos básicos de ambas as linguagens serão utilizadas, caso deseje formatar e adicionar requisitos a mais, será necessário algum conhecimento mais avançado, porém para realização e execução de nosso script, todos os comandos estarão disponível;
•	Bibliotecas Python: Pandas  biblioteca de software criada para a linguagem Python para manipulação e análise de dados. Em particular, oferece estruturas e operações para manipular tabelas numéricas e séries temporais;
•	Ter uma conta no Google Colab  escolhido por não necessitar de programas extras para instalação, apenas linhas de códigos para sua execução;
•	Adicionar o material fornecido  alguns arquivos serão fornecidos para que possa rodar o programa em perfeita harmonia, um desses arquivos é o “Genoma de Referência” - no caso disponibilizamos a versão do Homo Sapiens GRCh37 (Homo_sapiens_assembly19.fasta).

3.2. Pré-requisitos opcionais:
•	Conhecimentos avançados das linguagens  Python (versão 3) e Bash  caso deseja realizar algumas modificações nesse código livre gratuito para estudos e aperfeiçoamento do programa;
•	Conhecimento de outras linguagens  R, é um bom exemplo de linguagem principalmente análise de dados;
•	Conhecimentos em bancos de dados  da mesma maneira que seria interessante incrementar uma outra linguagem, aplicar os bancos de dados dessas linguagens, permitiria uma melhor análise.
Dica #02:
Caso deseje entender comandos mais avançados de linguagem, recomendarei a documentação de ambas as linguagens utilizadas nesse script: Python3 e Bash. Disponível em: https://docs.python.org/3/ e https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html.
4. Introdução:
4.1. Montar o Ambiente de Trabalho:
I. Criar conta no Google Colab:
Começa criando uma conta no Google Colab (https://colab.research.google.com/). Após criado o Colab, crie um “novo notebook” e der dois cliques no título, onde estará escrito “Untitled1.ipynb”, e renomeei como desejar (recomendo colocar nome do nosso script + _ + seu nome). A tela inicial do Colab apareça conforme mostrado na figura abaixo:
 
Figura 7. Tela Inicial do Colab. Fonte: https://colab.research.google.com/.
Conecte o Colab com ambiente na nuvem do Google Drive. Usando a seguinte linha de código:
from google.colab import drive
drive.mount(‘/my-drive’) #Modificar o caminho entre () e colocar onde deseja salvar seu programa. 
Após rodar a linha de código e aparecer algumas mensagens pelo qual vai precisar só apertar os seguintes botões: “Conectar ao Google Drive” e “Permitir”.
 
 
Figura 8-9. Permissões do Google Drive. Fonte: https://colab.research.google.com/.
Se tudo der certo, o seu ambiente de trabalho vai ficar da seguinte maneira:
 
Figura 10. Google Drive conectado ao Google Colab. Fonte: https://colab.research.google.com/.
Observações #07: 
Você pode usar uma pasta que pode estar no “meu drive” ou em “drives compartilhados”, só precisa copiar o caminho pelo qual está selecionado a linha vermelha (demostrado na figura abaixo). 
 
Figura 11. Tela do Google Drive. Fonte: https://colab.research.google.com/.

II. Criar Diretórios:
Antes de mostra os comandos usados dentro do Google Colab para criar diretórios, irei usar esse espaço para explicar um pouco da linguagem Bash e uma lista de comandos básicos que você precisará aprender para aplicar esse Pipe em seu projeto. 
 
Figura 12. Comandos de Gestão de Arquivos e Diretorias do Bash. Fonte: https://www.devmedia.com.br/comandos-importantes-linux/23893.

GNU Bash ou simplesmente Bash (abreviação de “Bourne Again SHell”) é um shell Unix e um interpretador de linguagem de comando. Um entre os diversos tradutores entre o usuário e o sistema operacional conhecidos como shell. Acrônimo para "Bourne-Again SHell", o Bash é uma evolução retrocompatível muito mais interativa do Bourne Shell. Foi lançado em 8 de junho de 1989 e tem como criador Brian Fox. Fonte: https://pt.wikipedia.org/wiki/Bash.
Todas as linguagens possui diversos tipos de conceitos, sejam eles variáveis, objetos, tipos de dados, funções, métodos e diversos elementos necessários para sua sintaxe. Em Bash não é muito diferente, possui diversos comandos que tem diferentes funções, tais como: Controle e Acesso / Comunicações / Ajuda e Documentação / entre outros. 
Dica #02:
Caso deseje entender os principais comandos e funções de Bash, recomendarei a leitura no blog DevMedia. Disponível em: https://www.devmedia.com.br/comandos-importantes-linux/23893.

Tabela 01. Listas de Comando Básico Bash
|            | Comandos de Gestão de Arquivos e Directorias                                                                                                                                                       |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| cd         | Mudar de diretório atual, como por exemplo cd diretório, cd .., cd /                                                                                                                               |
| chmod      | Mudar a proteção de um arquivo ou diretório, como por exemplo chmod 777, parecido com o attrib do MS-DOS                                                                                           |
| chown      | Mudar o dono ou grupo de um arquivo ou diretório, vem de change owner                                                                                                                              |
| chgrp      | Mudar o grupo de um arquivo ou diretório                                                                                                                                                           |
| cmp        | Compara dois arquivos                                                                                                                                                                              |
| comm       | Seleciona ou rejeita linhas comuns a dois arquivos selecionados                                                                                                                                    |
| cp         | Copia arquivos, como o copy do MS-DOS                                                                                                                                                              |
| crypt      | Encripta ou Descripta arquivos (apenas CCWF)                                                                                                                                                       |
| diff       | Compara o conteúdo de dois arquivos ASCII                                                                                                                                                          |
| file       | Determina o tipo de arquivo                                                                                                                                                                        |
| grep       | Procura um arquivo por um padrão, sendo um filtro muito útil e usado, por exemplo um cat a.txt \| grep ola irá mostrar-nos apenas as linhas do arquivo a.txt que contenham a palavra “ola”         |
| gzip       | Comprime ou expande arquivo                                                                                                                                                                        |
| ln         | Cria um link a um arquivo                                                                                                                                                                          |
| ls         | Lista o conteúdo de uma diretório, semelhante ao comando dir no MS-DOS                                                                                                                             |
| lsof       | Lista os arquivos abertos, vem de list open files                                                                                                                                                  |
| mkdir      | Cria uma diretório, vem de make directory                                                                                                                                                          |
| mv         | Move ou renomeia arquivos ou diretórios                                                                                                                                                            |
| pwd        | Mostra-nos o caminho por inteiro da diretório em que nos encontramos em dado momento, ou seja um pathname                                                                                          |
| quota      | Mostra-nos o uso do disco e os limites                                                                                                                                                             |
| rm         | Apaga arquivos, vem de remove, e é semelhante ao comando del no MS-DOS, é preciso ter cuidado com o comando rm * pois apaga tudo sem confirmação por defeito                                       |
| rmdir      | Apaga diretório, vem de remove directory                                                                                                                                                           |
| stat       | Mostra o estado de um arquivo, útil para saber por exemplo a hora e data do último acesso ao mesmo                                                                                                 |
| sync       | Faz um flush aos buffers do sistema de arquivos, sincroniza os dados no disco com a memória, ou seja escreve todos os dados presentes nos buffers da memória para o disco                          |
| sort       | Ordena, une ou compara texto, podendo ser usado para extrair informações dos arquivos de texto ou mesmo para ordenar dados de outros comandos como por exemplo listar arquivos ordenados pelo nome |
| tar        | Cria ou extrai arquivos, muito usado como programa de backup ou compressão de arquivos                                                                                                             |
| tee        | Copia o input para um standard output e outros arquivos                                                                                                                                            |
| tr         | Traduz caracteres                                                                                                                                                                                  |
| umask      | Muda as proteções de arquivos                                                                                                                                                                      |
| uncompress | Restaura um arquivo comprimido                                                                                                                                                                     |
| uniq       | Reporta ou apaga linhas repetidas num arquivo                                                                                                                                                      |
| wc         | Conta linhas, palavras e mesmo caracteres num arquivo                                                                                                                                                                    | uncompress | Restaura um arquivo comprimido                                                                                                                                                                     | uniq       | Reporta ou apaga linhas repetidas num arquivo                                                                                                                                                      | wc         | Conta linhas, palavras e mesmo caracteres num arquivo                     
                                                                                                                         
Após explicado o que é Bash e alguns comandos básicos, use a seguinte linha de código para criar diretórios: 
%%bash
mkdir savep_v01 #comando para cariar diretório
mkdir homoSapiens_refSeq #diretório onde vai recebe o material fornecido
mkdir anotacao #diretório onde vai receber o output final
mv homoSapiens_refSeq savep_v01 #mover diretório para dentro do diretório principal
mv anotacao savep_v01 #mover diretório para dentro do diretório principal
cd savep_v01 #para entrar nesse diretório e fixar como principal
O resultado do seu ambiente deve ficar da seguinte maneira: 
 
Figura 13. Tela do Colab com diretórios criados. Fonte: https://colab.research.google.com/.
Observações #08: 
Mova os diretórios para dentro do drive utilizando o comando mv + nomes_direitorios. É possível você crie já as pastas necessárias dentro do seu Google Drive e quando você linkar com o ambiente de nuvem, terá já o ambiente para receber seus outputs. Para confirmar que o diretório que será utilizado é o que queremos, podemos usar: %%bash + pwd.
Dica #03:
Há uma outra maneira de criar diretórios sem usar linhas de códigos. Seria indo diretamente no seu Google Drive e criando pasta diretamente lá. Conseguiria renomear, colocar arquivos, mover, excluir e tudo mais, basicamente poderia criar todo ambiente antes e após conectar seu Drive no Colab, todo seu ambiente estaria presente na aba “Arquivos”.  
4.2. Instalar Programas:
IIIa. Instalando VEP ensembl-vep-105.0 - via GitHub
Tempo de Instalação: ~1 minuto
A versão escolhida para ser utilizada é 105.0. No GitHub possui uma região que possui todos os repositório, vai possui não apenas da versão 105.0 do VEP, mas de todas as versões que já foram lançadas.
 
Figura 14. Repositórios do VEP. Fonte: https://github.com/Ensembl/ensembl-vep/tags.
Use a linha de código para instalação:
%%bash
mkdir savep_v01 #comando para cariar diretório
mkdir homoSapiens_refSeq #diretório onde vai recebe o material fornecido
mkdir instalacao #diretório onde vai recebe os arquivos de instalação
mkdir anotacao #diretório onde vai receber o output final
mv homoSapiens_refSeq savep_v01 #mover diretório para dentro do diretório principal
mv anotacao savep_v01 #mover diretório para dentro do diretório principal
mv instalacao savep_v01 #mover diretório para dentro do diretório principal
cd savep_v01 #para entrar nesse diretório e fixar como principal
Mover o arquivo
mv /content/105.0.tar.gz /content/savep_v01/instalacao







Como resultado do código acima, deve aparecer:
 
Figura 15. Tela de Instalação do VEP concluída. Fonte: https://colab.research.google.com/.

•	Verificação do VEP - executando o script ./vep
Entrei dentro do diretório ensembl-vep-105.0 e executei o script ./vep.
%%bash
cd ensembl-vep-105.0/
./vep
Resultado esperado:
 
Figura 16. Tela de Menu do VEP. Fonte: https://colab.research.google.com/.

IIIB. Instalando biblioteca Pandas
Tempo de Instalação: ~30 segundos
Use a linha de código para instalação:
!pip install pandas
Observação #09:
O comando %%bash é usado para usar as funções da linguagem Bash, enquanto o comando com sinal de exclamação ! é usado para usar as funções da linguagem Python.
Resultado esperado:
 
Figura 17. Tela de Instalação Concluída do Panda. Fonte: https://colab.research.google.com/.

4.3. Material Fornecido:
O material fornecido é graça ao profissional Renato Puga (link do GitHub dele). Disponível no link: https://drive.google.com/drive/folders/1s_UInfwIbATc8qEw4pBbT5w-Tdb9P2MF. 
Use esse link para baixar os seguintes arquivos: 
	"homo_sapiens_refseq/Homo_sapiens_assembly19.fasta" – contém 2,9 Gg;
	"homo_sapiens_refseq/Homo_sapiens_assembly19.fasta.fai" – contém 3 Kb.
Após baixar selecionando os arquivos (conforme a figura 18), pode adicionar diretamente no Google Colab, fazendo um upload (conforme a figura 19).
 
Figura 18. Fazendo Download do Material Fornecido. 

Dica #04: 
Conforme dito na Dica #03, poderia adicionar os arquivos e dados da pasta de material do seu computador ou de um ambiente que esteja os documentos diretamente em uma pasta do Drive que deseja conecta-la ao Colab, e todo seu material fornecido estaria disponível desde do começo. 
 
Figura 19. Fazendo Upload do Material Fornecido. 

O resultado esperado ao fim de tudo:
 
Figura 20. Arquivo Fasta no Colab. 

4.4. Adicionar Arquivos Input:
Formato do Arquivo:
É possível subir arquivos no formato VCF que conectam o sequenciamento, independentemente da origem do sequenciamento. Arquivos compactados de extensão .gz também são aceitos. 
 
Figura 21. Exemplo de Arquivo VCF. 
Observação #10:
Qualquer arquivo VCF que se encaixe no modelo acima que esteja na sua máquina, você pode utilizar.
Para adicionar o arquivo no seu script, possui duas possibilidades:
Opção A – utilize o seguinte comando:
from google.colab import files
Seu_Arquivo_VCF = files.upload()
 
O resultado esperado:
 
Figura 22. Exemplo de Arquivo VCF sendo adicionado. 

Opção B - Conforme visto na figura 19, basta você adicionar o arquivo VCF em questão diretamente no Google Colab.
 
Figura 23. Fazendo Upload do Arquivo VCF 

Dica #05: 
Conforme dito na Dica #03 e #04, poderia adicionar os arquivos VCF do seu computador ou nuvem diretamente em uma pasta do Drive que deseja conecta-la ao Colab, e assim adiantaria o processo de criar uma linha de código. 
O resultado esperado é este: 
 
Figura 24. Arquivo VCF no Colab. 

Verificação do VCF --> quantas variantes possui - executando o script !zgrep -cv "#" + caminho onde está seu VCF
Exemplo:
 
Caso tenha seguido as Dicas #03, #04 e #05, seu drive estará semelhante a figura abaixo:
 
Figura 25. Tela do Drive com suas pastas e arquivos já inseridos. Fonte: https://drive.google.com/drive/folders/.

5. Aplicações:
Documentação para VEP
Disponível em: http://www.ensembl.org/info/docs/tools/vep/script/vep_download.html#installer.


5.1. Etapa I – Toda o tópico de Introdução
Essa etapa pode se resumi praticamente por toda parte anterior de introdução, desde da criação do ambiente, até instalações dos programas e importação dos arquivos, seja o fornecido quanto o que deseja avalia.

5.2. Etapa II – Aplicar VEP para filtrar arquivo VCF
Tempo de Instalação: ~6-8 minuto  Usando um arquivo VCF que tinha  17.151 variantes (WP312.filtered.vcf.gz)
%%bash
./ensembl-vep-105.0/vep  \ #Ativa o programa ensembl-vep-105.0
  --fork 3 \ #Mandar ele rodar em 3 máquinas
  -i /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz \ #Colocar o input de onde está o arquivo VCF que deseja rodar no programa
  -o /content/drive/Shareddrives/T4-2022/GuilhermeBueno/NOVEMBRO2022/WP312.filtered.vcf.tsv \  #Diretório onde deseja salvar seu output
  --dir_cache /content/drive/Shareddrives/T4-2022/ \ #Diretório onde deseja salvar seu output
  --fasta /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/Homo_sapiens_assembly19.fasta \ #Caminho onde está o arquivo Fasta do Material Fornecido
  --cache --offline --assembly GRCh37 --refseq  \
  --pick --pick_allele --force_overwrite --tab --symbol --check_existing --variant_class --everything --filter_common \ #Configurações de filtragem e análise do arquivo VCF
  --fields "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
  --individual all
Dica #06:
Caso deseje entender e compreender os filtros que podem ser usados, leia a documentação de filtros do VEP. Disponível em: https://www.devmedia.com.br/comandos-importantes-linux/23893.

Tabela 02. Listas de Flags Utilizadas
| Parâmetros            | Descrição                                                                              |
| :-------------------- |:---------------------------------------------------------------------------------------|
| -i                    | arquivo CF input                                                                       | 
| -o                    | arquivo formato .tsv output                                                            |   
| --fork                | número de threads                                                                      |
| --dir_cache           | diretório de cache                                                                     |
| --fasta               | sequência referência                                                                   |
| --cache               | habilita uso do cache                                                                  |
| --offline             | roda sem uso de conexão com internet                                                   |
| --assembly            | versão da montagem                                                                     |
| --refseq              | esepcificque esta opção caso tenha instalado refseq cache                              |
| --pick                | Escolha uma linha ou bloco de dados de consequência por variante                       |
| --pick_allele         | escolhe uma linha ou bloco de dados de consequência por alelo variante                 |
| --force_overwrite     | escreve sobre arquivo existente                                                        |
| --tab                 | output em formato tabular                                                              |
| --symbol              | adiciona símbolo dos genes                                                             |
| --check_existing      | Verifica a existência de variantes conhecidas que estão co-localizadas com sua entrada |
| --variant_class       | Fornce a classe de variante da Sequence Ontology                                       |
| --everything          | Busca todas as colunas                                                                 |
| --filter_common       | Exclui variantes com frequência > 1% no banco 1000 geomas                              |

Verificar do Arquivo Output gerado:
!grep -v "##" /content/drive/Shareddrives/T4-2022/GuilhermeBueno/NOVEMBRO2022/WP312.filtered.vcf.tsv #Diretório onde salvou seu output
Resultado esperado:
 
Figura 26. Arquivo VCF pós VEP 

5.2. Etapa III – Gerar uma Tabela com Filtros das Variantes
Aplica um código para gerar a tabela com filtros das variantes do seu arquivo output da etapa anterior. 
import pandas as pd
import csv
tabela = pd.read_csv('/content/drive/Shareddrives/T4-2022/GuilhermeBueno/NOVEMBRO2022/WP312.filtered.vcf.tsv', sep='\t', skiprows=38) #Usar o caminho do diretório onde está salvou o output do seu arquivo VCF pós passar pelo VEP
df = pd.DataFrame(tabela)
df
Dica #07:
O comando skiprows do Pandas, é utilizado para pular algumas linhas. Esse recurso é ótimo para este caso, pois não precisamos do cabeçalho, assim ele não alterar a formatação da tabela que será gerada. Para mais entendimento do comando, acesse a documentação. Disponível em: https://www.statology.org/pandas-skip-rows/.

Exemplo:
 
Figura 27. Arquivo final com FIltros

6. Agradecimentos:
Renato Puga (Acessem seu perfil: https://github.com/renatopuga)
Foi graça ao material de fornecido (fasta) e o comando skiprows (etapa III), que ajudou na construção de Pipeline. Além de todo suporte e conhecimento trocado. É um excelente profissional e tem todo meu respeito!
Keren Xu (Acessem seu perfil: https://github.com/XUKEREN) 
Foi graça a essa incrível profissional e seu projeto nomeado de “vcfannotatoR” (https://github.com/XUKEREN/vcfannotatoR) que inspirou em criar esse novo pipeline como alternativa para anotação de variantes somáticas. 

7. Contato
	Email: gbueno0331@gmail.com
	LinkedIn: linkedin.com/in/guilherme-bueno-a96806192/
	Instagram: @gbuen0_
