## `src/components/ClientesContent/ClientesContent.tsx`

O arquivo `src/components/ClientesContent/ClientesContent.tsx` define o componente responsável pela área principal de conteúdo da página de clientes. Sua função é decidir qual conteúdo deve ser apresentado ao usuário considerando dois estados possíveis: o carregamento dos dados ou a exibição da lista de clientes.

Esse componente aplica o conceito de renderização condicional. A renderização condicional ocorre quando a interface apresentada depende de uma condição lógica. Nesse caso, a condição principal é o valor de `carregando`. Quando a aplicação está buscando os dados, deve ser apresentada uma mensagem de carregamento. Quando a carga termina, deve ser apresentada a lista de clientes.

A interface `ClientesContentProps` define as propriedades necessárias para o funcionamento do componente.

```tsx
interface ClientesContentProps {
  carregando: boolean;
  clientes: Cliente[];
  onEditar: (cliente: Cliente) => void;
  onExcluir: (id: string) => void;
}
```

A propriedade `carregando` indica se os dados ainda estão sendo buscados. A propriedade `clientes` contém a lista que será exibida. As propriedades `onEditar` e `onExcluir` representam as funções encaminhadas para os componentes da lista, permitindo que as ações de edição e exclusão continuem sendo controladas pela página.

O componente recebe essas propriedades por desestruturação.

```tsx
export function ClientesContent({
  carregando,
  clientes,
  onEditar,
  onExcluir,
}: Readonly<ClientesContentProps>) {
```

A utilização de `Readonly<ClientesContentProps>` mantém o mesmo padrão adotado nos demais componentes. As propriedades recebidas são utilizadas apenas para leitura e encaminhamento, sem alteração interna.

A primeira decisão do componente ocorre quando `carregando` é verdadeiro.

```tsx
if (carregando) {
  return <div className="empty-state surface">Carregando clientes...</div>;
}
```

Esse trecho indica que, enquanto os dados estão sendo carregados, a lista não deve ser renderizada. Em seu lugar, é exibida uma mensagem informando que a operação está em andamento. As classes `empty-state` e `surface` reutilizam padrões visuais já definidos no CSS global, evitando a criação de regras específicas para esse caso.

Quando `carregando` é falso, o componente renderiza a lista de clientes.

```tsx
return <ClienteList clientes={clientes} onEditar={onEditar} onExcluir={onExcluir} />;
```

Nesse trecho, `ClientesContent` encaminha para `ClienteList` a lista de clientes e as funções de edição e exclusão. A responsabilidade de percorrer o vetor e renderizar cada item não pertence a `ClientesContent`, mas sim a `ClienteList`.

Assim, `ClientesContent.tsx` atua como um componente de decisão visual. Ele não carrega dados, não filtra clientes, não abre modais e não executa operações de persistência. Sua responsabilidade é organizar a área principal da página, escolhendo entre a apresentação do estado de carregamento e a apresentação da lista de clientes.
