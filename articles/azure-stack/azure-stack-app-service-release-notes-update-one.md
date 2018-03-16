---
title: "Azure yığın güncelleştirme bir uygulama hizmeti | Microsoft Docs"
description: "Azure yığın uygulama hizmeti için güncelleştirme nedir hakkında bilgi edinin bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu."
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 0c33c8fdefbb27ba8414e58bed1b42ee7aaba88a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="app-service-on-azure-stack-update-one-release-notes"></a>Azure yığın uygulama hizmeti güncelleştirmesi bir sürüm notları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu sürüm notları, Azure App Service düzeltmeler ve geliştirmeler Azure yığın güncelleştirme 1 ve bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları yapı (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> Azure tümleşik yığını sisteminizi 1802 güncelleştirmesini veya Azure uygulama hizmeti dağıtmadan önce en son Azure yığın Geliştirme Seti dağıtın.
>
>

## <a name="build-reference"></a>Derleme başvurusu

Uygulama hizmeti Azure yığın güncelleştirme 1 yapı numarası olduğu **69.0.13698.9**

### <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Azure uygulama hizmeti Azure yığında şimdi gerektiren bir [üç konulu bir joker sertifika](azure-stack-app-service-before-you-get-started.md#get-certificates) içinde SSO Kudu için şimdi işlenir Azure App Service'te şekilde geliştirmeleri nedeniyle.  Yeni konu ** *. sso.appservice.<region>. <domainname>.<extension>**
>
>

Başvurmak [önce Get Started belgelerine](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure uygulama hizmeti Azure yığın güncelleştirme 1 aşağıdaki geliştirmeleri ve düzeltmeler içerir:

- **Yüksek kullanılabilirlik, Azure App Service** -arasında dağıtılacak şekilde Azure yığın 1802 etkin güncelleştirme iş yükleri hata etki alanları.  Bu nedenle uygulama hizmeti hata etki alanlarında dağıtılacak hataya dayanıklı olması mümkün altyapısıdır.  Azure yığın 1802 önce tamamlandı dağıtımlar için uygulanan güncelleştirme başvurmak ancak varsayılan olarak bu özellik Azure App Service, tüm yeni dağıtımlar sahip [App Service hata etki alanı belgeleri](azure-stack-app-service-fault-domain-update.md)

- **Mevcut sanal ağda dağıtmak** -müşteriler var olan bir sanal ağ içindeki Azure yığın uygulama hizmeti şimdi dağıtabilir.  Varolan bir sanal ağı dağıtma özel bağlantı noktaları üzerinden Azure uygulama hizmeti için gerekli bir dosya sunucusu ve SQL Server için bağlanmasına olanak sağlar.  Dağıtım sırasında varolan bir sanal ağı, ancak dağıtmak için müşteriler seçebilirsiniz [uygulama hizmeti tarafından kullanım için alt ağlar oluşturmanız gerekir](azure-stack-app-service-before-you-get-started.md#virtual-network) dağıtımından önce.

- Güncelleştirmeleri **uygulama hizmet Kiracı, yönetim işlevleri portalları ve Kudu Araçları**.  Azure yığın portalı SDK sürümü ile tutarlı.

- **Aşağıdaki uygulama çerçeveleri ve Araçları güncelleştirmelerini**:
    - Eklenen **.Net 2.0 çekirdek** desteği
    - Eklenen **Node.JS** sürümleri:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Eklenen **NPM** sürümleri:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Eklenen **PHP** güncelleştirmeleri:
        - 5.6.32
        - 7.0.26 (x86 hem x64)
        - 7.1.12 (x86 hem x64)
    - Güncelleştirilmiş **Windows için Git** v 2.14.1
    - Güncelleştirilmiş **Mercurial** v4.5.0 için

  - Desteği eklendi **yalnızca HTTPS** uygulama servis Kiracı Portalı'nda özel etki alanı özelliği içindeki özelliği. 

  - Azure işlevleri için özel depolama seçicisinde depolama bağlantısı eklenen doğrulama 

#### <a name="fixes"></a>Düzeltmeler

- Bir çevrimdışı dağıtım paketi oluştururken, müşteriler artık erişim reddedildi klasörü uygulama hizmeti Yükleyicisi'nden açılırken hata iletisi almaz

- Uygulama hizmeti Kiracı portalı özel etki alanları özelliğinde çalışırken sorunları çözümlendi.

- Kurulum sırasında ayrılmış yönetici adları kullanan müşteriler engelle

- Uygulama hizmeti dağıtımı ile etkin **etki alanına katılmış** dosya sunucusu

- Azure yığın kök geliştirilmiş alınmasını komut dosyasında sertifika ve şimdi uygulama hizmeti yükleyici kök cert doğrulayın.

- Azure Resource Manager için bir abonelik olduğunda döndürülen sabit durumu yanlış Microsoft.Web ad alanında kapsanan bu kaynakları sildi.

### <a name="known-issues-with-the-deployment-process"></a>Dağıtım işlemi ile ilgili bilinen sorunlar

- Azure uygulama hizmeti Azure yığın güncelleştirme 1 dağıtımı için bilinen herhangi bir sorun vardır.

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Azure uygulama hizmeti Azure yığın güncelleştirme 1 güncelleştirmesi için bilinen herhangi bir sorun vardır.

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Azure uygulama hizmeti Azure yığın güncelleştirme 1'yüklemesi için bilinen herhangi bir sorun vardır.

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bulut Azure uygulama hizmeti Azure yığında işletim yöneticileri için bilinen sorunlar

Belgelerde başvurmak [Azure yığın 1802 sürüm notları](azure-stack-update-1802.md)

## <a name="see-also"></a>Ayrıca bkz.

- Azure uygulama hizmeti genel bakış için bkz: [Azure uygulama hizmeti Azure yığın genel bakış](azure-stack-app-service-overview.md).
- Azure yığın uygulama hizmeti dağıtmak hazırlanması hakkında daha fazla bilgi için bkz: [Azure yığın uygulama hizmeti ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
