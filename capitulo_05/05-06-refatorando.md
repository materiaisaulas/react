# Implementação inicial de tema e Design System: tokens de cores

Nesta etapa, é construido a organização visual do projeto por meio da criação de **tokens de cores**. A ideia não é alterar a estrutura funcional da aplicação, mas preparar o código para uma manutenção visual mais organizada, padronizada e escalável.

Foram alterados dois arquivos principais:

```txt
src/styles/variables.css
src/styles/utilities.css
```

O arquivo `variables.css` concentra a definição das cores utilizadas no projeto. Essa organização é importante porque evita que valores de cor fiquem espalhados por vários arquivos CSS. Em vez de cada componente declarar diretamente cores como `#ffffff`, `#2563eb` ou `#0f172a`, essas cores passam a ser nomeadas e centralizadas em variáveis CSS.

## Organização anterior das variáveis de cor

O arquivo `variables.css` já utilizava variáveis CSS, porém, essas variáveis estavam organizadas diretamente como tokens finais da interface, sem uma separação clara entre a paleta base e os papéis visuais de cada cor.

```css
:root {
  --color-page: #f8fafc;
  --color-surface: #ffffff;

  --color-text-primary: #0f172a;
  --color-text-secondary: #64748b;
  --color-text-muted: #94a3b8;

  --color-border: #dbe3ef;
  --color-border-strong: #cbd5e1;

  --color-primary: #2563eb;
  --color-primary-hover: #1d4ed8;

  --color-neutral: #f1f5f9;
  --color-neutral-hover: #e2e8f0;
  --color-neutral-text: #334155;

  --color-info: #0369a1;
  --color-info-bg: #e0f2fe;
  --color-info-bg-hover: #bae6fd;

  --color-danger: #b91c1c;
  --color-danger-strong: #991b1b;
  --color-danger-bg: #fee2e2;
  --color-danger-bg-hover: #fecaca;

  --radius-sm: 10px;
  --radius-md: 12px;
  --radius-lg: 14px;

  --shadow-surface: 0 18px 40px rgba(15, 23, 42, 0.08);
  --shadow-primary: 0 10px 20px rgba(37, 99, 235, 0.25);

  --spacing-page: 40px 20px;

  --transition-default: 0.2s ease;
}
```

Esse modelo funciona, mas tem uma limitação estrutural: os valores das cores ficam diretamente associados aos tokens usados pela interface. Assim, caso seja necessário alterar a paleta visual, criar variações de tema ou implementar um modo escuro, a manutenção tende a ser menos clara.

## Nova organização com cores primitivas e tokens semânticos

Após a alteração, o arquivo passou a separar as cores em camadas. A primeira camada contém as **cores primitivas**, que representam a paleta base do projeto.

```css
:root {
  /* =========================================================
     1. DESIGN TOKENS - CORES PRIMITIVAS
     Estas variáveis representam a paleta base do projeto.
     ========================================================= */
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

  --sky-100: #e0f2fe;
  --sky-200: #bae6fd;
  --sky-700: #0369a1;

  --red-100: #fee2e2;
  --red-200: #fecaca;
  --red-700: #b91c1c;
  --red-800: #991b1b;

  --white: #ffffff;
}
```

As cores primitivas descrevem apenas a cor em si. Elas não indicam diretamente onde a cor será aplicada. Por exemplo, `--blue-600` representa um tom de azul, enquanto `--slate-900` representa um tom escuro de cinza. Essa camada funciona como a matéria-prima visual do projeto.

A segunda camada é formada pelos **tokens semânticos**, que indicam o papel que determinada cor exerce na interface.

```css
:root {
  /* =========================================================
     2. TOKENS SEMÂNTICOS - TEMA CLARO
     Os componentes devem usar estes tokens, não a paleta direta.
     ========================================================= */
  --color-page: var(--slate-50);
  --color-surface: var(--white);
  --color-surface-input: var(--white);

  --color-text-primary: var(--slate-900);
  --color-text-secondary: var(--slate-500);
  --color-text-muted: var(--slate-400);

  --color-border: #dbe3ef;
  --color-border-strong: var(--slate-300);

  --color-primary: var(--blue-600);
  --color-primary-hover: var(--blue-700);
  --color-primary-text: var(--white);

  --color-neutral: var(--slate-100);
  --color-neutral-hover: var(--slate-200);
  --color-neutral-text: var(--slate-700);

  --color-info: var(--sky-700);
  --color-info-bg: var(--sky-100);
  --color-info-bg-hover: var(--sky-200);

  --color-danger: var(--red-700);
  --color-danger-strong: var(--red-800);
  --color-danger-bg: var(--red-100);
  --color-danger-bg-hover: var(--red-200);

  --color-overlay-modal: rgba(15, 23, 42, 0.45);
}
```

Essa abordagem torna o código mais claro, pois o nome da variável passa a comunicar a função visual da cor. O token `--color-primary`, por exemplo, indica a cor principal da interface. O token `--color-text-primary` indica a cor principal dos textos. O token `--color-surface` representa a superfície dos elementos, como cartões, áreas de conteúdo ou componentes visuais.

Dessa forma, os componentes não precisam saber qual tom exato de azul, cinza ou vermelho está sendo utilizado. Eles apenas utilizam o token correspondente ao papel visual necessário.

## Tokens complementares do Design System

Além das cores, o arquivo `variables.css` também mantém tokens relacionados a bordas arredondadas, sombras, espaçamentos e transições.

```css
:root {
  /* =========================================================
     3. TOKENS DE FORMA, SOMBRA, ESPAÇAMENTO E MOVIMENTO
     ========================================================= */
  --radius-sm: 10px;
  --radius-md: 12px;
  --radius-lg: 14px;

  --shadow-surface: 0 18px 40px rgba(15, 23, 42, 0.08);
  --shadow-primary: 0 10px 20px rgba(37, 99, 235, 0.25);

  --spacing-page: 40px 20px;

  --transition-default: 0.2s ease;

  --shadow-focus-primary: 0 0 0 3px rgba(37, 99, 235, 0.12);
}
```

Esses elementos também fazem parte de um Design System, pois ajudam a manter consistência visual em toda a aplicação. O raio de borda, a sombra e o espaçamento deixam de ser decisões isoladas de cada componente e passam a seguir uma referência comum.

# Alterações em `src/styles/utilities.css`

No arquivo `utilities.css`, a alteração principal foi substituir valores fixos por tokens semânticos. Essa mudança aproxima os componentes do tema visual definido no arquivo `variables.css`.

## Alteração no botão primário

Antes da alteração, o botão primário já usava uma variável para a cor de fundo, mas ainda mantinha a cor do texto fixa como `#ffffff`.

```css
.button--primary {
  background-color: var(--color-primary);
  color: #ffffff;
  box-shadow: var(--shadow-primary);
}
```

Após a alteração, a cor do texto passou a usar um token semântico específico.

```css
.button--primary {
  background-color: var(--color-primary);
  color: var(--color-primary-text);
  box-shadow: var(--shadow-primary);
}
```

Essa mudança é importante porque o botão deixa de depender diretamente de um valor fixo de cor. A cor do texto passa a ser controlada pelo token `--color-primary-text`, definido no arquivo central de variáveis.

Com isso, caso o tema visual seja alterado futuramente, não será necessário localizar todos os botões no CSS para trocar a cor do texto. A alteração poderá ser feita no próprio token.

## Alteração no campo de formulário

Antes da alteração, o campo de formulário utilizava uma cor fixa para o fundo.

```css
.form-input {
  min-height: 44px;
  border: 1px solid var(--color-border-strong);
  border-radius: var(--radius-sm);
  padding: 0 14px;
  font-size: 0.95rem;
  outline: none;
  color: var(--color-text-primary);
  background-color: #ffffff;
}
```

Depois da alteração, o fundo passou a utilizar um token semântico.

```css
.form-input {
  min-height: 44px;
  border: 1px solid var(--color-border-strong);
  border-radius: var(--radius-sm);
  padding: 0 14px;
  font-size: 0.95rem;
  outline: none;
  color: var(--color-text-primary);
  background-color: var(--color-surface-input);
}

```

Nesse caso, o campo de formulário passa a seguir o tema visual da aplicação. O token `--color-surface-input` representa a superfície de campos de entrada. Assim, o componente deixa de carregar uma decisão visual fixa e passa a depender de uma decisão centralizada no tema.

Essa alteração é especialmente importante para a futura implementação do **dark mode manual**. Em um modo escuro, o fundo do campo de formulário provavelmente não será branco. Com o uso do token, bastará redefinir `--color-surface-input` no tema escuro.

Outro ponto de alteração é o foco do formulário, o campo usava diretamente um valor de sombra.

```css
.form-input:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.12);
}

```

Esse código funcionava visualmente, mas ainda mantinha uma decisão de design fixa dentro da classe .form-input. Isso dificultaria uma alteração futura, especialmente em um cenário com múltiplos temas.

Após a alteração, o valor da sombra foi substituído por um token semântico.


```css
.form-input:focus {
  border-color: var(--color-primary);
  box-shadow: var(--shadow-focus-primary);
}

```

Com essa mudança, o comportamento visual do foco continua o mesmo, mas a definição da sombra passa a ser controlada pelo Design System. A classe .form-input deixa de conhecer o valor exato da sombra e passa apenas a utilizar o token definido para esse tipo de interação.

## Alteração no fundo de sobreposição do modal

Antes da alteração, o fundo do modal era definido diretamente no arquivo ClienteFormModal.css.

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  z-index: 1000;
  display: grid;
  place-items: center;
  padding: 24px;
  background-color: rgba(15, 23, 42, 0.45);
}

```

Esse valor representa a camada escurecida que aparece atrás do modal. Embora pareça um detalhe simples, ele também faz parte do padrão visual da aplicação.

Depois da alteração, o valor fixo foi substituído por um token.

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  z-index: 1000;
  display: grid;
  place-items: center;
  padding: 24px;
  background-color: var(--color-overlay-modal);
}

```

A partir dessa mudança, o fundo de sobreposição passa a seguir a mesma lógica dos demais elementos visuais do sistema. Caso seja necessário alterar a intensidade do escurecimento ou adaptar esse comportamento para um modo escuro, a mudança poderá ser feita diretamente no arquivo variables.css.