---
title: Bulut hizmeti ayırma hatalarıyla ilgili sorunları giderme | Microsoft Docs
description: Azure’da Cloud Services dağıtımı sırasında oluşan ayırma hatalarını giderme
services: azure-service-management, cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: d1f24c3661a23496d1873f12ce46083bf5258269
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61435517"
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Azure’da Cloud Services dağıtımı sırasında oluşan ayırma hatalarını giderme
## <a name="summary"></a>Özet
Örnek bir bulut hizmetine dağıtın veya yeni bir web veya çalışan rolü örnekleri eklemek, Microsoft Azure işlem kaynakları ayırır. Azure abonelik limitleri geçebilmek bu işlemleri yaparken ara sıra hatalar alabilirsiniz. Bu makalede, ortak bir ayırma hatalarının bazı nedenleri açıklanır ve olası düzeltmeler önerir. Hizmetlerinizi dağıtımını planlarken bilgiler de yararlı olabilir.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Arka plan – ayırma nasıl çalışır?
Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Yeni bir bulut hizmeti ayırma isteği birden fazla kümede denenir. İlk örneği bulut hizmeti bulut hizmeti için (içinde hazırlama veya üretim) dağıtıldığında bir kümeye sabitlenebilir. Bulut hizmeti için diğer tüm dağıtımlar, aynı kümede gerçekleşir. Bu makalede, bu "bir kümeye sabitlenebilir gibi" diyoruz. Aşağıdaki diyagramda 1 birden fazla kümede denenir, normal bir ayırma durumu gösterir; Diyagram 2 ayırma durumunu gösterir, var olan bulut hizmeti CS_1 barındırıldığı olduğu için kümeye 2'ye sabitlenmiş.

![Ayırma diyagramı](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Ayırma hatası neden olur
Ayırma isteği için bir küme sabitlenmiş olduğunda daha yüksek kullanılabilir bir kaynak havuzu bir kümeye sınırlı olduğundan ücretsiz kaynakları bulmak başarısız olan kaybedilebilir. Ayrıca, küme ücretsiz kaynağa sahip olsa bile, ayırma isteğinin bir kümeye sabitlenmiş, ancak istediğiniz kaynak türünü, küme tarafından desteklenmiyor, isteği reddeder. Aşağıdaki diyagramda 3 yalnızca aday küme kaynakları serbest bırakmak olmadığından burada sabitlenmiş bir ayırma başarısız durum gösterilmektedir. Diyagram 4 yalnızca aday küme istenen VM boyutu desteklemediği için küme ücretsiz kaynaklara sahip olsa da burada bir Sabitlenmiş ayırma başarısız durumda gösterir.

![Sabitlenmiş ayırma hatası](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Bulut Hizmetleri için oluşan ayırma hatalarını giderme
### <a name="error-message"></a>Hata İletisi
Aşağıdaki hata iletisini görebilirsiniz:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Genel Sorunlar
Tek bir küme için sabitlenmelidir ayırma isteği neden ortak bir ayırma senaryoları aşağıda verilmiştir.

* Bir bulut hizmeti ya da yuvasında bir dağıtım varsa - hazırlama yuvasına dağıtma, ardından tüm bulut hizmeti için belirli bir küme sabitlenir.  Diğer bir deyişle, üretim yuvasında bir dağıtım zaten varsa, yeni bir hazırlama dağıtımı yalnızca üretim yuvası ile aynı kümede ayrılabilir. Kümenin kapasitesine yaklaşıyor istek başarısız.
* Ölçeklendirme - mevcut bir bulut hizmeti için yeni örnekler ekleniyor, aynı kümede ayırmanız gerekir.  Küçük ölçeklendirme istekleri her zaman olmasa da genellikle ayrılabilir. Kümenin kapasitesine yaklaşıyor istek başarısız.
* Bulut hizmeti bir benzeşim grubuna Sabitlenen sürece benzeşim grubu - bu bölgedeki herhangi bir küme dokusunda tarafından boş bir bulut hizmetinde yeni bir dağıtım ayrılabilir. Dağıtımları için aynı benzeşim grubu, aynı kümede denenecek. Kümenin kapasitesine yaklaşıyor istek başarısız.
* Benzeşim grubu vNet - eski sanal ağları benzeşim grupları yerine bölge için bağlı ve bulut hizmetlerini bu sanal ağlardaki benzeşim grubu kümeye sabitlenmiş. Bu sanal ağ türü için dağıtımlar sabitlenmiş kümede denenecek. Kümenin kapasitesine yaklaşıyor istek başarısız.

## <a name="solutions"></a>Çözümler
1. Yeni bir bulut hizmetine yeniden dağıtın: Bu platform bu bölgedeki tüm kümeler aralarından seçim yapabileceğiniz sağladığından en başarılı olma olasılığı bir çözümdür.

   * İş yükü için yeni bir bulut hizmeti dağıtma  
   * CNAME veya bir kayıt trafiği yeni bulut hizmetine işaret edecek şekilde güncelleştirin
   * Sıfır trafiği, eski siteye gitmesini sonra eski bulut hizmeti silebilirsiniz. Bu çözüm, sıfır kapalı kalma süresi tabi.
2. Hem üretim hem de hazırlık yuvalarını silin: Bu çözüm, mevcut bir DNS adını korur, ancak uygulamanız için kesintiye neden.

   * Üretim ve bulut hizmeti boş, böylece yuvası var olan bir bulut hizmetinin hazırlık silin ve ardından
   * Var olan bir bulut hizmetinde yeni bir dağıtımını oluşturun. Bu bölgedeki tüm kümelerde ayırma yeniden dener. Bulut hizmeti bir benzeşim grubuna bağlı değildir emin olun.
3. Ayrılmış IP - Bu çözüm korumak, mevcut IP adresi, ancak uygulamanız için kesintiye neden.  

   * Powershell kullanarak mevcut dağıtımınız için bir ReservedIP oluşturma

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Yukarıdaki hizmetin CSCFG içinde yeni ReservedIP belirtmek sağlamaktan #2 izleyin.
4. -Yeni dağıtımlar için benzeşim grubu, benzeşim grupları artık önerilen kaldırın. Yeni bir bulut hizmeti dağıtmak için yukarıdaki #1 için adımları izleyin. Bulut hizmeti bir benzeşim grubunda olmadığından emin olun.
5. Bir bölgesel sanal ağa - bakın dönüştürme [benzeşim grupları bir bölgesel sanal ağa (VNet) geçirme](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
