# link do problema: https://leetcode.com/problems/two-sum/

# Descrição do problema

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9
**Output:** [0,1]
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

**Input:** nums = [3,2,4], target = 6
**Output:** [1,2]

**Example 3:**

**Input:** nums = [3,3], target = 6
**Output:** [0,1]

**Constraints:**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than `O(n2)` time complexity?

*Minha resolução inicial* (os comentários são antes de revisar a syntax)
![[{8255401D-61E5-4D89-9DF1-F88B51CD3940}.png]]
Eu pensei inicialmente numa lógica obvia de comparação buble sort ( mas o exercicio explicita evitar usar n*n ) então num desenho alternativo pensei numa solução simples de O(n) 
![[IMG_20260331_221443.jpg]]

A ideia inicialmente era ver qual valor era maior comparar com um valor resultante (faltante que apelidei nesse caso), mas logo percebi os problemas de pensar dessa forma:
O primeiro problema em pensar nessa lógica é que: 
1 - O par correto nem sempre envolve o maior número.
um exemplo simples:

nums = [3, 3]
target = 6

O retorno é: [0, 0]

Analisando no ChatGpt logo vi que a solução padrão ou esperada é usar um hash map para armazenar os números e seus índices, permitindo uma busca eficiente para encontrar o complemento necessário para atingir o target.

porém apesar de já ter estudado sobre dicionários e hash maps, não tinha pensando em usar essa estrutura para resolver esse problema em específico, o que me fez perceber a importância de conhecer bem as estruturas de dados e suas aplicações práticas.

Então fui atrás da explicação,

Um map (ou dicionário) é uma estrutura de dados que armazena pares de chave-valor, permitindo acesso rápido aos valores com base nas chaves.

chave → valor

Exemplo uma agenda:

"Ana" → "123456789"
"João" → "987654321"~

Não há necessidade de procurar pelo valor, há necessidade procurar pela chave.

A ideia principal é que dada a chave, obtemos o valor instantaneamente, o que é muito eficiente para este tipo de problema.

Internamente ele usa hashing, que transforma a chave em um índice.

Então o código para resolver o problema usando um hash map seria algo assim:

```python
def twoSum(nums, target):

    num_dict = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_dict:
            return (num_dict[complement], i)
        num_dict[num] = i
    return None
```

Uma junção da noção do hash map com a lógica de encontrar o complemento necessário para atingir o target.

Explicação do código:
1. Criamos um dicionário vazio `num_dict` para armazenar os números e seus índices.
2. Iteramos sobre a lista `nums` usando `enumerate` para obter tanto o índice `i` quanto o número `num`.
3. Calculamos o complemento necessário para atingir o `target` subtraindo o número atual do `target`.
4. Verificamos se o complemento já está presente no dicionário `num_dict`.
   - Se estiver, significa que encontramos os dois números que somam o `target`, e retornamos os índices correspondentes.   - Se não estiver, adicionamos o número atual e seu índice ao dicionário para futuras referências.
5. Se a iteração terminar sem encontrar uma solução, retornamos `None` (embora o problema garanta que sempre haverá uma solução, então isso é mais por segurança).

### 🪝 Dúvida: "E `num_dict[num] = i`?" (não existe posição `num` antes)

- Em um dicionário, `num` é a **chave**, não posição indexada como lista.
- Quando executamos `num_dict[num] = i`, criamos/atualizamos o par chave/valor:
  - chave = `num` (por exemplo, 7)
  - valor = `i` (por exemplo, 1)
- Depois disso, `num_dict[num]` retorna `i` diretamente, graças à tabela hash interna.

#### Exemplo passo a passo:
- `num_dict = {}`
- i=0, num=2 → `num_dict[2] = 0`  (agora `{2: 0}`)
- i=1, num=7 → `num_dict[7] = 1`  (agora `{2:0, 7:1}`)
- i=2, num=11 → `complement = -2` não está no dicionário
- i=3, num=9 → `complement = 0` não está no dicionário
- i=4, num=7 → `complement = 2` está no dicionário, então `num_dict[2]` é 0.

#### Por que não precisa iterar todo o dicionário?
- `if complement in num_dict` e `num_dict[complement]` usam hash lookups internos.
- Isso é `O(1)` (média), diferente de procurar por todas as chaves manualmente.

#### Nota importante:
- Se `num` aparece mais de uma vez, o valor em `num_dict[num]` é o último índice salvo, mas no algoritmo Two Sum isso não prejudica porque a checagem do complemento acontece antes de sobrescrever o índice atual nesse loop.