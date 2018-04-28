---
title: Linux’ta App Service’e Giriş | Microsoft Docs
description: Linux’ta Azure App Service hakkında bilgi edinin.
keywords: azure app service, linux, oss
services: app-service
documentationcenter: ''
author: naziml
manager: cfowler
editor: ''
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/16/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 7800c2e72b1bfd78f83bf7b90a35e47f6109a014
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Linux’ta Azure App Service’e Giriş

[Web App](../app-service-web-overview.md), web sitelerini ve web uygulamalarını barındırmak için en uygun hale getirilmiş, tam olarak yönetilen bir işlem platformudur. Müşteriler Linux’ta App Service’i kullanarak desteklenen uygulama yığınları için Linux’ta yerel olarak web uygulamaları barındırabilir. Aşağıdaki bölümlerde, şu an desteklenen uygulama yığınları listelenmiştir.

## <a name="languages"></a>Diller

Linux’ta App Service, geliştirici üretkenliğini artırmaya yönelik çeşitli Yerleşik görüntüleri destekler. Yerleşik görüntülerde uygulamanızın gerektirdiği çalışma zamanı desteklenmiyorsa, [kendi Docker görüntünüzü](tutorial-custom-docker-image.md) oluşturarak Kapsayıcılar için Web App’e dağıtmaya yönelik yönergeler sunulmaktadır.

| Dil | Desteklenen Sürümler |
|---|---|
| Node.js | 4.4, 4.5, 4.8, 6.2, 6.6, 6.9, 6.10, 6.11, 8.0, 8.1, 8.2, 8.8, 8.9, 9.4 |
| Java * | 8.0 |
| PHP | 5.6, 7.0, 7.2 |
| .NET Core | 1.0, 1.1, 2.0 |
| Ruby | 2.3 |
| Başlayın | 1.0 |
| Apache Tomcat | 8.5, 9.0 |

Daha fazla ayrıntı için bkz. [Linux’ta App Service’te Java web uygulaması oluşturma](https://docs.microsoft.com/en-us/azure/app-service/containers/quickstart-java).

## <a name="deployments"></a>Dağıtımlar

* FTP
* Yerel Git
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Hazırlık ortamları
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro) ve DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>Konsol, Yayımlama ve Hata Ayıklama

* Ortamlar
* Dağıtımlar
* Temel konsol
* SSH

## <a name="scaling"></a>Ölçeklendirme

* Müşteriler [App Service planı](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview?toc=%2fazure%2fapp-service-web%2ftoc.json) katmanlarını değiştirerek web uygulamalarının ölçeğini büyütüp küçültebilirler

## <a name="locations"></a>Konumlar

[Azure Durum Panosu](https://azure.microsoft.com/status)’nu inceleyin.

## <a name="limitations"></a>Sınırlamalar

Azure portalı, yalnızca şu anda Kapsayıcılar için Web App ile kullanılabilen özellikleri gösterir. Yeni özellikler etkinleştirildikçe portalda görünmeye başlayacaktır.

Sanal ağ tümleştirme, Azure Active Directory/üçüncü taraf kimlik doğrulaması veya Kudu site uzantıları gibi bazı özellikler henüz kullanılamamaktadır. Bu özellikler kullanıma sunulduğunda belgelerimizi ve blogumuzu güncelleştireceğiz.

Linux’ta App Service yalnızca [Temel ve Standart](https://azure.microsoft.com/pricing/details/app-service/plans/) uygulama hizmeti planlarıyla desteklenir ve [Ücretsiz veya Paylaşılan](https://azure.microsoft.com/pricing/details/app-service/plans/) katman içermez. Zaten Linux dışı Web App’ler barındıran bir App Service planında Kapsayıcılar için Web App oluşturamazsınız.

## <a name="troubleshooting"></a>Sorun giderme

Uygulamanız başlatılamazsa veya uygulamanızdan alınan günlük kayıtlarına bakmak isterseniz LogFiles dizinindeki Docker günlüklerini inceleyin. Bu dizine SCM siteniz üzerinden veya FTP aracılığıyla erişebilirsiniz.
Kapsayıcınızdan `stdout` ve `stderr` değerlerini günlüğe kaydetmek için **Tanılama Günlükleri** altında **Docker Kapsayıcı günlüğe kaydetme**’yi etkinleştirmeniz gerekir.

![Günlüğe Kaydetmeyi Etkinleştirme][2]

![Kudu kullanarak Docker günlüklerini görüntüleme][1]

SCM sitesine **Geliştirme Araçları** menüsündeki **Gelişmiş Araçlar**’dan erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Linux’ta App Service kullanmaya başlamak için aşağıdaki bağlantılara bakın. Sorularınızı ve çekincelerinizi [forumumuzda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) paylaşabilirsiniz.

* [Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanma](quickstart-docker-go.md)
* [Linux üzerinde Azure App Service’te .NET Core Kullanma](quickstart-dotnetcore.md)
* [Linux üzerinde Azure App Service’te Ruby Kullanma](quickstart-ruby.md)
* [Kapsayıcılar için Azure App Service Web App SSS](app-service-linux-faq.md)
* [Linux üzerinde Azure App Service için SSH desteği](app-service-linux-ssh-support.md)
* [Azure App Service’te hazırlık ortamları ayarlama](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Kapsayıcılar için Web App ile Docker Hub Sürekli Dağıtımı](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
