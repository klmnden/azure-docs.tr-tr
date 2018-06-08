---
title: Bir giriş Azure yığın Geliştirme Seti (ASDK) | Microsoft Docs
description: ASDK nedir açıklar ve Microsoft Azure yığın değerlendirmek için ortak kullanım durumları.
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
ms.topic: overview
ms.custom: mvc
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 951cd1adc09373b9af560097b088fd740ceb51a8
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850636"
---
# <a name="what-is-the-azure-stack-development-kit"></a>Azure yığın Geliştirme Seti nedir?
[Microsoft Azure yığın tümleşik sistemleri](.\.\azure-stack-poc.md) aralık boyutu 4-12 düğümlerinden ve ortaklaşa bir donanım iş ortakları ve Microsoft tarafından desteklenir. Azure tümleşik yığını sistemleri, üretim iş yükleri için yeni senaryoları etkinleştirmek için kullanın. Tümleşik sistemleri altyapısını yöneten ve hizmetleri sunan bir Azure yığın işleç değilseniz, bkz bizim [işleci belgelerine](https://docs.microsoft.com/azure/azure-stack).

Azure yığın Geliştirme Seti (ASDK) tek bir düğüm indirin ve kullanan Azure yığınının dağıtımıdır **ücretsiz**. Sanal bir tek ana bilgisayar sunucu bilgisayarda çalışan karşılaması veya aşması gerekir makinelerinizde tüm ASDK bileşenlerinin yüklü olduğu [en düşük donanım gereksinimleri](asdk-deploy-considerations.md#hardware). ASDK Azure yığın değerlendirin ve API'lerini kullanarak ve Azure ile tutarlı tooling modern uygulamalar geliştirme bir ortam sağlamak için tasarlanmıştır bir *üretim dışı* ortamı. 

> [!IMPORTANT]
> ASDK kullanılan veya bir üretim ortamında desteklenen kullanılmaya yönelik değildir.

ASDK bileşenlerinin tümünü tek Geliştirme Seti ana bilgisayara dağıtılan olduğundan sınırlı fiziksel kaynakların kullanılabilir yok. ASDK dağıtımlarında Azure yığın altyapı VM'ler ve Kiracı sanal makineleri üzerinde bir arada aynı sunucu bilgisayarındaki. Bu yapılandırma, Ölçek veya performans değerlendirme için tasarlanmamıştır.

ASDK bir Azure-karma bulut için tutarlı bir deneyim sağlayacak şekilde tasarlanmıştır:
- **Yöneticiler** (Azure yığın işleçleri). ASDK değerlendirmek ve mevcut Azure yığın hizmetleri hakkında bilgi almak için mükemmel bir kaynaktır.
- **Geliştiriciler**. ASDK, karma veya modern uygulamalar şirket içi (geliştirme ve test ortamları) geliştirmek için kullanılabilir. Bu, Yinelenebilirlik öncesinde veya yanı sıra, Azure yığın üretim dağıtımları geliştirme deneyimi sunar. 

ASDK hakkında daha fazla bilgi için bu kısa videosunu izleyin.

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>ASDK ve çok düğümlü Azure yığın farklılıkları
Tek düğümlü ASDK dağıtımları çok düğümlü Azure yığın dağıtımlarından farkında olmanız gereken birkaç önemli farklar vardır.

|Açıklama|ASDK|Birden çok düğümlü Azure yığını|
|-----|-----|-----|
|**Ölçeklendirme**|Tüm bileşenleri tek düğümlü sunucu bilgisayarda yüklü.|4-12 düğümlerinden boyutu değişebilir.|
|**Esnekliği**|Tek bir düğüm yapılandırması yüksek oranda kullanılabilirlik sağlamaz|[Yüksek kullanılabilirliği](.\.\azure-stack-key-features.md#high-availability-for-azure-stack) özellikleri desteklenir.|
|**Ağ**|ASDK AzS BGPNAT01 adlı bir VM'den tüm ASDK ağ trafiğini yönlendirmek için kullanır. Ek anahtar gereksinimi yoktur.|AzS BGPNAT01 VM çok düğümlü dağıtımlar yok. Daha karmaşık [ağ Yönlendirme Altyapısı](.\.\azure-stack-network.md#network-infrastructure) üst, raf üstü (TOR), temel kart yönetim denetleyicisi (BMC) ve Kenarlık (veri merkezi ağındaki) anahtarları dahil olmak üzere gereklidir.|
|**Düzeltme ve güncelleştirme işlemi**|ASDK yeni bir sürümüne taşımak için Geliştirme Seti ana bilgisayarda ASDK yeniden dağıtmanız gerekir.|[Düzeltme eki ve güncelleştirme](.\.\azure-stack-updates.md) yüklü olan Azure yığın sürümü güncelleştirmek için kullanılan işlem.|
|**Destek**|MSDN Azure yığın forum. Microsoft Müşteri Hizmetleri ve desteği (CSS) desteği *değil* üretim dışı ortamlar için kullanılabilir.|[MSDN Azure yığın Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) ve tam CSS desteği.|
| | |

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetler hakkında bilgi edinin
Bir Azure yığın operatör olarak, hangi hizmetlerin, kullanıcılarınız için kullanılabilir duruma getirebilirsiniz bilmeniz gerekir. Azure yığını Azure hizmetlerinin bir kısmını destekler ve desteklenen hizmetlerin listesini zamanla gelişmeye devam eder.

### <a name="foundational-services"></a>Temel Hizmetleri
Varsayılan olarak, aşağıdaki "temel Hizmetleri" Azure yığın içerir ASDK dağıttığınızda:
- İşlem
- Depolama
- Ağ
- Key Vault

Bu temel hizmetlerle minimal yapılandırma ile kullanıcılarınıza-olarak-hizmet altyapı (Iaas) sunabilir.

### <a name="additional-services"></a>Ek hizmetler
Şu anda aşağıdaki ek olarak bir-hizmet Platform (PaaS) Hizmetleri desteklenir:
- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları

> [!NOTE]
> Bu hizmetler, bunları kullanılabilir kullanıcılarınıza hale getirmeden önce ek yapılandırma gerektirir ve ASDK yüklediğinizde varsayılan olarak kullanılabilir değil.

## <a name="service-roadmap"></a>Hizmet yol haritası
Azure yığın ek Azure Hizmetleri için destek eklemek devam eder. Azure yığın ile sonraki yakında bilgi edinmek için [Azure yığın yol haritası](https://azure.microsoft.com/roadmap/?tag=azure-stack). 


## <a name="next-steps"></a>Sonraki adımlar
Azure yığın değerlendirme başlamak için Geliştirme Seti ana sunucu bilgisayarı hazırlamanız gerekir ve ardından [ASDK yükleme](asdk-install.md). Bundan sonra yönetici ve kullanıcı portalı için Azure yığın kullanmaya başlamak için oturum açabilir.