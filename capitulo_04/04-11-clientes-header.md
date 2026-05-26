## `src/components/ClientesHeader/ClientesHeader.tsx`

O arquivo `src/components/ClientesHeader/ClientesHeader.tsx` define o componente responsável pelo cabeçalho da página de clientes. Sua função é apresentar a identificação da tela, por meio de título e subtítulo, e disponibilizar a ação principal de cadastro de um novo cliente.

Esse componente foi separado da página para reduzir a quantidade de elementos visuais diretamente em `ClientesPage.tsx`. Dessa forma, a página permanece como coordenadora da tela, enquanto o cabeçalho passa a concentrar apenas a sua própria estrutura visual e a ação associada ao botão de criação.

A interface `ClientesHeaderProps` define a propriedade recebida pelo componente.

```tsx
interface ClientesHeaderProps {
  onNovoCliente: () => void;
}
```

A propriedade `onNovoCliente` representa a função executada quando o usuário aciona o botão “Novo cliente”. Essa função não é definida dentro do cabeçalho, pois a decisão sobre abrir o modal de cadastro pertence ao componente superior. O cabeçalho apenas apresenta o botão e encaminha o evento.

O componente recebe essa propriedade por desestruturação.

```tsx
export function ClientesHeader({ onNovoCliente }: Readonly<ClientesHeaderProps>) {
```

A utilização de `Readonly<ClientesHeaderProps>` indica que a propriedade recebida é somente leitura. O componente não altera a função recebida; apenas a executa quando o botão é pressionado.

A estrutura principal é formada por um elemento `header`.

```tsx
<header className="clientes-header row row--between gap-md">
```

O uso de `header` é adequado porque o componente representa a área introdutória da página. A classe `clientes-header` identifica o componente no CSS específico. As classes `row`, `row--between` e `gap-md` são utilitárias e organizam o conteúdo em linha, distribuindo o bloco textual e o botão nas extremidades com espaçamento padronizado.

O primeiro bloco do cabeçalho contém o título e o subtítulo.

```tsx
<div className="clientes-header__heading stack stack--sm">
  <h1 className="clientes-header__title">Cadastro de clientes</h1>

  <p className="clientes-header__subtitle">
    Gerenciamento simples de clientes consumindo uma API em JSON.
  </p>
</div>
```

O título identifica a finalidade da página. O subtítulo complementa essa identificação, indicando que a tela realiza o gerenciamento de clientes por meio de uma API em JSON. As classes `stack` e `stack--sm` organizam os textos verticalmente, com espaçamento reduzido entre eles.

O botão representa a ação principal do cabeçalho.

```tsx
<button className="button button--primary" type="button" onClick={onNovoCliente}>
  <FaPlus />
  Novo cliente
</button>
```

A classe `button` aplica o padrão geral de botão do sistema. A classe `button--primary` indica que se trata da ação principal da tela. O ícone `FaPlus` reforça visualmente a operação de inclusão. O atributo `type="button"` evita comportamento de submissão, e o evento `onClick` executa a função `onNovoCliente`.

Assim, `ClientesHeader.tsx` concentra a responsabilidade de apresentação do cabeçalho da página. Ele não controla estado, não abre diretamente o modal e não manipula dados de clientes. Sua função é exibir a identificação da tela e encaminhar a solicitação de criação de um novo cliente para o componente que coordena a página.
