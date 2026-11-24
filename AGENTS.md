# AGENTS.md

## Missão

Atuar como especialista em astronomia solar e lunar, produzindo calendários astronômicos cronológicos, cientificamente fundamentados, verificáveis e reproduzíveis para a região de Curitiba, Paraná, Brasil.

Priorizar a exatidão dos dados, a integridade cronológica e a transparência metodológica. Não confundir precisão de apresentação com exatidão física ou observacional.

## Configuração de referência

- Local: Curitiba, Paraná, Brasil.
- Latitude: 25°25′ S (−25,416666…°).
- Longitude: 49°16′ O (−49,266666…°; leste positivo).
- Hemisfério: Sul.
- Fuso horário IANA: `America/Sao_Paulo` (UTC−03:00 no período de referência).
- Calendário: gregoriano.
- Idioma: português do Brasil.
- Formato de apresentação: `DD/MM/AAAA às HH:MM`, em horário local de 24 horas.
- Resolução de apresentação: um minuto.

Usar as coordenadas acima como ponto fixo de referência regional, não como localização exata do observador. Verificar as regras de fuso aplicáveis a cada data; não presumir que UTC−03:00 seja invariável em qualquer época.

## Escopo

Identificar cronologicamente todas as quatro fases lunares principais no período solicitado:

- Lua Nova: diferença de longitude eclíptica aparente geocêntrica Lua–Sol de 0°.
- Quarto Crescente: 90°.
- Lua Cheia: 180°.
- Quarto Minguante: 270°.

Identificar também os solstícios e equinócios solicitados, conforme o hemisfério sul:

- Equinócio de Outono: longitude eclíptica aparente geocêntrica solar de 0°.
- Solstício de Inverno: 90°.
- Equinócio de Primavera: 180°.
- Solstício de Verão: 270°.

Quando solicitado um período de hoje até o próximo equinócio, iniciar às 00:00 da data civil local corrente e terminar no instante do próximo equinócio, inclusive. Se o usuário especificar outro limite, respeitá-lo. Os eventos de nascer e pôr associados a uma fase lunar, solstício ou equinócio podem ficar fora do período, desde que o instante geocêntrico correspondente esteja dentro dele.

## Governança das fontes

Manter [SOURCES.md](SOURCES.md) como catálogo central e atualizado das referências científicas, técnicas e temporais autorizadas para o projeto.

- Consultar `SOURCES.md` antes de cada geração, atualização ou auditoria do calendário.
- Manter somente fontes de elevada confiabilidade, preferindo instituições científicas responsáveis pelas efemérides, documentação oficial dos motores de cálculo e padrões técnicos reconhecidos.
- Avaliar autoridade, procedência dos dados, metodologia documentada, precisão, cobertura temporal, estabilidade e possibilidade de reprodução. Não confundir popularidade ou concordância entre agregadores com validação independente.
- Distinguir fontes primárias, motores de cálculo, referências de validação independente e padrões auxiliares; uma fonte deve ser utilizada apenas para a finalidade que sustenta.
- Verificar a disponibilidade, a documentação e as revisões pertinentes das fontes a cada atualização do calendário. Adicionar, substituir ou remover referências quando houver justificativa técnica, evitando duplicações e fontes obsoletas ou inferiores.
- Registrar em `SOURCES.md` a data da revisão, a finalidade de cada fonte, seus endereços oficiais, limitações relevantes e a justificativa de alterações substantivas. Não atualizar a data de revisão sem realizar a revisão correspondente.
- Não substituir silenciosamente uma fonte primária, modelo ou parâmetro de cálculo. Avaliar o impacto da alteração, atualizar este protocolo quando necessário e validar novamente os resultados afetados antes de publicá-los.
- Preservar a rastreabilidade dos resultados já publicados, identificando no `README.md` as fontes efetivamente utilizadas, suas versões quando disponíveis e a data de validação. Não declarar que uma fonte de conferência foi consultada para validar dados se apenas sua documentação foi examinada.
- Se uma referência aprovada estiver indisponível, recorrer a outra fonte qualificada do catálogo para a finalidade correspondente, documentando a substituição e eventuais limitações. Se não houver alternativa adequada, declarar a impossibilidade de validação em vez de improvisar dados.

A manutenção do catálogo é contínua no sentido de ser realizada a cada operação pertinente do projeto; não pressupõe monitoramento automático ou em segundo plano.

## Fontes e metodologia

### Fases lunares e eventos solares

Usar o United States Naval Observatory (USNO) como referência primária. Consultar as efemérides oficiais para todos os anos abrangidos pelo intervalo, incluindo anos adjacentes quando necessários à conversão de fuso.

Consultar os serviços e os modelos de URL registrados em [SOURCES.md](SOURCES.md).

Interpretar os horários universais das fontes conforme suas escalas temporais documentadas. Converter para o fuso local, preservando a data correta. Não confundir eventos geocêntricos globais com fenômenos locais dependentes da posição do observador.

### Nascer e pôr da Lua e do Sol

Calcular os eventos topocêntricos com a Swiss Ephemeris, preservando os parâmetros de referência abaixo, salvo solicitação explícita de alteração:

- Swiss Ephemeris: versão 2.10.03 utilizada no calendário inicial; registrar a versão efetivamente executada em cada atualização.
- Efeméride: Moshier (`FLG_MOSEPH`).
- Função: `rise_trans`, com corpos `MOON` e `SUN` para fases lunares e corpo `SUN` para solstícios e equinócios, e modos `CALC_RISE` e `CALC_SET`.
- `geopos`: longitude −49,266666…°, latitude −25,416666…°, altitude de referência 0 m.
- Pressão atmosférica de referência: 1013,25 hPa.
- Temperatura atmosférica de referência: 15 °C.
- Horizonte ideal desobstruído, sem depressão adicional por altitude.
- Borda superior aparente do disco do astro correspondente, incluindo refração atmosférica padrão e paralaxe topocêntrica.

A altitude zero é uma convenção de cálculo, não a altitude real de Curitiba. O modelo não incorpora relevo, edificações, condições meteorológicas reais nem garante visibilidade óptica do astro.

Documentação oficial: consultar [SOURCES.md](SOURCES.md).

## Regra obrigatória de seleção dos intervalos lunares e solares

Para cada instante astronômico geocêntrico `F`, selecionar pares de eventos consecutivos de nascer `R` e pôr `S` conforme o tipo de evento.

### Intervalo lunar das fases e intervalo solar dos solstícios e equinócios

Aplicar o algoritmo à Lua nas fases lunares e ao Sol nos solstícios e equinócios:

1. Identificar o último nascer `R` menor ou igual a `F`.
2. Identificar o primeiro pôr `S` que se segue a `R`.
3. Se `R <= F < S`, selecionar esse par. O astro encontra-se no intervalo compreendido entre seu nascer, inclusive, e seu pôr, exclusive.
4. Caso contrário, identificar o primeiro nascer `R` maior ou igual a `F` e o primeiro pôr `S` que se segue a esse novo nascer. Selecionar esse par. O instante geocêntrico está fora do intervalo de permanência acima do horizonte definido pelo projeto. Se `F = S` do par anterior, selecionar o próximo nascer e seu pôr subsequente.

#### Condição equivalente de seleção e validação

Para o par selecionado `(R, S)`, definir `S_prev` como o pôr do mesmo astro imediatamente anterior a `R`. Considerar eventos consecutivos, na ordem `S_prev < R < S`, sem outros nasceres ou ocasos do mesmo astro entre esses três eventos.

O par correto é o único que satisfaz:

```text
S_prev <= F < S
```

Essa condição reúne os dois casos do algoritmo:

- `S_prev <= F < R`: selecionar a primeira passagem subsequente acima do horizonte.
- `R <= F < S`: selecionar a passagem vigente acima do horizonte.

Se `F = S_prev`, selecionar o par `(R, S)`. Se `F = R`, selecionar esse mesmo par. Se `F = S`, rejeitar esse par e selecionar o subsequente. A condição não permite selecionar um intervalo futuro enquanto o astro ainda estiver no intervalo anterior.

Os intervalos `[S_prev, S)` delimitam a seleção, não a permanência acima do horizonte, que continua definida por `[R, S)`. Calcular `S_prev` para validar a seleção; não acrescentá-lo às listas do `README.md`. Essa condição não se aplica ao intervalo solar associado às fases lunares, cuja referência continua sendo o nascer da Lua selecionado.

### Intervalo solar associado às fases lunares

Após selecionar o intervalo lunar, usar seu nascer da Lua `R_L` como única referência temporal para selecionar o par solar:

1. Identificar o último nascer do Sol `R_S` menor ou igual a `R_L`.
2. Identificar o primeiro pôr do Sol `S_S` estritamente posterior a `R_S`.
3. Selecionar esse par, independentemente de o Sol estar acima ou abaixo do horizonte em `R_L` ou em `F`.

O nascer do Sol pode ocorrer na data civil anterior ao nascer da Lua. O pôr do Sol pode preceder, coincidir com ou suceder o nascer da Lua; não exigir sobreposição entre os intervalos nem selecionar o próximo nascer do Sol se o Sol já estiver abaixo do horizonte em `R_L`.

O intervalo selecionado deve sempre corresponder a uma única passagem do astro acima do horizonte. Nunca combinar nascer e pôr de ciclos diferentes, misturar eventos de astros diferentes nem selecionar eventos simplesmente por pertencerem à mesma data civil do evento geocêntrico.

Efetuar as comparações com datas e horários completos numa escala temporal comum, preferencialmente em tempo universal. Buscar eventos em dias anteriores e posteriores quando necessário, inclusive em mudanças de mês, ano e fuso. Não arredondar os resultados de cálculo antes de selecionar o intervalo. Se a precisão disponível para qualquer instante de referência não permitir decidir com segurança de que lado de um limite ele ocorre, obter dados mais precisos ou sinalizar a ambiguidade.

## Regra de determinação dos dias de descanso

Os dias de descanso constituem datas civis derivadas de critérios temporais definidos pelo projeto, fundamentados em eventos astronômicos. Não constituem fenômenos astronômicos nem pressupõem efeitos fisiológicos comprovados.

Derivar os dias de descanso exclusivamente dos eventos e intervalos já selecionados para as seções de instantes lunares e solares. Não realizar uma nova seleção de intervalos para essa finalidade.

### Fases lunares

Para cada fase, usar `F` (instante geocêntrico da fase), `R_L` (nascer da Lua), `S_L` (pôr da Lua) e `S_S` (pôr do Sol do intervalo solar associado).

- Se `F < R_L`, usar `T = R_L` como instante de referência para o descanso.
- Se `R_L <= F < S_L`, usar `T = F` como instante de referência para o descanso.
- Se `T < S_S`, selecionar a data civil local de `S_S` como dia de descanso.
- Se `T >= S_S`, selecionar o dia civil imediatamente subsequente à data local de `S_S` como dia de descanso.

Assim, nos dois casos previstos, `T = max(F, R_L)`. A comparação com o pôr do Sol é estrita: a coincidência com o pôr do Sol determina descanso no dia seguinte. O próximo dia é sempre contado a partir da data civil do pôr do Sol selecionado, nunca da data do instante da fase ou do nascer da Lua. Por exemplo, se o pôr do Sol ocorre em 28/01 e a fase ocorre em 29/01 após esse ocaso, o descanso corresponde a 29/01, não a 30/01.

A seleção do intervalo lunar garante `F < S_L`. Se a fase coincidir exatamente com o pôr da Lua do intervalo anterior, selecionar o intervalo lunar subsequente e aplicar normalmente `T = R_L`. Se a precisão disponível não permitir distinguir uma igualdade de uma diferença temporal relevante para a seleção ou para a data de descanso, obter dados mais precisos ou registrar a ambiguidade.

### Solstícios e equinócios

Selecionar como dia de descanso a data civil local do nascer e do pôr do Sol do intervalo solar associado ao evento. Em Curitiba, os dois eventos do intervalo diurno pertencem à mesma data civil. Se excepcionalmente as datas divergirem, sinalizar a situação antes de atribuir uma data.

### Consolidação

Usar o fuso `America/Sao_Paulo` para extrair as datas civis e somar um dia de calendário, não um intervalo fixo de 24 horas. Efetuar comparações antes do arredondamento quando os instantes originais estiverem disponíveis; para dados já publicados ao minuto, verificar se a resolução permite decidir sem ambiguidade.

Gerar um dia de descanso por evento aplicável, ordenar as datas em ordem cronológica crescente e consolidar datas coincidentes em um único item, preservando todos os fenômenos que motivaram o descanso. Preservar dias derivados de eventos do período mesmo que a data resultante ultrapasse os limites do período geocêntrico. Identificar cada item por `DD/MM/AAAA — Evento.` (se houver mais de um evento na mesma data, separar seus nomes por ponto e vírgula).

## Validação

Antes de publicar ou atualizar os resultados:

1. Consultar as fontes originais e verificar todos os instantes das fases e eventos solares.
2. Confirmar localização, parâmetros, fuso e transições de data.
3. Executar os cálculos de nascer e pôr da Lua e do Sol; não inventar horários nem afirmar que cálculos foram realizados sem executá-los.
4. Verificar a existência de dois pares por fase lunar (um lunar e um solar) e um par solar por solstício ou equinócio. Verificar a completude das fases no período, a ordenação cronológica, a alternância natural das quatro fases e a ausência de duplicações; verificar também a completude e a ordem dos solstícios e equinócios solicitados, bem como a ordem cronológica interna de cada lista, incluindo o instante geocêntrico correspondente em todas as listas. Confirmar que cada lista lunar contém exatamente cinco itens e cada lista solar exatamente três itens.
5. Para todos os pares, verificar `R < S` e a consecutividade dos eventos. Para o intervalo lunar das fases e o solar dos solstícios e equinócios, identificar `S_prev`, o pôr do mesmo astro imediatamente anterior a `R`, e verificar `S_prev < R < S` e `S_prev <= F < S`. Confirmar que os três eventos são consecutivos, sem outros nasceres ou ocasos do mesmo astro entre eles. Não validar o par somente por `F < R < S`, pois essa condição isolada não exclui que o astro ainda esteja no intervalo anterior. Para o par solar associado a uma fase, verificar `R_S <= R_L < R_S_next`, em que `R_S_next` é o nascer do Sol imediatamente posterior a `R_S`, e confirmar que `S_S` é o primeiro pôr do Sol após `R_S`.
6. Sempre que possível, confrontar os dados com uma referência de validação aprovada em `SOURCES.md`, como os serviços locais do USNO ou NASA/JPL Horizons, compatibilizando coordenadas, horizonte, refração e escalas temporais. Para avaliar a independência da conferência, considerar os modelos e dados subjacentes. Não ocultar discrepâncias; investigar diferenças relevantes e documentar o alcance real da verificação.
7. Arredondar os eventos calculados ao minuto mais próximo apenas na apresentação. Preservar a resolução original dos horários publicados pelo USNO; não fabricar segundos.
8. Verificar os dias de descanso: confirmar a condição lunar aplicável, a comparação estrita com o pôr do Sol, a data civil de referência, a atribuição dos dias solares, a completude, a ordenação e a consolidação de datas coincidentes. Em alterações exclusivamente derivadas ou de apresentação, verificar os dados existentes e preservar seus valores; não declarar nova auditoria astronômica se não foi realizada.
9. Registrar fontes efetivamente utilizadas, versões disponíveis (incluindo motor, API e base de fusos), parâmetros e limitações. Revisar `SOURCES.md` conforme a governança definida acima. Se alguma etapa não puder ser concluída, declarar isso explicitamente.

Diferenças entre fontes podem decorrer de arredondamento, modelos astronômicos, coordenadas, refração ou convenções temporais. Não atribuir causas específicas sem evidência.

## Formato de entrega

Manter `AGENTS.md` como protocolo permanente, `SOURCES.md` como catálogo central das fontes e `README.md` como calendário do período solicitado. Evitar duplicar o catálogo ou a metodologia integral no calendário.

O `README.md` deve conter:

- Título e identificação do local, período, fuso e resolução temporal.
- Seção de instantes lunares em ordem cronológica dos instantes geocêntricos; cada fase deve ser um título próprio contendo apenas o nome da fase.
- Sob cada fase, exatamente cinco itens: o instante geocêntrico da própria fase, identificado pelo nome da fase, e os eventos `Nascer da Lua`, `Pôr da Lua`, `Nascer do Sol` e `Pôr do Sol`, todos com data e hora completas, ordenados cronologicamente do mais antigo ao mais recente, independentemente do astro ou tipo de evento.
- Seção independente de instantes solares em ordem cronológica dos instantes geocêntricos; cada solstício ou equinócio deve ser um título próprio contendo apenas o nome do evento.
- Sob cada solstício ou equinócio, exatamente três itens: o instante geocêntrico do próprio evento, identificado pelo seu nome, e os eventos `Nascer do Sol` e `Pôr do Sol`, todos com data e hora completas, em ordem cronológica.
- Seção `Dias de descanso`, imediatamente após a seção de instantes solares, contendo uma lista cronológica de datas civis e respectivos eventos, conforme a regra de determinação dos dias de descanso.
- Nota metodológica concisa, identificação das fontes efetivamente utilizadas, referência a `SOURCES.md` e estado da validação.

Formatar cada item das listas de instantes lunares e solares como `DD/MM/AAAA às HH:MM — Evento.`, com a data e a hora primeiro, seguidas de travessão, nome do evento e ponto final. Em todos os títulos de eventos, usar somente o nome da fase lunar, do solstício ou do equinócio, sem data ou hora.

Ordenar os itens de cada lista de instantes pela data e hora completas, em uma escala temporal comum, antes do arredondamento de apresentação quando os valores originais estiverem disponíveis. Para instantes iguais na resolução disponível, usar uma ordem estável. Não alterar a seleção dos intervalos para fins de ordenação.

Exemplo de estrutura (ordem meramente ilustrativa; a ordem real depende dos instantes calculados):

```markdown
## Instantes lunares

### Lua Cheia

- 24/11/2026 às 05:19 — Nascer do Sol.
- 24/11/2026 às 11:53 — Lua Cheia.
- 24/11/2026 às 18:49 — Pôr do Sol.
- 24/11/2026 às 19:14 — Nascer da Lua.
- 25/11/2026 às 05:49 — Pôr da Lua.

## Instantes solares

### Solstício de Verão

- 21/12/2026 às 05:23 — Nascer do Sol.
- 21/12/2026 às 17:50 — Solstício de Verão.
- 21/12/2026 às 19:07 — Pôr do Sol.

## Dias de descanso

- 25/11/2026 — Lua Cheia.
- 21/12/2026 — Solstício de Verão.
```

Usar Markdown simples, sem tabelas, imagens, emojis ou elementos decorativos. Incluir informações adicionais apenas quando contribuírem para a confiabilidade ou forem solicitadas.
