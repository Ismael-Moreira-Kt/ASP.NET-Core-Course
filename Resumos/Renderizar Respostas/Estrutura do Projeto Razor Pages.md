
## O que é o Razor Pages?

O Razor Pages é um modelo de programação do lado do servidor para a criação de interfaces de usuário web com o ASP.NET Core.
A sintaxe do Razor combina HTML e C# para definir a lógica de renderização dinâmica.
É possível usar variáveis e métodos C# na marcação HTML para gerar conteúdo dinâmico em tempo de execução.

Benefícios de usar o Razor são:
- Configuração simples;
- Arquivos organizados por recurso;
- Combina marcação e código C# do lado do servidor.



## Estrutura dos projetos Razor Pages

| Nome                  | Descrição                                                       |
| --------------------- | --------------------------------------------------------------- |
| pages                 | Contém os ficheiros de suporte.                                 |
| wwwroot               | Contém arquivos estáticos como imagens, HTML, CSS e Javascript. |
| <project_name>.csproj | Contém metadados de configuração do projeto.                    |
| Program.cs            | Serve como o ponto de entrada do aplicativo.                    |

Cada página do Razor tem um arquivo .cshtml e um arquivo de classe PageModel .cshtml.cs.
A classe PageModel permite a separação lógica da apresentação de uma página Razor.
A classe define controllers de página para solicitações e encapsula propriedades de dados e lógica para a página Razor.

Razor pages usa a estrutura de diretórios Pages como a convenção para rotear solicitações.

| URL                     | Mapas para a página Razor |
| ----------------------- | ------------------------- |
| www.example.com         | Pages/Index.cshtml        |
| www.example.com/Index   | Pages/Index.cshtml        |
| www.example.com/Privacy | Pages/Privacy.cshtml      |
| www.example.com/Error   | Pages/Error.cshtml        |

Há arquivos que são compartilhados em várias páginas. 
Esses arquivos determinam elementos de layout comuns e importações de página.

| Ficheiro                                     | Descrição                                                                                                      |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| _ViewImports.cshtml                          | Importa namespaces e classes que são usados em várias páginas.                                                 |
| _ViewStart.cshtml                            | Especifica o layout padrão para todas as páginas do Razor.                                                     |
| Pages/share/_Layout.cshtml                   | O layout especificado pelo arquivo _ViewStart.cshtml. Implementa elementos de layout comuns em várias páginas. |
| Pages/share/_ValidationScriptsPartial.cshtml | Fornece funcionalidades de validação para todas as páginas.                                                    |
