---
title: "Azure web uygulamaları için dağıtım SSS | Microsoft Docs"
description: "Azure App Service Web Apps özelliği için dağıtımı hakkında sık sorulan soruların yanıtlarını alın."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 318a236652229c4e093ca33886ac1831686aed73
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Azure Web uygulamalarının dağıtımını SSS

Bu makale için dağıtım sorunları hakkında sık sorulan sorular (SSS) yanıtlarını sahip [Azure App Service Web Apps özelliğini](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>I yalnızca App Service web apps ile çalışmaya başlama. Kodumu nasıl yayımlanacak?

Web uygulama kodunuzda yayımlamak için bazı seçenekler şunlardır:

*   Visual Studio kullanarak dağıtın. Visual Studio çözümünüz varsa web uygulama projesine sağ tıklayın ve ardından **Yayımla**.
*   Bir FTP istemcisi kullanarak dağıtın. Azure portalında kodunuzu dağıtmak istediğiniz web uygulaması için yayımlama profili indirin. Ardından, dosyaları aynı yayımlama profili FTP kimlik bilgilerini kullanarak \site\wwwroot için karşıya yükleme.

Daha fazla bilgi için bkz: [uygulamanızı App Service'e dağıtma](app-service-deploy-local-git.md).

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this"></a>Visual Studio'dan dağıtmak çalıştığınızda hata iletisine bakın. Bu nasıl giderebilirim?

Aşağıdaki iletiyi görürseniz, SDK'ın eski bir sürümünü kullanıyor olabilirsiniz: "'YourResourceGroup' kaynak grubundaki 'YourResourceName' kaynağının dağıtımı sırasında bir hata oluştu: MissingRegistrationForLocation: Abonelik 'Orta ABD' konumunda 'Bileşenleri' kaynak türü için kayıtlı değil. Lütfen bu sağlayıcı için bu konuma erişimi olması için yeniden kaydedin." 

Bu hatayı gidermek için yükseltme [son SDK](https://azure.microsoft.com/downloads/). Bu iletiyi görürseniz ve en son SDK'sına sahip bir destek isteği gönderin.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a>Uygulama hizmeti Visual Studio'dan ASP.NET uygulamasını nasıl dağıtırım?
<a id="deployasp"></a>

Öğretici [beş dakika içinde ilk ASP.NET web uygulamanızı oluşturma](app-service-web-get-started-dotnet.md) Visual Studio 2017 kullanarak App Service'in web uygulamasında bir ASP.NET web uygulamasına dağıtma gösterilmektedir.

## <a name="what-are-the-different-types-of-deployment-credentials"></a>Dağıtım kimlik bilgileri farklı türleri nelerdir?

Uygulama hizmeti yerel Git dağıtımı ve FTP/S dağıtımı için iki kimlik bilgisi türlerinin kullanılmasını destekler. Dağıtım kimlik bilgileri yapılandırma hakkında daha fazla bilgi için bkz: [dağıtım kimlik bilgileri App Service için yapılandırma](app-service-deployment-credentials.md).

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a>App Service web Uygulamam dosya veya dizin yapısını nedir?

Uygulama hizmeti uygulamanızı dosya yapısı hakkında daha fazla bilgi için bkz: [dosya yapısı Azure'da](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a>Ne giderebilirim "FTP hata 550 - orada disk üzerinde yeterli alan yok" çalıştığımda dosyalarımı FTP?

Bu iletiyi görürseniz, disk kotası web uygulamanız için hizmeti planında çalıştırıyorsanız olasıdır. Disk alanı gereksinimlerinize göre daha yüksek bir hizmet katmanı ölçeği artırma gerekebilir. Fiyatlandırma planları ve kaynak sınırları hakkında daha fazla bilgi için bkz: [uygulama hizmeti fiyatlandırma](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>My App Service web uygulaması için sürekli dağıtım nasıl ayarlarım?

Visual Studio Team Services, OneDrive, GitHub, Bitbucket, Dropbox ve diğer Git depoları gibi çeşitli kaynaklardan sürekli dağıtım ayarlayabilirsiniz. Bu seçenekler Portalı'nda kullanılabilir. [Uygulama hizmeti için sürekli dağıtım](app-service-continuous-deployment.md) sürekli dağıtım ayarlayın açıklanmaktadır yararlı bir öğreticidir.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>GitHub ve Bitbucket sürekli dağıtım sorunlarını giderme nasıl?

GitHub veya Bitbucket sürekli dağıtım sorunları incelemeye Yardım için bkz. [sürekli dağıtım araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this"></a>Sitem için FTP olamaz ve kodumu yayımlayın. Bu nasıl giderebilirim?

FTP sorunları gidermek için:

1. Doğru ana bilgisayar adı ve kimlik bilgilerini girme olduğunu doğrulayın. Farklı türlerde kimlik bilgileri ve bunların nasıl kullanılacağını hakkında ayrıntılı bilgi için bkz: [dağıtım kimlik bilgileri](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).
2. FTP bağlantı noktalarını güvenlik duvarı tarafından engellenmiyor olduğunu doğrulayın. Bağlantı noktaları, bu ayarlara sahip olmalıdır:
    * FTP denetim bağlantı noktası: 21
    * FTP veri bağlantı noktası: 989, 10001 10300

## <a name="how-do-i-publish-my-code-to-app-service"></a>Uygulama hizmeti nasıl kodumu yayımlanacak?

Azure hızlı başlangıç dağıtım yığını ve tercih ettiğiniz yöntemi kullanarak uygulamanızı dağıtmanıza yardımcı olmak için tasarlanmıştır. Azure portalında hızlı başlangıç kullanmak için Git **ayarları** > **uygulama dağıtımı**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a>Neden Uygulamam bazen App Service'e dağıtım sonrasında yeniden?

Koşullar altında bir uygulama dağıtımının neden olabilir bir yeniden başlatma hakkında bilgi edinmek için [dağıtım çalışma zamanı sorunlarına karşılaştırması](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts"). Makaleyi açıklandığı gibi uygulama hizmeti dosyaları wwwroot klasörüne dağıtır. Bunu doğrudan uygulamanızı yeniden başlatmaz.

## <a name="how-do-i-integrate-visual-studio-team-services-code-with-app-service"></a>Uygulama hizmeti ile nasıl Visual Studio Team Services kod tümleşik mu?

Visual Studio Team Services ile kullanarak sürekli dağıtımı için iki seçeneğiniz vardır:

*   Git proje kullanın. Uygulama hizmeti bu depo için dağıtım seçeneklerini kullanarak bağlanın.
*   Team Foundation sürüm denetimi (TFVC'yi) proje kullanın. Uygulama hizmeti için derleme aracısı kullanarak dağıtın.

Bu her iki seçenek için sürekli kod dağıtım varolan geliştirici iş akışları ve iade etme yordamları bağlıdır. Daha fazla bilgi için bu makalelere bakın: 

*   [Bir Azure Web sitesine, uygulamanızın sürekli dağıtım uygulama](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Bir web uygulamasına dağıtabilmek için bir Visual Studio Team Services hesabı ayarlamanız](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a>Uygulamam App Service'e dağıtmak için FTP veya FTPS nasıl kullanırım?

Web uygulamanızı App Service'e dağıtmak üzere FTP veya FTPS kullanma hakkında daha fazla bilgi için bkz: [FTP/S kullanarak uygulamanızı App Service'e dağıtma](app-service-deploy-ftp.md).
