# Cálculo TF-IDF e Similaridade Cosseno

## Tabela de IDF (base 10)

| Termo        | % Docs | Cálculo IDF              | IDF (base 10) |
|---------------|--------|--------------------------|---------------:|
| computer      | 10%    | log10(10)               | 1.00000 |
| software      | 10%    | log10(10)               | 1.00000 |
| bugs          | 5%     | log10(20)               | 1.30103 |
| code          | 2%     | log10(50)               | 1.69897 |
| developer     | 2%     | log10(50)               | 1.69897 |
| programmers   | 2%     | log10(50)               | 1.69897 |

---

## Frequência de termos (TF)

| Documento | Termo | Ocorrências (tf) |
|------------|--------|-----------------:|
| D1 | programmers | 1 |
| D1 | write | 1 |
| D1 | computer | 1 |
| D1 | software | 1 |
| D1 | code | 1 |
| D2 | software | 3 |
| D2 | bugs | 2 |
| D3 | bugs | 1 |
| D3 | software | 1 |
| D3 | code | 1 |

---

## TF Normalizado (1 + log10(tf))

| Termo | tf | TF Normalizado |
|:--|--:|--:|
| 1 | 1 | 1.00000 |
| 2 | 2 | 1.30103 |
| 3 | 3 | 1.47712 |

---

## TF-IDF

| Documento | Termo | TF | IDF | TF Normalizado | TF-IDF |
|------------|--------|----:|----:|----:|----:|
| D1 | computer | 1 | 1.00000 | 1.00000 | 1.00000 |
| D1 | software | 1 | 1.00000 | 1.00000 | 1.00000 |
| D1 | code | 1 | 1.69897 | 1.00000 | 1.69897 |
| D1 | programmers | 1 | 1.69897 | 1.00000 | 1.69897 |
| D2 | software | 3 | 1.00000 | 1.47712 | 1.47712 |
| D2 | bugs | 2 | 1.30103 | 1.30103 | 1.69564 |
| D3 | bugs | 1 | 1.30103 | 1.00000 | 1.30103 |
| D3 | software | 1 | 1.00000 | 1.00000 | 1.00000 |
| D3 | code | 1 | 1.69897 | 1.00000 | 1.69897 |

---

## Vetores TF-IDF (por termo relevante)

| Termo        | D1 | D2 | D3 | Q (binário) |
|---------------|----:|----:|----:|----:|
| computer      | 1.00000 | 0.00000 | 0.00000 | 1 |
| software      | 1.00000 | 1.47712 | 1.00000 | 1 |
| programmers   | 1.69897 | 0.00000 | 0.00000 | 1 |

---

## Similaridade do Cosseno

| Documento | Produto Escalar (Q·D) | ‖Q‖ | ‖D‖ | Similaridade |
|------------|-----------------------:|----:|----:|----:|
| D1 | 3.69897 | 1.73205 | 2.33333 | **0.912** |
| D2 | 1.47712 | 1.73205 | 2.15567 | **0.397** |
| D3 | 1.00000 | 1.73205 | 2.20416 | **0.262** |

---

## Ranking de Similaridade

| Documento | Similaridade |
|------------|--------------:|
| D1 | **0.912** |
| D2 | 0.397 |
| D3 | 0.262 |