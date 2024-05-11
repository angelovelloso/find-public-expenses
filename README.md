# Projeto para Identificar, sumarizar e classificar futuras despesas públicas



## Objetivo

Como servidor do Governo do Estado de Rondônia e trabalhando diretamente na extração e organização dos dados relacionados às despesas, conseguimos ter uma visão bastante profunda e detalhada sobre os gastos públicos comprometidos ou incorridos utilizando tecnologias e metodologias de Engenharia de Dados.

Isso se dá porque há uma adesão total ao Sistema Financeiro adotado na gestão do ciclo orçamentário, que envolve a definição dos limites de crédito orçamentário e passa pelo Empenho, Liquidação e Pagamento.

O principal gargalo está em identificar ou gerenciar o ciclo de licitação e contratação, uma vez que esses processos acontecem em diversas unidades descentralizadamente e não são gerenciadas ou imputadas em nenhum sistema de informação que dê suporte ao processo e sirva de fonte da informação.

Mapear iniciativas de aquisições públicas antes que se tornem compromissos na fase de Empenho interessa à administração especialmente em um contexto de frustração de receitas e necessidade de limitação de empenho e movimentação financeira.

Este projeto olha para essa necessidade e surge a partir do desafio promovido pela Alura na Imersão Inteligência Artificial 2ª Edição e propõe o uso da poderosa tecnologia da API do **Gemini**, a IA Generativa do Google.

Neste contexto **três objetivos** serão perseguidos:
* Identificar e sistematizar as novas iniciativas de aquisição pública;
* Sumarizar o que está a Administração Pública pretende adquirir;
* Classificar de forma útil as futuras despesas.

## Abordagem

Para atingir os objetivos delineados, a abordagem proposta passa por:
1. Monitorar a publicação do Diário Oficial do Estado de Rondônia;
2. A cada nova edição, utilizar a API do **Gemini** para analisar o documento e **identificar** novas iniciativas de aquisição pública que estejam sendo divulgadas, **sistematizando** e **estruturando** a informação de forma útil;
3. Considerando que estes documentos (Editais, Atas, Termos de Referência) apresentam um grande volume de informação e detalhes, a API do **Gemini** será responsável por também **sumarizar** os documentos e **extrair** dados úteis para descrever de forma objetivo o que a Administração Pública pretende adquirir;
4. Ao final, a partir dos dados sumarizados obtidos e, visando direcionar um suposto revisor de despesas, a expectativa é que cada objeto seja **classificado** de forma pré-estabelecida e para o qual serão adotadas como referência as categorias previstas no § 1° do Art. 57 da Lei de Diretrizes Orçamentária do Estado de Rondônia para o exercício de 2024.
5. Essas categorias dizem respeito às despesas que devem ser observadas prioritariamente no processo de limitação de despesas.

Como referência, segue o texto integral do artigo e as categorias enumeradas:

> `Art. 57. Se verificado, ao final de um bimestre, que a realização da receita poderá não comportar o cumprimento das metas de resultado primário ou nominal estabelecidas no Anexo de Metas Fiscais, na forma do artigo 9° da Lei Complementar Federal n° 101, de 2000, os Poderes, o Ministério Público, a Defensoria Pública do Estado e o Tribunal de Contas do Estado promoverão, por ato próprio e nos montantes necessários, nos 30 (trinta) dias subsequentes, limitação de empenho e movimentação financeira, de forma proporcional à queda de arrecadação estimada nas fontes de recursos específicas que`
> `suportam as dotações orçamentárias do respectivo Poder ou Órgão.`
> 
> `§ 1° O Poder Executivo de forma proporcional às suas dotações adotará medidas necessárias para o cumprimento do caput, observadas as respectivas fontes de recursos, em especial, nas seguintes despesas:`
> 
> `I - contrapartida para projetos ou atividades vinculadas a recursos oriundos de fontes extraordinárias, como transferências voluntárias, operações de crédito e alienação de ativos, desde que ainda não comprometidos;`
> `II - obras em geral, cuja fase ou etapa ainda não esteja iniciada;`
> `III - aquisição de combustíveis e derivados, destinada à frota de veículos, exceto dos setores de saúde, educação e segurança pública;`
> `IV - dotação para material de consumo e outros serviços de terceiros para as diversas atividades;`
> `V - diárias de viagem;`
> `VI - festividades, homenagens, recepções e demais eventos da mesma natureza;`
> `VII - despesas com publicidade institucional; e`
> `VIII - horas-extras.`

## Considerações

O projeto foi desenvolvido **parcialmente**, carecendo de novas rodadas de incremento e melhoria.

#### Escopo realizado
Até o momento foi possível validar as capacidades da API do **Gemini** para performar de forma excepcional as tarefas 2 e 3 descritas na abordagem, com os seguintes pontos de atenção:

##### Time out
A experiência com a API de forma programática exigiu que os dados fossem particionados (*chuncks*) e várias requisições fossem realizadas em sequência para cobrir o documento completo.

Na interface do AI Studio isso não era necessário, sendo possível extrair o conteúdo completo do documento (aprox. 500 mil tokens) em um tempo relativamente baixo.

Nos testes e desenvolvimento, também houve o atingimento do limite de quota.

Imagino que isso tenha a ver com a disponibilização de recursos e políticas do próprio Google, mas precisará ser investigado ou tratado para manter a solução estável.

##### Formato de saída dos dados
Com o particionamento um desafio é o fato de que a IA generativa apresentar algumas variações no formato de saída dos dados entre as requisições.

Os itens, por exemplo, em alguns blocos são formatados como uma lista (*array*) simples e em outros momentos como chave-valor (*struct*). 

Também foram notadas variação em algumas chaves do JSON. A coluna Fornecedor em alguns momentos era extraída como "Empresas". Isso melhorou com a adição de instruções "claras" sobre as chaves que deveriam ser usadas pelo modelo.

Em todo caso, novos testes e alterações precisam ser feitas para alcançar um modelo mais padronizado ou possível de ser tratado.

#### Escopo a realizar (incrementos)
Verifica a capacidade de a API do Gemini performar nas atividades propostas de forma satisfatória, a solução precisará ser incrementada para os demais itens da abordagem sugerida (1 e 4).

##### Monitoramento e download de novas edições do DOE
A sugestão é que através de  *web scraping* seja verificada em horário pré-estabelecido a existência e publicação de novos diários e iniciado o processo de tratamento, preferencialmente com integração ao orquestrador utilizado.

##### Classificação e ordenação das despesas
Realizar novas requisições para os dados formatados buscando que as despesas sejam rotuladas conforme descrito no item 4.
