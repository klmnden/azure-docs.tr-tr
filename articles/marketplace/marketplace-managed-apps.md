---
title: Azure uygulamaları yönetilen uygulama teklifi yayımlama Kılavuzu
description: Bu makalede Market'te bir yönetilen uygulama yayımlama için gereksinimleri anlatılmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: qianw211
manager: evansma
ms.service: marketplace
ms.topic: article
ms.date: 06/14/2018
ms.author: v-qiwe
ms.openlocfilehash: 29546b0969751a43959a55860fc22e9f3c3e225b
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154948"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Azure uygulamaları için: Yönetilen uygulama teklifi yayımlama Kılavuzu

Yönetilen bir uygulamaya bir çözüm Market'te yayımlamak için temel yollarından biridir. Bu teklif gereksinimlerini anlamak için bu kılavuzu kullanın. 

Hangi dağıtılır ve faturalandırılır Market aracılığıyla işlem teklifler şunlardır. Bir kullanıcının gördüğü eylem çağrısı "Şimdi edinin." olan

Azure uygulama kullanma: yönetilen uygulama Teklif türü aşağıdaki koşullar gerekli olduğunda:
- Her bir abonelik tabanlı çözüm müşteriniz için bir VM veya Iaas tabanlı tüm bir çözümü kullanarak dağıtırsınız.
- Siz veya müşteriniz gerektiren çözüm iş ortağı tarafından yönetiliyor.

>[!NOTE]
>Örneğin, bir iş ortağı bir sı veya yönetilen hizmet sağlayıcısı (MSP) olabilir.  

## <a name="managed-application-offer"></a>Yönetilen uygulama teklifi

|Gereksinimler |Ayrıntılar  |
|---------|---------|
|Bir müşterinin Azure aboneliğinize dağıtılır | Yönetilen uygulamaları müşteri aboneliğinde dağıtılmış olması gereken ve bir üçüncü taraf tarafından yönetilebilir. | 
|Faturalama ve ölçüm    |  Müşterinin Azure aboneliğinde kaynakları sağlanır. Microsoft, Müşteri'nin Azure aboneliği (DÖNÜŞTÜREBİLMEMİZ) faturalandırılır aracılığıyla müşteri ile Kullandıkça Öde (DÖNÜŞTÜREBİLMEMİZ) sanal makineler işlem temelli. <br> Microsoft, müşteri aboneliğinde altyapı maliyetleri faturalandırır sırada getirin-kendi lisansını söz konusu olduğunda, müşteri masrafları doğrudan lisans transact.        |
|Azure ile uyumlu sanal sabit disk (VHD)    |   Windows veya Linux Vm'leri oluşturulmalıdır.<ul> <ul> <li>Linux VHD'si oluşturma hakkında daha fazla bilgi için bkz. [Azure'da desteklenen Linux dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Bir Windows VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure ile uyumlu bir VHD oluşturma](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |

>[!NOTE]
> Yönetilen uygulamalar Market aracılığıyla dağıtılabilir olmalıdır. Müşteri iletişimi önemliyse, müşteri paylaşımı etkinleştirdikten sonra sonra isteyen müşterilere ulaşmanızı.  

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](./cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı.

## <a name="next-steps"></a>Sonraki adımlar
Zaten yapmadıysanız, 

- [Kayıt](https://azuremarketplace.microsoft.com/sell) Market'te.

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,

- [Bulut iş ortağı portalında oturum açın](https://cloudpartner.azure.com) oluşturmak veya teklifiniz tamamlayın.
