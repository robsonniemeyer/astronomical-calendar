# SOURCES.md

Catálogo central das fontes de referência para o calendário astronômico solar e lunar de Curitiba, Paraná, Brasil.

Última revisão do catálogo: 24/11/2026.

O protocolo de seleção, manutenção e utilização das fontes está em [AGENTS.md](AGENTS.md). A presença de uma referência neste catálogo não significa que ela tenha sido utilizada na validação de todos os resultados do [README.md](README.md).

## 1. United States Naval Observatory (USNO)

Astronomical Applications Department — fonte primária para os instantes geocêntricos das fases lunares, solstícios e equinócios; referência de conferência para os eventos locais de nascer e pôr.

- [Dates of Primary Phases of the Moon](https://aa.usno.navy.mil/data/MoonPhases)
- [Earth’s Seasons — Equinoxes and Solstices](https://aa.usno.navy.mil/data/Earth_Seasons)
- [Complete Sun and Moon Data for One Day](https://aa.usno.navy.mil/data/RS_OneDay)
- [Moonrise/Moonset — Annual Tables](https://aa.usno.navy.mil/data/RS_OneYear)
- [Rise, Set, and Twilight Definitions](https://aa.usno.navy.mil/faq/RST_defs)
- [Application Programming Interface Documentation](https://aa.usno.navy.mil/data/api)

### Consultas programáticas

Fases lunares por ano:

```text
https://aa.usno.navy.mil/api/moon/phases/year?year=AAAA
```

Solstícios e equinócios por ano:

```text
https://aa.usno.navy.mil/api/seasons?year=AAAA
```

Eventos locais diários — exemplo para o ponto de referência do projeto:

```text
https://aa.usno.navy.mil/api/rstt/oneday?date=2026-11-24&coords=-25.4166666667,-49.2666666667&tz=-3
```

`coords` recebe latitude e longitude, nessa ordem. O valor `tz=-3` é válido para a data do exemplo; calcular o deslocamento aplicável para cada data consultada. Para as fases e estações, preferir consultas em tempo universal e realizar a conversão local conforme o protocolo. Respeitar as escalas temporais documentadas, a cobertura das consultas e a resolução original dos dados.

## 2. Swiss Ephemeris — Astrodienst

Motor de cálculo adotado para o nascer e pôr topocêntricos da Lua e do Sol. A documentação do próprio desenvolvedor é a referência normativa para as funções, opções e parâmetros da biblioteca.

- [Programmer’s Documentation](https://www.astro.com/swisseph/swephprg.htm)
- [Programmer’s Documentation — versão 2.10](https://www.astro.com/ftp/swisseph/doc/swephprg.2.10.htm)

Utilizar `rise_trans` (interface Python de `swe_rise_trans`) com os corpos `MOON` e `SUN`, o modelo Moshier e os parâmetros definidos em `AGENTS.md`. A versão utilizada no calendário inicial é 2.10.03; registrar a versão efetivamente executada nas atualizações.

O resultado depende do modelo astronômico, da posição geográfica, da refração e da definição de horizonte. A biblioteca oferece cálculo, não medição das condições reais do observador. Uma atualização de versão ou alteração de modelo exige avaliação e nova validação dos resultados afetados.

## 3. NASA/JPL — Horizons System

Jet Propulsion Laboratory, Solar System Dynamics — referência de alta precisão para conferência independente dos cálculos astronômicos, quando tecnicamente compatível com o fenômeno e a metodologia adotada.

- [Horizons System](https://ssd.jpl.nasa.gov/horizons/)
- [Horizons User Manual](https://ssd.jpl.nasa.gov/horizons/manual.html)

Empregar efemérides de observador para conferir eventos locais de nascer e pôr ou obter posições astronômicas para verificações adicionais. A comparação exige compatibilidade de coordenadas, altitude, escala temporal, horizonte, refração e definição do evento. Avaliar a independência dos modelos e dados antes de tratar a concordância como validação independente.

Aprovado para futuras verificações; não utilizado nesta revisão para recalcular ou validar o calendário inicial.

## 4. IANA — Time Zone Database

Referência técnica para as regras de conversão de horários civis, incluindo deslocamentos UTC e horário de verão.

- [Time Zone Database](https://www.iana.org/time-zones)
- [Time Zone Database Releases](https://www.iana.org/time-zones/releases)

Utilizar a zona `America/Sao_Paulo` e verificar a versão da base de fusos disponível no ambiente de execução. Não presumir que o deslocamento atual se aplique a todas as datas históricas ou futuras. A base representa regras conhecidas e pode ser revista após mudanças legais.

## Histórico de revisão

- 24/11/2026 — Criação do catálogo. Verificação das referências oficiais e da documentação dos serviços. Centralização de USNO e Swiss Ephemeris; inclusão de NASA/JPL Horizons como referência de conferência e IANA como padrão de conversão civil. As fontes agregadoras não integram o catálogo por não serem necessárias diante das referências selecionadas. Esta revisão trata das fontes, não de uma nova auditoria dos instantes astronômicos.

- 24/11/2026 — Extensão do escopo para nascer e pôr do Sol. Confirmada a documentação oficial da Swiss Ephemeris (§§ 8.12 e 8.12.1) para a definição astronômica de nascer e ocaso solares e consultada a API de estações do USNO (4.0.1). A consulta diária do USNO para conferência dos horários solares não retornou dados. As fontes e sua hierarquia foram mantidas.

- 24/11/2026 — Inclusão de intervalos solares associados às fases lunares, sem mudança de fontes ou parâmetros. Reconsultadas as efemérides de fases e estações do USNO (API 4.0.1), a documentação Swiss Ephemeris 2.10 (§§ 7.12 e 7.12.1) e as páginas institucionais do IANA e NASA/JPL Horizons. Base de fusos local: 2025b; a IANA anuncia a versão 2026f, não instalada neste ambiente. A consulta diária de conferência do USNO permaneceu inacessível. NASA/JPL Horizons não foi utilizado para validar os horários desta atualização.
