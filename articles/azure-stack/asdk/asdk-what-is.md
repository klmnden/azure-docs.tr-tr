---
title: Azure Stack geliştirme Seti'ni (ASDK) giriş | Microsoft Docs
description: ASDK yenilikler açıklanır ve Microsoft Azure Stack değerlendirme ortak kullanım durumları.
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
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 54eb2ff43a5f36999294b8d0c580bc425ab65b28
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629064"
---
# <a name="what-is-the-azure-stack-development-kit"></a>Azure Stack geliştirme Seti'ni nedir?
[Microsoft Azure Stack tümleşik sistemleri](../azure-stack-poc.md) aralık boyutu 4-16 düğümlerden ve tüm dünyada bir donanım iş ortağı ve Microsoft tarafından desteklenir. Azure Stack tümleşik sistemleri, üretim iş yükleriniz için yeni senaryoları etkinleştirmek için kullanın. Tümleşik sistemler altyapıyı yöneten ve hizmetleri sunan Azure Stack operatörü kullanıyorsanız bkz bizim [operatör belgeleri](https://docs.microsoft.com/azure/azure-stack).

Azure Stack geliştirme Seti'ni (ASDK) indirin ve kullanabileceğiniz bir Azure Stack, tek düğümlü dağıtımıdır **ücretsiz**. Tek bir ana bilgisayar sunucu bilgisayarda çalışan karşılayan veya aşan sanal makinelerin içinde tüm ASDK bileşenler yüklü [en düşük donanım gereksinimlerini](asdk-deploy-considerations.md#hardware). ASDK Azure Stack'i değerlendirin ve Azure'da tutarlı araç ve API'leri kullanarak modern uygulamalar geliştirin, bir ortam sağlamak üzere tasarlanmıştır bir *üretim dışı* ortam. 

> [!IMPORTANT]
> ASDK kullanılabilir veya desteklenen bir üretim ortamında kullanılmaya yönelik değildir.

ASDK bileşenlerinin tümünü tek bir geliştirme seti ana bilgisayara dağıtıldığından, sınırlı fiziksel kaynakları kullanılabilen vardır. ASDK dağıtımlarda Azure Stack altyapısı Vm'leri ve Kiracı Vm'leri üzerinde bir arada aynı sunucu bilgisayarındaki. Bu yapılandırma, Ölçek veya performans değerlendirmesi için tasarlanmamıştır.

ASDK için Azure ile tutarlı bir hibrit bulut deneyimi sunmak üzere tasarlanmıştır:
- **Yöneticiler** (Azure Stack operatörleri). ASDK değerlendirmek ve kullanılabilir Azure Stack hizmetleri hakkında bilgi edinmek için harika bir kaynaktır.
- **Geliştiriciler**. ASDK, karma veya modern uygulamalar şirket içi (geliştirme/test ortamlarını) geliştirmek için kullanılabilir. Bu geliştirme deneyiminin yanı sıra, Azure Stack üretim dağıtımları veya öncesinde yinelenebilirliği sunar. 

ASDK hakkında daha fazla bilgi için şu kısa videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>ASDK ve çok düğümlü Azure Stack farklılıkları
Tek düğümlü ASDK dağıtımları çok düğümlü Azure Stack dağıtımları farkında olmanız gereken birkaç önemli şekilde değişir.

|Açıklama|ASDK|Çok düğümlü Azure Stack|
|-----|-----|-----|
|**Ölçeklendirme**|Tüm bileşenleri tek bir düğüm server yüklü bir bilgisayara yüklenir.|4-16 düğüm boyutu değişebilir.|
|**Esnekliği**|Tek bir düğüm yapılandırması, yüksek kullanılabilirlik sağlamaz|[Yüksek kullanılabilirlik](../azure-stack-overview.md#providing-high-availability) özellikleri desteklenir.|
|**Ağ**|ASDK konak tüm ASDK ağ trafiğini yönlendirir. Ek geçiş gereksinimi yoktur.|Daha karmaşık [ağ Yönlendirme Altyapısı](../azure-stack-network.md#network-infrastructure) Top-Of-Rack (TOR), temel kart yönetim denetleyicisi (BMC) ve Kenarlık (veri merkezi ağı) anahtarları dahil olmak üzere çok düğümlü dağıtımda gereklidir.|
|**Düzeltme eki ve güncelleştirme işlemi**|ASDK yeni bir sürümüne taşımak için Geliştirme Seti ana bilgisayarda ASDK yeniden dağıtmanız gerekir.|[Düzeltme eki uygulama ve güncelleştirme](../azure-stack-updates.md) yüklü olan Azure Stack sürümü güncelleştirmek için kullanılan işlem.|
|**Destek**|Azure Stack MSDN Forumu. Microsoft Müşteri Hizmetleri ve desteği (CSS) desteği *değil* üretim dışı ortamlar için kullanılabilir.|[Azure Stack MSDN Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) ve tam CSS desteği.|
| | |

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetleri hakkında bilgi edinin
Azure Stack operatör hangi hizmetlerin, kullanıcılarınıza sunabileceğiniz bilmeniz gerekir. Azure Stack Azure hizmetlerin bir alt kümesini destekler ve desteklenen hizmet listesini zamanla gelişmesinin devam eder.

### <a name="foundational-services"></a>Temel Hizmetleri
Varsayılan olarak, Azure Stack şu "temel" Hizmetleri ASDK dağıtırken:
- İşlem
- Depolama
- Ağ
- Key Vault

Bu temel hizmetlerle minimal yapılandırma ile kullanıcılarınıza-olarak-hizmet altyapı (Iaas) sunabilir.

### <a name="additional-services"></a>Ek hizmetler
Şu anda, aşağıdaki ek olarak bir-hizmet Platform (PaaS) Hizmetleri desteklenmektedir:
- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları

> [!NOTE]
> Bu hizmetler, bunları kullanıcılarınıza sunabileceğiniz önce ek yapılandırma gerektirir ve ASDK yüklediğinizde varsayılan olarak kullanılabilir değil.

## <a name="service-roadmap"></a>Hizmet yol haritası
Azure Stack, ek Azure Hizmetleri için destek eklemeye devam edeceğiz. Azure Stack ile beklediğini bilgi edinmek için [Azure Stack yol haritası](https://azure.microsoft.com/roadmap/?tag=azure-stack). 


## <a name="next-steps"></a>Sonraki adımlar
Azure Stack değerlendirme başlama için öncelikle gerekir [son ASDK indirme](asdk-download.md) ve ASDK ana bilgisayar hazırlayın. Geliştirme Seti konak hazırladıktan sonra ASDK yüklemek ve Azure Stack kullanmaya başlamak için yönetici ve kullanıcı portalı için oturum açın.