---
title: "Azure uygulama hizmeti çevrimdışı güncelleştirme | Microsoft Docs"
description: "Azure uygulama hizmeti Azure yığında çevrimdışı güncelleştirme hakkında ayrıntılı kılavuz"
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
ms.openlocfilehash: 5b71c9fd58636e9871bf592904d5dca4bc0ec966
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="offline-update-of-azure-app-service-on-azure-stack"></a>Azure uygulama hizmeti Azure yığında çevrimdışı güncelleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu makaledeki yönergeleri izleyerek, Yükseltme yapabileceğiniz [uygulama hizmeti kaynak sağlayıcısı](azure-stack-app-service-overview.md) olan bir Azure yığın ortamda dağıtılabilir:

* Internet'e bağlı değil
* Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli.

> [!IMPORTANT]
> Yükseltme çalıştırılmadan önce zaten tamamladığınızdan emin olun [Azure yığın kaynak sağlayıcısı Azure uygulama hizmeti dağıtımı](azure-stack-app-service-deploy-offline.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Uygulama hizmeti kaynak sağlayıcısı yükleyiciyi çalıştırın

Uygulama hizmeti kaynak sağlayıcısı Azure yığın ortamında yükseltmek için bu görevleri tamamlamanız gerekir:

1. Karşıdan [uygulama hizmeti yükleyici](https://aka.ms/appsvcupdate1installer)
2. Çevrimdışı bir yükseltme paketi oluşturun.
3. Uygulama Hizmeti Yükleyici (appservice.exe) çalıştırın ve yükseltmeyi tamamlayın.

Bu işlem sırasında yükseltme olur:

* Uygulama hizmeti önceki dağıtımını Algıla
* Depolama alanına yükleme
* Tüm uygulama hizmeti rolleri yükseltme (denetleyicileri, yönetim, ön uç, yayımcı ve çalışan rolleri)
* Uygulama hizmeti ölçek kümesi tanımları güncelleştir
* Uygulama hizmeti kaynak sağlayıcısı bildirimini güncelleştir

## <a name="create-an-offline-upgrade-package"></a>Çevrimdışı bir yükseltme paketi oluşturun.

Uygulama hizmeti bağlantısı kesilmiş bir ortamda yükseltmek için önce Internet'e bağlı bir makinede çevrimdışı bir yükseltme paketi oluşturmalısınız.

1. Appservice.exe bir yönetici olarak çalıştır

    ![Uygulama Hizmeti Yükleyici][1]

2. Tıklatın **Gelişmiş** > **çevrimdışı Paket Oluştur**

    ![Gelişmiş uygulama hizmeti yükleyici][2]

3. Uygulama Hizmeti Yükleyici çevrimdışı bir yükseltme paketi oluşturur ve yolunu görüntüler.  Tıklayabilirsiniz **Klasör Aç** dosya Gezgini'nde klasörü açın.

4. Yükleyici (AppService.exe) ve çevrimdışı yükseltme paketi Azure yığın ana makinenizdeki kopyalayın.

## <a name="complete-the-upgrade-of-app-service-on-azure-stack"></a>Azure yığın uygulama hizmeti yükseltmeyi tamamlayın

> [!IMPORTANT]
> Uygulama Hizmeti Yükleyici hangi Azure yığın yönetici Azure Kaynak Yöneticisi uç noktası ulaşabileceği bir makinede çalıştırılması gerekir.
>
>

1. Appservice.exe yönetici olarak çalıştırın.  

    ![Uygulama Hizmeti Yükleyici][1]

2. Tıklatın **Gelişmiş** > **tamamlamak çevrimdışı yükleme veya yükseltme**.

    ![Gelişmiş uygulama hizmeti yükleyici][2]

3. Daha önce oluşturduğunuz çevrimdışı yükseltme paketinin konuma göz atın ve ardından **sonraki**.

4. Gözden geçirin ve Microsoft Yazılım Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

5. Gözden geçirin ve üçüncü taraf Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

6. Olduğundan emin olun Azure yığın Azure Kaynak Yöneticisi uç noktası ve Active Directory Kiracı bilgileri doğru. Varsayılan ayarları Azure yığın Geliştirme Seti dağıtımı sırasında kullanılan, varsayılan değerleri kabul edebilir. Ancak, Azure yığın dağıtıldığında seçenekleri özelleştirdiyseniz, yansıtmak üzere bu penceresindeki değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki kullanırsanız *mycloud.com*, Azure yığın Azure Resource Manager uç noktanız için değiştirmeniz gerekir *management.region.mycloud.com*. Bilgilerinizi doğruladıktan sonra tıklatın **sonraki**.

    ![Azure yığın bulut bilgileri][3]

7. Sonraki sayfada:

   1. Tıklatın **Bağlan** düğmesine **Azure yığın abonelikleri** kutusu.
        * Azure Active Directory (Azure AD) kullanıyorsanız, Azure AD yönetici hesabı ve Azure yığın dağıtıldığında, verdiğiniz parolayı girin. Tıklatın **oturum**.
        * Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, yönetici hesabı sağlayın. Örneğin,  *cloudadmin@azurestack.local* . Parolanızı girin ve tıklayın **oturum**.
   2. İçinde **Azure yığın abonelikleri** kutusunda, aboneliğinizi seçin.
   3. İçinde **Azure yığın konumu** kutusunda, dağıtımına bölgeyi karşılık gelen konumu seçin. Örneğin, seçin **yerel** varsa Azure yığın Geliştirme Seti dağıtma.
   4. Var olan bir uygulama hizmeti dağıtıma bulunmuşsa, kaynak grubu ve depolama hesabı doldurulur ve devre dışı.
   5. Tıklatın **sonraki** yükseltme özetini gözden geçirmek için.

    ![Uygulama Hizmeti yüklemesi algılandı][4]

8. Özet sayfasında:
   1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanın **önceki** düğmeleri önceki sayfaları ziyaret edin.
   2. Yapılandırmaları doğruysa, onay kutusunu seçin.
   3. Yükseltme işlemini başlatmak için tıklatın **sonraki**.

       ![Uygulama hizmeti yükseltme özeti][5]

9. Yükseltme işlemi ilerleme durumu sayfası:
    1. Yükseltme işlemi ilerleme durumu izleyin. Azure yığın uygulama hizmeti yükseltilmesi sırasında dağıtılan rol örneklerinin sayısını bağımlı değişir.
    2. Yükseltme başarıyla tamamlandıktan sonra **çıkış**.

        ![Uygulama hizmeti yükseltme işlemi ilerleme durumu][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update-offline/app-service-exe.png
[2]: ./media/azure-stack-app-service-update-offline/app-service-exe-advanced.png
[3]: ./media/azure-stack-app-service-update-offline/app-service-azure-resource-manager-endpoints.png
[4]: ./media/azure-stack-app-service-update-offline/app-service-installation-detected.png
[5]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-summary.png
[6]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-complete.png

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca diğer deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

* [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
* [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)
