Na última entrega (Dezembro)

O software desktop atuará como uma Interface Homem-Máquina de supervisão energética, sendo responsável por processar os sinais analógicos e digitais recebidos do microcontrolador e permitir a intervenção do operador em tempo real.
1. Processamento e Apresentação de Telemetria (Hardware → Desktop)

O software receberá continuamente as amostras brutas dos sensores do microcontrolador (via comunicação serial no futuro ou simuladas nesta entrega):

    Leitura e Tratamento de Tensão (V) e Corrente (A):

        O software deve ler a tensão eficaz (VRMS) e a corrente (IRMS) enviadas pelo sensor (ex.: ACS712 / ZMPT101B).

    Cálculo da Potência Ativa (P):

        O software calcula a potência instantânea aproximada em Watts (W) ou Quilowatts (kW) através da relação:
        P=V×I

    Renderização no Dashboard:

        Exibir os valores instantâneos de V, I e P em indicadores numéricos de alta visibilidade (QLCDNumber ou QLabel customizadas com destaque de cor).

        Atualizar o Gráfico de Tendência Temporal (PyQtGraph ou Matplotlib) plotando a curva de consumo de potência ao longo do tempo. O gráfico deve ser inicializado com histórico pré-carregado no momento da abertura da tela.

2. Monitoramento de Estado Binário e Segurança (Hardware → Desktop)

O software deve monitorar o estado físico do disjuntor/chave de proteção geral da instalação:

    Sinalização Visual de Estado (Aberto / Fechado):

        Fechado (Energizado / Normal): Exibir um indicador gráfico de status verde (ex.: badge/LED virtual com o texto "Disjuntor: FECHADO / NORMAL").

        Aberto (Desenergizado / Trip/Falha): Exibir o indicador em vermelho (ex.: "Disjuntor: ABERTO / PROTEÇÃO ATIVADA").

    Detecção e Registro de Eventos de Disparo:

        Sempre que o estado mudar espontaneamente no hardware (ex.: disjuntor caiu por sobrecarga), o software deve disparar um aviso visual (QMessageBox.warning) e gerar um registro automático na tabela de histórico.

3. Atuação e Parametrização do Sistema (Desktop → Hardware)

O software fornecerá ao operador meios de intervir na carga e definir parâmetros operacionais:

    Corte Emergencial da Carga (Relé de Saída):

        Disponibilizar um botão de grande destaque ("CORTE DE EMERGÊNCIA" ou "DESLIGAR CARGA").

        Ao ser acionado, o software exibirá uma caixa de confirmação modal (QMessageBox.question). Se confirmado, enviará o comando binário (ex.: RELAY_OFF) para o microcontrolador abrir o relé.

    Ajuste de Limite de Alerta de Consumo (Setpoints):

        O operador poderá definir um limite máximo tolerável de corrente ou potência através de um campo numérico (QSpinBox / QDoubleSpinBox) ou seletor (QSlider).

    Lógica Local de Proteção de Software:

        Se a potência calculada pelo software ultrapassar o limite (setpoint) configurado pelo usuário, o software deve alterar a cor do dashboard para amarelo/vermelho, emitir um alerta visual e registrar a ocorrência de sobrecorrente.

4. Gestão de Histórico e Auditoria (Logs em Tabela)

Toda a atividade do sistema deve ser centralizada na QTableWidget de registros:
Coluna 	Descrição do Dado 	Exemplo de Preenchimento
Data / Hora 	Carimbo de data/hora exato do evento (Timestamp) 	10/08/2026 10:30:15
Tipo de Evento 	Categoria do evento registrado 	Comando, Alerta ou Status
Descrição 	Detalhes do evento ou valor medido na ocorrência 	Corte de emergência acionado via software
Valor Medido 	Potência/Corrente no momento do evento 	12.5 A / 2750 W

    💡 Opção de Tema Livre (Prototipagem de Microcontroladores):

    As equipes que preferirem podem aplicar o software supervisor a um protótipo físico/projeto desenvolvido em disciplinas como Microcontroladores, Microprocessadores ou Sistemas Embarcados, desde que o projeto atenda aos mesmos requisitos de leitura analógica/digital e acionamento de atuadores.

Entrega A1/1

Nesta primeira entrega, a aplicação não precisa estar fisicamente conectada ao hardware (a integração serial ocorrerá na Unidade 4). No entanto, toda a arquitetura de código em camadas (MVC), navegação entre janelas, layout responsivo e interface gráfica devem estar prontos.
Requisitos Visuais e Funcionalidades Obrigatórias

    Dashboard Principal de Monitoramento (Smart Grid):

        Leituras de Telemetria: Indicadores visuais para Tensão (V), Corrente (A) e Potência Calculada (W).

        Indicador de Estado Binário: Sinalizador visual (LED virtual ou badge) indicando o status do Disjuntor/Chave de Proteção (Aberto / Fechado).

        Comandos de Acionamento: Botão de "Corte Emergencial de Carga" (Relé) e controles para ajuste visual do limite de alerta de consumo.

        Gráfico de Tendência (Com Carga Inicial de Dados): Área de plotagem de gráfico (usando pyqtgraph ou matplotlib) exibindo um histórico de consumo (ex.: curva de demanda das últimas 24 horas) preenchido com dados amostraria/iniciais no momento em que a tela abre, para demonstrar a renderização do componente.

    Painel de Configuração da Comunicação Serial:

        Seleção de Porta COM (QComboBox), Baud Rate (9600, 115200), Timeout e botões de "Conectar / Desconectar". (Nesta etapa A1/1, a conexão apenas atualizará o status visual na tela).

    Janela de Configuração de Limites / Parâmetros (QDialog Modal):

        Formulário secundário para cadastro de regras de alerta (ex.: limite máximo de corrente, limite máximo de tensão).

        No mínimo duas regras.

    Histórico de Eventos e Registros (QTableWidget):

        Tabela para listagem de eventos do sistema (ex.: registros de cortes de emergência, ultrapassagem de limite de corrente e trocas de estado do disjuntor).

Padrão de Arquitetura e Organização do Código

    Padrão MVC: Separação estrita de pastas:

        /ui: Arquivos .ui do Qt Designer e telas compiladas .py.

        /controllers: Classes de controle com a lógica das janelas e eventos (Signals & Slots).

        main.py: Script de entrada enxuto que apenas inicializa a aplicação.

    Encapsulamento: Nenhuma regra de negócio ou evento deve ser escrito diretamente dentro dos arquivos gerados pelo Qt Designer.

Forma de entrega:
Forma de Entrega e Apresentação (A1/1)

    Envio do Projeto: Submissão do link do repositório GitHub no Google Forms (clicando aqui) até 17/08/2026.

    Apresentação em Sala: No dia 17/08/2026, no laboratório, as equipes farão uma demonstração prática executando o código clonado do próprio GitHub.

Regras Obrigatórias do Git (Leia com Atenção):

    Evolução do Código: Os commits devem demonstrar a evolução progressiva do projeto ao longo do período de desenvolvimento. Enviar todo o código em um único commit no dia da entrega não será aceito.

    Avaliação Individual: A participação de cada membro é auditada pelo histórico do Git. O aluno que NÃO possuir commits registrados em seu usuário do GitHub receberá nota ZERO na entrega.

Critérios avaliativos:
Categoria 	Critério Detalhado 	Nota Máx.
1. Arquitetura de Software & Padrão MVC 	

• Separação estrita de pastas (/ui, /controllers, /models, main.py).

• Isolação completa dos arquivos compilados pelo Qt Designer (nenhuma regra de negócio dentro da pasta /ui).

• main.py enxuto, funcionando apenas como ponto de partida da aplicação.

• Organização de classes, boas práticas de POO e legibilidade do código em Python.
	2,5 pts
2. Interface Gráfica (UI/UX) & Layouts Responsivos 	

• Utilização de Layouts no Qt Designer para alinhamento dos componentes (evitando elementos sobrepostos ou desalinhados no código).

• Organização visual clara, boa distribuição dos elementos na tela e hierarquia das informações (títulos, campos e botões bem definidos).

• Padronização estética (cores, fontes) e facilidade na navegação entre as janelas.

	2,0 pts
3. Módulo de Telemetria & Gráfico Pré-carregado (Smart Grid) 	

• Presença e funcionamento dos indicadores visuais para Tensão (V), Corrente (A) e Potência Calculada (\( P = V \times I \)).

• Indicador gráfico de status do disjuntor (Aberto / Fechado) com mudança visual de cor/estado.

• Área de gráfico (PyQtGraph ou Matplotlib) inicializada com dados pré-carregados simulando a curva de consumo.
	2,0 pts
4. Componentes Avançados & Múltiplas Janelas 	

• Janela secundária de configuração (QDialog modal) com envio e resgate correto de parâmetros para a tela principal.

• Uso funcional e preenchimento correto de QTableWidget (histórico de eventos), QComboBox, QDateEdit e validações com QMessageBox.
	2,0 pts
5. Governança, Versionamento & Auditoria Git 	

• Repositório ativo e bem estruturado no GitHub/GitLab com arquivo README.md identificando a equipe e o projeto.

• Distribuição equitativa de commits entre todos os integrantes da equipe (auditado pelo histórico do Git).

• Padrão coerente nas mensagens de commit.
	1,5 pt
NOTA TOTAL 	  	10,0 pts