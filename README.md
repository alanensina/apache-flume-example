# Apache Flume
Nesse repositório, iremos seguir o exemplo básico do Apache Flume que está disponível em sua documentação oficial.

# Requisitos*: 
- Java 8+
- Apache Flume 1.9.0
- Netcat

_* Configurar as variáveis de ambiente para o Java e o Apache Flume_

# Arquivo de configuração do agente Flume:
O primeiro passo é configurarmos um arquivo de configuração para o nosso Agente Flume. Esse arquivo se assemelha a um formato de arquivo de propriedade Java com configurações de propriedade hierárquicas.
Quanto a extensão do arquivo, o Flume não restringe a utilização para um tipo só de extensão, porém por convenção geralmente é utilizado como .conf.
Criamos um example.conf onde colocaremos todas as configurações do nosso agente Flume. 
Primeiro, vamos definir qual será o nome do nosso agente Flume. Podemos colocar qualquer nome, nesse caso, nosso agente Flume é chamado a1.
Em seguida, na primeira seção devemos definir o nome de todos os nossos componentes, ou seja, o source (fonte), o sink (coletor) e os channels (canais). Como podemos observar, nosso source será chamado s1, nosso sink será k1, e o channel será c1.
Na seção seguinte definimos as configurações da nossa source. Como estamos usando a origem Netcat, os valores de configuração especificam como ele deve se vincular à rede. Uma fonte semelhante ao netcat abre uma porta especificada e escuta os dados e transforma cada linha de texto em um evento. A expectativa é que os dados fornecidos sejam texto separado por nova linha. Cada linha de texto é transformada em um evento Flume e enviada por meio do canal conectado.
```
type	–	Tipo de componente, no caso o netcat
bind	–	host ou IP que iremos nos conectar
port	–	porta liberada para acesso ao IP especificado acima
```
Na terceira seção definimos a configuração do nosso sink. Esse coletor de criador de logs que é configurado por meio da linha de comandos ou do arquivo de propriedades log4j.
Na quarta seção vamos definir as configurações dos canais.
Nessa seção definimos os canais a serem utilizados, em seguida, adicionam os valores de configuração específicos do tipo. Neste caso, estamos usando o canal de memória e especificamos sua capacidade, mas é não persistente, não há mecanismo de armazenamento externo. Os eventos são armazenados em uma fila na memória com tamanho máximo configurável. É ideal para fluxos que precisam de uma taxa de transferência mais alta e estão preparados para perder os dados testados em caso de falha de um agente. 
As propriedades que definimos são:
```
Type – Tipo de canal, no caso memory
Capacity – Número máximo de eventos armazenados no canal
transactionCapacity – O número máximo de eventos que o canal irá obter de uma fonte ou fornecer a um coletor por transação
```
Por fim, na última seção, configuramos o canal a ser usado para a origem e o coletor, ou seja, vão ser as “amarras” entre um componente e outro.

# Comandos:

No exemplo que iremos simular, utilizaremos apenas dois comandos: um para a inicialização do Flume e outro para a inicialização do netcat.
Primeiro vamos ao Flume, onde temos esse primeiro comando:
```
flume-ng agent --conf <diretório raiz da configuração do Flume> --conf-file <diretório com o arquivo de configuração do agente Flume>/<nome do arquivo de configuração do agente> --name <nome do agente> -Dflume.root.logger=INFO,console
```
Passamos então esse comando em um terminal, dizendo flume-ng agent --conf e o diretório das configurações onde o Flume está instalado, ou seja, dentro da pasta raiz de instalado, existe uma pasta conf com todas as configurações default do Flume, incluindo a do log4j que irei mostrar logo em seguida. 
Depois, passamos **--conf-file** com o diretório e arquivo de configuração do agente Flume, ou seja, onde está nosso projeto. Em seguida **--name** e o nome do agente que definimos no nosso arquivo de configuração de agente Flume, onde no nosso caso definimos como **a1**. Por fim, **-Dflume.root.logger=INFO,console** é a forma que inicializaremos nossa aplicação de logger, ou seja, qual configuração o log4j utilizará para gerenciar essas informações que serão recebidas.
Agora vamos para a inicialização do netcat, que nesse caso funcionaria como nosso Data Source, ou seja, nossa fonte.
Abrimos um outro terminal e entramos em modo administrador. Em seguida digitamos:
```
telnet <host> <porta>
```
No nosso caso, que definimos nas nossas configurações do Flume, localhost 44444.

# Executando o Apache Flume e o Netcat
Abra dois terminais, no primeiro deles iremos iniciar o Flume com o primeiro comando citado na seção anterior.
No segundo terminal, entre como modo administrador e utilize o segundo comando para iniciar o netcat passando localhost 44444.
Envie mensagens através desse segundo terminal e veja as mensagens chegando no primeiro terminal do Apache Flume.

# Referência:
https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html
