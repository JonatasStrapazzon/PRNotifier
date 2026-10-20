# PRNotifier

Instalador do **Pull Requests Notifier**: aplicativo de bandeja do Windows que avisa sobre eventos em Pull Requests do Azure DevOps (Server/TFS e Cloud).

## Instalar

1. Baixe `PRNotifier-win-Setup.exe` na [versão mais recente](../../releases/latest).
2. Execute. A instalação é por usuário (não pede administrador) e instala o .NET 9 Desktop Runtime se ele ainda não existir.
3. Na primeira abertura, preencha URL, collection, projeto, repositório e PAT em **Configurações** e use **Testar Conexão**.

> O instalador não é assinado digitalmente. Se o Windows mostrar "O Windows protegeu o computador", clique em **Mais informações → Executar assim mesmo**.

## Atualizações

O app verifica novas versões ao abrir e a cada 6 horas. Quando houver uma, aparece uma notificação: clique nela para baixar e atualizar. Também dá para verificar manualmente em **bandeja → Verificar atualizações**.

Suas configurações e o PAT ficam em `%APPDATA%\PullRequestsNotifier` e não são apagados na atualização.
