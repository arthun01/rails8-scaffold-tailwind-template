# Como Utilizar os Templates Customizados do Scaffold com Tailwind CSS

Este documento documenta todas as alterações feitas para padronizar e modernizar a interface gerada automaticamente pelo comando `rails generate scaffold`, integrando-a com as melhores práticas de UI/UX, Tailwind CSS e ícones SVG.

## O Que Foi Feito

1. **Sobrescrita dos Templates Padrão (`lib/templates/erb/scaffold/`)**
   O Rails permite que você substitua os templates padrão utilizados pelos geradores de código. Nós criamos versões exclusivas para os seguintes arquivos:
   - `index.html.erb`: Visualização em lista. Possui um layout responsivo avançado (tabelas para telas grandes, cards para telas pequenas) e um "Empty State" (estado vazio) amigável caso não existam registros.
   - `show.html.erb`: Página de detalhes do registro, desenhada como um "card" centralizado.
   - `_form.html.erb`: Formulário de criação/edição. Os inputs receberam estilos padronizados focados, anéis de foco coloridos (focus rings) e a área de mensagens de erro foi redesenhada.
   - `new.html.erb` e `edit.html.erb`: Páginas de contêiner para o formulário.

2. **Substituição de Emojis por Ícones Profissionais (Tabler Icons)**
   Removemos todos os emojis dos botões e textos e os substituímos por **SVGs inline** da biblioteca [Tabler Icons](https://github.com/tabler/tabler-icons).
   - O uso de SVGs inline através da sintaxe de bloco no Ruby (`<%= link_to ... do %> <svg>...</svg> <% end %>`) garante que os ícones renderizem com perfeita qualidade, combinem com a cor do texto e pareçam profissionais.
   - Ícones utilizados incluem: `plus` (Adicionar), `eye` (Ver), `pencil` (Editar), `trash` (Deletar), `alert-triangle` (Aviso/Erro), `arrow-left` (Voltar) e `inbox` (Vazio).

3. **Helpers Globais (`app/helpers/application_helper.rb`)**
   Para manter o código limpo, em especial para elementos que se repetem fora do scaffold, garantimos a presença de métodos úteis para gerar Badges (etiquetas) e Botões, como:
   - `badge_success(text)`
   - `badge_warning(text)`
   - `badge_danger(text)`
   *Esses métodos podem ser usados em qualquer View do projeto.*

## Como Utilizar (O Dia a Dia)

Você não precisa mudar **nada** na forma como você trabalha com o Rails. Os templates criados na pasta `lib/templates/` são lidos automaticamente de maneira silenciosa.

### 1. Gerando um Novo Recurso (Scaffold)

Basta rodar o comando padrão do Rails. Por exemplo, se quiser criar um gerenciador de categorias:

```bash
bin/rails generate scaffold Category name:string description:text active:boolean
```

**O que vai acontecer?**
O Rails vai gerar os models, controllers e rotas padrão. Mas na hora de gerar as Views (HTML), ele vai pegar os arquivos da nossa pasta `lib/templates/erb/scaffold/`, preencher com o nome da model e criar as páginas com um visual premium em Tailwind, já com SVGs e responsividade aplicada.

### 2. Atualizando as Migrações

Como de costume, lembre-se de rodar:
```bash
bin/rails db:migrate
```

### 3. Modificando um Scaffold Existente (Regeração)

Se você já tinha gerado um scaffold e agora quer que ele use o novo layout de Tailwind, você pode rodar o comando do scaffold novamente passando a flag `--force` para ele reescrever os arquivos de View:

```bash
bin/rails generate scaffold Post title:string content:text --force
```

*(Lembre-se de ter cuidado com a flag `--force` caso você tenha lógicas customizadas inseridas nos controllers ou models gerados anteriormente, pois ela sobrescreve tudo se você aceitar).*

## Observações Adicionais

- **Tipos de Campos no Formulário:** O template `_form.html.erb` foi construído com inteligência para adaptar as classes CSS com base no tipo de dado do banco de dados (ex: `text_area` ganha uma altura maior, `check_box` ganha o formato correto).
- **Sem Dependências Externas JS:** Todo o layout (incluindo responsividade desktop vs mobile) foi feito exclusivamente com as classes utilitárias do Tailwind CSS. Não há Javascript adicional que você precise manter.
- **Caso precise mudar o layout padrão:** Basta abrir os arquivos correspondentes na pasta `/lib/templates/erb/scaffold/` e editar o HTML/Tailwind. Todos os próximos scaffolds gerados herdarão a sua alteração automaticamente.
