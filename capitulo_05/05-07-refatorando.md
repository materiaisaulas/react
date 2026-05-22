# Implementação dos tokens de tipografia no Design System

Após a organização dos tokens de cores, a próxima etapa do Design System foi a criação dos **tokens de tipografia**. Essa etapa tem como objetivo centralizar as decisões relacionadas à fonte, aos tamanhos de texto, aos pesos tipográficos e às alturas de linha.

Assim como ocorreu com as cores, a proposta não é alterar a lógica da aplicação. A mudança foi estrutural e visual. O projeto deixa de utilizar valores tipográficos fixos espalhados pelos arquivos CSS e passa a utilizar variáveis centralizadas no arquivo `variables.css`.

Os arquivos a serem alterados são:

```txt
src/styles/variables.css
src/styles/reset.css
src/styles/utilities.css
src/App.css
src/components/ClienteFormModal/ClienteFormModal.css
src/components/ClienteItem/ClienteItem.css
src/components/SearchBar/SearchBar.css
```

## Inclusão dos tokens de tipografia em `variables.css`

Antes desta alteração, os tamanhos de fonte, pesos e família tipográfica estavam definidos diretamente nos arquivos CSS. Depois da alteração, esses valores passaram a ser centralizados como tokens.

```css
/* =========================================================
   4. TOKENS DE TIPOGRAFIA
   Estes tokens centralizam família, tamanho, peso e altura de linha.
   ========================================================= */
--font-family-base: Inter, Arial, Helvetica, sans-serif;

--font-size-xs: 0.9rem;
--font-size-sm: 0.92rem;
--font-size-md: 0.95rem;
--font-size-base: 1rem;
--font-size-lg: 1.05rem;
--font-size-xl: 1.25rem;
--font-size-2xl: 2rem;

--font-weight-regular: 400;
--font-weight-medium: 500;
--font-weight-semibold: 600;
--font-weight-bold: 700;

--line-height-tight: 1.2;
--line-height-base: 1.5;
```

Esses tokens definem uma escala tipográfica básica para o projeto. O token `--font-family-base` centraliza a família de fonte principal. Os tokens `--font-size-*` organizam os tamanhos usados na interface. Os tokens `--font-weight-*` padronizam os pesos da fonte, enquanto os tokens `--line-height-*` controlam a altura de linha dos textos.

## Alteração da fonte base em `reset.css`

Antes, a fonte principal era definida diretamente no `body`.

```css
body {
  margin: 0;
  font-family: Inter, Arial, Helvetica, sans-serif;
  background-color: var(--color-page);
  color: var(--color-text-primary);
}
```

Depois, a família tipográfica passou a usar o token definido no Design System.

```css
body {
  margin: 0;
  font-family: var(--font-family-base);
  background-color: var(--color-page);
  color: var(--color-text-primary);
  font-size: var(--font-size-base);
  line-height: var(--line-height-base);
}
```

Essa alteração torna o `body` responsável por aplicar a base tipográfica geral da aplicação. Com isso, os elementos internos herdam uma configuração mais consistente de fonte, tamanho e altura de linha.

## Alteração dos textos principais em `App.css`

Antes, o título principal da página usava valores fixos de tamanho e altura de linha.

```css
.clientes-page__title {
  margin: 0;
  font-size: 2rem;
  line-height: 1.2;
  color: var(--color-text-primary);
}
```

Depois, esses valores foram substituídos por tokens.

```css
.clientes-page__title {
  margin: 0;
  font-size: var(--font-size-2xl);
  line-height: var(--line-height-tight);
  color: var(--color-text-primary);
}
```

O subtítulo também foi ajustado. Antes:

```css
.clientes-page__subtitle {
  margin: 0;
  font-size: 0.98rem;
  color: var(--color-text-secondary);
}
```

Depois:

```css
.clientes-page__subtitle {
  margin: 0;
  font-size: var(--font-size-base);
  color: var(--color-text-secondary);
}
```

Com isso, a hierarquia textual da página passa a depender da escala tipográfica centralizada no Design System.

## Alteração dos componentes utilitários em `utilities.css`

No arquivo `utilities.css`, foram substituídos valores fixos usados em botões, ícones, labels, inputs e mensagens.

Antes, o botão usava peso fixo:

```css
.button {
  font-weight: 700;
}
```

Depois, passou a usar token:

```css
.button {
  font-weight: var(--font-weight-bold);
}
```

O mesmo princípio foi aplicado aos labels dos formulários.

Antes:

```css
.form-label {
  font-size: 0.9rem;
  font-weight: 700;
  color: var(--color-neutral-text);
}
```

Depois:

```css
.form-label {
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-bold);
  color: var(--color-neutral-text);
}
```

Os campos de formulário também foram ajustados.

Antes:

```css
.form-input {
  font-size: 0.95rem;
}
```

Depois:

```css
.form-input {
  font-size: var(--font-size-md);
}
```

A mensagem de erro passou a utilizar token de peso tipográfico.

Antes:

```css
.message {
  font-weight: 600;
}
```

Depois:

```css
.message {
  font-weight: var(--font-weight-semibold);
}
```

Essas alterações deixam os componentes utilitários mais coerentes com a escala tipográfica do projeto.

## Alteração nos componentes específicos

No modal de cliente, o título deixou de usar tamanho fixo.

Antes:

```css
.cliente-modal__title {
  margin: 0;
  font-size: 1.25rem;
  color: var(--color-text-primary);
}
```

Depois:

```css
.cliente-modal__title {
  margin: 0;
  font-size: var(--font-size-xl);
  color: var(--color-text-primary);
}
```

No item de cliente, o nome e as informações também passaram a usar tokens.

Antes:

```css
.cliente-item__name {
  margin: 0;
  font-size: 1.05rem;
  color: var(--color-text-primary);
}

.cliente-item__info {
  margin: 0;
  color: var(--color-text-secondary);
  font-size: 0.92rem;
}
```

Depois:

```css
.cliente-item__name {
  margin: 0;
  font-size: var(--font-size-lg);
  color: var(--color-text-primary);
}

.cliente-item__info {
  margin: 0;
  color: var(--color-text-secondary);
  font-size: var(--font-size-sm);
}
```

No campo de busca, o tamanho da fonte também foi padronizado.

Antes:

```css
.search-bar__input {
  font-size: 0.95rem;
}
```

Depois:

```css
.search-bar__input {
  font-size: var(--font-size-md);
}
```
