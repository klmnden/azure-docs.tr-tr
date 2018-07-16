---
title: Azure uygulamaları yönetilen uygulama teklifi yayımlama Kılavuzu
description: Bu makalede Market'te bir yönetilen uygulama yayımlama için gereksinimleri anlatılmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: 674e91a7c1de026a26cf9a3bf1eaead7f5e86144
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060554"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Azure uygulamaları: Yönetilen uygulama yayımlama Kılavuzu'nu teklifi

Çözüm şablonları Market'te çözüm yayımlama için temel yollarından biridir. Bu teklif gereksinimlerini anlamak için bu kılavuzu kullanın. 

Hangi dağıtılır ve faturalandırılır Market aracılığıyla işlem teklifler şunlardır. Bir kullanıcının gördüğü eylem çağrısı "Şimdi edinin." olan

Azure uygulama kullanma: yönetilen uygulama Teklif türü aşağıdaki koşullar gerekli olduğunda:
- Her bir abonelik tabanlı çözüm müşteriniz için bir VM veya Iaas tabanlı tüm bir çözümü kullanarak dağıtırsınız.
- Siz veya müşteriniz gerektiren çözüm iş ortağı tarafından yönetiliyor.

>[!NOTE]
>Örneğin, bir iş ortağı bir sı veya yönetilen hizmet sağlayıcısı (MSP) olabilir.  

## <a name="managed-application-offer"></a>Yönetilen uygulama teklifi

|Gereksinimler |Ayrıntılar  |
|---------|---------|
|Bir müşterinin Azure aboneliğinize dağıtılır | Yönetilen uygulamaları müşteri aboneliğinde dağıtılmış olması gereken ve 3. taraf tarafından yönetilebilir. | 
|Faturalama ve ölçüm    |  Müşterinin Azure aboneliğinde kaynakları sağlanır. Kullandıkça Öde (DÖNÜŞTÜREBİLMEMİZ) sanal makineler aracılığıyla müşterinin Azure aboneliği (DÖNÜŞTÜREBİLMEMİZ) faturalandırılır. Microsoft, müşteri ile tamamlanmasını 
Getirin-kendi lisansını söz konusu olduğunda, Microsoft Müşteri aboneliğinde altyapı maliyetleri faturalandırır sırasında müşteriye ücretleri doğrudan lisans transact        |
|Azure ile uyumlu sanal sabit disk (VHD)    |   Windows veya Linux Vm'leri oluşturulmalıdır.<ul> <li>Azure ile uyumlu bir VHD (Linux tabanlı) bölümünde bulunan oluşturma Linux VHD'si oluşturma hakkında daha fazla bilgi için ziyaret [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2- Create-an-Azure-Compatible-VHD-Linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Bir Windows VHD oluşturma hakkında daha fazla bilgi için Azure ile uyumlu bir VHD (Windows tabanlı) bölümünde bulunan Oluştur ziyaret [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3- Create-an-Azure-Compatible-VHD-Windows-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul>      |

>[!NOTE]
> Yönetilen uygulamalar Market aracılığıyla dağıtılabilir olmalıdır. Müşteri iletişimi önemliyse, müşteri paylaşımı etkinleştirdikten sonra sonra isteyen müşterilere ulaşmanızı.  


## <a name="next-steps"></a>Sonraki Adımlar
Zaten yapmadıysanız, 

- [Kayıt](https://azuremarketplace.microsoft.com/sell) Market'te

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,

- [Bulut iş ortağı portalında oturum açın](https://cloudpartner.azure.com) oluşturmak veya teklifiniz tamamlamak için
