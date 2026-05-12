### Componente `ProdutoCard`

O arquivo `ProdutoCard.tsx` define um componente responsável por renderizar visualmente um único produto na interface. Ele representa o nível mais interno da árvore de componentes usada nesta atividade.

O componente recebe os dados de um produto por meio de `props`. Para isso, importa o tipo `Produto` definido no arquivo `data/produtos.ts`.


```ts
/**************************
data/ProdutoCard.tsx
***************************/

import type { Produto } from "../data/produtos";

type ProdutoCardProps = {
  produto: Produto;
  destacado?: boolean;
};

export function ProdutoCard({ produto, destacado = false }: Readonly<ProdutoCardProps>) {
  return (
    <article className={destacado ? "product-card product-card--highlight" : "product-card"}>
      <p className="level level--success">Nível 3</p>

      <h4 className="product-card__title">{produto.nome}</h4>

      <p className="product-card__description">{produto.descricao}</p>

      <p className="product-card__price">
        {produto.preco.toLocaleString("pt-BR", {
          style: "currency",
          currency: "BRL",
        })}
      </p>
    </article>
  );
}
```

## Componente `ListaProdutos`

O arquivo `ListaProdutos.tsx` define o componente responsável por organizar a exibição de uma lista de produtos. Ele representa o nível intermediário da árvore de componentes, pois recebe um conjunto de dados e delega a renderização de cada item ao componente `ProdutoCard`.

O componente importa o tipo `Produto`, usado apenas para tipagem, e importa também o componente `ProdutoCard`, que será utilizado para renderizar cada produto individualmente.

```tsx
/**************************
data/ListaProdutos.tsx
***************************/

import type { Produto } from "../data/produtos";
import { ProdutoCard } from "./ProdutoCard";

type ListaProdutosProps = {
    produtos: Produto[];
    mostrarCards?: boolean;
    limiteCards?: number;
    destacarUltimoCard?: boolean;
};

export function ListaProdutos({
    produtos,
    mostrarCards = true,
    limiteCards = produtos.length,
    destacarUltimoCard = false,
}: Readonly<ListaProdutosProps>) {
    const produtosVisiveis = produtos.slice(0, limiteCards);

    return (
        <section className="product-list">
            <div className="product-list__intro">
                <p className="level level--primary">Nível 2</p>

                <h3 className="product-list__title">Lista de Produtos</h3>

                <p className="card__text">
                    O componente <code>ListaProdutos</code> recebe um conjunto de dados por
                    propriedade e usa <code>map</code> para renderizar um{" "}
                    <code>ProdutoCard</code> para cada item visível.
                </p>
            </div>

            {mostrarCards ? (
                <div className="product-list__grid">
                    {produtosVisiveis.map((produto, indice) => (
                        <ProdutoCard
                            key={produto.id}
                            produto={produto}
                            destacado={destacarUltimoCard && indice === produtosVisiveis.length - 1}
                        />
                    ))}
                </div>
            ) : (
                <div className="product-list__grid">
                    {produtos.map((produto) => (
                        <div className="mock-root" key={produto.id}>
                            <p className="mock-root__label">ProdutoCard</p>
                            <div className="mock-root__box">
                                Card de {produto.nome} ainda não expandido
                            </div>
                        </div>
                    ))}
                </div>
            )}
        </section>
    );
}
```

## Componente `PaginaProdutos`

O arquivo `PaginaProdutos.tsx` define o componente responsável por representar uma área maior da interface: a página de produtos. Ele corresponde ao nível superior da árvore usada nesta atividade e organiza a exibição da seção antes de delegar a renderização da lista para o componente `ListaProdutos`.

O componente importa a lista de produtos definida no arquivo de dados e também importa o componente que será responsável por renderizar a lista.


```tsx
/**************************
data/PaginaProdutos.tsx
***************************/

import { produtos } from "../data/produtos";
import { ListaProdutos } from "./ListaProdutos";

type PaginaProdutosProps = {
    mostrarLista?: boolean;
    mostrarCards?: boolean;
    limiteCards?: number;
    destacarUltimoCard?: boolean;
};

export function PaginaProdutos({
    mostrarLista = true,
    mostrarCards = true,
    limiteCards = produtos.length,
    destacarUltimoCard = false,
}: Readonly<PaginaProdutosProps>) {
    return (
        <section>
            <header className="page-header">
                <p className="level level--primary">Nível 1</p>

                <h2 className="page-header__title">Página de Produtos</h2>

                <p className="card__text">
                    O componente <code>PaginaProdutos</code> representa uma área maior da
                    interface. Ele organiza o título da página e pode delegar a exibição
                    dos produtos ao componente <code>ListaProdutos</code>.
                </p>
            </header>

            {mostrarLista ? (
                <ListaProdutos
                    produtos={produtos}
                    mostrarCards={mostrarCards}
                    limiteCards={limiteCards}
                    destacarUltimoCard={destacarUltimoCard}
                />
            ) : (
                <div className="mock-root mock-root--spaced">
                    <p className="mock-root__label">Próximo componente</p>
                    <div className="mock-root__box">
                        ListaProdutos ainda não renderizado nesta etapa
                    </div>
                </div>
            )}
        </section>
    );
}
```

## Componente `RenderizacaoPorEtapa`

O arquivo `RenderizacaoPorEtapa.tsx` define o componente responsável por controlar o conteúdo exibido em cada etapa do playground. Ele funciona como o componente que seleciona qual explicação e qual parte da interface devem aparecer conforme a etapa escolhida.

O componente importa a lista de produtos, o componente `PaginaProdutos` e uma imagem que representa a árvore conceitual da interface.

```tsx
/*****************************
data/RenderizacaoPorEtapa.tsx
*****************************/

import { produtos } from "../data/produtos";
import { PaginaProdutos } from "./PaginaProdutos";
import arvoreProdutos from "../assets/arvore-produtos.png";

type RenderizacaoPorEtapaProps = {
    etapa: number;
    quantidadeCards: number;
    onProximoCard: () => void;
    onReiniciarCards: () => void;
};

export function RenderizacaoPorEtapa({
    etapa,
    quantidadeCards,
    onProximoCard,
    onReiniciarCards,
}: Readonly<RenderizacaoPorEtapaProps>) {
    if (etapa === 0) {
        return (
            <section className="card">
                <div className="explanation">
                    <h2 className="explanation__title">Árvore conceitual</h2>
                    <p className="explanation__text">
                        Antes da renderização visual, a interface pode ser compreendida como
                        uma árvore de componentes. Essa árvore mostra quais componentes
                        dependem de outros componentes.
                    </p>
                </div>

                <h2 className="section-title">Árvore de componentes</h2>
                
                <img
                    className="mock__image"
                    src={arvoreProdutos}
                    alt="Hierarquia de componentes: App, PaginaProdutos, ListaProdutos e ProdutoCard"
                />
            </section>
        );
    }

    if (etapa === 1) {
        return (
            <section className="card">
                <div className="explanation">
                    <h2 className="explanation__title">Etapa 1: raiz React</h2>
                    <p className="explanation__text">
                        O React começa a aplicação a partir de uma raiz. Em um projeto real,
                        essa raiz é criada com <code>createRoot</code> e ligada a um elemento
                        HTML, normalmente <code>div id="root"</code>.
                    </p>
                </div>

                <div className="mock-root">
                    <p className="mock-root__label">DOM do navegador</p>
                    <div className="mock-root__box">
                        &lt;div id="root"&gt; Aplicação React será renderizada aqui &lt;/div&gt;
                    </div>
                </div>
            </section>
        );
    }

    if (etapa === 2) {
        return (
            <section className="card">
                <div className="explanation">
                    <h2 className="explanation__title">Etapa 2: renderização da página</h2>
                    <p className="explanation__text">
                        O componente <code>App</code> renderiza o componente{" "}
                        <code>PaginaProdutos</code>. Neste momento, aparece a estrutura
                        principal da página, mas a lista detalhada ainda não foi exibida.
                    </p>
                </div>

                <PaginaProdutos mostrarLista={false} />
            </section>
        );
    }

    if (etapa === 3) {
        return (
            <section className="card">
                <div className="explanation">
                    <h2 className="explanation__title">Etapa 3: renderização da lista</h2>
                    <p className="explanation__text">
                        O componente <code>PaginaProdutos</code> passa os dados para{" "}
                        <code>ListaProdutos</code>. A lista já aparece, mas os cards ainda
                        são representados de forma conceitual.
                    </p>
                </div>

                <PaginaProdutos mostrarLista={true} mostrarCards={false} />
            </section>
        );
    }

    if (etapa === 4) {
        return (
            <section className="card">
                <div className="explanation">
                    <h2 className="explanation__title">Etapa 4: renderização dos cards finais</h2>
                    <p className="explanation__text">
                        O componente <code>ListaProdutos</code> usa <code>map</code> para
                        renderizar um <code>ProdutoCard</code> para cada produto. Esta é a
                        interface final visível ao usuário.
                    </p>
                </div>

                <PaginaProdutos mostrarLista={true} mostrarCards={true} />
            </section>
        );
    }

    return (
        <section className="card">
            <div className="explanation">
                <h2 className="explanation__title">Etapa 5: cards renderizados um a um</h2>
                <p className="explanation__text">
                    Esta etapa simula mudanças incrementais na interface. A cada clique, o
                    estado muda e a lista passa a conter mais um card. O React recalcula a
                    interface necessária e atualiza no DOM apenas as diferenças observadas
                    pela reconciliação.
                </p>
            </div>

            <div className="controls">
                <p className="controls__text">
                    Cards visíveis: {quantidadeCards} de {produtos.length}
                </p>

                <div className="controls__actions">
                    <button
                        className="action-button action-button--primary"
                        onClick={onProximoCard}
                        disabled={quantidadeCards >= produtos.length}
                    >
                        Renderizar próximo card
                    </button>

                    <button
                        className="action-button action-button--secondary"
                        onClick={onReiniciarCards}
                    >
                        Reiniciar
                    </button>
                </div>
            </div>

            <PaginaProdutos
                mostrarLista={true}
                mostrarCards={true}
                limiteCards={quantidadeCards}
                destacarUltimoCard={true}
            />
        </section>
    );
}
```