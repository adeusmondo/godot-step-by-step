# Godot Step by Step

Godot documentation tutorials. Motivação desse projeto é documentar e traduzir os tutoriais passo-a-passo dos elementos principais da game engine Godot, a fim de exercício.

# Conceitos Chaves em Godot

Em Godot, um jogo consiste de uma árvore (`tree`) de nós (`nodes`) que são agrupados em uma ou mais cenas (`scenes`). No qual podemos amarrar esses nodes para que eles possam se comunicar utilizando `signals`.

## Scenes

Em Godot quebramos nosso jogo em cenas reutilizáveis. Uma cena pode ser um personagem, uma arma, um menu em uma interface de usuário, uma única casa, um nível inteiro, etc. As cenas em Godot preenchem o papel de tanto `prefabs` como cenas em outros game engines.

Você pode aninhar cenas também, colocando um personagem em um nível e arrastando e largando uma cena como filho dele.

## Nodes

Uma cena é composta por um ou mais nodes. Um node em nosso jogo se trata do menor “bloco de construção“ que podemos organizar em árvores. Dado um exemplo: Um Personagem é uma árvore com vários nodes e esses seus (`Sprite2D`, `Camera2D`, etc) compõem a árvore que é o personagem, quando você salva a árvore de nodes como uma cena, ele mesmo acaba se tornando um node.

## Scene tree

Todas as cenas do nosso jogo se agrupam em uma scene tree (árvore de cenas) e essas cenas são árvores de nodes, e também essa árvore de cenas é uma árvore de nodes, tudo é um node. Porém é fácil imaginar o nosso jogo em termos de cenas que podem representar personagens, armas, portas ou interface de usuário.

## Signals

Nodes emitem signals quando algum evento ocorre. Essa feature permite que você faça com que os nodes se comuniquem sem muita complexidade em seu código. 

Signals em Godot são a versão do Pattern de Observer: https://gameprogrammingpatterns.com/observer.html

Por exemplo, botões emitem signals quando pressionados. Você pode fazer uma conexão com esse signal para que seja executado um código como reação a esse evento, como iniciar o seu jogo ou abrir um menu.

Outros signals built-in podem lhe dizer quando dois objetos colidem, quando um personagem ou monstro entra em uma dada área, etc.

# Nodes e Cenas

## Nodes

Nodes são os blocos de construção fundamentais do nosso jogo. Sendo todos os nodes possuindo as seguintes características:

- Um nome
- Propriedades editáveis
- Eles recebem callbacks para atualizar a cada frame
- Você pode estendê-los com novas propriedades e funções
- Você pode adicionar ele a outro node como um filho

Juntos, nodes formam uma árvore, que é uma poderosa forma de organizar nosso projeto.

## Cenas

Quando organizamos os nodes em uma árvore, chamamos essa construção de cena. Quando salvos, essas cenas funcionam como novos tipos de nodes em nosso editor, assim podendo adicioná-lo como um filho de um outro node já existente.

Além de agir como nodes, as cenas possuem as seguintes características:

- Eles sempre possuem um node raiz, como por exemplo um Personagem
- Você pode salvar a cena em seu disco local e depois carregá lo em seu projeto mais tarde
- Você pode criar quantas instâncias da cena você quiser. Por exemplo, ter 5 ou 10 Personagens em seu jogo, todos criados a partir da cena do seu Personagem

## Instâncias de Cena como Linguagem de Design

Instâncias e cenas são a linguagem de design utilizada em Godot para desenvolvimento de jogos. Quando se desenvolve um jogo em Godot, devemos ignorar padrões de código e arquitetura como MVC e Entity-Relationship. Em vez disso, podemos imaginar os elementos que os jogadores verão no jogo e estruturar o código em torno deles.

Considerando que cada elemento em nosso jogo representa uma entidade visível em nosso jogo na perspectiva do jogador, podemos criar uma cena para cada elemento e então utilizar a instanciação, via código ou editor para construir nossa árvore de cenas e compor nosso jogo.

# Linguagens de Script

Scripts são anexados a um node e estendem o seu comportamento, pois os scripts herdam todas as funções e propriedades dos nodes ao qual ele está conectado.

## Criando um script

Quando criamos um script um nome especial `_init` pode ser dado ao construtor da nossa função. A engine chama as funções `_init()` para cada objeto ou node quando criado em memória, caso você defina a função.

Os ângulos em Godot funcionam em radianos por padrão, mas tem funções e propriedades internas disponíveis se preferir calcular os ângulos em graus.

Para que uma ação seja realizada a cada frame no loop do jogo, podemos chamar a função virtual  `_processs()` de uma classe Node, essa função recebe um delta que é o tempo decorrido entre o último frame e o atual. Essa função pode ser definida em qualquer classe que se estenda da classe Node.

Utilizamos esse mecanismo de fazer as atualizações com base no delta para que o movimento de renderização seja independente da nossa taxa de quadros, já que podemos encontrar diferentes taxas de frames em diferentes tipos de dispositivos.

# Signals

São mensagens que os nodes emitem quando algo em específico acontece com eles, como um botão ser pressionado. Outros nodes podem ser conectados a um signal e chamar uma função quando o evento ocorrer.

Signals é um mecanismo de delegação nativo do Godot que permite que um objeto de jogo reaja a mudanças em outro objeto sem que um faça referência ao outro. Uso de signals limita o acoplamento e mantém seu código flexível.

Por exemplo, você pode ter uma barra de vida na sua tela que representa a saúde do player. Quando o player toma algum dano ou usa uma poção de cura, você quer que a barra reflita a mudança. Para fazer isso, em Godot, utilizamos signals.

Podemos conectar um signal via editor ou via código.

## Custom signals

Você pode criar seus próprios signals, definidos em um script. Por exemplo, digamos que você queira mostrar uma tela de game over quando a vida do jogador chega a zero. Para fazer isso, você pode definir um signal chamado `died` or `health_depleted` quando a vida chega a zero.

> “Signals representam eventos que ocorreram, nós geralmente usamos um verbo de ação.”

```gdscript
extends Node2D

signal health_depleted

var health = 10
```


Nossos signals funcionam da mesma maneira que os nativos: eles aparecem na aba de nodes e você pode conectar a um node como qualquer outro.

Para que o nosso signal seja emitido em nosso script, devemos chamar o método “emit()” do signal.

```gdscrip
func take_damage(amount):
    health -= amount
    if health <= 0:
        health_depleted.emit()
```

Um signal pode opcionalmente declare um ou mais argumentos. Especificando o nome do argumento entre parênteses: 

```gdscript
extends Node

signal health_changed(old_value, new_value)

var health = 10
```
