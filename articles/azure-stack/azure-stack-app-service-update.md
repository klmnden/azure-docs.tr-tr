---
title: "Azure uygulama hizmeti Azure yığında güncelleştirme | Microsoft Docs"
description: "Azure uygulama hizmeti Azure yığında güncelleştirme hakkında ayrıntılı kılavuz"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: anwestg
ms.openlocfilehash: 7c5c77e57a1cfc6b99c0f7baa91ec8de92be37ec
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="update-azure-app-service-on-azure-stack"></a>Azure uygulama hizmeti Azure yığında güncelleştir

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*


Bu makaledeki yönergeleri izleyerek, Yükseltme yapabileceğiniz [uygulama hizmeti kaynak sağlayıcısı](azure-stack-app-service-overview.md) Internet'e bağlı bir Azure yığın ortamda dağıtılabilir.

> [!IMPORTANT]
> Yükseltme çalıştırılmadan önce zaten tamamladığınızdan emin olun [Azure yığın kaynak sağlayıcısı Azure uygulama hizmeti dağıtımı](azure-stack-app-service-deploy.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Uygulama hizmeti kaynak sağlayıcısı yükleyiciyi çalıştırın

Bu işlem sırasında yükseltme olur:

* Uygulama hizmeti önceki dağıtımını Algıla
* Tüm güncelleştirme paketlerini ve yeni sürümlerini dağıtılacak tüm OSS kitaplıkları hazırlama
* Depolama alanına yükleme
* Tüm uygulama hizmeti rolleri yükseltme (denetleyicileri, yönetim, ön uç, yayımcı ve çalışan rolleri)
* Uygulama hizmeti ölçek kümesi tanımları güncelleştir
* Uygulama hizmeti kaynak sağlayıcısı bildirimini güncelleştir

> [!IMPORTANT]
> Uygulama Hizmeti Yükleyici hangi Azure yığın yönetici Azure Resource Manager endpoint ulaşabileceği bir makinede çalıştırılması gerekir.
>
>

Azure yığın uygulama hizmeti dağıtımını yükseltmek için aşağıdaki adımları izleyin:

1. Karşıdan [uygulama hizmeti yükleyici](https://aka.ms/appsvcupdate1installer)

2. Appservice.exe bir yönetici olarak çalıştır

    ![Uygulama Hizmeti Yükleyici][1]

3. Tıklatın **uygulama hizmeti Dağıt veya en son sürüme yükseltin.**

4. Gözden geçirin ve Microsoft Yazılım Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

5. Gözden geçirin ve üçüncü taraf Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

6. Olduğundan emin olun Azure yığın Azure Kaynak Yöneticisi uç noktası ve Active Directory Kiracı bilgileri doğru. Varsayılan ayarları Azure yığın Geliştirme Seti dağıtımı sırasında kullanılan, varsayılan değerleri kabul edebilir. Ancak, Azure yığın dağıtıldığında seçenekleri özelleştirdiyseniz, yansıtmak üzere bu penceresindeki değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki kullanırsanız *mycloud.com*, Azure yığın Azure Resource Manager uç noktanız için değiştirmeniz gerekir *management.region.mycloud.com*. Bilgilerinizi doğruladıktan sonra tıklatın **sonraki**.

    ![Azure yığın bulut bilgileri][2]

7. Sonraki sayfada:

   1. Tıklatın **Bağlan** düğmesine **Azure yığın abonelikleri** kutusu.
        * Azure Active Directory (Azure AD) kullanıyorsanız, Azure AD yönetici hesabı ve Azure yığın dağıtıldığında, verdiğiniz parolayı girin. Tıklatın **oturum**.
        * Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, yönetici hesabı sağlayın. Örneğin,  *cloudadmin@azurestack.local* . Parolanızı girin ve tıklayın **oturum**.
   2. İçinde **Azure yığın abonelikleri** kutusunda, aboneliğinizi seçin.
   3. İçinde **Azure yığın konumu** kutusunda, dağıtımına bölgeyi karşılık gelen konumu seçin. Örneğin, seçin **yerel** varsa Azure yığın Geliştirme Seti dağıtma.
   4. Var olan bir uygulama hizmeti dağıtıma bulunmuşsa, kaynak grubu ve depolama hesabı doldurulur ve devre dışı.
   5. Tıklatın **sonraki** yükseltme özetini gözden geçirmek için.

    ![Uygulama Hizmeti yüklemesi algılandı][3]

8. Özet sayfasında:
   1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanın **önceki** düğmeleri önceki sayfaları ziyaret edin.
   2. Yapılandırmaları doğruysa, onay kutusunu seçin.
   3. Yükseltme işlemini başlatmak için tıklatın **sonraki**.

       ![Uygulama hizmeti yükseltme özeti][4]

9. Yükseltme işlemi ilerleme durumu sayfası:
    1. Yükseltme işlemi ilerleme durumu izleyin. Azure yığın uygulama hizmeti yükseltilmesi sırasında dağıtılan rol örneklerinin sayısını bağımlı değişir.
    2. Yükseltme başarıyla tamamlandıktan sonra **çıkış**.

        ![Uygulama hizmeti yükseltme işlemi ilerleme durumu][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca diğer deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

* [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
* [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)
