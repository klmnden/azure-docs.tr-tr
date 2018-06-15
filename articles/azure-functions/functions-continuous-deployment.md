---
title: Azure işlevleri için sürekli dağıtım | Microsoft Docs
description: Azure işlevleri yayımlamak için sürekli dağıtım özellikleri Azure App Service'in kullanın.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: db10cd957f4dc59f787e2ac625355a96c888356e
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34735712"
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure İşlevleri için sürekli dağıtım
Azure işlevleri uygulama hizmeti sürekli tümleştirme kullanarak işlevi uygulamanızı dağıtmak kolaylaştırır. İşlevler, BitBucket, Dropbox, GitHub ve Visual Studio Team Services (VSTS) ile tümleşir. Bu, Azure'a bu tümleşik hizmetler tetikleyici dağıtımı birini kullanarak yapılan burada işlev kodu güncelleştirmeleri bir iş akışı sağlar. Azure işlevleri yeniyseniz, başlayın [Azure işlevlerine genel bakış](functions-overview.md).

Sürekli dağıtım, birden fazla ve sık gerçekleşen katkıların tümleştirildiği projeler için mükemmel bir seçenektir. Ayrıca, kaynak denetimi işlevleri kodunuzu tutmanıza sağlar. Aşağıdaki dağıtım kaynakları şu anda desteklenir:

* [Bitbucket](https://bitbucket.org/)
* [Açılan kutu](https://www.dropbox.com/)
* Dış depo (Git veya Mercurial)
* [Yerel Git deposu](../app-service/app-service-deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Visual Studio Team Services](https://www.visualstudio.com/team-services/)

Dağıtımları bir işlev başına uygulama temelinde yapılandırılır. Sürekli dağıtım etkinleştirildikten sonra portal işlev kodu erişim kümesine *salt okunur*.

## <a name="continuous-deployment-requirements"></a>Sürekli dağıtım gereksinimleri

Yapılandırılan dağıtım kaynağınız ve işlevleri kodunuz, sürekli dağıtım ayarlayın. önce dağıtım kaynağı olmalıdır. Verilen işlevin uygulama dağıtımı her işlevi dizin adı işlevin adı olduğu bir adlandırılmış alt dizininde bulunur.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

VSTS dağıtabilmesi için VSTS hesabınızı Azure aboneliğinizle bağlamanız gerekir. Daha fazla bilgi için bkz: [VSTS hesabınız için fatura ayarlama](https://docs.microsoft.com/vsts/billing/set-up-billing-for-your-account-vs?view=vsts#set-up-billing-via-the-azure-portal).

## <a name="set-up-continuous-deployment"></a>Sürekli dağıtım ayarlama
Varolan bir işlev uygulaması için sürekli dağıtım yapılandırmak için bu yordamı kullanın. GitHub depo ile tümleştirme adımları gösterir, ancak Visual Studio Team Services veya diğer Dağıtım Hizmetleri için benzer adımları uygulayın.

1. İşlev uygulamanızda [Azure portal](https://portal.azure.com), tıklatın **Platform özellikleri** ve **dağıtım seçenekleri**. 
   
    ![Sürekli dağıtım ayarlayın](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Ardından **dağıtımları** dikey penceresinde **Kurulum**.
 
    ![Sürekli dağıtım ayarlayın](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. İçinde **dağıtım kaynağı** dikey penceresinde tıklatın **Kaynak Seç**, seçtiğiniz dağıtım kaynağınız bilgileri doldurun ve **Tamam**.
   
    ![Dağıtım kaynağı seçin](./media/functions-continuous-deployment/choose-deployment-source.png)

Sürekli dağıtım yapılandırıldıktan sonra tüm dosya değişiklikleri dağıtım kaynağınızda işlev uygulaması kopyalanır ve bir sitenin tam dağıtım tetiklenir. Kaynak dosyalar güncelleştirildiğinde site yeniden dağıtıldı.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Bazı tipik dağıtım senaryoları şunlardır:

- [Hazırlama dağıtımı oluşturma](#staging)
- [Var olan işlevler için sürekli dağıtım taşıma](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Hazırlama dağıtımı oluşturma

İşlev uygulamalar henüz dağıtım yuvaları desteklemiyor. Ancak, yine de, sürekli tümleştirme kullanarak ayrı hazırlama ve üretim dağıtımları yönetebilirsiniz.

Yapılandırma ve hazırlama dağıtımı ile çalışmak için işlemi genellikle şuna benzer:

1. İki işlev uygulamalarının aboneliğiniz, üretim kodu için bir tane ve hazırlama için bir tane oluşturun. 

2. Zaten yoksa, bir dağıtım kaynağı oluşturun. Bu örnekte [GitHub].

3. Üretim işlevi uygulamanız için yukarıdaki adımları tamamlayın **sürekli dağıtım ayarlayın** ve GitHub deponuz ana dala dağıtım şube ayarlayın.
   
    ![Dağıtım şube seçin](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Hazırlama işlev uygulaması için bu adımı yineleyin, ancak hazırlama dal, bunun yerine, GitHub depo seçin. Dağıtım kaynağı dallanma desteklemiyorsa, farklı bir klasör kullanın.
    
5. Hazırlama şube veya klasör kodunuzda güncelleştirmeleri yapmak ve sonra bu değişiklikleri hazırlama dağıtımda yansıtılır doğrulayın.

6. Test sonra birleştirme ana dala hazırlama daldan değiştirir. Bu birleştirme üretim işlev uygulaması için dağıtım tetikler. Dağıtım kaynağı dalları desteklemiyorsa, üretim klasöründeki dosyaları hazırlama klasördeki dosyaların üzerine yazın.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Var olan işlevler için sürekli dağıtım taşıma
Ne zaman oluşturulur ve Portalı'nda saklanır, FTP kullanarak, varolan işlevi kod dosyaları karşıdan yüklemeniz veya yerel Git deposu, önce yukarıda açıklandığı gibi sürekli dağıtım ayarlayabilirsiniz var olan işlevler sahip. Uygulama hizmeti ayarlarında işlevi uygulamanız için bunu yapabilirsiniz. Dosyalarınızı İndirildikten sonra seçilen sürekli dağıtım kaynağınız karşıya yükleyebilirsiniz.

> [!NOTE]
> Sürekli Tümleştirme yapılandırdıktan sonra artık kaynak dosyalarınız işlevleri portalındaki düzenlemeniz mümkün olmayacaktır.

- [Nasıl yapılır: dağıtım kimlik bilgilerini yapılandırın](#credentials)
- [Nasıl yapılır: FTP kullanarak dosyaları indirme](#downftp)
- [Nasıl yapılır: yerel Git deposu kullanarak dosyaları indirme](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Nasıl yapılır: dağıtım kimlik bilgilerini yapılandırın
FTP veya yerel Git deposu ile işlevi uygulamanızdan dosyalarını indirmeden önce siteye erişmek için kimlik bilgilerinizi yapılandırmanız gerekir. Kimlik bilgileri işlevi uygulama düzeyinde ayarlanır. Azure portalında dağıtım kimlik bilgilerini ayarlamak için aşağıdaki adımları kullanın:

1. İşlev uygulamanızda [Azure portal](https://portal.azure.com), tıklatın **Platform özellikleri** ve **dağıtım kimlik bilgileri**.
   
    ![Yerel dağıtım kimlik bilgilerini ayarlama](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Bir kullanıcı adı ve parolasını yazın ve ardından **kaydetmek**. Bu kimlik bilgileri şimdi işlevi uygulamanızı FTP ya da yerleşik Git deposuna erişmek için de kullanabilirsiniz.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Nasıl yapılır: FTP kullanarak dosyaları indirme

1. İşlev uygulamanızda [Azure portal](https://portal.azure.com), tıklatın **Platform özellikleri** ve **özellikleri**, değerlerini kopyalayın **FTP/dağıtım kullanıcı**, **FTP konak adı**, ve **FTPS konak adı**.  

    **FTP/dağıtım kullanıcısı** FTP sunucusu için uygun bağlamı sağlamak için uygulama adı dahil olmak üzere Portalı'nda görüntülenen girilmelidir.
   
    ![Dağıtım bilgileri alma](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. Bağlantı bilgileri, bir FTP istemcisinden kullanın, uygulamanızın bağlanmak ve işlevlerinizi kaynak dosyalarını indirmek için toplanan.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Nasıl yapılır: yerel bir Git deposu kullanarak dosyaları indirme

1. İşlev uygulamanızda [Azure portal](https://portal.azure.com), tıklatın **Platform özellikleri** ve **dağıtım seçenekleri**. 
   
    ![Sürekli dağıtım ayarlayın](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Ardından **dağıtımları** dikey penceresinde **Kurulum**.
 
    ![Sürekli dağıtım ayarlayın](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. İçinde **dağıtım kaynağı** dikey penceresinde tıklatın **yerel Git deposu** ve ardından **Tamam**.

3. İçinde **Platform özellikleri**, tıklatın **özellikleri** ve Git URL'si değerini not edin. 
   
    ![Sürekli dağıtım ayarlayın](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Uygulamanızı yerel makinenizde Git algılayan bir komut istemi veya sık kullanılan Git aracını kullanarak depoyu kopyalayın. Git kopya komut şöyle görünür:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Dosyaları aşağıdaki örnekteki gibi yerel bilgisayarınızda kopyaya işlevi uygulamanızdan getirin:
   
        git pull origin master
   
    İstenirse, sağlamanız, [dağıtım kimlik bilgileri yapılandırılmış](#credentials).  

[GitHub]: https://github.com/

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
