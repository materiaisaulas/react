## `src/hooks/useClienteModal.ts`

O arquivo `src/hooks/useClienteModal.ts` define um hook personalizado para controlar o estado de abertura e fechamento do modal de formulário. Sua função é isolar a lógica de interação relacionada ao modal, evitando que a página concentre diretamente todas as operações de seleção, abertura e limpeza do cliente em edição.

Esse hook não trata da persistência dos dados, não realiza chamadas à API e não manipula os campos do formulário. Sua responsabilidade está limitada ao controle da interface: indicar se o modal está aberto e qual cliente está selecionado no momento da edição.

O primeiro estado controlado pelo hook é `modalAberto`.

```ts
const [modalAberto, setModalAberto] = useState(false);
```

Esse estado possui valor booleano. Quando `modalAberto` é `false`, o modal de formulário não deve ser exibido. Quando `modalAberto` é `true`, a página renderiza o componente `ClienteFormModal`. O valor inicial é `false`, pois a tela deve iniciar sem o modal aberto.

O segundo estado é `clienteSelecionado`.

```ts
const [clienteSelecionado, setClienteSelecionado] = useState<Cliente | null>(null);
```

Esse estado armazena o cliente que será editado. A tipagem `Cliente | null` indica que o valor pode ser um objeto do tipo `Cliente` ou `null`. Essa definição é necessária porque o mesmo modal é utilizado para duas operações distintas: cadastro e edição. Quando o valor é `null`, o modal representa um novo cadastro. Quando há um objeto `Cliente`, o modal representa a edição de um registro existente.

A função `abrirModalCadastro` prepara o modal para a criação de um novo cliente.

```ts
function abrirModalCadastro() {
  setClienteSelecionado(null);
  setModalAberto(true);
}
```

Inicialmente, o cliente selecionado é definido como `null`. Essa ação garante que o formulário não seja preenchido com dados de uma edição anterior. Em seguida, `setModalAberto(true)` abre o modal. Dessa forma, o estado do modal fica coerente com a operação de cadastro.

A função `abrirModalEdicao` prepara o modal para alterar um cliente existente.

```ts
function abrirModalEdicao(cliente: Cliente) {
  setClienteSelecionado(cliente);
  setModalAberto(true);
}
```

Nesse caso, a função recebe um objeto `Cliente` como parâmetro. Esse objeto é armazenado em `clienteSelecionado` e será utilizado pelo formulário para preencher os campos com os dados atuais do cliente. Em seguida, o modal é aberto.

A função `fecharModal` encerra o uso do modal e limpa o cliente selecionado.

```ts
function fecharModal() {
  setClienteSelecionado(null);
  setModalAberto(false);
}
```

Essa função executa duas ações complementares: remove qualquer cliente selecionado e fecha o modal. A limpeza do cliente é importante porque impede que uma nova abertura do modal mantenha dados antigos em memória de interação.

Ao final, o hook retorna os estados e as funções necessários para o controle do modal.

```ts
return {
  modalAberto,
  clienteSelecionado,
  abrirModalCadastro,
  abrirModalEdicao,
  fecharModal,
};
```

Esse retorno define a interface de uso do hook. A página passa a receber o estado de abertura, o cliente selecionado e as funções responsáveis por abrir o modal em modo de cadastro, abrir o modal em modo de edição e fechar o modal.

Dessa forma, `useClienteModal.ts` organiza uma responsabilidade específica da aplicação: controlar a interação de abertura e fechamento do modal de cliente. A página continua responsável por coordenar a tela, mas a lógica interna do modal deixa de ficar espalhada no componente principal.
