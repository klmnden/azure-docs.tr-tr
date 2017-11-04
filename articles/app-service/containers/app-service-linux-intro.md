---
title: "Uygulama hizmeti Linux'ta giriş | Microsoft Docs"
description: "Linux üzerinde Azure uygulama hizmeti hakkında bilgi edinin."
keywords: Azure uygulama hizmeti, linux, oss
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/16/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 89cb7dc488da42724f212d13f8550064ff8b9188
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Azure uygulama hizmeti Linux'ta giriş

[Web uygulaması](../app-service-web-overview.md) Web siteleri ve web uygulamalarını barındırmak için optimize edilmiştir tam olarak yönetilen bir işlem platformudur. Müşteriler uygulama hizmeti Linux'ta ana web uygulamalarında yerel Linux için desteklenen uygulama yığınları için kullanabilirsiniz. Aşağıdaki bölümlerde şu anda desteklenen uygulama yığınları listeler.

## <a name="languages"></a>Diller

Uygulama hizmeti Linux üzerinde Geliştirici üretkenliği artırmak için yerleşik görüntü sayısını destekler. Uygulamanızın gerektirdiği çalışma zamanı yerleşik görüntüleri desteklenmiyorsa için yönergeler vardır [kendi Docker görüntü yapı](tutorial-custom-docker-image.md) kapsayıcıları için Web uygulamasına dağıtma.

| Dil | Desteklenen sürümleri |
|---|---|
| Node.js | 4.4, 4.5, 6.2, 6.6, 6.9-6.11, 8.0, 8.1 |
| PHP | 5.6, 7.0 |
| .NET Core | 1.0, 1.1 |
| Ruby | 2.3 |

## <a name="deployments"></a>Dağıtımlar

* FTP
* Yerel Git
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Hazırlık ortamları
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/container-registry-intro) ve DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>Konsolunda, yayımlama ve hata ayıklama

* Ortamlar
* Dağıtımlar
* Temel konsol
* SSH

## <a name="scaling"></a>Ölçeklendirme

* Müşteriler ölçeklendirilebilir web uygulamaları yukarı ve aşağı katmanını değiştirerek kendi [uygulama hizmeti planı](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview?toc=%2fazure%2fapp-service-web%2ftoc.json)

## <a name="locations"></a>Konumlar

Denetleme [Azure durum Panosu](https://azure.microsoft.com/status).

## <a name="limitations"></a>Sınırlamalar

Azure portal yalnızca kapsayıcıları için Web uygulaması için şu anda iş özellikleri gösterir. Daha fazla özellik etkinleştirme gibi portalda görünür olur.

Sanal ağ tümleştirmesinin, Azure Active Directory/üçüncü taraf kimlik doğrulama veya Kudu site uzantıları gibi bazı özellikler henüz kullanılabilir değil. Bu özellikler kullanılabilir olduktan sonra bizim belgelerini ve blogu değişiklikler hakkında güncelleştireceğiz.

Linux üzerinde App Service ile desteklenen yalnızca [temel ve standart](https://azure.microsoft.com/pricing/details/app-service/plans/) uygulama hizmeti planları ve sahip olmayan bir [ücretsiz veya paylaşılan](https://azure.microsoft.com/pricing/details/app-service/plans/) katmanı. Ayrıca Linux uygulama hizmeti için önemli kısıtlamalar şunlardır:

* Web uygulaması kapsayıcıları için zaten Linux Web Apps olmayan barındırma, uygulama hizmeti planı oluşturulamıyor.
* Web uygulaması kapsayıcıları için Linux Web Apps olmayan içeren bir kaynak grubunda oluştururken, varolan bir uygulama hizmeti planı farklı bir bölgede bir uygulama hizmeti planı oluşturmalısınız.

## <a name="troubleshooting"></a>Sorun giderme

Uygulamanızı başlatılamadığında veya günlük uygulamanızdan denetlemek istediğiniz, Docker LogFiles dizininde günlüklerini denetleyin. SCM siteniz veya FTP aracılığıyla bu dizine erişebilir.
Oturum `stdout` ve `stderr` etkinleştirmeniz gerekiyor, kapsayıcıdan **Docker kapsayıcısı günlük** altında **tanılama günlükleri**.

![Günlüğü etkinleştirme][2]

![Kudu Docker günlükleri görüntülemek için kullanma][1]

SCM sitesinden erişebilirsiniz **Gelişmiş Araçlar** içinde **geliştirme araçları** menüsü.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama hizmeti Linux'ta kullanmaya başlamak için aşağıdaki bağlantılara bakın. Hakkında sorular ve sorunları nakledebilirsiniz [forumumuzda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Kapsayıcıları için Web uygulaması için özel bir Docker görüntü kullanma](quickstart-custom-docker-image.md)
* [Azure uygulama hizmetinde Linux'ta .NET Core kullanma](quickstart-dotnetcore.md)
* [Azure uygulama hizmetinde Linux'ta Ruby kullanma](quickstart-ruby.md)
* [Azure App Service Web uygulaması için kapsayıcı SSS](app-service-linux-faq.md)
* [Azure uygulama hizmeti Linux üzerinde SSH desteği](app-service-linux-ssh-support.md)
* [Hazırlık Azure App Service ortamları ayarlama](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Docker hub'a sürekli dağıtımı ile Web uygulaması kapsayıcıları için](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
