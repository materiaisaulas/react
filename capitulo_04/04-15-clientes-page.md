## `src/pages/ClientesPage/ClientesPage.tsx`

O arquivo `src/pages/ClientesPage/ClientesPage.tsx` define a página principal do cadastro de clientes. Trata-se do componente responsável por coordenar os dados, os estados de interação e os componentes visuais que compõem a tela.

Em uma aplicação React, uma página representa uma unidade maior da interface. Diferentemente de componentes menores, que normalmente possuem responsabilidades específicas, uma página atua como ponto de composição. Ela conecta hooks, componentes reutilizáveis, componentes de apresentação e ações do usuário. Dessa forma, a página não deve concentrar toda a lógica do sistema, mas deve organizar a comunicação entre as partes.

No projeto, `ClientesPage` assume essa função de coordenação. Ela utiliza hooks para obter dados e controlar interações, renderiza os componentes da interface e define como as ações de cadastro, edição e exclusão serão encaminhadas.

O componente inicia utilizando o hook `useClientes`.

```tsx
const {
  clientes,
  carregando,
  mensagemErro,
  salvarNovoCliente,
  salvarEdicaoCliente,
  removerCliente,
} = useClientes();
```

Esse trecho obtém os dados e as operações relacionadas aos clientes. O hook fornece a lista de clientes, o estado de carregamento, a mensagem de erro e as funções de persistência. Assim, a página não acessa diretamente o `clienteService.ts`, mantendo a comunicação com a API isolada na camada própria.

Em seguida, a página declara o estado da busca.

```tsx
const [valorBusca, setValorBusca] = useState("");
```

O estado `valorBusca` armazena o texto digitado no campo de busca. A função `setValorBusca` atualiza esse valor. Esse estado permanece na página porque a busca afeta a lista exibida e precisa ser compartilhada entre a barra de ferramentas e o hook que calcula os clientes filtrados.

O controle do modal de formulário é feito por meio do hook `useClienteModal`.

```tsx
const {
  modalAberto,
  clienteSelecionado,
  abrirModalCadastro,
  abrirModalEdicao,
  fecharModal,
} = useClienteModal();
```

Esse trecho concentra as informações relacionadas ao modal de cadastro e edição. `modalAberto` indica se o formulário deve ser exibido. `clienteSelecionado` indica se a operação é de edição ou cadastro. As funções `abrirModalCadastro`, `abrirModalEdicao` e `fecharModal` organizam as transições de estado do modal.

A lista exibida na tela é obtida por meio do hook `useClientesFiltrados`.

```tsx
const clientesFiltrados = useClientesFiltrados(clientes, valorBusca);
```

Esse hook recebe a lista original de clientes e o texto da busca. O resultado é uma lista derivada, contendo apenas os clientes compatíveis com o critério informado. Assim, a página mantém separada a lista carregada da API e a lista efetivamente exibida na interface.

A função `salvarCliente` define a decisão entre cadastro e edição.

```tsx
async function salvarCliente(dadosCliente: ClienteFormData) {
  if (clienteSelecionado) {
    await salvarEdicaoCliente(clienteSelecionado.id, dadosCliente);
  } else {
    await salvarNovoCliente(dadosCliente);
  }

  fecharModal();
}
```

Essa função recebe os dados vindos do formulário. Quando existe `clienteSelecionado`, a operação corresponde à edição de um registro já existente. Nesse caso, o identificador do cliente é usado junto com os dados atualizados. Quando não existe cliente selecionado, a operação corresponde ao cadastro de um novo cliente. Após a conclusão da operação, o modal é fechado.

Essa decisão permanece na página porque ela conhece o contexto da operação. O `ClienteFormModal` apenas coleta os dados do formulário. O hook `useClientes` apenas executa as operações de persistência. A página articula essas duas responsabilidades.

A página também controla a confirmação de exclusão por meio do modal genérico do sistema. Para isso, mantém um estado com o cliente que será removido.

```tsx
const [clienteIdParaExcluir, setClienteIdParaExcluir] = useState<string | null>(null);
```

Esse estado indica se existe uma exclusão pendente de confirmação. Quando seu valor é `null`, nenhum modal de confirmação deve ser exibido. Quando possui um `id`, o modal de confirmação é aberto para aquele cliente.

A solicitação de exclusão não remove imediatamente o registro. Ela apenas registra o cliente que será confirmado.

```tsx
function solicitarExclusaoCliente(id: string) {
  setClienteIdParaExcluir(id);
}
```

Essa função é chamada quando o usuário clica no botão de exclusão em um item da lista. A exclusão efetiva ainda não ocorre nesse momento, pois a ação depende da confirmação do usuário.

O cancelamento da exclusão limpa o estado de confirmação.

```tsx
function cancelarExclusaoCliente() {
  setClienteIdParaExcluir(null);
}
```

Quando o usuário cancela a ação, o estado volta a ser `null` e o modal deixa de ser exibido.

A confirmação executa a remoção e, em seguida, fecha o modal.

```tsx
async function confirmarExclusaoCliente() {
  if (!clienteIdParaExcluir) {
    return;
  }

  await removerCliente(clienteIdParaExcluir);
  setClienteIdParaExcluir(null);
}
```

A verificação inicial impede a execução da exclusão caso não exista um cliente definido. Em seguida, a função `removerCliente`, fornecida por `useClientes`, remove o registro da API e atualiza a lista. Ao final, o estado de confirmação é limpo.

A estrutura visual da página começa pelo elemento principal da tela de clientes.

```tsx
return (
  <section className="clientes-page stack stack--lg">
```

A classe `clientes-page` identifica a página. As classes `stack` e `stack--lg` organizam os blocos principais verticalmente, com espaçamento padronizado. Isso evita a repetição de margens entre os componentes internos.

O cabeçalho é renderizado pelo componente `ClientesHeader`.

```tsx
<ClientesHeader onNovoCliente={abrirModalCadastro} />
```

Esse componente recebe a função que abre o modal em modo de cadastro. O cabeçalho não controla o estado do modal; apenas encaminha a ação para a página.

A área de ferramentas é renderizada por `ClientesToolbar`.

```tsx
<ClientesToolbar valorBusca={valorBusca} onChangeBusca={setValorBusca} />
```

A página passa o valor atual da busca e a função de atualização. Assim, o campo de busca continua sendo controlado pela página, enquanto a apresentação visual permanece no componente de toolbar.

A mensagem de erro é renderizada por `ClientesFeedback`.

```tsx
<ClientesFeedback mensagemErro={mensagemErro} />
```

Esse componente recebe a mensagem produzida pelo hook `useClientes`. Caso a mensagem esteja vazia, nada é renderizado. Caso exista erro, a mensagem é exibida na interface.

O conteúdo principal é renderizado por `ClientesContent`.

```tsx
<ClientesContent
  carregando={carregando}
  clientes={clientesFiltrados}
  onEditar={abrirModalEdicao}
  onExcluir={solicitarExclusaoCliente}
/>
```

Esse componente recebe o estado de carregamento, a lista filtrada e as funções relacionadas às ações da lista. A edição abre o modal com o cliente selecionado. A exclusão não remove diretamente o cliente, mas abre a confirmação por meio de `solicitarExclusaoCliente`.

O modal de formulário é renderizado de forma condicional.

```tsx
{modalAberto && (
  <ClienteFormModal
    clienteSelecionado={clienteSelecionado}
    onSalvar={salvarCliente}
    onCancelar={fecharModal}
  />
)}
```

Esse trecho indica que o formulário só aparece quando `modalAberto` é verdadeiro. O modal recebe o cliente selecionado, a função de salvamento e a função de cancelamento. Assim, o mesmo componente pode atuar tanto no cadastro quanto na edição.

O modal de confirmação também é renderizado condicionalmente.

```tsx
{clienteIdParaExcluir && (
  <ConfirmModal
    title="Confirmar exclusão"
    message="Deseja realmente excluir este cliente?"
    confirmText="Excluir"
    cancelText="Cancelar"
    variant="danger"
    onConfirm={confirmarExclusaoCliente}
    onCancel={cancelarExclusaoCliente}
  />
)}
```

Esse modal é exibido apenas quando existe um cliente pendente de exclusão. A propriedade `variant="danger"` indica que a ação é destrutiva. O componente não conhece a regra de exclusão; ele apenas apresenta a confirmação e executa as funções recebidas por `props`.

Assim, `ClientesPage.tsx` atua como componente coordenador da tela. Ele não realiza chamadas diretas à API, não contém a estrutura interna da lista, não implementa o formulário e não define o visual do modal. Sua responsabilidade é articular os hooks e componentes, controlando o fluxo de interação da página de clientes.
