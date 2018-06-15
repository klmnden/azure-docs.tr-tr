---
title: Azure bağlantısı kesilmiş dağıtım kararları Azure yığınının tümleşik sistemleri | Microsoft Docs
description: Dağıtım Planlama Azure yığın Azure bağlı çok düğümlü dağıtımlar için kararları belirleyin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 49697a57e59b652fed4997d57bc7ae15cc596cf7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32151136"
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Planlama kararları Azure yığınının azure bağlantısı kesilmiş dağıtım sistemleri tümleşik
Karar verdim sonra [nasıl karma bulut ortamınıza Azure yığın tümleştirecek](azure-stack-connection-models.md), ardından Azure yığın dağıtım kararlarınızı sonlandır.

Bağlantısı kesilmiş olan Azure dağıtım seçeneği, dağıtmak ve Internet bağlantısı olmadan Azure yığın kullanın. Ancak, bağlantısı kesilmiş bir dağıtım ile bir AD FS kimlik deposu ve kapasite tabanlı faturalama modeli sınırlıdır. 

Gerekirse bu seçeneği belirleyin:
- Güvenlik veya Azure yığın Internet'e bağlı olmayan bir ortamda dağıtmak ihtiyaç duyduğunuz başka kısıtlamalar vardır.
- (Kullanım verilerini dahil) veri bloğu Azure'a gönderilen istiyorsunuz.
- Tamamen şirket intranetinize dağıtılan ve karma senaryoları ilginizi olmayan bir özel bulut çözümü olarak Azure yığın kullanmak istiyorsunuz.

> [!TIP]
> Bazı durumlarda, bu tür bir ortamda aynı zamanda "Denizaltı senaryo" olarak adlandırılır.

Bağlantısı kesilmiş bir dağıtım kesinlikle, daha sonra Azure yığın Örneğiniz için Azure karma Kiracı VM senaryoları için bağlanamadığınızı anlamına gelmez. Dağıtım sırasında Azure bağlantısı yoksa veya Azure Active Directory kimlik deposu olarak kullanmak istemediğiniz anlamına gelir.

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Engelli veya kullanılamaz durumda bağlantısı kesilmiş dağıtımları özellikleri 
Azure yığın bazı özellikleri ve işlevleri engelli ya da bağlantısı kesilmiş modunda tamamen kullanılamaz olduğunu dikkate almak önemlidir Azure'a bağlıyken en iyi çalışacak şekilde tasarlanmıştır. 

|Özellik|Bağlantı kesik modda etkileri|
|-----|-----|
|VM dağıtımı DSC uzantısı VM post dağıtımı yapılandırmak için|Engelli - DSC uzantısı için en son WMF Internet'e arar.|
|Docker komutları çalıştırmak için Docker uzantılı VM dağıtımı|Engelli – Docker Internet en son sürümü için kontrol eder ve bu denetimi başarısız olur.|
|Azure yığın portalındaki belgelere bağlantılar|Kullanılamıyor – görüş, Yardım, hızlı başlangıç, vb. bir Internet URL'si çalışmaz kullanan gibi bağlantılar.|
|Uyarı düzeltme/çevrimiçi düzeltme kılavuzda başvuran azaltma|Kullanılamaz – bir Internet URL'si çalışmaz kullanan herhangi bir uyarı düzeltme bağlar.|
|Market dağıtım – seçin ve doğrudan Azure Marketi'nden galeri paketleri ekleme olanağı|Engelli – bağlantısız modda (olmadan, herhangi bir internet bağlantısı) Azure yığın dağıttığınızda, Azure yığın portalını kullanarak Market öğesi yükleyemiyor. Ancak, kullanabileceğiniz [Market dağıtım aracı](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) Market öğeleri Internet bağlantısına sahip bir makineye yüklemek ve ardından bunları Azure yığın ortamınıza aktarmak için.|
|Bir Azure yığın dağıtımını yönetmek için Azure Active Directory Federasyon hesaplarını kullanma|Kullanılamıyor – bu özellik Azure bağlantısı gerektirir. AD FS yerel Active Directory örneği ile bunun yerine kullanılmalıdır.|
|Uygulama Hizmetleri|Engelli - WebApps güncelleştirilmiş içerik için Internet erişimi gerekebilir.|
|Komut Satırı Arabirimi (CLI)|Engelli – CLI kimlik doğrulaması ve hizmet ilkeleri sağlama açısından işlevselliği düşürdü.|
|Visual Studio – bulut bulma|Engelli – Cloud Discovery ya da farklı Bulutlar keşfeder veya hiç çalışmaz.|
|Visual Studio – AD FS|Visual Studio Enterprise AD FS'nin desteklediği yalnızca – engelli.
Telemetri|Kullanılamıyor – Azure yığınının Telemetri verilerini ilişkin telemetri verilerini bağımlı iyi gibi tüm üçüncü taraf galeri paketler.|
|Sertifikalar|Sertifika iptal listesi (CRL) ve çevrimiçi sertifika durumu Protokolü (OSCP) hizmeti HTTPS bağlamında kullanılamıyor – Internet bağlantısı gereklidir.|
|Anahtar kasası|Engelli – genel bir kullanım örneği anahtar kasası için çalışma zamanında gizli anahtarları okumak bir uygulama olmalıdır. Bu uygulamanın bir hizmet sorumlusu dizininde gerekir. Azure Active Directory'de normal kullanıcıların (Yönetici olmayanlar) hizmet asıl adı eklemek için izin verilen varsayılan olarak gelir. (ADFS kullanarak) AD içinde değiller. Bir her zaman herhangi bir uygulama eklemek için dizin Yöneticisi gitmeniz gerekir çünkü bu bir hurdle uçtan uca deneyimi yerleştirir.| 

## <a name="learn-more"></a>Daha fazla bilgi edinin
- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için bkz: [Azure yığın](https://azure.microsoft.com/overview/azure-stack/) ürün sayfası.
- Tümleşik sistemleri Azure yığını için yol haritası ve coğrafi kullanılabilirlik hakkında bilgi için teknik incelemesine bakın: [Azure yığın: Azure uzantısı](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Microsoft Azure yığın paketleme ve fiyatlandırma hakkında daha fazla bilgi edinmek için [.pdf karşıdan](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Sonraki adımlar
[Veri Merkezi ağ tümleştirme](azure-stack-network.md)