## `src/components/ConfirmModal/ConfirmModal.tsx`

O arquivo `src/components/ConfirmModal/ConfirmModal.tsx` define um componente genérico de confirmação. Sua função é apresentar uma janela modal do sistema para validar ações que exigem uma decisão explícita do usuário, como excluir um registro, cancelar uma operação ou confirmar uma ação crítica.

Esse componente substitui o uso de recursos nativos do navegador, como `confirm(...)`, por uma solução visual integrada à interface da aplicação. Dessa forma, a confirmação deixa de depender de uma caixa padrão do navegador e passa a seguir o mesmo padrão visual dos demais elementos do sistema.

O componente é genérico porque não está vinculado exclusivamente ao domínio de clientes. Ele não conhece cliente, produto, usuário ou qualquer outra entidade específica. Sua responsabilidade é apenas receber textos, variação visual e funções de confirmação ou cancelamento.

A interface `ConfirmModalProps` define os parâmetros necessários para configurar o modal.

```tsx
interface ConfirmModalProps {
  title: string;
  message: string;
  confirmText?: string;
  cancelText?: string;
  variant?: "danger" | "primary";
  onConfirm: () => void | Promise<void>;
  onCancel: () => void;
}
```

A propriedade `title` define o título exibido no modal. A propriedade `message` define a mensagem principal apresentada ao usuário. As propriedades `confirmText` e `cancelText` são opcionais e permitem alterar os textos dos botões. A propriedade `variant` também é opcional e controla a variação visual do modal, podendo assumir o valor `"primary"` ou `"danger"`.

A propriedade `onConfirm` representa a função executada quando o usuário confirma a ação. Como essa função pode executar uma operação assíncrona, seu retorno aceita `void` ou `Promise<void>`. A propriedade `onCancel` representa a função executada quando o usuário cancela a ação ou fecha o modal.

O componente recebe essas propriedades e define valores padrão para algumas delas.

```tsx
export function ConfirmModal({
  title,
  message,
  confirmText = "Confirmar",
  cancelText = "Cancelar",
  variant = "primary",
  onConfirm,
  onCancel,
}: Readonly<ConfirmModalProps>) {
```

Os valores padrão permitem usar o componente sem informar todos os textos em cada chamada. Caso `confirmText` não seja informado, o botão exibirá “Confirmar”. Caso `cancelText` não seja informado, o botão exibirá “Cancelar”. Caso `variant` não seja informado, o modal assume a variação `"primary"`.

A classe do botão de confirmação é definida de acordo com a variação visual.

```tsx
const confirmButtonClass = variant === "danger" ? "button--danger" : "button--primary";
```

Essa definição separa a intenção da ação da estrutura visual. Quando a ação representa risco ou remoção de dados, utiliza-se `button--danger`. Quando a ação é comum ou positiva, utiliza-se `button--primary`.

A estrutura do modal utiliza o elemento HTML `<dialog>`.

```tsx
<dialog className="modal-card" aria-labelledby="confirm-modal-title" open>
```

O uso de `<dialog>` é adequado porque o componente representa semanticamente uma janela de diálogo. O atributo `open` indica que o diálogo deve estar visível. O atributo `aria-labelledby` associa o modal ao título identificado por `confirm-modal-title`, favorecendo a leitura por tecnologias assistivas.

O cabeçalho do modal apresenta o ícone, o título e o botão de fechamento.

```tsx
<header className="modal-header row row--between gap-md">
  <div className="modal-title-group row gap-sm">
    <FaExclamationTriangle className={`modal-icon modal-icon--${variant}`} />

    <h2 id="confirm-modal-title" className="modal-title">
      {title}
    </h2>
  </div>

  <button
    className="icon-button icon-button--neutral"
    type="button"
    onClick={onCancel}
    title="Fechar"
  >
    <FaTimes />
  </button>
</header>
```

O ícone `FaExclamationTriangle` reforça visualmente que a ação exige atenção. A classe dinâmica `modal-icon--${variant}` permite alterar a cor do ícone conforme a variação do modal. O botão com `FaTimes` executa `onCancel`, mantendo o mesmo comportamento do botão “Cancelar”.

O corpo do modal apresenta a mensagem de confirmação.

```tsx
<div className="modal-body">
  <p className="modal-message">{message}</p>
```

A mensagem é recebida por `props`, o que permite reutilizar o componente em diferentes situações. O componente não fixa o texto internamente, pois sua finalidade é ser configurável.

O rodapé apresenta os botões de cancelamento e confirmação.

```tsx
<footer className="modal-footer row row--end gap-sm">
  <button className="button button--secondary" type="button" onClick={onCancel}>
    {cancelText}
  </button>

  <button className={`button ${confirmButtonClass}`} type="button" onClick={onConfirm}>
    {confirmText}
  </button>
</footer>
```

O botão de cancelamento utiliza a variação secundária, indicando uma ação de recuo. O botão de confirmação utiliza a classe definida pela propriedade `variant`, permitindo diferenciar visualmente uma confirmação comum de uma ação destrutiva.

Assim, `ConfirmModal.tsx` estabelece um componente reutilizável para confirmações do sistema. Ele não executa diretamente regras de negócio, não conhece a entidade que será afetada e não controla sozinho sua exibição. Sua responsabilidade é apresentar a confirmação e encaminhar a decisão do usuário para as funções recebidas por `props`.
