## `src/components/SearchBar/SearchBar.css`

O arquivo `src/components/SearchBar/SearchBar.css` define os estilos específicos do componente de busca. Sua função é complementar as classes utilitárias aplicadas em `SearchBar.tsx`, mantendo no CSS do componente apenas aquilo que pertence à sua estrutura visual particular.

O componente já utiliza classes globais no elemento principal.

```tsx
<div className="search-bar surface row gap-md">
```

A classe `surface` aplica fundo, borda, raio e sombra. A classe `row` organiza ícone e campo em linha. A classe `gap-md` define o espaçamento entre os elementos. Dessa forma, o CSS específico do `SearchBar` não precisa repetir essas regras.

A classe `search-bar` define apenas o tamanho mínimo e o espaçamento interno do campo de busca.

```css
.search-bar {
  min-height: 48px;
  padding: 0 var(--space-xl);
}
```

A propriedade `min-height` garante que a barra tenha altura suficiente para boa leitura e interação. O `padding` horizontal utiliza o token `--space-xl`, mantendo o espaçamento alinhado ao design system. Não há definição de cor, borda ou sombra nesse trecho porque esses aspectos já são tratados pela classe `surface`.

O ícone de busca recebe apenas a cor secundária do texto.

```css
.search-bar__icon {
  color: var(--color-text-secondary);
}
```

Essa cor indica que o ícone tem função auxiliar. Ele reforça visualmente a finalidade do campo, mas não compete com o conteúdo principal da interface.

O campo de entrada é configurado para ocupar a largura disponível e se integrar visualmente à superfície da busca.

```css
.search-bar__input {
  width: 100%;
  border: none;
  outline: none;
  color: var(--color-text-primary);
  background-color: transparent;
  font-size: var(--font-size-md);
}
```

A propriedade `width: 100%` faz com que o campo ocupe o espaço restante ao lado do ícone. A remoção de `border` e `outline` evita que o input crie uma caixa visual própria dentro da barra. O fundo transparente permite que a aparência do campo seja dada pela superfície externa. A cor e o tamanho da fonte utilizam tokens do sistema visual.

O placeholder também recebe um token semântico de cor.

```css
.search-bar__input::placeholder {
  color: var(--color-text-muted);
}
```

Essa regra diferencia o texto auxiliar do texto digitado pelo usuário. A cor esmaecida indica que se trata de uma orientação de preenchimento, não de um valor efetivo do campo.

Assim, `SearchBar.css` permanece restrito ao comportamento visual específico da barra de busca. A estrutura geral, a superfície, o alinhamento e o espaçamento são reaproveitados de `utilities.css`. Essa organização reduz repetição e mantém o CSS do componente pequeno, objetivo e coerente com a padronização do projeto.
