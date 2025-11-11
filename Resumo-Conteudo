# Fundamentos de Sistemas Operacionais
Processos, Threads, Escalonamento e Sincronização

Este README resume os conceitos fundamentais de sistemas operacionais apresentados, com exemplos corretos (com cálculos) para cada algoritmo de escalonamento citado.

---

## 1. PROCESSOS
Um processo é um programa em execução — ou seja, programa + contexto de execução + espaço de memória próprio.

Cada processo possui:
- Código executável
- Dados (variáveis em memória)
- Contexto (registradores, contador de programa)
- Espaço de memória próprio

Estados de um processo:
- Novo (new): criado, ainda não na fila de execução
- Pronto (ready): esperando a CPU
- Executando (running): usando a CPU
- Bloqueado (waiting): esperando evento (I/O, etc.)
- Finalizado (terminated): terminou execução

Troca de contexto (context switch): salvar/restaurar registradores, contador de programa, estado. Tem custo.

---

## 2. THREADS
Thread = menor unidade de execução dentro de um processo.

Comparação:
- Processo: memória própria, troca cara, comunicação lenta
- Thread: compartilha memória com outras threads do mesmo processo, troca leve, comunicação rápida

Ex.: navegador (processo) → abas como threads.

---

## 3. ESCALONAMENTO DE PROCESSOS
Escalonamento = como o SO escolhe qual processo usar a CPU.

Objetivos:
- maximizar uso da CPU
- reduzir tempo de espera
- evitar starvation
- respeitar prioridades

Tipos:
- Não-preemptivo: processo não é interrompido até terminar ou bloquear
- Preemptivo: processo pode ser interrompido (por quantum, chegada de processo com prioridade maior, etc.)

Fórmulas rápidas:
- Tempo de espera = início − chegada (ou turnaround − burst)
- Tempo de retorno (turnaround) = término − chegada

---

## 4. ALGORITMOS DE ESCALONAMENTO (com exemplos corretos)

Para cada exemplo abaixo: tabela de processos (PID, chegada, burst, [prioridade se aplicável]), Gantt chart, tempos de espera e turnaround, média.

1) FIFO / FCFS (First Come, First Served) — não-preemptivo
- Regras: ordem de chegada.

Processos:
PID | Chegada | Burst
--- | --- | ---
P1 | 0 | 5
P2 | 2 | 3
P3 | 4 | 1

Gantt:
```
P1 [0 — 5] | P2 [5 — 8] | P3 [8 — 9]
```

Cálculos:
- P1: espera = 0, turnaround = 5 − 0 = 5
- P2: espera = 5 − 2 = 3, turnaround = 8 − 2 = 6
- P3: espera = 8 − 4 = 4, turnaround = 9 − 4 = 5

Médias:
- Espera média = (0 + 3 + 4) / 3 = 7/3 ≈ 2.33
- Turnaround médio = (5 + 6 + 5) / 3 = 16/3 ≈ 5.33

Observação: sujeito ao efeito "convoy".

---

2) SJF (Shortest Job First) — não-preemptivo
- Regras: entre os prontos, escolhe o com menor burst. Não interrompe.

Processos (todos chegam em 0 para ilustrar escolha por tamanho do job):
PID | Chegada | Burst
--- | --- | ---
P1 | 0 | 7
P2 | 0 | 4
P3 | 0 | 1

Gantt:
```
P3 [0 — 1] | P2 [1 — 5] | P1 [5 — 12]
```

Cálculos:
- P3: espera = 0, turnaround = 1
- P2: espera = 1 − 0 = 1, turnaround = 5 − 0 = 5
- P1: espera = 5 − 0 = 5, turnaround = 12 − 0 = 12

Médias:
- Espera média = (0 + 1 + 5) / 3 = 6/3 = 2
- Turnaround médio = (1 + 5 + 12) / 3 = 18/3 = 6

Observação: bom tempo médio, mas pode causar starvation de jobs longos.

---

3) SRTF (Shortest Remaining Time First) — preemptivo (SJF preemptivo)
- Regras: a cada chegada, compara tempo restante; preempciona se chegada tem menor restante.

Processos:
PID | Chegada | Burst
--- | --- | ---
P1 | 0 | 8
P2 | 1 | 4
P3 | 2 | 2

Gantt (cronologia com preempções):
```
P1 [0 — 1] | P2 [1 — 2] | P3 [2 — 4] | P2 [4 — 7] | P1 [7 — 14]
```

Cálculos (fim − chegada = turnaround; espera = turnaround − burst):
- P1: término = 14 → turnaround = 14 − 0 = 14 → espera = 14 − 8 = 6
- P2: término = 7 → turnaround = 7 − 1 = 6 → espera = 6 − 4 = 2
- P3: término = 4 → turnaround = 4 − 2 = 2 → espera = 2 − 2 = 0

Médias:
- Espera média = (6 + 2 + 0) / 3 = 8/3 ≈ 2.67
- Turnaround médio = (14 + 6 + 2) / 3 = 22/3 ≈ 7.33

Observação: preempção melhora resposta para jobs curtos, mas aumenta trocas de contexto.

---

4) PRIORIDADE (menor número = maior prioridade)
- Implementações: não-preemptiva e preemptiva. Problema comum: starvation; solução: aging.

Exemplo (mostrando preempção):
PID | Chegada | Burst | Prioridade (menor é mais alta)
--- | --- | --- | ---
P1 | 0 | 10 | 2
P2 | 1 | 1  | 1
P3 | 2 | 2  | 3

- Não-preemptivo (se P1 começar em 0, ele vai até o fim → P2 e P3 esperam; má resposta).
- Preemptivo: se P1 está em execução e P2 (prioridade 1, maior que P1) chega, P2 preempciona.

Gantt (preemptivo):
```
P1 [0 — 1] | P2 [1 — 2] | P1 [2 — 12] | P3 [12 — 14]
```

Cálculos:
- P1: término = 12 → turnaround = 12 − 0 = 12 → espera = 12 − 10 = 2
- P2: término = 2 → turnaround = 2 − 1 = 1 → espera = 1 − 1 = 0
- P3: término = 14 → turnaround = 14 − 2 = 12 → espera = 12 − 2 = 10

Observação: P3 sofreu muito (starvation possível). Aging: aumentar prioridade de processos que esperam muito (diminuir seu número de prioridade ao longo do tempo) para evitar starvation.

---

5) ROUND ROBIN (RR)
- Regras: cada processo tem um quantum (time slice); ao acabar quantum, se não terminou, vai para o fim da fila. Muito pequeno → muitas trocas; muito grande → tende a FIFO.

Exemplo (quantum = 3):
PID | Chegada | Burst
--- | --- | ---
P1 | 0 | 5
P2 | 1 | 4
P3 | 2 | 2

Gantt (quantum 3):
```
P1 [0 — 3] | P2 [3 — 6] | P3 [6 — 8] | P1 [8 — 10] | P2 [10 — 11]
```

Cálculos:
- P1: término = 10 → turnaround = 10 − 0 = 10 → espera = 10 − 5 = 5
- P2: término = 11 → turnaround = 11 − 1 = 10 → espera = 10 − 4 = 6
- P3: término = 8  → turnaround = 8 − 2  = 6 → espera = 6 − 2 = 4

Médias:
- Espera média = (5 + 6 + 4) / 3 = 15/3 = 5
- Turnaround médio = (10 + 10 + 6) / 3 = 26/3 ≈ 8.67

Observação: escolha do quantum é crítica.

---

6) Múltiplas Filas com Realimentação (MFQ — Multilevel Feedback Queue)
- Regras gerais: várias filas com prioridades diferentes e quantums diferentes; processos que usam toda a fatia descem de fila; processos que aguardam muito podem subir (aging).

Exemplo simples (3 filas):
- Fila 1 (mais alta): quantum = 2
- Fila 2: quantum = 4
- Fila 3 (mais baixa): FIFO (sem preempção por quantum)

Processos:
PID | Chegada | Burst
--- | --- | ---
P1 | 0 | 10
P2 | 0 | 4
P3 | 1 | 6

Execução (política típica: sempre esvazia/serve fila mais alta disponível):
- t0–2: P1 (usou quant=2, rem=8 → desce para Fila2)
- t2–4: P2 (usou quant=2, rem=2 → desce para Fila2)
- t4–6: P3 (chegou em t1; usa quant=2, rem=4 → desce para Fila2)
- Agora Fila1 vazia → serve Fila2 com quant=4:
  - t6–10: P1 (usa quant=4, rem=4 → excedeu quant → desce para Fila3)
  - t10–12: P2 (rem=2 ≤ quant4 → termina)
  - t12–16: P3 (rem=4 ≤ quant4 → termina)
- Fila3:
  - t16–20: P1 (rem=4 → termina)

Gantt:
```
P1 [0—2] | P2 [2—4] | P3 [4—6] | P1 [6—10] | P2 [10—12] | P3 [12—16] | P1 [16—20]
```

Observações:
- MFQ é adaptativo: processos CPU-bound tendem a descer para filas de menor prioridade (menos preempção); I/O-bound/curtos tendem a permanecer em filas superiores, ganhando mais responsividade.
- Aging pode ser aplicado (subir filas) para evitar starvation.

---

## 5. SINCRONIZAÇÃO DE PROCESSOS
Quando processos/threads acessam recurso compartilhado, surge risco de condições de corrida.

- Condição de corrida (race condition): duas ou mais execuções acessam/modificam mesmo dado simultaneamente causando resultado inconsistente.
- Seção crítica: trecho de código que acessa recurso compartilhado. Regra: apenas um processo por vez na seção crítica.

---

## 6. SEMÁFOROS
Semáforo = variável especial para controlar acesso a recursos.

Operações:
- P() ou wait(): decrementa. Se < 0, bloqueia o processo.
- V() ou signal(): incrementa. Se houvesssem processos bloqueados, libera um.

Exemplo clássico: Produtor–Consumidor com buffer circular (pseudocódigo):

```
semaphores:
  mutex = 1           // exclusão mútua
  vagas = N           // número de vagas livres no buffer
  itens = 0           // itens disponíveis

Produtor:
  while (true) {
    produzir_item()
    P(vagas)          // aguarda vaga livre
    P(mutex)          // entra em seção crítica
    inserir_no_buffer(item)
    V(mutex)          // sai da seção crítica
    V(itens)          // sinaliza que há item disponível
  }

Consumidor:
  while (true) {
    P(itens)          // aguarda item disponível
    P(mutex)          // entra em seção crítica
    item = remover_do_buffer()
    V(mutex)          // sai da seção crítica
    V(vagas)          // sinaliza vaga livre
    consumir_item(item)
  }
```

Semáforos resolvem exclusão mútua e sincronização entre produtores/consumidores.

---

## PALAVRAS-CHAVE (resumo rápido)
- Processo: Programa em execução
- Thread: Execução dentro do processo
- FIFO: Ordem de chegada
- SJF: Processo mais curto
- SRTF: Menor tempo restante (preempta)
- PRIORIDADE: Menor número = maior prioridade
- RR: Quantum / fila circular
- Starvation: Processo esquecido
- Aging: Aumenta prioridade com o tempo
- Seção crítica: Recurso compartilhado
- Semáforo: Controle P() e V()

---
