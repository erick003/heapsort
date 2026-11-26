# 📚 Guia Completo de Estudo: Algoritmo Heapsort

**Integrantes do Grupo:**
* Wagner Alves
* João Gabriel
* Bruno Dias
* Erick Carlos

---

## Parte 1: Guia de Estudo Simples e Prático

### 1.1 O que é o Heapsort? (A Ideia Intuitiva)
Imagine que você tem uma pilha bagunçada de provas com notas diferentes e precisa organizá-las da menor para a maior. Em vez de sair procurando a maior nota olhando prova por prova toda vez, você organiza essas provas em uma **pirâmide hierárquica** (como o organograma de uma empresa).

Nessa pirâmide, existe apenas uma regra de ouro: **nenhum subordinado pode ter uma nota maior que a do seu chefe direto**. 

Se você respeitar essa regra, quem sempre estará no topo absoluto da pirâmide (o "presidente")? **A maior nota de todas!**

O **Heapsort** funciona exatamente assim:
1. Ele transforma um vetor comum nessa pirâmide organizada (chamada de *Max-Heap*).
2. Ele pega o "presidente" (o maior elemento no topo) e o coloca no final do vetor, pois aquele já é o lugar definitivo dele.
3. Ele reorganiza a pirâmide com os elementos que sobraram para descobrir o próximo maior elemento e repete o processo até que todos estejam ordenados.

---

### 1.2 Aspectos Teóricos Fundamentais

#### A Estrutura de Dados: Árvore Binária Completa em um Vetor
Embora a gente visualize o *Heap* como uma árvore (pirâmide), na memória do computador ele é armazenado como um **vetor comum (array)**, sem gastar memória extra com ponteiros. 

Para um elemento na posição (índice) `i` de um vetor que começa no índice `0`, encontramos seus parentes usando matemática simples:
* **Pai:** `(i - 1) / 2`
* **Filho da Esquerda:** `2 * i + 1`
* **Filho da Direita:** `2 * i + 2`

#### Propriedade de Max-Heap
Para ordenar em ordem crescente, usamos um **Max-Heap**, onde para todo nó `i`:
> `Vetor[Pai(i)] >= Vetor[i]`

#### Complexidade Assintótica (Análise de Desempenho)
O grande trunfo do Heapsort é a sua **previsibilidade**. Diferente do Quicksort (que pode degringolar para `O(n²)` se o pivô for mal escolhido), o Heapsort garante o mesmo tempo em qualquer cenário:

| Métrica | Complexidade | Explicação |
| :--- | :---: | :--- |
| **Melhor Caso** | `O(n log n)` | Mesmo já ordenado, precisa construir e desmontar o heap. |
| **Caso Médio** | `O(n log n)` | Desempenho padrão consistente. |
| **Pior Caso** | `O(n log n)` | Nunca piora além disso; excelente para sistemas críticos. |
| **Espaço Auxiliar** | `O(1)` | É um algoritmo *in-place* (ordena no próprio vetor). |
| **Estabilidade** | Não estável | Elementos iguais podem trocar de ordem relativa. |

---

### 1.3 Aspectos Práticos: Como o Algoritmo Funciona Passo a Passo?

O algoritmo é dividido em duas etapas principais:

#### Etapa 1: `heapify` (A operação de "afundar" / corrigir)
Imagine que você colocou um estagiário no cargo de presidente. Ele não tem experiência (valor menor que seus subordinados). A função `heapify` compara esse elemento com seus dois filhos. Se algum filho for maior, eles trocam de lugar. O elemento vai "afundando" na árvore até encontrar uma posição onde seja maior que seus filhos (ou chegar à base).
* **Custo:** `O(log n)`, pois a altura da árvore é `log2(n)`.

#### Etapa 2: Construção e Ordenação
1. **Build Max-Heap:** Começamos do último nó que possui filhos (índice `n/2 - 1`) e vamos subindo até a raiz (índice `0`), aplicando o `heapify`. Isso transforma qualquer vetor bagunçado em um Max-Heap válido em tempo linear `O(n)`.
2. **Extração (Sort):** 
   * Trocamos a raiz (índice `0`, o maior elemento) com o último elemento do vetor (`n - 1`).
   * Reduzimos o tamanho considerado do heap em `1` (isolando o maior elemento que já está ordenado no fim).
   * Aplicamos `heapify` na nova raiz (`0`) para restaurar a pirâmide.
   * Repetimos até sobrar apenas 1 elemento.

#### Teste de Mesa Rápido
Vetor inicial: `[4, 10, 3, 5, 1]`

1. **Após Build Max-Heap:** O `10` sobe para o topo. Vetor vira: `[10, 5, 3, 4, 1]`
2. **Iteração 1:** Troca `10` com `1` -> `[1, 5, 3, 4 | 10]`. Aplica `heapify` no `1` -> `[5, 4, 3, 1 | 10]`.
3. **Iteração 2:** Troca `5` com `1` -> `[1, 4, 3 | 5, 10]`. Aplica `heapify` no `1` -> `[4, 1, 3 | 5, 10]`.
4. **Iteração 3:** Troca `4` com `3` -> `[3, 1 | 4, 5, 10]`. Aplica `heapify` no `3` -> `[3, 1 | 4, 5, 10]`.
5. **Iteração 4:** Troca `3` com `1` -> `[1 | 3, 4, 5, 10]`. Fim!

---

## Parte 2: Situação-Problema (Estilo Maratona de Programação)

### Problema: O Sistema de Telemetria da Missão Artemis-X
**Tempo Limite:** 1.0 segundo | **Limite de Memória:** 64 MB *(Restrição severa de memória!)*

#### Contextualização
A agência espacial está monitorando os propulsores de um foguete durante a fase crítica de decolagem. Os sensores enviam pacotes de telemetria contendo:
* `ID_Sensor` (inteiro)
* `Nivel_Criticidade` (inteiro de `1` a `1.000.000.000`)
* `Timestamp_Falha` (inteiro representando milissegundos desde a ignição)

Devido ao hardware embarcado de bordo ter **memória RAM extremamente limitada (64 MB)** e exigir **garantia de tempo real** (sem risco de travamentos por pior caso `O(n²)`), algoritmos como *Mergesort* (que exige memória auxiliar `O(n)`) e *Quicksort* (que pode sofrer ataques ou dados adversários degradando para `O(n²)`) foram proibidos pela equipe de engenharia.

#### Tarefa
Você recebeu um lote de `N` registros de anomalias (`1 <= N <= 200.000`). Você deve ordenar todos os registros *in-place* utilizando **Heapsort** seguindo os critérios de prioridade abaixo para exibir os alertas do **mais urgente para o menos urgente** (ou seja, organizar o vetor final em ordem decrescente de urgência):

1. Maior `Nivel_Criticidade` primeiro.
2. Em caso de empate na criticidade, o menor `Timestamp_Falha` (anomalia que ocorreu mais cedo) tem maior urgência.
3. Persistindo o empate, o menor `ID_Sensor` tem maior urgência.

#### Entrada
A primeira linha contém um inteiro `N`. As próximas `N` linhas contêm três inteiros cada: `ID_Sensor`, `Nivel_Criticidade` e `Timestamp_Falha`.

#### Saída
Imprima os `N` registros ordenados conforme as regras de prioridade.

---

### Solução Comentada em Java

Para ordenar o vetor final em **ordem decrescente** (do mais urgente para o menos urgente) mantendo o algoritmo clássico do Heapsort, construímos um **Min-Heap** (onde o *menos* urgente fica no topo e vai sendo jogado para o final do vetor a cada iteração).

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class TelemetriaHeapsort {

    // Classe que representa cada pacote de telemetria
    static class Registro {
        int idSensor;
        int criticidade;
        long timestamp;

        public Registro(int idSensor, int criticidade, long timestamp) {
            this.idSensor = idSensor;
            this.criticidade = criticidade;
            this.timestamp = timestamp;
        }
    }

    /**
     * Define a ordem de prioridade entre dois registros.
     * Retorna true se 'a' for MAIS URGENTE que 'b'.
     */
    private static boolean ehMaisUrgente(Registro a, Registro b) {
        if (a.criticidade != b.criticidade) {
            return a.criticidade > b.criticidade; // Maior criticidade vence
        }
        if (a.timestamp != b.timestamp) {
            return a.timestamp < b.timestamp; // Menor timestamp (mais antigo) vence
        }
        return a.idSensor < b.idSensor; // Menor ID vence
    }

    /**
     * Implementação do Heapsort In-Place.
     * Como queremos o vetor final do MAIS urgente para o MENOS urgente,
     * montamos um Min-Heap (o menos urgente fica no topo e vai para o fim).
     */
    public static void heapSort(Registro[] arr) {
        int n = arr.length;

        // 1. Constrói o Min-Heap (rearranja o array)
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }

        // 2. Extrai um por um os elementos do heap
        for (int i = n - 1; i > 0; i--) {
            // Move a raiz atual (o menos urgente restante) para o final
            Registro temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            // Chama heapify na árvore reduzida
            heapify(arr, i, 0);
        }
    }

    /**
     * Garante a propriedade de Min-Heap para a subárvore com raiz no índice i.
     * 'n' é o tamanho atual do heap.
     */
    private static void heapify(Registro[] arr, int n, int i) {
        int menosUrgente = i; // Inicializa como raiz
        int esquerda = 2 * i + 1;
        int direita = 2 * i + 2;

        // Se o filho da esquerda existe e é MENOS urgente que a raiz atual
        if (esquerda < n && ehMaisUrgente(arr[menosUrgente], arr[esquerda])) {
            menosUrgente = esquerda;
        }

        // Se o filho da direita existe e é MENOS urgente que o menor atual
        if (direita < n && ehMaisUrgente(arr[menosUrgente], arr[direita])) {
            menosUrgente = direita;
        }

        // Se o menos urgente não for a raiz, troca e continua afundando
        if (menosUrgente != i) {
            Registro troca = arr[i];
            arr[i] = arr[menosUrgente];
            arr[menosUrgente] = troca;

            heapify(arr, n, menosUrgente);
        }
    }

    public static void main(String[] args) throws IOException {
        // Uso de BufferedReader e StringBuilder para I/O rápido (padrão Maratona)
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String linha = br.readLine();
        if (linha == null || linha.trim().isEmpty()) return;

        int n = Integer.parseInt(linha.trim());
        Registro[] registros = new Registro[n];

        for (int i = 0; i < n; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int id = Integer.parseInt(st.nextToken());
            int crit = Integer.parseInt(st.nextToken());
            long time = Long.parseLong(st.nextToken());
            registros[i] = new Registro(id, crit, time);
        }

        // Executa a ordenação garantida em O(N log N) e O(1) de espaço extra
        heapSort(registros);

        // Saída bufferizada para evitar Time Limit Exceeded (TLE)
        StringBuilder sb = new StringBuilder();
        for (Registro r : registros) {
            sb.append(r.idSensor).append(" ")
              .append(r.criticidade).append(" ")
              .append(r.timestamp).append("\n");
        }
        System.out.print(sb.toString());
    }
}
```

#### Exemplo de Entrada e Saída
**Entrada:**
```text
5
104 80 5000
101 95 4200
105 80 4100
102 95 4200
103 50 1000
```

**Saída:**
```text
101 95 4200
102 95 4200
105 80 4100
104 80 5000
103 50 1000
```
*(Note como `101` e `102` empataram na criticidade `95` e no timestamp `4200`, sendo desempatados pelo menor `ID_Sensor`!)*
