## Seminário de Linguagens de Programação

---

Membros

* Gabriel Tiso
* João Gabriel 
* Luiz Eduardo
* Pedro Tiso 

---

<img src="./perl.jpeg" alt="drawing" style="width:400px;"/>

---

## História e Surgimento da linguagem

---

- Criada em 1987 por Larry Wall (4 anos antes do surgimento do kernel Linux)
- Linguagem open source

--

- Foi criada com a intenção de unificar aspectos que se destacavam em ferramentas de manipulação de texto, tais como
    * Expressões regulares do programa `sed`
    * Identificação de padrões do `awk`
    * Sintaxe baseada em C e em linguagens de Shell scripts

--

> There's more than one way to do it

---

### Paradigma e domínio de aplicação (onde / para o que é utilizada?)

---

- Possui suporte aos paradigmas funcional, orientado a objetos e procedural
- Entre os diversos usos da linguagem Perl, se destacam os seguintes domínios:
    * Tarefas administrativas em sistemas UNIX (Criação de scripts)
    * Ideal para programação web devido às suas capacidades de manipulação de texto, bem como 
        ao seu rápido ciclo de desenvolvimento

---

## Escopo, variáveis e tipos de dados

---

- Variáveis geralmente são prefixadas com a keyword `my`, para representar
valores locais.
- Um nome válido começa com uma letra ou underscore, seguida de qualquer número de 
letras, underscores e números

--

- Em Perl, variáveis podem ser classificadas de três formas com relação ao tipo:
    * Escalar: Prefixados pelo símbolo `$`, representam valores únicos, que podem 
    ser strings, inteiros e de ponto flutuante.

    * Strings em aspas duplas interpretam símbolos de escape como "\n", bem como outras
    variáveis. Strings em aspas simples escapam esses símbolos e os armazenam na string 
    como valores literais.

-- 

```perl
my $animal = "camel"
my $answer = 42
```

--

* Array: Representam uma lista de valores. São prefixados pelo símbolo `@`.

```perl
my @animals = ("camel", "llama", "cow");
my @numbers = (2, 3, 5);
my @mixed   = ("camel", 42, 1.23);

print "Original `mixed`: @mixed\n";
@mixed[0] = "crab";

print "Favorite animal: $animals[0]\n";
print "Last number: $numbers[$#numbers]\n";
print "Modified `mixed`: @mixed\n";
```
```bash
Original `mixed`: camel 42 1.23
Favorite animal: camel
Last number: 5
Modified `mixed`: crab 42 1.23
```

--

* Hash: Representa um conjunto de pares chave/valor. São prefixados
pelo símbolo `%`.

```perl
my %sounds = (
    cow => "mooooo",
    duck => "quack",
);

$sounds{pig} = "oink";
$sounds{sheep} = "baa";

delete $sounds{sheep};

while (my ($animal, $sound) = each %sounds) {
    print "$animal makes $sound!\n";
}
```
```bash
pig makes oink!
duck makes quack!
cow makes mooooo!
```

--

- Referências: valores escalares que se referem à arrays ou hashes.
São criadas utilizando `\`

```perl
my @nums = (1, 2, 3);
my $numsref = \@nums;

print "Nums: @nums, Nums ref: $numsref\n";
print "Second element: $numsref->[1]\n";
```
```bash
Nums: 1 2 3, Nums ref: ARRAY(0x5629f411baf8)
Second element: 2
```

--- 

## Estruturas de controle (decisão e repetição)

---

- Perl possui estruturas de decisão bastante similares às lingagens baseadas em C.

```perl
if ($var) {
  ...
} elsif ($var eq 'bar') {
  ...
} else {
  ...
}
```

```perl
# Outra forma de implementar a expressão `if (!condition)`
unless (condition) {
    ...
}
```

--

- Estruturas de repetição de simples entendimento.
- `for`: similar à C: contém `(inicialização; condição; incremento)`
- `foreach`: percorre uma lista, um array, ou até mesmo um hash
- É uma boa prática prefixar variáveis dos loops com `my`

--

```perl
for (my $i = 0; $i <= 10; $i++) {
    print "$i...\n"; # Contagem de 0 -> 10
}

foreach my $i (reverse 0..10) {
    print "$i...\n"; # Contagem regressiva
}
```

--

```perl
%hash = (dog => "lazy", fox => "quick");
foreach my $key (keys %hash) {
    print "The $key is $hash{$key}.\n";
}
```

```bash
The fox is quick.
The dog is lazy.
```

---

## Funções / Métodos

---

- As funções ou subrotinas possuem uma declaração simples e 
de fácil entendimento.

- Utilizamos a keyword `sub` para indicá-las, seguido do corpo da função
delimitado pelas chaves

- Os parâmetros passados pelos chamadores encontram-se em um array especial, `@_`.

--

```perl
sub write_name_to_file {
    my @params = @_;
    my $name = $params[0];

    open(my $fh, ">", "name.txt")
        or die "Can't open > name.txt $!";

    print $fh "Hello, $name!\n";
}

write_name_to_file("Gabriel");
```
--

- Utilizando a feature `'signatures'`, podemos declarar 
parâmetros junto da definição da função

```perl
use feature 'signatures';

sub soma($a, $b) {
    return $a + $b;
}

print soma(4, 5);
```

--

- Subrotinas podem retornar:
    * Explicitamente utilizando `return`
    * Implicitamente como o valor da última declaração executada.

- Podemos utilizar a keyword `wantarray` para definir o contexto do retorno

```perl
sub ten {
    return wantarray() ? (1 .. 10) : 10;
}

@ten = ten();          # (1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
$ten = ten();          # 10
($ten) = ten();        # (1)
($one, $two) = ten();  # (1, 2)
```

--

```perl
sub fibonacci {
    my ($n) = @_;
    die "Number must be positive" if $n <= 0;
    return 1 if $n <= 2;
    return (fibonacci($n-1) + fibonacci($n-2));
}

foreach my $i (1..5) {
    my $fib = fibonacci($i);
    print "fibonacci($i) is $fib\n";
}
```

```bash
fibonacci(1) is 1
fibonacci(2) is 1
fibonacci(3) is 2
fibonacci(4) is 3
fibonacci(5) is 5
```

---

## Características extras 

---

- Uma das features mais importantes e famosas da linguagem Perl são as suas
capacidades de processamento de string e expressões regulares (Regex)

--

- Para verificar se uma determinada string contém o conteúdo especificado por uma 
expressão regular, podemos usar a seguinte sintaxe:

```perl
# Checa se o match existe
"Hello World" =~ /World/;

# Checa se o match não existe
"Hello World" != /World/;
```

--

- Também podemos utilizar variáveis dentro das expressões regulares:
```perl
my $greeting = "everyone"
print "Matches!\n" if "Hello everyone" =~ /$greeting/;
```

--

- Se uma regex encontrar o padrão em mais de um lugar na string, Perl sempre realizará um match no ponto mais inicial da 
string:
```perl
"Hello World" =~ /o/;       # match no 'o' de 'Hello'
"That hat is red" =~ /hat/; # match no 'hat' de 'That'
```

--

- Os operadores de regex utilizados rotineiramente também estão presentes na linguagem. Alguns exemplos são:
```perl
$x = 'bcr';
/[$x]at/;   # match em 'bat', 'cat', ou 'rat'
/item[0-9]/; # match em item0 or item1 or ... or item9
```

--

- Também é possível extrair valores de uma determinada string:
```perl
($hours, $minutes, $second) = (
        $time =~ /(\d\d):(\d\d):(\d\d)/
);
```

--

- Search and Replace
```perl
$x = "I batted 4 for 4";
$x =~ s/4/four/;   # $x contém "I batted four for 4"
$x = "I batted 4 for 4";
$x =~ s/4/four/g;  # $x contém "I batted four for four".
                   # Observe a adição do operador `g`
```

--

- Split
```perl
$x = "Perl é legal";
@word = split /\s+/, $x;  # $word[0] = 'Perl'
                          # $word[1] = 'é'
                          # $word[2] = 'legal'
```

---

## Ambientes de desenvolvimento (IDEs)

---

- Padre. IDE para a linguagem Perl, escrita em Perl
- VSCode, Intellij e outras IDEs tambem possuem suporte por meio de plugins

---

## Principais bibliotecas e frameworks

---

- Mojolicious: Framework para a web. Possui suporte para rotas REST, plugins, templates, gerenciamento de sessões e 
muito mais

--

- DBI - Database independent interface for Perl: É um módulo de acesso à bancos de dados para a linguagem Perl.
De acordo com a documentação oficial: "Ele define um conjunto de métodos, variáveis e convenções que fornecem 
uma interface de banco de dados consistente, independente do banco de dados real que está sendo usado."

--

- JSON: Ferramenta capaz de codificar e decodificar estruturas do tipo JSON

---

## Curiosidades e cases de projetos que usam a linguagem

---

- Inicialmente se chamaria PEARL, porém, uma linguagem com esse nome já existia, e o nome
    foi alterado para Perl

---

## Obrigado!
