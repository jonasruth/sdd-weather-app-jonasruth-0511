# Discovery — Aplicação de Previsão do Tempo

## Contexto

A empresa solicitou uma aplicação para que pessoas consultem as condições
meteorológicas de cidades. O briefing estabelece busca por cidade, visualização
do clima atual, previsão para cinco dias, alternância entre Celsius e
Fahrenheit e uso em dispositivos móveis.

O público informado são pessoas adultas que usam desktop, laptop ou smartphone.
Além de buscar cidades, a aplicação deve tentar obter a localização atual
automaticamente e sempre permitir que a pessoa escolha outra cidade.

O objetivo desta etapa é esclarecer necessidades, restrições e decisões que
devem orientar a especificação do produto. O briefing ainda não define todos os
dados meteorológicos exibidos, o comportamento da busca nem os requisitos de
disponibilidade e acessibilidade.

## Requisitos Funcionais

- **RF-01 — Selecionar cidade:** a aplicação deve tentar obter a localização da
	pessoa automaticamente e permitir que ela pesquise e selecione uma cidade
	manualmente, inclusive para substituir a localização obtida.
- **RF-02 — Consultar clima atual:** após selecionar uma cidade, a aplicação deve
	apresentar temperatura, sensação térmica, condição meteorológica, vento,
	umidade e precipitação.
- **RF-03 — Consultar previsão:** após selecionar uma cidade, a aplicação deve
	apresentar um resumo diário para cinco dias, incluindo o dia atual e os quatro
	dias seguintes.
- **RF-04 — Alternar unidade de temperatura:** a pessoa usuária deve poder
	alternar a temperatura entre Celsius e Fahrenheit. A unidade escolhida deve
	ser lembrada para visitas futuras.
- **RF-05 — Atualizar dados:** a pessoa usuária deve poder solicitar a
	atualização dos dados meteorológicos da cidade selecionada.
- **RF-06 — Informar atualização:** a aplicação deve indicar quando os dados
	foram atualizados, em formato relativo e legível, como “atualizado há 5
	minutos”.
- **RF-07 — Cidades recentes:** a aplicação deve manter as cinco cidades mais
	recentemente pesquisadas e apresentá-las em ordem da mais recente para a mais
	antiga, para seleção rápida.

## Requisitos Não-Funcionais

- **RNF-01 — Responsividade:** conteúdo e controles devem permanecer legíveis e
	utilizáveis em smartphones, laptops e desktops, sem sobreposição ou perda das
	funções principais.
- **RNF-02 — Acessibilidade:** fluxos e controles principais devem ser
	utilizáveis por teclado e apresentar nomes e estados compreensíveis para
	tecnologias assistivas.
- **RNF-03 — Feedback de consulta:** ao iniciar uma consulta, a aplicação deve
	informar que ela está em andamento. O estado de carregamento deve permanecer
	visível por no mínimo dois segundos, mesmo se a resposta chegar antes.
- **RNF-04 — Resiliência:** falhas de rede ou do serviço meteorológico não devem
	encerrar a aplicação; a pessoa usuária deve receber uma mensagem clara e uma
	forma de tentar novamente.
- **RNF-05 — Privacidade:** a aplicação não deve exigir conta nem coletar ou
	enviar dados de uso para um servidor. O histórico das cinco cidades e a
	preferência de unidade devem ser mantidos somente no dispositivo, conforme
	decisão de produto a validar.
- **RNF-06 — Localização:** a interface deve ser apresentada em português do
	Brasil, conforme decisão do treinamento.
- **RNF-07 — Clareza dos dados:** cidade selecionada, unidade de temperatura e
	período da previsão devem ser identificáveis para reduzir interpretações
	equivocadas.

## Riscos

| Categoria | Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- | --- |
| Técnico | Indisponibilidade ou lentidão do Open-Meteo impede ou atrasa a consulta | Média | Alto | Exibir carregamento, explicar a falha em linguagem simples e permitir nova tentativa; monitorar falhas e validar limites e condições de uso do serviço. |
| Técnico | Resposta incompleta ou formato alterado causa dados ausentes ou incorretos | Média | Alto | Validar os dados recebidos antes de exibi-los, tratar campos ausentes e manter testes para os dados consumidos. |
| Técnico | Falta de internet interrompe a busca ou atualização | Alta | Médio | Informar que a conexão foi interrompida ou que não foi possível atualizar e oferecer nova tentativa sem perder a cidade selecionada. |
| Técnico | Localização automática falha, é negada ou aponta para um local impreciso | Média | Médio | Tratar recusa e falha sem bloquear o app; deixar a busca manual visível e permitir substituir a localização sugerida. |
| Técnico | Fuso horário do dispositivo difere do fuso da cidade e confunde o dia da previsão | Média | Médio | Aplicar a regra de fuso definida pelo produto de forma consistente e indicar claramente o dia e a atualização dos dados. |
| Técnico | Armazenamento local indisponível ou apagado faz perder unidade e cidades recentes | Baixa | Médio | Tratar a falta de armazenamento local sem impedir consultas; usar valores padrão e explicar apenas quando necessário. |
| Produto | Cidades homônimas ou resultados de busca pouco claros levam à consulta da cidade errada | Média | Alto | Mostrar contexto de localização suficiente, como estado e país, antes da seleção; permitir corrigir a cidade rapidamente. |
| Produto | A espera mínima de dois segundos é percebida como lentidão, mesmo quando os dados chegam antes | Alta | Médio | Validar a decisão com pessoas usuárias e limitar a espera visual ao mínimo acordado, sem atrasar a requisição ou esconder erros. |
| Produto | Histórico local de buscas e preferência salva são interpretados como coleta de dados | Média | Alto | Deixar claro que os dados ficam apenas no dispositivo, não são enviados ao servidor, e permitir apagar o histórico. |
| Produto | Layout ou controles não atendem às necessidades de acessibilidade ou a diferentes telas | Média | Alto | Validar navegação por teclado, leitor de tela, contraste e fluxos principais em smartphones, laptops e desktops. |

## Perguntas em Aberto (Open Questions)

As perguntas abaixo permanecem abertas após as respostas recebidas. Cada
resposta pode alterar o escopo ou a experiência.

1. **Como diferenciar cidades homônimas e quais dados de localização mostrar
	para cada resultado?** **Impacto:** sem contexto como estado ou país, a pessoa
	pode selecionar a cidade errada.
2. **A busca deve oferecer sugestões durante a digitação e como tratar acentos,
	nomes alternativos e ausência de resultados?** **Impacto:** afeta a facilidade
	de encontrar uma cidade e a recuperação de buscas malsucedidas.
3. **Como proceder quando a permissão de localização é negada, indisponível ou
	imprecisa?** **Impacto:** determina se e como a busca manual fica disponível
	como alternativa.
4. **“Não coleta nada” permite guardar no próprio dispositivo as cidades
	recentes e a unidade escolhida?** **Impacto:** essas funções exigem
	persistência local, embora não enviem dados ao servidor.
5. **Qual é o tempo máximo aceitável para uma consulta?** **Impacto:** foi
	definida uma espera mínima de dois segundos, mas não um limite para respostas
	lentas ou falhas.
6. **Quais navegadores e tecnologias assistivas devem ser suportados?**
	**Impacto:** “responsivo em geral” não determina uma matriz objetiva de
	compatibilidade e validação.
7. **Como identificar e comunicar a atualidade dos dados e o período de
	precipitação exibido?** **Impacto:** sem definição, o horário de atualização
	pode ser interpretado como horário da condição meteorológica.

## Suposições (Assumptions)

- Se a localização automática não estiver disponível ou não for autorizada, a
	pessoa poderá buscar uma cidade manualmente.
- O histórico de cinco cidades e a preferência de unidade serão guardados no
	dispositivo, sem sincronização ou envio ao servidor; essa interpretação de
	“não coleta nada” ainda precisa de confirmação.
- A aplicação é um site responsivo para adultos em smartphones, laptops e
	desktops, não necessariamente um aplicativo instalado.

## Personas e Objetivos

- **Viajante:** quer consultar o clima atual e a previsão da cidade de destino
	para se preparar para a viagem.
- **Esportista:** quer verificar as condições meteorológicas antes de praticar
	atividades ao ar livre.
- **Dona de casa:** quer acompanhar as condições do tempo para planejar as
	atividades e a rotina da casa.

## Decisões

As decisões abaixo fecham questões do briefing para orientar a especificação.

- **Fonte de dados: Open-Meteo, sem API key.** **Justificativa:** atende à
	necessidade de dados meteorológicos sem exigir que a pessoa usuária configure
	uma chave. **Resolve:** qual fonte será usada e se é necessário obter uma
	chave de API.
- **“Cinco dias” significa hoje mais os quatro dias seguintes.**
	**Justificativa:** estabelece um período único e previsível para a previsão.
	**Resolve:** se o dia atual está incluído na contagem dos cinco dias.
- **Unidade padrão: Celsius.** **Justificativa:** define uma apresentação
	inicial consistente para o público-alvo do produto. **Resolve:** qual unidade
	será exibida inicialmente; a pessoa ainda poderá alternar para Fahrenheit.
- **Sem autenticação e sem persistência de dados em servidor.**
	**Justificativa:** mantém o uso direto, sem conta, e limita dados mantidos pela
	empresa. **Resolve:** se haverá cadastro, login ou armazenamento de dados em
	servidor. **Não resolve:** se a persistência local das cidades recentes e da
	unidade é compatível com a expectativa de “não coletar nada”; esse ponto segue
	aberto.
- **Idioma da interface: português do Brasil (pt-BR).** **Justificativa:**
	define o idioma principal da experiência. **Resolve:** qual idioma será usado
	na interface.

### Decisões Complementares do Produto

- **Conteúdo meteorológico:** temperatura, sensação térmica, condição, vento,
	umidade e precipitação; previsão em resumo diário.
- **Unidade:** a pessoa usuária pode alternar Celsius/Fahrenheit; por enquanto,
	o controle altera somente a temperatura, decisão ainda sujeita a revisão.
- **Público e dispositivos:** pessoas adultas em smartphones, laptops e
	desktops; a interface deve ser responsiva.
- **Localização e fuso horário:** tentar obter a localização e o fuso horário
	automaticamente, permitir substituir a cidade e usar o horário de Brasília
	como fallback de fuso horário.
- **Cidades e preferência:** manter localmente as cinco cidades mais recentes e
	a unidade escolhida, sem sincronização ou envio ao servidor; a compatibilidade
	disso com “não coletar nada” permanece em aberto.
- **Atualização:** oferecer ação manual para atualizar e indicar o tempo desde a
	última atualização em linguagem legível.
- **Conectividade:** quando a atualização falhar por falta de rede, informar o
	problema em linguagem simples e permitir nova tentativa.
- **Feedback e métrica de sucesso:** manter o carregamento visível por no mínimo
	dois segundos; considerar sucesso quando a cidade for consultada e os dados
	retornados.
- **Coleta de dados:** não coletar dados de uso ou pessoais.