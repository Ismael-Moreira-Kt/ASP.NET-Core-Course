O *ASP.Net Core* oferece suporte a duas abordagens para criar *API's*:



### API's baseadas em Controladores: 

É a abordagem mais tradicional para desenvolver *API's*. 
Nesta abordagem, cada **endpoint** é mapeado para uma classe de **controller** específica. 
O *controller* manipula a solicitação e executa a lógica para retornar uma resposta.


```csharp
	[ApiController]
	[Route("[controller]")]
	public class WeatherForecastController : ControllerBase
```

Um classe *controller* deve derivar de **ControllerBase** e não de **Controller**. 

*Controller* é derivado de *ControllerBase* e oferece suporte para exibições.
Por outras palavras, *Controller* serve para manipular páginas web.

Por sua vez, *ControllerBase* fornece muitas propriedades e métodos para lidar com solicitações HTTP.

```csharp
	[HttpPost]
	[ProducesResponseType(StatusCodes.Status201Created)]
	[ProducesResponseType(StatusCodes.Status400BadRequest)]
	public ActionResult<Pet> Create(Pet pet) {
		pet.Id = _petsInMemoryStore.Any() ? __petsInMemoryStore.Max(p => p.Id) + 1 : 1;
		_petsInMemoryStore.Add(pet);

		return CreatedAtAction(nameof(GetById), new { id = pet.Id }, pet);
	}
```

Neste exemplo, **CreatedAction** retorna um código de status 201.



### API Mínima

É uma abordagem simplificada para criar *API's* de HTTP rápidas.
É possível criar *endpoints* REST funcionais com o minimo de código e configuração possíveis.

```csharp
	var app = WebApplication.Create(args);
	app.MapGet("/", () => "Hello World!");
	app.Run();
```

O código cria uma *API* que ao ser chamada na raiz **/** retorna o texto "Hello World!".

```csharp
	var builder = WebApplication.CreateBuilder(args);

	var app = builder.Build();
	app.MapGet("/users/{userId}/books/{bookId}", (int userId, int bookId) => $"The user id is {userId} and book id is {bookId}");
	app.Run();
```

A maioria das *API* aceita parâmetros como parte da rota.
*API's* minimas dão suporte a configuração e personalização de rotas para lidar com rotas complexas, controlar o conteúdo das respostas e aplicar regras de autorização.