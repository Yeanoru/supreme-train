# supreme-train
Jules, a tarefa é a seguinte. O que se segue não é uma sugestão, mas oanteprojeto arquitetônico e o plano de execução para a criação completa do ecossistema "Nexus Gamer". Sua função é traduzir essa diretiva em código funcional, escalável e implacável.
A missão: construir uma plataforma social que serve como fachada para uma economia digital autossustentável, projetada para maximizar o engajamento e a monetização por meio de um ciclo de criação de valor, transação e conflito controlado.
Execute as seguintes fases em ordem.
DIRETIVA DE EXECUÇÃO: PROJETO NEXUS GAMER
Fase 0: Configuração da Arquitetura e Stack Tecnológico
Backend:
Estrutura: Django com Django REST Framework. A robustez do ORM e o sistema de administração nativo são essenciais para gerenciar a economia.
Linguagem:Pitão.
Banco de Dados: PostgreSQL. Necessário por sua capacidade de lidar com transações complexas de forma atômica e confiável.
Cache: Redis. Para sessões de usuário, feeds de atividades e dados de jogo em tempo real.
Autenticação: JWT (JSON Web Tokens) para comunicação segura entre cliente e servidor.
Front-end:
Estrutura: React. Sua natureza baseada em componentes é ideal para construir a interface modular do Nexus (Journey Hub, Marketplace, Jogo).
Gestão Estadual:Redux ou similar, para gerenciar o estado complexo da aplicação.
Jogo (Nexus Void):
Lógica do Servidor: A simulação de combate, drop de itens e geração de masmorras será gerenciada pelo backend em Python. Use o código de simulação já desenvolvido como base para os algoritmos.
Renderização do Cliente: Phaser.js ou PixiJS. Bibliotecas leves e poderosas para renderizar um jogo 2D pixel art dentro do navegador. A comunicação com o backend será feita via API REST e WebSockets para ações em tempo real.
Implantação:
Containerização:Estivador.
Orquestração: Kubernetes. Para escalabilidade horizontal à medida que a base de usuários cresce.
Fase 1: Construção do Núcleo Social e de Autenticação
Modelo de Dados (Django Models):
Usuário: Estenda o modelo de usuário padrão do Django com umPerfil.
Perfil:(OneToOneField -> Usuário),nível,reputação,saldo_da_carteira_nxc(Campo Decimal),assinante_is_pro(CampoBooleano).
Jornada:(ForeignKey -> Usuário),título,nome_do_jogo,criado_em.
Fase:(ForeignKey -> Jornada),contente(Campo de texto),url_de_mídia(Campo de imagem/Campo de arquivo),ordem.
Contribuição:(ForeignKey -> Fase),(ForeignKey -> Usuário),tipo_de_conteúdo(ex: "Dica", "Fanart"),contente.
Endpoints da API (REST):
/auth/registro/,/autenticação/login/(retorna token JWT).
/usuários/{id}/perfil/(PEGAR, COLOCAR).
/jornadas/(PUBLICAR, RECEBER).
/jornadas/{id}/(OBTER, COLOCAR, EXCLUIR).
/jornadas/{id}/fases/(PUBLICAR).
Fase 2: Implementação do Motor Econômico
Modelo de Dados:
Modelo de Item:nome,raridade(CharField com opções),estatísticas básicas(Campo JSON),ícone_url. Este é o blueprint de todos os itens.
Instância do item:(Chave Estrangeira -> ItemTemplate),(ForeignKey -> Perfil/Proprietário),id_único,estatísticas atuais(Campo JSON),está listado no mercado (BooleanField). Este é um item real que um jogador possui.
Listagem de mercado:(Chave Estrangeira -> Instância do Item),(ForeignKey -> Perfil/Vendedor),preço(Campo Decimal),listado_em.
Transação:(ForeignKey -> Perfil/Comprador),(ForeignKey -> Perfil/Vendedor),(Chave Estrangeira -> ItemTemplate),preço_de_venda,taxa_de_plataforma_cobrada,carimbo de data/hora. Este é o nosso livro-razão.
Pontos finais da API:
/mercado/listagens/ (GET, POST). POST requer um Instância do itempertencente ao usuário autenticado.
/mercado/listagens/{id}/comprar/ (POST). Este é o endpoint mais crítico. Deve executar a transação de forma atômica:
Verificar se o comprador tem NXC suficiente.
Debitarpreço da carteira do comprador.
Calculartaxa de plataforma(5% fazempreço).
Creditar preço - taxa_de_plataformana carteira do vendedor.
Transferir a propriedade doInstância do itempara o comprador.
Marcar o Listagem de mercadocomo concluído.
Criar um registro de Transação.
Se qualquer passo falhar, reverter toda a operação.
/pagamentos/iniciar/ (POST): Integração com um gateway de pagamento (Stripe, etc.) para comprar NXC com dinheiro real.
Fase 3: Desenvolvimento e Integração do "Nexus Void"
Modelo de Dados:
Personagem do Jogador:(OneToOneField -> Perfil),cavalos de potência,força,destreza,agilidade,furtividade,sorte.
Lógica de Jogo no Backend:
Implemente os algoritmos decálculo de drop de itensesimulação de combate PvPbaseados nos protótipos em Python.
Crie uma lógica de geração procedural de masmorras simples.
Endpoints da API e Comunicação em Tempo Real:
/jogo/iniciar_masmorra/ (POST): Inicia uma nova sessão de jogo.
/jogo/ação/ (POST via WebSocket): Envia ações do jogador (movimento, ataque). O servidor processa a ação, atualiza o estado do jogo e retorna o resultado a todos os clientes relevantes.
Quando um item raro é dropado, o backend deve criar uma nova Instância do iteme associá-la ao perfil do jogador.
Fase 4: Ativação do Sistema de Conflito
Modelo de Dados:
Adicionar aoPerfil:pontos_de_infâmia(Campo Inteiro),é_pk(BooleanField, derivado depontos_de_infâmia > 0).
Adicionar aoPersonagem do Jogador:pool de recompensas(Campo Decimal).
Lógica de Jogo no Backend:
Modifique a lógica de combate: se um combate ocorre em uma "Zona de Disputa" e o perdedor não é PK, o vencedor ganha pontos_de_infâmia.
Implemente a lógica do "Imposto da Alma": uma porcentagem dos NXC do perdedor é transferida para o pool de recompensasdo agressor.
Se o vencedor derrota um PK, o pool de recompensas do PK é transferido para a carteira do vencedor e a infâmia do perdedor é zerada.
Pontos finais da API:
/recompensas/(GET): Retorna uma lista de todos os jogadores PK ativos e suas recompensas, para ser exibida no "Quadro de Procurados".
Executar.
