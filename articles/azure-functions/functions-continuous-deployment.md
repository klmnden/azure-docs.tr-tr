---
title: Azure işlevleri için sürekli dağıtım | Microsoft Docs
description: Azure App Service'in sürekli dağıtım özellikleri, işlevlerinizin yayımlamak için kullanın.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: cb3f3ad3bb7b42429654ea4bf9b49f7e230db1da
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943893"
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure İşlevleri için sürekli dağıtım
Azure işlevleri işlev uygulamanızı sürekli tümleştirmeyi kullanarak dağıtma kolaylaştırır. İşlevleri, büyük kod depoları ve dağıtım kaynakları ile tümleşir. Bu tümleştirme, bu hizmetleri tetikleyici dağıtımı biri aracılığıyla Azure'a yapılan işlev kodunu burada güncelleştirmeleri bir iş akışı sağlar. Azure işlevleri'ne yeni başladıysanız başlayın [Azure işlevlerine genel bakış](functions-overview.md).

Sürekli dağıtım, birden çok burada tümleştiriyorsanız projeler için mükemmel bir seçenektir ve sık sık katkıda. Ayrıca, işlev kodunuzun üzerinde kaynak denetimi sağlamanıza olanak tanır. Azure işlevleri, aşağıdaki dağıtım kaynaklarını destekler:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Dış depo (Git veya Mercurial)
* [Yerel Git deposu](../app-service/deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Azure DevOps](https://azure.microsoft.com/services/devops/)

Dağıtımları, bir işlev başına uygulama başına yapılandırılır. Sürekli dağıtım etkinleştirildikten sonra portalda işlev kodunu erişimi kümesine *salt okunur*.

## <a name="requirements-for-continuous-deployment"></a>Sürekli dağıtım için gereksinimleri

Sürekli dağıtım Ayarla önce dağıtım kaynağı olarak yapılandırılan dağıtım kaynağınız ve işlev kodunuzu olması gerekir. Bir işlev uygulaması dağıtımında, her dizin adı işlevin adını olduğu bir adlandırılmış alt dizinde işlevidir.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Azure DevOps dağıtın edebilmek için Azure DevOps kuruluşunuzun Azure aboneliğinizle bağlamanız gerekir. Daha fazla bilgi için [Azure DevOps kuruluşunuz için fatura bilgilerini ayarlayın](https://docs.microsoft.com/azure/devops/organizations/billing/set-up-billing-for-your-organization-vs?view=vsts#set-up-billing-via-the-azure-portal).

## <a name="set-up-continuous-deployment"></a>Sürekli dağıtım ayarlama
Var olan bir işlev uygulaması için sürekli dağıtımını yapılandırmak için bu yordamı kullanın. Bir GitHub deposu ile tümleştirme adımları gösterir, ancak Azure DevOps veya diğer Dağıtım Hizmetleri için benzer adımları uygulayın.

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com)seçin **Platform özellikleri** > **dağıtım seçenekleri**. 
   
    ![Dağıtım Seçenekleri'ni açmak için seçimleri](./media/functions-continuous-deployment/setup-deployment.png)
 
1. Üzerinde **dağıtımları** dikey penceresinde **Kurulum**.
 
    ![Dağıtımları dikey penceresi](./media/functions-continuous-deployment/setup-deployment-1.png)
   
1. Üzerinde **dağıtım kaynağı** dikey penceresinde **Kaynak Seç**. Seçilen dağıtım kaynağınız için bilgileri girin ve ardından **Tamam**.
   
    ![Dağıtım kaynağı seçme](./media/functions-continuous-deployment/choose-deployment-source.png)

Sürekli dağıtımı ayarlayın, sonra dağıtım kaynağınız tüm dosya değişiklikleri işlev uygulamasına kopyalanır ve bir sitenin tam dağıtım tetiklenir. Kaynaktaki dosyalardan güncelleştirildiğinde site yeniden dağıtıldı.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Tipik dağıtım senaryoları, hazırlık dağıtım oluşturma ve mevcut işlevleri için sürekli dağıtım taşıma içerir.

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Hazırlama dağıtımı oluşturma

İşlev uygulamaları henüz dağıtım yuvalarını desteklemez. Ancak, hazırlama ve üretim dağıtımları ayrı sürekli tümleştirmeyi kullanarak yönetmeye devam edebilirsiniz.

Yapılandırma ve hazırlama dağıtımı ile çalışma işlemi genellikle şu şekilde görünür:

1. Aboneliğinizde iki işlev uygulamaları oluşturun: biri üretim kodu, diğeri için hazırlama. 

1. Zaten yoksa, bir dağıtım kaynağı oluşturun. Bu örnekte [GitHub].

1. Üretim işlev uygulamanız için önceki adımları tamamlayın [sürekli dağıtım ayarlama](#set-up-continuous-deployment) ve GitHub deponuza'nın ana dalı için dağıtım ayarlayın.
   
    ![Seçimlerin bir dağıtım dalı seçin](./media/functions-continuous-deployment/choose-deployment-branch.png)

1. Hazırlama işlev uygulaması için 3. adımı yineleyin ancak hazırlama dal yerine GitHub deponuzda seçin. Dağıtım kaynağınız dallanma desteklemiyorsa, farklı bir klasör kullanın.
    
1. Hazırlama dal veya klasör kodunuzda güncelleştirmeleri yapmak ve hazırlama dağıtım bu değişiklikleri yansıttığını doğrulayın.

1. Test edildikten sonra birleştirme hazırlama dalından ana dala değiştirir. Bu birleştirme üretim işlev uygulaması için dağıtım tetikler. Dal dağıtım kaynağınız desteklemiyorsa, üretim klasöründeki dosyaları hazırlama klasördeki dosyaların üzerine yazın.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Mevcut işlevleri için sürekli dağıtım Taşı
Oluşturulan ve tutulan portalda FTP kullanarak, işlev kod dosyası indirmeniz gerekmez veya yerel Git deposu, önce sürekli dağıtım daha önce açıklanan şekilde ayarlayabilirsiniz mevcut işlevleri olduğunda. Azure App Service ayarlarında işlev uygulamanız için bunu yapabilirsiniz. Dosyaları indirdikten sonra seçilen sürekli dağıtım kaynağınız karşıya yükleyebilirsiniz.

> [!NOTE]
> Sürekli tümleştirmeyi yapılandırdıktan sonra kaynak dosyalarınızda işlevler portalına artık düzenleyebilirsiniz.

<a name="credentials"></a>
#### <a name="configure-deployment-credentials"></a>Dağıtım kimlik bilgilerini yapılandırma
FTP veya yerel bir Git deposu kullanarak işlev uygulamanızı dosyalarını indirebilmesinden önce siteye erişmek için kimlik bilgilerinizi yapılandırmanız gerekir. Kimlik bilgileri işlevi uygulama düzeyinde ayarlanır. Azure portalında dağıtım kimlik bilgilerini ayarlamak için aşağıdaki adımları kullanın:

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com)seçin **Platform özellikleri** > **dağıtım kimlik bilgileri**.
   
1. Bir kullanıcı adı ve parola girin ve ardından **Kaydet**. 

   ![Yerel dağıtım kimlik bilgilerini ayarlamak için seçimleri](./media/functions-continuous-deployment/setup-deployment-credentials.png)

Şimdi, işlev uygulamanızı FTP ya da yerleşik Git deposu erişmek için bu kimlik bilgilerini kullanabilirsiniz.

<a name="downftp"></a>
#### <a name="download-files-by-using-ftp"></a>FTP kullanarak dosyaları indirme

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com)seçin **Platform özellikleri** > **özellikleri**. Ardından, değerlerini kopyalayın **FTP/dağıtım kullanıcısı**, **FTP konak adı**, ve **FTPS konak adı**.  

   **FTP/dağıtım kullanıcısı** FTP sunucusu için uygun bağlam sağlamak için uygulama adıyla birlikte portalında görüntülenen şekliyle girilmelidir.
   
   ![Seçim, dağıtım bilgilerini almak için](./media/functions-continuous-deployment/get-deployment-credentials.png)

1. FTP istemcinizde, uygulamanıza bağlanıp işlevleriniz için kaynak dosyalarını indirmek için topladığınız bağlantı bilgilerini kullanın.

<a name="downgit"></a>
#### <a name="download-files-by-using-a-local-git-repository"></a>Yerel Git deposunu kullanarak dosyaları indirme

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com)seçin **Platform özellikleri** > **dağıtım seçenekleri**. 
   
    ![Dağıtım Seçenekleri'ni açmak için seçimleri](./media/functions-continuous-deployment/setup-deployment.png)
 
1. Ardından **dağıtımları** dikey penceresinde **Kurulum**.
 
    ![Dağıtımları dikey penceresi](./media/functions-continuous-deployment/setup-deployment-1.png)
   
1. Üzerinde **dağıtım kaynağı** dikey penceresinde **yerel Git deposu** > **Tamam**.

1. İçinde **Platform özellikleri**seçin **özellikleri** ve Git URL'sini değerini not edin. 
   
    ![Git URL'sini alma seçimleri](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

1. Git kullanan bir komut istemi veya en sevdiğiniz Git aracını kullanarak depoyu yerel makinenize kopyalayın. Git kopya komut şöyle görünür:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

1. Dosyaları aşağıdaki örnekte olduğu gibi yerel bilgisayarınızda kopya için işlev uygulamanızın getirme:
   
        git pull origin master
   
    İstenirse, sağlamanız, [dağıtım kimlik bilgileri yapılandırılmış](#credentials).  

[GitHub]: https://github.com/

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
