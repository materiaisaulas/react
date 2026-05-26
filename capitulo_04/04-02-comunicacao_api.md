## `src/services/clienteService.ts`

O arquivo `src/services/clienteService.ts` concentra a comunicação da aplicação com a API de clientes. Trata-se da camada responsável por isolar as operações externas de carga, cadastro, atualização e exclusão dos dados, evitando que componentes ou páginas tenham acesso direto às chamadas `fetch`.

Essa separação é importante porque estabelece uma fronteira entre a interface e a persistência dos dados. A tela não precisa conhecer os detalhes da URL, dos métodos HTTP, dos cabeçalhos ou do tratamento inicial da resposta. Sua responsabilidade permanece relacionada à interação com o usuário, enquanto o `service` assume a responsabilidade de comunicação com a API.

A constante `API_URL` define o endereço-base utilizado nas requisições.

```ts
const API_URL = "http://localhost:3000/clientes";
```

Esse valor indica que a aplicação consome uma API local, acessando o recurso `clientes` na porta `3000`. Dessa forma, todas as funções do arquivo utilizam a mesma origem de dados, o que evita repetição da URL em diferentes partes do sistema.

A função `listarClientes` realiza a busca dos clientes cadastrados.

```ts
export async function listarClientes(): Promise<Cliente[]> {
  const resposta = await fetch(API_URL);

  if (!resposta.ok) {
    throw new Error("Erro ao listar clientes.");
  }

  return resposta.json();
}
```

Essa função utiliza o método `GET`, que é o comportamento padrão do `fetch`. Caso a resposta da API não seja bem-sucedida, uma exceção é lançada. Caso contrário, o conteúdo da resposta é convertido para JSON e retornado como uma lista de objetos do tipo `Cliente`.

A função `cadastrarCliente` realiza a inclusão de um novo cliente.

```ts
export async function cadastrarCliente(
  dadosCliente: ClienteFormData
): Promise<Cliente> {
  const resposta = await fetch(API_URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(dadosCliente),
  });

  if (!resposta.ok) {
    throw new Error("Erro ao cadastrar cliente.");
  }

  return resposta.json();
}
```

Neste caso, a função recebe um objeto do tipo `ClienteFormData`, isto é, somente os dados preenchidos no formulário. A requisição utiliza o método `POST`, informa que o conteúdo enviado está no formato JSON e converte os dados do formulário por meio de `JSON.stringify`. Ao final, a API retorna o cliente criado.

A função `atualizarCliente` realiza a alteração de um cliente já existente.

```ts
export async function atualizarCliente(
  id: string,
  dadosCliente: ClienteFormData
): Promise<Cliente> {
  const resposta = await fetch(`${API_URL}/${id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(dadosCliente),
  });

  if (!resposta.ok) {
    throw new Error("Erro ao atualizar cliente.");
  }

  return resposta.json();
}
```

Essa função utiliza o `id` para localizar o cliente que será alterado. A URL passa a ser composta pelo endereço-base acrescido do identificador do registro. O método `PUT` indica que a operação corresponde à atualização dos dados de um recurso existente.

A função `excluirCliente` remove um cliente da API.

```ts
export async function excluirCliente(id: string): Promise<void> {
  const resposta = await fetch(`${API_URL}/${id}`, {
    method: "DELETE",
  });

  if (!resposta.ok) {
    throw new Error("Erro ao excluir cliente.");
  }
}
```

Essa operação também utiliza o `id` para identificar o registro. O método `DELETE` indica a remoção do recurso. Como não há necessidade de retornar um novo objeto após a exclusão, a função possui retorno `Promise<void>`.

