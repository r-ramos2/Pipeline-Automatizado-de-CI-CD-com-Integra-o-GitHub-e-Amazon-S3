# Pipeline Automatizado de CI/CD com Integração GitHub e Amazon S3

## Visão Geral
Este guia fornece os passos para configurar um **Pipeline de Integração Contínua/Implantação Contínua (CI/CD)** utilizando GitHub e Amazon S3. Ao seguir estes passos, poderá automatizar o processo de implantação da sua aplicação, garantindo que ela esteja sempre atualizada com as últimas alterações no código. Este pipeline ajudará a criar uma aplicação segura, escalável e full-stack capaz de lidar com altos níveis de tráfego.

## Configuração do Repositório GitHub
Esta seção orienta sobre o processo de configuração de um repositório GitHub que será a fonte para o pipeline de CI/CD.

1. Crie uma conta no GitHub.
2. Crie um novo repositório onde o projeto será armazenado.
3. Carregue todos os arquivos do projeto.

## Configuração do Bucket S3
Esta seção fornece os passos para configurar um bucket Amazon S3. Este bucket servirá como o ambiente de implantação para sua aplicação.

1. Abra o S3 em uma aba separada.
2. Clique em "Create Bucket".
3. Insira as seguintes informações: Bucket Name="qualquer nome único globalmente ou MyFirstApp123456", AWS Region="escolha a mais próxima do país onde será utilizado ou escolha US-East-1 como padrão". **Nota**: O nome do bucket deve ser único globalmente entre todos os nomes de buckets existentes no Amazon S3.
4. Em "Block public access (bucket settings)", desmarque todas as opções. **Nota**: Desmarcar estas opções tornará seu bucket acessível publicamente. Certifique-se de que deseja fazer isso.
5. Deixe as configurações padrão e clique em "Create bucket".
6. Em "Buckets", escolha "Properties".
7. Em "Static website hosting", clique em "Edit".
8. Em "Static website hosting", escolha "Enable".
9. Em "Hosting type", escolha "Host static website".
10. Em "Index", digite "index.html".
11. Deixe as configurações padrão e clique em "Save changes".
12. Em "Buckets", escolha "Permission".
13. Em "Bucket Policy", clique em "Edit".
14. Insira a política JSON fornecida neste projeto. Se não tiver a política JSON, pode encontrá-la [aqui](https://github.com/r-ramos2/Pipeline-Automatizado-de-CI-CD-con-Integraci-n-de-GitHub-y-Amazon-S3/blob/main/s3_public_read_policy.json).
15. Substitua "arn:aws:s3:::Bucket-Name/*" pelo ARN do Bucket na parte superior da tela. **Nota**: O ARN do Bucket é um identificador único para seu bucket.
16. Clique em "Save changes".

## Configuração do CodePipeline
Esta seção é o núcleo do pipeline de CI/CD. Ela orienta sobre o processo de configuração de um pipeline no AWS CodePipeline, que automatiza o processo de implantação do seu repositório GitHub para seu bucket Amazon S3.

1. Abra o CodePipeline em uma aba separada.
2. Clique em "Create pipeline".
3. Insira as seguintes informações: Pipeline name="qualquer nome ou MyPipeline123456", Pipeline type="V1", Role name="inclua detalhes como qual serviço AWS está utilizando, região AWS e nome do projeto original". **Nota**: A definição de pipeline do tipo V1 contém os parâmetros padrão de pipeline, estágio e nível de ação. A definição de pipeline do tipo V2 estende a definição para adicionar seções adicionais de configurações, como gatilhos e variáveis.
4. Marque a caixa "Allows AWS CodePipeline to create a service role so it can be used with this new service", depois clique em "Next".
5. Em "Source provider", escolha "GitHub (Version 2)".
6. Em "Connection", clique em "Connect to GitHub".
7. Em "Create GitHub App connection", insira Connection name="qualquer nome ou MyPipeline123456".
8. Clique em "Connect to GitHub", depois clique em "Install a new app".
9. Na tela do GitHub, role para baixo até "Repository access".
10. Selecione o repositório para conceder acesso, depois clique em "Save" e em "Connect".
11. Em "Connection", clique na conexão do GitHub que acabou de criar.
12. Em "Repository name", escolha o bucket S3 correspondente.
13. Em "Pipeline trigger", escolha "Push in a branch".
14. Em "Branch name", escolha "main".
15. Em "Output artifact format", escolha "CodePipeline default", depois clique em "Next".
16. Em "Build - optional", escolha conforme suas necessidades ou "Skip build stage", depois clique em "Next".
17. Insira as seguintes informações: Deploy provider=Amazon S3, Region=sua região atual, Bucket=seu bucket S3, S3 object key=clique em "Extract file before deploy", depois clique em "Next" e em "Create pipeline".
18. Aguarde até a tela mostrar marcas verdes para Source e Deploy.

## Fase de Testes
Esta seção orienta sobre o processo de testar seu pipeline de CI/CD.

1. Abra o S3 em uma aba separada.
2. Em "Buckets", escolha "Properties".
3. Em "Static website hosting", clique na URL abaixo de "Bucket website endpoint".
4. Abra o repositório GitHub com seu projeto, faça uma alteração e salve.
5. Abra o CodePipeline e verifique se a alteração foi detectada.
6. Atualize a URL abaixo de "Bucket website endpoint" e veja a alteração.

## Limpeza
Para evitar custos desnecessários, verifique se não há recursos subjacentes ainda em execução.

Veja como pode esvaziar e encerrar o CodePipeline:
1. Navegue até o painel do CodePipeline.
2. Clique no pipeline que deseja excluir.
3. Clique no botão "Delete" para remover o pipeline.

Veja como pode esvaziar e encerrar o bucket S3:
1. Navegue até o painel do S3.
2. Clique no bucket que deseja excluir.
3. Clique no botão "Empty" para remover todos os objetos do bucket.
4. Uma vez que o bucket estiver vazio, clique no botão "Delete" para remover o bucket.
**Nota**: Pode ser necessário excluir a política do bucket antes de poder excluir o bucket.

## Conclusão
Neste projeto, demonstramos como implantar um pipeline de Integração Contínua/Implantação Contínua (CI/CD) usando GitHub e Amazon S3. Seguindo estas etapas, criamos um pipeline de CI/CD totalmente funcional que implanta uma aplicação em um bucket Amazon S3 sempre que houver alterações enviadas para o repositório GitHub. Não hesite em enviar sugestões de melhorias de código.

## Agradecimento
Este tutorial é adaptado do [Tutorial: Create a pipeline that uses Amazon S3 as a deployment provider website](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-s3deploy.html) fornecido pela Amazon Web Services. Estendemos nosso agradecimento à AWS por fornecer este recurso valioso, que serviu como base para o tutorial "Pipeline Automatizado de CI/CD com Integração GitHub e Amazon S3".
