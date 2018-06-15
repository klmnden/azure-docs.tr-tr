---
title: Azure yığın uygulama hizmeti güncelleştirme 2 sürüm notları | Microsoft Docs
description: Güncelleştirmede nedir hakkında Azure yığın uygulama hizmeti için iki, bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.date: 05/18/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 8e1790b7d0b3a210a9142fc8580ff8ed4d64311c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34360408"
---
# <a name="app-service-on-azure-stack-update-2-release-notes"></a>Güncelleştirme 2 sürüm notları Azure yığın uygulama hizmeti

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu sürüm notları, Azure App Service düzeltmeler ve geliştirmeler Azure yığın güncelleştirme 2 ve bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları yapı (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> Azure tümleşik yığını sisteminizi 1804 güncelleştirmesini veya Azure App Service 1.2 dağıtmadan önce en son Azure yığın Geliştirme Seti dağıtın.
>
>

## <a name="build-reference"></a>Derleme başvurusu

Uygulama hizmeti Azure yığın güncelleştirme 2 yapı numarası olduğu **72.0.13698.10**

### <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Azure uygulama hizmeti Azure yığında yeni dağıtımı şimdi gerektiren bir [üç konulu bir joker sertifika](azure-stack-app-service-before-you-get-started.md#get-certificates) içinde SSO Kudu için şimdi işlenir Azure App Service'te şekilde geliştirmeleri nedeniyle. Yeni konu  **\*. sso.appservice.\< Bölge\>.\< DomainName\>.\< uzantısı\>**
>
>

Başvurmak [önce Get Started belgelerine](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure uygulama hizmeti Azure yığın güncelleştirme 2 giderir ve aşağıdaki geliştirmeleri içerir:

- Güncelleştirmeleri **uygulama hizmet Kiracı, yönetim işlevleri portalları ve Kudu Araçları**. Azure yığın portalı SDK sürümü ile tutarlı.

- Güvenilirlik ve daha kolay tanılama yaygın sorunların çoğunu etkinleştirme Mesajlaşma hata geliştirmek için çekirdek hizmet güncelleştirmeleri.

- **Aşağıdaki uygulama çerçeveleri ve Araçları güncelleştirmelerini**:
  - Eklenen .net Framework 4.7.1
  - Eklenen **Node.JS** sürümleri:
    - NodeJS 6.12.3
    - NodeJS 8.9.4
    - NodeJS 8.10.0
    - NodeJS 8.11.1
  - Eklenen **NPM** sürümleri:
    - 5.6.0
  - Güncelleştirilmiş .net Core genel bulutta Azure uygulama hizmeti ile tutarlı olacak şekilde bileşenleri.
  - Güncelleştirilmiş Kudu

- Dağıtımın otomatik takas yuvası özelliğinin etkin - [otomatik yapılandırma değiştirme](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing#configure-auto-swap)

- Üretim özelliğinin etkin - test [üretimde test giriş](https://azure.microsoft.com/resources/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

- Azure işlevleri etkin - proxy'leri [Azure işlevleri proxy ile çalışma](https://docs.microsoft.com/en-us/azure/azure-functions/functions-proxies)

- Uygulama hizmeti yönetici uzantısını UX desteklemek için eklendi:
  - Gizli döndürme
  - Sertifika döndürme
  - Sistem kimlik bilgileri döndürme
  - Bağlantı dizesi döndürme

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Uygulama hizmeti var olan bir sanal ağda dağıtılmış ve dosya sunucusu yalnızca özel ağda kullanılabilir dosya sunucusuna erişemedi çalışanlardır.

Varolan bir sanal ağı ve dosya sunucusuna bağlanmak için bir iç IP adresi dağıtmak seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Bunu yapmak için yönetim portalında WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
 * Kaynak: tüm
 * Kaynak bağlantı noktası aralığı: *
 * Hedef: IP adresleri
 * Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
 * Hedef bağlantı noktası aralığı: 445
 * Protokol: TCP
 * Eylem: izin ver
 * Öncelik: 700
 * Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bulut Azure uygulama hizmeti Azure yığında işletim yöneticileri için bilinen sorunlar

Belgelerde başvurmak [Azure yığın 1804 sürüm notları](azure-stack-update-1804.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure uygulama hizmeti genel bakış için bkz: [Azure uygulama hizmeti Azure yığın genel bakış](azure-stack-app-service-overview.md).
- Azure yığın uygulama hizmeti dağıtmak hazırlanması hakkında daha fazla bilgi için bkz: [Azure yığın uygulama hizmeti ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
