---
title: Azure App Service Web Apps için Java uygulaması Ekle
description: Bu öğretici bir sayfa veya Java kullanmak için zaten yapılandırılmış Azure App Service Web Apps örneğiniz uygulamaya nasıl ekleneceğini gösterir.
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: 9b46528b-e2d0-4f26-b8d7-af94bd8c31ef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/11/2018
ms.author: robmcm
ms.openlocfilehash: 71009370282fcfbd71c00b30d4ea22802035714d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Azure App Service Web Apps için Java uygulaması Ekle
Java web uygulamanıza başlatıldıktan sonra [Azure App Service] [ Azure App Service] adresinde belgelenen gibi [Azure App Service'te bir Java web uygulaması oluşturma](app-service-web-get-started-java.md), WAR koyarak uygulamanızı karşıya yükleyebilir **webapps** klasör.

Gezinti yolunu **webapps** klasör Web Apps örneğinizi nasıl ayarladığınıza göre değişir.

* Azure Market yolunu kullanarak web uygulamanızı ayarlarsanız **webapps** klasördür biçiminde **d:\home\site\wwwroot\bin\application\_server\webapps**, burada **uygulama\_sunucu** uygulama sunucusunun etkin Web Apps örneğinizi adıdır. 
* Azure yapılandırma kullanıcı Arabirimi, yolunu kullanarak web uygulamanızı ayarlarsanız **webapps** klasördür biçiminde **d:\home\site\wwwroot\webapps**. 

Uygulama veya web sayfaları dahil olmak üzere, karşıya yüklemek için kaynak denetimi kullanabileceğinizi unutmayın [sürekli tümleştirme senaryolarına](app-service-continuous-deployment.md). FTP, ayrıca uygulama veya web sayfalarından karşıya yükleme için bir seçenektir; FTP üzerinden uygulamalarınızı dağıtma hakkında daha fazla bilgi için bkz: [FTP kullanarak uygulamanızı dağıtma](app-service-deploy-ftp.md).

Tomcat web uygulamaları için Not: WAR dosyanızı yüklediğiniz sonra **webapps** klasörü, Tomcat uygulama sunucusu bunu eklemiş ve otomatik olarak algılar. KÖK dizinine (dışında WAR dosyaları) dosyaları kopyalarsanız, uygulama sunucusu bu dosyaları kullanılmadan önce yeniden başlatılması gerektiğini unutmayın. Azure üzerinde çalışan Tomcat Java web uygulamaları için autoload işlevi eklenmekte olan yeni bir WAR dosyasını temel alarak ya da yeni dosya veya dizinlerin eklenen **webapps** klasör. 

<a name="see-also"></a>

## <a name="see-also"></a>Ayrıca Bkz.
Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi].

[Application-insights-App-insights-Java-Get-Started](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
