# 📊 Simulador de Investimentos em Fundos Imobiliários (FIIs)

Este repositório contém uma ferramenta de simulação desenvolvida no Excel para projetar o crescimento de investimentos em Fundos Imobiliários ao longo dos anos. A planilha permite que o usuário responda rapidamente às principais perguntas de negócio sobre seu dinheiro e planeje uma divisão de carteira com base no seu perfil de investidor.

## 🎯 As 5 Perguntas de Negócio que a ferramenta responde
A interface principal da ferramenta (Planilha1) foi desenhada com cara de aplicativo para responder de forma clara:
1. **Quanto investir por mês?** (Célula F16)
2. **Por quantos anos?** (Célula F17)
3. **Qual a taxa de rendimento mensal?** (Célula F18)
4. **Quanto de patrimônio vai acumular?** (Célula F19)
5. **Quanto vai receber de dividendos por mês?** (Célula F20)

Além disso, a planilha projeta **Cenários de Longo Prazo** detalhando o patrimônio e os dividendos esperados em 2, 5, 10, 20 e 30 anos (células A24 a C28).

## ⚙️ Como funciona por baixo dos panos

Para tornar as fórmulas limpas e legíveis, utilizei recursos intermediários/avançados do Excel, deixando a manutenção muito mais simples:

### 1. O uso da função VF (Valor Futuro)
Para calcular o **Patrimônio Acumulado**, utilizei a função matemática-financeira `=VF(taxa_mensal; qtd_anos*12; aporte*-1)`. 
Ela recolhe a taxa de rendimento, o total de períodos em meses (anos x 12) e projeta o crescimento do aporte contínuo considerando os juros compostos.

### 2. O uso do PROCV com Chave Composta
Para dividir automaticamente o aporte conforme o tipo de fundo (Papel, Tijolo, etc.) e o Perfil selecionado (Conservador, Moderado, Agressivo), foi criada uma aba de apoio (`Planilha2`). 
* Utilizei uma **Chave Composta** unindo o `Perfil & "-" & Tipo de FII` (ex: `Agressivo-PAPEL`) na coluna A.
* Na aba principal, a função `=PROCV()` usa o perfil selecionado (Célula B31) e o tipo de FII (A35:A40) como valor procurado, cruzando os dados e retornando perfeitamente a porcentagem alocada para aquele exato cenário, que então é multiplicada pelo Valor a ser Investido por Mês (`aporte`).

### 3. Intervalos Nomeados (Name Manager)
Em vez de depender de referências de células engessadas como `$F$16`, as variáveis do projeto foram transformadas em **Intervalos Nomeados**, facilitando a leitura e auditoria das fórmulas. Foram criados:
* `aporte` 
* `patrimonio` 
* `qtd_anos` 
* `rendimento`
* `salario` 
* `sugestao_investimento` 
* `taxa_mensal`

## 💼 Percentuais de cada Perfil (Divisão da Carteira)
Os percentuais foram construídos de forma escalonada na aba de apoio para somarem sempre 100%, variando o grau de risco conforme o perfil:

| Tipo de FII | Conservador | Moderado | Agressivo |
| :--- | :---: | :---: | :---: |
| **Papel** | 35% | 30% | 25% |
| **Tijolo** | 50% | 45% | 35% |
| **Híbridos** | 10% | 10% | 10% |
| **FOFs** | 5% | 5% | 5% |
| **Desenvolvimento** | 0% | 5% | 15% |
| **Hotelarias** | 0% | 5% | 10% |
*(Nota: Estes valores são fictícios e criados apenas para o estudo de desenvolvimento técnico, não constituindo recomendação de investimento).*

## 🚀 Evoluções em relação ao projeto original
Fiz melhorias para expandir a funcionalidade ensinada no desafio:
* **Automação de Valores Financeiros:** Além de puxar a porcentagem com o PROCV na divisão dos FIIs, a ferramenta já calcula imediatamente o **Valor em Reais** (`R$`) que deve ser destinado a cada categoria de fundo de acordo com o `aporte` mensal (Células C35 a C40).
* **Parâmetros Baseados em Sugestões Reais:** Há um campo simulando uma sugestão de investimento baseada em um teto de 30% do Salário base do usuário (Célula B18).
* **Base de dados isolada e limpa:** A matriz de risco (Chave Composta e Percentuais) foi isolada em uma segunda planilha (`Planilha2`), garantindo que o usuário só tenha contato direto com o "App" frontal.

---

> 💡 **Como testar:** Baixe o arquivo `.xlsx` do repositório, abra no Excel e experimente mudar o "Salário", os "Anos" ou trocar o "PERFIL" entre Conservador, Moderado e Agressivo (célula B31) para ver toda a distribuição do aporte mudando automaticamente sem quebrar nenhuma fórmula!
