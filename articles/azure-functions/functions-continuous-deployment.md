---
title: Azure işlevleri için sürekli dağıtım | Microsoft Docs
description: Azure App Service'in sürekli dağıtım özellikleri, Azure işlevleri yayımlamak için kullanın.
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
ms.openlocfilehash: fd8fa690c508b8bf748490668c1e9aaa811ac247
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60731302"
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure İşlevleri için sürekli dağıtım
Azure işlevleri işlev uygulamanızı App Service'e sürekli tümleştirme kullanarak dağıtma kolaylaştırır. İşlevleri, BitBucket, Dropbox, GitHub ve Azure DevOps ile tümleşir. Bu, Azure'a bu tümleşik hizmetler tetikleyici dağıtımı birini kullanarak yapılan işlev kodunu burada güncelleştirmeleri bir iş akışı sağlar. Azure işlevleri'ne yeni başladıysanız, başlayan [Azure işlevlerine genel bakış](functions-overview.md).

Sürekli dağıtım, birden fazla ve sık gerçekleşen katkıların tümleştirildiği projeler için mükemmel bir seçenektir. Ayrıca işlev kodunuz üzerinde kaynak denetimi sağlamanıza olanak tanır. Aşağıdaki dağıtım kaynakları şu anda desteklenir:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Dış depo (Git veya Mercurial)
* [Yerel Git deposu](../app-service/deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Azure DevOps](https://azure.microsoft.com/services/devops/)

Dağıtımları, bir işlev başına uygulama başına yapılandırılır. Sürekli dağıtım etkinleştirildikten sonra portalda işlev kodunu erişimi kümesine *salt okunur*.

## <a name="continuous-deployment-requirements"></a>Sürekli dağıtım gereksinimleri

Dağıtım kaynağı, sürekli dağıtım ayarlama önce dağıtım kaynağınız yapılandırılmış ve işlev kodunuz olmalıdır. Verilen işlevin uygulama dağıtımında her işlevi dizin adı işlevin adını olduğu bir adlandırılmış alt dizinde bulunur.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Azure DevOps dağıtın edebilmek için Azure DevOps kuruluşunuzun Azure aboneliğinizle bağlamanız gerekir. Daha fazla bilgi için [Azure DevOps kuruluşunuz için fatura bilgilerini ayarlayın](https://docs.microsoft.com/azure/devops/organizations/billing/set-up-billing-for-your-organization-vs?view=vsts#set-up-billing-via-the-azure-portal).

## <a name="set-up-continuous-deployment"></a>Sürekli dağıtım ayarlama
Var olan bir işlev uygulaması için sürekli dağıtımını yapılandırmak için bu yordamı kullanın. Bir GitHub deposu ile tümleştirme adımları gösterir, ancak Azure DevOps veya diğer Dağıtım Hizmetleri için benzer adımları uygulayın.

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com), tıklayın **Platform özellikleri** ve **dağıtım seçenekleri**. 
   
    ![Sürekli dağıtımı](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Ardından **dağıtımları** dikey penceresinde **Kurulum**.
 
    ![Sürekli dağıtımı](./media/functions-continuous-deployment/setup-deployment-1.png)
   
3. İçinde **dağıtım kaynağı** dikey penceresinde tıklayın **Kaynak Seç**, seçtiğiniz dağıtım kaynağınız için bilgileri doldurun ve **Tamam**.
   
    ![Dağıtım kaynağını seçin](./media/functions-continuous-deployment/choose-deployment-source.png)

Sürekli dağıtım yapılandırdıktan sonra işlev uygulaması için dağıtım kaynağınız tüm dosya değişiklikleri kopyalanır ve sitenin tam bir dağıtım tetiklenir. Kaynaktaki dosyalardan güncelleştirildiğinde site yeniden dağıtıldı.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Bazı tipik dağıtım senaryoları şunlardır:

- [Hazırlama dağıtımı oluşturma](#staging)
- [Mevcut işlevleri için sürekli dağıtım Taşı](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Hazırlama dağıtımı oluşturma

İşlev uygulamaları henüz dağıtım yuvalarını desteklemiyor. Ancak, hazırlama ve üretim dağıtımları ayrı sürekli tümleştirmeyi kullanarak yönetmeye devam edebilirsiniz.

Yapılandırma ve hazırlama dağıtımı ile çalışma işlemi genellikle şu şekilde görünür:

1. Aboneliğiniz, biri üretim kodu için ve biri hazırlama için iki işlev uygulamaları oluşturun. 

2. Zaten yoksa, bir dağıtım kaynağı oluşturun. Bu örnekte [GitHub].

3. Üretim işlev uygulamanız için önceki adımları tamamlayın **sürekli dağıtım ayarlama** ve GitHub deponuza'nın ana dalı için dağıtım ayarlayın.
   
    ![Dağıtım dal seçin](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Hazırlama işlev uygulaması için bu adımı yineleyin ancak hazırlama dal yerine GitHub deponuzda seçin. Dağıtım kaynağınız dallanma desteklemiyorsa, farklı bir klasör kullanın.
    
5. Hazırlama dal veya klasör kodunuzda güncelleştirmeleri yapmak ve ardından bu değişiklikleri hazırlama dağıtım yansıtılır doğrulayın.

6. Test edildikten sonra birleştirme hazırlama dalından ana dala değiştirir. Bu birleştirme üretim işlev uygulaması için dağıtım tetikler. Dal dağıtım kaynağınız desteklemiyorsa, üretim klasöründeki dosyaları hazırlama klasördeki dosyaların üzerine yazın.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Mevcut işlevleri için sürekli dağıtım Taşı
Oluşturduğunuz ve portalda tutulan, FTP kullanarak, mevcut işlev kod dosyası indirmeniz gerekmez veya yerel Git deposu, önce yukarıda açıklandığı gibi sürekli dağıtım ayarlama ayarlayabilirsiniz mevcut işlevleri olduğunda. App Service ayarlarında işlev uygulamanız için bunu yapabilirsiniz. Dosyalarınızı İndirildikten sonra seçilen sürekli dağıtım kaynağınız karşıya yükleyebilirsiniz.

> [!NOTE]
> Sürekli tümleştirmeyi yapılandırdıktan sonra artık kaynak dosyalarınızı işlevler portalından düzenlemeniz mümkün olacaktır.

- [Nasıl yapılır: Dağıtım kimlik bilgilerini yapılandırma](#credentials)
- [Nasıl yapılır: FTP kullanarak dosyaları indirme](#downftp)
- [Nasıl yapılır: Yerel Git deposunu kullanarak dosyaları indirme](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Nasıl yapılır: Dağıtım kimlik bilgilerini yapılandırma
FTP veya yerel Git deposu ile işlevi uygulamanızdan dosyalarını indirebilmesinden önce siteye erişmek için kimlik bilgilerinizi yapılandırmanız gerekir. Kimlik bilgileri işlevi uygulama düzeyinde ayarlanır. Azure portalında dağıtım kimlik bilgilerini ayarlamak için aşağıdaki adımları kullanın:

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com), tıklayın **Platform özellikleri** ve **dağıtım kimlik bilgileri**.
   
    ![Yerel dağıtım kimlik bilgilerini ayarlama](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Bir kullanıcı adı ve parola yazın ve ardından tıklayın **Kaydet**. Şimdi, işlev uygulamanızı FTP ya da yerleşik Git deposu erişmek için bu kimlik bilgilerini kullanabilirsiniz.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Nasıl yapılır: FTP kullanarak dosyaları indirme

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com), tıklayın **Platform özellikleri** ve **özellikleri**, sonra da değerleri kopyalayın **FTP/dağıtım kullanıcısı**, **FTP konak adı**, ve **FTPS konak adı**.  

    **FTP/dağıtım kullanıcısı** FTP sunucusu için uygun bağlam sağlamak için uygulama adıyla birlikte portalında görüntülenen şekliyle girilmelidir.
   
    ![Dağıtım bilgilerinizi alın](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. FTP istemcinizde, bağlantı bilgilerini kullanın. uygulamanıza bağlanıp işlevleriniz için kaynak dosyalarını indirmek için toplanan.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Nasıl yapılır: Yerel Git deposunu kullanarak dosyaları indirme

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com), tıklayın **Platform özellikleri** ve **dağıtım seçenekleri**. 
   
    ![Sürekli dağıtımı](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Ardından **dağıtımları** dikey penceresinde **Kurulum**.
 
    ![Sürekli dağıtımı](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. İçinde **dağıtım kaynağı** dikey penceresinde tıklayın **yerel Git deposu** ve ardından **Tamam**.

3. İçinde **Platform özellikleri**, tıklayın **özellikleri** ve Git URL'si değerini not edin. 
   
    ![Sürekli dağıtımı](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Uygulamanızı yerel makinenizde Git kullanan bir komut istemi veya en sevdiğiniz Git aracını kullanarak depoyu kopyalayın. Git kopya komut şöyle görünür:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Dosyaları aşağıdaki örnekte olduğu gibi yerel bilgisayarınızda kopya için işlev uygulamanızın getirme:
   
        git pull origin master
   
    İstenirse, sağlamanız, [dağıtım kimlik bilgileri yapılandırılmış](#credentials).  

[GitHub]: https://github.com/

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
