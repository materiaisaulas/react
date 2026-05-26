## `src/types/Cliente.ts`

O arquivo `src/types/Cliente.ts` estabelece a estrutura formal dos dados utilizados na aplicação. Trata-se da camada responsável por definir os tipos que orientam a manipulação dos clientes no sistema, indicando quais informações compõem um registro completo e quais informações pertencem ao processo de preenchimento do formulário.

A definição desses tipos é necessária porque a aplicação utiliza TypeScript. Nesse contexto, a tipagem atua como uma forma de controle estrutural dos dados, permitindo que componentes, hooks e serviços compartilhem a mesma representação sobre o objeto cliente. Dessa forma, reduz-se a possibilidade de inconsistência entre aquilo que é carregado da API, exibido na interface e enviado pelo formulário.

A interface `Cliente` representa o registro completo de um cliente no sistema.

```ts
export interface Cliente {
  id: string;
  nome: string;
  email: string;
  telefone: string;
  criadoEm?: string;
  atualizadoEm?: string;
}
```

O campo `id` representa o identificador único do cliente. Os campos `nome`, `email` e `telefone` correspondem às informações principais do cadastro e são utilizados tanto na listagem quanto na edição dos registros. Os campos `criadoEm` e `atualizadoEm` são opcionais, pois dependem da forma como a API ou a base de dados controla as datas de criação e atualização.

A interface `ClienteFormData` representa os dados manipulados pelo formulário.

```ts
export interface ClienteFormData {
  nome: string;
  email: string;
  telefone: string;
}
```

Essa estrutura contém apenas os campos editáveis pelo usuário. O formulário não manipula diretamente o `id`, nem os campos de data, pois esses elementos pertencem ao registro completo e não ao preenchimento realizado na tela.

Assim, o arquivo diferencia duas dimensões do mesmo domínio: o cliente persistido e o cliente em edição. O primeiro corresponde ao dado completo da aplicação; o segundo corresponde ao dado necessário para cadastrar ou atualizar um registro. Essa separação torna o código mais preciso, organiza a comunicação entre as partes do sistema e explicita a responsabilidade de cada estrutura de dados.
