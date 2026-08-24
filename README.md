# Rails 8 Scaffold Template · Tailwind CSS

Template pronto para sobrescrever as views padrão do `rails generate scaffold`, trazendo um design premium, responsivo e consistente sem precisar estilizar nada na mão.

---

## O que é este projeto?

Quando você roda `rails generate scaffold Post title:string`, o Rails gera views HTML completamente sem estilização. Este template **substitui esses arquivos automaticamente** com um design moderno baseado em Tailwind CSS — e você continua trabalhando da mesma forma que sempre trabalhou.

### O que vem incluso

| Arquivo | O que resolve |
|---|---|
| `lib/templates/erb/scaffold/index` | Tabela (desktop) + cards (mobile) + empty state |
| `lib/templates/erb/scaffold/show` | Detalhes do registro em layout limpo |
| `lib/templates/erb/scaffold/new` e `edit` | Páginas de formulário estilizadas |
| `lib/templates/erb/scaffold/_form` | Formulário que adapta o campo ao tipo de dado |
| `base_layout/application.html.erb` | Layout com Navbar glassmorphism + fonte Inter |
| `base_layout/_flash.html.erb` | Toasts animados para as mensagens do Rails |
| `auth_templates/sessions/new` | Tela de login |
| `auth_templates/passwords/new` | Tela de "Esqueci a senha" |
| `auth_templates/passwords/edit` | Tela de redefinição de senha |

### O que **não** está incluso

| Feature | Por quê |
|---|---|
| 🔍 Filtros e buscas | Requer `Ransack` + lógica de controller |
| 📄 Paginação | Requer `Kaminari` ou `Pagy` |
| 📊 Dashboard / gráficos | Lógica específica de cada projeto |
| 🛡️ Permissões / Roles | Requer `CanCanCan`, `Pundit` etc. |

---

## Stack

- **Ruby on Rails 8**
- **Tailwind CSS** · via `tailwindcss-rails`
- **Ícones** · SVGs inline da [Tabler Icons](https://tabler-icons.io/)
- **Fonte** · [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts

---

## Screenshots

### Desktop

| Index | Show |
|---|---|
| ![Index Desktop](images/desktop_index.jpg) | ![Show Desktop](images/desktop_show.jpg) |

| New | Edit |
|---|---|
| ![New Desktop](images/desktop_new.jpg) | ![Edit Desktop](images/desktop_edit.jpg) |

### Mobile

<table>
  <tr>
    <td align="center"><b>Index</b></td>
    <td align="center"><b>Show</b></td>
    <td align="center"><b>New</b></td>
    <td align="center"><b>Edit</b></td>
  </tr>
  <tr>
    <td><img src="images/mobile_index.jpg" width="180"/></td>
    <td><img src="images/mobile_show.jpg" width="180"/></td>
    <td><img src="images/mobile_new.jpg" width="180"/></td>
    <td><img src="images/mobile_edit.jpg" width="180"/></td>
  </tr>
</table>

---

## Como Instalar

### 1. Clone este repositório

```bash
git clone https://github.com/arthun01/rails8-scaffold-tailwind-template.git
```

### 2. Copie os templates do scaffold para seu projeto Rails

Copie a pasta `templates/` para dentro de `lib/` no seu projeto:

```
meu-projeto-rails/
└── lib/
    └── templates/
        └── erb/
            └── scaffold/
                ├── _form.html.erb
                ├── edit.html.erb
                ├── index.html.erb
                ├── new.html.erb
                └── show.html.erb
```

A partir daí, todo `rails generate scaffold` já usará o novo visual automaticamente.

### 3. (Opcional) Layout Base

Para que o visual seja completo desde o início, copie o layout base para o seu projeto:

```bash
cp base_layout/application.html.erb app/views/layouts/application.html.erb
cp base_layout/_flash.html.erb app/views/layouts/_flash.html.erb
```

> **Importante:** O `application.html.erb` inclui a fonte Inter via Google Fonts. Troque `AppName` pelo nome da sua aplicação nos dois arquivos.

---

## Como Usar no Dia a Dia

Nada muda no seu fluxo de trabalho. Basta rodar o scaffold normalmente:

```bash
bin/rails generate scaffold Product name:string description:text price:decimal active:boolean
bin/rails db:migrate
```

O Rails vai encontrar os templates em `lib/templates/erb/scaffold/` e gerar as views com o novo design automaticamente.

### Substituindo views de um scaffold já existente

```bash
bin/rails generate scaffold Product name:string description:text --force
```

> ⚠️ A flag `--force` sobrescreve os arquivos existentes. Use com cuidado se já houver lógica customizada nas views ou controllers.

---

## Templates de Autenticação

O Rails 8 tem um gerador nativo de autenticação. Os templates estilizados ficam na pasta `auth_templates/`.

### Telas incluídas

| Tela | Arquivo |
|---|---|
| Login | `auth_templates/sessions/new.html.erb` |
| Esqueci a senha | `auth_templates/passwords/new.html.erb` |
| Redefinição de senha | `auth_templates/passwords/edit.html.erb` |

### Como instalar

```bash
# 1. Rode o gerador nativo
bin/rails generate authentication
bin/rails db:migrate

# 2. Substitua pelas versões estilizadas
cp auth_templates/sessions/new.html.erb app/views/sessions/new.html.erb
cp auth_templates/passwords/new.html.erb app/views/passwords/new.html.erb
cp auth_templates/passwords/edit.html.erb app/views/passwords/edit.html.erb
```

> **Nota:** O Rails 8 Authentication não gera tela de cadastro. Se precisar de uma, crie-a manualmente seguindo o padrão visual dos arquivos em `auth_templates/`.

---

## Helpers Globais Opcionais

Adicione ao seu `app/helpers/application_helper.rb` para ter badges de status disponíveis em qualquer view:

```ruby
module ApplicationHelper
  def badge_success(text)
    content_tag :span, text, class: "inline-flex items-center gap-1 px-2.5 py-1 text-xs font-semibold rounded-full bg-emerald-50 text-emerald-700 ring-1 ring-emerald-200"
  end

  def badge_warning(text)
    content_tag :span, text, class: "inline-flex items-center gap-1 px-2.5 py-1 text-xs font-semibold rounded-full bg-amber-50 text-amber-700 ring-1 ring-amber-200"
  end

  def badge_danger(text)
    content_tag :span, text, class: "inline-flex items-center gap-1 px-2.5 py-1 text-xs font-semibold rounded-full bg-red-50 text-red-700 ring-1 ring-red-200"
  end
end
```

---

## Autor

Feito por **Arthur Ramos Vieira** · [@arthun01](https://github.com/arthun01)

Pull requests e issues são bem-vindos!
