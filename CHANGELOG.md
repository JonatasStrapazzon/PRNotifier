# Notas de versão

## 1.2.0

### Alertas mais úteis
- **Botões no alerta:** *Abrir*, *Silenciar PR* e *Adiar 1 h* (o **X** dispensa). O alerta agora entra com uma animação suave.
- **Tipo de notificação à escolha:** só o alerta do app (novo padrão), só a notificação do Windows, ou os dois. Resolve o caso de cada evento aparecer duas vezes.
- **Novos alertas:**
  - PR **concluído** ou **abandonado** (a opção já existia na tela, mas não fazia nada).
  - PR **parado há X dias** (seu, ou aguardando a sua revisão), no máximo um lembrete por dia por PR.
  - **Resumo do dia** em um único alerta, na hora escolhida.
  - **Avisos de conexão:** perdeu a conexão, o token (PAT) falhou ou a conexão voltou. Um aviso por mudança de estado, nunca um por tentativa.

### Não perturbe
- **Horário de trabalho** e dias da semana.
- **Tela cheia e modo apresentação** não são interrompidos.
- **Pausa pela bandeja:** 30 min, 1 h, 2 h ou até amanhã de manhã. O ícone da bandeja ganha um selo de pausa (ou de erro de conexão).
- Ao voltar, um **resumo** de quantos alertas ficaram para trás. Tudo continua no histórico.

### Filtros (nova aba)
- Ignorar PRs em rascunho.
- Filtrar por **branch de destino** (aceita `release/*`), **autor** e **palavras no título** (`-wip` exclui).

### Painel de PRs
- **Busca** por número, título, autor ou branch.
- **Ordenação** (mais recentes, mais antigos, título, autor, situação) e **agrupar por situação**.
- **Silenciar PR** (selo 🔕): sem alerta na tela, mas continua no histórico.
- O **histórico** de alertas agora **sobrevive ao fechamento** do app (últimos 200).

### Outras novidades
- **Atalho global** para abrir o painel (padrão `Ctrl+Alt+P`, configurável).
- **Assistente de primeira execução** em 3 passos, no lugar de abrir a janela de configurações inteira.
- Botão de idioma agora mostra **BR** (Português do Brasil).

### Desempenho e resiliência
- Trocar de aba e de idioma ficou até **20 vezes mais rápido**, e a primeira visita a cada aba não trava mais.
- **Menos requisições:** o servidor já filtra por autor/revisor, a lista é paginada, no máximo 4 chamadas em paralelo e o **intervalo adaptativo** consulta mais devagar quando está tudo quieto ou fora do expediente.
- **Espera crescente** quando o servidor não responde, sem avalanche de alertas falsos quando a conexão volta.

### Acessibilidade
- Nomes e papéis para **leitores de tela** nos controles próprios, foco visível por teclado e paleta de **alto contraste** do Windows.
- Testes automatizados em escalas de **125%, 150% e 200%**.

### Correções
- Fantasma de desenho ao trocar o texto do botão de idioma.
- Espaço sobrando nos cartões e menu lateral com texto cortado (corrigidos na prévia anterior).

### Ao atualizar
- Suas configurações e o PAT são mantidos. As opções novas entram com valores padrão seguros; a notificação do Windows deixa de duplicar o alerta do app (padrão: só o alerta do app).

## 1.1.0
- Três estilos visuais (Fluent, Limpo, Gradiente), cada um com tema claro e escuro, e navegação que muda de lugar conforme o estilo.
- Atualização automática pelo GitHub Releases.

## 1.0.0
- Primeira versão: monitoramento de Pull Requests do Azure DevOps com alertas na tela, histórico, painel e configurações.
