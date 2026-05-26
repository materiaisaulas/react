## `src/components/ClientesFeedback/ClientesFeedback.tsx`

O arquivo `src/components/ClientesFeedback/ClientesFeedback.tsx` define o componente responsável pela apresentação de mensagens de erro na página de clientes. Sua função é concentrar, em uma estrutura própria, a decisão de exibir ou ocultar o feedback visual relacionado a falhas nas operações da aplicação.

Em aplicações React, a renderização condicional é utilizada quando determinado elemento da interface deve aparecer apenas em uma situação específica. Nesse caso, a mensagem de erro só deve ser exibida quando houver conteúdo em `mensagemErro`. Quando não há erro, o componente não deve produzir nenhum elemento visual.

A interface `ClientesFeedbackProps` define a propriedade recebida pelo componente.

```tsx
interface ClientesFeedbackProps {
  mensagemErro: string;
}
```

A propriedade `mensagemErro` representa o texto que será exibido ao usuário quando uma operação falhar. Esse texto é produzido em outra parte do sistema, especialmente no hook `useClientes`, e chega ao componente apenas para apresentação visual.

O componente recebe essa propriedade por desestruturação.

```tsx
export function ClientesFeedback({ mensagemErro }: Readonly<ClientesFeedbackProps>) {
```

A utilização de `Readonly<ClientesFeedbackProps>` indica que a propriedade recebida não será modificada internamente. O componente apenas verifica seu conteúdo e decide se a mensagem será renderizada.

A primeira verificação trata o caso em que não existe mensagem de erro.

```tsx
if (!mensagemErro) {
  return null;
}
```

Esse trecho representa uma renderização condicional. Quando `mensagemErro` está vazia, o componente retorna `null`, o que significa que nada será renderizado na tela. Essa prática evita a criação de elementos HTML vazios e mantém a interface limpa quando não há falhas a comunicar.

Quando existe uma mensagem, o componente retorna o bloco visual de erro.

```tsx
return (
  <div className="message message--error clientes-feedback full-width">
    {mensagemErro}
  </div>
);
```

A classe `message` aplica o padrão geral de mensagem do sistema. A classe `message--error` define a variação visual de erro. A classe `clientes-feedback` identifica o componente, e `full-width` garante que a mensagem ocupe a largura disponível sem repetir a regra `width: 100%` no CSS específico.

Assim, `ClientesFeedback.tsx` concentra a responsabilidade de exibir mensagens de erro da página. Ele não define o erro, não executa tratamento de exceção e não realiza comunicação com a API. Sua função é apenas apresentar uma informação já produzida por outra camada da aplicação, mantendo a lógica de feedback visual separada da lógica de dados.
