# Ponderada das Cadeias de Markov

[![Vídeo da Ponderada](https://img.youtube.com/vi/-wHYIdgCWdY/maxresdefault.jpg)](https://www.youtube.com/watch?v=-wHYIdgCWdY)
> Link do vídeo: https://www.youtube.com/watch?v=-wHYIdgCWdY
> Google Colab: https://colab.research.google.com/drive/1ig4IC78GNEFWUCeudntlgAtJ5notuvhR?usp=sharing

Neste README.md apresento o resumo do que foi feito no Notebook [ponderada-markov.ipynb](ponderada-markov.ipynb) , que simula o fluxo atual do operacional do Metrô-SP utilizando **Cadeias de Markov** e o fluxo proposto com a integração de **Inteligência Artificial** e **versionamento de documentos** (Proposta do Grupo 02 - Brigadeiro/SubWay Surfers:

* o cenário **AS-IS** (situação atual, com limitações e retrabalho);
* o cenário **TO-BE** (proposta com integração de IA e versionamento).

A simulação foi feita conforme o tutorial **“Markov Chains in Python: Beginner Tutorial” (DataCamp)**, utilizando o mesmo formato de matriz 3×3 e a função `np.random.choice` para amostrar os próximos estados.

---

##  Contexto do problema

O Metrô-SP utiliza hoje ferramentas como **Miro**, **SharePoint**, **Teams** e planilhas do **Office** para gerenciar projetos e relatórios de portfólio.
Esse modelo apresenta **ineficiências operacionais**: conflitos de edição, falta de rastreabilidade, revisão manual e alto índice de retrabalho.

O novo sistema proposto no TAP (Termo de Abertura do Projeto) tem como meta:

* **Substituir o SharePoint** como fonte primária de documentos;
* Implementar um **comparador automático** e uma **pipeline de integração com GitHub**;
* Incorporar um **chat com RAG (Retrieval-Augmented Generation)** para consulta de dados;
* Melhorar os **requisitos não funcionais** de segurança, disponibilidade e confiabilidade.

---

##  Modelagem da Cadeia de Markov

### Estados considerados

Com base nos fluxos reais de trabalho e nos documentos do parceiro, defini **três estados principais**, alinhados às etapas observadas no ciclo documental:

| Estado       | Descrição no AS-IS                                                                                 | Descrição no TO-BE                                                                  |
| ------------ | -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Editar**   | Preenchimento manual em planilhas e documentos no Miro e SharePoint, com risco de erro e conflito. | Edição assistida por IA com campos estruturados e versionamento controlado (Git).   |
| **Revisar**  | Validação manual feita por múltiplas áreas, comparando arquivos e versões.                         | Revisão semiautomática com checks automáticos, comparador de versões e dashboard.   |
| **Publicar** | Upload manual no SharePoint, com risco de sobrescrita e republicações.                             | Publicação auditável, protegida e rastreável, com geração automática de relatórios. |

Esses três estados capturam o **funil essencial** do processo — da criação até a disponibilização final — e representam de forma simplificada o fluxo de informação do portfólio.

---

## 🧮 Matriz de transição (AS-IS x TO-BE)

As matrizes refletem **probabilidades de transição observadas ou esperadas** entre os estados, considerando padrões reais do cliente e melhorias esperadas com o novo sistema.

### AS-IS (atual)

```python
P_as_is = [
 [0.55, 0.40, 0.05],  # Editar → Editar/Revisar/Publicar
 [0.20, 0.65, 0.15],  # Revisar → Editar/Revisar/Publicar
 [0.00, 0.15, 0.85]   # Publicar → Revisar/Publicar
]
```

➡️ Representa um fluxo **lento e repetitivo**, com **muitos loops** em `Revisar` e **republicações frequentes**.

### TO-BE (proposto)

```python
P_to_be = [
 [0.35, 0.60, 0.05],
 [0.10, 0.30, 0.60],
 [0.00, 0.03, 0.97]
]
```

Representa o fluxo **automatizado**, com **maior chance de revisão produtiva** e **quase nenhum retrabalho pós-publicação**.

A probabilidade `Publicar → Publicar = 0.97` simboliza a **estabilidade do sistema proposto**, onde 97% dos documentos permanecem válidos e não exigem correção, graças ao versionamento, checks automáticos e auditoria.

---

## Passo a passo que segui (com base no Tutorial do DataCamp)

1. Defini cada **matriz de transição** 3×3.
2. `np.random.choice(p=linha_do_estado)` para sortear o **próximo estado**.
3. Repeti milhares de vezes (`runs=5000`) o processo com **8 passos** por simulação.
4. Conagem das frequências de estados finais para estimar a probabilidade de terminar em `Publicar`.

Essa metodologia é exatamente a mesma apresentada no tutorial, adaptada ao contexto do Metrô-SP.

---

## Resultado das simulações

| Métrica                                 | AS-IS  | TO-BE  | Diferença                  |
| --------------------------------------- | ------ | ------ | -------------------------- |
| Probabilidade de terminar em `Publicar` | ≈ 0.68 | ≈ 0.90 | **+22 pontos percentuais** |

Com base nessas simulações,  o sistema proposto **aumenta a taxa de publicação bem-sucedida**, reduzindo o retrabalho e o tempo de ciclo de aprovação.

---

## 💬 Conclusões pessoais

Na minha visão, este exercício mostra claramente **como a modelagem probabilística pode apoiar decisões de arquitetura**.
Mesmo com um modelo simples (3 estados, 3×3), foi possível **quantificar o ganho qualitativo** da automação e da governança de dados.

O novo sistema:

* Elimina pontos críticos de retrabalho e conflito;
* Melhora a previsibilidade do fluxo de publicações;
* Reforça os requisitos não funcionais de **segurança**, **rastreabilidade** e **eficiência operacional**.