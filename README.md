# Instrumentação Industrial II
## Aula Prática 06: Medição de Nível em Tanque Pressurizado com Transmissor LD301

Esta prática apresenta a base teórica e a execução, no simulador **LasecSimul**, de uma medição de nível por **pressão diferencial** em um **tanque fechado e pressurizado**. A medição usa o transmissor **Smar LD301 (HART)**, configurado pelo **PACTware**.

O material foi organizado para ligar a hidrostática da instalação ao que o transmissor mede, com foco em:

- medição de nível por pressão hidrostática;
- diferença entre tanque aberto e tanque fechado (pressurizado);
- perna seca, perna molhada, selos e capilares;
- supressão e elevação de zero e o cálculo de LRV e URV;
- efeito das densidades do processo, do selo e do capilar;
- configuração do LD301 pelo PACTware para **indicar o nível em metros**.

---

## Índice

1. [Introdução](#1-introdução)
2. [Princípio da medição de nível por pressão](#2-princípio-da-medição-de-nível-por-pressão)
3. [Tanque fechado: perna seca e perna molhada](#3-tanque-fechado-perna-seca-e-perna-molhada)
4. [A instalação simulada](#4-a-instalação-simulada)
5. [Dinâmica do tanque](#5-dinâmica-do-tanque)
6. [A planta no LasecSimul](#6-a-planta-no-lasecsimul)
7. [O transmissor LD301 na medição de nível](#7-o-transmissor-ld301-na-medição-de-nível)
8. [Arquivos da prática](#8-arquivos-da-prática)
9. [Como executar](#9-como-executar)
10. [Conexão com o PACTware](#10-conexão-com-o-pactware)
11. [Configuração do LD301 para nível](#11-configuração-do-ld301-para-nível)
12. [Sugestão de atividade experimental](#12-sugestão-de-atividade-experimental)
13. [Conclusão](#13-conclusão)
14. [Referências](#14-referências)
15. [Apêndice: lista de imagens](#15-apêndice-lista-de-imagens)

---

## 1. Introdução

A medição de nível por **pressão diferencial** é uma das técnicas mais usadas na indústria. Ela aproveita o fato de que a pressão no fundo de um líquido depende da altura da coluna e da densidade do líquido. Um transmissor de pressão diferencial, como o LD301, mede essa pressão e a converte em sinal de 4–20 mA.

Esse tipo de medição é amplamente utilizado em:

- tanques de armazenamento e de processo;
- vasos pressurizados e caldeiras (tubulão);
- colunas de destilação e reatores;
- tanques com produtos agressivos, com o uso de selos remotos.

A técnica se destaca por apresentar:

- instrumento padronizado, sem partes móveis dentro do tanque;
- boa exatidão quando a densidade do líquido é conhecida;
- aplicação em tanques abertos e fechados, com ajuste de zero e de faixa.

Como cuidado principal, a medição depende da **densidade** do líquido e dos fluidos nas tubulações de impulso. Se ela muda, o nível indicado também muda.

**Figura 1. Transmissor de pressão diferencial em tanque fechado**

![Figura 1](assets/fig01_transmissor_tanque.jpg)

> **Imagem a inserir:** foto de um transmissor de pressão diferencial (de preferência um LD301) instalado em um tanque ou vaso fechado, com as duas tomadas de pressão visíveis: a de baixo, perto do fundo, e a de cima, no topo do tanque, ligada por tubulação ou capilar com selo remoto.

---

## 2. Princípio da medição de nível por pressão

A pressão exercida por uma coluna de líquido de altura `h` é:

$$
P = \rho\,g\,h
$$

Onde:

- `ρ` é a massa específica do líquido (kg/m³);
- `g` é a aceleração da gravidade (9,80665 m/s²);
- `h` é a altura da coluna de líquido acima da tomada (m).

Na instrumentação é comum usar a **densidade relativa** `SG = ρ/ρ_{água}` e expressar a pressão em **milímetros de coluna d'água (mmH2O)**. Assim, a pressão de uma coluna fica simplesmente:

$$
P\,[\text{mmH2O}] = 1000 \cdot SG \cdot h\,[\text{m}]
$$

Por exemplo, 3 m de água (`SG = 1`) exercem 3000 mmH2O, e 3 m de um óleo com `SG = 0,8` exercem 2400 mmH2O.

Em um **tanque aberto**, a tomada de alta (H) fica no fundo e a de baixa (L) fica aberta para a atmosfera. O transmissor mede diretamente `ΔP = 1000·SG·h`.

**Figura 2. Tanque aberto e tanque fechado**

![Figura 2](assets/fig02_aberto_fechado.png)

> **Imagem a inserir:** dois esquemas lado a lado. À esquerda, um tanque aberto com o transmissor de pressão diferencial no nível do fundo: tomada H ligada ao fundo e tomada L aberta à atmosfera. À direita, um tanque fechado com gás pressurizado acima do líquido: tomada H no fundo e tomada L ligada ao topo do tanque. Indique `h`, a pressão do gás `P_gás` e as setas de pressão em cada tomada.

---

## 3. Tanque fechado: perna seca e perna molhada

Em um tanque **fechado e pressurizado**, a pressão do gás `P_gás` age sobre o líquido e se soma à pressão no fundo. Se a tomada de baixa ficasse aberta, o transmissor mediria `1000·SG·h + P_gás` e o nível ficaria errado. Por isso a tomada de baixa vai ao **topo do tanque**: a pressão do gás aparece nas duas tomadas e se cancela no `ΔP`.

A tubulação que liga o topo ao lado L pode ser:

- **perna seca:** cheia de gás. Só serve quando o gás não condensa;
- **perna molhada:** cheia de um líquido de selagem, com densidade `SG_{selo}`. É usada quando o vapor do tanque condensaria na tubulação e mudaria a medição. A coluna fica sempre cheia e constante.

Com a perna molhada, a coluna do selo pesa sobre o lado L **mesmo com o tanque vazio**. O `ΔP` fica negativo na maior parte da faixa. Esse deslocamento é chamado de **elevação de zero**. Quando o transmissor está abaixo da tomada de baixo, a coluna extra do lado H soma uma **supressão de zero**.

**Figura 3. Perna seca e perna molhada**

![Figura 3](assets/fig03_perna_seca_molhada.png)

> **Imagem a inserir:** dois esquemas de tanque fechado lado a lado. À esquerda, a perna seca: tubulação do topo até o lado L vazia, com um pote de dreno na parte de baixo. À direita, a perna molhada: tubulação do topo até o lado L cheia de selo, com um pote de selagem na saída do topo do tanque. Destaque com cores diferentes o líquido do processo e o líquido de selagem.

---

## 4. A instalação simulada

O LD301 está **abaixo** do tanque. A tomada de baixo está no nível mínimo (nível zero), e o transmissor fica `L_{cap}` metros abaixo dela.

```text
          gás (Pgás)
   ┌────────────┐ ── tomada de cima ──[selo]── perna molhada (SGselo) ──┐
   │            │                                                        │ H + Lcap
   │  líquido   │ H                                                      │
   │  (SGp)     │                                                        │
   └────────────┘ ── tomada de baixo ──[selo]── capilar (SGcap) ──┐ Lcap │
                                                                  HIGH  LOW  → LD301
```

- **HIGH (alta):** a coluna do líquido acima da tomada de baixo, mais o capilar de comprimento `L_{cap}` até o transmissor, cheio de um fluido de densidade `SG_{cap}`, mais a pressão do gás.
- **LOW (baixa):** a perna molhada do topo até o transmissor, de altura `H + L_{cap}`, sempre cheia de selo de densidade `SG_{selo}`, mais a pressão do gás.

Em mmH2O, com `P_gás` em kPa:

$$
HIGH = 1000\,(SG_p\,h + SG_{cap}\,L_{cap}) + 101{,}97\,P_{gás}
$$

$$
LOW = 1000\,SG_{selo}\,(H + L_{cap}) + 101{,}97\,P_{gás}
$$

E o transmissor mede:

$$
\Delta P = HIGH - LOW = 1000\left[SG_p\,h + SG_{cap}\,L_{cap} - SG_{selo}\,(H + L_{cap})\right]
$$

A pressão do gás **se cancela**. O `ΔP` só depende do nível e das densidades.

### 4.1 Faixa do transmissor (LRV e URV)

Com o tanque vazio (`h = 0`) e cheio (`h = H`):

$$
LRV = 1000\left[SG_{cap}\,L_{cap} - SG_{selo}\,(H + L_{cap})\right]
$$

$$
URV = LRV + 1000\,SG_p\,H
$$

Com os valores padrão (`H = 3` m, todas as densidades iguais a 1, `L_{cap} = 0,2` m):

- tanque vazio: `ΔP = 200 − 3200 = −3000` mmH2O (**LRV**, 4 mA);
- tanque cheio: `ΔP = 3200 − 3200 = 0` mmH2O (**URV**, 20 mA).

**Figura 4. Alturas e pressões na instalação simulada**

![Figura 4](assets/fig04_instalacao_cotas.png)

> **Imagem a inserir:** esquema do tanque fechado com o transmissor abaixo dele, cotado: altura `H` entre as tomadas, nível `h`, capilar `L_{cap}` da tomada de baixo até o transmissor e perna molhada de altura `H + L_{cap}`. Escreva ao lado de cada perna a pressão que ela exerce (`1000·SG_{selo}·(H+L_{cap})` e `1000·(SG_p·h + SG_{cap}·L_{cap})`) e indique que `P_gás` atua nas duas.

---

## 5. Dinâmica do tanque

O nível varia com o balanço entre a vazão de entrada e a de saída:

$$
\frac{dh}{dt} = \frac{Q_{in} - Q_{out}}{A}
\qquad
A = \frac{\pi D^2}{4}
$$

- **Vazão de entrada:** `Q_{in} = Q_{in,max}·u_{in}/100`, zerada quando o tanque enche.
- **Vazão de saída:** depende da coluna de líquido e da pressão do gás, que empurra o líquido para fora:

$$
Q_{out} = Q_{out,max}\cdot\frac{u_{out}}{100}\cdot\sqrt{\frac{SG_p\,h + 0{,}102\,P_{gás}}{SG_p\,H + 0{,}102\,P_{gás}}}
$$

Com os valores padrão (`D = 1` m, `Q_{in,max} = 36` m³/h) e só a válvula de entrada aberta, o nível sobe:

$$
\frac{dh}{dt} = \frac{36/3600}{0{,}785} = 12{,}7\ \text{mm/s}
$$

ou seja, o tanque vai de vazio a cheio em cerca de 4 minutos.

---

## 6. A planta no LasecSimul

O projeto [`tanque_pressurizado_ld301.lsproj`](tanque_pressurizado_ld301.lsproj) reproduz o tanque fechado com as duas válvulas e o LD301 ligado às pernas HIGH e LOW. A planta vem **incorporada ao projeto**: não é preciso instalar nada além do LasecSimul (versão **0.0.83 ou mais nova**).

**Figura 5. Projeto completo no LasecSimul**

![Figura 5](assets/fig05_projeto_completo.png)

> **Imagem a inserir:** captura de tela do LasecSimul com o projeto `tanque_pressurizado_ld301.lsproj` aberto e a simulação **rodando** (selo verde "Running"). Devem aparecer: os sliders "Válvula de entrada (%)" e "Válvula de saída (%)" à esquerda, a planta do tanque no centro, o LD301 à direita com a fonte de 24 V, o resistor de 250 Ω, o amperímetro, o modem HART e o osciloscópio. Use o botão de "ajustar tudo à tela".

### 6.1 Parâmetros (Propriedades exportadas)

Selecione a planta e abra o painel **Properties** → **Propriedades exportadas**. Os valores mudam com a simulação rodando:

| Parâmetro | Padrão |
|---|---|
| Altura do tanque (tomada de baixo até a de cima) `H` | 3 m |
| Diâmetro do tanque `D` | 1 m |
| Densidade relativa do líquido de processo `SG_p` | 1,0 |
| Densidade relativa do selo da perna molhada (LOW) `SG_{selo}` | 1,0 |
| Densidade relativa do fluido do capilar de alta (HIGH) `SG_{cap}` | 1,0 |
| Capilar de alta abaixo do nível mínimo `L_{cap}` | 0,2 m |
| Pressão do gás no topo (manométrica) `P_gás` | 50 kPa |
| Vazão máxima de entrada (válvula 100 %) | 36 m³/h |
| Vazão máxima de saída (válvula 100 %, tanque cheio) | 36 m³/h |

O nível começa em 1,5 m (50 %).

**Saídas da planta:** HIGH e LOW (mmH2O), ligadas ao LD301, e Nível (m) e Nível (%), que podem ser usadas em um controlador.

**Figura 6. Detalhe da planta do tanque pressurizado**

![Figura 6](assets/fig06_planta.png)

> **Imagem a inserir:** captura ampliada só da planta, com a simulação rodando: o tanque com o líquido azul na altura do nível, a válvula de entrada no topo e a de saída no fundo (com o selo de abertura em %), o "gás pressurizado" no topo, a perna molhada amarela (LOW) e o capilar laranja (HIGH) descendo até o nível do transmissor, as cotas `H` e `Lcap`, os displays brancos de medição (Nível, ΔP, vazões, tomadas) e os displays amarelos de parâmetros.

**Figura 7. Propriedades exportadas da planta**

![Figura 7](assets/fig07_propriedades_exportadas.png)

> **Imagem a inserir:** captura do painel **Properties** com a planta selecionada, rolado até "Propriedades exportadas", mostrando os campos de altura, diâmetro, densidades, capilar, pressão do gás e vazões máximas.

### 6.2 Valores esperados com os parâmetros padrão

| Nível h (m) | Nível (%) | HIGH (mmH2O) | LOW (mmH2O) | ΔP (mmH2O) | Corrente (mA) |
|---:|---:|---:|---:|---:|---:|
| 0,00 | 0 | 5299 | 8299 | −3000 | 4,00 |
| 0,75 | 25 | 6049 | 8299 | −2250 | 8,00 |
| 1,50 | 50 | 6799 | 8299 | −1500 | 12,00 |
| 2,25 | 75 | 7549 | 8299 | −750 | 16,00 |
| 3,00 | 100 | 8299 | 8299 | 0 | 20,00 |

HIGH e LOW incluem os 5099 mmH2O do gás (50 kPa), que se cancelam no `ΔP`.

---

## 7. O transmissor LD301 na medição de nível

O LD301 mede a diferença entre as pressões dos lados **H** e **L** e a converte em corrente de 4 a 20 mA. A comunicação **HART** é sobreposta à corrente e permite configurar o transmissor pelo PACTware.

### 7.1 Faixa de calibração com zero elevado

No LD301, os limites **inferior (LRV)** e **superior (URV)** da calibração são de **pressão**. Para esta instalação:

- LRV = −3000 mmH2O (tanque vazio, 4 mA);
- URV = 0 mmH2O (tanque cheio, 20 mA).

A corrente é linear com o `ΔP` e, portanto, com o nível:

$$
I = 4 + 16\cdot\frac{\Delta P - LRV}{URV - LRV}\quad[\text{mA}]
$$

Na medição de nível a função de transferência é **linear**. A raiz quadrada é para vazão.

### 7.2 Unidade do Usuário e display

A **Unidade do Usuário (User Unit)** dá unidade e escala ao % da faixa: 0 % corresponde a 0 m e 100 % a 3 m. O resultado é a **PV** do LD301, mostrada no display com o ícone **PV**. O manual do LD301 recomenda exatamente esse recurso para indicar nível em engenharia quando há supressão ou elevação de zero.

O display alterna até duas variáveis a cada 3 s (**First Var** e **Second Var**):

| Opção | Mostra |
|---|---|
| OUT | saída em mA |
| PV% | variável de processo em % (o nível em %) |
| PV | variável de processo na unidade do usuário (o nível em m) |
| PRES | pressão diferencial (negativa nesta instalação) |
| TEMP | temperatura do sensor |
| None | só na 2ª variável: não alterna |

O LD301 do projeto vem com tag **LT100**, faixa −3000…0 mmH2O e display em PV% / TEMP.

**Figura 8. Malha 4–20 mA do LD301 no simulador**

![Figura 8](assets/fig08_malha_4_20.png)

> **Imagem a inserir:** captura ampliada da região do LD301 no LasecSimul: o transmissor com o display, a fonte de 24 V, o resistor de 250 Ω, o amperímetro com a corrente (12,00 mA com o nível em 50 %) e o modem HART ligado sobre o resistor. Destaque com setas os fios HIGH (laranja) e LOW (amarelo) vindos da planta.

---

## 8. Arquivos da prática

Os arquivos desta pasta são:

- [`tanque_pressurizado_ld301.lsproj`](tanque_pressurizado_ld301.lsproj): projeto do LasecSimul com a planta incorporada e o LD301;
- [`assets/`](assets): imagens deste roteiro.

---

## 9. Como executar

1. Instale ou atualize a extensão **LasecSimul** no VS Code (versão 0.0.83 ou mais nova).
2. Abra esta pasta no VS Code e dê dois cliques em `tanque_pressurizado_ld301.lsproj`.
3. Clique em **Run** (▶). O selo no canto superior direito muda para "Running".
4. Use os sliders **"Válvula de entrada (%)"** e **"Válvula de saída (%)"** para encher e esvaziar o tanque.
5. Observe o líquido no tanque, os displays, a corrente no amperímetro e o display do LD301.
6. Para mudar os parâmetros, selecione a planta e use **Propriedades exportadas**.

Observações importantes:

- com as duas válvulas fechadas, o nível fica parado; use isso para anotar valores estáveis;
- ao mudar uma densidade, a altura ou o capilar, o **LRV/URV do LD301 precisa ser recalculado**;
- a pressão do gás muda a vazão de saída, mas não muda o `ΔP`.

---

## 10. Conexão com o PACTware

O **modem HART** do projeto está ligado sobre o resistor de 250 Ω e se comunica com o computador por uma porta serial virtual.

1. Selecione o **Modem HART** e confira em Properties: **Ligação com o PC** = Serial (COM / CNC) e **Porta serial** = `CNCB0`.
2. No **com0com**, crie (ou confira) o par de portas `CNCB0` ↔ `COM20`. O LasecSimul usa um lado; o PACTware usa o outro.
3. No PACTware, abra **Ver → Catálogo de instrumentos**, atualize o catálogo e adicione **HART Communication**.
4. Dê dois cliques no driver HART e selecione a porta **COM20**. Desmarque **Multimaster and Burst mode support**. Clique em **Apply** e **OK**.
5. Clique com o botão direito no driver → **Adicionar instrumento** → escolha o DTM do **LD301** (Smar).
6. Com a simulação rodando, clique com o botão direito no LD301 → **Conectar**. Os ícones ficam verdes e o tag **LT100** aparece.
7. Abra a **Parametrização online** do LD301.

**Figura 9. Par de portas virtuais no com0com**

![Figura 9](assets/fig09_com0com.png)

> **Imagem a inserir:** janela de configuração do com0com (*Setup for com0com*) mostrando o par com `CNCB0` de um lado e `COM20` do outro, com as opções padrão.

**Figura 10. LD301 conectado no PACTware**

![Figura 10](assets/fig10_pactware_conectado.png)

> **Imagem a inserir:** tela do PACTware com a árvore do projeto mostrando "HOST PC → HART Communication (COM20) → LD301" com os ícones verdes de conexão, e a janela do DTM aberta exibindo o tag LT100 e a variável primária negativa em mmH2O (por exemplo −1500 com o nível em 50 %).

---

## 11. Configuração do LD301 para nível

Faça uma alteração por vez, aplique (**Write/Apply**) e confira o efeito no simulador antes da próxima.

### 11.1 Calibração (faixa de ΔP com zero elevado)

Em **Calibration**: unidade **mmH2O**, **Lower = −3000**, **Upper = 0** → aplicar. O projeto já vem com essa faixa. Confira, e refaça sempre que mudar algum parâmetro da instalação.

**Figura 11. Tela de calibração do LD301**

![Figura 11](assets/fig11_dtm_calibration.png)

> **Imagem a inserir:** tela **Calibration** do DTM do LD301 com a unidade mmH2O, Lower Value −3000 e Upper Value 0 preenchidos.

### 11.2 Função de transferência

Em **Configuration → Function**: confira que está **Linear**.

### 11.3 Unidade do Usuário

Em **User Unit**: **On**, unidade **m**, **0 % = 0** e **100 % = 3** → aplicar.

**Figura 12. Unidade do Usuário em metros**

![Figura 12](assets/fig12_dtm_user_unit.png)

> **Imagem a inserir:** tela **User Unit** do DTM com a opção habilitada (On), a unidade **m** selecionada na lista de nível e os valores 0 (0 %) e 3 (100 %).

### 11.4 Display

Em **Display (LCD Indicator)**: **First Var = PV** e **Second Var = PV%** → aplicar.

**Figura 13. Configuração do display**

![Figura 13](assets/fig13_dtm_display.png)

> **Imagem a inserir:** tela de configuração do display do DTM com First Var = PV e Second Var = PV%.

**Figura 14. Display do LD301 indicando nível**

![Figura 14](assets/fig14_lcd_nivel.png)

> **Imagem a inserir:** duas capturas ampliadas do display do LD301 no simulador, com o nível em 1,5 m: uma com "1.500 m" e o ícone PV aceso, e outra, 3 s depois, com "50.00" e o ícone de %. Monte as duas lado a lado.

### 11.5 Conferência

| Nível h (m) | ΔP (mmH2O) | Corrente (mA) | LCD (PV) | LCD (PV%) |
|---:|---:|---:|---:|---:|
| 0,00 | −3000 | 4,00 | 0,000 m | 0,00 |
| 0,75 | −2250 | 8,00 | 0,750 m | 25,00 |
| 1,50 | −1500 | 12,00 | 1,500 m | 50,00 |
| 2,25 | −750 | 16,00 | 2,250 m | 75,00 |
| 3,00 | 0 | 20,00 | 3,000 m | 100,00 |

---

## 12. Sugestão de atividade experimental

### Parte A: calibração e linearidade

1. Configure o LD301 como na seção 11.
2. Usando as válvulas, leve o nível a aproximadamente 0, 0,75, 1,5, 2,25 e 3 m. Feche as válvulas para estabilizar.
3. Registre o nível real (display da planta), o `ΔP` (PACTware), a corrente (amperímetro) e o nível indicado no LD301.

| Nível real (m) | ΔP (mmH2O) | Corrente (mA) | Nível indicado (m) | Erro (m) |
|---:|---:|---:|---:|---:|
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |

**Figura 15. Corrente em função do nível**

![Figura 15](assets/fig15_grafico_corrente_nivel.png)

> **Imagem a inserir:** gráfico com o nível (0 a 3 m) no eixo horizontal e a corrente (4 a 20 mA) no vertical, feito com os valores medidos na Parte A, mostrando a reta entre (0 m; 4 mA) e (3 m; 20 mA). Se fizer a Parte B, acrescente a reta deslocada com o selo de densidade 0,9.

### Parte B: densidade do selo

1. Com o nível em 1,5 m, mude a densidade do selo da perna molhada para **0,9**.
2. Registre o `ΔP`, a corrente e o nível indicado. Qual o erro?
3. Recalcule LRV e URV com as equações da seção 4.1, configure o LD301 e confira de novo.

Resultado esperado: `ΔP = −1180` mmH2O, 13,71 mA e indicação de 1,82 m (erro de +0,32 m). A nova faixa é **LRV = −2680** e **URV = 320** mmH2O.

### Parte C: pressão do gás

1. Volte o selo para 1,0. Com o nível parado, mude a pressão do gás de 50 para 200 kPa.
2. Registre HIGH, LOW e `ΔP`. O nível indicado mudou? Explique.
3. Abra a válvula de saída e compare o tempo de esvaziamento com 50 e com 200 kPa.

### Parte D: comprimento e fluido do capilar

1. Mude o capilar de 0,2 para 0,5 m com todas as densidades iguais a 1. O LRV muda?
2. Mude agora a densidade do fluido do capilar para 1,2. Recalcule o LRV (resultado esperado: −2900 mmH2O).

### Parte E: densidade do processo

1. Com o tanque cheio (3 m) e a faixa original, mude a densidade do líquido de processo para **0,8**.
2. Registre a indicação do LD301. Resultado esperado: `ΔP = −600` mmH2O, 80 % e 2,4 m com o tanque cheio.
3. Qual URV corrige a medição? (resultado esperado: −600 mmH2O)

### Parte F: dinâmica

1. Com o nível em 1,5 m, abra só a válvula de entrada em 100 % e meça o tempo até o tanque encher.
2. Compare com o tempo calculado pela seção 5 (cerca de 118 s para 1,5 m).

### Questões para o relatório

1. Por que a tomada de baixa de um tanque pressurizado vai ao topo, e não à atmosfera?
2. Por que o `ΔP` é negativo nesta instalação? O que é elevação de zero?
3. Quais parâmetros da instalação exigem recalibrar o LD301 quando mudam? Quais não exigem?
4. Por que o LD301 de nível usa função linear, e o de vazão por placa de orifício usa raiz quadrada?

Essa atividade conecta a hidrostática da instalação, as escolhas de projeto das pernas de impulso e a configuração real de um transmissor HART.

---

## 13. Conclusão

A prática mostra, de forma aplicada, a cadeia completa de uma medição de nível em tanque pressurizado:

- o nível do líquido gera uma pressão hidrostática proporcional à altura e à densidade;
- a tomada de baixa no topo cancela a pressão do gás;
- a perna molhada e o capilar deslocam o zero da medição;
- o LD301 mede o `ΔP` com a faixa de zero elevado (−3000 a 0 mmH2O);
- a Unidade do Usuário e o display apresentam o nível em metros;
- qualquer mudança de densidade exige recalcular a faixa do transmissor.

Com isso, o roteiro serve tanto como material de estudo quanto como apoio para a aula prática de instrumentação.

---

## 14. Referências

- Apostila de Instrumentação Industrial II - UFU
- Smar. *LD301 — Manual de Instruções, Operação e Manutenção* (seção 3: calibração com supressão e elevação de zero, User Unit e LCD Indicator).
- Bega, E. A. (org.). *Instrumentação Industrial.* Editora Interciência.
- Lipták, B. G. *Instrument Engineers' Handbook, Volume 1: Process Measurement and Analysis.* CRC Press (capítulo de medição de nível por pressão diferencial).
- [LasecSimul — LASEC/UFU](https://github.com/LASEC-UFU/LasecSimul)

---

## 15. Apêndice: lista de imagens

Todas as imagens ficam na pasta [`assets/`](assets), com os nomes exatos da tabela. Cada figura do texto já traz, no quadro "Imagem a inserir", o que ela precisa mostrar. Este apêndice reúne como produzir cada uma.

### 15.1 Como produzir as imagens

- **Capturas do LasecSimul:** VS Code maximizado (1920 × 1080), barra lateral fechada (`Ctrl+B`) e escala do Windows em 100 %. Rode a simulação e espere os valores estabilizarem antes de capturar. Use **Ajustar tudo** ou **Ajustar seleção** na barra do esquemático e a Ferramenta de Captura (`Win+Shift+S`), recortando só a área de interesse. Salve em PNG.
- **Capturas do PACTware:** janela maximizada, LD301 conectado (ícones verdes). Recorte a árvore do projeto e a área do DTM.
- **Fotos:** luz difusa, sem flash direto no display, câmera paralela ao objeto. Salve em JPG. Fotos de catálogo ou da internet só com licença de uso e com a fonte na legenda.
- **Desenhos:** diagrams.net (draw.io), Inkscape ou PowerPoint, fundo branco, exportados em PNG com cerca de 1600 px de largura.
- **Anotações:** setas e números em vermelho, fonte sem serifa. Explique cada número na legenda ou no parágrafo da figura.
- **Cores:** use as mesmas do LasecSimul em todos os desenhos: líquido de processo em **azul**, capilar e lado alto (H) em **laranja**, selo e lado baixo (L) em **amarelo**.

### 15.2 Figuras

| Figura | Arquivo | Tipo | Como obter | O que destacar |
|---:|---|---|---|---|
| 1 | `fig01_transmissor_tanque.jpg` | Foto | Foto de uma instalação real de transmissor de pressão diferencial em vaso fechado (planta, visita técnica ou catálogo com licença). | Tomada de baixo (H) perto do fundo, tomada de cima (L) no topo, capilares ou tubulação de impulso e o transmissor. |
| 2 | `fig02_aberto_fechado.png` | Desenho | Dois tanques lado a lado: aberto, com L para a atmosfera, e fechado, com gás no topo e L ligado ao topo. Transmissor no nível do fundo nos dois. | Altura `h`, `P_gás` agindo sobre o líquido e nas duas tomadas do tanque fechado. |
| 3 | `fig03_perna_seca_molhada.png` | Desenho | Dois tanques fechados: à esquerda com a perna L vazia e um pote de dreno; à direita com a perna L cheia de selo e um pote de selagem no topo. | Líquido de processo em azul e selo em amarelo; potes de dreno e de selagem. |
| 4 | `fig04_instalacao_cotas.png` | Desenho | Redesenhe a instalação simulada: tanque, transmissor `L_{cap}` abaixo da tomada de baixo, capilar laranja (H) e perna molhada amarela (L). Pode partir de uma captura da planta. | Cotas `H`, `h`, `L_{cap}` e `H + L_{cap}`; ao lado de cada perna, a pressão que ela exerce. |
| 5 | `fig05_projeto_completo.png` | Captura LasecSimul | Abra `tanque_pressurizado_ld301.lsproj`, clique em **Run**, deixe as duas válvulas fechadas (nível em 1,5 m) e clique em **Ajustar tudo**. Capture o esquemático inteiro com o selo "Running". | Números de 1 a 9: sliders de entrada e saída, planta, LD301, fonte, resistor, amperímetro, modem HART e osciloscópio. |
| 6 | `fig06_planta.png` | Captura LasecSimul | Com o nível em 1,5 m, abra a válvula de entrada em 50 % por alguns segundos para o líquido subir e feche. Selecione a planta, clique em **Ajustar seleção** e recolha o painel Properties. Capture só a planta. | Líquido azul no tanque, válvulas com o selo de abertura, perna amarela (LOW), capilar laranja (HIGH), cotas `H` e `Lcap` e os displays. |
| 7 | `fig07_propriedades_exportadas.png` | Captura LasecSimul | Selecione a planta, abra o painel **Properties** e role até "Propriedades exportadas". | Os campos de altura, diâmetro, densidades, capilar, pressão do gás e vazões. |
| 8 | `fig08_malha_4_20.png` | Captura LasecSimul | Com o nível em 1,5 m, aproxime a região do LD301 com `Ctrl` + roda do mouse e capture. | Fios HIGH (laranja) e LOW (amarelo), fonte, resistor de 250 Ω, amperímetro com 12,00 mA e modem HART. |
| 9 | `fig09_com0com.png` | Captura Windows | Menu Iniciar → **com0com** → **Setup**. Capture a janela com o par de portas. | Os nomes `CNCB0` e `COM20` do mesmo par. |
| 10 | `fig10_pactware_conectado.png` | Captura PACTware | Com a simulação rodando e o nível em 1,5 m, conecte o LD301 e abra a Parametrização online. | Ícones verdes, tag LT100 e PV de −1500 mmH2O. |
| 11 | `fig11_dtm_calibration.png` | Captura PACTware | Tela **Calibration** com os valores da faixa. | Unidade mmH2O, Lower −3000 e Upper 0. |
| 12 | `fig12_dtm_user_unit.png` | Captura PACTware | Tela **User Unit** com a lista de unidades aberta em **m**. | On, m, 0 (0 %) e 3 (100 %). |
| 13 | `fig13_dtm_display.png` | Captura PACTware | Tela de configuração do display. | First Var = PV e Second Var = PV%. |
| 14 | `fig14_lcd_nivel.png` | Captura LasecSimul (montagem) | Nível em 1,5 m. Aproxime o display do LD301 (zoom de cerca de 300 %) e capture duas vezes com 3 s de intervalo. Monte as duas lado a lado. | "1.500 m" com o ícone PV aceso e "50.00" com o ícone de %. |
| 15 | `fig15_grafico_corrente_nivel.png` | Gráfico | Planilha ou Python com os valores medidos na Parte A: nível no eixo x e corrente no eixo y. Se fizer a Parte B, acrescente a série com o selo de 0,9. | A reta de (0 m; 4 mA) a (3 m; 20 mA) e o deslocamento causado pelo selo de 0,9. |
