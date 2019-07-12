---
title: Linux'ta - Azure App Service'e Giriş | Microsoft Docs
description: Linux’ta Azure App Service hakkında bilgi edinin.
keywords: azure app service, linux, oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 1/11/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 6bca1b067f5ec667e8b5da92a182a5618582b2f3
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617440"
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Linux’ta Azure App Service’e Giriş

[Azure App Service](../overview.md) Web siteleri ve web uygulamalarını barındırmak için iyileştirilmiş tam olarak yönetilen bir işlem platformudur. Müşteriler Linux’ta App Service’i kullanarak desteklenen uygulama yığınları için Linux’ta yerel olarak web uygulamaları barındırabilir. [Dilleri](#languages) bölümde şu anda desteklenen uygulama yığınları listelenmiştir.

## <a name="languages"></a>Languages

Linux’ta App Service, geliştirici üretkenliğini artırmaya yönelik çeşitli Yerleşik görüntüleri destekler. Yerleşik görüntülerde uygulamanızın gerektirdiği çalışma zamanı desteklenmiyorsa, [kendi Docker görüntünüzü](tutorial-custom-docker-image.md) oluşturarak Kapsayıcılar için Web App’e dağıtmaya yönelik yönergeler sunulmaktadır.

| Dil | Desteklenen Sürümler |
|---|---|
| Node.js | 4.4, 4.5, 4.8, 6.2, 6.6, 6.9, 6.10, 6.11, 8.0, 8.1, 8.2, 8.8, 8.9, 8.11, 8.12, 9.4, 10.1, 10.10, 10.14 |
| Java * | Tomcat 8.5, 9.0, Java SE WildFly 14 (tüm çalışan JRE 8) |
| PHP | 5.6, 7.0, 7.2, 7.3 |
| Python | 2.7, 3.6, 3.7 |
| .NET Core | 1.0, 1.1, 2.0, 2.1, 2.2 |
| Ruby | 2.3, 2.4, 2.5, 2.6 |

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

* Müşteriler [App Service planı](https://docs.microsoft.com/azure/app-service/overview-hosting-plans?toc=%2fazure%2fapp-service-web%2ftoc.json) katmanlarını değiştirerek web uygulamalarının ölçeğini büyütüp küçültebilirler

## <a name="locations"></a>Konumlar

[Azure Durum Panosu](https://azure.microsoft.com/status)’nu inceleyin.

## <a name="limitations"></a>Sınırlamalar

Azure portalı, yalnızca şu anda Kapsayıcılar için Web App ile kullanılabilen özellikleri gösterir. Yeni özellikler etkinleştirildikçe portalda görünmeye başlayacaktır.

Linux'ta App Service ile desteklenen yalnızca [ücretsiz, temel, standart ve Premium](https://azure.microsoft.com/pricing/details/app-service/plans/) app service planları ve olmayan bir [paylaşılan](https://azure.microsoft.com/pricing/details/app-service/plans/) katmanı. Zaten Linux dışı Web App'ler barındıran bir App Service planında bir Linux Web uygulaması oluşturulamıyor.  

Geçerli bir sınırlama göre aynı kaynak grubunun aynı bölgede Windows ve Linux uygulamaları karıştırılamaz.

## <a name="troubleshooting"></a>Sorun giderme

Uygulamanız başlatılamazsa veya uygulamanızdan alınan günlük kayıtlarına bakmak isterseniz LogFiles dizinindeki Docker günlüklerini inceleyin. Bu dizine SCM siteniz üzerinden veya FTP aracılığıyla erişebilirsiniz. Oturum `stdout` ve `stderr` etkinleştirmeniz gerekiyor, kapsayıcıdan **Docker kapsayıcı günlüğe kaydetme** altında **uygulama hizmeti günlükleri**. Ayar, hemen geçerli olur. App Service, değişikliği algılar ve kapsayıcı otomatik olarak yeniden başlatır.

SCM sitesine **Geliştirme Araçları** menüsündeki **Gelişmiş Araçlar**’dan erişebilirsiniz.

![Kudu kullanarak Docker günlüklerini görüntüleme][1]

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler farklı dillerde yazılmış web uygulamalarıyla Linux'ta App Service hizmetini kullanmaya başlamanızı sağlar:

* [.NET Core](quickstart-dotnetcore.md)
* [PHP](https://docs.microsoft.com/azure/app-service/containers/quickstart-php)
* [Node.js](quickstart-nodejs.md)
* [Java](quickstart-java.md)
* [Python](quickstart-python.md)
* [Ruby](quickstart-ruby.md)
* [Go](quickstart-docker-go.md)
* [Birden çok kapsayıcılı uygulamalar](quickstart-multi-container.md)

Linux'ta App Service hakkında daha fazla bilgi için bkz:

* [Linux’ta App Service hakkında SSS](app-service-linux-faq.md)
* [Linux'ta App Service için SSH desteği](app-service-linux-ssh-support.md)
* [App Service’te hazırlık ortamları ayarlama](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Docker Hub sürekli dağıtımı](app-service-linux-ci-cd.md)

Sorularınızı ve çekincelerinizi [forumumuzda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) paylaşabilirsiniz.

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
