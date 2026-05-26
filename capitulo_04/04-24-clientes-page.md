## `src/pages/ClientesPage/ClientesPage.css`

O arquivo `src/pages/ClientesPage/ClientesPage.css` define o estilo específico da página de clientes. Sua função é delimitar a área principal da tela, controlando a largura do conteúdo e sua centralização horizontal.

Após a refatoração do CSS, esse arquivo ficou reduzido porque a maior parte dos padrões visuais foi deslocada para `variables.css` e `utilities.css`. Essa decisão é coerente com a padronização adotada no projeto: a página mantém apenas aquilo que pertence à sua estrutura específica, enquanto espaçamentos, superfícies, botões, mensagens, formulários e modais são tratados por classes reutilizáveis.

A classe principal é `clientes-page`.

```css
.clientes-page {
  width: 100%;
  max-width: var(--max-width-page);
  margin: 0 auto;
}
```

A propriedade `width: 100%` indica que a página pode ocupar toda a largura disponível do contêiner externo. Entretanto, essa largura é limitada por `max-width: var(--max-width-page)`, token definido em `variables.css`.

Esse limite evita que o conteúdo fique excessivamente espalhado em telas largas. Assim, a interface mantém uma largura adequada para leitura e uso, mesmo quando o navegador possui grande resolução.

A propriedade `margin: 0 auto` centraliza horizontalmente a página. O valor `0` remove margens verticais, enquanto `auto` nas laterais distribui o espaço restante de maneira igual à esquerda e à direita.

A organização vertical dos elementos da página não está nesse CSS. Ela é definida no próprio componente `ClientesPage.tsx`, por meio das classes utilitárias:

```tsx
<section className="clientes-page stack stack--lg">
```

A classe `stack` organiza os blocos em coluna, e `stack--lg` define o espaçamento vertical entre cabeçalho, toolbar, feedback, conteúdo e modais condicionais. Com isso, o arquivo `ClientesPage.css` não precisa declarar margens entre os componentes internos.

Assim, `ClientesPage.css` possui uma responsabilidade delimitada: controlar a largura e a centralização da página de clientes. Ele não define estilos de cabeçalho, lista, formulário, botões ou modais. Essa separação reduz redundância e mantém o CSS da página restrito ao seu papel estrutural.
