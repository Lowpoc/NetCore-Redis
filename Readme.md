![N|Lowpoc](https://i.ibb.co/MGshCk7/Black-Technology-Linked-In.png)

# Antes de iniciar nossa jornada de conhecimento, mostrarei como ficar� dividido esse cont�udo:

| Steps
| ------
| Conceitos
| Expondo ferramentas envolvidas 
| Criando um cen�rio 
| Resolvendo o cen�rio 
| Refer�ncias
| Minhas Redes sociais 

# Conceitos
    - O que � redis?
     R: � um projeto open source que tem como objetivo fornecer uma storage para armazemento de dados que pode ser utilizado como cache ou message broker. Nele podemos armazenar em geral: Strings, conjunto de dados(Array,List) e etc.
    - O que � .NetCore?
     R: � um framework desenvolvido pela Microsoft que tem como base linguist�sca: C#, F# por exemplo, al�m de poder ser utilizados nos sistemas operacionais: Windows, MacOS e distribui��es Linux.
    - Qual o papel de um "CACHE"?
     R: Visa armazenar um conjunto de informa��es que s�o frequentementes utilizados afim de recuper�-los de forma mais r�pida.
    - O que o Docker vai ajudar?
     R: A fornecer um conjunto de produtos de plataforma como servi�os de forma isoladas uns dos outros, trazendo transpar�ncia e facilidade na hora de rodar aplica��o. 
     - Como funciona o Cache distribu�do?
     R: Ele � armezanado em um conjunto de servidores(clusters), na qual tem como objetivo principal de melhorar o desempenho, escalabilidade e resili�ncia das informa��es.

# Expondo ferramentas envolvidas
   - [Instalar .Net Core 3.1 SDK](https://dotnet.microsoft.com/download)
   - [Instalar Docker [Windows, MacOs, Linux]](https://docs.docker.com/get-docker/)

# Criando um Cen�rio

> Imaginemos que temos uma aplica��o que tem  como finalidade monitorar o tempo da sua localidade
> e ele possui uma api rest construida com .Net Core que est� passando  por grandes instabiliadades devido a 
> demora de receber os conteudos de um enpoint X.
> Esse mesmo endpoint � muito solicitado, conhecido como "C�DIGO QUENTE", pois a cada 10 ms(mil�simo de segundo) � feito um request, tornando o processo de monitoria inv�lido. Ent�o a equipe chamou o grande "Developer Marcus" pra resolver HAHAH e assim foi feito!
> Vamos analisar o seguintes passos para entender como ele resolveu?

# Resolvendo o Cen�rio
 - Primeira etapa:
        -  Vamos realizar uma bateria de testes para entender como est� a sa�de desse nosso endpoint , para isso iremos utilizar uma ferramenta muito boa, por�m pouco conhecida que � o [Runner do Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/), ela � muito boa para realizarmos o teste de requisi��o em massa no tempo informado l� em cima que � de "10ms".
        -  J� ia esquecendo antes de rodar o runner, vamos configurar nosso endpoint l� no Postman.
        ![](https://i.ibb.co/wSRGG9J/Endpoint-Postman.png)
        - Analisando a imagem acima podemos perceber que nosso endpoint � [https://localhost:32784/weatherforecast] e alguns scripts de testes que fazem alguns checkes que ser�o explicados nos pr�ximos passos.
        - Vamos startar a aplica��o utilizando o comando `` docker-compose up -d `` e magicamente utilizando o docker e docker compose subir� a aplica��o .NetCore + Redis
        - Rode o ```comando docker container ls```, ele ir� listar os servi�os/produtos rodando dentro dos containers. Deve aparecer algo parecido com isso: 
        ![](https://i.ibb.co/KrHTH8d/Ls.png)
        - Como falei anteriormente, tinha criado tr�s checks
    - Response Time: Valida o tempo aceitavel pelo endpoint
    - Body: Checka se est� retornando conte�do no corpo do response.
    - Status: Checka se est� retornando 200.
    - Vamos abrir nosso runner e preencher os campos iterations com 100 e Delay 10, ou seja ocorrer� 1 requisi��o no intervalo de 10ms at� chegar 100.
    - Resultado dessa primeira etapa foi: das 100 solicita��es foram reprovadas 100% no check de "response time", porque a taxa de retorno � > 100ms, conforme escrito no teste que � a taxa aceit�vel pela empresa. ![](https://i.ibb.co/MN7rYJD/Check-Response.png)
    
 - Etapa 2:
     - Bom agora que sabemos que o ofensor de fato � a nossa lat�ncia, vamos aplicar nosso cache distribuido com redis.
     - Para isso criei algunas classes:

| Nome | Funcionalidade | Endere�o
| ------ |  ------ | ------ |  
| ResponseCacheRedis | Respons�vel por abstrair as funcionalidades de salvar e resgatar o cache | [link]()
| IResponseCacheRedis  | Interface que define os contratos a serem respeitados pelo ResponseCacheRedis | [link]()
| Cached | Um atributo/filter de action ass�ncrono, na qual ser� respons�vel por interceptar o request e identificar se tem  algo em cache ou n�o, assim reduzindo  drasticamente o tempo de resposta. | [link]()
| Redis |  Respons�vel por aplicar as configura��es base do redis: Connection String, Interface, Servi�o. | [link]()

- Etapa 3:
    - Agora basta colocar dentro da nossa controller nosso atributo chamado "Cached" especial para ver a m�gica acontecer!
    - ![](https://i.ibb.co/QvtZ5xr/Controller.png)
    - Vamos ver o o resultado:
    ![](https://i.ibb.co/jbcwgxX/Capturar.png)
    - WOW o resultado foi fant�stico, dos nossos 100 checks de response time , tivemos apenas 7 erros ou seja uma melhoria de 93% de comparado com o resultado anterior. Agora nosso servi�o de monitoramento poder� trabalhar de forma muita mais tranquila, pois o tempo de resposa saiu de 1000ms media para 35ms.

# Opini�o
   - O cache � uma ferramenta muita poderosa e que deve ser utilizada de forma estr�tegica nas nossas aplica��es, afim de resolver alguns cen�rios de perfomace.
   - Nem todo cen�rio precisa de cache.
   - Existem outros m�todos para melhorar perfomace como por exemplo: Aloca��o no GC[Garbage Collection].
# Refer�ncias
   - [Redis](https://redis.io/)
   - [.NetCore](https://docs.microsoft.com/pt-br/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1)
   - [Cache Distribuido com .NetCore](https://docs.microsoft.com/pt-br/aspnet/core/performance/caching/distributed?view=aspnetcore-3.1)
   - [Docker](https://www.docker.com/)

# Minhas Redes Sociais

   - [Linkedin](https://www.linkedin.com/in/marcus-vinicius-santana-silva-0a1602117/)
   - [Instagram](@olasoumarcus)
   - [Youtube](https://www.youtube.com/channel/UCwNHLO-2BAaIfuMxK_OERxw)
 

Copyright (c) 2020-present, [Marcus V.S. Silva [Lowpoc]](https://github.com/Lowpoc)