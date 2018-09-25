---
title: Yönetme ve Azure Machine Learning hizmeti için kotalar isteği
description: Bu nasıl yapılır kılavuzunda, Azure Machine Learning ve görüntüleme ve daha fazla kota isteği için kaynaklar üzerinde çeşitli kotalar açıklar.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 9/24/2018
ms.openlocfilehash: 0ee0a29be6a313e09cd77051dc29bc0131b57a40
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997820"
---
# <a name="manage-and-request-quotas-for-azure-resources"></a>Yönetme ve Azure kaynakları için kota isteği

Olarak diğer Azure hizmetleriyle sınırı yoktur Azure Machine Learning hizmetiyle ilişkili belirli kaynaklar. Bu sınırları alır gerçek temel alınan işlem sınırları için oluşturabileceğiniz bir çalışma alanı sayısı için üst sınır arasındadır modellerinizde eğitim veya çıkarım için kullanılır. Bu makalede, aboneliğiniz için çeşitli Azure kaynakları üzerinde önceden yapılandırılmış sınırları hakkında daha fazla ayrıntı sağlar ve ayrıca her kaynak türü için istek kota geliştirmeleri için kullanışlı bağlantılar içerir.

Bu kotalar, tasarım ve Azure ML kaynaklarınızı ölçeklendirme aklınızda bulundurun. Kümenizi, belirtilen düğümlerin hedef sayısını ulaşmaz, örneğin, daha sonra bir Batch AI çekirdek, aboneliğiniz için sınırına ulaştınız. Batch AI üretim iş yükleri çalıştırmayı planlıyorsanız, bir veya daha üstüne varsayılan kotaları artırmak gerekebilir. Sınırı veya kota varsayılan sınırı artırmak istiyorsanız, ücretsiz bir çevrimiçi müşteri destek isteği açın. Aşağıdaki tablolarda gösterilen üst sınırı değerini yukarıda sınırları yükseltilemez. Üst sınır sütun yok ise, kaynak sınırları ayarlanabilir sahip değil. 

## <a name="special-considerations"></a>Özel durumlar

+ Kota kapasitesini garanti bir kredi sınırına ' dir. Büyük ölçekli kapasite gereksinimleriniz varsa, Azure desteğine başvurun.

+ Kota tüm hizmetlerde, Batch AI dışında bunlardan biri olan Azure Machine Learning, aboneliğinizdeki paylaşılır. Kota kullanımını tüm hizmetler arasında kapasite gereksinimlerinizi değerlendirirken hesaplamak emin olun.

+ Varsayılan sınırları, F, G, teklif, ücretsiz deneme, Kullandıkça Öde ve gibi Dv2 serisi gibi kategori türüne göre değişir ve benzeri.

## <a name="default-resource-quotas"></a>Varsayılan kaynak kotaları

Azure aboneliğiniz içindeki çeşitli kaynak türleri göre kota sınırları bir dökümü aşağıda verilmiştir. 

> [!Important]
> Limitlerdir değiştirilebilir. Her zaman en son hizmet düzeyi kotanın bulunabilir [belge](https://docs.microsoft.com/azure/azure-subscription-service-limits/) tüm Azure için.

### <a name="virtual-machines"></a>Sanal makineler 
Bir Azure aboneliğinde, hizmetler arasında veya tek başına bir şekilde sağlayabilirsiniz sanal makinelerin sayısına bir sınır yoktur. Toplam çekirdek üzerinde hem de başına ailesi temelinde bölge düzeyinde bu sınırdır.

Bölgesel bir toplam limiti ve ayrı ayrı uygulanır; boyutu seri (Dv2, F vb.) başına bölgesel bir sanal makine çekirdeğe sahip olduğunu vurgulamak önemlidir. Örneğin, Doğu ABD toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu aboneliğin 30 adet A1 sanal makinesi veya 30 adet D1 sanal makinesi ya da ikisinin toplamda 30 çekirdeği geçmeyecek bir birleşimini (örneğin, 10 adet A1 sanal makinesi ve 20 adet D1 sanal makinesi) dağıtmasına izin verilir.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

Kota sınırları daha ayrıntılı ve güncel listesi için Azure genelinde kota makale onay [burada](https://docs.microsoft.com/azure/azure-subscription-service-limits#subscription-limits-1).

### <a name="batch-ai-clusters"></a>Batch AI kümeleri
Batch AI çekirdek sayısı ve bir abonelikte bölge başına izin verilen küme sayısı varsayılan bir kota sınırı yoktur. Batch AI kota VM çekirdek kotası yukarıdaki ayrıdır ve çekirdek sınırları arasında iki kaynak türleri şu anda paylaşılmaz.

Kullanılabilir kaynaklar:
+ Bölge başına adanmış çekirdekler 10-24'ın varsayılan bir sınırı vardır.  Batch AI abonelik başına adanmış çekirdekler sayısı artırılabilir. Artırma seçeneklerini görüşmek için Azure desteğine başvurun.

+ Bölge başına düşük öncelikli çekirdekler 10-24'ın varsayılan bir sınırı vardır.  Batch AI abonelik başına düşük öncelikli çekirdek sayısı artırılabilir. Artırma seçeneklerini görüşmek için Azure desteğine başvurun.

+ Bölge başına kümeler, varsayılan bir sınırı 20 ve 200 üst sınırına sahip. Bu sınırı aşan bir artırım talebinde bulunmak isterseniz Azure desteğine başvurun.

Kota sınırları daha ayrıntılı ve güncel listesi için Azure genelinde kota makale onay [burada](https://docs.microsoft.com/azure/azure-subscription-service-limits#batch-ai-limits).

### <a name="container-instances"></a>Kapsayıcı örnekleri

(Saatlik kapsamlı) belirli bir süre içinde veya genelinde tüm aboneliğinizi ayarlama çalıştırabileceğinizi container Instances, sayısına bir sınır yoktur.

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

Kota sınırları daha ayrıntılı ve güncel listesi için Azure genelinde kota makale onay [burada](https://docs.microsoft.com/azure/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Depolama
Depolama hesabı da belirli bir abonelikte bölge başına sayısına bir sınır yoktur. Varsayılan sınır 200'dür ve standart ve Premium depolama hesapları içerir. Belirli bir bölgede 200'den fazla depolama hesabı gerekiyorsa, bir istekte aracılığıyla [Azure Destek](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). Azure depolama ekibi, işinizin durumunu inceler ve 250 depolama hesapları için belirli bir bölgeye kadar onaylayabilir.


## <a name="find-your-quotas"></a>Kotanızı Bul

Sanal makineler, depolama, ağ, gibi çeşitli kaynaklar için kota görüntüleme Azure portalı üzerinden kolay bir işlemdir.

1. Sol bölmeden **tüm hizmetleri** seçip **abonelikleri** genel kategori altında.

1. Abonelikler listesinden kota aradığınız aboneliği seçin.

   **Bir uyarı yok**, Batch AI kota görüntülemek için özel olarak. Yukarıda belirtildiği gibi kota aboneliğinizde işlem kotası ayrıdır. 
   Batch AI seçtikten sonra için **tüm hizmetleri**ve aramak için Batch AI hizmeti açın.

1. Altında **ayarları**seçin **kullanım ve kotalar** geçerli kotası ve kullanımı görüntülemek için.

1. Kota sınırları görüntülemek için aboneliğinizi seçin. Hizmet ve bölge ilgilendiğiniz filtrelemek unutmayın.


## <a name="request-quota-increases"></a>Kota artırımlarına iste

Yukarıda belirtilen varsayılan sınırı, kotası ve sınırı artırmak istiyorsanız [bir çevrimiçi müşteri destek isteği açın](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) ücret olmadan. 

Sınırları, yukarıdaki tabloda gösterilen üst sınırı değeri yükseltilemez. Maksimum sınır varsa, kaynak sınırları ayarlanabilir sahip değil. [Bu](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) makalede kota artışı işlem daha ayrıntılı ele alınmıştır.

Kota artışı isteğinde bulunurken, AzureML kota, Batch AI kota ya da depolama kotası gibi hizmetleri olabilir, kotaya Yükselt isteyen hizmet seçmeniz gerekir. 

> [!NOTE]
> [Ücretsiz deneme abonelikleri](https://azure.microsoft.com/offers/ms-azr-0044p) sınırı veya kota artırma işlemleri için uygun değildir. Varsa bir [ücretsiz deneme aboneliği](https://azure.microsoft.com/offers/ms-azr-0044p), Yükseltme yapabileceğiniz bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik. Daha fazla bilgi için [yükseltme Azure ücretsiz denemesini Kullandıkça Öde aboneliğine](../../billing/billing-upgrade-azure-subscription.md) ve [ücretsiz deneme aboneliği SSS](https://azure.microsoft.com/free/free-account-faq).
