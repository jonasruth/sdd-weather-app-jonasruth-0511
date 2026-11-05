# Especificação do Produto — Weather App

## Overview

O Weather App permite que pessoas adultas consultem condições meteorológicas
atuais e uma previsão diária de cinco dias para uma cidade. O produto deve
facilitar a consulta por localização automática ou busca manual, apresentar
informações compreensíveis em português do Brasil e funcionar em smartphones,
laptops e desktops.

Uma consulta tem sucesso quando os dados meteorológicos da cidade selecionada
são recebidos e apresentados. O produto usa Open-Meteo, inicia em Celsius e
oferece uma interface em pt-BR. Não exige conta, não coleta analytics e não
armazena dados em servidor. A preferência de unidade e as cinco cidades
consultadas mais recentemente são armazenadas somente no dispositivo. As
coordenadas necessárias à consulta são enviadas ao provedor externo e não são
armazenadas pela aplicação.

## Functional Requirements

- **FR-01 — Localização automática:** na primeira abertura, a aplicação deve
  tentar obter a localização atual do dispositivo. A busca manual deve
  permanecer disponível durante a tentativa e permitir substituir a cidade
  sugerida. A tentativa deve terminar após 10 segundos se não retornar posição.
- **FR-02 — Busca e seleção de cidade:** a pessoa deve poder informar o nome de
  uma cidade, consultar resultados e selecionar a cidade desejada. O termo deve
  ser removido de espaços externos antes da busca; nomes com acentos e caracteres
  Unicode devem ser aceitos. Cada resultado deve mostrar cidade, região
  administrativa e país quando disponíveis.
- **FR-03 — Condições atuais:** para a cidade selecionada, a aplicação deve
  apresentar temperatura, sensação térmica, condição meteorológica, velocidade
  do vento, umidade relativa e precipitação. As unidades devem ser temperatura
  em Celsius/Fahrenheit conforme preferência, vento em km/h, umidade em % e
  precipitação em mm, acumulada na hora anterior conforme o dado atual do
  provedor.
- **FR-04 — Previsão diária:** para a cidade selecionada, a aplicação deve
  apresentar cinco resumos diários: hoje e os quatro dias seguintes no fuso
  horário do dispositivo, contendo data, condição, temperaturas mínima e
  máxima, e precipitação acumulada do dia. Usar o fuso de Brasília se o do
  dispositivo não puder ser identificado.
- **FR-05 — Unidade de temperatura:** a aplicação deve iniciar em Celsius e
  permitir alternar entre Celsius e Fahrenheit. A unidade escolhida deve ser
  lembrada em visitas futuras. A alternância afeta somente valores de
  temperatura.
- **FR-06 — Atualização manual:** a pessoa deve poder solicitar a atualização
  dos dados atuais e da previsão para a cidade selecionada. Após uma atualização
  bem-sucedida, a aplicação deve indicar o tempo desde o recebimento bem-sucedido
  da resposta em linguagem relativa, por exemplo, “Atualizado há 5 minutos”.
- **FR-07 — Cidades recentes:** a aplicação deve apresentar as cinco cidades
  consultadas com sucesso mais recentemente, em ordem da mais recente para a
  mais antiga. Uma nova consulta da mesma cidade deve movê-la para o início sem
  criar duplicata. A lista e a preferência de unidade devem persistir somente
  no dispositivo.
- **FR-08 — Falhas de consulta:** se a consulta ou atualização não puder ser
  concluída, a aplicação deve explicar o problema em linguagem simples e
  permitir tentar novamente sem obrigar a pessoa a redigitar a cidade.

## User Stories

- **US-01 — Viajante:** quero que o app identifique minha localização ou me permita
  buscar e escolher uma cidade para consultar o destino que me interessa.
  **Requisitos relacionados:** FR-01, FR-02.
- **US-02 — Viajante:** quero consultar as condições atuais e a previsão diária da
  cidade de destino para me preparar para a viagem. **Requisitos relacionados:**
  FR-03, FR-04.
- **US-03 — Esportista:** quero verificar temperatura, sensação térmica, condição,
  vento, umidade e precipitação para planejar atividades ao ar livre.
  **Requisitos relacionados:** FR-03.
- **US-04 — Dona de casa:** quero consultar o resumo diário de hoje e dos próximos
  quatro dias para organizar as atividades e a rotina da casa.
  **Requisitos relacionados:** FR-04.
- **US-05 — Viajante:** quero alternar entre Celsius e Fahrenheit e manter minha
  preferência para consultar temperaturas na unidade que prefiro.
  **Requisitos relacionados:** FR-05.
- **US-06 — Dona de casa:** quero atualizar os dados da cidade e ver há quanto
  tempo foram atualizados para tomar decisões com informação recente.
  **Requisitos relacionados:** FR-06.
- **US-07 — Esportista:** quero selecionar uma cidade pesquisada recentemente para
  consultar seu clima novamente sem redigitar o nome.
  **Requisitos relacionados:** FR-07.
- **US-08 — Viajante:** quero receber uma mensagem simples quando uma consulta
  falhar e poder tentar novamente para obter os dados sem repetir a busca.
  **Requisitos relacionados:** FR-08.

## Acceptance Criteria

Os critérios abaixo usam **Dado / Quando / Então** e descrevem resultados
observáveis. Cada requisito funcional possui critérios verificáveis.

### FR-01 — Localização automática

- **AC-FR01-01 —** **Dado** que esta é a primeira abertura e a permissão de localização está
  concedida, **Quando** a aplicação inicia, **Então** ela deve obter a posição,
  identificar uma cidade e iniciar sua consulta.
- **AC-FR01-02 —** **Dado** que a permissão foi negada ou a posição não foi obtida, **Quando** a
  pessoa nega a permissão ou passam 10 segundos sem posição, **Então** a busca
  manual deve permanecer visível e utilizável sem aguardar mais.
- **AC-FR01-03 —** **Dado** que uma cidade foi identificada automaticamente, **Quando** a pessoa
  seleciona outra cidade nos resultados da busca, **Então** o nome da cidade
  selecionada deve ser exibido e a consulta seguinte deve usar essa cidade.

### FR-02 — Busca e seleção de cidade

- **AC-FR02-01 —** **Dado** que a pessoa informa um termo não vazio com resultados conhecidos,
  **Quando** solicita a busca, **Então** a lista deve apresentar os resultados
  correspondentes retornados pelo serviço de geocodificação.
- **AC-FR02-02 —** **Dado** que os resultados incluem cidades de mesmo nome, **Quando** a lista é
  exibida, **Então** cada resultado deve mostrar o contexto geográfico disponível,
  incluindo estado ou país quando fornecidos pelo serviço.
- **AC-FR02-03 —** **Dado** que a lista contém um resultado selecionável, **Quando** a pessoa o
  seleciona, **Então** o nome desse resultado deve aparecer como cidade ativa e a
  consulta meteorológica deve usar suas coordenadas.
- **AC-FR02-04 —** **Dado** que o serviço retorna zero resultados, **Quando** a busca termina,
  **Então** a aplicação deve exibir uma mensagem de cidade não encontrada e
  manter o campo disponível para nova busca.
- **AC-FR02-05 —** **Dado** que a pessoa busca `São Paulo`, **Quando** o provedor retorna uma
  correspondência com esse nome, **Então** a busca deve exibir essa
  correspondência sem remover ou corromper os caracteres acentuados.
- **AC-FR02-06 —** **Dado** que o campo de busca está vazio ou contém somente espaços, **Quando**
  a pessoa solicita a busca, **Então** nenhuma consulta de geocodificação deve ser
  iniciada e uma mensagem deve solicitar o nome de uma cidade.

### FR-03 — Condições atuais

- **AC-FR03-01 —** **Dado** que a resposta da cidade selecionada contém os seis campos atuais,
  **Quando** a consulta termina, **Então** temperatura, sensação térmica,
  condição, vento, umidade e precipitação devem estar visíveis.
- **AC-FR03-02 —** **Dado** que os valores recebidos são temperatura `20 °C`, vento `12 km/h`,
  umidade `65%` e precipitação `0,4 mm`, **Quando** a unidade selecionada é
  Celsius, **Então** esses valores e unidades devem ser exibidos sem troca entre
  campos.
- **AC-FR03-03 —** **Dado** que o provedor retorna precipitação de `0,4 mm` para a hora anterior,
  **Quando** as condições atuais são exibidas, **Então** o valor deve aparecer
  em mm e identificado como acumulado na hora anterior.
- **AC-FR03-04 —** **Dado** que um campo atual está ausente na resposta, **Quando** os dados são
  apresentados, **Então** esse campo deve ser indicado como indisponível e nenhum
  valor deve ser mostrado para ele.
- **AC-FR03-05 —** **Dado** que os dados atuais estão visíveis, **Quando** a pessoa consulta o
  painel, **Então** o nome da cidade e a unidade aplicada à temperatura devem
  estar visíveis.

### FR-04 — Previsão diária

- **AC-FR04-01 —** **Dado** que a resposta contém previsão diária para o período solicitado,
  **Quando** a consulta termina, **Então** devem ser exibidos cinco resumos diários
  em ordem, para hoje e os quatro dias seguintes.
- **AC-FR04-02 —** **Dado** que a resposta contém dados diários completos, **Quando** um resumo é
  exibido, **Então** ele deve conter data, condição, temperaturas mínima e
  máxima e precipitação acumulada em mm.
- **AC-FR04-03 —** **Dado** que o fuso horário do dispositivo é conhecido, **Quando** os cinco
  dias são determinados, **Então** a data atual e os quatro dias seguintes
  devem ser calculados nesse fuso, mesmo que a cidade consultada esteja em outro
  fuso.
- **AC-FR04-04 —** **Dado** que o fuso horário do dispositivo não pode ser identificado,
  **Quando** os dias da previsão são determinados, **Então** deve ser usado o
  fuso horário de Brasília.
- **AC-FR04-05 —** **Dado** que a resposta não contém previsão para um ou mais dias, **Quando** a
  previsão é exibida, **Então** cada dia sem dados deve ser identificado como
  indisponível e os demais dias devem continuar visíveis.
- **AC-FR04-06 —** **Dado** que os cinco resumos estão visíveis, **Quando** a pessoa consulta cada
  resumo, **Então** cada um deve exibir uma identificação de data distinta.

### FR-05 — Unidade de temperatura

- **AC-FR05-01 —** **Dado** que não há preferência de unidade salva, **Quando** os dados de
  temperatura são exibidos, **Então** os valores devem estar identificados em
  Celsius.
- **AC-FR05-02 —** **Dado** que um dado de referência é `0 °C`, **Quando** a pessoa seleciona
  Fahrenheit, **Então** esse valor deve ser exibido como `32 °F`.
- **AC-FR05-03 —** **Dado** que um dado de referência é `-10 °C`, **Quando** a pessoa seleciona
  Fahrenheit, **Então** esse valor deve ser exibido como `14 °F`.
- **AC-FR05-04 —** **Dado** que há dados atuais e previsão visíveis, **Quando** a pessoa alterna a
  unidade, **Então** todos os valores de temperatura visíveis devem usar a nova
  unidade e apresentar seu identificador.
- **AC-FR05-05 —** **Dado** que a pessoa salvou uma preferência de unidade e o armazenamento
  local está disponível, **Quando** ela abre uma nova visita, **Então** os valores
  de temperatura devem usar a preferência salva.
- **AC-FR05-06 —** **Dado** que a pessoa alterna Celsius/Fahrenheit, **Quando** os outros campos
  meteorológicos são exibidos, **Então** seus valores não relacionados à
  temperatura devem permanecer inalterados.

### FR-06 — Atualização manual

- **AC-FR06-01 —** **Dado** que uma cidade está selecionada, **Quando** a pessoa aciona atualizar,
  **Então** uma nova consulta deve ser iniciada para as coordenadas dessa cidade
  e um indicador de carregamento deve ser exibido.
- **AC-FR06-02 —** **Dado** que uma atualização foi concluída com sucesso, **Quando** o novo
  resultado é exibido, **Então** o rótulo deve indicar “Atualizado há menos de
  um minuto”.
- **AC-FR06-03 —** **Dado** que o relógio de referência avança cinco minutos após uma atualização
  bem-sucedida, **Quando** o rótulo de atualização é recalculado, **Então** ele deve
  indicar cinco minutos decorridos.
- **AC-FR06-04 —** **Dado** que a atualização falha, **Quando** a mensagem de erro é apresentada,
  **Então** o horário da última atualização bem-sucedida não deve ser alterado.

### FR-07 — Cidades recentes

- **AC-FR07-01 —** **Dado** que uma consulta de cidade foi concluída com sucesso, **Quando** a
  lista recente é exibida, **Então** essa cidade deve aparecer no início da lista.
- **AC-FR07-02 —** **Dado** que a busca de uma cidade não retorna dados meteorológicos,
  **Quando** a consulta termina sem sucesso, **Então** essa cidade não deve ser
  adicionada à lista recente.
- **AC-FR07-03 —** **Dado** que seis ou mais cidades foram consultadas com sucesso, **Quando** a
  lista recente é exibida, **Então** ela deve conter exatamente as cinco cidades
  mais recentes, em ordem decrescente de recência.
- **AC-FR07-04 —** **Dado** que uma cidade já consta na lista recente, **Quando** ela é consultada
  novamente, **Então** deve aparecer uma única vez no início da lista.
- **AC-FR07-05 —** **Dado** que uma cidade está na lista recente, **Quando** a pessoa a seleciona,
  **Então** uma consulta para essa cidade deve começar sem nova digitação.
- **AC-FR07-06 —** **Dado** que o armazenamento local está disponível e contém cidades
  recentes, **Quando** a pessoa inicia uma nova visita, **Então** as cidades devem
  ser restauradas. Se o armazenamento estiver indisponível, uma nova busca ainda
  deve poder ser realizada.

### FR-08 — Falhas de consulta

- **AC-FR08-01 —** **Dado** que o dispositivo está sem conexão, **Quando** uma consulta falha,
  **Então** a mensagem “Você está sem internet” deve ser exibida.
- **AC-FR08-02 —** **Dado** que o dispositivo está conectado, mas o serviço não responde ou
  retorna erro, **Quando** a consulta falha, **Então** a mensagem “Não foi possível
  atualizar” deve ser exibida.
- **AC-FR08-03 —** **Dado** que uma solicitação de geocodificação ou meteorologia está em
  andamento, **Quando** não há resposta após 10 segundos, **Então** a solicitação
  deve terminar como timeout e a mensagem de falha deve ser exibida.
- **AC-FR08-04 —** **Dado** que uma mensagem de falha está visível, **Quando** a pessoa observa as
  ações disponíveis, **Então** deve existir uma ação identificável para tentar
  novamente.
- **AC-FR08-05 —** **Dado** que uma consulta para uma cidade falhou, **Quando** a pessoa aciona
  tentar novamente, **Então** uma nova consulta deve ser feita para a mesma cidade
  sem exigir redigitação.

## Non-Functional Requirements

- **NFR-01 — Responsividade:** os fluxos principais devem funcionar em larguras
  de viewport de 320 a 1920 CSS px, sem rolagem horizontal ou conteúdo e
  controles sobrepostos/cortados. A largura mínima deve ser testada a 320 CSS px.
- **NFR-02 — Acessibilidade:** os fluxos principais devem poder ser operados
  por teclado e atender ao WCAG 2.2 nível AA. Controles devem ter nomes e estados
  programaticamente identificáveis; foco, ordem de navegação, contraste e
  mensagens de estado devem atender a esse nível.
- **NFR-03 — Feedback de carregamento:** quando uma consulta é iniciada, a
  aplicação deve indicar imediatamente que está em andamento. Para respostas
  bem-sucedidas, o indicador deve permanecer visível por pelo menos dois
  segundos; erros e timeouts devem ser comunicados assim que detectados.
- **NFR-04 — Resiliência:** uma falha de rede, localização ou serviço externo
  não deve encerrar nem tornar inutilizável a aplicação. A pessoa deve receber
  feedback e poder recorrer à busca manual ou tentar novamente, conforme o
  caso. Cada solicitação de geocodificação ou meteorologia deve encerrar após
  10 segundos sem resposta.
- **NFR-05 — Privacidade e dados:** não deve haver autenticação, analytics,
  persistência de dados em servidor ou envio de dados de uso pela aplicação. A
  preferência de unidade e as cinco cidades consultadas com sucesso devem ser
  armazenadas somente no dispositivo. Coordenadas devem ser enviadas ao
  Open-Meteo somente para atender à consulta e não devem ser persistidas pela
  aplicação. A interface deve informar que a localização será compartilhada com
  o provedor para obter os dados meteorológicos.
- **NFR-06 — Localização da interface:** todos os textos e mensagens da
  interface devem estar em português do Brasil (pt-BR).
- **NFR-07 — Clareza dos dados:** cidade selecionada, unidade de temperatura,
  dia da previsão e tempo desde a última atualização devem ser identificáveis.
- **NFR-08 — Fonte de dados:** as consultas meteorológicas devem usar Open-Meteo,
  sem exigir chave de API da pessoa usuária.
- **NFR-09 — Datas e fuso horário:** datas da previsão devem usar o fuso horário
  do dispositivo; se ele não puder ser identificado, deve-se usar
  America/Sao_Paulo (horário de Brasília).
- **NFR-10 — Unidades e precisão:** exibir velocidade do vento em km/h,
  umidade relativa em %, precipitação em mm e temperaturas com uma casa decimal.
  A conversão de temperatura deve usar a relação entre Celsius e Fahrenheit e
  arredondar o valor exibido para uma casa decimal.
- **NFR-11 — Compatibilidade de navegadores:** suportar as duas versões estáveis
  mais recentes de Chrome, Edge, Firefox e Safari, incluindo Safari em iOS e
  Chrome em Android. Os fluxos de localização, busca e consulta devem ser
  validados em cada navegador e plataforma suportados.

## Edge Cases

- **Busca vazia ou somente com espaços:** não iniciar uma consulta; solicitar que
  a pessoa informe uma cidade.
- **Cidade inexistente ou sem correspondência:** apresentar mensagem de nenhum
  resultado e permitir corrigir a busca.
- **Caracteres Unicode e acentos:** preservar o texto de entrada e mostrar os
  resultados retornados pelo provedor; por exemplo, uma busca por `São Paulo`
  não deve perder os acentos. Entradas vazias seguem a regra de busca vazia.
- **Várias cidades com o mesmo nome:** apresentar contexto geográfico disponível
  para que a pessoa escolha a correta; não selecionar silenciosamente uma opção
  ambígua.
- **Permissão de localização negada ou localização indisponível:** manter a
  busca manual utilizável.
- **Localização imprecisa:** permitir que a pessoa substitua a cidade sugerida
  antes ou depois da consulta.
- **Sem resultados de geocodificação:** informar que a cidade não foi encontrada
  e oferecer nova busca.
- **Falha de rede ou timeout após 10 segundos:** apresentar mensagem simples,
  conservar a cidade selecionada e permitir tentar novamente.
- **Serviço meteorológico indisponível ou resposta inválida:** não apresentar
  dados como atuais; explicar que não foi possível consultar e permitir nova
  tentativa.
- **Resposta parcial:** apresentar campos disponíveis e identificar os campos
  ausentes como indisponíveis, sem inventar valores.
- **Falha ao atualizar:** preservar os dados anteriormente exibidos, identificar
  que a atualização falhou e não avançar o rótulo “atualizado há”.
- **Armazenamento local indisponível ou apagado:** usar Celsius e uma lista de
  cidades recentes vazia; ainda permitir consulta e alternância de unidade na
  sessão atual.
- **Fuso horário do dispositivo diferente do da cidade consultada:** calcular os
  cinco dias no fuso horário do dispositivo, conforme regra do produto.
- **Falha de atualização com dados anteriores disponíveis:** manter os dados
  anteriores visíveis, marcar que a atualização falhou e manter o horário da
  última consulta bem-sucedida.
- **Resposta rápida:** manter o indicador de carregamento pelo período mínimo
  definido, sem impedir que falhas sejam comunicadas de forma clara.

## Assumptions

- A aplicação é um site responsivo, não um aplicativo nativo instalável.
- A localização automática depende de permissão do navegador; a busca manual é
  sempre uma alternativa.
- A cidade só entra na lista recente depois que os dados meteorológicos forem
  recebidos com sucesso; uma nova consulta da mesma cidade a move para o início,
  sem duplicá-la.
- A preferência de unidade e a lista de cinco cidades são armazenadas apenas
  localmente, não são sincronizadas nem enviadas ao servidor e podem ser apagadas
  pelos mecanismos de armazenamento do navegador.
- O rótulo relativo de atualização se refere ao momento em que os dados foram
  recebidos com sucesso, não ao momento em que cada observação meteorológica foi
  medida pelo provedor.
- O fuso horário para determinar os dias é o do dispositivo; o fallback é
  America/Sao_Paulo (horário de Brasília).
- O limite de espera de 10 segundos para cada solicitação é adequado ao produto;
  após esse período, o usuário recebe uma falha recuperável.

## Risks

| Categoria | Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- | --- |
| Técnico | Indisponibilidade, lentidão ou mudança de resposta do Open-Meteo | Média | Alto | Tratar indisponibilidade, validar os dados recebidos, exibir erro simples e permitir nova tentativa. |
| Técnico | Resposta parcial produz campos ausentes ou valores incorretos | Média | Alto | Validar campos antes de exibir; indicar valores indisponíveis em vez de inferi-los. |
| Técnico | Falta de conexão ou serviço sem resposta impede busca ou atualização | Alta | Médio | Encerrar a solicitação após 10 segundos, preservar a cidade selecionada, explicar a falha e oferecer nova tentativa. |
| Técnico | Localização negada ou imprecisa aponta para uma cidade errada | Média | Alto | Não bloquear busca manual e permitir substituir a localização sugerida. |
| Produto | Cidades homônimas causam consulta do lugar errado | Média | Alto | Mostrar estado, país ou outro contexto disponível antes da seleção. |
| Produto | Espera mínima de dois segundos é percebida como lentidão | Média | Médio | Aplicar o mínimo somente a respostas bem-sucedidas, comunicar o carregamento imediatamente e expor erros sem atraso. |
| Produto e privacidade | Compartilhamento de localização com o provedor surpreende a pessoa | Média | Alto | Informar antes da primeira consulta que as coordenadas serão enviadas ao Open-Meteo e não serão armazenadas pela aplicação. |
| Produto | Acessibilidade ou responsividade insuficientes excluem parte do público | Média | Alto | Validar teclado, leitores de tela, contraste e fluxos em diferentes tamanhos de tela. |
| Produto | O fuso horário usado pode divergir da cidade pesquisada | Média | Médio | Tornar datas explícitas e confirmar se previsões devem seguir o fuso do dispositivo ou o da cidade. |

## Out of Scope

- Autenticação, cadastro, perfil de usuário ou sincronização entre dispositivos.
- Persistência de dados meteorológicos ou de usuário em servidor.
- Avaliações por estrelas.
- Previsões para além de cinco dias, dados meteorológicos históricos ou
  climatológicos e previsão horária; o escopo atual é um resumo diário de hoje e
  dos quatro dias seguintes.
- Mapas meteorológicos, radar, imagens de satélite, qualidade do ar, índice UV,
  pólen ou outros indicadores além dos campos definidos nos requisitos.
- Alertas meteorológicos, notificações push ou por e-mail, favoritos ou mais de
  cinco cidades recentes.
- Unidades configuráveis para vento, umidade ou precipitação; somente a unidade
  de temperatura pode ser alternada.
- Aplicativo nativo para iOS ou Android.
- Interface em idiomas diferentes de pt-BR.
- Garantia de uso offline ou apresentação de previsões meteorológicas em cache.
- Coleta de analytics ou dados pessoais pela aplicação.

## Open Questions

As decisões de produto e os comportamentos verificáveis estão definidos. Antes
do lançamento público, é necessário concluir estas validações externas:

1. **Termos e limites do Open-Meteo:** confirmar que as condições de uso,
  atribuição exigida, cobertura geográfica e limites de solicitação são
  compatíveis com o lançamento planejado.
2. **Aviso de privacidade:** validar o texto que informa o envio de coordenadas
  ao Open-Meteo e o armazenamento local da unidade e das cinco cidades recentes.