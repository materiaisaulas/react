## `src/hooks/useClientesFiltrados.ts`

O arquivo `src/hooks/useClientesFiltrados.ts` define um hook personalizado para produzir uma lista derivada de clientes a partir da lista original e do valor informado no campo de busca. Sua responsabilidade não é carregar dados, alterar registros ou controlar a interface, mas calcular quais clientes devem permanecer visíveis conforme o critério de filtragem.

Neste arquivo aparece o uso de `useMemo`. O `useMemo` é um hook nativo do React utilizado para armazenar o resultado de um cálculo entre renderizações. Segundo a documentação oficial, ele permite manter em cache o valor calculado e recalculá-lo apenas quando suas dependências forem alteradas. ([React][1])

No contexto deste projeto, o cálculo memorizado corresponde à filtragem da lista de clientes. A lista filtrada depende de dois valores: `clientes` e `valorBusca`. Quando esses valores não mudam, não há necessidade de refazer o filtro.

```ts
export function useClientesFiltrados(clientes: Cliente[], valorBusca: string) {
  return useMemo(() => {
    const busca = valorBusca.trim().toLowerCase();

    if (!busca) {
      return clientes;
    }

    return clientes.filter((cliente) => {
      const nome = cliente.nome.toLowerCase();
      const email = cliente.email.toLowerCase();
      const telefone = cliente.telefone.toLowerCase();

      return nome.includes(busca) || email.includes(busca) || telefone.includes(busca);
    });
  }, [clientes, valorBusca]);
}
```

A função `useClientesFiltrados` recebe dois parâmetros. O primeiro é `clientes`, tipado como `Cliente[]`, indicando uma lista de objetos do tipo `Cliente`. O segundo é `valorBusca`, tipado como `string`, indicando o texto digitado no campo de busca.

O trecho seguinte prepara o valor usado na comparação.

```ts
const busca = valorBusca.trim().toLowerCase();
```

O método `trim()` remove espaços em branco no início e no final do texto digitado. O método `toLowerCase()` converte o texto para letras minúsculas. Essa transformação evita que a busca seja prejudicada por diferenças de caixa, como “Maria”, “maria” ou “MARIA”.

Em seguida, há uma verificação para o caso em que a busca esteja vazia.

```ts
if (!busca) {
  return clientes;
}
```

Quando não há texto de busca, o hook retorna a lista completa. Essa decisão evita aplicar filtros desnecessários e mantém todos os clientes visíveis.

Quando existe um valor de busca, a função `filter` percorre a lista de clientes e mantém apenas os registros compatíveis.

```ts
return clientes.filter((cliente) => {
  const nome = cliente.nome.toLowerCase();
  const email = cliente.email.toLowerCase();
  const telefone = cliente.telefone.toLowerCase();

  return nome.includes(busca) || email.includes(busca) || telefone.includes(busca);
});
```

Cada cliente é analisado com base em três campos: `nome`, `email` e `telefone`. Esses campos também são convertidos para letras minúsculas, mantendo o mesmo padrão aplicado ao texto da busca. O método `includes` verifica se o valor digitado aparece em algum desses campos. Caso apareça em pelo menos um deles, o cliente permanece na lista filtrada.

O vetor de dependências do `useMemo` é definido ao final.

```ts
}, [clientes, valorBusca]);
```

Esse vetor indica que o cálculo deve ser refeito quando a lista de clientes mudar ou quando o valor da busca for alterado. Caso esses valores permaneçam iguais, o React pode reutilizar o resultado anteriormente calculado.

Assim, `useClientesFiltrados.ts` organiza a lógica de derivação dos dados. Ele não cria a lista de clientes, não altera a API e não define a estrutura visual da busca. Sua função é transformar uma lista original em uma lista filtrada, mantendo essa regra isolada da página e facilitando a leitura do fluxo principal da aplicação.

[1]: https://react.dev/reference/react/useMemo "useMemo"
