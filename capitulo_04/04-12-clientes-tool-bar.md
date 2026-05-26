## `src/components/ClientesToolbar/ClientesToolbar.tsx`

O arquivo `src/components/ClientesToolbar/ClientesToolbar.tsx` define o componente responsável pela área de ferramentas da página de clientes. No estado atual da aplicação, essa área concentra a barra de busca, mas sua separação permite que outros elementos de controle sejam adicionados posteriormente, como filtros, ordenação ou ações complementares.

Esse componente atua como uma camada intermediária entre a página e o componente reutilizável `SearchBar`. Sua função não é executar a busca nem filtrar diretamente os clientes. A responsabilidade do `ClientesToolbar` é organizar a região visual em que o campo de busca será exibido e repassar ao `SearchBar` os dados necessários para seu funcionamento.

A interface `ClientesToolbarProps` define as propriedades recebidas pelo componente.

```tsx
interface ClientesToolbarProps {
  valorBusca: string;
  onChangeBusca: (valor: string) => void;
}
```

A propriedade `valorBusca` representa o texto atual da busca. A propriedade `onChangeBusca` representa a função responsável por atualizar esse texto no componente superior. Essa organização mantém o estado da busca fora do `ClientesToolbar`, pois ele não deve ser responsável por armazenar o valor digitado.

O componente recebe essas propriedades por desestruturação.

```tsx
export function ClientesToolbar({
  valorBusca,
  onChangeBusca,
}: Readonly<ClientesToolbarProps>) {
```

A utilização de `Readonly<ClientesToolbarProps>` indica que as propriedades recebidas são somente leitura. O componente apenas repassa essas informações ao componente filho, sem alterar diretamente os valores recebidos.

A estrutura visual é composta por uma `div` que envolve o componente `SearchBar`.

```tsx
return (
  <div className="clientes-toolbar full-width">
    <SearchBar valorBusca={valorBusca} onChangeBusca={onChangeBusca} />
  </div>
);
```

A classe `clientes-toolbar` identifica a área da barra de ferramentas. A classe utilitária `full-width` garante que o componente ocupe toda a largura disponível, sem a necessidade de repetir a regra `width: 100%` em um arquivo CSS específico.

O componente `SearchBar` recebe exatamente as mesmas propriedades.

```tsx
<SearchBar valorBusca={valorBusca} onChangeBusca={onChangeBusca} />
```

Essa passagem direta de propriedades mantém o fluxo unidirecional dos dados. O valor da busca vem da página, passa pelo `ClientesToolbar` e chega ao `SearchBar`. Quando o usuário digita no campo, o `SearchBar` chama `onChangeBusca`, e a alteração retorna ao componente que controla o estado.

Assim, `ClientesToolbar.tsx` organiza a área de ferramentas da página sem assumir responsabilidades que pertencem a outras partes do sistema. Ele não controla estado, não filtra a lista e não conhece a regra de busca. Sua função é estruturar visualmente a região da busca e conectar a página ao componente reutilizável `SearchBar`.
