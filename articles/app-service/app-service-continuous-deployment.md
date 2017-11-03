---
title: "Azure uygulama hizmeti için sürekli dağıtım | Microsoft Docs"
description: "Azure Uygulama Hizmeti’ne sürekli dağıtımı etkinleştirme hakkında bilgi edinin."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: ef5924607868bcb3dc35e279539b78d5a0e17c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="continuous-deployment-to-azure-app-service"></a>Azure Uygulama Hizmeti’ne Sürekli Dağıtım
Bu öğretici için sürekli dağıtım iş akışı yapılandırılacağı gösterilmektedir, [Azure Web Apps](app-service-web-overview.md). BitBucket, GitHub, uygulama hizmeti tümleşme ve [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) projenizden en son güncelleştirmeleri Azure burada çekmesi sürekli dağıtım iş akışı yayımlanan Bu hizmetlerden biri için etkinleştirir. Sürekli dağıtım, birden fazla ve sık gerçekleşen katkıların tümleştirildiği projeler için mükemmel bir seçenektir.

El ile Azure Portal tarafından listelenmeyen bir bulut deposu sürekli dağıtımını yapılandırmak nasıl öğrenmek için (gibi [GitLab](https://gitlab.com/)), bkz: [el ile adımları kullanarak sürekli dağıtım ayarı](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="overview"></a>Sürekli dağıtımı etkinleştirme
Sürekli dağıtımı etkinleştirmek için,

1. Uygulamanızın içeriğini sürekli dağıtım için kullanılacak depoda yayımlayın.  
    Projenizi bu hizmetlerde yayımlama hakkında daha fazla bilgi için bkz. [Depo oluşturma (GitHub)], [Depo oluşturma (BitBucket)] ve [VSTS kullanmaya başlama].
2. Uygulamanızın [Azure Portal] menü dikey penceresinde **UYGULAMA DAĞITIMI > Dağıtım seçenekleri**’ne tıklayın. **Kaynak Seç**’e tıklayın, ardından dağıtım kaynağını seçin.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > Uygulama Hizmeti dağıtımına yönelik bir VSTS hesabı yapılandırmak için lütfen bu [öğreticiye](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App) bakın.
   > 
   > 
3. Yetkilendirme iş akışını tamamlayın.
4. **Dağıtım kaynağı** dikey penceresinde dağıtımın yapılacağı projeyi ve dalı seçin. İşiniz bittiğinde, **Tamam**’a tıklayın.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > GitHub veya BitBucket ile sürekli dağıtımı etkinleştirirken hem genel hem de özel projeler gösterilir.
   > 
   > 
   
    Uygulama Hizmeti seçili depo ile bir ilişki oluşturur, belirtilen daldan dosyaları seçer ve deponuzun bir kopyasını Uygulama Hizmeti uygulamasında tutar. Azure portalında VSTS sürekli dağıtımını yapılandırdığınızda tümleştirme, her `git push` ile derleme ve dağıtım görevini otomatik hale getiren Uygulama Hizmeti [Kudu dağıtım altyapısını](https://github.com/projectkudu/kudu/wiki) kullanır. VSTS’de sürekli dağıtımı ayrıca ayarlamanız gerekmez. Bu işlem tamamlandıktan sonra **Dağıtım seçenekleri** uygulama dikey penceresinde dağıtımın başarılı olduğunu belirten bir etkin dağıtım gösterilir.
5. Uygulamanın başarıyla dağıtıldığını doğrulamak için Azure portalındaki uygulama dikey penceresinin üstünde bulunan **URL**’ye tıklayın.
6. Sürekli dağıtımın seçtiğiniz depodan gerçekleştiğini doğrulamak için depoya bir değişiklik gönderin. Depoya gönderilen değişiklik tamamlandıktan kısa süre sonra uygulamanız değişiklikleri yansıtacak şekilde güncelleştirilir. Uygulamanızın **Dağıtım seçenekleri** dikey penceresinde güncelleştirmenin alındığını doğrulayabilirsiniz.

## <a name="VSsolution"></a>Visual Studio çözümünün sürekli dağıtımı
Azure Uygulama Hizmeti’ne bir Visual Studio çözümünün gönderilmesi, basit bir index.html dosyası göndermek kadar kolaydır. Uygulama Hizmeti dağıtım işlemi, NuGet bağımlılıklarını geri yükleme ve uygulama ikili dosyalarını oluşturma dahil tüm ayrıntıları kolaylaştırır. Kodu yalnızca Git deponuzda tutmayı ve Uygulama Hizmeti dağıtımının geri kalan işlemlerini yapmasına izin vermeyi içeren kaynak denetimi en iyi uygulamalarını izleyebilirsiniz.

Çözümünüzü ve deponuzu aşağıdaki gibi yapılandırmanız şartıyla, Visual Studio çözümünüzü Uygulama Hizmeti’ne gönderme adımları [önceki bölümde](#overview) verilen adımlarla aynıdır:

* Aşağıdaki görüntüye benzer bir `.gitignore` dosyası oluşturmak veya deponuzda bu [.gitignore örneğine](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore) benzer içeriğe sahip bir `.gitignore` dosyasını el ile eklemek için Visual Studio kaynak denetimi seçeneğini kullanın.
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Çözümün tamamına ait dizin ağacını, .sln dosyası depo kökünde olacak şekilde deponuza ekleyin.

Deponuzu anlatılan şekilde ayarlayıp Azure’da uygulamanızı çevrimiçi Git depolarından birinden sürekli yayımlama için yapılandırdıktan sonra, Visual Studio’da ASP.NET uygulamanızı yerel olarak geliştirebilir ve değişikliklerinizi çevrimiçi Git deponuza göndererek kodunuzu sürekli olarak dağıtabilirsiniz.

## <a name="disableCD"></a>Sürekli dağıtımı devre dışı bırakma
Sürekli dağıtımı devre dışı bırakmak için,

1. Uygulamanızın [Azure Portal] menü dikey penceresinde **UYGULAMA DAĞITIMI > Dağıtım seçenekleri**’ne tıklayın. Ardından **Dağıtım seçenekleri** dikey penceresinde **Bağlantıyı Kes**’e tıklayın.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Onay iletisine **Evet** cevabını verdikten sonra başka bir kaynaktan yayımlamayı ayarlamak isterseniz uygulamanın dikey penceresine geri dönüp **UYGULAMA DAĞITIMI > Dağıtım seçenekleri**’ne tıklayabilirsiniz.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Sürekli dağıtımla ilgili yaygın sorunları araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure için PowerShell'i kullanma]
* [Mac ve Linux için Azure Komut Satırı Araçlarını kullanma]
* [Git belgeleri]
* [Kudu Projesi](https://github.com/projectkudu/kudu/wiki)
* [Otomatik olarak ASP.NET 4 uygulama dağıtmak için bir CI/CD ardışık düzen oluşturmak için Azure kullanın](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

[Azure Portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure için PowerShell'i kullanma]: /powershell/azureps-cmdlets-docs
[Mac ve Linux için Azure Komut Satırı Araçlarını kullanma]:../cli-install-nodejs.md
[Git Belgeleri]: http://git-scm.com/documentation

[Depo oluşturma (GitHub)]: https://help.github.com/articles/create-a-repo
[Depo oluşturma (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[VSTS kullanmaya başlama]: https://www.visualstudio.com/docs/vsts-tfs-overview
