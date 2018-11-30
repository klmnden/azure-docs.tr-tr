---
title: Java APM ve izleme araçları Linux üzerinde Azure App Service ile yapılandırma
description: Günlükleri ve ölçümleri NewRelic ve uygulama Dynamics APM sağlayıcıları için App Service Linux üzerinde çalışan Java uygulamalarınız için gönderme hakkında bilgi edinin
services: app-service\web
author: rloutlaw
manager: angerobe
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 11/27/2018
ms.author: astay;routlaw
ms.openlocfilehash: 06ae71fea1b85a74d87588d2635038c64c8540cc
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52500429"
---
# <a name="how-to-application-performance-monitoring-tools-with-java-apps-on-azure-app-service-on-linux"></a>Nasıl yapılır: uygulama performansı izleme ile Linux üzerinde Azure App Service'te Java uygulamaları araçları

Bu makalede, Linux üzerinde Azure App Service'te (APM) platformları NewRelic ve AppDynamics uygulama performansı izleme ile dağıtılan Java uygulamalarını bağlamak gösterilmektedir.

## <a name="configure-new-relic"></a>Yeni Relic yapılandırın
1. NewRelic hesabında oluşturma [NewRelic.com](https://newrelic.com/signup)
1. Olan NewRelic Java agent yükleme, benzer bir dosya adı olacaktır `newrelic-java-x.x.x.zip`.
1. Lisans anahtarınızı kopyalayın, buna daha sonra aracıyı yapılandırmak için ihtiyacınız olacak.
1. [App Service örneğinizin içine SSH](/azure/app-service/containers/app-service-linux-ssh-support) ve yeni bir dizin oluşturmak `/home/site/wwwroot/apm`. 
1. Paketten çıkarılan NewRelic Java aracı dosyalarını bir dizin altında dosya yükleme `/home/site/wwwroot/apm`. Aracınızı dosyaları olmalıdır `/home/site/wwwroot/apm/newrelic`.
1. YAML dosyasının değiştirme `/home/site/wwwroot/apm/newrelic/newrelic.yml` ve yer tutucu lisans değerini kendi lisans anahtarı ile değiştirin.
1. Azure portalında App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Uygulamanızı kullanıyorsa **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Bir ortam değişkeni için zaten varsa `JAVA_OPTS` veya `CATALINA_OPTS`, ekleme `javaagent` seçeneği sonuna kadar geçerli değeri.

## <a name="configure-appdynamics"></a>AppDynamics yapılandırın
1. AppDynamics hesabınız oluşturma [AppDynamics.com](https://www.appdynamics.com/community/register/)
1. AppDynamics Web sitesinden Java agent yükleme, dosya adı şuna benzer olacaktır. `AppServerAgent-x.x.x.xxxxx.zip`
1. [App Service örneğinizin içine SSH](/azure/app-service/containers/app-service-linux-ssh-support) ve yeni bir dizin oluşturmak `/home/site/wwwroot/apm`. 
1. Java aracı dosyalarını bir dizin altında dosya yükleme `/home/site/wwwroot/apm`. Aracınızı dosyaları olmalıdır `/home/site/wwwroot/apm/appdynamics`.
1. Makinedeki Azure portalında, App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Kullanıyorsanız **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` burada `<YOUR_SITE_NAME>` App Service adınız.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` burada `<YOUR_SITE_NAME>` App Service adınız.
