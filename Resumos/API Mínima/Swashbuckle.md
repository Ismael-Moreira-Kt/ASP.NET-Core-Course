**Swashbuckle** é um pacote de geração automática de documentação do **Swagger** para projetos **ASP.Net Web API**.



## O que é o Swagger?

O *Swagger* é uma ferramenta que visa ajudar os desenvolvedores para projetar, criar, documentar e consumir *API's* REST. 



## O que é o Swashbuckle 

Com o *Swashbuckle*, os desenvolvedores podem adicionar documentação do *Swagger* dos projetos  API.
Ele usa os *endpoints*, os parâmetros e as respostas da *API* para gerar um arquivo **JSON** do *Swagger*, que pode ser usado para gerar a documentação interativa da *API* e *SDKs* de clientes.



## Componentes do Swashbuckle

- **Swashbuckle.AspNetCore.Swagger:** Um modelo de objeto *Swagger* e **Middlware** para exportar objetos **SwaggerDocument** como *endpoints*.
- **Swashbuckle.AspNetCore.SwaggerGen:** Um gerador *Swagger* que cria objetos *SwaggerDocument* diretamente diretamente das rotas, controllers e models. Ele pode ser combinado com *Middleware* de *endpoint* do *Swagger* para exportar automaticamente o *JSON* do *Swagger*.
- **Swashbuckle.AspNetCore.SwaggerUI:** Uma versão incorporada da interface de usuário do *Swagger*. Ele inclui os agentes de teste para os métodos públicos.

Para adicionar o Swashbuckle ao projeto, é usado o comando dotnet add.

```bash
	dotnet add <name>.csproj package Swashbuckle.AspNetCore -v 6.5.0
```



## Configurar o Middleware do Swagger

Para adicionar o gerador do *Swagger* a *API*, pode-se adicionar as seguintes linhas:

```csharp
	builder.Services.AddEndpointsApiExplorer();
	builder.Services,AddSwaggerGen();
```

Para servir o documento *JSON* gerado, a interface do usuário do *Swagger*, podemos adicionar: 

```csharp
	app.UseSwagger();

	if(app.Environment.IsDevelopment()) {
		app.app.UseSwaggerUI();
	}
```

O endpoint comum para o *Swagger* é: http:hostname:port/swagger.



## Personalizar a Documentação do Swagger

O *Swagger* oferece algumas opções para documentar o projeto e personalizar a interface.
A ação de configuração passada no método **AddSwaggerGen** pode incluir informações adicionais por meio da classe **OpenApiInfo**.

```csharp
	using Microsoft.OpenApi.Models;

	builder.Services.AddSwaggerGen(options => {
		options.SwaggerDoc("v1", new OpenApiInfo {
			Version = "V1",
			Title = "Fruit API",
			Description = "API for managing a list of fruit their stock status.",
			TermsOfService = new Uri("https://example.com/terms")
		});
	});
```

Esse código mostra como adicionar informações a serem exibidas na documentação da *API*.
É possível adicionar títulos mais descritivos nas diferentes funções ao usar a opção **.WithTags**.

```csharp
	app.MapPost("/fruitlist", async (Fruit fruit, FruitDb db) => {
		db.Fruits.Add(fruit);
		await db.SaveChangesAsync();

		return Results.Created($"/fruitlist/{fruit.Id}", fruit);
	}).WithTags("Add fruit to list");
```

Esse código adiciona "Add fruit to list" como um título de um mapeamento POST.