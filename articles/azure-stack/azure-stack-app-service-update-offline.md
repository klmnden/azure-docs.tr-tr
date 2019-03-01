---
title: Azure uygulama hizmeti çevrimdışı güncelleştirmesi | Microsoft Docs
description: Azure Stack'te Azure App Service çevrimdışı güncelleştirmeye yönelik ayrıntılı kılavuz
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.openlocfilehash: 9d941c36499f851f20c41fa6dd01faf14e4192ba
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992779"
---
# <a name="offline-update-of-azure-app-service-on-azure-stack"></a>Azure Stack'te Azure App Service'in çevrimdışı güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!IMPORTANT]
> 1901 güncelleştirmesini, daha sonra Azure Stack tümleşik sistemi veya Azure App Service 1.5 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın. 

Bu makaledeki yönergeleri takip ederek, Yükseltme yapabileceğiniz [App Service kaynak sağlayıcısı](azure-stack-app-service-overview.md) olan bir Azure Stack ortamında dağıtılır:

* Internet'e bağlı değil
* Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli.

> [!IMPORTANT]
> Yükseltmeyi çalıştırmadan önce zaten tamamladığınızdan emin olun [Azure Stack kaynak sağlayıcısı üzerinde Azure App Service dağıtım](azure-stack-app-service-deploy-offline.md) ve, okuduğunuz [sürüm notları](azure-stack-app-service-release-notes-update-five.md), hangi eşlik 1.5 sürümünü, yeni işlevler, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için.

## <a name="run-the-app-service-resource-provider-installer"></a>App Service kaynak sağlayıcısı yükleyiciyi çalıştırın

App Service kaynak sağlayıcısı bir Azure Stack ortamında yükseltmek için bu görevleri tamamlamanız gerekir:

1. İndirme [uygulama hizmeti yükleyicisi](https://aka.ms/appsvcupdate4installer)
2. Çevrimdışı bir yükseltme paketi oluşturun.
3. App Service yükleyiciyi (appservice.exe) çalıştırın ve yükseltme tamamlayın.

Bu işlem sırasında yükseltme yapar:

* App Service'nın önceki dağıtım algılayın
* Depolamaya yükleme
* Tüm App Service rolleri Yükselt (denetleyicileri, yönetim, ön uç, yayımcı ve çalışan rolleri)
* App Service ölçek kümesi tanımlarını güncelleştir
* App Service kaynak sağlayıcısı bildirimini güncelleştir

## <a name="create-an-offline-upgrade-package"></a>Çevrimdışı bir yükseltme paketi oluşturma

App Service bağlantısı kesilmiş bir ortamda yükseltmek için önce Internet'e bağlı bir makinede çevrimdışı bir yükseltme paketi oluşturmanız gerekir.

1. Appservice.exe bir yönetici olarak çalıştır

    ![Uygulama hizmeti yükleyicisi][1]

2. Tıklayın **Gelişmiş** > **çevrimdışı Paket Oluştur**

    ![App Service gelişmiş yükleyici][2]

3. App Service yükleyici, çevrimdışı bir yükseltme paketi oluşturur ve ona yolunu görüntüler.  Tıklayabilirsiniz **Klasör Aç** klasörü dosya gezgini'nizi açın.

4. Yükleyici (AppService.exe) ve çevrimdışı yükseltme paketi için Azure Stack ana makinesi kopyalayın.

## <a name="complete-the-upgrade-of-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service'te olan tamamlayın

> [!IMPORTANT]
> App Service yükleyici, Azure Stack yönetici Azure Resource Manager uç noktası ulaşabileceği bir makineye çalıştırmanız gerekir.
>
>

1. Appservice.exe yönetici olarak çalıştırın.

    ![Uygulama hizmeti yükleyicisi][1]

2. Tıklayın **Gelişmiş** > **tamamlamak çevrimdışı yükleme veya yükseltme**.

    ![App Service gelişmiş yükleyici][2]

3. Önceden oluşturduğunuz çevrimdışı yükseltme paketinin konumuna göz atın ve ardından **sonraki**.

4. Gözden geçirin ve Microsoft yazılım lisans koşullarını kabul edin ve ardından **sonraki**.

5. Gözden geçirin ve üçüncü taraf lisans koşullarını kabul edin ve ardından **sonraki**.

6. Emin olun Azure Stack Azure Resource Manager uç noktasını ve Active Directory Kiracısı bilgilerdir doğru. Azure Stack geliştirme Seti'ni dağıtım sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri kabul edebilir. Ancak, Azure Stack dağıtıldığında seçenekleri özelleştirdiyseniz, bu pencereyi değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki kullanırsanız *mycloud.com*, Azure Stack Azure Resource Manager uç noktanız için değiştirmeniz gerekir *management.region.mycloud.com*. Bilgilerinizi onayladıktan sonra tıklayın **sonraki**.

    ![Azure Stack bulut bilgileri][3]

7. Sonraki sayfada:

   1. Tıklayın **Connect** düğmesinin yanındaki **Azure Stack aboneliklerini** kutusu.
        * Azure Active Directory (Azure AD) kullanıyorsanız, Azure AD yönetici hesabı ve Azure Stack dağıttığınızda sağladığınız parolayı girin. Tıklayın **oturum**.
        * Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, yönetici hesabı sağlayın. Örneğin, _cloudadmin@azurestack.local_. Parolanızı girin ve tıklayın **oturum**.
   2. İçinde **Azure Stack aboneliklerini** kutusunda **varsayılan sağlayıcı aboneliği**.
   3. İçinde **Azure Stack konumları** kutusunda, için dağıtmakta bölgeyi karşılık gelen konum seçin. Örneğin, **yerel** , Azure Stack Geliştirme Seti için dağıtma.
   4. Mevcut bir App Service dağıtımı algılanırsa, kaynak grubu ve depolama hesabı doldurulur ve devre dışı.
   5. Tıklayın **sonraki** yükseltme özetini gözden geçirmek için.

    ![Uygulama Hizmeti yüklemesi algılandı][4]

8. Özet sayfasında:
   1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanmanız **önceki** düğmelerini önceki sayfalarını ziyaret edin.
   2. Yapılandırmaları doğruysa, onay kutusunu işaretleyin.
   3. Yükseltmeyi başlatmak için tıklatın **sonraki**.

       ![Uygulama hizmet yükseltme özeti][5]

9. Yükseltme işlemi ilerleme durumu sayfası:
    1. Yükseltmenin ilerleme durumunu izleyin. Azure Stack üzerinde App Service'nın yükseltilmesi dağıtılmış rol örneklerinin sayısını bağımlı değişir.
    2. Yükseltme başarıyla tamamlandıktan sonra tıklayın **çıkış**.

        ![App Service yükseltme işlemi ilerleme durumu][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update-offline/app-service-exe.png
[2]: ./media/azure-stack-app-service-update-offline/app-service-exe-advanced.png
[3]: ./media/azure-stack-app-service-update-offline/app-service-azure-resource-manager-endpoints.png
[4]: ./media/azure-stack-app-service-update-offline/app-service-installation-detected.png
[5]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-summary.png
[6]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-complete.png

## <a name="next-steps"></a>Sonraki adımlar

Diğer de deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

* [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
* [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)
