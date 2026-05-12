## `render`

O método **`render`** é utilizado para exibir uma estrutura React dentro de uma raiz criada previamente com `createRoot`. Ele recebe como argumento um elemento React e solicita ao React que apresente essa estrutura no elemento DOM associado à raiz.

A documentação oficial informa que, depois de criar uma raiz com `createRoot`, deve-se chamar `root.render` para exibir um componente React dentro dela. ([react.dev](https://react.dev/reference/react-dom/client/createRoot))

O uso pode aparecer de forma encadeada:

```tsx
createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

A mesma estrutura pode ser escrita em duas etapas:

```tsx
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

Essa segunda forma evidencia que `render` não é uma função isolada importada diretamente. Ele é um método do objeto retornado por `createRoot`.

A estrutura recebida por `render` é escrita em TSX:

```tsx
<StrictMode>
  <App />
</StrictMode>
```

Esse bloco representa a estrutura inicial da aplicação. O componente `<App />` é a interface principal renderizada, enquanto `<StrictMode>` envolve essa estrutura para ativar verificações adicionais em ambiente de desenvolvimento.

O método `render` não cria o elemento HTML de montagem. Esse elemento já existe no `index.html`. Também não cria a raiz React. Essa responsabilidade pertence ao `createRoot`. O papel de `render` é definir o que será exibido dentro da raiz já criada.

Em termos conceituais, `render` estabelece o primeiro conteúdo React visível da aplicação. A partir dessa renderização inicial, o React passa a controlar a atualização da interface conforme componentes, propriedades, estados e rotas mudam.