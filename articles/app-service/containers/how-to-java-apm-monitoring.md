---
title: Java APM ve izleme araçları Linux'ta - Azure App Service'ı yapılandırma
description: Günlükleri ve ölçümleri NewRelic ve uygulama Dynamics APM sağlayıcıları için App Service Linux üzerinde çalışan Java uygulamalarınız için gönderme hakkında bilgi edinin
services: app-service\web
author: rloutlaw
manager: angerobe
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 03/21/2019
ms.author: astay;routlaw
ms.custom: seodec18
ms.openlocfilehash: e6a22258266bda18c9ff79590d88e70d512f6c77
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60849974"
---
# <a name="how-to-application-performance-monitoring-tools-with-java-apps-on-azure-app-service-on-linux"></a>Nasıl yapılır: Uygulama performansı izleme ile Linux üzerinde Azure App Service'te Java uygulamaları araçları

Bu makalede, Linux üzerinde Azure App Service'te (APM) platformları NewRelic ve AppDynamics uygulama performansı izleme ile dağıtılan Java uygulamalarını bağlamak gösterilmektedir.

## <a name="configure-new-relic"></a>Yeni Relic yapılandırın

1. NewRelic hesabında oluşturma [NewRelic.com](https://newrelic.com/signup)
2. Olan NewRelic Java agent yükleme, benzer bir dosya adı olacaktır `newrelic-java-x.x.x.zip`.
3. Lisans anahtarınızı kopyalayın, buna daha sonra aracıyı yapılandırmak için ihtiyacınız olacak.
4. [App Service örneğinizin içine SSH](/azure/app-service/containers/app-service-linux-ssh-support) ve yeni bir dizin oluşturmak `/home/site/wwwroot/apm`.
5. Paketten çıkarılan NewRelic Java aracı dosyalarını bir dizin altında dosya yükleme `/home/site/wwwroot/apm`. Aracınızı dosyaları olmalıdır `/home/site/wwwroot/apm/newrelic`.
6. YAML dosyasının değiştirme `/home/site/wwwroot/apm/newrelic/newrelic.yml` ve yer tutucu lisans değerini kendi lisans anahtarı ile değiştirin.
7. Azure portalında App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Uygulamanızı kullanıyorsa **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Kullanıyorsanız **WildFly**, New Relic belgelerine bakın [burada](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) JBoss yapılandırma ve Java agent yükleme hakkında yönergeler için.
    - Bir ortam değişkeni için zaten varsa `JAVA_OPTS` veya `CATALINA_OPTS`, ekleme `javaagent` seçeneği sonuna kadar geçerli değeri.

## <a name="configure-appdynamics"></a>AppDynamics yapılandırın

1. AppDynamics hesabınız oluşturma [AppDynamics.com](https://www.appdynamics.com/community/register/)
1. AppDynamics Web sitesinden Java agent yükleme, dosya adı şuna benzer olacaktır. `AppServerAgent-x.x.x.xxxxx.zip`
1. [App Service örneğinizin içine SSH](/azure/app-service/containers/app-service-linux-ssh-support) ve yeni bir dizin oluşturmak `/home/site/wwwroot/apm`.
1. Java aracı dosyalarını bir dizin altında dosya yükleme `/home/site/wwwroot/apm`. Aracınızı dosyaları olmalıdır `/home/site/wwwroot/apm/appdynamics`.
1. Azure portalında App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Kullanıyorsanız **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` burada `<YOUR_SITE_NAME>` App Service adınız.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` burada `<YOUR_SITE_NAME>` App Service adınız.
    - Kullanıyorsanız **WildFly**, AppDynamics belgelerine bakın [burada](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) JBoss yapılandırma ve Java agent yükleme hakkında yönergeler için.
