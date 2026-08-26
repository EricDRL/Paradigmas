# Derivação Gramatical — Linguagem C

## Fonte consultada

**C Language Reference (Microsoft Learn)**
`https://learn.microsoft.com/en-us/cpp/c-language/c-language-reference?view=msvc-170`

Essa página é o índice geral da referência da linguagem C implementada pelo MSVC. A gramática sintática detalhada fica distribuída em subpáginas como *C Language Syntax Summary*
(`https://learn.microsoft.com/en-us/cpp/c-language/c-language-syntax-summary?view=msvc-170`), que reúne as regras usadas em todo o manual, e nas páginas de "Expressions" e "Statements" ligadas a partir dela.

## Notação utilizada

A Microsoft descreve explicitamente que a notação usada é uma variante do **BNF (Backus-Naur Form)**: os não terminais aparecem em itálico, os terminais (palavras-chave, símbolos literais) aparecem tal como são escritos no código, colchetes `[ ]` indicam elementos opcionais, e a barra vertical `|` indica alternativas — o mesmo estilo usado no apêndice de gramática do K&R e adotado como base pela documentação da Microsoft. Não é EBNF completo (não há repetição com `{ }` formalizada como metassímbolo padrão) nem PEG; é um BNF "informal", como a própria página deixa claro ao dizer que a notação serve para determinar a sintaxe exata de cada componente da linguagem.

## Regras de produção selecionadas

Para derivar um comando de **atribuição com expressão aritmética**, um subconjunto suficiente (simplificado a partir das regras de "statement" e "expression" da referência) é:

```
<statement>              ::= <expression-statement>
<expression-statement>   ::= <expression> ";"
<expression>             ::= <assignment-expression>
<assignment-expression>  ::= <identifier> "=" <additive-expression>
<additive-expression>    ::= <identifier> "+" <identifier>
<identifier>              ::= "a" | "b" | "x"
```

### Significado das principais regras

- **`<statement>`**: um comando de expressão C.
- **`<expression-statement>`**: uma expressão seguida de `;`, que é como C transforma uma expressão em um comando executável.
- **`<expression>`**: ponto de entrada genérico para qualquer expressão.
- **`<assignment-expression>`**: modela `variável = expressão`, correspondendo ao operador `=` da linguagem.
- **`<additive-expression>`**: modela a soma de dois operandos, correspondendo ao operador `+`.
- **`<identifier>`**: nome de variável; aqui restrito ao pequeno conjunto `a`, `b`, `x` só para manter o exemplo finito.

## Código a ser gerado

```c
x = a + b;
```

## Derivação

Partindo do símbolo inicial `<statement>`:

```
<statement>
⇒ <expression-statement>
⇒ <expression> ";"
⇒ <assignment-expression> ";"
⇒ <identifier> "=" <additive-expression> ";"
⇒ "x" "=" <additive-expression> ";"
⇒ "x" "=" <identifier> "+" <identifier> ";"
⇒ "x" "=" "a" "+" <identifier> ";"
⇒ "x" "=" "a" "+" "b" ";"
```

## Resultado final

```c
x = a + b;
```

### Como as regras foram utilizadas

1. `<statement>` foi expandido para `<expression-statement>` porque um comando de atribuição em C é, sintaticamente, uma expressão seguida de `;`.
2. `<expression-statement>` foi expandido em `<expression> ";"`, introduzindo o terminal `";"`.
3. `<expression>` foi reescrito como `<assignment-expression>`, escolhendo a alternativa de atribuição em vez de outras formas de expressão.
4. `<assignment-expression>` foi expandido em `<identifier> "=" <additive-expression>`, introduzindo o operador terminal `"="`.
5. O primeiro `<identifier>` foi substituído pelo terminal `"x"` (o lado esquerdo da atribuição).
6. `<additive-expression>` foi expandido em `<identifier> "+" <identifier>`, introduzindo o operador terminal `"+"`.
7. Os dois `<identifier>` restantes foram substituídos pelos terminais `"a"` e `"b"`.

Cada etapa aplicou exatamente uma produção, substituindo um não terminal pelo lado direito da regra correspondente, até restarem apenas terminais — a sequência de tokens `x = a + b ;`.

## Identificação dos terminais e não terminais

| Tipo | Símbolos usados |
|---|---|
| **Não terminais** | `<statement>`, `<expression-statement>`, `<expression>`, `<assignment-expression>`, `<additive-expression>`, `<identifier>` |
| **Terminais** | `x`, `a`, `b`, `=`, `+`, `;` |

Os não terminais representam categorias sintáticas abstratas (definidas por outras regras), enquanto os terminais são os tokens concretos que aparecem literalmente no código-fonte final e não podem mais ser reescritos.