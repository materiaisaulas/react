## `src/components/ClientesHeader/ClientesHeader.css`

O arquivo `src/components/ClientesHeader/ClientesHeader.css` define os estilos específicos do cabeçalho da página de clientes. Sua função é complementar a estrutura visual do componente `ClientesHeader.tsx`, mantendo nesse arquivo apenas os elementos próprios do cabeçalho, como a organização responsiva, o título e o subtítulo.

O componente já utiliza classes utilitárias na estrutura principal.

```tsx
<header className="clientes-header row row--between gap-md">
```

As classes `row`, `row--between` e `gap-md` são responsáveis por organizar o cabeçalho em linha, distribuir o bloco textual e o botão nas extremidades e aplicar espaçamento padronizado. Dessa forma, o CSS específico do cabeçalho não precisa repetir regras gerais de `display: flex`, `align-items`, `justify-content` ou `gap`.

A classe `clientes-header` trata o comportamento próprio do cabeçalho.

```css
.clientes-header {
  flex-wrap: wrap;
}
```

A propriedade `flex-wrap: wrap` permite que os elementos do cabeçalho quebrem linha quando não houver espaço horizontal suficiente. Esse ajuste é importante em telas menores, pois evita que o título, o subtítulo e o botão fiquem comprimidos ou ultrapassem os limites da área visível.

O bloco textual do cabeçalho é identificado por `clientes-header__heading`.

```css
.clientes-header__heading {
  min-width: 0;
}
```

Essa regra permite que o conteúdo textual se ajuste adequadamente dentro de um layout flexível. O `min-width: 0` evita que textos longos forcem a largura do contêiner, contribuindo para um comportamento mais estável em diferentes tamanhos de tela.

O título da página recebe a maior hierarquia visual.

```css
.clientes-header__title {
  margin: 0;
  color: var(--color-text-primary);
  font-size: var(--font-size-2xl);
}
```

A margem padrão do navegador é removida. A cor do título utiliza o token `--color-text-primary`, indicando sua função como texto principal. O tamanho `--font-size-2xl` reforça a hierarquia do título em relação aos demais textos da página.

O subtítulo possui uma apresentação mais discreta.

```css
.clientes-header__subtitle {
  margin: 0;
  color: var(--color-text-secondary);
  font-size: var(--font-size-base);
}
```

A cor secundária indica que o subtítulo complementa o título, sem competir com ele. O tamanho de fonte base mantém a legibilidade e sustenta a função explicativa do texto.

Assim, `ClientesHeader.css` permanece restrito aos aspectos específicos do cabeçalho. A organização geral em linha, os espaçamentos e o padrão do botão são tratados por classes utilitárias. O arquivo concentra apenas a responsividade do cabeçalho e a hierarquia visual de título e subtítulo, mantendo a padronização e evitando repetição de CSS.
