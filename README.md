# Laborat-rio-IA

Projeto, criar um modelo de previsão com seus devidos pontos de extremidades configurados.

Para isso criei e configurei a conta gratuita no azure (https://portal.azure.com) seguindo a documentação do curso.

1 Criei meu workspace com as seguintes informações:

    Grupo de recursos: labIA
    Nome : Laboratórioia900.
    Região: East US.
    Conta de armazenamento: laboratorioia93070201461.
    Insights do aplicativo: laboratorioia94762566361.

2 Depois selecionei Revisão + Criar
3 Selecinar Estudio de inicialização e dentro do estúdio Machine Learning do Azure encontrei a area recém criada "laboratorioia900"

Com o workspace criado é chegada a hora de configurar.

1 Ir em ML automatizada e inserir as configurações básicas:

Configurações básicas :

    Nome de trabalho : mslearn-bike-automl
    Novo nome de experimento : mslearn-bike-rental
    Descrição: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
    Tags : Nenhum

Tipo & dados da tarefa:

    Selecione o tipo de tarefa : Regressão
    Selecione o conjunto de dados: Crie um novo conjunto de dados com as seguintes configurações:
        Tipo de dados:
            Nome : aluguel de bicicletas
            Descrição : Dados históricos de aluguer de bicicletas
            Tipo : Tabular
        Fonte de dados:
            Selecione a partir de arquivos web
        URL da Web:
            URL da Web:https://aka.ms/bike-rentals
            Ignorar a validação de dados : não selecione
        Configurações:
            Formato de arquivo : Delimitado
            Delimitador : Coma
            Encoding : UTF-8 (versão para o usuário)
            Cabeçadores de coluna : Apenas primeiro arquivo tem cabeçalhos
            Ignorar as linhas : Nenhum
            Dataset contém dados multi-linha : não selecione
        Schema ('):
            Inclua todas as colunas que não sejam Caminho
            Revise os tipos detectados automaticamente

    Selecione Criar. Após a criação do conjunto de dados, selecione o conjunto de dados de aluguel de bicicletas para continuar a enviar o trabalho ML Automatizado.

Configurações de tarefa :

    Tipo de tarefa : Regressão
    Dataset : aluguel de bicicletas
    Coluna alvo : Locação (ingerto)
    Configurações adicionais de configuração:
    Mémétrica primária : Erro quadrado médio de raiz normalizado
    Explique o melhor modelo: Não selecionado
    Use todos os modelos suportados: Não selecionado. Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.
    Modelos permitidos: Selecione apenas RandomForest e LightGBM - normalmente você gostaria de tentar o maior número possível, mas cada modelo adicionado aumenta o tempo que leva para executar o trabalho.
    Limites:Expanda esta seção
    Testes máximos: 3
    Ensaios simultâneos máximos: 3
    O símdia de Max : 3
    Limite de pontuação métrica: 0,085 (para que, se um modelo atingir uma raiz normalizada escore métrico de erro quadrado médio de 0,085 ou menos, o trabalho termina.)
    Tempo limite : 15
    Tempo limite de iteração : 15
    Habilite a rescisão antecipada : Selecionado
    Validação e teste:
    Tipo de validação : Divisões de validação de trem
    Percentagem de dados de validação: 10
    Testar o conjunto de dados : Nenhum

Compute :

    Selecione o tipo de computação : Serverless
    Tipo de máquina virtual: CPU
    Nível da máquina virtual: Dedicado
    Tamanho da máquina virtual : Standard_DS3_V2
    Número de instâncias: 1

Agora vamos rever o modelo:

Quando o trabalho automatizado de aprendizado de máquina for concluído, você pode revisar o melhor modelo treinado.

    Na guia Visão geral do trabalho automatizado de aprendizado de máquina, observe o melhor resumo do modelo. Screenshot of the best model summary of the automated machine learning job with a box around the algorithm name.

        Nota Você pode ver uma mensagem sob o status “Aviso: pontuação de saída especificada pelo usuário alcançada...”. Esta é uma mensagem esperada. Por favor, continuem para o próximo passo.

    Selecione o texto em Nome do Algoritmo para o melhor modelo para visualizar seus detalhes.

    Selecione a guia Métricas e selecione os gráficos de resíduos e previstos_true se ainda não estiverem selecionados.

    Revise os gráficos que mostram o desempenho do modelo. O gráfico de resíduos mostra os resíduos (as diferenças entre os valores previstos e reais) como um histograma. O gráfico_true previsto compara os valores previstos em relação aos valores reais.

Implantar e testar o modelo

    Sobre oModelo de Modeltabulação para o melhor modelo treinado pelo seu trabalho automatizado de aprendizado de máquina, selecioneDeploy (Deploy)e usar oServiço da Webopção para implantar o modelo com as seguintes configurações:
        Nome : preditual-rentals
        Descrição : Predict cycle rentals
        Tipo de computação : Instância do recipiente do Azure
        Habilitação de autenticação : Selecionado
    Aguarde o início da implantação - isso pode levar alguns segundos. O status de Implantação para o endpoint preditor-alugis será indicado na parte principal da página como Executando.
    Aguarde que o status de Implantação mude para Sucedido. Isso pode levar de 5 a 10 minutos.

Teste o serviço implantado

Agora você pode testar o seu serviço implantado.

    No Azure Machine Learning studio, no menu esquerdo, selecione Endpoints e abra o ponto de extremidade preditor-augur em tempo real.

    Na página de ponto de extremidade preditual-rentals em tempo real, visualize a guia Teste.

    Nos dados de entrada para testar o painel de endpoint, substitua o modelo JSON pelos seguintes dados de entrada:
    Código do código

 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }

Clique no botão Testar.

Analse os resultados do teste, que incluem um número previsto de locações com base nos recursos de entrada - semelhante a isso:
Código do código

     {
       "Results": [
         444.27799000000000
       ]
     }

    O painel de teste levou os dados de entrada e usou o modelo que você treinou para retornar o número previsto de aluguéis.

Vamos rever o que você fez. Você usou um conjunto de dados de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo prevê o número de aluguel de bicicletas esperados em um determinado dia, com base em características sazonais e meteorológicas.





