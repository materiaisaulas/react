## `src/components/ClienteItem/ClienteItem.tsx`

O arquivo `src/components/ClienteItem/ClienteItem.tsx` define o componente responsável pela apresentação individual de um cliente. Enquanto `ClienteList.tsx` organiza a coleção, `ClienteItem.tsx` representa cada registro da lista, exibindo suas informações principais e disponibilizando as ações de edição e exclusão.

Esse componente possui responsabilidade estritamente visual e interativa. Ele não carrega dados, não altera a API, não controla estado próprio e não decide se o modal será aberto. Sua função é receber um cliente por `props`, apresentar seus dados e encaminhar eventos para o componente superior.

A interface `ClienteItemProps` define os parâmetros necessários para o funcionamento do componente.

```tsx
interface ClienteItemProps {
  cliente: Cliente;
  onEditar: (cliente: Cliente) => void;
  onExcluir: (id: string) => void;
}
```

A propriedade `cliente` representa o objeto que será exibido. A propriedade `onEditar` representa a função executada quando o usuário solicita a edição do registro. A propriedade `onExcluir` representa a função executada quando o usuário solicita a exclusão. A diferença entre elas está no dado enviado: a edição recebe o objeto completo `Cliente`, enquanto a exclusão recebe apenas o `id`, pois para remover um registro basta identificar qual cliente deve ser excluído.

O componente recebe essas propriedades por desestruturação.

```tsx
export function ClienteItem({
  cliente,
  onEditar,
  onExcluir,
}: Readonly<ClienteItemProps>) {
```

A utilização de `Readonly<ClienteItemProps>` indica que as propriedades recebidas não devem ser modificadas internamente. O componente apenas lê o objeto `cliente` e aciona as funções recebidas quando necessário.

A estrutura principal do componente é um elemento `article`.

```tsx
<article className="cliente-item surface row row--between gap-md">
```

O uso de `article` é adequado porque cada cliente representa uma unidade independente de conteúdo dentro da lista. A classe `cliente-item` identifica o componente. A classe `surface` aplica o padrão visual de superfície. As classes `row`, `row--between` e `gap-md` organizam o conteúdo em linha, distribuem os elementos entre as extremidades e aplicam espaçamento padronizado.

A primeira parte do componente apresenta os dados do cliente.

```tsx
<div className="cliente-item__content stack stack--sm">
  <h3 className="cliente-item__name">{cliente.nome}</h3>

  <p className="cliente-item__info">
    <strong>E-mail:</strong> {cliente.email}
  </p>

  <p className="cliente-item__info">
    <strong>Telefone:</strong> {cliente.telefone}
  </p>
</div>
```

O nome do cliente é apresentado como título do item. O e-mail e o telefone são apresentados como informações complementares. As classes `stack` e `stack--sm` organizam essas informações verticalmente, com espaçamento reduzido entre elas.

A segunda parte do componente apresenta os botões de ação.

```tsx
<div className="cliente-item__actions row gap-sm">
```

Essa área agrupa os botões de edição e exclusão. As classes `row` e `gap-sm` mantêm os botões alinhados horizontalmente e separados por um espaçamento padronizado.

O botão de edição executa a função `onEditar`, enviando o cliente completo.

```tsx
<button
  className="icon-button icon-button--edit"
  type="button"
  onClick={() => onEditar(cliente)}
  title="Editar cliente"
>
  <FaEdit />
</button>
```

O envio do objeto completo é necessário porque a edição precisa preencher o formulário com os dados atuais do cliente. Assim, o componente superior recebe o cliente selecionado e pode abrir o modal em modo de edição.

O botão de exclusão executa a função `onExcluir`, enviando apenas o identificador do cliente.

```tsx
<button
  className="icon-button icon-button--delete"
  type="button"
  onClick={() => onExcluir(cliente.id)}
  title="Excluir cliente"
>
  <FaTrash />
</button>
```

A exclusão não é realizada diretamente nesse componente. O botão apenas informa qual cliente foi selecionado para exclusão. A decisão de abrir o modal de confirmação e executar a remoção permanece no componente superior.

Assim, `ClienteItem.tsx` representa a unidade visual da listagem. Ele exibe os dados de um cliente e encaminha ações para quem controla a tela. Essa separação mantém o componente simples, evita acoplamento com regras de negócio e permite que a listagem continue organizada por composição: a lista renderiza itens, e cada item conhece apenas os dados e eventos necessários para sua própria apresentação.
