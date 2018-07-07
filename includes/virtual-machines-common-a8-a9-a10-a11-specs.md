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
ms.openlocfilehash: 296e92d803bb69376f286aa60cfb4a955b08010f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "34669363"
---
## <a name="deployment-considerations"></a>Dağıtma konuları
* **Azure aboneliği** – fazla sayıda bilgi işlem yoğun örnekler dağıtın, Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerini göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* **Fiyatlandırma ve kullanılabilirlik** -bu VM boyutları, yalnızca standart fiyatlandırma katmanında sunulur. [Bölgelere göre kullanılabilir Ürünler] denetleyin (https://azure.microsoft.com/regions/services/) Azure bölgesi içinde kullanılabilirlik için. 
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

RDMA özellikli HPC VM'lerin aynı kullanılabilirlik kümesine veya VM ölçek kümesi (Azure Resource Manager dağıtım modeli kullandığınız zaman) ya da aynı bulut hizmeti (Klasik dağıtım modeli kullandığınız zaman) dağıtın. Bir VM ölçek kümesi kullanıyorsanız, tek bir yerleştirme grubu dağıtımı sınırladığınızdan emin olun; Örneğin, bir Resource Manager şablonunda ayarlamak *singlePlacementGroup* özelliğini *true*. Azure RDMA ağ erişmek RDMA özellikli HPC VM'ler için ek gereksinimler izleyin.