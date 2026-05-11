## 2. Fundamentos da aplicação React




## 2.2 Árvore de componentes

A **árvore de componentes** é a estrutura hierárquica formada pela composição entre componentes React. Nesse modelo, um componente pode conter outros componentes, que por sua vez podem conter novos componentes, formando uma organização em níveis.

A documentação oficial do React descreve que aplicações React são construídas a partir de componentes, e que esses componentes podem ser combinados para formar interfaces completas. ([react.dev](https://react.dev/learn/your-first-component))

Uma árvore de componentes pode ser representada assim:

![Árvore de componentes](./arvore_componentes.png){ width=50% }

Nessa representação, `App` é o componente de nível superior. Ele contém `Header`, `MainContent` e `Footer`. O componente `MainContent` contém `ProductList` e `CartSummary`. O componente `ProductList`, por sua vez, contém componentes `ProductCard`.

A árvore indica uma relação de composição, não uma relação de herança. Um componente não “herda” automaticamente o comportamento de outro. Ele é renderizado dentro da estrutura retornada por outro componente.

Exemplo simplificado:

```tsx
function Header() {
  return <header>Aplicação</header>;
}

function ProductCard() {
  return <article>Produto</article>;
}

function ProductList() {
  return (
    <section>
      <ProductCard />
      <ProductCard />
    </section>
  );
}

function App() {
  return (
    <main>
      <Header />
      <ProductList />
    </main>
  );
}
```

A estrutura acima forma a seguinte árvore:

![Árvore de programa](./arvore_programa.png){ width=50% }

Componentes de nível superior costumam organizar regiões maiores da interface. Componentes intermediários agrupam seções e listas. Componentes internos representam partes mais específicas da tela.

A árvore de componentes é importante porque a renderização ocorre a partir de uma raiz. A raiz React recebe uma estrutura inicial, e a partir dela os componentes descendentes são avaliados e renderizados.

![Renderização](./renderizacao.png){ width=50% }

## 2.7 Renderização da interface

A **renderização da interface** é o processo pelo qual o React interpreta a estrutura declarada pelos componentes e coordena sua exibição no navegador. Em uma aplicação React, os componentes descrevem o que deve aparecer na tela, e o React organiza a atualização da interface quando dados, propriedades, estado ou rotas mudam.

A documentação oficial do React descreve a renderização como parte de um fluxo composto por três momentos: disparo da renderização, renderização dos componentes e confirmação das alterações no DOM. Esse fluxo é apresentado como **trigger**, **render** e **commit**. ([react.dev](https://react.dev/learn/render-and-commit))

Na renderização inicial, o disparo ocorre quando a aplicação chama `root.render(...)`. Nas renderizações posteriores, o disparo pode ocorrer por mudança de estado, alteração de propriedades, atualização de dados ou mudança de rota.

Um componente simples pode ser representado assim:

```tsx
function Message() {
  return <p>Pedido recebido.</p>;
}
```

Ao renderizar esse componente, o React interpreta o retorno JSX e produz a estrutura correspondente na interface do navegador.

A renderização também pode depender de dados recebidos pelo componente:

```tsx
type CartMessageProps = {
  itemsCount: number;
};

function CartMessage({ itemsCount }: CartMessageProps) {
  return <p>Itens no carrinho: {itemsCount}</p>;
}
```

Nesse exemplo, a interface renderizada depende do valor de `itemsCount`. Se esse valor mudar, o React pode atualizar a parte correspondente da interface.

A renderização em React é declarativa. O componente não precisa informar passo a passo como alterar cada elemento do DOM. Ele descreve a estrutura esperada, e o React determina como refletir essa descrição na interface.

Exemplo declarativo:

```tsx
function StatusLabel({ completed }: { completed: boolean }) {
  return (
    <span>
      {completed ? "Concluído" : "Pendente"}
    </span>
  );
}
```

Nesse exemplo, o componente descreve o texto esperado conforme o valor de `completed`. O React é responsável por atualizar a interface quando esse valor muda.

Em uma aplicação organizada como árvore de componentes, a renderização não ocorre apenas em um componente isolado. O React percorre a estrutura a partir da raiz renderizada e avalia os componentes necessários para produzir a interface.

A renderização da interface deve ser compreendida como o mecanismo que transforma componentes, propriedades e estados em uma interface visível e atualizável no navegador.


## 2.8 React DOM

O **React DOM** é o pacote responsável por conectar a interface descrita pelos componentes React ao **DOM do navegador**. Enquanto o React organiza a interface em componentes e coordena sua renderização, o React DOM fornece as APIs que permitem inserir essa interface em um elemento HTML real.

A documentação oficial apresenta as APIs de cliente do `react-dom/client`, entre elas `createRoot`, usada para criar uma raiz React dentro de um nó DOM do navegador. Essa raiz permite renderizar componentes React no documento HTML. ([react.dev](https://react.dev/reference/react-dom/client))

Essa distinção é necessária porque o React, isoladamente, não define onde a interface será exibida. A estrutura descrita pelos componentes precisa ser associada a um elemento existente no HTML. Essa associação é realizada pelo React DOM.

Em projetos React atuais, essa integração normalmente ocorre por meio da importação de `createRoot` a partir de `react-dom/client`:

```tsx
import { createRoot } from "react-dom/client";
```

O React DOM atua na fronteira entre a estrutura declarativa dos componentes React e o documento HTML interpretado pelo navegador. Ele não substitui o React; fornece o mecanismo necessário para que a interface construída com React seja montada e atualizada no DOM.

## 2.9 SPA — Single Page Application

Uma **SPA — Single Page Application** é uma aplicação web estruturada para carregar um documento HTML principal e atualizar a interface dinamicamente no navegador conforme a interação do usuário. Nesse modelo, a navegação entre telas lógicas da aplicação não exige, necessariamente, o carregamento completo de uma nova página HTML a cada mudança de seção.

Em uma aplicação multipágina tradicional, cada navegação tende a solicitar ao servidor um novo documento HTML completo. Em uma SPA, a aplicação já carregada no navegador interpreta a mudança de rota e renderiza a interface correspondente.

Em uma SPA, o arquivo HTML principal funciona como base de montagem da aplicação. A partir dele, o JavaScript carregado no navegador assume a responsabilidade de controlar a interface, responder às interações e renderizar os componentes correspondentes ao estado ou à rota atual.

O conceito de SPA não significa que a aplicação tenha apenas uma tela. Significa que ela possui um documento HTML principal e que as mudanças de tela são tratadas dinamicamente pela aplicação no navegador.

## 2.10 Elemento `root` no HTML

O elemento **`root`** é o ponto de referência definido no HTML para receber a aplicação React. Em projetos criados com Vite, esse elemento normalmente aparece no arquivo `index.html` como uma `div` vazia com o identificador `root`.

```html
<div id="root"></div>
```

Esse elemento não representa uma tela da aplicação e não contém, inicialmente, os componentes React. Sua função é indicar o local do documento HTML em que a aplicação será inserida posteriormente pelo código de inicialização.

No mesmo arquivo `index.html`, também aparece o carregamento do arquivo principal da aplicação:

```html
<script type="module" src="/src/main.tsx"></script>
```

O atributo `id="root"` permite que esse elemento seja localizado pelo código JavaScript/TypeScript:

```tsx
const rootElement = document.getElementById("root");
```

A relação entre os dois trechos deve ser coerente:

```html
<div id="root"></div>
```

```tsx
document.getElementById("root");
```

O valor `"root"` usado no HTML deve ser o mesmo valor usado na chamada de `document.getElementById`. Se o identificador no HTML for alterado, o código de inicialização também precisa ser alterado.

O nome `root` é uma convenção, não uma exigência da linguagem HTML ou do React. O projeto poderia usar outro identificador, como `app`, desde que o mesmo nome fosse usado nos dois pontos.

```html
<div id="app"></div>
```

```tsx
const rootElement = document.getElementById("app");
```

A documentação oficial do React informa que a criação de uma raiz React ocorre sobre um **nó DOM do navegador**. Esse nó DOM é justamente o elemento HTML previamente definido para receber a aplicação. ([react.dev](https://react.dev/reference/react-dom/client/createRoot))

O elemento `root` estabelece a referência inicial entre o HTML estático e o código React carregado a partir do arquivo de entrada da aplicação.

## 2.11 Montagem da aplicação React

A **montagem da aplicação React** corresponde ao processo de inicialização da interface no navegador. Nessa etapa, o código React é associado a um elemento HTML previamente existente e passa a controlar a renderização da interface dentro desse ponto de montagem.

A montagem ocorre no arquivo de entrada da aplicação, normalmente o `main.tsx`. Esse arquivo localiza o elemento HTML definido no `index.html`, cria uma raiz React sobre esse elemento e renderiza a estrutura inicial da aplicação.

Em termos de código, a montagem pode ser representada de forma simplificada assim:

```tsx
const rootElement = document.getElementById("root");

if (!rootElement) {
  throw new Error("Elemento root não encontrado");
}

createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

A primeira instrução localiza o elemento HTML que receberá a aplicação:

```tsx
const rootElement = document.getElementById("root");
```

Essa etapa apenas recupera a referência ao elemento do DOM. A existência e a função do elemento `root` foram tratadas no subtítulo anterior.

Em seguida, ocorre a validação da referência obtida:

```tsx
if (!rootElement) {
  throw new Error("Elemento root não encontrado");
}
```

Essa verificação é necessária porque `document.getElementById` pode retornar `null` quando não encontra o elemento solicitado. Em TypeScript, esse comportamento precisa ser considerado, pois a montagem da aplicação depende de um elemento válido.

Depois da validação, o elemento é entregue ao React DOM:

```tsx
createRoot(rootElement)
```

Nesse ponto, o React DOM cria uma raiz React associada ao elemento HTML. A partir dessa raiz, o React passa a controlar a renderização da interface naquele ponto do documento.

Por fim, a estrutura inicial da aplicação é renderizada:

```tsx
.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

O método `render` recebe a estrutura React que será exibida inicialmente. No exemplo, essa estrutura é composta pelo componente principal `App`, envolvido por `StrictMode`.

A documentação oficial informa que `createRoot` cria uma raiz React para exibir componentes dentro de um nó DOM do navegador e que, depois da criação da raiz, o método `root.render` deve ser usado para exibir um componente React dentro dela. ([react.dev](https://react.dev/reference/react-dom/client/createRoot))

A montagem da aplicação pode ser sintetizada assim:

A montagem não define o conteúdo específico das telas. Ela estabelece o vínculo técnico inicial entre o HTML, o React DOM e a árvore de componentes que será exibida pela aplicação.


## 2.12 `main.tsx`

O arquivo **`main.tsx`** é o ponto de entrada da aplicação React. Ele é carregado a partir do `index.html` e concentra a inicialização da aplicação no navegador.

Em projetos criados com Vite, o `index.html` normalmente carrega o `main.tsx` por meio de um script do tipo módulo:

```html
<script type="module" src="/src/main.tsx"></script>
```

Esse script informa ao navegador que o arquivo `/src/main.tsx` deve ser carregado como módulo JavaScript. A partir desse arquivo, a aplicação React passa a ser inicializada.

Uma estrutura típica de `main.tsx` pode ser representada assim:

```tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";

import "./index.css";
import { App } from "./App";

const rootElement = document.getElementById("root");

if (!rootElement) {
  throw new Error("Elemento root não encontrado");
}

createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

A primeira parte reúne as importações necessárias para a inicialização:

```tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";

import "./index.css";
import { App } from "./App";
```

A importação de `StrictMode` vem da biblioteca React. Esse recurso será tratado em subtítulo próprio, pois sua função está relacionada às verificações realizadas em ambiente de desenvolvimento.

A importação de `createRoot` vem de `react-dom/client`. Esse recurso será detalhado no próximo subtítulo, pois ele é responsável por criar a raiz React no elemento HTML de montagem.

A importação de `./index.css` carrega o CSS global da aplicação. Essa importação será tratada em subtítulo próprio, pois se relaciona à aplicação de estilos globais no projeto.

A importação de `App` representa a estrutura inicial que será renderizada. Em projetos com roteamento, esse ponto pode ser substituído ou envolvido por provedores, como provedores de rotas ou de estado global.

A segunda parte localiza o elemento de montagem no documento HTML:

```tsx
const rootElement = document.getElementById("root");
```

Esse trecho estabelece a ligação entre o elemento `root`, definido no `index.html`, e o código de inicialização da aplicação.

A terceira parte valida a existência do elemento:

```tsx
if (!rootElement) {
  throw new Error("Elemento root não encontrado");
}
```

Essa validação é necessária porque `document.getElementById` pode retornar `null` caso o elemento não exista no documento. A verificação impede que a aplicação tente criar uma raiz React sobre uma referência inexistente.

A quarta parte realiza a montagem da aplicação:

```tsx
createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

Nesse trecho, o elemento HTML validado é entregue ao React DOM, e a estrutura inicial da aplicação é renderizada. A explicação detalhada de `createRoot`, `render` e `StrictMode` será feita nos subtítulos seguintes.

A estrutura funcional do `main.tsx` pode ser sintetizada assim:

```txt
main.tsx
├── importa recursos de inicialização
├── importa o CSS global
├── importa a estrutura inicial da aplicação
├── localiza o elemento de montagem
├── valida a existência desse elemento
└── inicia a renderização da aplicação
```

O `main.tsx` não representa uma tela da aplicação. Sua função é iniciar a aplicação React, conectando o documento HTML, o React DOM, o CSS global e a estrutura principal que será renderizada no navegador.


## 2.13 `createRoot`

A função **`createRoot`** pertence ao pacote `react-dom/client` e é utilizada para criar uma **raiz React** em um elemento do DOM do navegador. Essa raiz é o ponto a partir do qual o React passa a controlar a renderização da interface dentro de um elemento HTML existente.

A documentação oficial define `createRoot` como a API usada para criar uma raiz React para exibir componentes dentro de um nó DOM do navegador. Depois da criação dessa raiz, utiliza-se o método `render` para exibir a estrutura React desejada. ([react.dev](https://react.dev/reference/react-dom/client/createRoot))

A importação é feita da seguinte forma:

```tsx
import { createRoot } from "react-dom/client";
```

O uso básico segue esta estrutura:

```tsx
const rootElement = document.getElementById("root");

if (!rootElement) {
  throw new Error("Elemento root não encontrado");
}

createRoot(rootElement);
```

O argumento recebido por `createRoot` deve ser um elemento real do DOM. No exemplo, esse elemento é obtido por meio de:

```tsx
const rootElement = document.getElementById("root");
```

Essa instrução localiza no `index.html` o elemento com `id="root"`:

```html
<div id="root"></div>
```

A validação do elemento é necessária porque `document.getElementById("root")` pode retornar `null`. Em TypeScript, esse comportamento precisa ser tratado antes da chamada de `createRoot`.

```tsx
if (!rootElement) {
  throw new Error("Elemento root não encontrado");
}
```

Sem essa verificação, haveria o risco de passar um valor nulo para uma função que espera um elemento DOM válido. A validação torna explícita a dependência entre o HTML e o código de inicialização.

A função `createRoot` não renderiza a interface por si só. Ela apenas cria a raiz React. A renderização ocorre em seguida, por meio do método `render`.

```tsx
createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

Também é possível escrever o mesmo processo em duas etapas:

```tsx
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

Essa forma evidencia melhor que `createRoot` produz um objeto de raiz, e que esse objeto possui o método `render`.

A raiz React representa o vínculo entre o DOM do navegador e a árvore de componentes da aplicação. A partir dela, o React consegue gerenciar a renderização da interface, atualizar os componentes quando necessário e manter a correspondência entre a estrutura React e o conteúdo exibido no navegador.

Em versões atuais do React, `createRoot` substitui o padrão antigo baseado em `ReactDOM.render`, utilizado em versões anteriores. O uso de `createRoot` está associado ao modelo moderno de criação de raiz introduzido no React 18 e mantido nas versões atuais da documentação oficial.


## 2.14 `render`

O método **`render`** é utilizado para exibir uma estrutura React dentro de uma raiz criada previamente com `createRoot`. Ele recebe como argumento um elemento React e solicita ao React que apresente essa estrutura no elemento DOM associado à raiz.

A documentação oficial informa que, depois de criar uma raiz com `createRoot`, deve-se chamar `root.render` para exibir um componente React dentro dela. ([react.dev](https://react.dev/reference/react-dom/client/createRoot))

O uso pode aparecer de forma encadeada:

```tsx
createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

A mesma estrutura pode ser escrita em duas etapas:

```tsx
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

Essa segunda forma evidencia que `render` não é uma função isolada importada diretamente. Ele é um método do objeto retornado por `createRoot`.

A estrutura recebida por `render` é escrita em TSX:

```tsx
<StrictMode>
  <App />
</StrictMode>
```

Esse bloco representa a estrutura inicial da aplicação. O componente `<App />` é a interface principal renderizada, enquanto `<StrictMode>` envolve essa estrutura para ativar verificações adicionais em ambiente de desenvolvimento.

O método `render` não cria o elemento HTML de montagem. Esse elemento já existe no `index.html`. Também não cria a raiz React. Essa responsabilidade pertence ao `createRoot`. O papel de `render` é definir o que será exibido dentro da raiz já criada.

Em termos conceituais, `render` estabelece o primeiro conteúdo React visível da aplicação. A partir dessa renderização inicial, o React passa a controlar a atualização da interface conforme componentes, propriedades, estados e rotas mudam.


## 2.15 `StrictMode`

O **`StrictMode`** é um componente do React utilizado para ativar verificações adicionais durante o desenvolvimento da aplicação. Sua função é auxiliar na identificação antecipada de problemas potenciais em componentes, efeitos, referências e uso de APIs depreciadas.

A documentação oficial define `<StrictMode>` como um recurso que habilita comportamentos e avisos adicionais de desenvolvimento para a árvore de componentes envolvida por ele. Essas verificações são executadas apenas em ambiente de desenvolvimento e não afetam o build de produção. ([react.dev](https://react.dev/reference/react/StrictMode))

A importação é feita a partir do pacote `react`:

```tsx
import { StrictMode } from "react";
```

No arquivo de entrada da aplicação, o `StrictMode` normalmente envolve a estrutura inicial renderizada:

```tsx
createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

Nesse trecho, tudo o que estiver dentro de `<StrictMode>` passa a ser submetido às verificações adicionais do React durante o desenvolvimento.

O `StrictMode` não renderiza nenhum elemento visual na interface. Ele atua como um componente auxiliar de verificação. Ao inspecionar a página no navegador, não haverá uma tag HTML correspondente ao `StrictMode`.

Entre os comportamentos adicionais ativados em desenvolvimento, os componentes podem ser renderizados mais de uma vez para detectar problemas causados por renderização impura. Efeitos também podem ser executados novamente para verificar se possuem limpeza adequada.

Esse comportamento explica uma situação comum em projetos React: durante o desenvolvimento, algumas mensagens no console ou alguns efeitos podem parecer ocorrer mais de uma vez. Isso não significa, necessariamente, erro na aplicação. Em muitos casos, trata-se de uma verificação intencional do `StrictMode`.

Exemplo simples:

```tsx
function Example() {
  console.log("Renderizando componente");

  return <p>Exemplo</p>;
}
```

Se esse componente estiver dentro de `<StrictMode>`, a mensagem no console pode aparecer mais de uma vez em ambiente de desenvolvimento. Esse comportamento ajuda a identificar componentes que executam efeitos colaterais durante a renderização.

A renderização de um componente deve ser pura. Isso significa que o componente deve calcular e retornar a interface esperada sem modificar variáveis externas, alterar dados compartilhados ou executar operações com efeitos colaterais diretamente no corpo da função.

Exemplo inadequado:

```tsx
let counter = 0;

function CounterLabel() {
  counter = counter + 1;

  return <p>Renderização número {counter}</p>;
}
```

Nesse exemplo, o componente modifica uma variável externa durante a renderização. Esse tipo de comportamento torna a renderização impura, pois o resultado passa a depender de uma alteração externa ao fluxo normal de propriedades e estado.

Forma mais adequada:

```tsx
function CounterLabel() {
  return <p>Componente renderizado</p>;
}
```

Nesse segundo exemplo, o componente apenas retorna a interface. Ele não modifica dados externos durante a renderização.

O `StrictMode` também contribui para identificar problemas relacionados a efeitos. Quando um efeito cria uma inscrição, temporizador, conexão ou escutador de evento, ele deve fornecer uma função de limpeza.

Exemplo com limpeza:

```tsx
import { useEffect } from "react";

function WindowSizeLogger() {
  useEffect(() => {
    function handleResize() {
      console.log("Janela redimensionada");
    }

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return <p>Monitorando redimensionamento da janela</p>;
}
```

Nesse exemplo, o evento `resize` é registrado quando o componente é montado e removido quando o componente é desmontado. A função retornada pelo `useEffect` realiza a limpeza necessária.

O `StrictMode` pode envolver a aplicação inteira ou apenas uma parte da árvore de componentes.

```tsx
function App() {
  return (
    <>
      <Header />

      <StrictMode>
        <MainContent />
      </StrictMode>

      <Footer />
    </>
  );
}
```

Nesse caso, as verificações adicionais seriam aplicadas aos componentes dentro de `<MainContent />`, mas não aos componentes `Header` e `Footer`.

O `StrictMode` deve ser entendido como um mecanismo de diagnóstico. Ele não substitui testes, não corrige automaticamente problemas no código e não define a estrutura visual da aplicação. Sua finalidade é expor comportamentos que podem gerar erros em componentes React, principalmente quando há efeitos colaterais, ausência de limpeza ou dependência de APIs antigas.

## 2.9 `StrictMode`

O **`StrictMode`** é um componente do React utilizado para ativar verificações adicionais durante o desenvolvimento da aplicação. Sua função é auxiliar na identificação antecipada de problemas potenciais em componentes, efeitos, referências e uso de APIs depreciadas.

A documentação oficial define `<StrictMode>` como um recurso que habilita comportamentos e avisos adicionais de desenvolvimento para a árvore de componentes envolvida por ele. Essas verificações são executadas apenas em ambiente de desenvolvimento e não afetam o build de produção. ([React][1])

A importação é feita a partir do pacote `react`:

```tsx
import { StrictMode } from "react";
```

No arquivo `main.tsx`, o `StrictMode` envolve a estrutura inicial da aplicação:

```tsx
createRoot(rootElement).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>
);
```

Nesse trecho, tudo o que estiver dentro de `<StrictMode>` passa a ser submetido às verificações adicionais do React durante o desenvolvimento.

O `StrictMode` não renderiza nenhum elemento visual na interface. Ele atua como um componente auxiliar de verificação. Portanto, ao inspecionar a página no navegador, não haverá uma tag HTML correspondente ao `StrictMode`.

Entre os comportamentos adicionais ativados em desenvolvimento, a documentação oficial destaca que os componentes podem ser renderizados uma vez a mais para detectar problemas causados por renderização impura; os efeitos podem ser executados novamente para detectar ausência de limpeza; callbacks de referência também podem ser executados novamente para detectar ausência de limpeza; e o React pode verificar o uso de APIs depreciadas. ([React][1])

Essas verificações explicam um comportamento comum observado em projetos React: durante o desenvolvimento, alguns trechos podem parecer executar mais de uma vez. Isso não significa, necessariamente, erro na aplicação. Em muitos casos, trata-se de uma verificação intencional do `StrictMode`.

Exemplo simplificado:

```tsx
function Example() {
  console.log("Renderizando componente");

  return <p>Exemplo</p>;
}
```

Se esse componente estiver dentro de `<StrictMode>`, a mensagem no console pode aparecer mais de uma vez em ambiente de desenvolvimento. Esse comportamento auxilia na identificação de código que produz efeitos colaterais durante a renderização.

A renderização de um componente deve ser pura. Isso significa que o componente deve calcular e retornar a interface esperada sem modificar variáveis externas, alterar dados compartilhados ou executar operações com efeitos colaterais diretamente no corpo da função.

Exemplo inadequado:

```tsx
let counter = 0;

function CounterLabel() {
  counter = counter + 1;

  return <p>Renderização número {counter}</p>;
}
```

Nesse exemplo, o componente modifica uma variável externa durante a renderização. Esse tipo de comportamento torna a renderização impura, pois o resultado passa a depender de uma alteração externa ao fluxo normal de propriedades e estado.

Forma mais adequada:

```tsx
function CounterLabel() {
  return <p>Componente renderizado</p>;
}
```

Nesse segundo exemplo, o componente apenas retorna a interface. Ele não modifica dados externos durante a renderização.

O `StrictMode` também ajuda a identificar problemas relacionados a efeitos. Em React, efeitos normalmente são definidos com `useEffect`. Quando um efeito cria uma inscrição, temporizador, conexão ou escutador de evento, ele deve fornecer uma função de limpeza. A documentação oficial informa que, em desenvolvimento, o `StrictMode` pode executar efeitos uma vez a mais para detectar problemas de limpeza ausente. ([React][1])

Exemplo com limpeza:

```tsx
import { useEffect } from "react";

function WindowSizeLogger() {
  useEffect(() => {
    function handleResize() {
      console.log("Janela redimensionada");
    }

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return <p>Monitorando redimensionamento da janela</p>;
}
```

Nesse exemplo, o evento `resize` é registrado quando o componente é montado e removido quando o componente é desmontado. A função retornada pelo `useEffect` realiza a limpeza necessária.

O `StrictMode` pode envolver a aplicação inteira ou apenas uma parte da árvore de componentes. No `main.tsx`, ele envolve toda a estrutura renderizada:

```tsx
<StrictMode>
  <RouterProvider router={router} />
</StrictMode>
```

Também seria possível aplicá-lo apenas a uma parte da interface:

```tsx
function App() {
  return (
    <>
      <Header />

      <StrictMode>
        <MainContent />
      </StrictMode>

      <Footer />
    </>
  );
}
```

Nesse caso, as verificações adicionais seriam aplicadas aos componentes dentro de `<MainContent />`, mas não aos componentes `Header` e `Footer`.

No projeto, o uso no arquivo `main.tsx` indica que a aplicação inteira foi envolvida por `StrictMode`. Essa é uma prática recomendada especialmente em aplicações novas, pois permite que problemas sejam identificados durante o desenvolvimento, antes da geração da versão de produção. A documentação oficial recomenda envolver a aplicação inteira em `StrictMode`, especialmente em aplicações recém-criadas. ([React][1])

O `StrictMode` deve ser entendido como um mecanismo de diagnóstico. Ele não substitui testes, não corrige automaticamente problemas no código e não define a estrutura visual da aplicação. Sua finalidade é expor comportamentos que podem gerar erros em componentes React, principalmente quando há efeitos colaterais, ausência de limpeza ou dependência de APIs antigas.

[1]: https://react.dev/reference/react/StrictMode?utm_source=chatgpt.com "<StrictMode> – React"

## 2.16 Importação do CSS global

A **importação do CSS global** corresponde ao carregamento do arquivo de estilos que define regras visuais aplicáveis à aplicação como um todo. Em projetos React com Vite, essa importação normalmente ocorre no arquivo de entrada da aplicação, como o `main.tsx`.

No arquivo `main.tsx`, a importação aparece da seguinte forma:

```tsx
import "./index.css";
```

Esse comando não importa uma função, uma constante ou um componente. Ele importa um arquivo CSS para que suas regras sejam processadas pelo ambiente de desenvolvimento e aplicadas à interface renderizada no navegador.

A documentação oficial do Vite informa que arquivos CSS podem ser importados diretamente a partir de arquivos JavaScript. O conteúdo importado é inserido na página e também participa do processo de atualização durante o desenvolvimento. ([vite.dev](https://vite.dev/guide/features.html#css))

O arquivo `index.css` é chamado de global porque suas regras podem afetar elementos, classes e estruturas presentes em diferentes partes da aplicação. Regras aplicadas ao `body`, a links, botões, classes utilitárias ou variáveis CSS ficam disponíveis para toda a interface.

Exemplo simplificado:

```css
body {
  margin: 0;
  font-family: system-ui, sans-serif;
}

button {
  font: inherit;
}
```

Essas regras não pertencem a um componente específico. Elas definem comportamentos visuais gerais da aplicação.

A importação no `main.tsx` garante que o CSS global seja carregado desde o início da aplicação, antes da renderização visual das telas. Como o `main.tsx` é o ponto de entrada do projeto, todo estilo importado nesse arquivo passa a fazer parte da base visual comum da aplicação.

A importação do CSS global não renderiza conteúdo na tela. Ela apenas disponibiliza regras visuais que serão aplicadas aos elementos renderizados pelos componentes React.
