## `src/styles/reset.css`

O arquivo `src/styles/reset.css` define a base de normalização visual da aplicação. Sua função é reduzir diferenças de comportamento entre navegadores e remover estilos padrões que poderiam interferir na construção da interface.

Todo navegador aplica estilos iniciais aos elementos HTML. Títulos possuem margens próprias, botões podem ter aparência nativa diferente, inputs variam conforme o sistema operacional e o modelo de caixa pode afetar o cálculo de largura e altura dos elementos. O reset atua justamente nesse ponto, criando uma base comum para que os estilos do projeto sejam aplicados de forma mais previsível.

A primeira regra normalmente aplicada é o controle do modelo de caixa.

```css
* {
  box-sizing: border-box;
}
```

Essa regra faz com que largura e altura de um elemento incluam conteúdo, borda e espaçamento interno. Sem essa definição, o cálculo visual dos elementos pode se tornar menos previsível, pois `padding` e `border` podem aumentar a dimensão final do componente. Com `box-sizing: border-box`, a construção de layouts torna-se mais controlada.

O reset também pode abranger os elementos estruturais da página.

```css
html,
body,
#root {
  min-height: 100%;
}
```

Essa definição garante que o documento, o corpo da página e o elemento raiz da aplicação possam ocupar a altura disponível. Como o React é montado dentro de `#root`, essa regra contribui para que o layout principal consiga preencher a área visível do navegador.

O corpo da página recebe a configuração visual básica da aplicação.

```css
body {
  margin: 0;
  font-family: var(--font-family-base);
  background-color: var(--color-page);
  color: var(--color-text-primary);
}
```

A remoção de `margin` elimina o espaçamento padrão aplicado pelo navegador ao `body`. A fonte, a cor de fundo e a cor principal do texto passam a vir dos tokens definidos em `variables.css`. Dessa forma, o reset não apenas remove padrões indesejados, mas também estabelece a base visual mínima da aplicação.

Elementos interativos, como botões e campos de formulário, também precisam seguir a tipografia do sistema.

```css
button,
input,
textarea,
select {
  font: inherit;
}
```

Essa regra faz com que esses elementos herdem a fonte definida no corpo da página. Sem essa padronização, botões e inputs podem assumir fontes nativas do navegador ou do sistema operacional, quebrando a consistência visual da interface.

Os botões recebem uma configuração básica de interação.

```css
button {
  cursor: pointer;
}
```

Essa regra indica visualmente que o botão é um elemento acionável. O cursor em forma de ponteiro melhora a percepção de interatividade e reforça a affordance do componente.

As imagens também podem receber uma normalização estrutural.

```css
img {
  max-width: 100%;
  display: block;
}
```

Essa definição impede que imagens ultrapassem a largura do contêiner e remove espaços residuais comuns em imagens renderizadas como elementos inline. Embora o projeto atual utilize poucas imagens, essa regra mantém a base preparada para elementos visuais futuros.

Assim, `reset.css` estabelece a base mínima para que a aplicação seja renderizada de maneira consistente. Ele não define componentes específicos, não cria estilos de botão, modal ou formulário, e não substitui o design system. Sua função é preparar o ambiente visual para que `variables.css`, `utilities.css` e os estilos específicos dos componentes possam atuar sobre uma base mais previsível.
