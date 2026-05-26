## `src/components/ClienteList/ClienteList.tsx`

O arquivo `src/components/ClienteList/ClienteList.tsx` define o componente responsável pela apresentação da coleção de clientes. Trata-se de um componente de listagem, cuja função é receber um conjunto de dados já preparado e transformar esse conjunto em elementos visuais renderizados na interface.

Neste ponto aparece um conceito importante do React: a renderização de listas. Em React, listas são normalmente produzidas a partir de estruturas como vetores, utilizando métodos JavaScript como `map`. Cada item do vetor é transformado em um componente ou elemento visual. Assim, uma lista de objetos pode ser convertida em uma lista de componentes renderizados na tela.

O componente recebe três propriedades.

```tsx
interface ClienteListProps {
  clientes: Cliente[];
  onEditar: (cliente: Cliente) => void;
  onExcluir: (id: string) => void;
}
```

A propriedade `clientes` representa o vetor de clientes que será exibido. A propriedade `onEditar` representa a função chamada quando o usuário solicita a edição de um cliente. A propriedade `onExcluir` representa a função chamada quando o usuário solicita a exclusão de um cliente. Dessa forma, o componente não executa diretamente a edição ou a exclusão; ele apenas encaminha essas ações para o componente superior.

A função do componente é definida a partir dessas propriedades.

```tsx
export function ClienteList({
  clientes,
  onEditar,
  onExcluir,
}: Readonly<ClienteListProps>) {
```

A utilização de `Readonly<ClienteListProps>` reforça que as propriedades recebidas são somente leitura. O componente utiliza os dados para renderização e os callbacks para comunicação, mas não altera diretamente as propriedades recebidas.

Antes de renderizar a lista, o componente verifica se o vetor de clientes está vazio.

```tsx
if (clientes.length === 0) {
  return (
    <div className="empty-state surface">
      Nenhum cliente encontrado.
    </div>
  );
}
```

Essa verificação evita que uma área vazia seja apresentada sem explicação. Quando não há clientes a exibir, o componente retorna uma mensagem de estado vazio. As classes `empty-state` e `surface` indicam que esse padrão visual já está centralizado no CSS utilitário, evitando a criação de estilos específicos desnecessários.

Quando existem clientes, o componente renderiza uma seção com a lista.

```tsx
return (
  <section className="cliente-list stack stack--md">
    {clientes.map((cliente) => (
      <ClienteItem
        key={cliente.id}
        cliente={cliente}
        onEditar={onEditar}
        onExcluir={onExcluir}
      />
    ))}
  </section>
);
```

A classe `cliente-list` identifica o componente. As classes `stack` e `stack--md` organizam os itens verticalmente com espaçamento padronizado. Isso mantém a estrutura visual da lista sem repetir regras de espaçamento no CSS específico do componente.

O método `map` percorre o vetor `clientes` e cria um componente `ClienteItem` para cada cliente encontrado. Cada item recebe o objeto `cliente`, além das funções `onEditar` e `onExcluir`.

O atributo `key` possui papel específico na renderização de listas em React.

```tsx
key={cliente.id}
```

Esse atributo permite que o React identifique cada item da lista de forma estável durante as renderizações. Como cada cliente possui um `id`, esse identificador é utilizado como chave. Essa prática evita ambiguidades quando a lista é atualizada, filtrada ou reorganizada.

Assim, `ClienteList.tsx` atua como componente intermediário entre a coleção de dados e o item individual da lista. Ele não carrega clientes, não filtra dados, não controla estado e não realiza operações de persistência. Sua responsabilidade é verificar se há dados a exibir e, quando houver, transformar cada cliente em um componente `ClienteItem`.
