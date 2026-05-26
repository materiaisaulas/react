## `src/styles/variables.css`

O arquivo `src/styles/variables.css` concentra os principais **tokens de design** da aplicação. Em um design system, tokens são valores padronizados que representam decisões visuais reutilizáveis, como cores, espaçamentos, sombras, raios de borda, tamanhos de fonte e largura máxima de elementos.

Essa organização evita que os componentes definam valores visuais de forma isolada. Em vez de cada arquivo CSS declarar diretamente uma cor, um espaçamento ou uma sombra, o projeto passa a utilizar variáveis centralizadas. Dessa forma, a identidade visual da aplicação fica mais consistente e a manutenção se torna mais simples.

O arquivo começa com os tokens de cores primitivas.

```css
--blue-50: #eff6ff;
--blue-100: #dbeafe;
--blue-600: #2563eb;
--blue-700: #1d4ed8;

--slate-50: #f8fafc;
--slate-100: #f1f5f9;
--slate-200: #e2e8f0;
--slate-300: #cbd5e1;
--slate-400: #94a3b8;
--slate-500: #64748b;
--slate-700: #334155;
--slate-900: #0f172a;
```

Essas variáveis representam a paleta base do projeto. Elas ainda não indicam diretamente a função de cada cor na interface. Por exemplo, `--blue-600` define uma tonalidade de azul, mas não afirma se ela será usada em botão, link, borda ou destaque. Essa separação é importante porque a paleta primitiva funciona como matéria-prima visual.

Em seguida, o arquivo define os tokens semânticos do tema claro.

```css
--color-page: var(--slate-50);
--color-surface: var(--white);
--color-surface-input: var(--white);

--color-text-primary: var(--slate-900);
--color-text-secondary: var(--slate-500);
--color-text-muted: var(--slate-400);
```

Os tokens semânticos indicam a função da cor na interface. `--color-page` representa a cor de fundo da página. `--color-surface` representa a cor de cartões, caixas e superfícies. `--color-text-primary`, `--color-text-secondary` e `--color-text-muted` definem níveis de importância textual.

Essa distinção entre cor primitiva e cor semântica é relevante. O componente não precisa saber qual azul ou qual cinza será usado. Ele deve utilizar o token semântico correspondente à função visual desejada. Assim, se a cor principal do sistema mudar futuramente, altera-se o token semântico, sem necessidade de revisar todos os componentes.

O mesmo padrão é aplicado às cores de ação.

```css
--color-primary: var(--blue-600);
--color-primary-hover: var(--blue-700);
--color-primary-text: var(--white);

--color-danger: var(--red-700);
--color-danger-hover: var(--red-800);
--color-danger-text: var(--white);
```

Os tokens `primary` representam ações principais da interface, como salvar ou criar um novo cliente. Os tokens `danger` representam ações destrutivas, como excluir um registro. A presença de variações `hover` permite padronizar o comportamento visual quando o usuário passa o cursor sobre botões ou elementos interativos.

O arquivo também define tokens relacionados à forma, sombra e espaçamento.

```css
--radius-sm: 10px;
--radius-md: 12px;
--radius-lg: 14px;
--radius-xl: 18px;

--shadow-surface: 0 4px 14px rgba(15, 23, 42, 0.06);
--shadow-primary: 0 8px 22px rgba(37, 99, 235, 0.22);
--shadow-danger: 0 8px 22px rgba(185, 28, 28, 0.18);
--shadow-modal: 0 24px 70px rgba(15, 23, 42, 0.24);
```

Os tokens de raio controlam o arredondamento dos elementos. Os tokens de sombra definem a profundidade visual de superfícies, botões e modais. Dessa forma, cartões, botões e janelas modais seguem um mesmo padrão visual, sem que cada componente precise criar uma sombra própria.

Os espaçamentos também são centralizados.

```css
--space-xs: 6px;
--space-sm: 8px;
--space-md: 10px;
--space-lg: 14px;
--space-xl: 16px;
--space-2xl: 18px;
--space-3xl: 20px;
--space-4xl: 24px;
--space-5xl: 26px;
--space-6xl: 32px;
```

Esses tokens permitem que margens, espaçamentos internos e distâncias entre elementos sejam definidos por uma escala comum. Isso reduz a repetição de valores fixos, como `16px`, `20px` ou `24px`, espalhados por vários arquivos CSS.

O arquivo também define medidas estruturais.

```css
--max-width-page: 980px;
--max-width-modal: 480px;

--z-index-modal: 1000;
```

`--max-width-page` limita a largura máxima do conteúdo principal. `--max-width-modal` define a largura máxima dos modais. `--z-index-modal` garante que a camada modal fique acima dos demais elementos da interface.

Por fim, são definidos tokens de tipografia.

```css
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
```

Esses tokens controlam família tipográfica, tamanhos de fonte, pesos e alturas de linha. A padronização tipográfica evita que títulos, textos, rótulos e botões sejam definidos de maneira arbitrária em cada componente.

Assim, `variables.css` atua como a base do design system do projeto. Ele não estiliza diretamente um componente específico, mas define os valores fundamentais que serão reutilizados por toda a aplicação. Essa separação torna o CSS mais consistente, reduz redundâncias e facilita futuras alterações visuais no sistema.
