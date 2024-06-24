O **protocolo de transferência de hipertexto** é usado para solicitar recursos de servidores.
Muitos tipos de recursos estão disponíveis na web e o *HTTP* define um conjunto de métodos para solicitar e acessar esses recursos.
No .Net Core, esses métodos são feitas por meio de uma instância do **HttpClient**.
Existem duas formas de implementar o *HttpClient*:
- **Clientes de longa duração:** Crie uma instância estática usando a classe *HttpClient* e defina **PooledConnectionLifetime**.
- **Clientes de curta duração:** Use clientes criados por **IHttpClientFactory**.



## Implementação com a Classe HttpClient

A classe **System.Net.Http.HttpClient** envia solicitações *HTTP* e recebe respostas *HTTP* de recursos identificados por uma **URL**.
Uma instância *HttpClient* é uma coleção de configurações aplicadas a todas as solicitações executadas por essa instância e cada uma das instâncias usa o seu próprio pool de conexões. 
Os pool isolam as solicitações de uma instância das outras instâncias. 

*HttpClient* só resolve entradas **DNS** quando uma conexão é criada.
Ele não controla as durações de **TTL** (tempo de vida) especificadas pelo servidor *DNS*.
Se as entradas *DNS* forem alteradas regularmente, o cliente não será atualizado.
Para resolver esse problema, é possível limitar o *TTL* da conexão definindo a propriedade **PooledConnectionLifetime** para que a pesquisa de *DNS* seja repetida quando a conexão for substituída.

```csharp
	var handler = new SocketsHttpHandler {
		PooledConnectionLifetime = TimeSpan.FromMinutes(15)
	};
	var sharedClient = new HtppClient(handler);
```

Nesse código, *HttpClient* está configurado para reutilizar conexões por 15 minutos.
Após o **TimeSpan** especificado no *PooledConnectionLifetime*, a conexão é fechada e uma nova é criada.



## Implementação com IHttpClientFactory

O **IHttpClientFactory** serve como uma abstração que pode criar instâncias HttpClient com configurações personalizadas.
Quando se chama qualquer um dos métodos da Extensão [AddHttpClient](https://learn.microsoft.com/pt-pt/dotnet/api/microsoft.extensions.dependencyinjection.httpclientfactoryservicecollectionextensions.addhttpclient?view=net-8.0), estamos a adicionar serviços *IHttpClientFactory* relacionados ao **IServiceCollection**.

Alguns dos benefícios são:
- Expor classes *HttpClient* como injeção de dependências.
- Fornece um local central para nomear e configurar instâncias *HttpClient*.
- Codifica o conceito de middleware através da delegação de manipuladores *HttpClient*.
- Fornece métodos de middlware baseados em **Polly** para tirar proveito da delegação de manipuladores no *HttpClient*.
- Gerencia o cache e o *TTL* das instâncias HttpClientHandler. O gerenciamento automático evita problemas comuns de DNS que ocorrem quando o gerenciamento é feito manualmente.
- Adiciona uma experiência de registro configurável para todas as solicitações enviadas através de clientes criados pela factory.