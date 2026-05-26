## `src/hooks/useClientes.ts`

Hooks são funções utilizadas no React para acessar recursos da biblioteca, como estado, efeitos, contexto e outros mecanismos associados ao ciclo de funcionamento dos componentes. A documentação oficial do React define os hooks como recursos que permitem usar funcionalidades do React em componentes, podendo ser hooks nativos ou hooks construídos pelo próprio desenvolvedor a partir da combinação desses recursos. ([React][1])

Nesse contexto, `useClientes.ts` define um hook personalizado. Um hook personalizado é uma função cujo nome começa com `use` e cuja finalidade é organizar uma lógica específica que pode envolver estado, efeitos e chamadas a outras funções. A documentação oficial do React indica que hooks personalizados permitem compartilhar lógica com estado, mas não compartilham automaticamente o estado entre diferentes chamadas. Cada uso do hook possui sua própria execução. ([React][2])

O hook `useClientes` concentra a lógica de dados dos clientes. Sua responsabilidade é controlar a lista de clientes, o estado de carregamento, a mensagem de erro e as operações de cadastro, edição e exclusão. Assim, a página não precisa manipular diretamente a API nem repetir a lógica de atualização dos dados após cada operação.

O primeiro conceito aplicado no arquivo é o `useState`. O `useState` é um hook nativo do React usado para declarar uma variável de estado. Segundo a documentação oficial, ele retorna dois valores: o estado atual e uma função para atualizar esse estado. Quando a função de atualização é chamada, o React agenda uma nova renderização com o valor atualizado. ([React][3])

No arquivo, o primeiro estado declarado é `clientes`.

```ts
const [clientes, setClientes] = useState<Cliente[]>([]);
```

Esse trecho cria um estado chamado `clientes` e uma função chamada `setClientes`. O estado `clientes` armazena o valor atual. A função `setClientes` atualiza esse valor. A expressão `useState<Cliente[]>([])` informa ao TypeScript que esse estado deve armazenar um vetor de objetos do tipo `Cliente`. O valor inicial é `[]`, ou seja, uma lista vazia.

A mesma lógica é aplicada aos estados `carregando` e `mensagemErro`.

```ts
const [carregando, setCarregando] = useState(false);
const [mensagemErro, setMensagemErro] = useState("");
```

O estado `carregando` representa uma informação booleana. Quando seu valor é `true`, indica que uma operação de carga está em andamento. Quando seu valor é `false`, indica que a operação terminou ou ainda não foi iniciada. O estado `mensagemErro` representa uma cadeia de caracteres. Quando está vazio, não há erro a ser exibido. Quando recebe uma mensagem, essa informação pode ser apresentada na interface.

A função `carregarClientes` organiza a busca dos dados na API.

```ts
async function carregarClientes() {
  try {
    setCarregando(true);
    setMensagemErro("");

    const clientesCarregados = await listarClientes();
    setClientes(clientesCarregados);
  } catch {
    setMensagemErro("Não foi possível carregar os clientes.");
  } finally {
    setCarregando(false);
  }
}
```

Essa função é assíncrona porque depende de uma operação externa: a chamada à API realizada por `listarClientes`. Inicialmente, o estado `carregando` é definido como `true`, indicando que a carga começou. Em seguida, a mensagem de erro é limpa. Caso a API retorne os dados corretamente, `setClientes(clientesCarregados)` atualiza a lista de clientes. Caso ocorra erro, `setMensagemErro` registra a falha. Por fim, o bloco `finally` define `carregando` como `false`, independentemente de sucesso ou falha.

O segundo conceito aplicado é o `useEffect`. O `useEffect` é um hook nativo usado para sincronizar um componente ou hook com algum sistema externo ao React, como uma API, um temporizador, um evento do navegador ou outro recurso não controlado diretamente pela renderização. ([React][4])

No arquivo, o `useEffect` é usado para executar a carga inicial dos clientes.

```ts
useEffect(() => {
  carregarClientes();
}, []);
```

Esse trecho executa `carregarClientes` quando o hook é utilizado pela primeira vez. O vetor vazio `[]` indica que o efeito não depende de outros valores reativos para ser repetido. Assim, a busca inicial ocorre apenas na montagem do componente que utiliza o hook.

As funções `salvarNovoCliente`, `salvarEdicaoCliente` e `removerCliente` representam as operações de persistência dos dados.

```ts
async function salvarNovoCliente(dadosCliente: ClienteFormData) {
  try {
    setMensagemErro("");

    await cadastrarCliente(dadosCliente);
    await carregarClientes();
  } catch {
    setMensagemErro("Não foi possível cadastrar o cliente.");
    throw new Error("Erro ao cadastrar cliente.");
  }
}
```

A função `salvarNovoCliente` recebe os dados do formulário, limpa mensagens de erro anteriores, chama `cadastrarCliente` no serviço de API e, após a inclusão, executa novamente `carregarClientes`. Essa recarga mantém a lista exibida sincronizada com os dados persistidos.

```ts
async function salvarEdicaoCliente(id: string, dadosCliente: ClienteFormData) {
  try {
    setMensagemErro("");

    await atualizarCliente(id, dadosCliente);
    await carregarClientes();
  } catch {
    setMensagemErro("Não foi possível atualizar o cliente.");
    throw new Error("Erro ao atualizar cliente.");
  }
}
```

A função `salvarEdicaoCliente` recebe o `id` do cliente e os dados atualizados do formulário. O identificador define qual registro será alterado. Após a atualização, a lista também é recarregada, mantendo o mesmo padrão de sincronização usado no cadastro.

```ts
async function removerCliente(id: string) {
  try {
    setMensagemErro("");

    await excluirCliente(id);
    await carregarClientes();
  } catch {
    setMensagemErro("Não foi possível excluir o cliente.");
  }
}
```

A função `removerCliente` recebe o `id` do cliente e executa a exclusão por meio do serviço. Essa função não solicita confirmação ao usuário, pois essa responsabilidade foi transferida para um modal genérico da interface. Dessa forma, o hook permanece restrito à lógica de dados.

Ao final, o hook retorna os estados e funções necessários para a página.

```ts
return {
  clientes,
  carregando,
  mensagemErro,
  salvarNovoCliente,
  salvarEdicaoCliente,
  removerCliente,
};
```

Esse retorno define o contrato de uso do hook. A página que utiliza `useClientes` passa a receber os dados, o estado de carregamento, a mensagem de erro e as funções de persistência, sem precisar conhecer os detalhes internos da comunicação com a API.

Dessa forma, `useClientes.ts` atua como uma camada de controle dos dados da aplicação. Ele não renderiza elementos visuais, não controla campos de formulário e não define a estrutura da tela. Sua responsabilidade é manter o ciclo de vida dos dados de clientes, articulando estado local, chamadas ao serviço e atualização da lista após cada operação.

[1]: https://react.dev/reference/react/hooks "Built-in React Hooks"
[2]: https://react.dev/learn/reusing-logic-with-custom-hooks "Reusing Logic with Custom Hooks"
[3]: https://react.dev/reference/react/useState "useState"
[4]: https://react.dev/reference/react/useEffect "useEffect"
