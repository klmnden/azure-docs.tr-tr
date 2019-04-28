---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/29/2018
ms.author: azcspmt;jonbeck;cynthn;danlep
ms.custom: include file
ms.openlocfilehash: c12fff63cdb7241d89e7511a3dac2ff9c1363ae6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60540492"
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
Bir alt işlem yoğunluklu örneklerinden (H16r, H16mr, A8 ve A9) bir ağ arabirimi için doğrudan uzak bellek erişimi (RDMA) bağlantı özelliği. (NC24r gibi 'r' ile belirtilen seçili N serisi de RDMA özellikli boyutlarda.) Diğer VM boyutları için kullanılabilir standart Azure ağ arabiriminin yanı sıra bu arabirimidir. 
  
Bu arabirim H16r, H16mr ve RDMA özellikli N serisi sanal makineler için FDR oranları ve QDR oranları A8 ve A9 sanal makineler için çalışan bir InfiniBand (IB) ağ üzerinden iletişim kurmak RDMA özellikli örnekler sağlar. Bu RDMA özelliklerini belirli ileti geçirme arabirimi (MPI) uygulamaları performans ve ölçeklenebilirliği artırabilir.

> [!NOTE]
> Azure'da IP IB üzerinde desteklenmiyor. Yalnızca IB üzerinden RDMA desteklenir.
>

