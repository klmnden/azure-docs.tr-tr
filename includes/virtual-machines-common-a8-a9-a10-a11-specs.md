---
title: "include dosyası"
description: "include dosyası"
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: ee32886ddb74bdbbe0f240310629c8ef26230a68
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
## <a name="deployment-considerations"></a>Dağıtma konuları
* **Azure aboneliği** – daha çok sayıda işlem yoğunluklu örnekler dağıtmak için Kullandıkça Öde aboneliğine veya diğer satın alma seçenekleri göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* **Fiyatlandırma ve kullanılabilirlik** -bu VM boyutları, yalnızca standart fiyatlandırma katmanı sunulur. [Ürünler kullanılabilir bölgeye göre] denetleyin (https://azure.microsoft.com/regions/services/) Azure bölgelerindeki kullanılabilirlik. 
* **Çekirdek kota** – Azure aboneliğinizde varsayılan değerden çekirdek Kotayı artırmak gerekebilir. Aboneliğinizi H-serisi dahil olmak üzere belirli VM boyutu aileleri, dağıtabilirsiniz çekirdek sayısı da sınırlayabilir. Bir kota artışı isteği göndermek üzere [bir çevrimiçi müşteri destek isteği açma](../articles/azure-supportability/how-to-create-azure-support-request.md) herhangi bir ücret alınmaz. (Varsayılan sınırları abonelik kategoriye göre farklılık gösterebilir.)
  
  > [!NOTE]
  > Büyük ölçekli kapasite gereksinimlerini varsa Azure desteğine başvurun. Azure kotaları alacak olan sınırlar, değil kapasite garantiler. Kotanızı bağımsız olarak, kullandığınız yalnızca çekirdek için ücretlendirilirsiniz.
  > 
  > 
* **Sanal ağ** – Azure bir [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) işlem yoğunluklu örnekler kullanmak için gerekli değildir. Şirket içi kaynaklara erişmesi gerekirse, ancak birçok dağıtımları için en az bir bulut tabanlı Azure sanal ağı veya siteden siteye bağlantı gerekir. Gerekli olduğunda, örnekleri dağıtmak için yeni bir sanal ağ oluşturun. Bir benzeşim grubundaki sanal bir ağa işlem yoğunluklu VM'ler ekleme desteklenmiyor.
* **Yeniden boyutlandırma** – kendi özelleştirilmiş donanım nedeniyle, yalnızca işlem yoğunluklu örnekler boyutu ailesini (S-serisi veya işlem yoğunluklu A-series) içinde yeniden boyutlandırabilirsiniz. Örneğin, yalnızca H-serisi bir boyut H-serisi bir VM'den diğerine boyutlandırabilirsiniz. Buna ek olarak, yoğun olmayan sabit boyutlu bir işlem yoğunluklu boyutuna yeniden boyutlandırma desteklenmiyor.  

## <a name="rdma-capable-instances"></a>RDMA özellikli örnekleri
Bir alt işlem yoğunluklu örnekler (H16r, H16mr, A8 ve A9), doğrudan uzak bellek erişimi (RDMA) bağlantısı için bir ağ arabirimi özelliği. (NC24r gibi 'r' belirlenmiş seçilen N-serisi boyutları ayrıca RDMA özelliğine sahip değildir.) Diğer VM boyutları için kullanılabilir standart Azure ağ arabirimi yanı sıra bu arabirimidir. 
  
Bu arabirim H16r, H16mr ve RDMA özellikli N-serisi sanal makineler için FDR hızlarını ve A8 ve A9 sanal makineler için QDR hızlarını işletim bir InfiniBand (IB) ağ üzerinden iletişim kurmak RDMA özellikli örneklerini verir. RDMA yeteneklere ölçeklenebilirlik ve belirli ileti geçirme arabirimi (MPI) uygulamaların performansını artırabilir.

> [!NOTE]
> Azure'da, IP IB üzerinde desteklenmiyor. Yalnızca IB üzerinden RDMA desteklenir.
>

RDMA özelliğine sahip HPC sanal makineleri aynı kullanılabilirlik kümesinde veya (Azure Resource Manager dağıtım modeli kullandığınızda) VM ölçek kümesi ya da aynı bulut hizmetine (Klasik dağıtım modeli kullandığınızda) dağıtın. Azure RDMA ağ erişmek RDMA özellikli HPC VM'ler için ek gereksinimler izleyin.