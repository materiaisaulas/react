## Design System

Sim. Antes de explicar `variables.css`, é melhor iniciar pelo conceito de **design system**, porque esse conceito fundamenta a organização dos estilos globais do projeto.

Um **design system** pode ser entendido como um conjunto organizado de decisões visuais, padrões de interface e regras de uso que orientam a construção de uma aplicação. Ele não se limita às cores ou à aparência da tela, pois também envolve espaçamentos, tipografia, botões, mensagens, formulários, modais, estados visuais e formas recorrentes de organização dos componentes.

No contexto do projeto, o design system aparece de maneira simplificada, mas já cumpre uma função importante: evitar que cada componente defina seus próprios valores de cor, tamanho, borda, sombra ou espaçamento. Em vez disso, esses valores são centralizados em arquivos globais, principalmente em `variables.css` e `utilities.css`.

Dessa forma, a aplicação passa a ter uma base visual comum. Os componentes deixam de repetir decisões de estilo e passam a utilizar tokens e classes reutilizáveis. Por exemplo, uma cor principal, um espaçamento padrão, uma sombra de modal ou um raio de borda não precisam ser escritos várias vezes em arquivos diferentes. Eles são definidos uma vez e reutilizados em todo o sistema.

Essa organização contribui para três aspectos principais: (a) consistência visual, porque os elementos seguem o mesmo padrão; (b) manutenção, porque uma alteração em um token pode refletir em vários componentes; e (c) redução de redundância, porque estilos comuns deixam de ser reescritos em cada arquivo CSS.

Assim, a explicação dos estilos deve partir dessa ideia central: o projeto utiliza uma versão inicial de design system, baseada em tokens e classes utilitárias, para padronizar a interface e reduzir repetições no CSS.

## Camada CSS

A camada CSS do projeto foi organizada a partir de uma separação entre **base visual**, **classes reutilizáveis** e **estilos específicos de componentes**. Essa organização reduz a repetição de código e torna mais explícito o papel de cada arquivo dentro do sistema.

O arquivo `variables.css` concentra os tokens do design system. Nele estão definidos os valores fundamentais da interface, tais como cores, espaçamentos, sombras, raios de borda, tipografia, largura máxima da página e largura máxima dos modais. Esses valores não pertencem a um componente específico, pois representam decisões visuais gerais da aplicação.

O arquivo `utilities.css` transforma os tokens em padrões reutilizáveis. Classes como `surface`, `row`, `stack`, `button`, `icon-button`, `form-input`, `message`, `empty-state`, `modal-overlay` e `modal-card` evitam que componentes diferentes repitam as mesmas regras de layout, botão, formulário, mensagem ou modal.

A partir dessa organização, os arquivos CSS específicos ficaram menores. Eles passaram a conter apenas o que pertence diretamente ao componente. Por exemplo, `SearchBar.css` define apenas os ajustes próprios da barra de busca; `ClienteItem.css` define apenas os detalhes visuais de um item da lista; `ClientesHeader.css` define apenas a hierarquia visual do cabeçalho; e `ClientesPage.css` define apenas a largura e centralização da página.

Esse padrão evita que a mesma decisão visual seja declarada em vários lugares. A estrutura de modal, por exemplo, não está repetida em `ClienteFormModal.css` e `ConfirmModal.css`. Ela foi centralizada em `utilities.css`, por meio das classes genéricas de modal. Assim, tanto o formulário de cliente quanto o modal de confirmação utilizam a mesma base visual.

A mesma lógica foi aplicada aos formulários. As classes `form-field`, `form-label` e `form-input` representam o padrão geral de campo. Com isso, o `ClienteFormModal` não precisa possuir um CSS próprio apenas para definir rótulo, input, borda, foco ou espaçamento entre campos.

Também houve padronização das ações. Os botões comuns usam a base `button`, com variações como `button--primary`, `button--secondary` e `button--danger`. Os botões com ícone usam `icon-button`, com variações como `icon-button--edit`, `icon-button--delete` e `icon-button--neutral`. Essa separação permite diferenciar a intenção da ação sem duplicar a estrutura visual do botão.

Dessa forma, a camada CSS ficou organizada em três níveis: primeiro, os tokens em `variables.css`; segundo, os padrões reutilizáveis em `utilities.css`; terceiro, os ajustes específicos nos arquivos CSS dos componentes. Essa divisão torna o código mais consistente, reduz redundâncias e facilita a manutenção visual da aplicação.

