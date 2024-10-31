# Sintaxe do Live
### Legenda da sintaxe
> Quando você encontrar um texto entre setas, exemplo \<texto\>, isso indica que é uma parte obrigatória da sintaxe.

> Quando você encontrar um texto entre colchetes, exemplo [texto], isso indica que é uma parte opcional da sintaxe.

## Scene
Uma scene ou cena é um escopo onde será aplicado diretrizes de renderização, lógica e ação. Cenas nomeadas podem ser unidas a outras cenas, removidas ou até mesmo intercaladas. De tal forma que é possível criar três cenas totalmente diferentes e exibir uma de cada vez. Só pode haver uma cena sem nome, que é a cena principal e onde começa a execução da lógica do seu aplicativo.
> ⚠ Você encontrará palavras-chave desconhecidas nos exemplos abaixo, mas eles são explicados logo em seguida.

> Sintaxe: scene [nome] { \<conteúdo\> }
```
scene {
    render span.title {
        Escolha uma das cenas para renderizar:
    }
    
    render button.one { Cena 1 }
    render button.two { Cena 2 }
    render button.three { Cena 3 }

    on click button.one {
        show One;
    }

    on click button.two {
        show Two;
    }

    on click button.three {
        show Three;
    }
}

scene One {
    render span.message {
        Essa é a primeira cena.
    }
}

scene Two {
    render span.message {
        Essa é a segunda cena.
    }
}

scene Three {
    render span.message {
        Essa é a terceira cena.
    }
}
```
## render
A diretiva render permite renderizar elementos HTML e especificar seu conteúdo, atributos, classe e o mais importante, quando renderizar. Sua sintaxe pode parecer assustadora, mas é bem simples. Veja exemplos abaixo:
> Sintaxe: render \<tag html\>.\<nome da classe\>(\<atributo=valor\>,...) { \<conteúdo\> } when (\<condição\>)
```
render input.username(type=text, placeholder="Digite seu nome");
render button.submit(type=submit) { Enviar } when(
    input.username.value.length > 0
);
```
O resultado em HTML deve ser:
```html
<input class="username" type=text placeholder="Digite seu nome" />
<!-- Quando o valor do input tiver um tamanho maior que zero -->
<button class="submit" type=submit>Enviar</button>
```
Você também pode renderizar elementos dentro de elementos ou aplicas outras diretivas:
```
render div.container {
    render span.message(style="color: blue") {
        Olá mundo!
    }
}
```
O resulto em HTML deve ser:
```html
<div class=container>
    <span class=message style="color: blue">Olá mundo</span>
</div>
```
## on
A diretiva on nos permite aplicar lógica após um evento ser disparado em um alvo específico, por exemplo, ao clicar em um botão, somar +1 em um contador. Sua sintaxe é bem simples e se utiliza de seletores css para definir o alvo:
> ⚠ A partir daqui estaremos introduzindo variáveis, tipagem e formatação de texto, não se assuste pois isso logo será explicado.

> Sintaxe: on \<nome do evento\> \<seletor css\> { \<conteúdo\> }
```
scene {
    let cliques: number = 0;

    render span.message {
        Você clicou no botão {cliques} vezes.
    }

    render button.add {
        Clique em mim!
    }

    on click button.add {
        cliques++;
    }
}
```
O resultado em HTML e JS deve ser:
```html
<span class=message>Você clicou no botão 0 vezes.</span>
<button class=add>Clique em mim!</button>
```
```js
const message = document.querySelector("span.message");
const add = document.querySelector("button.add");

let cliques = 0;

add.addEventListener("click", () => {
    cliques++;
    message.textContent = `Você clicou no botão ${cliques} vezes.`
});
```