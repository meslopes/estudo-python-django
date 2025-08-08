# Conteúdo de Estudo Python

Este documento contém resumos e exemplos de código Python, extraídos de uma conversa interativa, para auxiliar em seus estudos e consultas futuras.

## Gerenciador de Tarefas Pessoais

### Objetivo do Programa

Criação de um sistema de controle de tarefas pessoais (to-do list) com as seguintes funcionalidades:

*   Criar tarefas
*   Listar tarefas pendentes e concluídas
*   Marcar tarefas como feitas
*   Salvar dados em um arquivo para persistência

### Planejamento Básico

*   **Entrada e Saída de Dados**: Interação via terminal, com comandos ou opções de menu.
*   **Estrutura de Dados**: Tarefas guardadas como dicionários em uma lista.
*   **Persistência**: Salvar dados em um arquivo `.json`.

### Código Base

```python
import json
import os

TAREFAS_ARQUIVO = 'tarefas.json'

def carregar_tarefas():
    if os.path.exists(TAREFAS_ARQUIVO):
        with open(TAREFAS_ARQUIVO, 'r') as f:
            return json.load(f)
    return []

def salvar_tarefas(tarefas):
    with open(TAREFAS_ARQUIVO, 'w') as f:
        json.dump(tarefas, f)

def mostrar_menu():
    print("\n=== Gerenciador de Tarefas ===")
    print("1. Adicionar tarefa")
    print("2. Listar tarefas")
    print("3. Marcar como concluída")
    print("4. Sair")

def adicionar_tarefa(tarefas):
    titulo = input("Digite o título da tarefa: ")
    tarefas.append({'titulo': titulo, 'concluida': False})
    print("✅ Tarefa adicionada!")

def listar_tarefas(tarefas):
    if not tarefas:
        print("📭 Nenhuma tarefa cadastrada.")
        return
    for i, t in enumerate(tarefas):
        status = "✔️" if t['concluida'] else "❌"
        print(f"{i + 1}. {t['titulo']} [{status}]")

def concluir_tarefa(tarefas):
    listar_tarefas(tarefas)
    try:
        idx = int(input("Número da tarefa concluída: ")) - 1
        tarefas[idx]['concluida'] = True
        print("🎉 Tarefa concluída!")
    except:
        print("⚠️ Entrada inválida.")

def main():
    tarefas = carregar_tarefas()
    while True:
        mostrar_menu()
        escolha = input("Escolha uma opção: ")
        if escolha == '1':
            adicionar_tarefa(tarefas)
        elif escolha == '2':
            listar_tarefas(tarefas)
        elif escolha == '3':
            concluir_tarefa(tarefas)
        elif escolha == '4':
            salvar_tarefas(tarefas)
            print("Até mais! 👋")
            break
        else:
            print("Opção inválida!")

main()
```

### Possíveis Melhorias

*   Interface gráfica com Tkinter
*   Notificações de prazos usando `datetime`
*   Classificar tarefas por prioridade
*   Usar banco de dados (SQLite) em vez de JSON

## Cadastro de Restaurantes

### Funcionalidades Básicas

1.  **Cadastrar restaurantes**:
    *   Nome, tipo de comida, cidade, telefone, nota (opcional)
2.  **Listar todos os restaurantes cadastrados**
3.  **Buscar restaurantes por cidade ou tipo de comida**
4.  **Editar ou excluir cadastro**
5.  **Salvar os dados em um arquivo JSON**

### Estrutura Inicial do Projeto

```python
import json
import os

ARQUIVO = 'restaurantes.json'

def carregar_dados():
    if os.path.exists(ARQUIVO):
        with open(ARQUIVO, 'r') as f:
            return json.load(f)
    return []

def salvar_dados(restaurantes):
    with open(ARQUIVO, 'w') as f:
        json.dump(restaurantes, f, indent=4)

def cadastrar(restaurantes):
    nome = input("Nome do restaurante: ")
    tipo = input("Tipo de comida (ex: japonesa, italiana): ")
    cidade = input("Cidade: ")
    telefone = input("Telefone: ")
    nota = input("Nota (opcional): ")
    restaurantes.append({
        'nome': nome,
        'tipo': tipo,
        'cidade': cidade,
        'telefone': telefone,
        'nota': nota
    })
    print("✅ Restaurante cadastrado!")

def listar(restaurantes):
    if not restaurantes:
        print("📭 Nenhum restaurante cadastrado.")
        return
    for i, r in enumerate(restaurantes):
        print(f"{i + 1}. Nome: {r['nome']}, Tipo: {r['tipo']}, Cidade: {r['cidade']}, Telefone: {r['telefone']}, Nota: {r['nota'] if r['nota'] else 'N/A'}")

def buscar(restaurantes):
    termo = input("Buscar por cidade ou tipo de comida: ").lower()
    encontrados = [r for r in restaurantes if termo in r['cidade'].lower() or termo in r['tipo'].lower()]
    if not encontrados:
        print("🔎 Nenhum restaurante encontrado com esse termo.")
        return
    for i, r in enumerate(encontrados):
        print(f"{i + 1}. Nome: {r['nome']}, Tipo: {r['tipo']}, Cidade: {r['cidade']}")

def editar(restaurantes):
    listar(restaurantes)
    try:
        idx = int(input("Número do restaurante para editar: ")) - 1
        if 0 <= idx < len(restaurantes):
            restaurante = restaurantes[idx]
            restaurante['nome'] = input(f"Nome ({restaurante['nome']}): ") or restaurante['nome']
            restaurante['tipo'] = input(f"Tipo ({restaurante['tipo']}): ") or restaurante['tipo']
            restaurante['cidade'] = input(f"Cidade ({restaurante['cidade']}): ") or restaurante['cidade']
            restaurante['telefone'] = input(f"Telefone ({restaurante['telefone']}): ") or restaurante['telefone']
            restaurante['nota'] = input(f"Nota ({restaurante['nota']}): ") or restaurante['nota']
            print("✏️ Restaurante atualizado!")
        else:
            print("⚠️ Entrada inválida.")
    except ValueError:
        print("⚠️ Entrada inválida.")

def excluir(restaurantes):
    listar(restaurantes)
    try:
        idx = int(input("Número do restaurante para excluir: ")) - 1
        if 0 <= idx < len(restaurantes):
            restaurantes.pop(idx)
            print("🗑️ Restaurante excluído!")
        else:
            print("⚠️ Entrada inválida.")
    except ValueError:
        print("⚠️ Entrada inválida.")

def main_restaurantes():
    restaurantes = carregar_dados()
    while True:
        print("\n=== Gerenciador de Restaurantes ===")
        print("1. Cadastrar restaurante")
        print("2. Listar restaurantes")
        print("3. Buscar restaurante")
        print("4. Editar restaurante")
        print("5. Excluir restaurante")
        print("6. Sair")
        escolha = input("Escolha uma opção: ")

        if escolha == '1':
            cadastrar(restaurantes)
        elif escolha == '2':
            listar(restaurantes)
        elif escolha == '3':
            buscar(restaurantes)
        elif escolha == '4':
            editar(restaurantes)
        elif escolha == '5':
            excluir(restaurantes)
        elif escolha == '6':
            salvar_dados(restaurantes)
            print("Até mais! 👋")
            break
        else:
            print("Opção inválida!")

# Para executar o programa de restaurantes, descomente a linha abaixo:
# main_restaurantes()
```

### Possíveis Evoluções

*   Interface gráfica
*   Uso de banco de dados (ex: SQLite)
*   Funcionalidades de avaliação e comentários

## Explicação de Código JavaScript (Importante: Não é Python)

Foi identificado um trecho de código JavaScript no início da conversa. É crucial notar que este código **não é Python**.

```javascript
import path from 'node:path';
```

### O que essa linha faz:

*   `import path`: Importa o módulo chamado `path`, que oferece várias funções para lidar com caminhos de arquivos e diretórios.
*   `from 'node:path'`: Especifica que esse módulo vem da biblioteca padrão do Node.js.

### Utilidade do Módulo `path` (Node.js):

*   **Juntar partes de um caminho** (`path.join`)
*   **Encontrar o nome de um arquivo** (`path.basename`)
*   **Verificar extensões** (`path.extname`)
*   **Transformar caminhos relativos em absolutos** (`path.resolve`)


