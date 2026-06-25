# Evolutionary Strategies

Jupyter Notebook explorando **Estratégias Evolutivas** para otimização contínua, implementadas e analisadas com Python.

## Conteúdo

O notebook cobre três variantes progressivas de Estratégias Evolutivas:

### 1. ES $(1+\lambda)$
A estratégia mais simples: um único pai gera $\lambda$ filhos via perturbação gaussiana. O melhor indivíduo sobrevive para a próxima geração.

### 2. ES $(\mu, \lambda)$ com aproximação de gradiente
Usa toda a população para estimar a direção do gradiente. O centroide é atualizado pela combinação ponderada dos vetores de deslocamento, escalados pelo fitness normalizado:

$$\nabla f \approx -\frac{A \cdot N}{\lambda}$$

### 3. Canonical ES com atualização por rank
Melhoria sobre o $(\mu, \lambda)$: seleciona apenas os $\mu$ melhores indivíduos e usa seus **ranks** em vez dos valores absolutos de fitness. Isso confere invariância a transformações monotônicas do espaço de busca.

## Funções de Teste

| Função | Descrição | Dificuldade |
|--------|-----------|-------------|
| **Himmelblau** | $f(x,y) = (x^2+y-11)^2 + (x+y^2-7)^2$ — 4 mínimos globais em $f=0$ | Moderada |
| **Rosenbrock** | $f(x) = \sum (B(x_{i+1}-x_i^2)^2 + (a-x_i)^2)$ — vale estreito e curvo | Alta |

## Exercícios

O notebook contém 5 exercícios respondidos:

1. **Impacto de $\lambda$** no $(1+\lambda)$ ES — análise da convergência para a função Himmelblau
2. **Comparação Himmelblau vs Rosenbrock** — dificuldade do espaço de busca
3. **Impacto de $\alpha$ (learning rate)** no $(\mu, \lambda)$ ES
4. **Desempenho na Rastrigin** — função multimodal
5. **Gradiente absoluto vs rank** na função Rosenbrock

## Tecnologias

- Python 3
- NumPy
- Matplotlib
- Jupyter Notebook

## Como executar

```bash
pip install numpy matplotlib jupyter
jupyter notebook strategies/evolutionary_strategies.ipynb
```

## Estrutura do repositório

```
evolutionary_strategies/
└── strategies/
    └── evolutionary_strategies.ipynb
```

## Referências

- Wilson, D. G. — [Evolutionary Algorithms Course](https://d9w.github.io/evolution/)
- Chrabaszcz, P., Loshchilov, I., & Hutter, F. (2018). *Back to basics: benchmarking canonical evolution strategies for playing Atari.* IJCAI.
- Hansen, N. et al. (2011). *Impacts of invariance in search.* Applied Soft Computing.
