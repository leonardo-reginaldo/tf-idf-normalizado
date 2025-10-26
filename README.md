# 📚 Similaridade de Documentos usando TF-IDF e Similaridade Cosseno

Neste exemplo, vamos determinar a similaridade entre uma **consulta** e uma coleção de documentos usando o **método TF-IDF** e a **similaridade cosseno**.

A coleção de documentos contém a seguinte distribuição de termos:

| Termo        | % de documentos |
|--------------|:----------------:|
| computer     | 10%            |
| software     | 10%            |
| bugs         | 5%             |
| code         | 2%             |
| developer    | 2%             |
| programmers  | 2%             |

Vamos calcular a similaridade entre uma consulta: **Q = "computer software programmers".**

Considerando os documentos **D1** a **D3** descritos a seguir: 

| Documento | Descrição |
|-----------|-----------|
| D1        | programmers write computer software code |
| D2        | most software has bug, but good software has less bugs than bad software |
| D3        | some bugs can be found only by executing the software, not by examining the source code |


<p style="color:blue;"><b>Determine a similaridade pelo método do cosseno, usando os pesos TF-IDF para cada documento e pesos binários para a referida consulta.</b></p>


## 🧩 1. Vocabulário

| Termo        | % de documentos |
|--------------|:---------------:|
| computer      | 10%             |
| software      | 10%             |
| bugs          | 5%              |
| code          | 2%              |
| developer     | 2%              |
| programmers   | 2%              |


## 🎯 2. Conceito central: o que é IDF

**IDF (Inverse Document Frequency)** mede quão raro é um termo na coleção de documentos.  
A ideia é simples:

> quanto mais raro o termo, maior o seu poder de distinguir documentos.

A fórmula genérica é:

$$
\text{IDF}(t) = \log \left( \frac{N}{df_t} \right)
$$

onde:

- **N** = número total de documentos na coleção  
- **dfₜ** = número de documentos que contêm o termo *t*

## 📄 3. Contagem TF e normalização *(1 + log₁₀(tf))*

### 📊 Tabela de frequências (TF bruta)

| Termo        | D1 | D2 | D3 |
|---------------|----|----|----|
| computer      | 1  | 0  | 0  |
| software      | 1  | 3  | 1  |
| bugs          | 0  | 2  | 1  |
| code          | 1  | 0  | 1  |
| developer     | 0  | 0  | 0  |
| programmers   | 1  | 0  | 0  |

---

### ⚙️ 3.1 Normalização

Aplicamos a fórmula de normalização:

$$
TF_n = 1 + \log_{10}(tf)
$$

> Se \( TF = 0 \), então \( TF_n = 0 \).

---

### 📈 3.2 Tabela TF normalizado

| Termo        | D1     | D2     | D3     |
|---------------|---------|---------|---------|
| computer      | 1.00000 | 0.00000 | 0.00000 |
| software      | 1.00000 | 1.47712 | 1.00000 |
| bugs          | 0.00000 | 1.30103 | 1.00000 |
| code          | 1.00000 | 0.00000 | 1.00000 |
| developer     | 0.00000 | 0.00000 | 0.00000 |
| programmers   | 1.00000 | 0.00000 | 0.00000 |


## ⚖️ 4. Cálculo TF–IDF

O TF–IDF é obtido multiplicando o **TF normalizado** pelo **IDF**:

$$
\text{TF-IDF} = TF_n \times IDF
$$

---

### 📊 4.1 Tabela TF–IDF

| Termo        | D1       | D2        | D3       |
|---------------|:----------:|:-----------:|:----------:|
| computer      | 1.00000  | 0.00000   | 0.00000  |
| software      | 1.00000  | 1.47712   | 1.00000  |
| bugs          | 0.00000  | 1.30103×1.30103 = 1.69268 | 1.30103  |
| code          | 1.69897  | 0.00000   | 1.69897  |
| developer     | 0.00000  | 0.00000   | 0.00000  |
| programmers   | 1.69897  | 0.00000   | 0.00000  |

---

### 📌 Resumo por documento

- **D1:** `[1.00000, 1.00000, 0.00000, 1.69897, 0.00000, 1.69897]`  
- **D2:** `[0.00000, 1.47712, 1.69268, 0.00000, 0.00000, 0.00000]`  
- **D3:** `[0.00000, 1.00000, 1.30103, 1.69897, 0.00000, 0.00000]`

## 🔍 5. Vetor da Consulta (binário)

Consideremos a consulta:

**Q = [computer, software, programmers]**

O vetor binário correspondente é:

$$
\mathbf{q} = [1, 1, 0, 0, 0, 1]
$$

O comprimento (norma) do vetor é:

$$
\|\mathbf{q}\| = \sqrt{1^2 + 1^2 + 0^2 + 0^2 + 0^2 + 1^2} = \sqrt{3} \approx 1.73205
$$


## 📊 6. Produto escalar (q ⋅ D)

Somamos apenas as posições onde **q = 1** (computer, software, programmers):

| Documento | Produto escalar (q ⋅ D) |
|-----------|:-------------------------:|
| D1        | 1.00000 + 1.00000 + 1.69897 = 3.69897 |
| D2        | 0 + 1.47712 + 0 = 1.47712             |
| D3        | 0 + 1.00000 + 0 = 1.00000             |

## 📏 7. Norma de cada documento

A norma de cada documento é calculada como:

$$
\|\mathbf{D}\| = \sqrt{\sum_i (\text{TF-IDF}_i)^2}
$$

---

### 📊 7.1 Cálculo das normas

| Documento | Cálculo                                    | Norma    |
|-----------|:-------------------------------------------:|:----------:|
| D1        | √(1² + 1² + 1.69897² + 1.69897²) = √(7.7785) | 2.78899 |
| D2        | √(1.47712² + 1.69268²) = √(5.0469)           | 2.24709 |
| D3        | √(1² + 1.30103² + 1.69897²) = √(5.8088)     | 2.41099 |


## 🧠 8. Similaridade Cosseno

A similaridade cosseno entre a consulta **Q** e cada documento **D** é dada por:

$$
\text{Sim}(Q, D) = \frac{\mathbf{q} \cdot \mathbf{D}}{\|\mathbf{q}\| \, \|\mathbf{D}\|}
$$

onde:  
- $\mathbf{q} \cdot \mathbf{D}$ é o **produto escalar**  
- $\|\mathbf{q}\|$ é a **norma do vetor de consulta**  
- $\|\mathbf{D}\|$ é a **norma do documento**

---

### 📊 8.1 Resultados

| Documento | Produto escalar | \|\|D\|\| | Similaridade |
|:---------|----------------:|-----------:|-------------:|
| D1       | 3.69897         | 2.78899    | 3.69897 / (1.73205 × 2.78899) = 0.77440 |
| D2       | 1.47712         | 2.24709    | 1.47712 / (1.73205 × 2.24709) = 0.37911 |
| D3       | 1.00000         | 2.41099    | 1.00000 / (1.73205 × 2.41099) = 0.23927 |

## 🏁 9. Resultado Final

| Documento | Similaridade | Ordem |
|-----------|-------------|----------|
| D1        | 0.77440     | 1° 🥇    |
| D2        | 0.37911     | 2° 🥈    |
| D3        | 0.23927     | 3° 🥉    |

## 📝 10. Conclusão

**D1** é claramente o mais relevante porque contém **computer**, **software** e **programmers** — todos os termos da consulta — com **pesos TF–IDF altos**.  

**D2** tem muito **software** (alto TF), mas não contém **computer** nem **programmers**, o que reduz sua relevância para a consulta.  

**D3** contém **software**, mas não **computer** nem **programmers**, e ainda possui **code/bugs**, que não contribuem para a consulta, pois esta considera apenas os três termos selecionados.
