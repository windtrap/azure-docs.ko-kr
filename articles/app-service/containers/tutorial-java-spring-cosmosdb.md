---
title: Linux에서 Java 웹앱 빌드 - Azure App Service
description: Linux의 Azure App Service 및 Azure Cosmos DB를 사용하여 Spring Boot Java 웹앱을 빌드하고, 배포하고, 크기를 조정합니다.
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.service: app-service-web
ms.devlang: java
ms.topic: tutorial
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 792346edf1d2b2326f7f5f5f53304ceca347508d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57901120"
---
# <a name="tutorial-build-a-java-web-app-using-spring-and-azure-cosmos-db"></a>자습서: Spring 및 Azure Cosmos DB를 사용하여 Java 웹앱 빌드

이 자습서에서는 Azure에서 Java 웹앱을 빌드하고, 구성하고, 배포하고, 크기를 조정하는 프로세스를 단계별로 설명합니다. 작업이 완료되면 [Linux의 Azure App Service](/azure/app-service/containers)에서 실행되는 [Azure Cosmos DB](/azure/cosmos-db)에 [Spring Boot](https://projects.spring.io/spring-boot/) 애플리케이션 저장 데이터가 생성됩니다.

![Azure App Service에서 실행 중인 Java 앱](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * Cosmos DB 데이터베이스 만들기
> * 데이터베이스에 샘플 앱 연결 및 로컬로 테스트
> * Azure에 샘플 앱 배포
> * App Service에서 진단 로그 스트림
> * 추가 인스턴스를 추가하여 샘플 앱 확장

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>필수 조건

* 고유한 컴퓨터에 설치된 [Azure CLI](https://docs.microsoft.com/cli/azure/overview) 
* [Git](https://git-scm.com/)
* [Java JDK](https://aka.ms/azure-jdks)
* [Maven](https://maven.apache.org)

## <a name="clone-the-sample-todo-app-and-prepare-the-repo"></a>샘플 TODO 앱 복제 및 리포지토리 준비

이 자습서에서는 [Spring 데이터 Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb)에서 지원되는 Spring REST API를 호출하는 웹 UI에서 샘플 할 일 목록 앱을 사용합니다. 앱의 코드는 [GitHub](https://github.com/Microsoft/spring-todo-app)에서 사용할 수 있습니다. Spring 및 Cosmos DB를 사용하여 Java 앱을 작성하는 방법에 대한 자세한 내용은 [Azure Cosmos DB SQL API를 사용한 Spring Boot Starter 자습서](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db ) 및 [Spring 데이터 Azure Cosmos DB 빠른 시작](https://github.com/Microsoft/spring-data-cosmosdb#quick-start)을 참조하세요.


터미널에서 다음 명령을 실행하여 샘플 리포지토리를 복제하고 샘플 앱 환경을 설정합니다.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
cd e2e-java-experience-in-app-service-linux-part-2
yes | cp -rf .prep/* .
```

## <a name="create-an-azure-cosmos-db"></a>Azure Cosmos DB 만들기

구독에서 Azure Cosmos DB 데이터베이스를 만들려면 다음 단계를 수행합니다. 할 일 목록 앱을 실행하는 경우 이 데이터베이스에 연결되고 데이터를 저장하여 애플리케이션을 실행하는 위치에 상관없이 애플리케이션 상태를 유지합니다.

1. Azure CLI에 로그인하고 둘 이상의 로그인 자격 증명에 연결된 경우 필요에 따라 구독을 설정합니다.

    ```bash
    az login
    az account set -s <your-subscription-id>
    ```   

2. Azure 리소스 그룹을 만들고 리소스 그룹 이름을 입력합니다.

    ```bash
    az group create -n <your-azure-group-name> \
        -l <your-resource-group-region>
    ```

3. `GlobalDocumentDB` 종류로 Azure Cosmos DB를 만듭니다. Cosmos DB의 이름에는 대/소문자만 사용해야 합니다. 명령의 응답에서 `documentEndpoint` 필드를 입력합니다.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
    ```

4. Azure Cosmos DB 키를 가져와서 앱에 연결합니다. 다음 단계에서 필요하므로 `primaryMasterKey`, `documentEndpoint`를 가까운 위치에 유지합니다.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="configure-the-todo-app-properties"></a>TODO 앱 속성 구성

컴퓨터에서 터미널을 엽니다. 복제된 리포지토리에서 샘플 스크립트 파일을 복사하여 방금 만든 Cosmos DB 데이터베이스에 대해 사용자 지정할 수 있습니다.

```bash
cd initial/spring-todo-app
cp set-env-variables-template.sh .scripts/set-env-variables.sh
```
 
즐겨찾는 편집기에서 `.scripts/set-env-variables.sh`를 편집하고 Azure Cosmos DB 연결 정보를 입력합니다. App Service Linux 구성의 경우 이전과 동일한 지역(`your-resource-group-region`) 및 Cosmos DB 데이터베이스를 만들 때 사용된 리소스 그룹(`your-azure-group-name`)을 사용합니다. Azure 배포에서 웹앱 이름이 중복될 수 없으므로 고유한 WEBAPP_NAME을 선택합니다.

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>

# App Service Linux Configuration
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

그런 다음, 스크립트를 실행합니다.

```bash
source .scripts/set-env-variables.sh
```
   
이러한 환경 변수는 할 일 목록 앱의 `application.properties`에서 사용됩니다. 속성 파일의 필드는 Spring 데이터에 대한 기본 리포지토리 구성을 설정합니다.

```properties
azure.cosmosdb.uri=${COSMOSDB_URI}
azure.cosmosdb.key=${COSMOSDB_KEY}
azure.cosmosdb.database=${COSMOSDB_DBNAME}
```

```java
@Repository
public interface TodoItemRepository extends DocumentDbRepository<TodoItem, String> {
}
```

그런 다음, 샘플 앱은 `com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document`에서 가져온 `@Document` 주석을 사용하여 Cosmos DB에서 저장되고 관리될 엔터티 형식을 설정합니다.

```java
@Document
public class TodoItem {
    private String id;
    private String description;
    private String owner;
    private boolean finished;
```

## <a name="run-the-sample-app"></a>샘플 앱 실행

Maven을 사용하여 샘플을 실행합니다.

```bash
mvn package spring-boot:run
```

출력은 다음과 비슷합니다.

```bash
bash-3.2$ mvn package spring-boot:run
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 


[INFO] SimpleUrlHandlerMapping - Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] SimpleUrlHandlerMapping - Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] WelcomePageHandlerMapping - Adding welcome page: class path resource [static/index.html]
2018-10-28 15:04:32.101  INFO 7673 --- [           main] c.m.azure.documentdb.DocumentClient      : Initializing DocumentClient with serviceEndpoint [https://sample-cosmos-db-westus.documents.azure.com:443/], ConnectionPolicy [ConnectionPolicy [requestTimeout=60, mediaRequestTimeout=300, connectionMode=Gateway, mediaReadMode=Buffered, maxPoolSize=800, idleConnectionTimeout=60, userAgentSuffix=;spring-data/2.0.6;098063be661ab767976bd5a2ec350e978faba99348207e8627375e8033277cb2, retryOptions=com.microsoft.azure.documentdb.RetryOptions@6b9fb84d, enableEndpointDiscovery=true, preferredLocations=null]], ConsistencyLevel [null]
[INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
[INFO] TomcatWebServer - Tomcat started on port(s): 8080 (http) with context path ''
[INFO] TodoApplication - Started TodoApplication in 45.573 seconds (JVM running for 76.534)
```

앱이 시작되면 다음 링크를 사용하여 로컬로 Spring 할 일 앱에 액세스할 수 있습니다. [http://localhost:8080/](http://localhost:8080/)

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

“Started TodoApplication” 메시지 대신 예외가 표시되는 경우 이전 단계에서 `bash` 스크립트가 환경 변수를 제대로 내보냈는지, 생성한 Azure Cosmos DB 데이터베이스에 대한 값이 올바른지 확인합니다.

## <a name="configure-azure-deployment"></a>Azure 배포 구성

`initial/spring-boot-todo` 디렉터리에서 `pom.xml` 파일을 열고 다음 [Azure App Service의 Maven 플러그인](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) 구성을 추가합니다.

```xml    
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.4.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

## <a name="deploy-to-app-service-on-linux"></a>Linux의 App Service에 배포

`azure-webapp:deploy` Maven 목표를 사용하여 Linux의 Azure App Service에 할 일 앱을 배포합니다.

```bash

# Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.4.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesn't exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlanb6ba8178-5bbb-49e7'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

출력에는 배포된 애플리케이션의 URL이 포함됩니다(이 예제에서는 `https://spring-todo-app.azurewebsites.net`). 웹 브라우저에 이 URL을 복사하거나 터미널 창에서 다음 명령을 실행하여 앱을 로드할 수 있습니다.

```bash
open https://spring-todo-app.azurewebsites.net
```

주소 표시줄에는 원격 URL에서 실행 중인 앱이 표시됩니다.

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-in-app-service.jpg)

## <a name="view-logs-to-troubleshoot-the-app"></a>로그를 보고 앱 문제 해결

Linux의 App Service에서 배포된 Java 웹앱에 로깅을 사용하도록 설정합니다.

```bash
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
  --web-server-logging filesystem
```

그런 다음, 터미널에 웹앱 로그를 스트림합니다.

```bash
az webapp log tail --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME}
```

출력의 최신 줄을 확인하고 TODO 앱에 대한 새 요청이 만들어지면 콘솔에서 스트림하겠습니다. 콘솔을 종료하려면 Ctl+C 키를 사용합니다.

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: https://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers' needs non existent service `dev'
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======
```

## <a name="scale-out-the-todo-app"></a>할 일 앱 확장

다른 작업자를 추가하여 애플리케이션을 확장합니다.

```bash
az appservice plan update --number-of-workers 2 \
   --name ${WEBAPP_PLAN_NAME} \
   --resource-group <your-azure-group-name>
```

## <a name="clean-up-resources"></a>리소스 정리

다른 자습서에서 이러한 리소스가 필요하지 않으면([다음 단계](#next) 참조) Cloud Shell에서 다음 명령을 실행하여 삭제할 수 있습니다. 
  
```bash
az group delete --name your-azure-group-name
```

<a name="next"></a>

## <a name="next-steps"></a>다음 단계

[Java 개발자용 Azure](/java/azure/)
[Spring Boot](https://spring.io/projects/spring-boot), [Cosmos DB용 Spring Data](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable), [Azure Cosmos DB](/azure/cosmos-db/sql-api-introduction) 및 [App Service Linux](/azure/app-service/containers/app-service-linux-intro)

개발자 가이드에서 App Service on Linux의 Java 앱을 실행하는 방법에 대해 자세히 알아봅니다.

> [!div class="nextstepaction"] 
> [App Service Linux 개발자 가이드의 Java](/azure/app-service/containers/app-service-linux-java)
