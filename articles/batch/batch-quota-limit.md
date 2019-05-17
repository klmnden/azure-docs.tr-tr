---
title: Hizmet kotaları ve limitleri - Azure Batch | Microsoft Docs
description: Varsayılan Azure Batch kotaları, sınırları ve kısıtlamaları hakkında bilgi edinin ve kota isteğinde nasıl artırır
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 820eddff7da3bb52ca94ea0cb7e2361d89892a4a
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595313"
---
# <a name="batch-service-quotas-and-limits"></a>Batch hizmet kotaları ve limitleri

Diğer Azure hizmetleriyle olduğundan sınırları Batch hizmetiyle ilişkili belirli kaynaklar. Bu limitlerin çoğu, abonelik veya hesap düzeyinde Azure tarafından uygulanan varsayılan kotalardır. Bu makalede bu Varsayılanları açıklar ve kota isteğinde bulunabilirsiniz nasıl artırır.

Bu kotalar, tasarım ve Batch iş yüklerinizi ölçeği aklınızda bulundurun. Havuzunuzu, işlem düğümleri, belirtilen hedef sayısı ulaşmaz, örneğin, çekirdek kotası Batch hesabınız için sınırına.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelik ve farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

Batch'de üretim iş yükleri çalıştırmayı planlıyorsanız, bir veya daha üstüne varsayılan kotaları artırmak gerekebilir. Kotayı artırmak istiyorsanız, çevrimiçi açabilirsiniz [müşteri destek isteği](#increase-a-quota) ücret olmadan.

> [!NOTE]
> Kota kapasitesini garanti bir kredi sınırına ' dir. Büyük ölçekli kapasite gereksinimleriniz varsa, lütfen Azure desteğine başvurun.

## <a name="resource-quotas"></a>Kaynak kotaları
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]


### <a name="cores-quotas-in-user-subscription-mode"></a>Kullanıcı aboneliği modunda çekirdek kotaları

Havuz ayırma modu ayarlamak bir Batch hesabı oluşturduysanız **kullanıcı aboneliği**, kotalar farklı şekilde uygulanır. Bu modda, bir havuz oluşturulduğunda Batch Vm'leri ve diğer kaynaklar doğrudan aboneliğinizde oluşturulur. Azure Batch çekirdek kotaları bu modda oluşturulan bir hesap için geçerli değildir. Bunun yerine, kotalar bölge için aboneliğinizdeki çekirdek işlem ve diğer kaynaklara uygulanır. Bu kotaları hakkında daha fazla bilgi [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>Havuz boyutu sınırları

| **Kaynak** | **Üst Sınır** |
| --- | --- |
| **İşlem düğümleri, [etkin düğümler arası iletişimin havuzu](batch-mpi.md)**  ||
| Batch hizmeti Havuz ayırma modu | 100 |
| Batch aboneliği Havuz ayırma modu | 80 |
| **İşlem düğümleri, [havuzu ile özel VM görüntüsü oluşturulan](batch-custom-images.md)**<sup>1</sup> ||
| Adanmış düğümler | 2000 |
| Düşük öncelikli düğümler | 1000 |

<sup>1</sup> düğümler arası iletişimin etkin olmayan havuzlar için.

## <a name="other-limits"></a>Diğer sınırlamaları

| **Kaynak** | **Üst Sınır** |
| --- | --- |
| [Eş zamanlı görevleri](batch-parallel-node-tasks.md) işlem düğüm başına | düğümüne çekirdek 4 x sayısı |
| [Uygulamaları](batch-application-packages.md) Batch hesabı başına | 20 |
| Uygulama başına uygulama paketleri | 40 |
| Havuz başına uygulama paketleri | 10 |
| En fazla görev ömrü | 180 gün<sup>1</sup> |

<sup>1</sup> tamamlandığında, gelen ne zaman işe eklenir, bir görevin maksimum ömrü 180 gündür. Tamamlanan görevler yedi gün boyunca kalıcı; en fazla bir yaşam süresi içinde tamamlanmamış görevlerin verileri erişilemez.

## <a name="view-batch-quotas"></a>Batch kotaları görüntüle

Batch hesabı kotaları görüntüleme [Azure portalında][portal].

1. Seçin **Batch hesapları** portalda sonra ilgilendiğiniz Batch hesabı seçin.
1. Seçin **kotalar** Batch hesabının menüsünde.
1. Batch hesabına uygulanmakta kotaları görüntüle

    ![Batch hesabı kotaları][account_quotas]

## <a name="increase-a-quota"></a>Kota artırma

Kota isteği için aşağıdaki adımları kullanarak veya Batch hesabınız için artırmak izleyin [Azure portalında][portal]. Kota artışı türü, Batch hesabınızın Havuz ayırma moduna bağlıdır. Bir kota artırım talebinde bulunmak için kotayı artırmak için istediğiniz VM serisi eklemeniz gerekir. Kota artışı uygulandığında, bu Vm'leri tüm dizi uygulanır.

### <a name="increase-cores-quota-in-batch"></a>Toplu iş aboneliğindeki çekirdek kotasını artırma 

1. Seçin **Yardım + Destek** kutucuğunu portal panonuza veya soru işareti (**?**) portalın sağ üst köşenin içinde.
1. Seçin **yeni destek isteği** > **Temelleri**.
1. İçinde **Temelleri**:
   
    a. **Sorun türü** > **hizmet ve abonelik sınırlarını (kotalar)**
   
    b. Aboneliğinizi seçin.
   
    c. **Kota türü** > **Batch**
      
    **İleri**’yi seçin.
    
1. İçinde **ayrıntıları**:
      
    a. İçinde **ayrıntıları sağlayın**, konum, kota türünü belirtin ve Batch hesabı.
    
    ![Batch kota artışı][quota_increase]

    Kota türleri şunlardır:

    * **Batch hesabı**  
        Değerleri belirli tek bir Batch hesabı, ayrılmış ve düşük öncelikli çekirdek ve işler ve havuzları sayısı dahil olmak üzere.
        
    * **Bölge başına**  
        Tüm Batch hesaplarının bir bölge içinde geçerlidir ve her Abonelikteki bölge başına Batch hesabı sayısı içeren değerleri.

    Düşük öncelikli kota tüm VM serisi arasında tek bir değer var. Kısıtlanmış SKU'ları gerekirse seçmelisiniz **düşük öncelikli çekirdekler** ve istemek için VM aileleri içerir.

    b. Seçin bir **önem derecesi** göre [iş etkisi][support_sev].

    **İleri**’yi seçin.

1. İçinde **iletişim bilgilerini**:
   
    a. Seçin bir **tercih edilen iletişim yöntemi**.
   
    b. Gerekli kişi ayrıntılarını girin ve doğrulayın.
   
    Seçin **Oluştur** için destek isteği gönderin.

Azure desteği, destek isteğinizi gönderdikten sonra sizinle iletişime geçecektir. Kota isteği, iki iş günü kadar ya da birkaç dakika içinde tamamlanabilir.

## <a name="related-quotas-for-vm-pools"></a>Sanal makine havuzları için ilgili kotalar

Bir Azure sanal ağında otomatik olarak dağıtılan sanal makine yapılandırmasında batch havuzları, ek Azure kaynakları ayırın. Aşağıdaki kaynaklar, bir sanal ağdaki her 50 havuz düğümleri için gereklidir:

* Bir [ağ güvenlik grubu](../virtual-network/security-overview.md#network-security-groups)
* Bir [genel IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* Bir [yük dengeleyici](../load-balancer/load-balancer-overview.md)

Bu kaynaklar, Batch havuzu oluştururken, sağlanan sanal ağ'ı içeren aboneliği ayrılır. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../azure-subscription-service-limits.md) sınırlıdır. Bir sanal ağdaki dağıtımlar büyük havuz planlıyorsanız, bu kaynaklar için kotalar abonelik denetleyin. Gerekirse, Azure portalında bir artış seçerek istek **Yardım + Destek**.


## <a name="related-topics"></a>İlgili konular
* [Azure portalını kullanarak bir Azure Batch hesabı oluşturma](batch-account-create-portal.md)
* [Azure Batch özelliklerine genel bakış](batch-api-basics.md)
* [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: https://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png
