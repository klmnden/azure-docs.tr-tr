---
title: Dağıtım hakkında SSS - Azure uygulama hizmeti | Microsoft Docs
description: Azure App Service'in Web Apps özelliği için dağıtımı hakkında sık sorulan soruların yanıtlarını alın.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: beee76bdc443b3a66b4500b83d228075b84eed1e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65864763"
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Azure Web Apps'te dağıtım hakkında SSS

Bu makalede, dağıtım sorunları için hakkında sık sorulan sorular (SSS) yanıtlarını bulunur. [Azure App Service'in Web Apps özelliği](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>Ben yalnızca App Service web apps'i kullanmaya başlama. Benim kodumu nasıl yayımlayabilirim?

Web uygulaması kodunuzu yayımlamak için bazı seçenekler şunlardır:

*   Visual Studio kullanarak dağıtın. Visual Studio çözümünüz varsa, web uygulaması projesine sağ tıklayın ve ardından **Yayımla**.
*   Bir FTP istemcisi kullanarak dağıtın. Azure portalında, kodunuzu dağıtmak istediğiniz web uygulaması için yayımlama profili indirin. Ardından, dosyaları \site\wwwroot için yayımlama profili FTP kimlik bilgilerini kullanarak yükleyin.

Daha fazla bilgi için [uygulamanızı App Service'e dağıtma](deploy-local-git.md).

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this-error"></a>Visual Studio'dan dağıtmak çalıştığınızda bir hata iletisi konusuna bakın. Bu hatanın nasıl giderebilirim?

Aşağıdaki iletiyi görürseniz, SDK'ın eski bir sürümünü kullanıyor olabilirsiniz: "'YourResourceGroup' kaynak grubundaki 'YourResourceName' kaynağının dağıtımı sırasında hata oluştu: MissingRegistrationForLocation: Abonelik, 'Orta ABD' konum ' Bileşenler' kaynak türü için kayıtlı değil. Bu sağlayıcı için bu konuma erişimi olması için yeniden kaydedin." 

Bu hatayı gidermek için yükseltme [en son SDK'sı](https://azure.microsoft.com/downloads/). Bu iletiyi görürseniz ve en son SDK'sına sahip bir destek isteği gönderin.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a>Bir ASP.NET uygulamasını Visual Studio'dan App Service'e nasıl dağıtabilirim?
<a id="deployasp"></a>

Öğreticiyi [beş dakika içinde Azure'da ilk ASP.NET web uygulamanızı oluşturma](app-service-web-get-started-dotnet.md) Visual Studio kullanarak bir ASP.NET web uygulamasını App Service'te bir web uygulamasına dağıtma işlemi gösterilmektedir.

## <a name="what-are-the-different-types-of-deployment-credentials"></a>Dağıtım kimlik bilgileri farklı türleri nelerdir?

App Service, yerel Git dağıtımı ve FTP/S dağıtımı için iki tür kimlik bilgilerini destekler. Dağıtım kimlik bilgilerini yapılandırma hakkında daha fazla bilgi için bkz. [App Service için dağıtım kimlik bilgilerini yapılandırma](deploy-configure-credentials.md).

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a>My App Service web uygulaması dosya veya dizin yapısı nedir?

App Service uygulamanızı dosya yapısı hakkında daha fazla bilgi için bkz: [Azure'da dosya yapısı](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a>Ne giderebilirim "FTP hata - 550 orada diskte yeterli alan değil" dosyalarımı FTP denediğimde?

Bu iletiyi görürseniz, bir disk kotası web uygulamanız için service planında çalıştırıyorsanız olasıdır. Disk alanı gereksinimlerinize göre daha yüksek bir hizmet katmanına ölçeği gerekebilir. Fiyatlandırma planları ve kaynak sınırları hakkında daha fazla bilgi için bkz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>App Service web Uygulamam için sürekli dağıtımı nasıl ayarlayabilirim?

Azure DevOps, OneDrive, GitHub, Bitbucket, Dropbox ve diğer Git depoları da dahil olmak üzere çeşitli kaynaklardan sürekli dağıtım ayarlayabilirsiniz. Bu seçenekler, portalda kullanılabilir. [App Service'e sürekli dağıtımı](deploy-continuous-deployment.md) sürekli dağıtımı ayarlamak üzere nasıl açıklayan yardımcı bir öğreticidir.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>GitHub ve bitbucket aracılığıyla sürekli dağıtım ile ilgili sorunları nasıl giderebilirim?

Yardım için GitHub veya Bitbucket sürekli dağıtım sorunlarını araştırma [sürekli dağıtım araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this-issue"></a>Sitem için FTP olamaz ve kodum yayımlayın. Bu sorunu nasıl giderebilirim?

FTP sorunları çözmek için:

1. Doğru ana bilgisayar adı ve kimlik bilgilerini girdiğinizden emin olun. Farklı türde kimlik bilgileri ve bunların nasıl kullanılacağı hakkında ayrıntılı bilgi için bkz: [dağıtım kimlik bilgileri](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).
2. FTP bağlantı noktalarının bir güvenlik duvarı tarafından engellenmediğinden emin olun. Bağlantı noktalarını bu ayarlara sahip olmalıdır:
    * FTP denetim bağlantı noktası: 21
    * FTP veri bağlantı noktası: 989, 10001-10300

## <a name="how-do-i-publish-my-code-to-app-service"></a>Benim kodumu App Service'e nasıl yayımlayabilirim?

Azure hızlı başlangıç, uygulamanızın dağıtım yığını ve tercih ettiğiniz yöntemi kullanarak dağıtmanıza yardımcı olması için tasarlanmıştır. Azure portalında Hızlı Başlangıç'ı kullanmak için app service, altında giderek **dağıtım**seçin **hızlı**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a>Neden uygulamamı bazen App Service'e dağıtım sonrasında yeniden başlatılacak mı?

Koşullar altında bir uygulama dağıtımının neden olabilir bir yeniden başlatma hakkında bilgi edinmek için [çalışma zamanı sorunlarına ve dağıtım](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts"). Bu makalede açıklandığı gibi App Service dosyaları wwwroot klasörüne dağıtır. Hiçbir zaman doğrudan uygulamanızı yeniden başlatır.

## <a name="how-do-i-integrate-azure-devops-code-with-app-service"></a>Azure DevOps kodu App Service ile nasıl tümleştirebilirim?

Azure DevOps ile sürekli dağıtımı kullanmak için iki seçeneğiniz vardır:

*   Git proje kullanırsınız. App Service ile dağıtım merkezi kullanarak bağlanın.
*   Team Foundation sürüm denetimi (TFVC) projesini kullanın. App Service için yapı aracısını kullanarak dağıtın.

Kod sürekli dağıtım için bu seçeneklerin mevcut Geliştirici iş akışları ve iade etme yordamları bağlıdır. Daha fazla bilgi için şu makalelere bakın: 

*   [Uygulamanızı bir Azure Web sitesine uygulama sürekli dağıtım](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Uygulamayı bir web uygulamasına dağıtmak için bir Azure DevOps kuruluşu ayarlayın](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a>Uygulamam için App Service dağıtmak için FTP veya FTPS nasıl kullanırım?

Web uygulamanızı App Service'e dağıtmak için FTP veya FTPS kullanma hakkında daha fazla bilgi için bkz: [FTP/S kullanarak uygulamanızı App Service'e dağıtma](deploy-ftp.md).
