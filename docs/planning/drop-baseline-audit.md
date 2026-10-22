# Auditoria inicial de drops — 22/10/2026

Planejamento somente. Nenhum parâmetro do servidor foi alterado. Sem simulador implementado: a Fase 0 da skill solicitada exige confirmar métricas com o designer antes das fases de extração/simulação.

## Evidências

Código local no commit `f8af54d37e8849d8687e659c9b091c1e58ee12de`. Exportação de configuração do servidor fornecida pelo agente principal em `/tmp/openmu-balance-config.zip`, sem contas. SHA-256 do JSON GameConfiguration: `7e48a3115ba8ed1cc5bfdd877e209386d7555c07c26f1b423808cb5f4c407020`.

A exportação confirma Chance=0.2 nas três boxes, Luck=0.25, Excellent Defense AddChance=0.001, MaximumOptionsPerItem=2 e seis opções de defesa. Pools exportados: +1=67 itens, +2=74, +3=72. Correspondem à base atual; as futuras restrições de classes/fase exigirão recalcular os pools.

Fontes de método: [skill solicitada](https://github.com/thedivergentai/Game-Dev-Agentic-Skills/blob/main/skills/godot-monte-carlo-balancer/SKILL.md), e `references/00-game-audit.md` lido na cópia local `.local/balance-skill/00-game-audit.md`. Adaptar Resource Godot para configuração persistida OpenMU; não transportar bandas genéricas de win rate para grind.

## Cadeia real de obtenção

1. Dourados garantem Box of Kundun: `src/Persistence/Initialization/Version095d/InvasionMobsInitialization.cs:151` define Chance=1 e nível 7+tier. Season 6 herda esse método: `VersionSeasonSix/InvasionMobsInitialization.cs:69` Golden Goblin +1; `:102` Golden Dragon +3; Golden Titan +2 vem da base `Version095d/InvasionMobsInitialization.cs:142`.
2. Abrir uma box +1/+2/+3: 20% item Excellent; 80% Zen (50k/100k/150k). `VersionSeasonSix/Items/BoxOfLuck.cs:362`, `:410`, `:459`, `:1376`. O fallback tem Chance=1 mas o gerador seleciona primeiro o grupo menor e usa limiar total 1: não normaliza 0.2+1 para 1.2. `src/GameLogic/DefaultDropGenerator.cs:108` e `:555`.
3. Dentro do grupo Excellent o item é uniforme entre as definições do pool: `DefaultDropGenerator.cs:423`, `:433`. Não há favorecimento automático à classe do personagem.
4. Luck é sorteada normalmente antes de adicionar opções Excellent: `DefaultDropGenerator.cs:154`, `:180`, `:393`; configuração `src/Persistence/Initialization/GameConfigurationInitializerBase.cs:225` =25%, quando o item aceita Luck.
5. Excellent tem uma opção garantida uniforme; há uma tentativa independente para a segunda opção, probabilidade 0.001 = 0.1%, sem duplicatas. `DefaultDropGenerator.cs:462`. Não é 0.1% de ter qualquer Excellent; é a chance de uma segunda opção, condicionado a já receber item Excellent.
6. Opções defensivas têm seis possibilidades, DD=4% e Zen=+40% entre elas: `src/Persistence/Initialization/Items/ExcellentOptions.cs:116`. São propriedades de grupos defensivos (armaduras/escudos e alguns acessórios); armas usam opções ofensivas, portanto DD+Zen não é um alvo válido para armas. Acessórios exigem checagem de Luck; não aplicar automaticamente as probabilidades de armadura a eles.

O boss Illusion of Kundun 7 (#275) é outro objeto, definido em `src/Persistence/Initialization/VersionSeasonSix/Maps/Kalima7.cs:345`; não confundir matar Kundun com abrir Box of Kundun. Este relatório calcula caixas e suas fontes douradas, não promete loot específico do boss.

## Probabilidades analíticas

Seja `e=0.001`, `L=0.25`, `N` o tamanho do pool, e `d=0.2`. Condicionado a um item defensivo compatível:

- Uma opção defensiva específica presente: `(1+e)/6 = 16.68333%`.
- Essa opção e Luck: `L*(1+e)/6 = 4.170833%`.
- DD e Zen juntos: `e/C(6,2) = 1/15000 = 0.00666667%`.
- DD e Zen juntos e Luck: `L*e/15 = 1/60000 = 0.00166667%`.
- DD OU Zen: `(1-e)*2/6 + e*(1-C(4,2)/C(6,2)) = 33.36%`. Não multiplicar como opções independentes; o limite de duas gera correlação.

Para uma peça específica de armadura presente na box, multiplicar por `d/N`. Para qualquer peça aceitável, somar as probabilidades das peças compatíveis; acessórios sem Luck e armas não entram nesse conjunto.

Exemplo estrito: uma peça específica da box +3 com Luck+DD+Zen tem `p=(0.2/72)*(1/60000)=1/21600000` por box. Isso não representa conseguir qualquer item bom: representa uma combinação dupla exata com Luck e peça exata.

Para tentativas IID sem pity, `E[T]=1/p` e `Q(q)=ceil(log(1-q)/log(1-p))`:

| Alvo e unidade | Média | Mediana | p90 |
|---|---:|---:|---:|
| Opção defensiva específica + Luck, por item defensivo elegível | 23.98 | 17 | 55 |
| DD+Zen+Luck, por item defensivo elegível | 60,000 | 41,589 | 138,154 |
| Peça específica +3 DD+Zen+Luck, por box +3 | 21,600,000 | 14,971,979 | 49,735,837 |

São resultados analíticos, não estimativas Monte Carlo; não possuem erro amostral. Há incerteza sobre comportamento/economia, quantidade de tentativas e definição de alvo. Sem kills/h ou boxes/h medidos não converter para horas. Nenhum tempo-alvo de 20–30h foi aceito pelo jogador; ele explicitamente prefere mais grind.

A base comum usa grupo Excellent Chance=0.0001 e joias=0.001 (`GameConfigurationInitializerBase.cs:203–220`), porém isso NÃO significa universalmente 0.01%/0.1% por monstro: filtros de nível, grupos adicionais, totalChance e drops garantidos alteram o resultado. `DefaultDropGenerator.cs:273–320` normaliza grupos probabilísticos quando a soma excede 1 e grupos garantidos consomem slots primeiro. Calcular por mapa/monstro e configuração exportada antes de publicar taxa efetiva.

## Decisões necessárias antes de simular

- Escolher classe e alvo: primeira peça útil, set funcional com uma opção boa, ou set otimizado com duas opções exatas? Tratar esses marcos separadamente.
- Definir faixas de tempo desejadas (mediana e p90) para primeiro reset, MR e aquisição de melhoria relevante; distinguir horas ativas e tempo de mundo.
- Definir calendário/uptime: bots farmam só durante a sessão humana ou continuamente? Quantos, com quais limites, consumo próprio, estoque e oferta ao jogador?
- Definir comércio: venda por Zen/joias, negociação, preço/raridade e sinks. Bots que geram itens sem origem auditável equivalem a uma loja que injeta recursos.
- Definir loot em party e prioridade: bots ajudam a matar mas compartilham recompensa ou entregam tudo? Isso muda o ganho humano por evento.
- Definir resets/MR: níveis, quantidade, custos, atributos mantidos/perdidos, moeda e preços do shop, limites por fase. O shop não pode furar teto de conteúdo.
- Medir disponibilidade de dourados, deslocamento, kill/clear time, falhas, mortes, custo de consumíveis, DS/BC e taxa de venda dos bots. Calibrar em instância isolada quando autorizado.

Comparar posteriormente: drops mais generosos; drops moderados + comércio com bots; e proteção de azar/recurso determinístico (ex. shop MR) limitado pela fase. Comparar mediana/p90 de aquisição e inflow/outflow de joias/Zen, não só média de drop. Bots amplificam produção: multiplicar indistintamente drops e população pode saturar o mundo. Simular carreiras completas e consumos antes de aprovar valores finais.
