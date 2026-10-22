# Pull Requests Notifier

Aplicativo leve para **Windows** que fica na bandeja do sistema (perto do relógio) e **avisa na hora** quando algo acontece nos seus **Pull Requests do Azure DevOps** (Server/TFS e Services/Cloud): comentário novo, voto de revisor, conflito de merge, build que falhou, PR pronto para merge, PR concluído e mais.

Você não precisa ficar com a página de PRs aberta nem atualizando o navegador. O app consulta o Azure DevOps de tempos em tempos, compara com o que viu antes e mostra uma notificação elegante só do que mudou.

<p align="center">
  <img src="docs/img/lista-fluent-dark.png" alt="Lista de Pull Requests no estilo Fluent escuro" width="820">
</p>

> **Versão atual: 1.2.0.** Veja o que mudou nas [notas de versão](CHANGELOG.md).

## Sumário

- [Destaques](#destaques)
- [Como funciona](#como-funciona)
- [Requisitos](#requisitos)
- [Instalação passo a passo](#instalação-passo-a-passo)
- [Primeira configuração](#primeira-configuração)
- [Uso no dia a dia](#uso-no-dia-a-dia)
- [Referência de todas as opções](#referência-de-todas-as-opções)
  - [Aba Conexão](#aba-conexão)
  - [Aba Notificações](#aba-notificações)
  - [Aba Eventos a Notificar](#aba-eventos-a-notificar)
  - [Aba Filtros](#aba-filtros)
  - [Aba Sistema & Tema](#aba-sistema--tema)
- [Estilos visuais](#estilos-visuais)
- [Como são os alertas na tela](#como-são-os-alertas-na-tela)
- [Atualizações automáticas](#atualizações-automáticas)
- [Acessibilidade](#acessibilidade)
- [Onde ficam seus dados e segurança](#onde-ficam-seus-dados-e-segurança)
- [Solução de problemas](#solução-de-problemas)
- [Desinstalar](#desinstalar)
- [Limitações conhecidas](#limitações-conhecidas)

---

## Destaques

- **Alertas na tela** modernos, que não roubam o foco do que você está digitando, com **botões de ação**: *Abrir*, *Silenciar PR* e *Adiar 1 h*.
- **Mais de dez tipos de evento**, ligados ou desligados um a um: comentários, menções, votos, conflitos, builds, novos commits, novos PRs, PR concluído ou abandonado e outros. Mais lembrete de **PR parado** e **resumo do dia**.
- **Você escolhe como ser avisado:** só o alerta do app, só a notificação do Windows ou os dois.
- **Não perturbe:** horário de trabalho, tela cheia/apresentação e pausa pela bandeja, com um resumo do que ficou para trás.
- **Filtros:** branch de destino, autor, palavras no título e rascunhos.
- **Agrupamento inteligente** de vários eventos em um único resumo.
- **Painel de PRs** com busca, ordenação, agrupamento por situação e histórico que sobrevive ao fechamento do app.
- **3 estilos visuais × tema claro e escuro** (6 aparências), com a navegação mudando de lugar conforme o estilo.
- **Atalho global** de teclado para abrir o painel.
- **Português e Inglês**, trocáveis a qualquer momento.
- **Somente leitura:** o app só consulta o Azure DevOps, nunca altera nada nos seus PRs.
- **Atualização automática** com um clique.
- **Seguro:** o token de acesso (PAT) é guardado criptografado, vinculado ao seu usuário do Windows.
- **Acessível:** leitores de tela, foco por teclado e alto contraste.

## Como funciona

1. A cada **15 segundos** (configurável de 10 a 600) o app pergunta ao Azure DevOps quais são os Pull Requests ativos do repositório que você escolheu. O **servidor já filtra** os que você criou e/ou aqueles em que você é revisor, e a lista é **paginada**, então repositórios movimentados não ficam truncados.
2. Para cada PR, lê os comentários (tópicos), os status de validação (builds, SonarQube etc.) e os votos dos revisores, no máximo 4 consultas em paralelo.
3. Compara com o que viu na rodada anterior. **A primeira leitura depois de abrir o app serve só de referência** e não gera alertas; daí em diante, qualquer diferença vira um evento.
4. Cada evento passa pelos seus **filtros** e pelos tipos que você ligou em **Eventos a Notificar**, e entra no **histórico**. Se não estiver em silêncio (veja [Não perturbe](#não-perturbe)), vira um alerta.
5. Se um PR some da lista de ativos, o app confere se foi **concluído** ou **abandonado** e avisa.

**Intervalo adaptativo.** Sem novidades por um bom tempo, o app consulta mais devagar (até 5 minutos) e volta ao ritmo normal ao detectar algo, ao abrir o painel ou ao clicar em *Atualizar Agora*.

**Quedas de rede.** Se a VPN cair ou o servidor responder com erro, o app **não perde o que já sabia**: tenta de novo com espera crescente (até 5 minutos), mostra o estado no menu e no ícone da bandeja e avisa **uma vez** quando a conexão cai e **uma vez** quando volta. Não aparece avalanche de alertas falsos quando a conexão retorna.

> O app usa apenas requisições de **leitura** (`GET`) à API REST do Azure DevOps. Ele não aprova, não comenta e não altera PRs.

## Requisitos

| Item | Requisito |
|---|---|
| Sistema | Windows 10 ou 11, 64 bits |
| .NET | **.NET 9 Desktop Runtime**. O instalador baixa e instala sozinho se você não tiver. |
| Azure DevOps | Azure DevOps Server (TFS on-premises) ou Azure DevOps Services (nuvem) |
| Acesso | Um **Personal Access Token (PAT)** seu, com permissão de leitura de código (veja abaixo) |
| Rede | Alcance até o servidor do Azure DevOps (VPN, se for o caso) e acesso à internet para as atualizações |

> O app foi validado principalmente com **Azure DevOps Server**. Para o Azure DevOps Services (nuvem) use a configuração indicada em [Primeira configuração](#2-descobrir-os-dados-da-conexão); ela segue o mesmo formato de endereço da API.

## Instalação passo a passo

1. Abra a página de [**Releases**](../../releases/latest) deste repositório.
2. Em **Assets**, baixe **`PRNotifier-win-Setup.exe`**.
3. Dê um duplo clique no arquivo. A instalação é **por usuário**: não pede senha de administrador e leva poucos segundos. Se faltar o .NET 9 Desktop Runtime, o instalador o instala junto.
4. Ao terminar, o app abre sozinho e o ícone aparece na **bandeja do sistema** (se não estiver à vista, clique na setinha **^** perto do relógio). Na primeira vez, o **assistente de configuração** abre automaticamente.

### Aviso "O Windows protegeu o computador"

O instalador não tem assinatura digital paga, então o Windows SmartScreen pode mostrar essa tela na primeira execução. Clique em **Mais informações** e depois em **Executar assim mesmo**. Isso acontece só na instalação.

## Primeira configuração

Na primeira abertura, um **assistente de 3 passos** guia você. Se preferir, o botão **Configurações completas** abre a janela inteira de opções.

<p align="center">
  <img src="docs/img/assistente-1-fluent-dark.png" alt="Assistente, passo 1" width="300">
  <img src="docs/img/assistente-2-fluent-dark.png" alt="Assistente, passo 2" width="300">
  <img src="docs/img/assistente-3-fluent-dark.png" alt="Assistente, passo 3" width="300">
</p>

Você precisa de quatro informações do seu Azure DevOps e de um token.

### 1. Criar o Personal Access Token (PAT)

1. No Azure DevOps, clique no ícone de usuário no canto superior direito e vá em **Personal access tokens** (ou **Security → Personal access tokens**).
2. Clique em **New Token**.
3. Dê um nome (por exemplo, `Pull Requests Notifier`) e escolha a validade.
4. Em **Scopes**, marque **Code → Read** (leitura de código). Isso basta para ler PRs, comentários e status.
5. Crie e **copie o token**: ele só é mostrado uma vez.

> Quando o token expirar, o app avisa "Falha de autenticação". Gere um novo e cole em Configurações.

### 2. Descobrir os dados da conexão

O endereço de um PR no navegador mostra tudo o que você precisa. Veja os dois formatos:

| Tipo | Endereço de um PR | URL do Servidor | Coleção / Organization | Projeto | Repositório |
|---|---|---|---|---|---|
| **Azure DevOps Server (TFS)** | `https://devops.exemplo.com:8081/DefaultCollection/MeuProjeto/_git/MeuRepo/pullrequest/123` | `https://devops.exemplo.com:8081` | `DefaultCollection` | `MeuProjeto` | `MeuRepo` |
| **Azure DevOps Services (nuvem)** | `https://dev.azure.com/minhaorg/MeuProjeto/_git/MeuRepo/pullrequest/123` | `https://dev.azure.com` | `minhaorg` (o nome da organização) | `MeuProjeto` | `MeuRepo` |

### 3. Preencher e testar

1. Preencha **URL do Servidor**, **Coleção / Organization**, **Projeto** e **Repositório** (passo 1) e o **PAT** (passo 2).
2. Clique em **Testar Conexão**. Se tudo estiver certo, aparece **"Conectado: seu nome!"** em verde.
3. No passo 3, escolha estilo, tema e idioma (a prévia aparece na hora) e clique em **Concluir e monitorar**.

<p align="center">
  <img src="docs/img/aba-conexao-fluent-dark.png" alt="Aba Conexão" width="760">
</p>

## Uso no dia a dia

### O ícone da bandeja

Clique com o **botão direito** no ícone para abrir o menu. Um **duplo clique** (ou o atalho global) abre o painel de PRs.

<p align="center">
  <img src="docs/img/bandeja-fluent-dark.png" alt="Menu da bandeja no estilo Fluent escuro" width="190">
  &nbsp;&nbsp;
  <img src="docs/img/bandeja-clean-light.png" alt="Menu da bandeja no estilo Limpo claro" width="190">
</p>

| Item do menu | Para que serve |
|---|---|
| Linha de status | Mostra o que o app está fazendo: "Conectando...", "Atualizado às 14:32 (3 PRs)", "Falha ao autenticar" etc. |
| **Meus PRs (n)** | Submenu com seus PRs ativos. Clicar em um abre o PR no navegador. PRs com conflito aparecem com ⚠️. |
| **Painel de Pull Requests...** | Abre o painel com a lista completa. |
| **Histórico de Notificações...** | Abre o painel já na página do histórico. |
| **Verificar Agora** | Consulta o Azure DevOps imediatamente, sem esperar o próximo ciclo. |
| **Pausar notificações** | Silencia os alertas por 30 minutos, 1 hora, 2 horas ou até amanhã de manhã. Enquanto pausado, o item mostra "Pausado até HH:mm" e traz a opção **Retomar agora**. |
| **Testar Notificações na Tela** | Mostra um alerta individual e um alerta agrupado de exemplo, para você ver como ficam com suas configurações. |
| **Configurações...** | Abre a janela de configurações. |
| **Verificar atualizações** | Procura uma versão nova do app. |
| Versão | Mostra a versão instalada. |
| **Sair** | Fecha o app. |

**O ícone muda de estado:** ganha um selo **vermelho** quando não há conexão ou o token falhou, e um selo **âmbar** quando as notificações estão em silêncio (pausa, fora do expediente ou tela cheia).

### O painel de Pull Requests

O painel tem uma **navegação** (que muda de lugar conforme o estilo escolhido) com três destinos: **PRs ativos**, **Histórico** e **Configurações**.

**PRs ativos.** Cada linha mostra o número do PR, o título, `branch de origem → branch de destino · autor` e uma **etiqueta de situação**:

| Etiqueta | Significado |
|---|---|
| 🟢 **Pronto p/ merge** | Sem conflitos de merge. |
| 🔴 **Conflito** | O PR tem conflitos que precisam ser resolvidos. |
| ⚪ outra (ex.: `queued`) | O Azure DevOps ainda está calculando o merge. |

Acima da lista há:

- **Busca:** filtra por número, título, autor ou branch enquanto você digita.
- **Ordenação:** mais recentes, mais antigos, título (A–Z), autor (A–Z) ou situação.
- **Agrupar por situação:** separa em *Com conflito*, *Prontos para merge* e *Em andamento*.

<p align="center">
  <img src="docs/img/lista-agrupada-fluent-dark.png" alt="Lista agrupada por situação, com um PR silenciado" width="820">
</p>

Um **duplo clique** (ou **Enter**) na linha abre o PR no navegador. O botão **Abrir no Navegador** faz o mesmo com a linha selecionada. **Silenciar PR** (ou **Reativar PR**) liga/desliga os alertas na tela daquele PR: ele ganha o selo 🔕 e continua aparecendo no histórico. **Atualizar Agora** (no topo) força uma consulta; se já houver uma em andamento, ele espera ela terminar.

**Histórico.** Lista os alertas que o app emitiu (os 200 mais recentes, do mais novo ao mais antigo), com horário, título, primeira linha da mensagem e o número do PR. Duplo clique abre o PR relacionado. **Limpar Histórico** esvazia a lista. O histórico é **gravado em disco** e continua disponível depois de fechar e abrir o app.

<p align="center">
  <img src="docs/img/historico-fluent-dark.png" alt="Histórico de notificações" width="820">
</p>

### Atalho global

O atalho **Ctrl+Alt+P** (configurável) abre o painel de qualquer programa. Veja [Aba Notificações](#aba-notificações).

## Referência de todas as opções

A janela de Configurações tem cinco abas. No rodapé:

- **Salvar e Monitorar**: grava tudo e **reinicia o monitoramento** já com as novas opções.
- **Cancelar**: descarta as alterações. Qualquer pré-visualização de estilo, tema ou idioma é desfeita.

No canto superior direito há dois atalhos: **Tema: Claro/Escuro** e **Idioma: BR/EN**, que alternam o visual e o idioma na hora (só valem de verdade depois de **Salvar**).

### Aba Conexão

Os dados para falar com o Azure DevOps.

<p align="center">
  <img src="docs/img/aba-conexao-gradient-light.png" alt="Aba Conexão no estilo Gradiente claro" width="760">
</p>

| Opção | O que faz | Padrão |
|---|---|---|
| **URL do Servidor DevOps** | Endereço base do servidor, sem caminho. Ex.: `https://devops.exemplo.com:8081` (Server) ou `https://dev.azure.com` (nuvem). | vazio |
| **Coleção / Organization** | No Azure DevOps Server, o nome da coleção (ex.: `DefaultCollection`). Na nuvem, o nome da organização. | vazio |
| **Projeto** | Nome do projeto. Nomes com espaço funcionam. | vazio |
| **Repositório Git Padrão** | Nome (ou ID) do repositório Git a monitorar. O app monitora **um repositório** por vez. | vazio |
| **Personal Access Token (PAT)** | Seu token de acesso. O ícone de olho à direita do campo mostra/oculta o texto digitado. | vazio |
| **Testar Conexão** | Autentica no servidor e mostra "Conectado: seu nome!" ou o erro. Use antes de salvar. | n/a |
| **Intervalo de verificação em segundo plano** | De quanto em quanto tempo o app consulta o Azure DevOps. Vai de 10 a 600 segundos. Intervalos menores avisam mais rápido, mas fazem mais requisições ao servidor. | 15 s |
| **Reduzir as consultas quando estiver ocioso (intervalo adaptativo)** | Sem novidades por um bom tempo, consulta mais devagar (no máximo a cada 5 min). Volta ao normal ao detectar algo ou ao abrir o painel. Fora do horário de trabalho também diminui o ritmo. | ligado |

### Aba Notificações

Como e quando os alertas aparecem.

<p align="center">
  <img src="docs/img/aba-notificacoes-fluent-dark.png" alt="Aba Notificações, parte de cima" width="760">
  <br>
  <img src="docs/img/aba-notificacoes-2-fluent-dark.png" alt="Aba Notificações, parte de baixo" width="760">
</p>

**Como avisar**

| Opção | O que faz | Padrão |
|---|---|---|
| **Tipo de notificação** | **Apenas o alerta do app**, **apenas a notificação do Windows** (a do ícone da bandeja, que vai para a Central de Ações) ou **os dois**. Com "os dois", cada evento aparece duas vezes. | Apenas o alerta do app |

**Som da Notificação**

| Opção | O que faz | Padrão |
|---|---|---|
| **Tocar som ao emitir notificação** | Liga/desliga o som. Quando vários alertas chegam juntos, toca um som só. | ligado |
| **Estilo do som** | Escolha entre 10 sons nativos do Windows: Windows Notify, Asterisk, Chimes, Chord, Ding, Balloon, Tada, Notify Classic, Exclamation e Beep. | Windows Notify |
| **Testar Som** | Toca o som escolhido para você ouvir. | n/a |

**Tempo e Fixação na Tela**

| Opção | O que faz | Padrão |
|---|---|---|
| **Fixar notificações na tela até fechar manualmente (no X)** | O alerta só some quando você clica no **X**. Alertas fixados ganham o selo **FIXADO**. | desligado |
| **Tempo de exibição quando não fixado** | Por quantos segundos o alerta fica visível antes de sumir (2 a 60). Fica desativado se a fixação estiver ligada. | 6 s |

> Enquanto o mouse estiver **sobre** o alerta, a contagem para e ele não fecha sozinho.

**Agrupamento de Notificações**

| Opção | O que faz | Padrão |
|---|---|---|
| **Agrupar notificações quando houver múltiplos eventos simultâneos** | Se uma mesma rodada de verificação gerar vários eventos, o app mostra **um alerta-resumo** em vez de vários. Clicar nele abre o PR (se for um só) ou a lista de PRs. Todos os eventos continuam no histórico. | desligado |
| **Agrupar em alerta único quando houver no mínimo** | A partir de quantos eventos simultâneos o resumo passa a ser usado (2 a 20). Fica desativado se o agrupamento estiver desligado. | 2 |

#### Não perturbe

| Opção | O que faz | Padrão |
|---|---|---|
| **Só avisar no horário de trabalho** | Fora do horário e dos dias escolhidos, os alertas não aparecem (continuam no histórico). | desligado |
| **Das (início) e até as (fim)** | Hora de início e de fim do expediente. Aceita janela que passa da meia-noite (ex.: 22:00 às 06:00). | 08:00 às 18:00 |
| **Dias da semana** | Dom, Seg, Ter, Qua, Qui, Sex, Sáb. | Seg a Sex |
| **Não avisar com tela cheia ou em apresentação** | Não interrompe jogos, filmes em tela cheia nem apresentações. | ligado |
| **Ao voltar, mostrar um resumo do que foi perdido** | Quando o silêncio acaba (pausa, fim de apresentação, volta do expediente), aparece **um** alerta com a quantidade de alertas que ficaram para trás. | ligado |

Além dessas regras, você pode **pausar na hora** pelo menu da bandeja ou pelo botão **Adiar 1 h** do alerta. As pausas têm prioridade sobre tudo.

#### Lembretes e resumo do dia

| Opção | O que faz | Padrão |
|---|---|---|
| **Lembrar de PRs parados há (dias)** | Avisa **uma vez por dia** sobre PRs **seus** ou **aguardando a sua revisão** (você ainda não votou) sem nenhuma atividade (comentário, push ou voto) há esse número de dias. PRs em rascunho são ignorados. `0` desliga. | 0 (desligado) |
| **Mostrar um resumo do dia em um único alerta** | Um alerta por dia, a partir da hora escolhida, com o total de PRs ativos e quantos aguardam sua revisão, têm conflito ou estão prontos para merge. Se o expediente estiver ligado, só em dias úteis. | desligado |
| **A partir das** | Hora do resumo diário. | 09:00 |

#### Atalho global

| Opção | O que faz | Padrão |
|---|---|---|
| **Abrir o painel com um atalho de teclado** | Liga o atalho global. | ligado |
| **Atalho** | Escolha entre `Ctrl+Alt+P`, `Ctrl+Shift+P`, `Ctrl+Alt+N` e `Win+Shift+P`. Se outro programa já usar a combinação, escolha outra. | Ctrl+Alt+P |

### Aba Eventos a Notificar

Escolha o que merece um alerta. **Cada caixa liga ou desliga um tipo de evento.** Por padrão, todas vêm marcadas.

<p align="center">
  <img src="docs/img/aba-eventos-fluent-dark.png" alt="Aba Eventos a Notificar" width="760">
</p>

**Commits & Conflitos de Merge**

| Opção | Quando avisa |
|---|---|
| **Novos commits enviados no branch do PR** | Alguém fez `push` no branch de origem do PR. |
| **Novo PR criado ou atribuído a mim** | Aparece um PR novo no seu escopo (criado por você ou em que você virou revisor) depois que o app já estava rodando. PRs em rascunho (*draft*) são ignorados. |
| **Conflitos de merge detectados** | O PR passou a ter conflitos. |
| **Conflitos de merge resolvidos** | O PR tinha conflitos e agora está sem eles. |
| **PR pronto para merge (aprovado e sem pendências)** | O PR tem pelo menos uma aprovação (ou aprovação com sugestões), nenhum revisor pedindo mudanças ou rejeitando, sem conflitos e sem validações falhas. Avisa **uma vez**, quando passa a estar pronto. |

**Discussões, Votos & Menções**

| Opção | Quando avisa |
|---|---|
| **Novos comentários e respostas nos tópicos** | Chega um comentário novo, inclusive resposta em um tópico que já existia. Comentários automáticos do sistema são ignorados. |
| **Tópicos de discussão resolvidos (Fixed)** | Um tópico foi marcado como resolvido. |
| **Tópicos de discussão reativados (Active)** | Um tópico resolvido foi reaberto. |
| **Menções diretas a você (@seu_nome)** | Alguém te mencionou em um comentário. Esse alerta substitui o de comentário comum. |
| **Votos de revisores (Aprovou, Rejeitou, etc.)** | Um revisor votou: *aprovou*, *aprovou com sugestões*, *está aguardando o autor* ou *rejeitou*. |

**Validações & Status do PR**

| Opção | Quando avisa |
|---|---|
| **Status de validações de Build e SonarQube** | Uma validação do PR mudou de estado (ex.: `pending` → `succeeded` ou `failed`). O ícone indica o resultado: ✅ passou, ❌ falhou, ⚙️ em andamento. |
| **Conclusão automática (Auto-complete) agendada** | Alguém agendou o *auto-complete* do PR. |
| **Mudança de status (PR Concluído ou Abandonado)** | O PR deixou de estar ativo: avisa se foi **concluído** (mesclado) ou **abandonado** e por quem. |
| **Notificar também minhas próprias ações (para testes)** | Se **desmarcada**, o app ignora o que **você mesmo** fez (seus comentários, votos, pushes, PRs e conclusões). Útil para testar o app sozinho com uma segunda conta. |

**Avisos de conexão** (sempre ativos): o app avisa quando perde a conexão com o servidor, quando o token (PAT) falha e quando a conexão volta, **um aviso por mudança de estado**.

### Aba Filtros

Escolha quais PRs merecem a sua atenção. **Campos vazios não filtram.** Os filtros valem para a lista, os alertas e os lembretes.

<p align="center">
  <img src="docs/img/aba-filtros-fluent-dark.png" alt="Aba Filtros" width="760">
</p>

| Opção | O que faz | Padrão |
|---|---|---|
| **Ignorar PRs em rascunho (draft)** | Esconde PRs marcados como rascunho. | desligado |
| **Branches de destino** | Só PRs que vão para esses branches. Separe por vírgula e use `*` como coringa. Ex.: `main, release/*`. | vazio (todos) |
| **Autores** | Só PRs de autores cujo nome ou e-mail contenha o texto. Ex.: `ana, bia@empresa.com`. | vazio (todos) |
| **Palavras no título** | O título precisa ter **ao menos uma** das palavras. Use `-palavra` para **excluir**. Ex.: `hotfix, -wip` (só hotfix, e nunca os WIP). | vazio (todos) |

Quando mais de um filtro está preenchido, o PR precisa passar por **todos**.

### Aba Sistema & Tema

Aparência, idioma, escopo e inicialização.

<p align="center">
  <img src="docs/img/aba-sistema-fluent-dark.png" alt="Aba Sistema & Tema" width="760">
</p>

| Opção | O que faz | Padrão |
|---|---|---|
| **Estilo do aplicativo** | Escolhe o layout de **todo** o app: Fluent (menu lateral), Limpo (abas no topo) ou Gradiente (barra de ícones). Veja [Estilos visuais](#estilos-visuais). A mudança aparece **na hora**, como pré-visualização. | Fluent |
| **Tema do aplicativo** | Claro ou Escuro. Combina com qualquer estilo. | Escuro |
| **Idioma da interface** | Português (Brasil) ou English (United States). | Português |
| **PRs onde sou revisor (Assigned to me)** | Monitora os PRs em que você foi incluído como revisor. | ligado |
| **PRs criados por mim (Author)** | Monitora os PRs que você abriu. | ligado |
| **Iniciar o Pull Requests Notifier automaticamente com o Windows** | Abre o app sozinho quando você entra no Windows. | desligado |

> Desmarcar **as duas** opções de escopo faz o app não monitorar nenhum PR.

## Estilos visuais

Em **Configurações → Sistema & Tema → Estilo** você escolhe entre três visuais. Cada um tem versão **clara** e **escura**. A **navegação muda de lugar** conforme o estilo, e as mesmas funções continuam disponíveis em todos.

| Estilo | Navegação | Personalidade |
|---|---|---|
| **Fluent** | Menu lateral com ícone e texto | Parecido com o Windows 11: cartões suaves, cantos arredondados, azul de destaque. |
| **Limpo** | Abas em formato de pílula no topo | Muito espaço em branco, visual leve e direto. |
| **Gradiente** | Barra estreita de ícones à esquerda (Configurações no rodapé dela) | Mais ousado: cantos bem arredondados e destaque em gradiente azul-ciano. |

### Painel de PRs

| Escuro | Claro |
|:--:|:--:|
| <img src="docs/img/lista-fluent-dark.png" alt="Fluent escuro" width="440"><br>**Fluent**, menu lateral | <img src="docs/img/lista-fluent-light.png" alt="Fluent claro" width="440"><br>**Fluent**, menu lateral |
| <img src="docs/img/lista-clean-dark.png" alt="Limpo escuro" width="440"><br>**Limpo**, abas no topo | <img src="docs/img/lista-clean-light.png" alt="Limpo claro" width="440"><br>**Limpo**, abas no topo |
| <img src="docs/img/lista-gradient-dark.png" alt="Gradiente escuro" width="440"><br>**Gradiente**, barra de ícones | <img src="docs/img/lista-gradient-light.png" alt="Gradiente claro" width="440"><br>**Gradiente**, barra de ícones |

### Janela de Configurações

| Escuro | Claro |
|:--:|:--:|
| <img src="docs/img/config-fluent-dark.png" alt="Configurações Fluent escuro" width="440"> | <img src="docs/img/config-fluent-light.png" alt="Configurações Fluent claro" width="440"> |
| <img src="docs/img/config-clean-dark.png" alt="Configurações Limpo escuro" width="440"> | <img src="docs/img/config-clean-light.png" alt="Configurações Limpo claro" width="440"> |
| <img src="docs/img/config-gradient-dark.png" alt="Configurações Gradiente escuro" width="440"> | <img src="docs/img/config-gradient-light.png" alt="Configurações Gradiente claro" width="440"> |

### Alertas na tela

Os alertas também seguem o estilo: barra lateral colorida no Fluent, cartão limpo no Limpo e faixa em gradiente no topo no Gradiente. A cor do ícone indica o tipo do evento (roxo para comentários, vermelho para conflitos e falhas, verde para aprovações e sucesso, âmbar para novos commits, lembretes e atualizações, azul para PR novo).

| Escuro | Claro |
|:--:|:--:|
| <img src="docs/img/notificacoes-fluent-dark.png" alt="Alertas Fluent escuro" width="380"> | <img src="docs/img/notificacoes-fluent-light.png" alt="Alertas Fluent claro" width="380"> |
| <img src="docs/img/notificacoes-clean-dark.png" alt="Alertas Limpo escuro" width="380"> | <img src="docs/img/notificacoes-clean-light.png" alt="Alertas Limpo claro" width="380"> |
| <img src="docs/img/notificacoes-gradient-dark.png" alt="Alertas Gradiente escuro" width="380"> | <img src="docs/img/notificacoes-gradient-light.png" alt="Alertas Gradiente claro" width="380"> |

## Como são os alertas na tela

- Aparecem no **canto inferior direito**, na tela onde está a barra de tarefas, com uma entrada suave (fade e deslize, se as animações do Windows estiverem ligadas), e **empilham** de baixo para cima. No máximo **4** ficam visíveis ao mesmo tempo; ao chegar o quinto, o mais antigo sai (tudo continua no histórico).
- **Não roubam o foco**: você continua digitando onde estava.
- **Botões de ação:**
  - **Abrir** (ou **Atualizar**, no aviso de nova versão): abre o PR no navegador, ou o histórico no caso de resumos de vários PRs. Clicar em qualquer ponto do alerta faz o mesmo.
  - **Silenciar PR**: para de mostrar alertas desse PR (ele continua no histórico). Para reverter, use **Reativar PR** no painel.
  - **Adiar 1 h**: silencia **todos** os alertas por uma hora.
  - **X**: só fecha o alerta.
- O mouse **sobre** o alerta pausa a contagem para fechar.
- Se você mudar a resolução ou conectar/desconectar um monitor, os alertas se reposicionam sozinhos.
- Com o tipo de notificação em **Windows** ou **os dois**, o ícone da bandeja também gera uma notificação nativa do Windows. Clicar nela faz o mesmo que clicar no alerta.

## Atualizações automáticas

- O app procura versão nova **30 segundos depois de abrir** e depois **a cada 6 horas**, consultando as versões publicadas neste repositório.
- Quando há uma, aparece uma notificação **"Nova versão X disponível"**. Clique em **Atualizar**: o app baixa, reinicia e já abre atualizado.
- Você também pode procurar manualmente: **bandeja → Verificar atualizações**.
- Só **instalações feitas pelo `Setup.exe`** se atualizam. Quem roda o executável solto (sem instalar) vê a mensagem "Atualização indisponível".
- Suas configurações e o PAT **não são apagados** na atualização.
- As notas de cada versão estão em [CHANGELOG.md](CHANGELOG.md) e na página de [Releases](../../releases).

## Acessibilidade

- **Leitores de tela** (Narrador, NVDA): botões, campos, caixas de seleção, listas e a navegação têm nome e papel anunciados; as caixas de seleção informam se estão marcadas.
- **Teclado:** tudo funciona sem mouse. Use **Tab** para andar entre os campos, **Espaço/Enter** para acionar, **setas** nas listas e na navegação, e **Esc/Enter** nos botões Cancelar/Salvar. O foco aparece destacado.
- **Alto contraste:** com um tema de alto contraste do Windows ligado, o app usa as cores do sistema e desliga as animações.
- **Escala do Windows:** testado em 125%, 150% e 200% sem texto cortado.

## Onde ficam seus dados e segurança

| O quê | Onde |
|---|---|
| Configurações (inclui PRs silenciados) | `%APPDATA%\PullRequestsNotifier\settings.json` |
| Histórico de alertas (últimos 200) | `%APPDATA%\PullRequestsNotifier\history.json` |
| Log (para diagnóstico) | `%APPDATA%\PullRequestsNotifier\app.log` (rotação automática, no máximo ~2 MB) |

**Segurança**

- O **PAT é gravado criptografado** com a proteção de dados do Windows (DPAPI) e só pode ser lido pelo **seu usuário do Windows**, nesta máquina. Copiar o arquivo para outro computador ou usuário não revela o token (você terá que digitá-lo de novo).
- O app é **somente leitura**: só faz consultas ao Azure DevOps.
- Ele só se comunica com **dois destinos**: o seu servidor Azure DevOps e o GitHub (para procurar atualizações). **Não envia telemetria** nem dados de uso.
- Use um PAT com o **menor escopo possível** (**Code → Read**) e com validade curta.

## Solução de problemas

**"Falha ao autenticar. Verifique URL e PAT."**
Confira: (1) a URL do servidor está só com o endereço base, sem `/_git` nem nome de projeto; (2) o nome da **coleção** (ou da organização) está certo; (3) o PAT não expirou e tem **Code → Read**; (4) você está na rede/VPN que alcança o servidor.

**"Erro de rede" / "Erro do servidor: HTTP ..." no menu da bandeja**
É um problema de conexão (VPN caída, servidor fora do ar). O app tenta de novo sozinho, com espera crescente, e **não perde** o estado anterior. Você recebe um aviso quando a conexão cai e outro quando volta.

**Cada evento aparece duas vezes (alerta do app e do Windows)**
O **Tipo de notificação** está em "Os dois". Em **Configurações → Notificações → Como avisar**, escolha **Apenas o alerta do app** (ou apenas o do Windows).

**A lista de PRs está vazia ou faltam PRs**
Verifique o **nome do repositório** (aba Conexão), se pelo menos uma opção de escopo está marcada (aba Sistema & Tema) e se algum **filtro** (aba Filtros) não está escondendo o PR. Só aparecem PRs **ativos** em que você é **autor** ou **revisor**.

**Não recebo notificações**
- Lembre que a **primeira leitura** depois de abrir o app só serve de referência: os alertas vêm das mudanças seguintes.
- Veja se o ícone da bandeja tem o selo **âmbar**: as notificações podem estar em silêncio (pausa, **fora do horário de trabalho** ou tela cheia). Os alertas continuam no histórico.
- O PR pode estar **silenciado** (selo 🔕 no painel). Use **Reativar PR**.
- Confira se o tipo de evento está marcado na aba **Eventos a Notificar** e se um filtro não o exclui.
- Se **Notificar também minhas próprias ações** estiver desmarcado, o que você mesmo faz não gera alerta.
- Use **bandeja → Testar Notificações na Tela** para ver se os alertas aparecem com as suas configurações.

**O atalho de teclado não abre o painel**
Outro programa pode já estar usando a combinação. Escolha outra em **Configurações → Notificações → Atalho global**.

**O app não abriu / diz que já está em execução**
Ele roda **uma instância por vez**. O ícone está na bandeja; clique na setinha **^** perto do relógio para ver os ícones ocultos.

**O ícone sumiu da bandeja**
Abra o app de novo pelo Menu Iniciar. Se quiser que ele abra sozinho, ligue **Iniciar com o Windows** na aba Sistema & Tema.

**Quero começar do zero**
Feche o app (bandeja → Sair) e apague a pasta `%APPDATA%\PullRequestsNotifier`. Ao abrir de novo, o assistente de configuração aparece como na primeira vez.

**Aparece "Atualização indisponível"**
Você está rodando uma versão que não foi instalada pelo `Setup.exe`. Baixe o instalador na página de [Releases](../../releases/latest).

## Desinstalar

1. Abra **Configurações do Windows → Aplicativos → Aplicativos instalados**.
2. Procure **Pull Requests Notifier** e clique em **Desinstalar**.
3. (Opcional) Para remover também as suas configurações, o histórico e o log, apague a pasta `%APPDATA%\PullRequestsNotifier`.

## Limitações conhecidas

- Monitora **um repositório por vez** (o configurado na aba Conexão).
- Funciona por **consulta periódica** (não é tempo real): o atraso máximo é o intervalo configurado (ou até 5 minutos no modo adaptativo ocioso).
- Os alertas só aparecem **enquanto o app está aberto**. Eventos que acontecem com o app fechado não são avisados depois.
- Os lembretes de PR parado recomeçam ao reabrir o app (no máximo um por dia por PR enquanto ele estiver aberto).
- Somente Windows.

---

Dúvidas ou sugestões? Abra uma *issue* neste repositório.
