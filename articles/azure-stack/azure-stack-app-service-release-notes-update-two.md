---
title: Azure Stack üzerinde App Service'te güncelleştirme 2 sürüm notları | Microsoft Docs
description: Güncelleştirmede nedir hakkında iki Azure Stack üzerinde App Service'te, bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: anwestg
ms.reviewer: sethm
ms.lastreviewed: 05/18/2018
ms.openlocfilehash: 98d084794accb99e0fecac85f7de861ec35bc15e
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446025"
---
# <a name="app-service-on-azure-stack-update-2-release-notes"></a>Güncelleştirme 2 sürüm notları Azure Stack üzerinde App Service'e

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, Azure App Service'te düzeltmeleri ve geliştirmeleri ve Azure Stack güncelleştirme 2 bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları (yükleme sonrası) yapı ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> 1804 güncelleştirme, Azure Stack tümleşik sistemi için geçerli veya Azure App Service 1.2 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.
>
>

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 2 derleme numarası üzerinde App Service, **72.0.13698.10**

### <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Azure Stack'te Azure App Service'in yeni dağıtımlar artık gerektiren bir [üç konulu bir joker sertifikası](azure-stack-app-service-before-you-get-started.md#get-certificates) iyileştirmeleri, SSO için Kudu artık Azure App Service'te işlenme nedeniyle. Yeni konu  **\*. sso.appservice.\< Bölge\>.\< DomainName\>.\< Uzantı\>**
>
>

Başvurmak [önce Get Started belgeleri](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 2 üzerinde'Azure App Service, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- Güncelleştirmeleri **App Service Kiracı, yönetici, İşlevler portalları ve Kudu Araçları**. Azure Stack portalı SDK sürümü ile tutarlı.

- Güncelleştirmeleri **Azure işlevleri çalışma zamanı** için **v1.0.11612**.

- Güvenilirlik ve sık karşılaşılan sorunları daha kolay tanılanması etkinleştirme hata geliştirmek için çekirdek hizmet güncelleştirmeleri.

- **Aşağıdaki uygulama çerçeveleri ve araçları güncelleştirmeleri**:
  - Eklenen .NET Framework 4.7.1
  - Eklenen **Node.JS** sürümleri:
    - NodeJS 6.12.3
    - NodeJS 8.9.4
    - NodeJS 8.10.0
    - NodeJS 8.11.1
  - Eklenen **NPM** sürümleri:
    - 5.6.0
  - Azure App Service Genel bulutta tutarlı olması için güncelleştirilmiş .NET Core bileşenleri.
  - Güncelleştirilmiş Kudu

- Dağıtımının otomatik takas yuvası özelliğinin etkin - [otomatik yapılandırma değiştirme](https://docs.microsoft.com/azure/app-service/deploy-staging-slots#configure-auto-swap)

- Test üretim özelliği etkin - [üretimde test giriş](https://azure.microsoft.com/resources/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

- Etkin - Azure işlevleri proxy'leri [iş ile Azure işlev proxy'leri](https://docs.microsoft.com/azure/azure-functions/functions-proxies)

- App Service yönetim uzantısı UX desteklemek için eklendi:
  - Gizli anahtar döndürme
  - Sertifika döndürme
  - Sistem kimlik bilgilerini döndürme
  - Bağlantı dizesi döndürme

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Çalışanları App Service, var olan bir sanal ağda dağıtılır ve dosya sunucusu yalnızca özel ağda kullanılabilir dosya sunucusuna erişemiyor.

Mevcut bir sanal ağ ve dosya sunucunuza bağlanmak için bir dahili IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Bunu yapmak için Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
* Kaynak: Herhangi biri
* Kaynak bağlantı noktası aralığı: *
* Hedef: IP Adresleri
* Hedef IP adresi aralığı: Dosya sunucusu için IP aralığı
* Hedef bağlantı noktası aralığı: 445
* Protokol: TCP
* Eylem: İzin Ver
* Önceliği: 700
* Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Azure App Service'te çalışan bulut yöneticileri için bilinen sorunlar

Belgeye başvurun [Azure Stack 1804 sürüm notları](azure-stack-update-1804.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service'e genel bakış için bkz. [Azure App Service, Azure Stack genel bakış](azure-stack-app-service-overview.md).
- Azure Stack üzerinde App Service'e dağıtmak hazırlanması hakkında daha fazla bilgi için bkz. [Azure Stack'te App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
