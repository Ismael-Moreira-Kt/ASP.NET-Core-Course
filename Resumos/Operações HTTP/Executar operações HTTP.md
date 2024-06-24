```csharp
	public class FruitModel {
		public int id { get; set; }
		public string? name { get; set; }
		public bool instock { get; set; }
	}
```

Este código representa um modelo de dados.



## Registar o IHttpClientFactory

Para adicionar o **IHttpClientFactory** num aplicativo, podemos registrar o **AddHtppClient** no arquivo Program.cs.

```csharp
	var builder = WebApplication.CreateBuilder(args);

	builder.Services.AddRazorPages();
	builder.Services.AddHttpClient("FruitAPI", httpClient => {
		httpClient.BaseAddress = new Uri("https://www.example.com);
	});

	var app = builder.Build();
```



## Identificar requisitos de operação

Antes de executar, é preciso identificar o que a API espera:
- **Endpoints:** Identificar o endpoint da operação para ajustar a URL.
- **Requisitos de Dados:** Identificar se a API retorna ou espera um array ou uma única parte de dados.

Por exemplo, uma operação GET pode retornar uma matriz se o endpoint retornar todos os dados.
Uma operação Get também pode retornar um único dado.



## Métodos Handler em Razor Pages

Os métodos Handler são métodos que manipulam solicitações e eventos HTTP.
Eles são usados para executar ações em resposta á entrada do usuário ou outros eventos.
Os métodos Handler seguem a convenção de nomenclatura da palavra "On" em conjunto do verbo da solicitação HTTP.

| Nome          | Descrição                                                                                          |
| ------------- | -------------------------------------------------------------------------------------------------- |
| OnGet         | Este método é chamado quando a página é solicitada através do método HTTP GET.                     |
| OnPost        | Esse método é chamado quando é enviado conteúdo através do método HTTP POST.                       |
| OnGetAsync    | Este método é chamado de forma assíncrona quando a página é solicitada através do método HTTP GET. |
| OnGetHandler  | Este método é chamado quando um manipulador específico é chamado com o método HTTP GET.            |
| OnPostHandler | Este método é chamado quando um manipulador específico é solicitado com o método HTTP POST.        |

Também existem outros métodos de Handler que podem ser usados para manipular solicitações e eventos HTTP.
Também é possível definir métodos Handler personalizados.



## Operações GET

Uma operação GET é usada para recuperar dados de um determinado recurso.
Para executar uma operação GET, dado um HttpClient e uma URI, podemos usar o método HttpClient.GetAsync.
Para criar uma tabela na pagina inicial de um aplicativo Razor Page, é necessário:
- Usar a injeção de dependências para adicionar o IHttpClientFactory ao modelo da página.
- Usar o atributo [BehindProperty] para vincular dados do formulário de páginas ao modelo de dados.
- Criar uma instância do HttpClient.
- Executar a operação Get e desserializar os resultados no modelo de dados.

```csharp
	namespace FruitWebApp.Pages {
		public class IndexModel : PageModel {
			private readonly IHttpClientFactory _httpClientFactory;

			public IndexModel(IHttpClientFactory httpClientFactory) {
				_httpClientFactory = httpClientFactory;
			}

			[BindProperty]
			public IEnumerable<FruitModel> FruitModels { get; set; }

			public async Task OnGet() {
				var httpClient = _httpClientFactory.CreateClient("FruitAPI");

				using HttpResponseMessage response = await httpClient,GetAsync("");

				if (response.IsSuccessStatusCode) {
					using var contentStream = await response.Content.ReadAsStreamAsync();
					FruitModels = await JsonSerializer.DeserializeAsync<IEnumerable<FruitModel>>(contentStream);
				}
			}
		}
	}
```

O código a cima é um exemplo de implementação de um método GET.



## Operações POST

Uma operação POST é usada para adicionar dados a um recurso.
Para executar uma operação GET, dado um HttpClient e uma URI, podemos usar o método HttpClient.PostAsync.
Para adicionar itens aos dados da página inicial, é necessário:
- Injeção de dependência para adicionar o IHttpClientFactory ao modelo da página.
- Usar o atributo [BindProperty] para vincular os dados do formulário de páginas ás propriedades do modelo de dados.
- Serialize os dados que deseja adicionar com o método JsonSerializer.Serialize.
- Criar uma instância do HttpClient.
- Executar uma Operação POST.

```csharp
	namespace FruitWebApp.Pages {
		public class IndexModel : PageModel {
			private readonly IHttpClientFactory _httpClientFactory;

			public AddModel(IHttpClientFactory httpClientFactory) => _httpClientFactory = httpClientFactory;

			[BindProperty]
			public FruitModel FruitModels { get; set; }

			public async Task<IActionResult> OnPost() {
				var jsonContent = new StringContent(JsonSerializer.Serialize(FruitModels), Encoding.UTF8, "application/json");

				var httpClient = _httpClientFactory.CreateClient("FruitAPI");

				using HttpResponseMessage response = await httpClient.PostAsync("", jsonContent);

				if (response.IsSuccessStatusCode) {
					return RedirectToPage("Index");
				}

				return Page();
			}
		}
	}
```



## Outras Operações REST
As outras operações como PUT e DELETE seguem o mesmo modelo.

| Verbo  | Método                 | Definição                                                                 |
| ------ | ---------------------- | ------------------------------------------------------------------------- |
| GET    | HttpClient.GetAsync    | Recupera um recurso.                                                      |
| POST   | HttpClient.PostAsync   | Cria um novo recurso.                                                     |
| PUT    | HttpClient.PutAsync    | Atualiza um recurso existente ou cria um novo recurso se ele não existir. |
| DELETE | HttpClient.DeleteAsync | Exclui um recurso.                                                        |
| PATCH  | HttpClient.PatchAsync  | Atualiza parcialmente um recurso existente.                               |
