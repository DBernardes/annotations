# Denis' annotations

## Gitflow

- Development branch might be a good idea to keep the main brach safe. It is helpful when you have a lot of collaobrators.
- É melhor ter branches pequenas do que grandes
- É bom deletar as branches resolvidas. Na verdade apenas limpa a lista, mas não deleta totalmente
- Posso remover as issues quando resolvidas. Posso referenciar a PR onde isso aconteceu

## Issues
- uma lista de coisas que precisam ser monitoradas no repo
- repos geralmente tem um padrão para os issues
- podem ser assinados por pessoas, possuem label para saber do que se trata
- milestones sobre as features atingidas
- recebem um número da issue
- Podem ser endereçadas nos commits

## Github mechanics
- quando dou um git undo, ele cria um novo commit com as mudanças apostas do ultimo commit
- os commits são imultáveis, nunca mude um commit, isso pode quebrar o repo


## Debugging
- criar um launch.json para configurar como rodar o debuger
- Criar breakpoints condicionais para loops (ex: i=35)

## Documentaion and Releasing code
- __version__ = "3.0.0"
- __init__.py roda tudo como a função __init__ de uma classe
- Manter github and pypi sincronizados
- Zenodo, permite citar codigo
- JOSS para open source software, focado para revisar seu código

## Code profiling
- cProfile indica a quantidade de tempo que minhas funções precisam para executar.
Chama a função 1000 vezes e passa o número de cores a serem usados.
Igual as estatísticas do Labview.
Ajuda a descobrir qual função speed up

### Como speed up
- use numpy sempre que possível
- evitar for e while loops
- codar em C e chamar em python


## Interview
- Maiorias das contratações são via networking
- Nasa tem diversos projetos instrumentais acontecendo
- Devo preparar meu CV com as keywords que o recutrador está procurando.
Ele recebe diversos de CVs todos os dias, logo, ele procura pelas keywords certas
As keywords são minhas habilidades. Isto deve aparecer na descrição de cada experiência.
Estruturar como atividade -> resultado.
- Na indústria é visto como um mindset ruim ter um PhD.
- Entender AWS?
- O mindset da industria está voltava para cumprir schedules rapida e eficientemente
- Foi uma experiência muito intensa para a Sarah. Eles se preocupavam com cada linha de código e cobravam isso.
Ela passou algumas semanas sem fazer um commit na main por isso.
- Essa pessoa vai nos fazer dinheiro? Precisa ter uma venda própria de suas capacidades.


## Add data in my package
- [mylink](https://sixty-north.com/blog/including-package-data-in-python-packages.html)