## `src/components/ClienteItem/ClienteItem.css`

O arquivo `src/components/ClienteItem/ClienteItem.css` define os estilos específicos do componente responsável por apresentar um cliente individual na lista. Sua função é complementar as classes utilitárias usadas em `ClienteItem.tsx`, mantendo no CSS do componente apenas as regras que dizem respeito à identidade visual do item.

No componente, a estrutura principal já utiliza classes globais.

```tsx
<article className="cliente-item surface row row--between gap-md">
```

A classe `surface` aplica o padrão de fundo, borda, sombra e arredondamento. As classes `row`, `row--between` e `gap-md` organizam o conteúdo em linha, distribuem as informações e os botões nas extremidades e aplicam espaçamento padronizado. Assim, o arquivo `ClienteItem.css` não precisa repetir regras gerais de cartão, alinhamento ou espaçamento.

A classe `cliente-item` define ajustes específicos do item da lista.

```css
.cliente-item {
  padding: var(--space-xl);
}
```

O `padding` interno cria uma área de respiro entre o conteúdo do cliente e as bordas do cartão. O uso de `var(--space-xl)` mantém o espaçamento alinhado aos tokens definidos em `variables.css`, evitando valores fixos isolados no componente.

O bloco de conteúdo textual é identificado por `cliente-item__content`.

```css
.cliente-item__content {
  min-width: 0;
}
```

Essa regra é útil em layouts flexíveis. O `min-width: 0` permite que o conteúdo textual se ajuste corretamente dentro da linha, evitando que textos longos forcem o componente a ultrapassar a largura disponível.

O nome do cliente recebe destaque visual próprio.

```css
.cliente-item__name {
  margin: 0;
  color: var(--color-text-primary);
  font-size: var(--font-size-lg);
}
```

A margem é removida para evitar espaçamentos nativos do navegador. A cor principal do texto e o tamanho de fonte vêm dos tokens do sistema. Dessa forma, o nome do cliente aparece como a informação de maior hierarquia dentro do item.

As informações complementares, como e-mail e telefone, possuem estilo mais discreto.

```css
.cliente-item__info {
  margin: 0;
  color: var(--color-text-secondary);
  font-size: var(--font-size-sm);
}
```

Essas regras diferenciam visualmente os dados secundários do nome do cliente. O uso de `--color-text-secondary` indica menor hierarquia informacional, sem comprometer a legibilidade.

A área de ações pode receber uma configuração para impedir que os botões sejam comprimidos.

```css
.cliente-item__actions {
  flex: 0 0 auto;
}
```

Essa definição mantém os botões de edição e exclusão com tamanho estável dentro do layout. O conteúdo textual pode se ajustar, mas os botões permanecem preservados como área de ação.

Assim, `ClienteItem.css` mantém apenas as regras específicas do item de cliente. A estrutura de cartão, alinhamento, espaçamento entre elementos e botões é reaproveitada das classes utilitárias. Essa separação reduz repetição e mantém o componente visualmente consistente com o restante da aplicação.
