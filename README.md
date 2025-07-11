# 🚚 Algoritmo de Prioridade de Pedidos

## ⚙️ Constantes de Peso
- `ALFA` 🟦 **1.0** — Peso para prioridade (`priorityScore`)
- `BETA` 🟨 **100.0** — Peso para inverso do prazo de expedição (`dispatchWindow`)
- `GAMA` 🟩 **5.0** — Peso para tamanho do pacote (`sizeWeight`)

---

## 📦 Estrutura do Pedido
```plaintext
Pedido:
  🆔 id                // Identificador único
  ⭐ priorityScore      // Score de prioridade (0 a 100)
  ⏳ dispatchWindow     // Minutos até expedição
  📏 sizeCategory      // Tamanho: 'P', 'M', 'G'
  ⚖️ sizeWeight        // Peso numérico do tamanho
  🧮 compositeScore    // Score final para ordenação
```

---

## 📏 Função: Peso do Tamanho
```plaintext
obterPesoTamanho(tamanho):
  se tamanho == 'P' → retorne 1
  senão se tamanho == 'M' → retorne 2
  senão → retorne 3
```

---

## 🧮 Função: Score Composto
```plaintext
calcularScore(priorityScore, dispatchWindow, sizeCategory):
  peso ← obterPesoTamanho(sizeCategory)
  retorne (ALFA * priorityScore) - (BETA * (1.0 / dispatchWindow)) + (GAMA * peso)
```

---

## 🗂️ Heap de Prioridade
- `filaPrioridade[1000]` — Vetor de pedidos
- `tamanhoHeap` — Tamanho atual do heap

---

## ➕ Inserir Pedido no Heap
```plaintext
inserirNaFila(p):
  tamanhoHeap ← tamanhoHeap + 1
  i ← tamanhoHeap
  enquanto i > 1 e filaPrioridade[i/2].compositeScore < p.compositeScore:
    filaPrioridade[i] ← filaPrioridade[i/2]
    i ← i / 2
  filaPrioridade[i] ← p
```

---

## ➖ Remover Pedido com Maior Prioridade
```plaintext
removerProximoPedido():
  topo ← filaPrioridade[1]
  ultimo ← filaPrioridade[tamanhoHeap]
  tamanhoHeap ← tamanhoHeap - 1
  i ← 1
  filho ← 2
  enquanto filho ≤ tamanhoHeap:
    se filho < tamanhoHeap e filaPrioridade[filho].compositeScore < filaPrioridade[filho+1].compositeScore:
      filho ← filho + 1
    se ultimo.compositeScore ≥ filaPrioridade[filho].compositeScore:
      pare
    filaPrioridade[i] ← filaPrioridade[filho]
    i ← filho
    filho ← 2 * i
  filaPrioridade[i] ← ultimo
  retorne topo
```

---

## ▶️ Fluxo Principal
```plaintext
principal():
  // Pedido 1
  p1.id ← 1
  p1.priorityScore ← 80
  p1.dispatchWindow ← 20
  p1.sizeCategory ← 'M'
  p1.sizeWeight ← obterPesoTamanho(p1.sizeCategory)
  p1.compositeScore ← calcularScore(p1.priorityScore, p1.dispatchWindow, p1.sizeCategory)
  inserirNaFila(p1)

  // Pedido 2
  p2.id ← 2
  p2.priorityScore ← 90
  p2.dispatchWindow ← 30
  p2.sizeCategory ← 'P'
  p2.sizeWeight ← obterPesoTamanho(p2.sizeCategory)
  p2.compositeScore ← calcularScore(p2.priorityScore, p2.dispatchWindow, p2.sizeCategory)
  inserirNaFila(p2)

  // Despachar pedido
  proximo ← removerProximoPedido()
  escreva("🚀 Despachando pedido ID: ", proximo.id)

