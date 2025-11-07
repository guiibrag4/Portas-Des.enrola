# Plano de Design UI/UX e Arquitetura Frontend para o App de Cálculo de Portas (V1.1)

**Autor:** Manus AI
**Data:** 07 de Novembro de 2025

## 1. Conceito de Design UI/UX

O objetivo é criar uma interface **limpa, funcional e eficiente**, que priorize a usabilidade para um aplicativo de trabalho. O design deve ser moderno, mas sem exageros, focando na clareza dos dados e na facilidade de inserção de informações.

### 1.1. Estilo Visual e Paleta de Cores

*   **Estilo:** **Minimalista e Profissional**. Uso de cartões (cards) para agrupar informações e sombras sutis para dar profundidade e hierarquia.
*   **Tipografia:** Fontes *sans-serif* de fácil leitura (ex: Roboto, Inter), com bom contraste.
*   **Paleta de Cores:**
    *   **Primária:** Um tom de azul ou verde escuro (associado à confiança e estabilidade).
    *   **Secundária:** Um tom de laranja ou amarelo (para botões de ação e destaque).
    *   **Fundo:** Branco ou cinza claro (para clareza).
    *   **Ações:** Vermelho para ações destrutivas (ex: "Excluir Cálculo").

### 1.2. Estrutura de Navegação (Nav-Bottom)

A navegação inferior (Nav-Bottom) é ideal para acesso rápido às funcionalidades principais. Serão utilizadas 4 abas principais:

| Ícone Sugerido | Nome da Aba | Função Principal | Justificativa |
| :--- | :--- | :--- | :--- |
| 🧮 | **Cálculo** | Tela principal para inserção de dados e realização do cálculo. | A função mais importante do app. |
| ⚙️ | **Configurações** | Edição dos valores das peças e outros parâmetros. | Acesso rápido para atualização de preços (requisito RF03). |
| 🚪 | **Presets** | (Futuro) Acesso e gestão de medidas de portas pré-definidas. | Funcionalidade de conveniência futura (RF07). |
| 📜 | **Histórico** | Visualização e gestão dos cálculos realizados e salvos. | Essencial para referência e geração de orçamentos. |

## 2. Fluxo de Primeira Utilização (FTUE) e Onboarding

O maior desafio no primeiro uso é o requisito de que **o usuário deve inserir inicialmente o valor de cada peça necessária para o cálculo**. O aplicativo não pode funcionar sem esses dados.

### 2.1. Fluxo de Onboarding Ideal

O **Fluxo de Primeira Utilização (FTUE)** é crucial para garantir que o aplicativo funcione corretamente desde o início, pois o cálculo depende da inserção inicial dos valores das peças pelo usuário. O fluxo ideal deve ser **forçado** e direto, garantindo que o requisito fundamental (RF02) seja atendido antes de liberar a funcionalidade principal.

O caminho crítico de onboarding é estruturado nas seguintes etapas:

| Passo | Tela | Ação do Usuário | Objetivo |
| :--- | :--- | :--- | :--- |
| **1. Boas-Vindas** | *Splash Screen* / Onboarding | Botão "Começar" | Apresentar o propósito do app (cálculo offline). |
| **2. Configuração Inicial** | **Configurações (Forçada)** | Inserir valores iniciais das peças. | Garantir que o app tenha os dados mínimos para o cálculo (requisito RF02). |
| **3. Confirmação** | *Modal* de Sucesso | Botão "Ir para o Cálculo" | Confirmar a conclusão da configuração e direcionar para a função principal. |
| **4. Uso Principal** | **Cálculo** | Inserir medidas e realizar o primeiro cálculo. | Iniciar o uso prático do aplicativo. |

É fundamental que a tela de **Cálculo** permaneça bloqueada ou exiba uma mensagem clara de orientação até que a configuração inicial dos valores das peças seja concluída, forçando o usuário a seguir o fluxo de dados necessário para a funcionalidade offline.

### 2.2. Onboarding de Dados Iniciais

A tela de **Configurações** no primeiro acesso deve apresentar um formulário claro com todas as peças necessárias para o cálculo, com campos de entrada para o valor unitário. Uma lista pré-definida de peças essenciais pode ser carregada para facilitar o processo, exigindo apenas a inserção dos valores. Após a conclusão, um *flag* de estado no banco de dados local será alterado para desbloquear o restante do aplicativo.

## 3. Detalhamento das Telas Principais

### 3.1. Tela: Cálculo (🧮)

A tela de **Cálculo** é o coração do aplicativo, projetada como um formulário de rolagem vertical para facilitar a inserção de dados.

| Seção | Campos/Elementos | Requisito Relacionado |
| :--- | :--- | :--- |
| **Dados do Cliente** | Nome do Cliente, Local, Número de Telefone. | RF09 |
| **Especificações Técnicas** | Seleção de Peso do Motor (200kg - 1000kg), Tipo de Testeira (1.1, 1.3, 1.4, 1.5), Tipo de Eixo (4 1/2", 5 1/2", 6 1/2", 7 1/2"). | RF10 |
| **Medidas da Porta** | Largura (m), Altura (m) | RF04 |
| **Itens Adicionais** | Lista de *Toggles* (Ligar/Desligar) para Nobreak, Portinhola, Sensor a Laser, Caixa de Proteção do Motor. Campo de texto/seleção para **Pintura Eletrostática** (com valor respectivo). | RF06 |
| **Margens e Descontos** | Porcentagem Bruta (Margem de Lucro), Porcentagem de Desconto (para o cliente) | RF04 |
| **Resultado** | Botão principal **"CALCULAR"**. Área de exibição do resultado: Valor dos Materiais, Valor Bruto, Desconto Aplicado, **VALOR FINAL**. Botão secundário: "Salvar Cálculo" (RF11). Botão "Compartilhar Orçamento" (RF12 - Baixa Prioridade). | RF05, RF11, RF12 |

### 3.2. Tela: Configurações (⚙️)

Esta tela gerencia os dados essenciais para o cálculo e as configurações do aplicativo.

| Seção | Elementos | Requisito Relacionado |
| :--- | :--- | :--- |
| **Valores das Peças** | Lista de todas as peças necessárias (ex: m² de lâmina, metro linear de guia, motor, etc.). Cada item com: Nome da Peça, Unidade de Medida e Campo de Valor (R$). Botão: "Adicionar Nova Peça". | RF02, RF03 |
| **Configurações do App** | Opção: Sincronizar com Backend (Botão "Sincronizar Agora"). Opção: Backup/Restauração Local. Opção: Ajuda/Sobre. | Funcionalidades de Manutenção |

### 3.3. Tela: Histórico (📜)

A tela de **Histórico** permite o acompanhamento e a gestão dos cálculos salvos.

| Elemento | Detalhe |
| :--- | :--- |
| **Lista de Cálculos** | Cada item exibe: Data/Hora, Medidas da Porta (ex: 3.00x2.50m), e o Valor Final. |
| **Ações** | Ação de *Swipe* (deslizar) ou Botão de Ação para: "Visualizar Detalhes" e "Excluir". |
| **Funcionalidades** | Filtros e Busca (por data, por cliente - se for adicionado futuramente). |

### 3.4. Tela: Presets (🚪) (Futuro)

Esta tela de acesso rápido a cálculos de medidas comuns será implementada em uma fase futura.

| Elemento | Detalhe | Requisito Relacionado |
| :--- | :--- | :--- |
| **Agrupamento** | Títulos de grupo (ex: "Portas de 200 a 300 de Largura") para categorização. | RF08 |
| **Itens** | Cada item do preset (ex: 250x250) deve ser clicável e levar o usuário para a tela de **Cálculo** com os campos de Largura e Altura pré-preenchidos. | RF07 |
| **Ação** | Botão: "Criar Novo Preset". | RF07 |

## 4. Plano de Construção do Aplicativo Completo (Arquitetura Frontend)

A arquitetura do Frontend deve ser modular, reativa e otimizada para o modo offline-first.

### 4.1. Estrutura de Código (React Native)

A estrutura de diretórios proposta visa a separação de responsabilidades e a modularidade:

| Diretório | Responsabilidade |
| :--- | :--- |
| **`src/assets/`** | Imagens, ícones, fontes. |
| **`src/components/`** | Componentes de UI reutilizáveis (Botões, Inputs, Cards). |
| **`src/screens/`** | Componentes de tela (Cálculo, Configurações, Histórico, Presets). |
| **`src/navigation/`** | Configuração do Nav-Bottom e do Stack Navigator. |
| **`src/data/`** | Camada de persistência e sincronização. |
| **`src/logic/`** | Lógica de negócios (o motor de cálculo). |
| **`src/hooks/`** | Lógica de estado e efeitos reutilizáveis. |

### 4.2. Camada de Dados (Offline-First)

A chave para o sucesso é a separação clara das responsabilidades entre a lógica de cálculo e a persistência de dados.

*   **`logic/calculationEngine.ts`:** Contém a função pura que recebe os parâmetros (Medidas, Valores das Peças, Adicionais Selecionados, Margens, Especificações Técnicas) e retorna o resultado do cálculo. **Não tem dependência de estado ou banco de dados.**
*   **`data/localDB.ts` (WatermelonDB/SQLite):** Responsável por armazenar os **Valores das Peças** (RF02), os **Cálculos Salvos** (Histórico, incluindo Dados do Cliente e Especificações Técnicas - RF11) e os **Presets** (RF07). Deve implementar a **Criptografia** para dados sensíveis.
*   **`data/syncService.ts` (Supabase):** Responsável por verificar a conectividade, enviar dados locais alterados para o Supabase e baixar novos Presets ou atualizações de valores de peças do Supabase.

### 4.3. Gerenciamento de Estado

Recomenda-se o uso de uma biblioteca de gerenciamento de estado reativo (ex: **Zustand** ou **Redux Toolkit**) para gerenciar o estado global do aplicativo, como o estado de conectividade (`isOnline`), o estado de carregamento de dados e os dados do cálculo atual.

### 4.4. Etapas de Desenvolvimento

O desenvolvimento será dividido em fases focadas para garantir a entrega incremental e a funcionalidade do núcleo offline primeiro.

| Fase | Foco | Tecnologias Chave |
| :--- | :--- | :--- |
| **Fase 1: Core Offline** | Implementação do Onboarding Forçado e do Motor de Cálculo (incluindo Especificações Técnicas). | React Native, WatermelonDB (sem sincronização), `logic/calculationEngine.ts`. |
| **Fase 2: UI Completa** | Construção de todas as telas (Cálculo, Configurações, Histórico) e a navegação Nav-Bottom. | Componentes de UI, `screens/`, Gerenciamento de Estado. |
| **Fase 3: Backend e Sincronização** | Configuração do Supabase e implementação do `syncService`. | Supabase SDK, `data/syncService.ts`. |
| **Fase 4: Presets, Distribuição e Compartilhamento** | Implementação da tela de Presets, geração do APK de Release e funcionalidade de Compartilhamento (Word/PDF). | WatermelonDB (Presets), Configuração de Build (APK), Biblioteca de Geração de Documentos. |
