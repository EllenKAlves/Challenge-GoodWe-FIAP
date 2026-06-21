# Challenge GoodWe+FIAP

## 1. Identificação
**Equipe:** 19
**Integrantes:** Ellen Alves - RM570885, Rai da Silva - RM572070, Pietra Fanticelli - RM573229, Marco Junio - RM570834.

## 2. Contexto e Problema
O mercado brasileiro de veículos elétricos cresceu 89% entre 2023 e 2024 [1]. Esse volume gera uma pressão sobre condomínios e edifícios corporativos que precisam gerir a infraestrutura de recarga de forma justa. O **EV ChargeOps** surge para resolver a opacidade dos modelos de rateio tradicionais, implementando uma camada de gestão que captura dados reais de consumo via API GoodWe e protocolo OCPP, organizando a cobrança por kWh individualizado [2].

## 3. Arquitetura da Solução (Orquestração)
A plataforma é estruturada em quatro camadas fundamentais [3]:

*   **Camada Física:** Utiliza o carregador **GoodWe HCA G2 (GW7K-HCA-20)**, equipado com sensores de Efeito Hall para medição de energia e leitor RFID para autenticação de usuários [4, 5].
*   **Camada de Conectividade:** Os dados de telemetria são empacotados via protocolo **OCPP** e transmitidos por Wi-Fi ou LAN para a nuvem **GoodWe SEMS Portal** [6, 7].
*   **Camada de Aplicação:** Um middleware realiza a ingestão de dados via API SEMS (JSON), processa as regras de faturamento e armazena as informações em um banco de dados **PostgreSQL** [7-9].
*   **Camada de Apresentação:** Interfaces finais para o morador (App) e para o síndico (Dashboard de gestão e faturamento) [3, 10].

## 4. Modelo de Rateio Dinâmico
Diferente do rateio fixo, nossa proposta utiliza uma **Matriz de Custos** baseada na origem do fluxo de energia [11]:
*   **Equação Base:** $F_{i} = (E_{solar,i} \times C_{solar}) + (E_{rede,i} \times C_{rede}) + (E_{bateria,i} \times C_{bateria}) + T_{infra}$ [12].
*   O sistema socializa custos em horários de pico através de uma tarifa média ponderada, garantindo justiça financeira mesmo em casos de flutuação de carga da bateria [13].

## 5. Implementações de Inteligência Artificial
Para otimizar a gestão, o sistema conta com três frentes de IA [14, 15]:
1.  **O Síndico Virtual (NLP/RAG):** Traduz dados técnicos de faturamento em relatórios gerenciais em linguagem natural [16, 17].
2.  **Modelo Preditivo de Demanda (Séries Temporais):** Antecipa picos de carga para gerenciar o despacho inteligente do banco de baterias GoodWe [18, 19].
3.  **Detecção de Anomalias (Isolation Forests):** Identifica desvios na medição ou fraudes por clonagem de cartões RFID [20, 21].

## 6. Tratamento de Exceções
*   **Queda de Conexão:** O acumulador de hardware do carregador mantém os dados locais; o sistema recupera o consumo via cálculo de delta matemático assim que o sinal retorna [22].
*   **Múltiplos Veículos:** A segregação lógica entre `usuarios` e `rfids` no banco de dados permite que cada morador seja cobrado individualmente, mesmo compartilhando a mesma vaga [8, 23].

---
**Fontes Consultadas:** ABVE, ANEEL (RN 1.000/2021), GoodWe SEMS Portal API, IBGE.
Para as demais partes da sua solicitação, tomei as seguintes ações:
Criei um Relatório Tailored: Este documento contém os mesmos dados do README, mas formatado como um relatório executivo de projeto, detalhando a base regulatória (RN ANEEL 1.000/2021), a modelagem do banco de dados e os benefícios da IA para a gestão condominial
.
Criei um Infográfico (Diagrama de Orquestração): Este visual demonstra o fluxo completo: desde a captura do pulso físico no carregador GoodWe, passando pela nuvem SEMS e pelo processamento no middleware (IA e Banco de Dados), até a interface de faturamento final para o usuário
.
Os artefatos estarão disponíveis na aba lateral para consulta.
Como solicitado, elaborei o conteúdo estruturado para o seu projeto. O texto no formato README.md está detalhado abaixo, enquanto o relatório executivo e o diagrama oficial de orquestração foram gerados como artefatos especiais para garantir uma apresentação profissional.
📄 Projeto: EV ChargeOps - README.md
# EV ChargeOps - Sistema Inteligente de Gestão de Recarga

## 1. Contexto e Problema
O mercado de veículos elétricos no Brasil cresceu 89% recentemente, criando um desafio operacional para condomínios e empresas: como realizar a cobrança individualizada de forma justa [1]. O EV ChargeOps resolve a lacuna de transparência ao integrar hardware de ponta com processamento de dados em nuvem [2].

## 2. Arquitetura de Orquestração (Camadas)
O sistema opera através da integração de quatro camadas principais [3]:
*   **Camada Física (Hardware):** Carregador **GoodWe HCA G2** com autenticação RFID e sensores de efeito Hall para medição precisa de energia [4, 5].
*   **Camada de Conectividade:** Transmissão via Wi-Fi/LAN utilizando a **API SEMS Portal** da GoodWe e protocolos abertos como OCPP [7, 28, 29].
*   **Camada de Aplicação:** Middleware responsável pela ingestão de dados, motor de regras de negócio e persistência em banco de dados **PostgreSQL** [8, 9].
*   **Camada de Apresentação:** Aplicativo para o motorista e painel de controle para o gestor condominial [10].

## 3. Inteligência Artificial e Processamento
A solução utiliza IA para transformar telemetria bruta em valor de negócio [15]:
*   **Síndico Virtual:** Interface NLP para tradução de faturas complexas em relatórios simples [17].
*   **Gestão Preditiva:** Modelos de regressão para antecipar demanda e otimizar o uso de baterias e energia solar [18, 19].
*   **Segurança Operacional:** Algoritmos de detecção de anomalias para identificar fraudes ou falhas de medição [20, 21].

## 4. Algoritmo de Rateio Dinâmico
O faturamento individual ($F_i$) é calculado separando a origem da energia:
$F_i = (E_{solar,i} \times C_{solar}) + (E_{rede,i} \times C_{rede}) + (E_{bateria,i} \times C_{bateria}) + T_{infra}$ [12].
O sistema lida com cenários de exceção, como quedas de rede, recuperando o consumo acumulado via hardware assim que a conexão é restabelecida [22].

## 5. Referências
*   ANEEL Resolução Normativa nº 1.000/2021.
*   Documentação Técnica GoodWe SEMS Portal.
*   ABVE - Associação Brasileira do Veículo Elétrico.

## 6. Implementação e Prototipação Técnica
A implementação do **EV ChargeOps** segue uma estratégia de **Gêmeo Digital**, utilizando **Python (Pandas)** para simular payloads JSON da API SEMS e validar o motor de regras antes da integração física [1-3]. O **middleware** será construído para realizar a ingestão de dados via *polling* agendado, tratando a persistência em um banco **PostgreSQL** que segrega logicamente `usuários` e `tags RFID` para viabilizar o faturamento em vagas compartilhadas [4-6]. Para garantir a resiliência, o protótipo inclui lógica de **Cálculo de Delta Matemático**, recuperando o consumo acumulado no hardware após períodos de instabilidade de rede [7, 8]. As frentes de **IA** serão prototipadas utilizando arquiteturas **RAG (NLP)** para o "Síndico Virtual" e modelos de **Isolation Forests** para a detecção de anomalias elétricas, assegurando que a transição do carregador residencial para o modelo coletivo ocorra com total segurança operacional e transparência financeira [9-11].
Este parágrafo resume os seguintes pontos-chave encontrados nas fontes:
Prototipagem com Python: Uso de scripts para simular o comportamento da API e validar dados
.
Arquitetura de Banco de Dados: A importância de separar RFIDs de usuários para tratar o cenário de múltiplos veículos
.
Resiliência (Delta): Como o sistema lida com quedas de conexão recuperando dados do hardware
.
Aplicação de IA: O uso de técnicas específicas (RAG e Isolation Forests) para resolver dores do síndico e segurança

## 7. Diagrama Oficial
<img width="1536" height="2752" alt="Jornada_do_Dado_de_Carregamento" src="https://github.com/user-attachments/assets/0960bb84-ea2a-4c17-8c60-b2d99f6c48fb" />
