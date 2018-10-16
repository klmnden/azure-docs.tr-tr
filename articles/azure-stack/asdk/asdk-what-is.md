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
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: fa20f746e55f784e02244355c96ac273b9906acc
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339551"
---
# <a name="what-is-the-azure-stack-development-kit"></a>Azure Stack geliştirme Seti'ni nedir?
[Microsoft Azure Stack tümleşik sistemleri](.\.\azure-stack-poc.md) aralık boyutu 4-12 düğümlerden ve tüm dünyada bir donanım iş ortağı ve Microsoft tarafından desteklenir. Azure Stack tümleşik sistemleri, üretim iş yükleriniz için yeni senaryoları etkinleştirmek için kullanın. Tümleşik sistemler altyapıyı yöneten ve hizmetleri sunan Azure Stack operatörü kullanıyorsanız bkz bizim [operatör belgeleri](https://docs.microsoft.com/azure/azure-stack).

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
|**Ölçeklendirme**|Tüm bileşenleri tek bir düğüm server yüklü bir bilgisayara yüklenir.|4-12 düğümlerden boyutu değişebilir.|
|**Esnekliği**|Tek bir düğüm yapılandırması, yüksek kullanılabilirlik sağlamaz|[Yüksek kullanılabilirlik](.\.\azure-stack-key-features.md#high-availability-for-azure-stack) özellikleri desteklenir.|
|**Ağ**|ASDK AzS-BGPNAT01 isimli bir VM'nin tüm ASDK ağ trafiği yönlendirmek için kullanır. Ek geçiş gereksinimi yoktur.|Çok düğümlü dağıtımda AzS-BGPNAT01 VM yok. Daha karmaşık [ağ Yönlendirme Altyapısı](.\.\azure-stack-network.md#network-infrastructure) Top-Of-Rack (TOR), temel kart yönetim denetleyicisi (BMC) ve Kenarlık (veri merkezi ağı) anahtarları dahil olmak üzere gereklidir.|
|**Düzeltme eki ve güncelleştirme işlemi**|ASDK yeni bir sürümüne taşımak için Geliştirme Seti ana bilgisayarda ASDK yeniden dağıtmanız gerekir.|[Düzeltme eki uygulama ve güncelleştirme](.\.\azure-stack-updates.md) yüklü olan Azure Stack sürümü güncelleştirmek için kullanılan işlem.|
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