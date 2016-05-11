<h2> Tutorial </h2>
<p align="justify">
Este projeto apresenta um recomendador de conteúdo, criado a partir do Apache Hadoop e Apache Mahout. Criamos esta arquitetura a partir de uma plataforma Linux (Ubuntu 12.0.4) 64 bits e Docker 1.5.0. Os  procedimentos a seguir descrevem passo a passo a configuração do ambiente e a execução do recomendador de conteúdo.
</p>
<p align="justify">
Após descarregar o projeto, entre no diretório e execute o seguinte comando para executar a build do Docker:
</p>

> docker build -t instance-name .

<p align="justify" style="padding-top: 15px;">
Certifique-se de que a build foi executada corretamente e de que todos os downloads foram executados. Agora você pode criar uma instância Docker dessa máquina. Veja:
</p>

> docker run -d -P --name instance-name instance-name

<p align="justify" style="padding-top: 15px;">
Execute o comando a seguir para descobrir a porta ssh que a instância foi disponibilizada:
</p>

> docker port instance-name

<p align="justify" style="padding-top: 15px;">
Agora você pode se conectar a instância usando ssh. A senha é: 'screencast'.
</p>

> ssh root@localhost -p ssh-port

<p align="justify" style="padding-top: 15px;">
Uma vez conectado a instãncia, crie uma formate a partição do Hadoop para que possamos trabalhar com arquivos. Veja:
</p>

> hadoop namenode -format

<p align="justify" style="padding-top: 15px;">
Inicie o Hadoop.
</p>

> start-all.sh

<p align="justify" style="padding-top: 15px;">
Registre o arquivo <b>dados.csv</b> no Hadoop File System.
</p>

> hadoop fs -put dados.csv dados.csv

<p align="justify" style="padding-top: 15px;">
Execute o Job de recomendação de conteúdo do Mahout e aguarde o processamento.
</p>

> hadoop jar /opt/mahout-distribution-0.9/mahout-core-0.9-job.jar org.apache.mahout.cf.taste.hadoop.item.RecommenderJob -s SIMILARITY_COOCCURRENCE --input dados.csv --output output

<p align="justify" style="padding-top: 15px;">
Recupere a lista de recumendação do Hadoop File System e salve em um arquivo chamado output.txt
</p>

> hadoop fs -getmerge output output.txt

<p align="justify" style="padding-top: 15px;">
O arquivo output.txt contém o identificador do usuário, seguido de uma lista de conteúdos (id e nota) a serem recomendados.
