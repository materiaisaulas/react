## `src/styles/index.css`

O arquivo `src/styles/index.css` funciona como o ponto central de importação dos estilos globais da aplicação. Sua responsabilidade é organizar a carga dos arquivos de base visual, garantindo que variáveis, reset e utilitários sejam carregados em uma ordem adequada.

Em projetos React, especialmente com Vite, é comum importar um único arquivo global no ponto de entrada da aplicação. No projeto, esse arquivo é importado em `main.tsx`, permitindo que os estilos globais estejam disponíveis para todos os componentes renderizados a partir de `App`.

```css
@import "./variables.css";
@import "./reset.css";
@import "./utilities.css";
```

A primeira importação carrega `variables.css`.

```css
@import "./variables.css";
```

Essa importação deve aparecer antes das demais porque os outros arquivos utilizam os tokens definidos nele. Cores, espaçamentos, sombras, raios de borda, tamanhos de fonte e medidas estruturais precisam estar disponíveis antes que `reset.css`, `utilities.css` e os componentes façam uso dessas variáveis.

A segunda importação carrega `reset.css`.

```css
@import "./reset.css";
```

O reset é carregado após as variáveis porque utiliza tokens como fonte, cor de fundo e cor de texto. Sua função é preparar a base visual da aplicação, removendo padrões indesejados do navegador e estabelecendo uma estrutura inicial mais previsível.

A terceira importação carrega `utilities.css`.

```css
@import "./utilities.css";
```

Os utilitários são carregados após as variáveis e o reset porque dependem dos tokens e atuam sobre a base já normalizada. Esse arquivo disponibiliza classes reutilizáveis para layout, botões, formulários, mensagens, superfícies e modais.

Assim, `index.css` não define estilos próprios de componentes. Sua função é organizar a sequência de carga da camada global de CSS. Essa centralização evita múltiplas importações espalhadas pelo projeto e torna mais explícita a composição da base visual da aplicação.
