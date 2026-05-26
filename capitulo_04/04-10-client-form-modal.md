## `src/components/ClienteFormModal/ClienteFormModal.tsx`

O arquivo `src/components/ClienteFormModal/ClienteFormModal.tsx` define o componente responsável pelo formulário de cadastro e edição de clientes. Trata-se de um modal específico do domínio de clientes, pois seus campos, seus dados e sua lógica interna estão diretamente relacionados à entidade `Cliente`.

Diferentemente do `ConfirmModal`, que é genérico e pode ser usado para confirmar diferentes ações do sistema, o `ClienteFormModal` possui uma finalidade delimitada: coletar os dados de `nome`, `email` e `telefone`, permitindo cadastrar um novo cliente ou editar um cliente já existente.

A interface `ClienteFormModalProps` define as propriedades necessárias para o funcionamento do componente.

```tsx
interface ClienteFormModalProps {
  clienteSelecionado: Cliente | null;
  onSalvar: (dadosCliente: ClienteFormData) => void | Promise<void>;
  onCancelar: () => void;
}
```

A propriedade `clienteSelecionado` indica se o modal será aberto em modo de edição ou em modo de cadastro. Quando seu valor é `null`, o formulário será usado para cadastrar um novo cliente. Quando seu valor contém um objeto do tipo `Cliente`, o formulário será preenchido com os dados desse cliente para edição.

A propriedade `onSalvar` representa a função executada quando o formulário é enviado. Essa função recebe um objeto do tipo `ClienteFormData`, pois o formulário manipula apenas os campos editáveis pelo usuário. A propriedade `onCancelar` representa a função usada para fechar o modal sem salvar alterações.

O objeto `formularioInicial` define a estrutura vazia do formulário.

```tsx
const formularioInicial: ClienteFormData = {
  nome: "",
  email: "",
  telefone: "",
};
```

Esse objeto é utilizado para inicializar o estado do formulário e também para limpar os campos quando o modal é aberto em modo de cadastro. Sua tipagem como `ClienteFormData` garante que os campos do formulário correspondam à estrutura esperada para cadastro ou edição.

O estado `formData` armazena os valores atuais dos campos do formulário.

```tsx
const [formData, setFormData] = useState<ClienteFormData>(formularioInicial);
```

Nesse trecho, `formData` contém os dados atualmente exibidos nos campos. A função `setFormData` atualiza esses dados quando o usuário digita ou quando o componente recebe um cliente para edição. A expressão `useState<ClienteFormData>` indica que o estado seguirá a estrutura definida pela interface `ClienteFormData`.

O título do modal é definido a partir da existência de um cliente selecionado.

```tsx
const tituloModal = clienteSelecionado ? "Editar cliente" : "Adicionar cliente";
```

Essa expressão condicional ajusta o texto exibido no cabeçalho. Quando existe `clienteSelecionado`, o modal assume a finalidade de edição. Quando não existe, assume a finalidade de cadastro.

O `useEffect` sincroniza o estado interno do formulário com o cliente selecionado.

```tsx
useEffect(() => {
  if (clienteSelecionado) {
    setFormData({
      nome: clienteSelecionado.nome,
      email: clienteSelecionado.email,
      telefone: clienteSelecionado.telefone,
    });

    return;
  }

  setFormData(formularioInicial);
}, [clienteSelecionado]);
```

Esse efeito é executado sempre que `clienteSelecionado` muda. Quando há um cliente selecionado, os campos do formulário são preenchidos com `nome`, `email` e `telefone` do registro. Quando não há cliente selecionado, o formulário é reiniciado com valores vazios. Dessa forma, o mesmo componente atende aos dois fluxos: cadastro e edição.

A função `atualizarCampo` altera um campo específico do formulário.

```tsx
function atualizarCampo(campo: keyof ClienteFormData, valor: string) {
  setFormData((dadosAtuais) => ({
    ...dadosAtuais,
    [campo]: valor,
  }));
}
```

O tipo `keyof ClienteFormData` indica que o parâmetro `campo` deve corresponder a uma das chaves existentes em `ClienteFormData`, isto é, `nome`, `email` ou `telefone`. Essa escolha evita que a função tente atualizar um campo inexistente.

O operador de espalhamento `...dadosAtuais` preserva os valores já existentes no formulário. Em seguida, `[campo]: valor` atualiza apenas o campo modificado. Assim, quando o usuário altera o e-mail, por exemplo, os valores de nome e telefone são preservados.

A função `enviarFormulario` controla o envio do formulário.

```tsx
function enviarFormulario(event: FormEvent<HTMLFormElement>) {
  event.preventDefault();
  onSalvar(formData);
}
```

O tipo `FormEvent<HTMLFormElement>` indica que o evento recebido pertence a um formulário HTML. O método `preventDefault()` impede o comportamento padrão do navegador, que seria recarregar a página ao enviar o formulário. Em seguida, `onSalvar(formData)` encaminha os dados atuais do formulário para o componente responsável por decidir se a operação será cadastro ou edição.

A estrutura visual do componente utiliza a base padronizada de modal do sistema.

```tsx
<div className="modal-overlay">
  <dialog className="modal-card" aria-labelledby="cliente-modal-title" open>
```

A classe `modal-overlay` cria a camada que cobre a tela e bloqueia a interação com os elementos inferiores. O elemento `<dialog>` representa semanticamente a janela modal. A classe `modal-card` aplica a estrutura visual comum aos modais do sistema. O atributo `open` indica que o diálogo está visível, e `aria-labelledby` associa o modal ao título do cabeçalho.

O cabeçalho apresenta o título e o botão de fechamento.

```tsx
<header className="modal-header row row--between gap-md">
  <h2 id="cliente-modal-title" className="modal-title">
    {tituloModal}
  </h2>

  <button
    className="icon-button icon-button--neutral"
    type="button"
    onClick={onCancelar}
    title="Fechar"
  >
    <FaTimes />
  </button>
</header>
```

As classes `modal-header`, `row`, `row--between` e `gap-md` seguem o padrão visual da aplicação. O botão de fechamento executa `onCancelar`, encerrando o modal sem submeter o formulário.

O formulário utiliza classes padronizadas para corpo, empilhamento e campos.

```tsx
<form className="modal-body stack stack--md" onSubmit={enviarFormulario}>
```

A classe `modal-body` define a área de conteúdo do modal. As classes `stack` e `stack--md` organizam os campos em coluna, com espaçamento padronizado. O evento `onSubmit` associa o envio do formulário à função `enviarFormulario`.

Cada campo do formulário segue a mesma estrutura: rótulo, campo de entrada, valor controlado e função de atualização.

```tsx
<input
  id="nome"
  className="form-input"
  type="text"
  value={formData.nome}
  onChange={(event) => atualizarCampo("nome", event.target.value)}
  required
/>
```

Esse campo é controlado pelo React. O valor exibido vem de `formData.nome`, e toda alteração feita pelo usuário chama `atualizarCampo`. O atributo `required` indica que o campo deve ser preenchido antes do envio.

A mesma lógica é aplicada aos campos `email` e `telefone`.

```tsx
<input
  id="email"
  className="form-input"
  type="email"
  value={formData.email}
  onChange={(event) => atualizarCampo("email", event.target.value)}
  required
/>
```

```tsx
<input
  id="telefone"
  className="form-input"
  type="text"
  value={formData.telefone}
  onChange={(event) => atualizarCampo("telefone", event.target.value)}
  required
/>
```

O campo de e-mail utiliza `type="email"`, permitindo ao navegador aplicar validações básicas de formato. O campo de telefone permanece como texto, pois números de telefone podem conter espaços, parênteses, hífens e outros caracteres de formatação.

O rodapé concentra as ações do formulário.

```tsx
<footer className="modal-footer row row--end gap-sm">
  <button className="button button--secondary" type="button" onClick={onCancelar}>
    Cancelar
  </button>

  <button className="button button--primary" type="submit">
    <FaSave />
    Salvar
  </button>
</footer>
```

O botão “Cancelar” executa `onCancelar` e não envia o formulário, pois seu tipo é `button`. O botão “Salvar” possui `type="submit"`, acionando o evento `onSubmit` do formulário. Dessa forma, o envio permanece associado ao comportamento formal do elemento `form`.

Assim, `ClienteFormModal.tsx` concentra a lógica local do formulário de clientes. Ele controla apenas os valores digitados, sincroniza esses valores com o cliente selecionado e encaminha os dados preenchidos para a função recebida por `props`. A decisão sobre cadastrar ou editar não pertence ao modal; essa responsabilidade permanece na página que utiliza o componente.
