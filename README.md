# Instruções para Criar uma Pipeline no Azure DevOps

## 1. Crie um Projeto no Azure DevOps
- Selecione o modo *AGILE*


---


## 2. Importe o Repositório do GitHub  
1. No menu lateral, acesse **Repos**  
2. Clique em **Import a repository**.  
3. Na seção **Clone URL**, cole o link abaixo e clique em **Import**:  
```
https://github.com/CarlosEduardo7700/BeautyTech_API.git
```


---


## 3. Selecione a Branch *MAIN*
1. Altere o arquivo que está no diretório */src/main/resources* com o nome *application.properties* e atualize o seguinte trecho de código
```
spring.datasource.url=jdbc:oracle:thin:@oracle.fiap.com.br:1521:ORCL
spring.datasource.username=rm552164
spring.datasource.password=100105
```
2. Altere a versão do java-version no pom.xml para o 17
```
<java.version>17</java.version>
```
3. Salve essas alterações na branch MAIN


---


## 4. Configure a Pipeline  
1. Selecione o Editor Clássico para criar a pipeline com YAML
2. Selecione o repositório importado e a branch *MAIN*
3. Selecione a opção MAVEN
4. Coloque o nome da Pipeline e a versão do sistema que será utilizado (windows-latest)
5. Clique em AGENT JOB 1 e altere o nome para *Build*
6. Clique em Maven pom.xml e altere o nome para *Maven*
7. Coloque o nome *Testes de Código* no titutlo do JUnit Tests Results
8. Na aba ADVANCED, altere o JDK para a versão 17
9. Clique em Copy Files e altere o nome para *Copiar Arquivos*
10. Modifique na aba Control Options para a primeira opção *Only when all previous tasks have succeeded*
11. Clique em Publish e altere o nome para *Publicar Artefatos*
12. Modifique o nome do artefato para *API* e troque a opção no Control Options para a primeira opção *Only when all previous tasks have succeeded*
13. Agora na aba OPTIONS da Pipeline, troque o Build Job para *Current Project*
14. Salve na opção *SAVE & QUEUE*
15. Adicione o comentário *Criando a primeira CI BeautyTech*
16. Aguarde o build


---


## 5. Criando Grupo de Recurso e Plano de Serviço
1. Entre no Portal da Azure
2. Abra o Cloud Shell
3. Selecione a sua conta para realizar os comandos
```
az account list
az account set --subscription <id da sua conta com as aspas>
```
4. Copie e cole o comando abaixo para criar o Grupo de Recurso, Plano de Serviço e o Aplicativo Web
```
rg=rg-beautytech && location=brazilsouth && appServicePlanName=plan-beautytech && webAppName=beautytech && sku=F1 && az group create --name $rg --location $location && az appservice plan create -n $appServicePlanName --location $location -g $rg --sku $sku && az webapp create -g $rg -p $appServicePlanName --runtime "TOMCAT:9.0-java17" -n $webAppName
```
5. Aguarde terminar o processo de criação e vamos criar nossa Release


---


## 6. Criando Release
1. Selecione a opção *Azure App Service deployment*
2. Troque o nome para *Desenvolvimento*
3. Crie um artefato e selecione o build que nós fizemos
4. Clique no raio do artefato criado e habilite o *Continuous deployment trigger*
5. Adicione um filtro e selecione a branch *MAIN*
6. Clique no Raio do Card de Desenvolvimento e habilite o *Artifact Filters*
7. Adicione o artefato criado e selecione a branch *MAIN*
8. Clique em *1 job, 1 task*
9. Selecione sua inscrição da Azure e caso apareça para autorizar, clique para autorizar
10. Selecione o plano de serviço criado anteriormente
11. Clique no *Run on agent* e troque o nome para *Release*
12. Selecione o *Azure Pipelines* e o *windows-latest*
13. Clique em *Deploy Azure App Service* e troque .zip para .jar no campo *Package or folder*
14. Salve e coloque no comentario *Criando CD BeautyTech*
15. Clique em *Create Release* e adicione o comentário *Deploy em Desenvolvimento*
16. Agora entre no plano de serviço no portal da Azure e pegue o endereço para utilizar a sua API


---


### Conclusão
Com esses passos, você criou uma pipeline para a API **BeautyTech** no Azure DevOps, configurando desde a importação do repositório até a execução do processo e utilização.
