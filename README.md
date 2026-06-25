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

## Exercícios e Respostas

### Exercício 1 — Impacto do parâmetro $\lambda$ no $(1+\lambda)$ ES

> Estude o impacto do parâmetro $\lambda$ modificando-o e re-executando a otimização. Qual o melhor valor de $\lambda$ para o problema Himmelblau?

O parâmetro $\lambda$ controla quantos filhos são gerados por geração. Com $\lambda$ pequeno (ex: 5), o algoritmo explora menos o espaço por iteração e converge mais devagar. Com $\lambda$ grande (ex: 50–100), explora mais por geração mas faz mais avaliações. Para a função Himmelblau, $\lambda=20$ já apresenta boa convergência em 100 gerações. Valores maiores como $\lambda=50$ convergem mais rápido nas primeiras gerações, mas o ganho marginal diminui. $\lambda=20$ representa um bom equilíbrio entre exploração e custo computacional para este problema.

---

### Exercício 2 — Comparação Himmelblau vs Rosenbrock

> Mude para o problema Rosenbrock. Qual a dificuldade desse espaço de busca comparado ao Himmelblau? Consegue encontrar um $\lambda$ adaptado?

A função Rosenbrock é significativamente mais difícil que a Himmelblau. Enquanto a Himmelblau possui 4 mínimos globais em $f=0$ com bacias de atração amplas e bem distribuídas, a Rosenbrock possui um vale estreito e curvo que leva ao mínimo global em $(1,1)$. O $(1+\lambda)$ ES tem dificuldade nesse vale porque mutações gaussianas isotrópicas têm baixa probabilidade de seguir a curvatura do vale. Para a Rosenbrock, $\lambda$ maior (ex: 50–100) ajuda pois aumenta a chance de amostrar na direção correta, mas a convergência ainda é muito mais lenta que na Himmelblau.

---

### Exercício 3 — Impacto do parâmetro $\alpha$ no $(\mu, \lambda)$ ES

> Estude o impacto do parâmetro $\alpha$ modificando-o e re-executando a otimização. Compare os melhores parâmetros encontrados com os resultados do $(1+\lambda)$ ES.

O parâmetro $\alpha$ (learning rate) controla o tamanho do passo na direção do gradiente aproximado. $\alpha$ muito pequeno (ex: 0.01) converge lentamente mas de forma estável. $\alpha$ muito grande (ex: 0.5+) causa instabilidade, podendo ultrapassar o mínimo. $\alpha=0.2$ (padrão) apresenta boa convergência para a Himmelblau. Comparado ao $(1+\lambda)$, o $(\mu,\lambda)$ converge mais suavemente por usar informação de toda a população no update, mas é mais sensível à escolha de $\alpha$ do que o $(1+\lambda)$ é à escolha de $\lambda$.

---

### Exercício 4 — Desempenho na função Rastrigin

> Estude o desempenho do $(\mu, \lambda)$ ES na função Rastrigin. Ele performa melhor ou pior que o $(1+\lambda)$ ES?

A função Rastrigin é multimodal com muitos mínimos locais, o que a torna mais difícil que a Himmelblau. O $(\mu,\lambda)$ ES tende a ter desempenho similar ou ligeiramente pior que o $(1+\lambda)$ na Rastrigin, pois o gradiente aproximado pode apontar para mínimos locais próximos. O $(1+\lambda)$ com $\lambda$ grande tem mais chance de escapar de mínimos locais por explorar mais aleatoriamente. O $(\mu,\lambda)$ é mais eficiente em espaços suaves, mas a Rastrigin penaliza sua tendência de seguir o gradiente local.

---

### Exercício 5 — Gradiente absoluto vs rank na função Rosenbrock

> Considere a função Rosenbrock. Como você espera que os rankings de gradiente se comparem à informação de gradiente absoluto? Serão muito similares ou diferentes?

Na Rosenbrock, espera-se que o gradiente por rank e o gradiente absoluto sejam bastante diferentes. O vale estreito e curvo cria grandes variações no fitness absoluto entre pontos próximos, tornando o gradiente absoluto ruidoso e potencialmente instável. O gradiente por rank, ao usar apenas a ordem relativa dos indivíduos, é mais robusto a essas variações extremas de fitness. No plot, os vetores do gradiente absoluto tendem a apontar de forma mais dispersa e com magnitudes irregulares, enquanto os vetores ponderados por rank são mais uniformes em magnitude e mais consistentes em direção.

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
