{
  "nodes": [
    {
      "id": "mba-core",
      "type": "text",
      "x": 0,
      "y": 0,
      "width": 400,
      "height": 100,
      "text": "🧠 **MBA_COGNITIVE**\nObjetivo: Consolidar todo o conteúdo e raciocínio do MBA sob estrutura de conhecimento digital (Obsidian + IA)"
    },
    {
      "id": "disciplinas",
      "type": "file",
      "x": -500,
      "y": -200,
      "width": 250,
      "height": 100,
      "file": "disciplinas.md"
    },
    {
      "id": "projetos",
      "type": "file",
      "x": 500,
      "y": -200,
      "width": 250,
      "height": 100,
      "file": "projetos.md"
    },
    {
      "id": "leituras",
      "type": "file",
      "x": -500,
      "y": 200,
      "width": 250,
      "height": 100,
      "file": "leituras.md"
    },
    {
      "id": "referencias",
      "type": "file",
      "x": 500,
      "y": 200,
      "width": 250,
      "height": 100,
      "file": "referencias.md"
    },
    {
      "id": "insights",
      "type": "file",
      "x": 0,
      "y": 300,
      "width": 250,
      "height": 100,
      "file": "insights.md"
    },
    {
      "id": "tarefas",
      "type": "file",
      "x": -300,
      "y": 500,
      "width": 250,
      "height": 100,
      "file": "tarefas.md"
    },
    {
      "id": "curadoria",
      "type": "file",
      "x": 300,
      "y": 500,
      "width": 250,
      "height": 100,
      "file": "curadoria.md"
    },
    {
      "id": "linha-do-tempo",
      "type": "file",
      "x": 0,
      "y": 650,
      "width": 250,
      "height": 100,
      "file": "linha-do-tempo.canvas"
    }
  ],
  "edges": [
    { "fromNode": "mba-core", "toNode": "disciplinas" },
    { "fromNode": "mba-core", "toNode": "projetos" },
    { "fromNode": "mba-core", "toNode": "leituras" },
    { "fromNode": "mba-core", "toNode": "referencias" },
    { "fromNode": "mba-core", "toNode": "insights" },
    { "fromNode": "mba-core", "toNode": "tarefas" },
    { "fromNode": "mba-core", "toNode": "curadoria" },
    { "fromNode": "mba-core", "toNode": "linha-do-tempo" }
  ]
}
