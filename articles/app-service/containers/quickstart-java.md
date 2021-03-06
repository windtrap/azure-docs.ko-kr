---
title: Linux에서 Java 웹앱 만들기 - Azure App Service
description: 이 빠른 시작에서는 몇 분 안에 Linux의 Azure App Service에서 첫 번째 Java Hello World를 배포합니다.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 12/10/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: 4b95c75b863cdc86dd8a1ebe8347b77b637b110c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57999108"
---
# <a name="quickstart-create-a-java-app-in-app-service-on-linux"></a>빠른 시작: Linux의 App Service에서 Java 앱 만들기

[Linux의 App Service](app-service-linux-intro.md)는 Linux 운영 체제를 사용하여 확장성이 높은 자체 패치 웹 호스팅 서비스를 제공합니다. 이 빠른 시작에서는 [Azure Web Apps용 Maven 플러그 인(미리 보기)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)에서 [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)를 사용하여 Java WAR(Web Archive) 파일을 배포하는 방법을 보여줍니다.

![Azure에서 실행되는 샘플 앱](media/quickstart-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java 앱 만들기

Cloud Shell 프롬프트에서 다음 Maven 명령을 실행하여 `helloworld`라는 새 앱을 만듭니다.

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Maven 플러그 인 구성

Maven에서 배포하려면 Cloud Shell의 코드 편집기를 사용하여 `helloworld` 디렉터리의 `pom.xml` 프로젝트 파일을 엽니다. 

```bash
code pom.xml
```

그런 다음, `pom.xml` 파일의 `<build>` 요소 내에 다음 플러그 인 정의를 추가합니다.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Linux           -->
    <!--*************************************************-->
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.5.3</version>
        <configuration>
            <!-- App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
   
            <!-- Java Runtime Stack for App on Linux-->
            <linuxRuntime>tomcat 8.5-jre8</linuxRuntime> 
        </configuration>
    </plugin>
</plugins>
```    


> [!NOTE] 
> 이 문서에서는 WAR 파일에 패키지된 Java 앱만 사용합니다. 플러그 인에서 JAR 웹 애플리케이션도 지원됩니다. [Linux 기반 App Service에 Java SE JAR 파일 배포](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)를 방문하여 직접 확인해 보세요.


플러그인 구성에서 다음 자리 표시자를 업데이트합니다.

| Placeholder | 설명 |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | 앱을 만들 새 리소스 그룹의 이름입니다. 앱의 모든 리소스를 한 그룹에 배치하여 다 함께 관리할 수 있습니다. 예를 들어 리소스 그룹을 삭제하면 앱과 연결된 모든 리소스가 삭제됩니다. 이 값을 고유한 새 리소스 그룹(예: *TestResources*)으로 업데이트합니다. 이 리소스 그룹 이름을 사용하여 이후 섹션에서 모든 Azure 리소스를 정리합니다. |
| `WEBAPP_NAME` | 앱 이름은 Azure(WEBAPP_NAME.azurewebsites.net)에 배포할 때 앱에 대한 호스트 이름의 일부가 됩니다. 이 값을 Java 앱을 호스팅할 새 App Service 앱의 고유 이름(예: *contoso*)으로 업데이트합니다. |
| `REGION` | 앱이 호스팅되는 Azure 지역입니다(예: `westus2`). `az account list-locations` 명령을 사용하여 Cloud Shell 또는 CLI에서 지역 목록을 가져올 수 있습니다. |

## <a name="deploy-the-app"></a>앱 배포

다음 명령을 사용하여 Java 앱을 Azure에 배포합니다.

```bash
mvn package azure-webapp:deploy
```

배포가 완료되면 웹 브라우저에서 다음 URL을 사용하여 배포된 애플리케이션으로 이동합니다(예: `http://<webapp>.azurewebsites.net/helloworld`). 

![Azure에서 실행되는 샘플 앱](media/quickstart-java/java-hello-world-in-browser-curl.png)

**축하합니다.** Linux의 App Service에 첫 번째 Java 앱을 배포했습니다.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 Maven을 사용하여 Java 앱을 만들고, [Azure Web Apps용 Maven 플러그 인](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)을 구성한 다음, 웹 보관용으로 패키지된 Java 앱을 Linux의 App Service에 배포했습니다. Linux의 App Serivce에서 Java 애플리케이션을 호스트하는 방법에 대한 자세한 내용은 다음 자습서 및 방법 문서를 참조하세요.

- [자습서: PostgreSQL을 사용하여 Java Enterprise 앱 배포](tutorial-java-enterprise-postgresql-app.md)
- [Tomcat 데이터 원본 구성](app-service-linux-java.md#tomcat)
- [Jenkins를 사용하는 CI/CD](/azure/jenkins/deploy-jenkins-app-service-plugin)
- [애플리케이션 성능 모니터링 도구 설정](how-to-java-apm-monitoring.md)
- [Linux 기반의 App Service에 대한 Java 개발자 가이드](app-service-linux-java.md)

