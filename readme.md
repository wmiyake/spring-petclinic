# Spring Pet Clinic and the Developer Sandbox for Red Hat OpenShift

Este repositório contém uma implementação pronta para contêiner do icônico aplicativo Spring Petclinic. Especificamente, este código é útil com a tecnologia OpenShift Source-to-Image (s2i) e faz parte do material introdutório para [Developer Sandbox for Red Hat OpenShift](https://developers.redhat.com/developer-sandbox).

## Implementação OpenShift
Obtenha seu cluster OpenShift *gratuito* para executar esta demonstração. Você pode obter acesso gratuito ao Developer Sandbox para Red Hat OpenShift em: https://developers.redhat.com/developer-sandbox


### Dev Console

Certifique-se de estar na perspectiva do desenvolvedor:

![Dev Perspective](images/3-switch-perspective.png)  

O primeiro passo é criar um banco de dados para armazenar nossos dados. Usaremos uma imagem MySQL para fazer isso.

Clique no botão `+Adicionar` e escolha a opção `Imagens do contêiner`:

![Container images option](images/container_images_option.png)

Insira a imagem MySQL que precisamos, `docker.io/mysql:5.7.41`


![MySQL image entry](images/deploy_image_mysql.png)  

Role até a parte inferior da página e selecione a opção para acessar opções avançadas para uma implantação:

![deployment options](images/advanced%20options%20deployment.png)

Usando a seção Variáveis ​​de ambiente, especifique os valores de banco de dados e de autenticação adicionando as quatro variáveis ​​de ambiente a seguir. 

![Environment variables](images/deploy_environment_variables.png)

Quando eles forem inseridos, clique no link "Tipo de recurso". Selecione "Implantação" como o tipo de recurso:


![MySQL Ephemeral](images/deploy_resource_type.png)  

Clique no botão `Criar`.

Após alguns minutos, e nos bastidores, um contêiner de banco de dados MySQL será iniciado. Neste ponto, você tem um mecanismo de banco de dados a ser usado pela aplicação. É hora de seguir em frente e criar o aplicativo.

### Deploy Pet Clinic App

Clique no botão `+Adicionar` e escolha o tipo `Importar do Git`:

Preencha o repositório git com o seguinte valor `https://github.com/redhat-developer-demos/spring-petclinic`. É onde as coisas começam a ficar interessantes.

Quando você insere a URL de um repositório git, o OpenShift examinará os arquivos do repositório e tentará discernir a melhor maneira de construir o aplicativo. Existem três resultados possíveis:
1. Construa usando o arquivo "devfile.yaml" encontrado no código-fonte
1. Construa usando o arquivo “dockerfile” encontrado no código-fonte
1. Se nenhum dos dois arquivos acima for encontrado, construa usando uma imagem do construtor e a tecnologia integrada de fonte para imagem (s2i). Este é o foco deste artigo.

Se, no caso deste exemplo (ou seja, Spring Petclinic), o OpenShift **não** escolher a opção de imagem do construtor, você precisará direcioná-lo para fazer isso. Veja como fazer isso:

Primeiro, clique no link "Editar estratégia de importação":

![Edit Import Strategy](images/edit_import_strategy.png)  

Em seguida, escolha a opção Builder Image e certifique-se de que o construtor Java esteja selecionado:

![Choose builder image](images/select_builder_image.png)

Role para baixo até a seção Opções avançadas e selecione a opção para alterar o tipo de recurso:

![Select Resource type](images/advanced_options.png)  

Escolha a opção Implantação:  

![Deployment resource type](images/resource_type_deployment.png)  

Perto da parte inferior da página, clique no link `Build Configuration`:

![Build Configuration](images/advanced_options_build_configuration.png)

Adicione as seguintes variáveis ​​de ambiente, conforme exibido na captura de tela a seguir:

```
SPRING_PROFILES_ACTIVE=mysql
MYSQL_URL=jdbc:mysql://mysql:3306/petclinic
```

![DC Env Vars](images/9-app-env-vars.png)

Por fim, clique no botão `Criar` e espere até que a construção seja concluída e o pod esteja instalado e funcionando (azul escuro ao redor do balão de implantação). Ao testar isso usando o Developer Sandbox para Red Hat OpenShift, essa etapa levou aproximadamente seis minutos. Observação: você *pode* ver mensagens de erro no Sandbox. Eles são temporários enquanto o aplicativo é compilado.

Em seguida, pressione o botão Abrir URL para visualizar o aplicativo Pet Clinic:

![Pet Clinic Deployment](images/10-petclinic-url.png)


![Pet Clinic UI](images/11-output-ui.png)

E se você visitar o Terminal de implantação do MySQL, você se conectará ao banco de dados para ver o esquema e os dados

```
mysql -u root -h mysql -p

petclinic

use petclinic;
show tables;
```

![MySQL Terminal](images/12-mysql-terminal-1.png)

```
select * from owners;
```

![MySQL Terminal](images/13-mysql-terminal-2.png)
`### End ###`
