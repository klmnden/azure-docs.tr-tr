---
title: Azure Stack'te Azure uygulama hizmeti güncelleştirmesi | Microsoft Docs
description: Azure Stack'te Azure App Service güncelleştirmeye yönelik ayrıntılı kılavuz
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: anwestg
ms.openlocfilehash: f12eb7d74d9c47567c72cddda8d2813f394fb1de
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42059238"
---
# <a name="update-azure-app-service-on-azure-stack"></a>Azure Stack'te Azure uygulama hizmeti güncelleştirmesi

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!IMPORTANT]
> Azure Stack tümleşik sisteminize 1807 güncelleştirmesini veya Azure App Service 1.3 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.
>
>

Bu makaledeki yönergeleri takip ederek, Yükseltme yapabileceğiniz [App Service kaynak sağlayıcısı](azure-stack-app-service-overview.md) Internet'e bağlı bir Azure Stack ortamında dağıtılır.

> [!IMPORTANT]
> Yükseltmeyi çalıştırmadan önce zaten tamamladığınızdan emin olun [Azure Stack kaynak sağlayıcısı üzerinde Azure App Service dağıtımı](azure-stack-app-service-deploy.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>App Service kaynak sağlayıcısı yükleyiciyi çalıştırın

Bu işlem sırasında yükseltme yapar:

* App Service'nın önceki dağıtım algılayın
* Tüm güncelleştirme paketlerini ve dağıtılması için tüm OSS kitaplıklarının yeni sürümlerini hazırlanma
* Depolamaya yükleme
* Tüm App Service rolleri Yükselt (denetleyicileri, yönetim, ön uç, yayımcı ve çalışan rolleri)
* App Service ölçek kümesi tanımlarını güncelleştir
* App Service kaynak sağlayıcısı bildirimini güncelleştir

> [!IMPORTANT]
> App Service yükleyici, Azure Stack yönetici Azure Resource Manager uç nokta ulaşabileceği bir makineye çalıştırmanız gerekir.
>
>

Azure Stack üzerinde App Service'te dağıtımınızın yükseltmek için aşağıdaki adımları izleyin:

1. İndirme [uygulama hizmeti yükleyicisi](https://aka.ms/appsvcupdate3installer)

2. Appservice.exe bir yönetici olarak çalıştır

    ![Uygulama hizmeti yükleyicisi][1]

3. Tıklayın **App Service dağıtın veya en son sürüme yükseltin.**

4. Gözden geçirin ve Microsoft yazılım lisans koşullarını kabul edin ve ardından **sonraki**.

5. Gözden geçirin ve üçüncü taraf lisans koşullarını kabul edin ve ardından **sonraki**.

6. Emin olun Azure Stack Azure Resource Manager uç noktasını ve Active Directory Kiracısı bilgilerdir doğru. Azure Stack geliştirme Seti'ni dağıtım sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri kabul edebilir. Ancak, Azure Stack dağıtıldığında seçenekleri özelleştirdiyseniz, bu pencerede, değişimi yansıtmak için değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki kullanırsanız *mycloud.com*, Azure Stack Azure Resource Manager uç noktanız için değiştirmeniz gerekir *management.region.mycloud.com*. Bilgilerinizi onayladıktan sonra tıklayın **sonraki**.

    ![Azure Stack bulut bilgileri][2]

7. Sonraki sayfada:

   1. Tıklayın **Connect** düğmesinin yanındaki **Azure Stack aboneliklerini** kutusu.
        * Azure Active Directory (Azure AD) kullanıyorsanız, Azure AD yönetici hesabı ve Azure Stack dağıttığınızda sağladığınız parolayı girin. Tıklayın **oturum**.
        * Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, yönetici hesabı sağlayın. Örneğin, *cloudadmin@azurestack.local*. Parolanızı girin ve tıklayın **oturum**.
   2. İçinde **Azure Stack aboneliklerini** kutusunda **varsayılan sağlayıcı aboneliği**.
   3. İçinde **Azure Stack konumları** kutusunda, için dağıtmakta bölgeyi karşılık gelen konum seçin. Örneğin, **yerel** , Azure Stack Geliştirme Seti için dağıtma.
   4. Mevcut bir App Service dağıtımı belirlendiyse, kaynak grubu ve depolama hesabı doldurulur ve devre dışı.
   5. Tıklayın **sonraki** yükseltme özetini gözden geçirmek için.

    ![Uygulama Hizmeti yüklemesi algılandı][3]

8. Özet sayfasında:
   1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanmanız **önceki** düğmelerini önceki sayfalarını ziyaret edin.
   2. Yapılandırmaları doğruysa, onay kutusunu işaretleyin.
   3. Yükseltmeyi başlatmak için tıklatın **sonraki**.

       ![Uygulama hizmet yükseltme özeti][4]

9. Yükseltme işlemi ilerleme durumu sayfası:
    1. Yükseltmenin ilerleme durumunu izleyin. Azure Stack üzerinde App Service'nın yükseltilmesi dağıtılmış rol örneklerinin sayısını bağımlı değişir.
    2. Yükseltme başarıyla tamamlandıktan sonra tıklayın **çıkış**.

        ![App Service yükseltme işlemi ilerleme durumu][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>Sonraki adımlar

Diğer de deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

* [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
* [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)
