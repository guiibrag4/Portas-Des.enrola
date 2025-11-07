# Relatório Técnico e Recomendações para o Desenvolvimento de Aplicativo Móvel de Cálculo de Portas de Enrolar

**Autor:** Manus AI
**Data:** 07 de Novembro de 2025
**Status:** Revisado (V1.2)

## 1. Introdução

Este relatório fornece uma análise técnica e recomendações para o desenvolvimento de um aplicativo móvel (App) focado no cálculo de valor de portas de enrolar, conforme os requisitos apresentados. O foco principal é garantir a funcionalidade **offline-first**, a **hospedagem gratuita** do backend e a **distribuição simplificada** fora das lojas de aplicativos tradicionais (App Store e Google Play).

## 2. Requisitos e Fluxo de Usuário

O aplicativo será desenvolvido para uma empresa de portas de enrolar, focado em cálculo de valor da porta.

### 2.1. Requisitos Funcionais

| ID | Requisito | Descrição | Prioridade |
| :--- | :--- | :--- | :--- |
| RF01 | **Cálculo Offline** | O cálculo do valor da porta deve ser realizado integralmente no dispositivo, sem a necessidade de conexão com a internet. | Alta |
| RF02 | **Configuração de Peças** | O usuário deve poder inserir e editar o valor de cada peça necessária para o cálculo. | Alta |
| RF03 | **Persistência de Configurações** | As peças e seus valores devem ser armazenados na parte de configurações para que o usuário possa editá-las conforme mudam de valor com o tempo. | Alta |
| RF04 | **Inserção de Dados** | O usuário deve inserir as medidas da porta (largura e altura), valor com porcentagem bruta e valor com porcentagem de desconto. | Alta |
| RF09 | **Dados do Cliente** | O usuário deve inserir o nome do cliente, local e número de telefone no orçamento. | Alta |
| RF10 | **Especificações Técnicas** | O usuário deve selecionar o peso do motor (200kg a 1000kg), o tipo de testeira (1.1, 1.3, 1.4, 1.5) e o tipo de eixo (4 1/2", 5 1/2", 6 1/2", 7 1/2"). | Alta |
| RF11 | **Geração de Orçamento** | O cálculo final deve ser salvo junto com os dados do cliente e as especificações técnicas. | Alta |
| RF12 | **Compartilhamento (Word/PDF)** | O usuário deve ter a opção de compartilhar o orçamento final em formato Word ou PDF. | Baixa |
| RF05 | **Cálculo Base** | O cálculo deve ser feito com base nos valores dos materiais inseridos e nas medidas da porta. | Alta |
| RF06 | **Itens Adicionais** | O usuário deve ter a opção de adicionar itens adicionais com valor fixo ao valor total: nobreak, portinhola de emergência, sensor a laser, pintura eletrostática + valor respectivo e caixa de proteção do motor. | Alta |
| RF07 | **Presets de Portas** | (Futuro) O usuário poderá selecionar pre-sets de portas com medidas comuns para cálculo imediato (Ex: 250x250; 250x300; 300x200). | Média |
| RF08 | **Categorização de Presets** | (Futuro) Os pre-sets podem ser classificados como grupos de pre-sets (Ex: portas de 200 a 300 de largura, depois de 300 a 400, etc.). | Média |

### 2.2. Fluxo de Usuário (Cálculo)

1.  **Acesso à Configuração (Offline):** O usuário acessa a tela de configurações para verificar/editar os valores das peças.
2.  **Edição de Valores (Offline):** O usuário atualiza o valor de uma ou mais peças e salva localmente.
3.  **Acesso à Tela de Cálculo (Offline):** O usuário acessa a tela principal de cálculo.
4.  **Inserção de Medidas (Offline):** O usuário insere as medidas da porta (largura, altura).
5.  **Seleção de Adicionais (Offline):** O usuário seleciona os itens adicionais (nobreak, pintura, etc.).
6.  **Cálculo (Offline):** O app executa o cálculo utilizando os valores das peças armazenados localmente.
7.  **Visualização do Resultado (Offline):** O resultado final (valor bruto, desconto, valor final) é exibido.
8.  **Sincronização (Online, Futuro):** Quando online, o app sincroniza os valores de peças atualizados e, futuramente, os presets com o backend.

## 3. Análise de Requisitos Técnicos

O aplicativo possui requisitos técnicos bem definidos, que influenciam diretamente a escolha da arquitetura e das tecnologias:

| Requisito | Implicação Técnica | Prioridade |
| :--- | :--- | :--- |
| **Cálculo Offline** | Necessidade de um banco de dados local robusto e eficiente. | Alta |
| **Backend Gratuito e Sem Callback** | Favorece soluções BaaS (Backend as a Service) com plano gratuito generoso, acessível via APIs REST/GraphQL, que não exigem *webhooks* (callback). | Alta |
| **Distribuição via APK/Sideloading** | Simplifica o processo de lançamento (não requer aprovação de loja), mas exige um método de distribuição seguro e fácil (ex: link direto, QR Code). | Média |
| **Geração de Documentos (Word/PDF)** | Necessidade de uma biblioteca de geração de documentos (ex: `react-native-html-to-pdf` ou similar) no frontend. | Baixa (MVP) |

### 3.1. Esclarecimento sobre "Callback"

O termo **"callback"** em desenvolvimento de software possui múltiplos significados. No contexto de serviços de backend, ele é frequentemente usado para descrever um **webhook**, onde o servidor (backend) faz uma requisição HTTP de volta para um endereço específico (o callback URL) do cliente ou de outro servidor após a conclusão de um evento (ex: pagamento processado, arquivo carregado).

**Sua preocupação:** O que você deseja evitar é a necessidade de fazer uma **requisição síncrona** para o servidor (online) e ter que esperar o retorno para que o cálculo seja realizado.

**Conclusão:** Sua concepção está correta para o seu objetivo de **funcionalidade offline**. A arquitetura **offline-first** recomendada garante que o cálculo (RF01) e a edição de valores (RF02) sejam feitos **localmente** no banco de dados do dispositivo. O backend (Supabase) será usado apenas para **sincronização** de dados (backups, presets) quando o dispositivo estiver online, utilizando requisições **assíncronas** (APIs REST/GraphQL) que não impedem o uso do app.

## 4. Escolha da Tecnologia Móvel: React Native

**Recomendação:** **React Native** (com TypeScript) é a tecnologia mais recomendada.

| Característica | React Native (JavaScript/TypeScript) | Justificativa para o Projeto |
| :--- | :--- | :--- |
| **Ecossistema Offline** | Bibliotecas maduras como `WatermelonDB` e `SQLite`. | Ecossistema vasto e maduro para soluções de persistência de dados offline, crucial para o RF01. |
| **Comunidade** | Maior comunidade e ecossistema JavaScript. | Facilita a busca por soluções e a manutenção futura. |

## 5. Arquitetura Offline-First e Persistência de Dados

### 5.1. Banco de Dados Local

**Recomendação:** **WatermelonDB** (ou SQLite puro) é a opção mais segura e robusta para o armazenamento local dos valores das peças e dos resultados dos cálculos. Ele é otimizado para performance em React Native e ideal para o modo offline-first.

### 5.2. Backend (Sincronização e Presets)

**Recomendação:** **Supabase** (Plano Gratuito) é a opção mais equilibrada. Ele oferece um BaaS com PostgreSQL (relacional, ideal para dados estruturados como peças e presets) e um plano gratuito robusto, acessível via APIs REST/GraphQL.

## 6. Segurança dos Dados (Valores das Peças)

Você solicitou que a segurança só fosse adicionada se os dados fossem ficar expostos.

**Análise de Exposição:**
1.  **Backend (Supabase):** Os dados não ficarão expostos publicamente. O acesso ao banco de dados é feito por meio de chaves de API e políticas de segurança (Row Level Security - RLS) que garantem que apenas o aplicativo autenticado possa ler/escrever.
2.  **Dispositivo Local (SQLite/WatermelonDB):** Os dados (valores das peças) são armazenados no banco de dados local do dispositivo. Se o dispositivo for perdido, roubado ou se um usuário mal-intencionado tiver acesso físico, os dados podem ser extraídos se não estiverem criptografados.

**Recomendação Revisada:** A segurança é **necessária** no dispositivo local.

*   **Criptografia Local:** É altamente recomendável usar uma biblioteca de criptografia para o banco de dados local (ex: `react-native-sqlite-storage` com suporte a SQLCipher, ou o próprio WatermelonDB com criptografia de dados sensíveis). Isso protege os valores das peças (propriedade intelectual da empresa) contra acesso não autorizado em caso de perda ou roubo do dispositivo.
*   **Segurança no Backend:** Implementar as políticas de **Row Level Security (RLS)** no Supabase para garantir que cada usuário (ou o aplicativo) só possa acessar os dados de sua própria conta.

## 7. Processos Críticos e Pontos de Atenção

| Processo Crítico | Descrição | Impacto |
| :--- | :--- | :--- |
| **Mecanismo de Sincronização** | Como o app irá lidar com a fusão de dados locais e remotos (ex: o usuário edita um valor offline e o valor é editado no backend por outro usuário). | **Crítico** para a integridade dos dados. |
| **Segurança dos Dados** | Criptografia do banco de dados local para proteger os valores das peças (propriedade intelectual). | **Alto** (Propriedade intelectual). |
| **Testes de Offline** | Testar rigorosamente o app em modo avião para garantir que a lógica de cálculo e a edição de valores funcionem sem falhas. | **Crítico** para o requisito principal. |
| **Atualizações do App** | Como notificar e forçar a atualização do APK nos dispositivos dos usuários, já que não haverá a App Store para gerenciar isso. | **Alto** (Manutenção e correções de bugs). |
| **Geração de Documentos** | Garantir que a geração do orçamento em Word/PDF seja precisa e formatada corretamente, utilizando os dados salvos do cliente e do cálculo. | **Médio** (Para a usabilidade final). |

## 8. Resumo das Recomendações

| Aspecto | Recomendação | Justificativa |
| :--- | :--- | :--- |
| **Linguagem/Framework** | **React Native** (com TypeScript) | Ecossistema maduro, vasta comunidade, excelente suporte a soluções offline. |
| **Banco de Dados Local** | **WatermelonDB** (ou SQLite puro) **com Criptografia** | Oferece persistência robusta e protege os dados sensíveis da empresa no dispositivo. |
| **Backend** | **Supabase** (Plano Gratuito) | BaaS com PostgreSQL, plano gratuito generoso, acessível via API (sem callback complexo), ideal para armazenar presets e backups. |
| **Distribuição** | **APK Sideloading** (Android) | Solução gratuita e prática para distribuição interna, atendendo ao requisito de não usar lojas. |

## Referências

[1] Creating Offline-First Apps with React Native and Flutter. *LinkedIn*. [URL de exemplo, pois não abri a URL]
[2] Top 5 Free Mobile Backend Services: Features and Benefits. *Back4app Blog*. [URL de exemplo, pois não abri a URL]
[3] Distribute proprietary in-house apps to Apple devices. *Apple Support*. [URL de exemplo, pois não abri a URL]
