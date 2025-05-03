
# 🚚 Cálculo de Frete Médio por SKU com Média Ponderada e Percentis

## 🎯 Objetivo

Estimar o **frete médio realista por SKU** para e-commerce em marketplaces, considerando:

- Distribuição geográfica real das vendas
- Variações de frete por região
- Peso real, volume e cubagem do produto
- Aplicação de **percentis** para cenários conservadores (ex: P75, P90)

Esse modelo ajuda a tomar decisões mais seguras na **precificação**, **análise de margem** e **logística**.

---

## 📊 Etapas do Processo

### 1. Coleta de Dados de Vendas

Coletar os últimos X meses de vendas com:

- `SKU`
- `Preço do produto`
- `UF` e `CEP de destino`

---

### 2. Agrupamento de Regiões por Estado

Dividir cada estado em 5 regiões (ex: norte, sul, leste, oeste e capital).  
Selecionar **1 CEP representativo por região**.

---

### 3. Simulação de Frete

Para cada SKU:

- Usar **peso real**, **volume** e **cubagem**
- Simular o valor de frete até os 5 CEPs selecionados
- Registrar o valor de frete por SKU e por CEP

---

### 4. Cálculo da Média Ponderada por Estado

Atribuir pesos às regiões com base na **quantidade de vendas reais** para cada uma.

#### Fórmula:

```math
\text{Frete médio por estado (SKU)} = \sum_{i=1}^{n}(\text{frete}_i \times \text{peso}_i)
```

---

### 5. Aplicação de Percentil (P50, P75, P90...)

Ao invés da média simples, usar **percentis** para lidar com variações extremas.

#### Passo a passo:

1. Ordenar os valores de frete simulados  
   Exemplo: `[18, 20, 21, 25, 30]`

2. Calcular a posição do percentil:

```math
\text{posição} = (n - 1) \times \left(\frac{\text{percentil}}{100}\right)
```

Exemplo para **P75** com 5 valores:

```math
\text{posição} = (5 - 1) \times 0{,}75 = 3{,}0
\Rightarrow P75 = \text{valor na posição 4 (índice 3)} = 25
```

Se a posição for decimal, aplicar **interpolação linear**:

> Exemplo para posição 3.75:
> 
> valor inferior = valor[3] = 21  
> valor superior = valor[4] = 25  
> parte decimal = 0.75

```math
P75 = 21 + 0{,}75 \times (25 - 21) = 24
```

---

### 6. Cálculo da Média Nacional

Após obter os valores por estado:

- Calcular a **média ponderada nacional**, ponderada pelo volume de vendas por estado
- (Opcional) Aplicar um segundo percentil para segurança adicional
- 
**Fórmula para Frete Médio Nacional por SKU:**

Frete_Médio_Nacional = ∑ (Frete_j × Peso_j)

Onde:
- Frete_j = valor médio de frete no estado j
- Peso_j = proporção de vendas no estado j


---

## 🧠 Por que usar Percentil?

| Percentil | Interpretação                          |
|-----------|----------------------------------------|
| **P50**   | Mediana — valor típico                 |
| **P75**   | Frete acima da média (mais seguro)     |
| **P90**   | Cenário conservador (frete elevado)    |

---

## 🛠 Possível Automação

Esse modelo pode evoluir para um sistema que:

- Integra dados de vendas automaticamente
- Usa APIs de transportadoras para simular frete
- Aplica os cálculos dinamicamente
- Gera relatórios por SKU com cenários de P50, P75, P90

---

## 📎 Exemplo Visual (Resumido)

| SKU     | Estado | Média Ponderada | P75 Estado | Ponderado Nacional |
|---------|--------|------------------|------------|--------------------|
| SKU123  | SP     | R$ 18,90         | R$ 20,50   | R$ 21,70           |
| SKU123  | MG     | R$ 21,00         | R$ 23,00   |                    |

---

📌 *Este README é um guia de referência técnica e estratégica. Ideal para apresentar ideias, estruturar sistemas e justificar decisões com base em dados logísticos reais.*
