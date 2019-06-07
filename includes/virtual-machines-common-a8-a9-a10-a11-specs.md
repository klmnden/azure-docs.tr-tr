---
title: include dosyası
description: include dosyası
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 05/29/2018
ms.author: azcspmt;jonbeck;cynthn;danlep;amverma
ms.custom: include file
ms.openlocfilehash: 5cbc19d5aade2bbcc8b8dca277352d1b17d1d35a
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755194"
---
## <a name="deployment-considerations"></a>Dağıtma konuları
* **Azure aboneliği** – fazla sayıda bilgi işlem yoğun örnekler dağıtın, Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerini göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* **Fiyatlandırma ve kullanılabilirlik** -bu VM boyutları, yalnızca standart fiyatlandırma katmanında sunulur. Denetleme [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) Azure bölgesi içinde kullanılabilirlik için. 
* **Çekirdek kota** – varsayılan değerinden Azure aboneliğinizdeki çekirdek kotasını artırmanız gerekebilir. Aboneliğiniz ayrıca H serisi dahil olmak üzere belirli sanal makine boyutu aileleri içinde dağıtabileceğiniz çekirdek sayısını sınırlayabilir. Bir kota artırım talebinde bulunmak [bir çevrimiçi müşteri destek isteği açın](../articles/azure-supportability/how-to-create-azure-support-request.md) ücret olmadan. (Varsayılan sınır, abonelik kategorisine bağlı olarak farklılık gösterebilir.)
  
  > [!NOTE]
  > Büyük ölçekli kapasite gereksinimleriniz varsa, Azure desteğine başvurun. Azure kotaları alacak olan limitleri, kapasite garantisi değil. Kotanızı bağımsız olarak, kullandığınız yalnızca çekirdekler için ücretlendirilirsiniz.
  > 
  > 
* **Sanal ağ** Azure bir [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) yoğun işlem gücü kullanımlı örnekler kullanmak için gerekli değildir. Şirket içi kaynaklara erişmeye ihtiyacınız varsa ancak pek çok dağıtımı için en az bir bulut tabanlı Azure sanal ağı veya bir siteden siteye bağlantı gerekir. Gerektiğinde, örneklerini dağıtmak için yeni bir sanal ağ oluşturun. Yoğun işlem gücü kullanımlı VM'ler bir benzeşim grubundaki sanal ağa ekleme desteklenmiyor.
* **Yeniden boyutlandırma** – kendi özelleştirilmiş donanım nedeniyle yalnızca aynı boyut ailesi (H serisi veya yoğun işlem gücü kullanımlı A serisi) işlem yoğunluklu örneklerinden boyutlandırabilirsiniz. Örneğin, yalnızca H serisi boyutlar H serisi bir VM'den diğer boyutlandırabilirsiniz. Ayrıca, bir bilgi işlem açısından yoğun olmayan boyutundan yoğun işlem gücü kullanımlı bir boyuta yeniden boyutlandırma desteklenmiyor.  

## <a name="rdma-capable-instances"></a>RDMA özellikli örnekler
Bir alt işlem yoğunluklu örneklerinden (A8, A9, H16r, H16mr, HB ve HC) bir ağ arabirimi için doğrudan uzak bellek erişimi (RDMA) bağlantı özelliği. Seçili N serisi boyutları NC24rs yapılandırmaları (NC24rs_v2 ve NC24rs_v3) gibi 'r' ile belirtilen RDMA özelliğine sahiptir. Diğer VM boyutları için kullanılabilir standart Azure ağ arabiriminin yanı sıra bu arabirimidir. 
  
Bu arabirim EDR oranlarda HB, HC, FDR H16r, H16mr ve RDMA özellikli N serisi sanal makineler için oranları ve QDR oranları A8 ve A9 sanal makineler için işletim bir InfiniBand (IB) ağ üzerinden iletişim kurmak RDMA özellikli örnekler sağlar. Bu RDMA özelliklerini belirli ileti geçirme arabirimi (MPI) uygulamaları performans ve ölçeklenebilirliği artırabilir. Hızı hakkında daha fazla bilgi için bu sayfada tablolarda ayrıntıları bakın.

> [!NOTE]
> Azure'da IP IB üzerinde yalnızca desteklenir SR-IOV özellikli VM'ler (şu anda HB ve HC). IB üzerinden RDMA, RDMA özellikli tüm örnekleri için desteklenir.
>

