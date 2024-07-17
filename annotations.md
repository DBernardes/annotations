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

