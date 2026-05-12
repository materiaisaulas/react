## 3. Conceitos

### 3.1 Responsabilidade única e coesão do código

O princípio da **responsabilidade única** estabelece que uma unidade de código deve possuir uma finalidade principal claramente definida. Essa unidade pode ser uma função, um componente, um módulo ou um arquivo. A finalidade desse princípio é evitar que o mesmo trecho de código acumule funções diferentes e independentes entre si.

A **coesão** está diretamente relacionada à responsabilidade única. Um código é coeso quando seus elementos internos trabalham para cumprir uma mesma finalidade. Quando uma função, componente ou módulo reúne tarefas muito diferentes, sua coesão diminui e sua manutenção se torna mais difícil.

Considere o exemplo abaixo:

```ts
type Aluno = {
  nome: string;
  nota1: number;
  nota2: number;
};

export function calcularMedia(aluno: Aluno) {
  return (aluno.nota1 + aluno.nota2) / 2;
}
```

Nesse exemplo, o tipo `Aluno` representa os dados necessários para o cálculo, e a função `calcularMedia` realiza apenas uma operação: calcular a média das notas. O código possui responsabilidade delimitada e boa coesão.

A função não imprime relatório, não salva dados, não manipula interface e não envia informações para um servidor. Ela apenas calcula a média.

Uma versão menos adequada seria:

```ts
type Aluno = {
  nome: string;
  nota1: number;
  nota2: number;
};

export function processarAluno(aluno: Aluno) {
  const media = (aluno.nota1 + aluno.nota2) / 2;

  console.log(`Aluno: ${aluno.nome}`);
  console.log(`Média: ${media}`);

  localStorage.setItem("ultimoAluno", aluno.nome);

  return media;
}
```

O problema não está apenas no tamanho da função, mas na mistura de finalidades. Se for necessário alterar a forma de cálculo, a forma de exibição ou a forma de armazenamento, todas essas mudanças estarão concentradas no mesmo ponto.

Uma organização mais coesa separa essas responsabilidades:

```ts
type Aluno = {
  nome: string;
  nota1: number;
  nota2: number;
};

export function calcularMedia(aluno: Aluno) {
  return (aluno.nota1 + aluno.nota2) / 2;
}

export function exibirMedia(nome: string, media: number) {
  console.log(`Aluno: ${nome}`);
  console.log(`Média: ${media}`);
}

export function salvarUltimoAluno(nome: string) {
  localStorage.setItem("ultimoAluno", nome);
}
```

Essa separação permite alterar uma responsabilidade sem interferir diretamente nas demais. A função de cálculo pode ser modificada sem alterar o armazenamento. A forma de exibição pode mudar sem afetar o cálculo. O armazenamento pode ser removido sem comprometer a lógica da média.

O mesmo raciocínio se aplica a componentes React. Um componente coeso deve representar uma parte clara da interface ou uma responsabilidade visual específica.

```tsx
type TituloProps = {
  texto: string;
};

export function Titulo({ texto }: TituloProps) {
  return <h1>{texto}</h1>;
}
```

Esse componente possui uma responsabilidade visual simples: renderizar um título. Ele não decide rotas, não consulta dados externos, não controla estado global e não mistura outras funções da aplicação.

Um exemplo menos coeso seria:

```tsx
export function TelaCompleta() {
  const nome = localStorage.getItem("nome") ?? "Usuário";

  console.log("Renderizando tela");

  return (
    <main>
      <h1>Bem-vindo, {nome}</h1>
      <button type="button" onClick={() => localStorage.clear()}>
        Limpar dados
      </button>
    </main>
  );
}
```

Uma separação mais adequada poderia isolar as responsabilidades:

```tsx
function obterNomeUsuario() {
  return localStorage.getItem("nome") ?? "Usuário";
}

function limparDadosUsuario() {
  localStorage.clear();
}

export function Saudacao() {
  const nome = obterNomeUsuario();

  return (
    <main>
      <h1>Bem-vindo, {nome}</h1>
      <button type="button" onClick={limparDadosUsuario}>
        Limpar dados
      </button>
    </main>
  );
}
```

A estrutura fica mais organizada porque cada parte possui uma finalidade clara.

A responsabilidade única não exige criar muitos arquivos pequenos sem necessidade. O objetivo não é fragmentar artificialmente o projeto. O objetivo é manter juntas as partes que pertencem à mesma finalidade e separar aquilo que muda por razões diferentes.

A responsabilidade única deve ser entendida como um critério de organização do código. Ela não depende apenas da estrutura de pastas. Pastas podem ajudar a agrupar arquivos relacionados, mas a responsabilidade real é determinada pelo conteúdo e pelo papel de cada módulo no funcionamento da aplicação.

## 3.2 Módulos, importações e exportações

Um **módulo** é um arquivo que disponibiliza código para ser utilizado por outros arquivos. Em projetos JavaScript e TypeScript modernos, cada arquivo pode funcionar como um módulo, exportando componentes, funções, tipos, constantes ou objetos.

A organização por módulos permite dividir o código em partes menores, com responsabilidades mais claras. Em vez de concentrar toda a aplicação em um único arquivo, cada módulo reúne elementos relacionados a uma finalidade específica.

### Exportação nomeada

A **exportação nomeada** disponibiliza um elemento pelo nome com que ele foi declarado. Esse tipo de exportação é comum quando um arquivo precisa disponibilizar uma ou mais funções, tipos ou constantes.

Exemplo:

```ts
export function somar(a: number, b: number) {
  return a + b;
}

export function subtrair(a: number, b: number) {
  return a - b;
}
```

Nesse exemplo, o módulo exporta duas funções: `somar` e `subtrair`.

Para usar essas funções em outro arquivo, utiliza-se a importação com chaves:

```ts
import { somar, subtrair } from "./operacoes";

const resultadoSoma = somar(10, 5);
const resultadoSubtracao = subtrair(10, 5);
```

As chaves indicam que a importação está buscando exportações nomeadas do módulo informado.

A exportação nomeada favorece clareza, porque o nome importado precisa corresponder ao nome exportado.

### Exportação de tipos

Em projetos TypeScript, também é comum exportar tipos. Isso permite definir um contrato de dados em um arquivo e reutilizá-lo em outros pontos do projeto.

Exemplo:

```ts
export type Pessoa = {
  nome: string;
  idade: number;
};
```

Esse tipo pode ser importado em outro arquivo:

```ts
import type { Pessoa } from "./pessoa";

const professor: Pessoa = {
  nome: "Ana",
  idade: 42,
};
```

O uso de `import type` indica que a importação é usada apenas para tipagem. Essa forma deixa explícito que o tipo não será utilizado como valor em tempo de execução.

### Exportação padrão

A **exportação padrão** disponibiliza um único valor principal de um módulo. Ela é feita com `export default`.

Exemplo:

```ts
export default function multiplicar(a: number, b: number) {
  return a * b;
}
```

A importação de uma exportação padrão não utiliza chaves:

```ts
import multiplicar from "./multiplicar";

const resultado = multiplicar(4, 3);
```

Nesse caso, o nome usado na importação pode ser escolhido pelo arquivo que importa:

```ts
import calcularProduto from "./multiplicar";
```

A função exportada continua sendo a mesma, mas foi importada com outro nome.

Em projetos React com TypeScript, é comum utilizar exportações nomeadas para componentes, tipos, dados e funções auxiliares, pois essa forma facilita a identificação explícita do que está sendo importado.

### Caminhos relativos de importação

O **caminho relativo de importação** indica onde está o arquivo que fornece o elemento importado. Esses caminhos normalmente começam com `./` ou `../`.

Em `aula.ts`, a importação pode ser:

```ts
import { somar } from "./matematica";
```

O trecho `./matematica` indica que o arquivo `matematica.ts` está no mesmo diretório de `aula.ts`.

Em `aula.ts`, para importar `matematica.ts`, o caminho seria:

```ts
import { somar } from "../utils/matematica";
```

O trecho `../` sobe um nível na estrutura de diretórios. Depois, `utils/matematica` indica a pasta e o arquivo desejado.

### Importação de bibliotecas externas

Além de importar arquivos locais, um projeto também pode importar bibliotecas instaladas como dependências. Nesse caso, o caminho não começa com `./` nem com `../`.

Exemplo:

```ts
import { useState } from "react";
```

Nesse caso, `react` é uma dependência instalada no projeto. O código importa de uma biblioteca externa, não de um arquivo local.

Outro exemplo:

```ts
import { createRoot } from "react-dom/client";
```

Nesse caso, a importação acessa o pacote `react-dom` e, dentro dele, o subcaminho `client`.

### Organização por módulos

A organização por módulos permite separar código por finalidade. Um arquivo pode concentrar tipos; outro, funções; outro, componentes; outro, dados simulados; outro, configuração.

Arquivo `tipos.ts`:

```ts
export type Produto = {
  nome: string;
  preco: number;
};
```

Arquivo `operacoes.ts`:

```ts
import type { Produto } from "./tipos";

export function calcularTotal(produtos: Produto[]) {
  return produtos.reduce((total, produto) => {
    return total + produto.preco;
  }, 0);
}
```

Arquivo `principal.ts`:

```ts
import type { Produto } from "./tipos";
import { calcularTotal } from "./operacoes";

const produtos: Produto[] = [
  { nome: "Caderno", preco: 20 },
  { nome: "Caneta", preco: 5 },
];

const total = calcularTotal(produtos);

console.log(total);
```

Nesse exemplo, cada arquivo possui uma finalidade delimitada:

Essa separação reduz acoplamento desnecessário e aumenta a coesão dos arquivos. O arquivo de tipos não precisa saber onde os dados serão usados. O arquivo de operações não precisa saber como os dados serão exibidos. O arquivo principal combina apenas os módulos necessários.

Na leitura de um projeto React, as importações indicam dependências entre arquivos. Quando um arquivo importa um componente, tipo, função ou constante, ele passa a depender daquele módulo para cumprir sua responsabilidade.


## 3.3 Roteamento e navegação

O **roteamento no front-end** é o mecanismo que associa caminhos de URL a partes específicas da interface. Em uma aplicação React, esse mecanismo permite exibir diferentes telas conforme o endereço acessado no navegador, sem exigir o carregamento completo de uma nova página HTML a cada navegação.

Em termos conceituais, o roteamento estabelece uma correspondência entre **rota** e **componente de tela**.

Nesse modelo, cada caminho representa uma entrada possível da aplicação. Quando o usuário acessa uma rota, a aplicação decide qual componente deve ser renderizado.

## Rota raiz

A **rota raiz** representa o caminho principal da aplicação. Normalmente, é indicada por `/`.

Em um sistema simples, a rota raiz poderia exibir uma página inicial:

```tsx
function HomePage() {
  return <h1>Página inicial</h1>;
}
```

A rota raiz é o ponto inicial de navegação. Ela costuma representar a primeira tela acessada pelo usuário ao abrir a aplicação.

## Rota index

A **rota index** é a rota padrão renderizada dentro de uma rota pai. Ela é usada quando a rota pai é acessada sem caminho adicional.

Nesse caso, ao acessar `/`, a aplicação exibe a rota index. Ao acessar `/perfil`, a aplicação exibe outra rota filha.

A rota index não possui um caminho próprio. Ela representa o conteúdo padrão da rota pai.

## Rotas filhas

As **rotas filhas** são rotas declaradas dentro de uma rota pai. Elas permitem organizar a navegação de modo hierárquico.

Exemplo:

![Rota simples](./rota_simples.png){ width=50% }

Nesse caso, a rota `/` atua como rota pai. As rotas `produtos` e `contato` são rotas filhas.

Esse modelo é adequado quando várias páginas compartilham uma mesma estrutura visual, como cabeçalho, menu ou rodapé.

## Rotas aninhadas

As **rotas aninhadas** são rotas organizadas em níveis. Uma rota pode conter outras rotas, criando uma estrutura hierárquica de navegação.

Exemplo conceitual:

![Rota aninhadas](./rota_aninhada.png){ width=50% }

Essa estrutura pode gerar caminhos como:

```txt
/produtos
/produtos/10
/produtos/25
```

As rotas aninhadas permitem combinar estruturas fixas com conteúdos variáveis.

## Rota dinâmica

Uma **rota dinâmica** é uma rota que possui uma parte variável no caminho. Essa parte variável é frequentemente representada por um nome precedido de dois-pontos.

Exemplo:

```txt
/produtos/:produtoId
```

Nesse exemplo, `:produtoId` representa um valor variável.

Caminhos válidos poderiam ser:

```txt
/produtos/1
/produtos/25
/produtos/abc
```

A estrutura da rota permanece a mesma, mas o valor do parâmetro muda.

A rota dinâmica é adequada quando várias páginas seguem o mesmo formato, mas exibem dados diferentes conforme o identificador recebido.

## Parâmetro de rota

O **parâmetro de rota** é o valor capturado a partir de uma rota dinâmica.

Na rota:

```txt
/produtos/:produtoId
```

o parâmetro é:

```txt
produtoId
```

Se o usuário acessar:

```txt
/produtos/25
```

o valor capturado para `produtoId` será:

```txt
25
```

Em React Router, parâmetros de rota podem ser acessados por meio do hook `useParams`. A aplicação desse recurso será retomada na leitura do arquivo em que ele aparece.

## Navegação declarativa

A **navegação declarativa** ocorre quando a interface descreve o destino de navegação por meio de elementos próprios, em vez de executar comandos imperativos para alterar manualmente a URL.

Exemplo conceitual:

```tsx
<Link to="/produtos">Ver produtos</Link>
```

Nesse caso, o elemento declara que o destino da navegação é `/produtos`.

Em aplicações React com roteamento, links declarativos favorecem leitura e manutenção, porque o destino fica explícito no próprio elemento da interface.

## Link ativo

Um **link ativo** é um item de navegação que corresponde à rota atualmente acessada. Ele permite indicar visualmente ao usuário qual seção da aplicação está selecionada.

Em React Router, o componente `NavLink` permite identificar se o link corresponde à rota atual. A aplicação pode usar essa informação para aplicar uma classe CSS específica ao item ativo.

Exemplo simplificado:

```tsx
<NavLink
  to="/produtos"
  className={({ isActive }) =>
    isActive ? "nav-link nav-link--active" : "nav-link"
  }
>
  Produtos
</NavLink>
```

Nesse exemplo, `isActive` indica se o link corresponde à rota atual. Quando for verdadeiro, a classe `nav-link--active` é adicionada.

## Layout persistente e página variável

Em aplicações com rotas filhas, é comum manter uma estrutura visual persistente e trocar apenas a página interna.

Exemplo:

![Rota filha](./rota_filha.png){ width=50% }

Nesse modelo, o layout permanece estável, enquanto a área central muda conforme a rota.

Essa estrutura evita repetição de cabeçalho e navegação em cada tela. O layout comum é declarado uma vez, e as páginas são renderizadas dentro dele.

## Síntese conceitual

O roteamento organiza a navegação da aplicação por caminhos. As rotas definem quais partes da interface devem aparecer para cada endereço. A navegação declarativa permite expressar destinos diretamente na interface. As rotas dinâmicas capturam valores variáveis da URL. O link ativo contribui para orientação visual. O layout persistente permite manter estruturas comuns enquanto páginas variáveis são exibidas conforme a rota acessada.

![Roteamento](./roteamento.png){ width=50% }

## 3.4 Dados simulados e modelo de domínio

**Dados simulados** são estruturas criadas manualmente para representar informações que, em uma aplicação completa, poderiam vir de uma API, de um banco de dados ou de outro serviço externo. Esses dados permitem desenvolver e testar a interface antes da implementação da camada real de backend.

Em projetos front-end, dados simulados também são chamados de **mocks**. Um mock representa uma versão controlada e simplificada de uma fonte de dados real.

Um exemplo simples de dados simulados pode ser uma lista de cursos:

```ts id="zu582j"
export const courses = [
  {
    id: "1",
    name: "Programação",
    workload: 80,
  },
  {
    id: "2",
    name: "Banco de Dados",
    workload: 60,
  },
];
```

Nesse exemplo, `courses` é um array de objetos. Cada objeto representa um curso. A estrutura é suficiente para testar uma interface que lista cursos, sem depender de uma API real.

## Modelo de domínio

O **modelo de domínio** representa os principais conceitos manipulados por uma aplicação. Ele descreve quais entidades existem, quais propriedades elas possuem e como elas se relacionam.

Em uma aplicação acadêmica, por exemplo, o domínio poderia conter entidades como:

```txt
Aluno
Disciplina
Professor
Turma
Matrícula
```

Em uma aplicação de biblioteca, o domínio poderia conter:

```txt
Livro
Autor
Usuário
Empréstimo
Devolução
```

O modelo de domínio não é apenas uma lista de nomes. Ele define os objetos principais com os quais o sistema trabalha.

Exemplo:

```ts
type Book = {
  id: string;
  title: string;
  authorId: string;
};

type Author = {
  id: string;
  name: string;
};
```

Nesse exemplo, existem duas entidades: `Book` e `Author`. O livro possui um `authorId`, que indica uma relação com o autor.

## Identificador `id`

O identificador **`id`** é uma propriedade usada para diferenciar registros dentro de uma coleção. Ele permite localizar, comparar, relacionar e renderizar itens de forma estável.

Exemplo:

```ts
const students = [
  { id: "1", name: "Ana" },
  { id: "2", name: "Bruno" },
  { id: "3", name: "Carla" },
];
```

Nesse array, cada estudante possui um `id` próprio. Mesmo que dois estudantes tenham o mesmo nome, o `id` permite distingui-los.

O `id` é especialmente importante quando os dados são organizados em arrays. Ele permite buscar um item específico:

```ts id="bc73dc"
const selectedStudent = students.find((student) => student.id === "2");
```

Nesse exemplo, `find` procura o estudante cujo `id` é igual a `"2"`.

## Relacionamento entre dados

O **relacionamento entre dados** ocorre quando uma entidade referencia outra por meio de um identificador. Esse padrão é comum em aplicações que trabalham com coleções relacionadas.

Exemplo:

```ts 
type Category = {
  id: string;
  name: string;
};

type Product = {
  id: string;
  name: string;
  categoryId: string;
};
```

Nesse caso, `Product` possui uma propriedade `categoryId`, que indica a categoria à qual o produto pertence.

```ts 
const categories: Category[] = [
  { id: "c1", name: "Material escolar" },
  { id: "c2", name: "Tecnologia" },
];

const products: Product[] = [
  { id: "p1", name: "Caderno", categoryId: "c1" },
  { id: "p2", name: "Notebook", categoryId: "c2" },
];
```

O relacionamento permite filtrar ou agrupar dados. Por exemplo, para obter apenas os produtos da categoria `c1`:

```ts id="7cl4av"
const schoolProducts = products.filter(
  (product) => product.categoryId === "c1"
);
```

Nesse exemplo, `filter` retorna apenas os produtos relacionados à categoria `"c1"`.

## Array de dados

Um **array de dados** é uma coleção ordenada de registros. Em aplicações front-end, arrays são frequentemente usados para representar listas que serão exibidas na interface.

Exemplo:

```ts id="lf84m9"
const tasks = [
  { id: "1", title: "Estudar React", completed: false },
  { id: "2", title: "Praticar TypeScript", completed: true },
];
```

Cada item do array é um objeto com propriedades próprias. Esse formato permite renderizar listas, aplicar filtros, buscar itens, ordenar registros ou calcular totais.

Exemplo com `map`:

```ts id="g4j3h1"
const taskTitles = tasks.map((task) => task.title);
```

Nesse exemplo, `map` gera um novo array contendo apenas os títulos das tarefas.

## Separação entre dados e interface

A **separação entre dados e interface** consiste em manter os dados em uma estrutura própria e a interface em componentes responsáveis por exibir ou manipular esses dados. Essa separação melhora a coesão do código e reduz a mistura entre representação de dados e representação visual.

Exemplo menos adequado:

```tsx
export function CourseList() {
  return (
    <ul>
      <li>Programação — 80h</li>
      <li>Banco de Dados — 60h</li>
    </ul>
  );
}
```

Nesse exemplo, os dados estão fixos diretamente dentro da interface. Isso dificulta reutilização, filtragem, alteração e futura integração com uma API.

Uma organização mais adequada separa os dados:

```ts
export const courses = [
  { id: "1", name: "Programação", workload: 80 },
  { id: "2", name: "Banco de Dados", workload: 60 },
];
```

E a interface:

```tsx
import { courses } from "./courses";

export function CourseList() {
  return (
    <ul>
      {courses.map((course) => (
        <li key={course.id}>
          {course.name} — {course.workload}h
        </li>
      ))}
    </ul>
  );
}
```
Com essa separação, a interface não depende de dados escritos manualmente dentro do JSX. Ela depende de uma fonte de dados externa ao componente.

## Preparação para futura API

O uso de dados simulados prepara a aplicação para uma futura substituição por dados reais. Inicialmente, os dados podem vir de um arquivo local. Posteriormente, podem ser obtidos de uma API.

Exemplo inicial com mock:

```ts
import { courses } from "./courses";

export function getCourses() {
  return courses;
}
```

Exemplo futuro com API:

```ts
export async function getCourses() {
  const response = await fetch("/api/courses");
  return response.json();
}
```

Nesse modelo, a interface pode continuar usando uma função de obtenção de dados, enquanto a origem dos dados muda de mock para API.

Dados simulados e modelo de domínio permitem construir a aplicação de forma incremental. Primeiro, define-se uma estrutura mínima de entidades, identificadores e relacionamentos. Depois, essa estrutura pode ser substituída ou ampliada por uma fonte real de dados.


## 3.5 Estado global

O **estado da aplicação** corresponde ao conjunto de informações que pode alterar o comportamento ou a interface durante a execução do sistema. Em React, uma parte desse estado pode ficar restrita a um único componente, enquanto outra parte pode precisar ser compartilhada entre diferentes componentes ou telas.

Um exemplo simples de estado local é um contador usado apenas dentro de um componente:

```tsx
import { useState } from "react";

export function ContadorLocal() {
  const [contador, setContador] = useState(0);

  return (
    <button type="button" onClick={() => setContador(contador + 1)}>
      Cliques: {contador}
    </button>
  );
}
```

Nesse exemplo, `contador` pertence apenas ao componente `ContadorLocal`. Outro componente não acessa esse valor diretamente.

O **estado global** é necessário quando a mesma informação precisa ser lida ou modificada por diferentes partes da aplicação.

Uma **store global** é uma unidade centralizada de armazenamento de estado. Ela concentra valores compartilhados e funções responsáveis por alterar esses valores.

No ecossistema React, uma biblioteca usada para criar stores globais é o **Zustand**. A documentação oficial descreve o Zustand como uma solução pequena, rápida e escalável para gerenciamento de estado, com uma API baseada em hooks. Também indica que uma store criada com Zustand funciona como um hook, podendo conter valores, objetos e funções; a atualização do estado é feita por meio da função `set`. ([zustand.docs.pmnd.rs](https://zustand.docs.pmnd.rs/))

Um exemplo mínimo de store global com Zustand é:

```ts
import { create } from "zustand";

type CounterStore = {
  count: number;
  increment: () => void;
  clear: () => void;
};

export const useCounterStore = create<CounterStore>((set) => ({
  count: 0,

  increment: () =>
    set((state) => ({
      count: state.count + 1,
    })),

  clear: () =>
    set({
      count: 0,
    }),
}));
```

O nome `useCounterStore` segue a convenção de hooks, porque a store criada pelo Zustand é consumida em componentes como uma função hook.

O consumo do estado em um componente pode ser feito assim:

```tsx
import { useCounterStore } from "./counter.store";

export function CounterView() {
  const count = useCounterStore((state) => state.count);

  return <p>Valor atual: {count}</p>;
}
```

Nesse trecho, o componente seleciona apenas o valor `count` da store.

A alteração do estado pode ser feita em outro componente:

```tsx
import { useCounterStore } from "./counter.store";

export function CounterActions() {
  const increment = useCounterStore((state) => state.increment);
  const clear = useCounterStore((state) => state.clear);

  return (
    <div>
      <button type="button" onClick={increment}>
        Incrementar
      </button>

      <button type="button" onClick={clear}>
        Limpar
      </button>
    </div>
  );
}
```

Nesse exemplo, `CounterView` lê o estado e `CounterActions` modifica o estado. Ambos dependem da mesma store global.

A **seleção de estado** ocorre quando um componente acessa apenas a parte da store de que precisa.

```tsx
const count = useCounterStore((state) => state.count);
```

Essa seleção evita que o componente dependa de toda a store quando precisa apenas de um valor específico.

A **ação da store** é uma função declarada dentro da store para modificar o estado de forma controlada.

```tsx
const increment = useCounterStore((state) => state.increment);
```

A atualização do estado deve preservar a **imutabilidade**. Isso significa que, em vez de modificar diretamente o objeto ou array existente, cria-se uma nova versão do estado com a alteração necessária.

Exemplo com objeto:

```ts
type Profile = {
  name: string;
  age: number;
};

const profile: Profile = {
  name: "Ana",
  age: 30,
};

const updatedProfile = {
  ...profile,
  age: 31,
};
```

O **spread operator** (`...`) copia as propriedades existentes e permite sobrescrever apenas o campo alterado.

Em arrays, a imutabilidade também exige criar um novo array em vez de alterar diretamente o original.

```ts
const items = [
  { id: "1", name: "Item A", quantity: 1 },
  { id: "2", name: "Item B", quantity: 3 },
];

const updatedItems = items.map((item) =>
  item.id === "1"
    ? { ...item, quantity: item.quantity + 1 }
    : item
);
```

Nesse exemplo, `map` percorre os itens e gera um novo array. O item com `id === "1"` recebe uma nova versão com `quantity` incrementado. Os demais itens são preservados.

O método `find` é usado quando é necessário localizar um item específico:

```ts
const selectedItem = items.find((item) => item.id === "2");
```

O método `reduce` é usado para acumular valores e produzir um resultado final:

```ts
const totalQuantity = items.reduce((total, item) => {
  return total + item.quantity;
}, 0);
```

Em uma store global, esses métodos são frequentemente usados para consultar, atualizar e recalcular partes do estado.

Exemplo integrado:

```ts
import { create } from "zustand";

type Item = {
  id: string;
  name: string;
  quantity: number;
};

type ItemStore = {
  items: Item[];
  itemsCount: number;
  addItem: (item: Omit<Item, "quantity">) => void;
  clearItems: () => void;
};

export const useItemStore = create<ItemStore>((set) => ({
  items: [],
  itemsCount: 0,

  addItem: (item) =>
    set((state) => {
      const existingItem = state.items.find(
        (currentItem) => currentItem.id === item.id
      );

      const updatedItems = existingItem
        ? state.items.map((currentItem) =>
            currentItem.id === item.id
              ? {
                  ...currentItem,
                  quantity: currentItem.quantity + 1,
                }
              : currentItem
          )
        : [
            ...state.items,
            {
              ...item,
              quantity: 1,
            },
          ];

      return {
        items: updatedItems,
        itemsCount: updatedItems.reduce((total, currentItem) => {
          return total + currentItem.quantity;
        }, 0),
      };
    }),

  clearItems: () =>
    set({
      items: [],
      itemsCount: 0,
    }),
}));
```

O tipo utilitário `Omit<Item, "quantity">` indica que a função `addItem` recebe um item sem a propriedade `quantity`. A quantidade é definida internamente pela própria store. Esse padrão evita que o componente consumidor informe um campo que pertence à lógica de atualização do estado.

O estado global deve ser usado quando uma informação precisa ser compartilhada entre diferentes partes da aplicação. Ele não deve substituir todo estado local. Informações restritas a um único componente podem permanecer locais. Informações compartilhadas, persistentes no fluxo de uso ou necessárias em várias telas tendem a justificar uma store global.

## 3.6 Estilização, responsividade e acessibilidade

A **estilização** define a aparência visual da interface. Em aplicações React, a estrutura da interface é declarada nos componentes, enquanto a apresentação visual costuma ser definida em arquivos CSS. Essa separação permite organizar marcação e regras visuais em responsabilidades distintas.

Exemplo simples:

```tsx
export function Card() {
  return (
    <article className="card">
      <h2>Curso de React</h2>
      <p>Introdução à construção de interfaces.</p>
    </article>
  );
}
```

```css
.card {
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgb(0 0 0 / 0.12);
}
```

Nesse exemplo, o componente define a estrutura do conteúdo, enquanto a classe `.card` define a aparência visual. O atributo `className` associa o elemento JSX à classe CSS correspondente.

## O padrão BEM 

O padrão **BEM** é uma convenção de nomenclatura para classes CSS. Seu objetivo é tornar o código visual mais organizado, previsível e fácil de manter. A sigla BEM significa **Block, Element, Modifier**, isto é, **Bloco, Elemento e Modificador**. Cada uma dessas partes representa uma função diferente dentro da estrutura do CSS.

O **bloco** é a unidade principal de uma interface. Ele representa um componente visual independente, que pode existir sozinho na página, por exemplo, classes como `.card`, `.sidebar`, `.mock-root`, `.product-list` e `.product-card` podem ser entendidas como blocos. Cada uma delas representa uma parte visual com identidade própria: um cartão, uma barra lateral, uma simulação de raiz, uma lista de produtos ou um card de produto. 

O **elemento** é uma parte interna de um bloco. Ele não deve ser pensado como algo independente, mas como uma parte que pertence a um bloco maior. Em BEM, o elemento é indicado com dois sublinhados `__`. Por exemplo, em `.card__title`, a classe indica que `title` é um elemento pertencente ao bloco `.card`. Da mesma forma, `.sidebar__title` representa o título da sidebar, `.product-card__price` representa o preço dentro de um card de produto e `.mock-root__box` representa a caixa interna do bloco `.mock-root`. 

O **modificador** representa uma variação de um bloco ou de um elemento. Ele é usado quando a estrutura base continua sendo a mesma, mas precisa receber uma aparência, estado ou comportamento visual diferente. Em BEM, o modificador é indicado com dois hífens `--`, por exemplo, `.step-button--active` representa uma variação ativa do botão, `.action-button--primary` representa uma variação principal do botão de ação, `.level--success` representa uma variação visual do rótulo de nível, e `.product-card--highlight` representa uma variação destacada do card de produto. 

A ideia central do modificador é evitar que a classe base seja alterada para resolver um caso específico. Por exemplo, se todos os elementos com a classe `.mock-root` receberem `margin-top: 24px`, todos os usos desse bloco serão afetados. Isso pode gerar espaçamentos indevidos em partes da interface que não precisam desse afastamento. Uma solução mais correta seria criar um modificador, como `.mock-root--spaced`, para representar apenas a variação com espaçamento superior.

Nesse caso, o JSX ficaria assim:

```tsx
<div className="mock-root mock-root--spaced">
```

Essa escrita significa que o elemento recebe primeiro o estilo base de `.mock-root` e, além disso, recebe uma variação específica definida por `.mock-root--spaced`. Assim, a classe base continua preservada, e apenas o caso especial recebe o novo comportamento visual.

Portanto, o BEM ajuda a separar melhor as responsabilidades no CSS. O bloco define a estrutura visual principal, o elemento define as partes internas desse bloco, e o modificador define variações controladas. Essa separação evita efeitos colaterais, reduz conflitos entre classes e torna o código mais didático, especialmente quando se trabalha com componentes React.

Em termos práticos, o padrão pode ser compreendido assim: `.product-card` define o card de produto; `.product-card__title` define o título dentro desse card; e `.product-card--highlight` define uma versão destacada desse mesmo card. A estrutura visual continua sendo a mesma, mas o modificador altera uma característica específica.


## CSS global

O **CSS global** contém regras aplicáveis a diferentes partes da aplicação. Ele pode definir estilos base, variáveis, tokens visuais, botões, cards, navegação, estados de interação e regras responsivas.

Exemplo:

```css
body {
  margin: 0;
  font-family: system-ui, sans-serif;
  background: #f8fafc;
  color: #0f172a;
}

a {
  color: inherit;
  text-decoration: none;
}
```

Essas regras não pertencem a um componente específico. Elas definem uma base comum para toda a interface.

Em um projeto React, o CSS global normalmente é importado uma vez no ponto de entrada da aplicação, como discutido no Capítulo 2:

```tsx
import "./index.css";
```

A importação faz com que as regras do arquivo CSS sejam carregadas e aplicadas aos elementos renderizados pela aplicação.

## Variáveis CSS e tokens visuais

As **variáveis CSS**, também chamadas de propriedades personalizadas, permitem declarar valores reutilizáveis no CSS. Segundo a documentação da MDN, propriedades personalizadas são definidas com a notação `--nome-da-propriedade` e podem ser reutilizadas com a função `var()`. ([developer.mozilla.org](https://developer.mozilla.org/pt-BR/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties))

Exemplo:

```css
:root {
  --color-primary: #2563eb;
  --color-surface: #ffffff;
  --color-text: #0f172a;
  --spacing-md: 1rem;
  --radius-lg: 16px;
}
```

O seletor `:root` representa o nível mais alto do documento. Ao declarar variáveis nesse ponto, elas ficam disponíveis para a interface de forma ampla.

Uso das variáveis:

```css
.card {
  background: var(--color-surface);
  color: var(--color-text);
  padding: var(--spacing-md);
  border-radius: var(--radius-lg);
}
```

Quando variáveis CSS são usadas para padronizar cores, fontes, espaçamentos, bordas e sombras, elas funcionam como **tokens visuais**.

A principal vantagem é a consistência. Se uma cor principal for alterada em `:root`, todos os elementos que usam essa variável passam a refletir a mudança.

## Flexbox

O **Flexbox** é um modelo de layout unidimensional. Ele organiza elementos em uma direção principal: linha ou coluna. A documentação da MDN descreve o flexbox como um modelo adequado para organizar itens em uma dimensão. ([developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout))

Exemplo:

```html
<div class="toolbar">
  <button>Voltar</button>
  <button>Salvar</button>
</div>
```

```css
.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}
```

Nesse exemplo, `display: flex` organiza os botões em uma mesma linha. A propriedade `justify-content` distribui o espaço horizontal. A propriedade `align-items` controla o alinhamento vertical. A propriedade `gap` define o espaçamento entre os itens.

O Flexbox é adequado para barras de navegação, cabeçalhos, grupos de botões, alinhamento de ícones com textos e organização de pequenos blocos horizontais ou verticais.

## CSS Grid

O **CSS Grid** é um modelo de layout bidimensional. Ele organiza elementos em linhas e colunas. A documentação da MDN descreve o CSS Grid como um recurso adequado para dividir páginas em regiões principais e definir relações de tamanho e posição entre partes da interface. ([developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout))

Exemplo:

```html
<section class="cards-grid">
  <article>Card 1</article>
  <article>Card 2</article>
  <article>Card 3</article>
</section>
```

```css
.cards-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
```

Nesse exemplo, `display: grid` cria uma grade. A propriedade `grid-template-columns: repeat(3, 1fr)` define três colunas com a mesma largura. A propriedade `gap` define o espaçamento entre os cards.

O CSS Grid é adequado para grades de cards, áreas principais de páginas, painéis, catálogos e estruturas com múltiplas colunas.

## Responsividade

A **responsividade** é a capacidade da interface de se adaptar a diferentes tamanhos de tela. Uma aplicação responsiva deve preservar legibilidade, navegação e usabilidade em celulares, tablets e telas maiores.

Um recurso central para responsividade é a **media query**.

```css
.cards-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 768px) {
  .cards-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

Nesse exemplo, a grade possui uma coluna em telas menores. A partir de `768px`, passa a ter três colunas.

Essa estratégia segue o princípio **mobile-first**. Primeiro define-se a experiência para telas menores. Depois, por meio de media queries, adicionam-se ajustes para telas maiores.

## Estados visuais

Estados visuais indicam mudanças de interação, como passagem do mouse, clique, foco ou item ativo.

Exemplo com `hover` e `active`:

```css
.button {
  padding: 0.75rem 1rem;
  border-radius: 10px;
  transition: transform 0.2s ease;
}

.button:hover {
  transform: translateY(-2px);
}

.button:active {
  transform: translateY(0);
}
```

O estado `hover` é aplicado quando o usuário passa o cursor sobre o elemento. O estado `active` é aplicado durante a ativação do elemento, como no clique.

Esses estados ajudam a indicar que o elemento é interativo.

## Foco visível

O **foco visível** é essencial para navegação por teclado. A pseudo-classe `:focus-visible` permite estilizar o foco quando o navegador identifica que o indicador visual é necessário, por exemplo durante navegação com teclado. A documentação da MDN descreve `:focus-visible` como uma pseudo-classe aplicada quando o elemento recebe foco e o navegador determina que o foco deve ser evidenciado. ([developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible))

Exemplo:

```css
button:focus-visible,
a:focus-visible {
  outline: 3px solid #2563eb;
  outline-offset: 4px;
}
```

Nesse exemplo, botões e links recebem um contorno visível quando estão focados. Isso permite que usuários que navegam por teclado saibam qual elemento está selecionado.

## Ícones em componentes React

Ícones podem ser usados para reforçar a compreensão visual de ações, estados ou áreas da interface. Em componentes React, bibliotecas de ícones permitem importar ícones como componentes.

Exemplo genérico:

```tsx
import { FiSearch } from "react-icons/fi";

export function SearchButton() {
  return (
    <button type="button">
      <FiSearch aria-hidden="true" />
      Buscar
    </button>
  );
}
```

Nesse exemplo, o ícone acompanha o texto “Buscar”. O atributo `aria-hidden="true"` indica que o ícone é decorativo, pois o botão já possui texto visível suficiente para comunicar sua função.

Ícones devem complementar a informação textual, não substituí-la sem critério. Quando o ícone for puramente decorativo, ele pode ser ocultado de tecnologias assistivas. Quando for essencial para o significado, deve haver texto acessível.

## Semântica HTML

A **semântica HTML** consiste em usar elementos conforme seu significado estrutural. Elementos como `header`, `main` e `nav` indicam regiões da página.

Exemplo:

```tsx
export function PageLayout() {
  return (
    <div>
      <header>
        <h1>Aplicação</h1>
      </header>

      <main>
        <p>Conteúdo principal.</p>
      </main>

      <nav aria-label="Navegação principal">
        <a href="/">Início</a>
        <a href="/contato">Contato</a>
      </nav>
    </div>
  );
}
```

A semântica melhora a organização do documento e favorece a interpretação por navegadores, ferramentas de acessibilidade e testes baseados em papéis acessíveis.

## Atributos ARIA

Atributos ARIA complementam a semântica quando o HTML nativo não é suficiente para descrever corretamente a função ou o significado de um elemento.

Exemplo com `aria-label`:

```tsx
<nav aria-label="Navegação principal">
  <a href="/">Início</a>
  <a href="/produtos">Produtos</a>
</nav>
```

O atributo `aria-label` fornece um nome acessível para a região de navegação.

Exemplo com `aria-hidden`:

```tsx
<span aria-hidden="true">★</span>
```

O atributo `aria-hidden="true"` remove o elemento da árvore de acessibilidade. Esse recurso deve ser usado em elementos decorativos que não acrescentam informação relevante.

O uso de ARIA deve ser cuidadoso. Quando há elemento HTML semântico adequado, ele deve ser preferido. ARIA complementa a semântica; não substitui uma estrutura HTML bem definida.

## Síntese conceitual

A estilização define a aparência da aplicação. A responsividade adapta a interface a diferentes telas. A acessibilidade contribui para que a interface seja compreensível e utilizável por diferentes perfis de usuário.

## 3.7 Testes de interface

Os **testes de interface** verificam se uma interface renderiza elementos esperados e se esses elementos podem ser localizados de maneira semelhante à forma como seriam percebidos por usuários ou tecnologias assistivas. Em aplicações React, esse tipo de teste é usado para validar o comportamento visível dos componentes, e não apenas a existência de funções internas.

Em um projeto React com TypeScript e Vite, é comum utilizar ferramentas como **Vitest**, **React Testing Library** e **jest-dom**.


## Teste automatizado

Um **teste automatizado** é um trecho de código que executa uma verificação de forma repetível. Ele permite confirmar se determinada parte da aplicação continua funcionando após alterações no código.

Exemplo simples:

```ts
function somar(a: number, b: number) {
  return a + b;
}

test("soma dois números", () => {
  expect(somar(2, 3)).toBe(5);
});
```

Nesse exemplo, o teste verifica se a função `somar` retorna o resultado esperado.

## Vitest

O **Vitest** é um framework de testes integrado ao ecossistema Vite. Ele permite escrever testes com funções como `describe`, `it` e `expect`, além de oferecer execução rápida em projetos que já utilizam Vite.

A documentação oficial apresenta o Vitest como um framework de testes de próxima geração, desenvolvido sobre o Vite. ([vitest.dev](https://vitest.dev/))

Exemplo:

```ts
import { describe, expect, it } from "vitest";

function multiplicar(a: number, b: number) {
  return a * b;
}

describe("multiplicar", () => {
  it("multiplica dois números", () => {
    expect(multiplicar(4, 5)).toBe(20);
  });
});
```

O `describe` organiza um conjunto de testes. O `it` descreve uma situação específica. O `expect` compara o resultado obtido com o resultado esperado.

## React Testing Library

A **React Testing Library** é uma biblioteca usada para testar componentes React a partir do comportamento visível da interface. Em vez de priorizar detalhes internos da implementação, ela incentiva testes que consultam a interface como o usuário a perceberia.

A documentação oficial da Testing Library afirma que a biblioteca tem como princípio testar componentes de forma semelhante à maneira como usuários os utilizam. ([testing-library.com](https://testing-library.com/docs/react-testing-library/intro/))

Exemplo simples:

```tsx
import { render, screen } from "@testing-library/react";
import { expect, test } from "vitest";
import "@testing-library/jest-dom";

function Saudacao() {
  return <h1>Olá, estudante</h1>;
}

test("exibe a saudação", () => {
  render(<Saudacao />);

  expect(
    screen.getByRole("heading", { name: /olá, estudante/i })
  ).toBeInTheDocument();
});
```

Nesse exemplo, o componente `Saudacao` é renderizado no teste. Depois, `screen.getByRole` procura um elemento com papel acessível de título (`heading`) e nome correspondente ao texto renderizado.

## `jest-dom`

O **jest-dom** adiciona matchers específicos para testar elementos do DOM. Um matcher é uma função usada com `expect` para expressar uma verificação.

Exemplo:

```tsx
expect(elemento).toBeInTheDocument();
```

O matcher `toBeInTheDocument` verifica se determinado elemento está presente no documento renderizado durante o teste.

Em projetos React, o `jest-dom` costuma ser importado em um arquivo de configuração de testes:

```ts
import "@testing-library/jest-dom";
```

Essa importação disponibiliza matchers adicionais nos testes, tornando as verificações mais expressivas.

## Renderização em teste

A **renderização em teste** consiste em montar um componente em um ambiente controlado, sem abrir a aplicação no navegador real. A função `render`, da React Testing Library, executa essa montagem.

Exemplo:

```tsx
render(<button type="button">Salvar</button>);
```

Depois da renderização, a interface pode ser consultada:

```tsx
const button = screen.getByRole("button", { name: /salvar/i });

expect(button).toBeInTheDocument();
```

Esse padrão permite verificar se a interface apresenta os elementos esperados.

## Consulta por papel acessível

A **consulta por papel acessível** localiza elementos conforme sua função semântica. Esse tipo de consulta é preferível porque aproxima o teste da forma como tecnologias assistivas interpretam a interface.

Exemplo:

```tsx
screen.getByRole("button", { name: /salvar/i });
```

Esse comando procura um botão cujo nome acessível contenha “salvar”.

Outro exemplo:

```tsx
screen.getByRole("heading", { name: /cadastro/i });
```

Esse comando procura um título cujo nome acessível contenha “cadastro”.

A consulta por papel acessível depende de boa estrutura semântica. Por isso, testes de interface e acessibilidade básica estão relacionados: componentes com elementos semânticos adequados são mais fáceis de testar de maneira robusta.

## `within`

A função **`within`** permite consultar elementos dentro de uma região específica da interface. Isso é útil quando há elementos com nomes semelhantes em diferentes partes da tela.

Exemplo:

```tsx
import { render, screen, within } from "@testing-library/react";
import { expect, test } from "vitest";
import "@testing-library/jest-dom";

function Menu() {
  return (
    <nav aria-label="Menu principal">
      <a href="/">Início</a>
      <a href="/produtos">Produtos</a>
    </nav>
  );
}

test("exibe link de produtos no menu", () => {
  render(<Menu />);

  const navigation = screen.getByRole("navigation", {
    name: /menu principal/i,
  });

  expect(
    within(navigation).getByRole("link", { name: /produtos/i })
  ).toBeInTheDocument();
});
```

Nesse exemplo, primeiro se localiza a região de navegação. Depois, `within(navigation)` restringe a busca ao interior dessa região.

## Teste baseado em comportamento visível

Um teste baseado em comportamento visível verifica o que aparece na interface, e não detalhes internos do componente.

Exemplo menos adequado:

```tsx
// Exemplo conceitual: verifica detalhe interno do componente
expect(componente.props.title).toBe("Cadastro");
```

Exemplo mais adequado:

```tsx
expect(
  screen.getByRole("heading", { name: /cadastro/i })
).toBeInTheDocument();
```

No segundo exemplo, o teste verifica se o título aparece para o usuário. Ele não depende diretamente da estrutura interna do componente.

Essa abordagem favorece testes mais estáveis. Se a implementação interna mudar, mas o comportamento visível permanecer igual, o teste tende a continuar válido.

## Síntese conceitual