## `src/components/SearchBar/SearchBar.tsx`

Componentes em React são unidades de construção da interface. Eles combinam marcação, estilo e lógica de apresentação em partes reutilizáveis da aplicação. A documentação oficial do React define um componente como uma parte da interface que possui sua própria lógica e aparência, podendo representar desde um botão até uma página completa. ([React][1])

A comunicação entre componentes ocorre por meio de `props`. As `props` são informações passadas de um componente pai para um componente filho. Elas permitem configurar o comportamento e a aparência de um componente sem que ele precise conhecer diretamente o estado do componente que o utiliza. A documentação oficial do React indica que componentes usam `props` para comunicar dados, funções, objetos, vetores e outros valores JavaScript. ([React][2])

Nesse contexto, o arquivo `src/components/SearchBar/SearchBar.tsx` define um componente visual responsável por exibir o campo de busca de clientes. Sua responsabilidade é restrita à interface de entrada da busca. Ele não filtra a lista, não armazena estado próprio e não conhece a origem dos dados. Sua função é receber o valor atual da busca e informar ao componente pai quando esse valor for alterado.

A interface `SearchBarProps` define os parâmetros esperados pelo componente.

```tsx
interface SearchBarProps {
  valorBusca: string;
  onChangeBusca: (valor: string) => void;
}
```

O campo `valorBusca` representa o texto atualmente digitado no campo de busca. O campo `onChangeBusca` representa uma função recebida por `props`, responsável por comunicar ao componente pai que o conteúdo do campo foi alterado. Essa função recebe uma `string`, pois o valor digitado no campo de texto é tratado como cadeia de caracteres.

O componente `SearchBar` recebe essas propriedades por desestruturação.

```tsx
export function SearchBar({ valorBusca, onChangeBusca }: Readonly<SearchBarProps>) {
```

A tipagem `Readonly<SearchBarProps>` indica que as propriedades recebidas não devem ser modificadas internamente pelo componente. Essa prática reforça a ideia de que `props` são dados de entrada. O componente pode utilizá-las para renderizar a interface e acionar callbacks, mas não deve alterar diretamente o objeto recebido.

A estrutura visual é composta por uma `div`, um ícone e um campo `input`.

```tsx
return (
  <div className="search-bar surface row gap-md">
    <FaSearch className="search-bar__icon" />

    <input
      className="search-bar__input"
      type="text"
      placeholder="Buscar cliente por nome, e-mail ou telefone"
      value={valorBusca}
      onChange={(event) => onChangeBusca(event.target.value)}
    />
  </div>
);
```

A `div` principal utiliza classes de estilo específicas e utilitárias. A classe `search-bar` identifica o componente. A classe `surface` aplica o padrão visual de superfície definido no CSS global. As classes `row` e `gap-md` organizam os elementos em linha, com espaçamento padronizado entre eles.

O componente `FaSearch` exibe o ícone de busca. Ele possui função visual e reforça a finalidade do campo. O comportamento da busca, contudo, não está no ícone, mas no campo `input` e na função `onChangeBusca`.

O `input` é um campo controlado. Em React, um campo controlado recebe seu valor por meio de uma propriedade e comunica alterações por meio de um evento. Nesse caso, o valor exibido no campo vem de `valorBusca`, e cada alteração chama `onChangeBusca(event.target.value)`.

```tsx
value={valorBusca}
onChange={(event) => onChangeBusca(event.target.value)}
```

Essa estrutura indica que o estado da busca não pertence ao `SearchBar`. O estado está no componente pai, enquanto o `SearchBar` apenas apresenta o valor e encaminha o novo conteúdo digitado. Dessa forma, o componente permanece simples, reutilizável e orientado por `props`.

Assim, `SearchBar.tsx` representa um componente de entrada visual. Ele não possui regra de filtragem, não manipula lista de clientes e não realiza comunicação com a API. Sua responsabilidade é apresentar o campo de busca e repassar ao componente superior o valor informado pelo usuário.

[1]: https://react.dev/learn "Quick Start"
[2]: https://react.dev/learn/passing-props-to-a-component "Passing Props to a Component"
