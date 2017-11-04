---
title: "Adlandırılmış kimlik doğrulama kimlik bilgilerini ayarlayın | Microsoft Docs"
description: "Visual Studio Azure uygulamaya Visual Studio'dan yayımlamak ya da var olan bir bulut hizmetini izlemeyi Azure, isteklerine kimlik doğrulaması için kullandığı kimlik bilgilerini sağlamanız öğrenin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 953b1aa459ddf5b7be00b9d32432e6dda97143e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-named-authentication-credentials"></a>Adlandırılmış kimlik doğrulama kimlik bilgilerini ayarlayın
Azure uygulama Visual Studio'dan yayımlamak ya da var olan bir bulut hizmetini izlemek için Visual Studio Azure isteklerine kimliğini doğrulamak için kullanabileceğiniz kimlik bilgilerini sağlamanız gerekir. Visual Studio'da burada bu kimlik bilgilerini sağlamak üzere oturum içinde çeşitli yerlerde vardır. Örneğin, Sunucu Gezgini'nden için kısayol menüsünü açarak **Azure** düğümü ve select **Microsoft Azure aboneliğine bağlanma**. Oturum açtığınızda Visual Studio'da Azure hesabınızla ilişkili abonelik bilgileri kullanılabilir. Hiçbir şey, daha fazla yapmanıza gerek yoktur.

Azure Araçları, kimlik bilgileri sağlama daha eski bir yol da destekler: Abonelik dosyasını (.publishsettings) kullanarak. Bu makalede Azure SDK 2.2 hala desteklenmektedir bu yöntem açıklanır.

Aşağıdaki öğeler, Azure için kimlik doğrulaması için gereklidir:

* Abonelik kimliği
* Geçerli bir X.509 v3 sertifikası

> [!NOTE]
> X.509 v3 sertifikası'nın anahtar uzunluğu en az 2.048 bit olmalıdır. Azure, bu gereksinimi karşılamıyor veya geçerli olmayan herhangi bir sertifikayı reddeder.
>
>

Visual Studio abonelik Kimliğinizi sertifika verileri ile birlikte kimlik bilgileri olarak kullanır. Uygun kimlik bilgilerini içeren bir ortak anahtar sertifikası için abonelik dosyasında (.publishsettings dosyasını) başvurulur. Abonelik dosyası birden fazla aboneliğiniz için kimlik bilgileri içerebilir.

Abonelik bilgileri düzenleyebilirsiniz **yeni abonelik** veya **Düzenle abonelik** iletişim kutusunda, bu makalenin sonraki bölümlerinde açıklandığı gibi.

Bir sertifika kendiniz oluşturmak istiyorsanız,'ndaki yönergeleri başvurabilirsiniz [oluşturun ve bir yönetim sertifikası için Azure yükleyin](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) ve sertifikayı el ile karşıya [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Bulut hizmetleri yönetmek için Visual Studio gerektirir bu kimlik bilgileri isteği Azure storage hizmetlerine karşı kimlik doğrulaması için gerekli kimlik bilgilerini değil.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>İçeri aktarma, ayarlama veya Visual Studio'da kimlik doğrulama bilgilerini Düzenle

1. Server Explorer'da için kısayol menüsünü açın **Azure** düğümü ve select **yönetin ve filtre abonelikleri**.
2. Seçin **sertifikaları** sekmesini tıklatın ve aşağıdaki yöntemlerden birini kullanın:

    * Seçin **alma** açmak için **alma Microsoft Azure abonelikleri** iletişim kutusu. Burada, abonelikleri dosyayı indirebilirsiniz yüklü abonelik için indirme konumuna göz atın ve kimlik doğrulaması kullanmak için alma.
    * Seçin **yeni** açmak için **yeni abonelik** iletişim kutusu. Burada, kimlik doğrulaması kullanmak için yeni bir abonelik ayarlayabilirsiniz.
    * Seçin **Düzenle** (etkin aboneliğinizi seçtikten sonra) açmak için **Düzenle abonelik** iletişim kutusu. Burada, kimlik doğrulaması kullanmak için mevcut bir aboneliğe düzenleyebilirsiniz. 

Aşağıdaki yordam varsayar **yeni abonelik** iletişim kutusunu açın.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Visual Studio'da kimlik doğrulama kimlik bilgilerini ayarlamak için
1. İçinde **kimlik doğrulaması için varolan bir sertifikayı seçin** listesinde, bir sertifika seçin.
2. Seçin **tam yolunu kopyalamak** bağlantı. Sertifika (.cer dosyası) yolunu Panoya kopyalandı.

   > [!IMPORTANT]
   > Azure uygulamanızı Visual Studio'dan yayımlamak için bu sertifikayı karşıya [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   >
   >
3. Azure portalına sertifikayı karşıya yüklemek için:

   a. [Azure portalı](http://go.microsoft.com/fwlink/p/?LinkID=525040) açın.
   
   b. İstenirse, portalında oturum açın ve sonra gidin **ayarları** > **yönetim sertifikaları**.
   
   c. İçinde **yönetim sertifikaları** bölmesinde, **karşıya**.
   
   d. Azure aboneliğinizi seçin, oluşturulan yeni .cer dosyasının tam yolunu yapıştırın ve seçin **karşıya**.

## <a name="next-steps"></a>Sonraki adımlar
* [Web uygulamaları genel bakış](https://docs.microsoft.com/azure/app-service/)
* [Uygulamanızı Azure App Service'e dağıtma](https://docs.microsoft.com/en-us/azure/app-service/app-service-deploy-local-git) 
* [Visual Studio kullanarak Web İşleri’ni dağıtma](https://docs.microsoft.com/en-us/azure/app-service/websites-dotnet-deploy-webjobs)
* [Oluşturma ve bir bulut hizmeti dağıtma](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)