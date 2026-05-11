## `StrictMode`

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

Nesse caso, as verificações adicionais seriam aplicadas aos componentes dentro de `<MainContent/>`, mas não aos componentes `Header` e `Footer`.

O `StrictMode` deve ser entendido como um mecanismo de diagnóstico. Ele não substitui testes, não corrige automaticamente problemas no código e não define a estrutura visual da aplicação. Sua finalidade é expor comportamentos que podem gerar erros em componentes React, principalmente quando há efeitos colaterais, ausência de limpeza ou dependência de APIs antigas.
