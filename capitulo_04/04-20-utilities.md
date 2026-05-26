## `src/styles/utilities.css`

O arquivo `src/styles/utilities.css` reúne classes reutilizáveis que representam padrões recorrentes da interface. Sua função é evitar que cada componente repita regras comuns de layout, espaçamento, botões, formulários, mensagens, superfícies e modais.

No design system do projeto, `variables.css` define os valores fundamentais, enquanto `utilities.css` transforma esses valores em classes de uso prático. Dessa forma, os componentes não precisam declarar repetidamente propriedades como `display: flex`, `gap`, `border-radius`, `box-shadow`, `padding`, `width: 100%` ou estilos básicos de botão.

A classe `surface` define o padrão visual de superfície.

```css
.surface {
  background-color: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-surface);
}
```

Essa classe é utilizada em elementos que precisam se destacar do fundo da página, como itens da lista, barra de busca e estados vazios. Ela aplica fundo, borda, arredondamento e sombra de forma padronizada. Com isso, evita-se que cada componente crie sua própria definição de cartão ou caixa visual.

As classes de texto e reset reduzem repetições simples.

```css
.text-muted {
  color: var(--color-text-muted);
}

.text-secondary {
  color: var(--color-text-secondary);
}

.title-reset,
.text-reset {
  margin: 0;
}
```

`text-muted` e `text-secondary` aplicam variações semânticas de cor textual. Já `title-reset` e `text-reset` removem margens padrão de elementos textuais. Esse tipo de classe evita a repetição de `margin: 0` em diversos títulos e parágrafos.

As classes estruturais controlam largura e comportamento de elementos em layouts flexíveis.

```css
.min-w-0 {
  min-width: 0;
}

.flex-static {
  flex: 0 0 auto;
}

.full-width {
  width: 100%;
}
```

A classe `full-width` substitui várias regras repetidas de `width: 100%` em componentes específicos. `min-w-0` é útil em elementos flexíveis que precisam permitir truncamento ou ajuste de largura. `flex-static` impede que determinado elemento cresça ou diminua dentro de um contêiner flexível.

As classes `stack` e suas variações definem organização vertical.

```css
.stack {
  display: flex;
  flex-direction: column;
}

.stack--sm {
  gap: var(--space-xs);
}

.stack--md {
  gap: var(--space-lg);
}

.stack--lg {
  gap: var(--space-3xl);
}
```

Esse padrão é utilizado quando os elementos devem ser organizados em coluna. Em vez de aplicar margens específicas entre componentes, a aplicação usa `gap`. Essa decisão melhora a padronização porque o espaçamento entre blocos passa a seguir a escala definida em `variables.css`.

As classes `row` organizam elementos horizontalmente.

```css
.row {
  display: flex;
  align-items: center;
}

.row--between {
  justify-content: space-between;
}

.row--end {
  justify-content: flex-end;
}
```

`row` define uma linha flexível com alinhamento vertical centralizado. `row--between` distribui os elementos entre as extremidades do contêiner. `row--end` alinha os elementos ao final. Essas classes aparecem em cabeçalhos, rodapés de modal, áreas de botões e itens da lista.

As classes de espaçamento horizontal complementam esse padrão.

```css
.gap-sm {
  gap: var(--space-md);
}

.gap-md {
  gap: var(--space-xl);
}
```

Elas padronizam a distância entre elementos dispostos em linha, evitando que cada componente declare valores próprios de `gap`.

O bloco de botões define o padrão geral e suas variações.

```css
.button {
  min-height: 42px;
  border: none;
  border-radius: var(--radius-sm);
  padding: 0 var(--space-2xl);
  font-weight: var(--font-weight-bold);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-sm);
  transition: var(--transition-default);
}
```

A classe `button` define a base comum dos botões: altura mínima, borda, arredondamento, espaçamento interno, alinhamento, peso da fonte e transição. As variações definem a intenção visual da ação.

```css
.button--primary {
  background-color: var(--color-primary);
  color: var(--color-primary-text);
  box-shadow: var(--shadow-primary);
}

.button--secondary {
  background-color: var(--color-neutral);
  color: var(--color-neutral-text);
}

.button--danger {
  background-color: var(--color-danger);
  color: var(--color-danger-text);
  box-shadow: var(--shadow-danger);
}
```

`button--primary` representa a ação principal. `button--secondary` representa uma ação neutra ou de cancelamento. `button--danger` representa uma ação destrutiva, como excluir um cliente. Essa separação torna o uso dos botões mais consistente, pois o componente escolhe a intenção visual por classe, sem redefinir estilos.

O mesmo princípio é aplicado aos botões com ícone.

```css
.icon-button {
  width: 38px;
  height: 38px;
  border: none;
  border-radius: var(--radius-sm);
  display: grid;
  place-items: center;
  font-size: var(--font-size-md);
  transition: var(--transition-default);
}
```

A classe `icon-button` define o formato comum dos botões quadrados usados em ações como editar, excluir ou fechar. As variações indicam a intenção visual.

```css
.icon-button--neutral
.icon-button--edit
.icon-button--delete
```

Dessa forma, o botão de fechar modal, editar cliente e excluir cliente compartilham a mesma base visual, diferenciando-se apenas pela variação de cor.

As classes de formulário também seguem o mesmo padrão de reutilização.

```css
.form-field {
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
}

.form-label {
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-bold);
  color: var(--color-neutral-text);
}

.form-input {
  min-height: 44px;
  border: 1px solid var(--color-border-strong);
  border-radius: var(--radius-sm);
  padding: 0 var(--space-lg);
  font-size: var(--font-size-md);
  outline: none;
  color: var(--color-text-primary);
  background-color: var(--color-surface-input);
}
```

Essas classes padronizam a estrutura dos campos do formulário. `form-field` organiza rótulo e campo em coluna. `form-label` define o padrão do texto do rótulo. `form-input` define altura, borda, arredondamento, espaçamento interno, fonte e cores do campo.

O estado de foco do campo também foi padronizado.

```css
.form-input:focus {
  border-color: var(--color-primary);
  box-shadow: var(--shadow-focus-primary);
}
```

Essa regra usa o token `--shadow-focus-primary`, evitando a repetição manual do valor da sombra. Assim, todos os campos que usam `form-input` passam a apresentar o mesmo comportamento visual ao receber foco.

As mensagens e estados vazios também são tratados como padrões reutilizáveis.

```css
.message {
  padding: var(--space-lg) var(--space-xl);
  border-radius: var(--radius-md);
  font-weight: var(--font-weight-semibold);
}

.message--error {
  background-color: var(--color-danger-bg);
  color: var(--color-danger-strong);
}

.empty-state {
  padding: var(--space-6xl);
  text-align: center;
  color: var(--color-text-secondary);
}
```

A classe `message` define a estrutura comum de uma mensagem. A variação `message--error` define a aparência específica de erro. A classe `empty-state` define o padrão para situações em que não há dados a exibir ou quando a aplicação está em carregamento.

Por fim, o arquivo concentra a base visual dos modais.

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  z-index: var(--z-index-modal);
  display: grid;
  place-items: center;
  padding: var(--space-4xl);
  background-color: var(--color-overlay-modal);
}
```

A classe `modal-overlay` cria a camada que cobre toda a tela. O uso de `position: fixed` e `inset: 0` faz com que a camada ocupe toda a viewport. O `z-index` coloca o modal acima dos demais elementos. O `display: grid` com `place-items: center` centraliza o modal na tela.

A classe `modal-card` define o padrão comum da janela modal.

```css
.modal-card {
  position: static;
  inset: auto;
  justify-self: center;
  align-self: center;
  display: block;
  width: min(100%, var(--max-width-modal));
  max-width: none;
  overflow: hidden;
  border: none;
  padding: 0;
  margin: 0;
  border-radius: var(--radius-xl);
  color: inherit;
  background-color: var(--color-surface);
  box-shadow: var(--shadow-modal);
}
```

Essa classe padroniza os modais do sistema e neutraliza estilos nativos do elemento `<dialog>`, como borda, margem e posicionamento. Com isso, `ClienteFormModal` e `ConfirmModal` compartilham a mesma base visual.

As demais classes de modal definem cabeçalho, título, corpo, mensagem e rodapé.

```css
.modal-header {
  padding: var(--space-4xl) var(--space-5xl);
  border-bottom: 1px solid var(--color-border);
}

.modal-title {
  margin: 0;
  color: var(--color-text-primary);
  font-size: var(--font-size-xl);
}

.modal-body {
  padding: var(--space-5xl);
}

.modal-message {
  margin: 0;
  color: var(--color-text-secondary);
  line-height: var(--line-height-base);
}

.modal-footer {
  margin-top: var(--space-md);
}
```

Essa padronização evita que cada modal recrie cabeçalho, corpo e rodapé em arquivos CSS específicos. O resultado é uma estrutura comum para janelas modais, com variações apenas quando necessário.

Assim, `utilities.css` transforma o design system em classes reutilizáveis. Ele reduz a repetição de CSS, estabelece padrões visuais compartilhados e permite que os componentes fiquem menores. Os arquivos CSS específicos passam a conter apenas aquilo que pertence ao componente, enquanto as regras comuns permanecem centralizadas em uma camada utilitária.
