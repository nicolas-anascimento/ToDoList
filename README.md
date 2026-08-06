# ToDoList

Uma lista de tarefas feita com **Vue 3** e **TypeScript**, criada como projeto de estudo para praticar os conceitos principais do Vue: reatividade, componentes, consumo de API e persistência de dados.

## Sobre o projeto

A aplicação permite criar, concluir e remover tarefas, além de filtrar a lista por status. Ao abrir pela primeira vez, ela busca algumas tarefas de exemplo em uma API pública; nas próximas vezes, carrega as tarefas salvas localmente no navegador, então nada se perde ao recarregar a página.

## Funcionalidades

- Adicionar novas tarefas
- Marcar/desmarcar tarefas como concluídas
- Remover tarefas
- Ver quantas tarefas ainda estão pendentes
- Filtrar entre Todas, Não concluídas e Concluídas
- Carregamento inicial de tarefas de exemplo via API pública ([JSONPlaceholder](https://jsonplaceholder.typicode.com/))
- Tarefas salvas automaticamente no navegador (`localStorage`), sem precisar de backend

## Tecnologias

- Vue 3 (Composition API)
- TypeScript
- Vite

## Autor

[nicolas-anascimento](https://github.com/nicolas-anascimento)
