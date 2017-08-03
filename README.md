# query-conllu

building a new query and database system for
the [cl-conllu](https://github.com/own-pt/cl-conllu) library. this is
a project of the [ED-2017-2](https://github.com/arademaker/ED-2017-2)
course.

## to-do

- [ ] Documentação pronta até 04/08 (de manhã)

## Query

- objetivo: conseguir a mesma capacidade de
  query
  [dessa linguagem](http://bionlp.utu.fi/searchexpressions-new.html)

- Gerar as estruturas utilizando tokens de forma a criar todas as
  opções combinações possíveis:

	- AND, OR, NOT, ...
	
	- Exemplo enviado por email:

			VERB <advcl L=correr&VERB
			L=correr&VERB >advcl VERB
				 (subj (advcl (and (upostag ~ "VERB")
					   (lemma ~ "correr"))
						(upostag ~ "VERB"))
				 (upostag ~ "PROP"))

## Base de Dados

- Documentos da base
  [Bosque-UD](https://github.com/own-pt/bosque-UD/tree/master/documents)

- Estrutura: binary tree ou hash table?
