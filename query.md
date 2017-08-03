# Tipos de query

<!-- made by @oilujcesarv and @odanoburu-->

copiando e comentando
[essa página](http://bionlp.utu.fi/searchexpressions-new.html).

> The basic target of a query is a **word** with possible restriction
> on its dependent and governor structures which can be recursively
> restricted upon as well.

- [ ] busca por palavra

- Procuras de palavras podem ser feitas conforme a escrita da palavra

- Se a palavra coincidir com uma das possíveis tags, é preciso colocar
  a palavra entre aspas para procurar por ela e não pela tag que ela
  representa.

- [ ] busca por lemma

- lemma (l=prefixo): todos verbos terminados com determinado prefixo
  serão buscados

- [ ] busca por tag

- NOUN: dá todos os tokens com POS tag substantivo

- PartForm procura em todos os tempos: presente (PartForm=Pres),
  passado (PartForm=Past), agentive (PartForm=Agt) e negativo
  (PartForm=Neg)

- [ ] busca por tag = value

- Case=Par: partitive case (caso gramatical)

- VerbForm=Inf: todos os verbos no infinitivo

- Tense=Past: todos os verbos no passado

- PartForm=Past: past pasrticiple


- [ ] combinação das queries anteriores usando & (and) e | (or) e !
  (not) -> busca por palavra assumindo função da POS tag

- walk&NOUN: devolve todas as vezes em que walk aparece como noun

-  L=walk&PartForm=Pres apresenta todas as formas de walk no present
   participle

-  L=walk|L=run apresenta todas os lemas ou de walk ou de run

-  !Tra: fornece todos os casos que não são traslativos

-  can&!AUX: verbo can e quando ele não é auxiliar

- [ ] suporte à wildcard `_`

- \_: token não especificado (underscore)

- [ ] busca por dependências (< > imitam setas no grafo de dependência)

- token1 > token2: token1 governa sobre token2 (< - quando é governado)

- walk < \_: todos os caso de walk que são governados por alguma palavra

- walk > \_: todos os casos de walk que governa sobre alguma palavra

- \_< walk: algum token governado por walk

> os últimos dois casos recuperam as mesmas frases; mas devemos
> lembrar que a busca é por token, de modo que elas recuperam tokens
> diferentes: no primeiro caso busca-se por walk, no segundo pela
> palavra que é governada por walk

> na nossa implementação, temos uma única função (governedby
> (governor-token governed-token) body), em que trocar a ordem dos
> tokens faz essa mudança. ou, por conveniência, podemos definir (>
> (governor-token governed-token) body) e (< (governed-token
> governor-token) body).

- [ ] busca por tipo de dependência

-  \_ <type \_ ou \_ >type _: o tipo de dependencia pode ser
   especificado logo depois do operador de dependencia

-  \_ >nmod \_ >nmod \_ searches for words that govern two distinct
   nominal modifiers (two nommod dependencies in parallel)

> como implementar a função acima?

-  \_ >nmod (\_ >nmod \_): parênteses demarca prioridade, logo,
   palavras que governam nominal modifier seriam buscadas e depois o
   mesmo modo acontecendo

		;; qual a interpretação de cada uma?
		(> _ (> _ _ 'nmod) 'nmod)
		(> _ _ 'nmod _ 'nmod)
		(and (> _ _ 'nmod) (> _ _ 'nmod))

- NOUN >amod (\_ >amod|>acl \_) searches for nouns that govern an
  adjectival modifier, where the adjectival modifier itself governs
  either another adjectival modifier or a participial modifier

- [ ] negação de dependências (não é a diferença de T - resultado da
  busca sem negação)

- \_ >nsubj:cop \_ !>cop \_ searches for all copula predicatives
  (governors of nsubj:cop dependents) that do not have a copula verb
  (do not govern a cop dependent)

- \_ <advcl \_ !>mark \_ searches for heads of unmarked adverbial
  clauses (governed by advcl but not governing mark)

- \_ <nsubj \_ !(>amod|>acl) \_ searches for subjects which do not
  govern adjectival or participial modifiers

- \_ <nsubj \_ >!amod _ searches for subjects which governs something
  but it cannot be an adjective (governed by nsubj and governs
  something which is not amod)

> Note that in \_ !>amod \_ it is accepted that the token does not
> have any dependent, whereas in \_ >!amod \_ the token must have at
> least one dependent which is not amod.

    ;; em lisp
    (not (> _ _ 'amod))
    (> _ _ '(not 'amod))

- [ ] busca por dependência com direção

-  **Direção**: determinada por @R ou @L (direita ou esquerda)

    - VERB >nsubj@R \_: fornece verbos que tem nsubj dependente a
      direita

    - \_ >amod@L \_ >amod@R \_ searches for words that have two
      distinct adjectival modifiers (two amod dependencies in
      parallel), one must be at the left side, the other at the right
      side

    - \_ <case@R \_ searches for case markers where the governor token
      is at the right side, i.e. prepositions (as compared to
      postpositions)

> como implementar o acima? fazer

    (> (governor-token governed-token &optional type direction) body)
    ;; mas e como lidar com vários governed token? fazer:
     (> (governor-token &rest governed-tokens) body)
    ;; em que governed token é substituído por quantas listas forem de (governed-token type direction)

- [ ] combinação de queries

- **Query combinadas**: query1 + query2 + query3 fornece todas as
  arvores que contenham as tres queries. O simbolo "+" determina que
  as queries serão combinadas.
  
  > como implementar queries combinadas?

- (VERB >aux \_ >aux \_) + (\_ >conj (\_ >conj \_)) searches for trees
  with a double auxiliary and a nested coordination

- [ ] busca com quantificação universal

- **Universal quantification** 

    - _ -> NOUN: todos os tokens devem ser substantivos retorna frases
    em que todas as palavras são substantivos

	- NOUN -> NOUN <acl:relcl _: todos os substantivo são goveernados
      por acl:relcl
