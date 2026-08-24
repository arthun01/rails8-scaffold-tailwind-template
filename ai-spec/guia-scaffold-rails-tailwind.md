# 🎨 Guia Completo: Personalizar Scaffolds Rails 8 com Tailwind CSS

## Visão Geral

Este guia fornece templates prontos para customizar os scaffolds do Rails com uma paleta de cores moderna, design mobile-first responsivo e melhor experiência de usuário.

**Paleta de Cores Primária:**
- Azul Primário: `#3B82F6` (blue-500)
- Azul Escuro: `#1E40AF` (blue-800)
- Verde Sucesso: `#10B981` (emerald-500)
- Laranja Aviso: `#F59E0B` (amber-500)
- Vermelho Erro: `#EF4444` (red-500)
- Cinza Neutro: `#6B7280` (gray-500)
- Branco: `#FFFFFF`
- Fundo: `#F9FAFB` (gray-50)

---

## 📁 Passo 1: Estrutura de Diretórios

Crie a seguinte estrutura no seu projeto Rails:

```bash
mkdir -p lib/templates/erb/scaffold
```

**Arquivos a criar:**
```
lib/templates/erb/scaffold/
├── index.html.erb
├── show.html.erb
├── new.html.erb
├── edit.html.erb
└── _form.html.erb
```

---

## 📋 Passo 2: Arquivo `index.html.erb` (Listagem)

**Caminho:** `lib/templates/erb/scaffold/index.html.erb`

```erb
<div class="min-h-screen bg-gray-50 py-6 px-4 sm:px-6 lg:px-8">
  <!-- Cabeçalho -->
  <div class="max-w-7xl mx-auto">
    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-8">
      <div>
        <h1 class="text-3xl sm:text-4xl font-bold text-gray-900">
          <%= @<%= plural_table_name %>.model_name.human(count: 2) %>
        </h1>
        <p class="text-gray-600 text-sm mt-1">Gerencie todos os registros de <%= plural_table_name %></p>
      </div>
      <div class="w-full sm:w-auto">
        <%= link_to "➕ Novo(a)", new_<%= singular_table_name %>_path, 
            class: "w-full sm:w-auto block text-center bg-blue-600 hover:bg-blue-700 active:bg-blue-800 text-white font-semibold py-3 px-6 rounded-lg transition duration-200 shadow-md hover:shadow-lg" %>
      </div>
    </div>

    <!-- Filtros (Desktop) -->
    <div class="hidden md:flex gap-3 mb-6">
      <div class="flex-1">
        <input type="text" placeholder="Pesquisar..." 
               class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent">
      </div>
    </div>

    <!-- Tabela Responsiva -->
    <div class="bg-white rounded-lg shadow-md overflow-hidden">
      <!-- Desktop View -->
      <div class="hidden md:block overflow-x-auto">
        <table class="w-full">
          <thead>
            <tr class="bg-gray-100 border-b border-gray-200">
              <% attributes.reject { |attr| attr.name == 'id' || attr.attachment? }.each do |attribute| %>
                <th class="px-6 py-4 text-left text-xs font-bold text-gray-700 uppercase tracking-wider">
                  <%= attribute.human_name %>
                </th>
              <% end %>
              <th class="px-6 py-4 text-left text-xs font-bold text-gray-700 uppercase tracking-wider">
                Ações
              </th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200">
            <% @<%= plural_table_name %>.each do |<%= singular_table_name %>| %>
              <tr class="hover:bg-blue-50 transition">
                <% attributes.reject { |attr| attr.name == 'id' || attr.attachment? }.first(4).each do |attribute| %>
                  <td class="px-6 py-4 text-sm text-gray-900">
                    <% if attribute.field_type == :boolean %>
                      <span class="inline-block px-3 py-1 text-xs font-semibold rounded-full 
                        <% if <%= singular_table_name %>.<%= attribute.name %> %>
                          bg-emerald-100 text-emerald-800
                        <% else %>
                          bg-gray-100 text-gray-600
                        <% end %>">
                        <%= <%= singular_table_name %>.<%= attribute.name %> ? '✓ Sim' : '✗ Não' %>
                      </span>
                    <% elsif attribute.field_type == :text %>
                      <%= truncate(<%= singular_table_name %>.<%= attribute.name %>, length: 50) %>
                    <% else %>
                      <%= <%= singular_table_name %>.<%= attribute.name %> %>
                    <% end %>
                  </td>
                <% end %>
                <td class="px-6 py-4 text-sm font-medium">
                  <div class="flex gap-2">
                    <%= link_to "👁️ Ver", <%= singular_table_name %>, 
                        class: "text-blue-600 hover:text-blue-800 font-semibold transition" %>
                    <%= link_to "✏️ Editar", edit_<%= singular_table_name %>_path(<%= singular_table_name %>), 
                        class: "text-amber-600 hover:text-amber-800 font-semibold transition" %>
                    <%= link_to "🗑️", <%= singular_table_name %>, method: :delete, 
                        data: { confirm: "Tem certeza?" }, 
                        class: "text-red-600 hover:text-red-800 font-semibold transition" %>
                  </div>
                </td>
              </tr>
            <% end %>
          </tbody>
        </table>
      </div>

      <!-- Mobile View: Cards -->
      <div class="md:hidden divide-y divide-gray-200">
        <% @<%= plural_table_name %>.each do |<%= singular_table_name %>| %>
          <div class="p-4 hover:bg-blue-50 transition">
            <div class="space-y-3">
              <% attributes.reject { |attr| attr.name == 'id' || attr.attachment? }.first(3).each do |attribute| %>
                <div>
                  <p class="text-xs font-semibold text-gray-600 uppercase">
                    <%= attribute.human_name %>
                  </p>
                  <p class="text-sm text-gray-900 mt-1">
                    <% if attribute.field_type == :boolean %>
                      <span class="inline-block px-2 py-1 text-xs font-semibold rounded 
                        <% if <%= singular_table_name %>.<%= attribute.name %> %>
                          bg-emerald-100 text-emerald-800
                        <% else %>
                          bg-gray-100 text-gray-600
                        <% end %>">
                        <%= <%= singular_table_name %>.<%= attribute.name %> ? '✓ Sim' : '✗ Não' %>
                      </span>
                    <% elsif attribute.field_type == :text %>
                      <%= truncate(<%= singular_table_name %>.<%= attribute.name %>, length: 40) %>
                    <% else %>
                      <%= <%= singular_table_name %>.<%= attribute.name %> %>
                    <% end %>
                  </p>
                </div>
              <% end %>
            </div>
            <div class="flex gap-2 mt-4 pt-4 border-t border-gray-100">
              <%= link_to "Ver", <%= singular_table_name %>, 
                  class: "flex-1 text-center bg-blue-600 text-white py-2 rounded font-semibold text-sm hover:bg-blue-700 transition" %>
              <%= link_to "Editar", edit_<%= singular_table_name %>_path(<%= singular_table_name %>), 
                  class: "flex-1 text-center bg-amber-600 text-white py-2 rounded font-semibold text-sm hover:bg-amber-700 transition" %>
            </div>
          </div>
        <% end %>
      </div>

      <!-- Mensagem de vazio -->
      <% if @<%= plural_table_name %>.empty? %>
        <div class="text-center py-12">
          <div class="text-gray-400 text-5xl mb-4">📭</div>
          <h3 class="text-lg font-semibold text-gray-900 mb-2">Nenhum(a) <%= singular_table_name %> encontrado(a)</h3>
          <p class="text-gray-600 mb-6">Comece criando um novo registro</p>
          <%= link_to "Criar Novo(a)", new_<%= singular_table_name %>_path, 
              class: "inline-block bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition" %>
        </div>
      <% end %>
    </div>
  </div>
</div>
```

---

## 📄 Passo 3: Arquivo `_form.html.erb` (Formulário)

**Caminho:** `lib/templates/erb/scaffold/_form.html.erb`

```erb
<%= form_with(model: @<%= singular_table_name %>, local: true, class: "space-y-6") do |form| %>
  <!-- Erros -->
  <% if @<%= singular_table_name %>.errors.any? %>
    <div class="bg-red-50 border-l-4 border-red-500 p-4 rounded">
      <div class="flex">
        <div class="text-red-500 text-lg mr-3">⚠️</div>
        <div>
          <h3 class="font-bold text-red-800 mb-2">
            <%= pluralize(@<%= singular_table_name %>.errors.count, "erro") %> encontrado(s):
          </h3>
          <ul class="text-sm text-red-700 space-y-1">
            <% @<%= singular_table_name %>.errors.full_messages.each do |message| %>
              <li>• <%= message %></li>
            <% end %>
          </ul>
        </div>
      </div>
    </div>
  <% end %>

  <!-- Campos do Formulário -->
  <% attributes.reject { |attr| attr.name == 'id' || attr.attachment? }.each do |attribute| %>
    <div>
      <%= form.label :<%= attribute.name %>, class: "block text-sm font-bold text-gray-700 mb-2" %>
      
      <% if attribute.field_type == :text_area %>
        <%= form.text_area :<%= attribute.name %>, 
            rows: 5,
            placeholder: "Digite aqui...",
            class: "w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none" %>
      
      <% elsif attribute.field_type == :boolean %>
        <div class="flex items-center">
          <%= form.check_box :<%= attribute.name %>, { class: "h-5 w-5 text-blue-600 rounded focus:ring-blue-500 cursor-pointer" }, "true", "false" %>
          <label for="<%= singular_table_name %>_<%= attribute.name %>" class="ml-2 block text-sm text-gray-700 cursor-pointer">
            <%= attribute.human_name %>
          </label>
        </div>
      
      <% elsif attribute.field_type == :datetime_select %>
        <%= form.datetime_select :<%= attribute.name %>, 
            class: "w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent" %>
      
      <% elsif attribute.field_type == :date_select %>
        <%= form.date_field :<%= attribute.name %>, 
            class: "w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent" %>
      
      <% elsif attribute.field_type == :time_select %>
        <%= form.time_field :<%= attribute.name %>, 
            class: "w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent" %>
      
      <% elsif attribute.reference? %>
        <%= form.collection_select :<%= attribute.name %>_id, 
            <%= attribute.name.classify %>.all, 
            :id, 
            :to_s,
            { prompt: "Selecione..." },
            class: "w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent" %>
      
      <% else %>
        <%= form.<%= attribute.field_type %> :<%= attribute.name %>,
            placeholder: "Digite aqui...",
            class: "w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent" %>
      <% end %>
    </div>
  <% end %>

  <!-- Botões -->
  <div class="flex flex-col sm:flex-row gap-3 pt-6 border-t border-gray-200">
    <%= form.submit @<%= singular_table_name %>.new_record? ? "Criar" : "Atualizar",
        class: "flex-1 bg-blue-600 hover:bg-blue-700 active:bg-blue-800 text-white font-bold py-3 px-4 rounded-lg transition duration-200 shadow-md hover:shadow-lg" %>
    
    <%= link_to "Cancelar", @<%= plural_table_name %>,
        class: "flex-1 text-center bg-gray-300 hover:bg-gray-400 text-gray-900 font-bold py-3 px-4 rounded-lg transition duration-200" %>
  </div>
<% end %>
```

---

## 📖 Passo 4: Arquivo `show.html.erb` (Detalhes)

**Caminho:** `lib/templates/erb/scaffold/show.html.erb`

```erb
<div class="min-h-screen bg-gray-50 py-6 px-4 sm:px-6 lg:px-8">
  <div class="max-w-4xl mx-auto">
    <!-- Cabeçalho -->
    <div class="mb-8">
      <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
        <div>
          <h1 class="text-3xl sm:text-4xl font-bold text-gray-900">
            <%= @<%= singular_table_name %>.to_s %>
          </h1>
          <p class="text-gray-600 text-sm mt-1">Detalhes do(a) registro</p>
        </div>
        <div class="w-full sm:w-auto flex flex-col sm:flex-row gap-2">
          <%= link_to "✏️ Editar", edit_<%= singular_table_name %>_path(@<%= singular_table_name %>), 
              class: "text-center bg-amber-600 hover:bg-amber-700 text-white font-bold py-2 px-4 rounded-lg transition" %>
          <%= link_to "🗑️ Deletar", @<%= singular_table_name %>, method: :delete, data: { confirm: "Tem certeza?" },
              class: "text-center bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-lg transition" %>
        </div>
      </div>
    </div>

    <!-- Card de Detalhes -->
    <div class="bg-white rounded-lg shadow-md p-6 sm:p-8 space-y-6">
      <% attributes.reject { |attr| attr.name == 'id' || attr.attachment? }.each do |attribute| %>
        <div class="pb-6 border-b border-gray-200 last:border-b-0">
          <p class="text-xs font-bold text-gray-600 uppercase tracking-wider mb-2">
            <%= attribute.human_name %>
          </p>
          <p class="text-lg text-gray-900 break-words">
            <% if attribute.field_type == :boolean %>
              <span class="inline-block px-3 py-1 text-sm font-semibold rounded-full 
                <% if @<%= singular_table_name %>.<%= attribute.name %> %>
                  bg-emerald-100 text-emerald-800
                <% else %>
                  bg-gray-100 text-gray-600
                <% end %>">
                <%= @<%= singular_table_name %>.<%= attribute.name %> ? '✓ Sim' : '✗ Não' %>
              </span>
            <% elsif attribute.field_type == :text_area %>
              <div class="bg-gray-50 p-4 rounded whitespace-pre-wrap">
                <%= @<%= singular_table_name %>.<%= attribute.name %> %>
              </div>
            <% else %>
              <%= @<%= singular_table_name %>.<%= attribute.name %> %>
            <% end %>
          </p>
        </div>
      <% end %>

      <!-- Metadados -->
      <div class="pt-6 border-t border-gray-200 grid grid-cols-1 sm:grid-cols-2 gap-4">
        <div>
          <p class="text-xs font-bold text-gray-600 uppercase">Criado em</p>
          <p class="text-sm text-gray-900"><%= l(@<%= singular_table_name %>.created_at) %></p>
        </div>
        <div>
          <p class="text-xs font-bold text-gray-600 uppercase">Atualizado em</p>
          <p class="text-sm text-gray-900"><%= l(@<%= singular_table_name %>.updated_at) %></p>
        </div>
      </div>
    </div>

    <!-- Rodapé de Navegação -->
    <div class="mt-8 flex flex-col sm:flex-row gap-3">
      <%= link_to "⬅️ Voltar", @<%= plural_table_name %>,
          class: "flex-1 text-center bg-gray-600 hover:bg-gray-700 text-white font-bold py-3 px-4 rounded-lg transition" %>
    </div>
  </div>
</div>
```

---

## ✨ Passo 5: Arquivo `new.html.erb` (Criar)

**Caminho:** `lib/templates/erb/scaffold/new.html.erb`

```erb
<div class="min-h-screen bg-gray-50 py-6 px-4 sm:px-6 lg:px-8">
  <div class="max-w-2xl mx-auto">
    <div class="mb-8">
      <h1 class="text-3xl sm:text-4xl font-bold text-gray-900">
        Criar novo(a) <%= singular_table_name.humanize.downcase %>
      </h1>
      <p class="text-gray-600 text-sm mt-2">Preencha os campos abaixo para criar um novo registro</p>
    </div>

    <div class="bg-white rounded-lg shadow-md p-6 sm:p-8">
      <%= render "form", <%= singular_table_name %>: @<%= singular_table_name %> %>
    </div>
  </div>
</div>
```

---

## 🔄 Passo 6: Arquivo `edit.html.erb` (Editar)

**Caminho:** `lib/templates/erb/scaffold/edit.html.erb`

```erb
<div class="min-h-screen bg-gray-50 py-6 px-4 sm:px-6 lg:px-8">
  <div class="max-w-2xl mx-auto">
    <div class="mb-8">
      <h1 class="text-3xl sm:text-4xl font-bold text-gray-900">
        Editar <%= singular_table_name.humanize.downcase %>
      </h1>
      <p class="text-gray-600 text-sm mt-2">Atualize os campos necessários</p>
    </div>

    <div class="bg-white rounded-lg shadow-md p-6 sm:p-8">
      <%= render "form", <%= singular_table_name %>: @<%= singular_table_name %> %>
    </div>

    <div class="mt-8">
      <%= link_to "⬅️ Voltar", @<%= singular_table_name %>,
          class: "inline-block bg-gray-600 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded-lg transition" %>
    </div>
  </div>
</div>
```

---

## 🎯 Passo 7: Usar os Templates Customizados

Depois de criar todos os arquivos, execute:

```bash
# Remover scaffolds antigos se houver
rails destroy scaffold Post

# Gerar novo scaffold com seus templates customizados
rails generate scaffold Post title:string content:text published:boolean
```

**Resultado:** Seus CRUDs serão gerados automaticamente com:
- ✅ Design mobile-first responsivo
- ✅ Paleta de cores profissional
- ✅ Tabelas em desktop, cards em mobile
- ✅ Formulários estilizados
- ✅ Mensagens de erro visual
- ✅ Ícones e feedback visual

---

## 🎨 Passo 8: Customizações Avançadas (Opcional)

### Adicionar Componente Helper para Botões

**Arquivo:** `app/helpers/application_helper.rb`

```ruby
module ApplicationHelper
  def btn_primary(text, path, **options)
    link_to text, path, class: "bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg transition #{options[:class]}", **options
  end

  def btn_danger(text, path, **options)
    link_to text, path, class: "bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-lg transition #{options[:class]}", **options
  end

  def btn_secondary(text, path, **options)
    link_to text, path, class: "bg-gray-600 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded-lg transition #{options[:class]}", **options
  end

  def badge_success(text)
    content_tag :span, text, class: "inline-block px-3 py-1 text-xs font-semibold rounded-full bg-emerald-100 text-emerald-800"
  end

  def badge_warning(text)
    content_tag :span, text, class: "inline-block px-3 py-1 text-xs font-semibold rounded-full bg-amber-100 text-amber-800"
  end

  def badge_danger(text)
    content_tag :span, text, class: "inline-block px-3 py-1 text-xs font-semibold rounded-full bg-red-100 text-red-800"
  end
end
```

**Usar nos templates:**
```erb
<%= btn_primary("Novo", new_post_path) %>
<%= badge_success("Ativo") %>
```

---

## 📱 Checklist de Responsividade

- [x] Tabelas se convertem em cards no mobile
- [x] Botões ocupam largura cheia no mobile
- [x] Inputs têm padding adequado para toque
- [x] Textos legíveis em qualquer tamanho
- [x] Imagens carregam otimizadas
- [x] Sem scroll horizontal desnecessário
- [x] Cores com alto contraste para acessibilidade

---

## 🚀 Próximos Passos

1. **Criar o projeto:**
   ```bash
   rails new app_name --css=tailwind
   ```

2. **Copiar os templates** para `lib/templates/erb/scaffold/`

3. **Gerar scaffolds:**
   ```bash
   rails generate scaffold Model field1:string field2:text
   ```

4. **Customizar conforme necessário** para suas marcas e paletas específicas

---

## 💡 Dicas Finais

- Use `truncate()` em textos longos nas listagens
- Adicione `data-confirm` em ações destrutivas
- Teste em múltiplos dispositivos (iPhone, tablet, desktop)
- Considere adicionar paginação com `kaminari` para listas grandes
- Use `ActionText` se precisar de editor rich text

**Paleta Tailwind Usada:**
- Azuis: `blue-50, blue-100, blue-500, blue-600, blue-700, blue-800`
- Verdes: `emerald-50, emerald-100, emerald-500, emerald-800`
- Laranjas: `amber-50, amber-100, amber-500, amber-600, amber-700`
- Vermelhos: `red-50, red-100, red-500, red-600, red-700`
- Cinzas: `gray-50, gray-100, gray-200, gray-300, gray-400, gray-500, gray-600, gray-700, gray-900`

---

**Versão:** Rails 8 + Tailwind CSS 3
**Autor:** Arthur Ramos Vieira
**Data:** 2025