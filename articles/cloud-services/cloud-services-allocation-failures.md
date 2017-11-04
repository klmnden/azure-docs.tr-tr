---
title: "Bulut hizmeti ayırma hatası sorunlarını giderme | Microsoft Docs"
description: "Azure’da Cloud Services dağıtımı sırasında oluşan ayırma hatalarını giderme"
services: azure-service-management, cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: 33d017d0e09e9b288b0514e85c8865f83a8a2fa1
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Azure’da Cloud Services dağıtımı sırasında oluşan ayırma hatalarını giderme
## <a name="summary"></a>Özet
Örnek bir bulut hizmetine dağıtma veya yeni web veya çalışan rolü örnekleri eklediğinizde, Microsoft Azure işlem kaynakları ayırır. Zaman zaman bile Azure abonelik limitleri düşmeden önce bu işlemleri yaparken hatalar alabilirsiniz. Bu makalede, bazı ortak ayırma hatalarının nedenlerini açıklar ve olası düzeltme önerir. Hizmetlerinizin dağıtımını planlarken bilgiler de yararlı olabilir.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Arka plan – ayırma nasıl çalışır?
Sunucuları Azure veri merkezlerinde kümeler halinde bölümlenir. Yeni bir bulut hizmeti ayırma isteği birden çok kümelerde denenir. İlk örneği, bulut hizmeti bir bulut hizmetinde için (hazırlama ya da üretim), dağıtıldığında bir kümeye sabitlenmiş. Bulut hizmeti için diğer tüm dağıtımlar aynı kümede gerçekleşir. Bu makalede, biz bu için "bir kümeye sabitlenmiş gibi" başvuruyor. Aşağıdaki diyagram 1, birden çok kümelerde denenir normal bir ayırma durumunun gösterir; Diyagram 2 bir ayırma durumunun gösterir, var olan bulut hizmeti CS_1 barındırıldığı olduğu için küme 2'ye sabitlenmiş.

![Ayırma diyagramı](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Ayırma hatası neden olur
Ayırma isteği bir kümeye sabitlenmiş, kullanılabilir kaynak havuzu bir kümeye sınırlı olduğundan boş kaynakları bulmak başarısız olan, daha yüksek bir fırsat yok. Küme ücretsiz kaynağa sahip olsa bile Ayrıca, bir kümeye ayırma isteği sabitlenmiş ancak, istenen kaynak türü, küme tarafından desteklenmiyor, isteğiniz başarısız olur. Aşağıdaki diyagram 3 yalnızca adayı küme kaynakları serbest bırakmak olmadığından burada Sabitlenmiş ayırma başarısız durumda gösterir. Diyagram 4 yalnızca adayı küme istenen VM boyutu desteklemediği için küme kaynakları serbest bırakmak sahip olsa bile burada Sabitlenmiş ayırma başarısız durumda gösterir.

![Sabitlenmiş ayırma hatası](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Bulut Hizmetleri için sorun giderme ayırma hatası
### <a name="error-message"></a>Hata iletisi
Aşağıdaki hata iletisini görebilirsiniz:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Yaygın Sorunlar
Tek bir küme için sabitlenmelidir ayırma isteği neden ortak ayırma senaryolar verilmiştir.

* Bir bulut hizmeti her iki yuvada bir dağıtım varsa hazırlama yuvası - dağıtma, ardından tüm bulut hizmeti için belirli bir kümeyi sabitlendi.  Bu, zaten bir dağıtım üretim yuvasında zaten varsa, ardından yeni bir hazırlama dağıtımı yalnızca üretim yuvasına aynı kümedeki ayrılabilen olduğunu anlamına gelir. Küme kapasitesine yaklaşıyor isteği başarısız olabilir.
* Ölçeklendirme - yeni örnekleri olan bir bulut hizmetini ekleme aynı kümede ayırmanız gerekir.  Küçük istekleri ölçekleme genellikle ayrılan, ancak her zaman değil. Küme kapasitesine yaklaşıyor isteği başarısız olabilir.
* Bulut hizmeti bir benzeşim grubuna sabitlenmiş sürece benzeşim grubu - bu bölgedeki herhangi bir küme yapıda tarafından boş bir bulut hizmetinde için yeni bir dağıtım ayrılabilir. Aynı benzeşim grubuna dağıtımları aynı kümede denenir. Küme kapasitesine yaklaşıyor isteği başarısız olabilir.
* Benzeşim grubu vNet - eski sanal ağları bölgeleri yerine benzeşim grupları için bağlı ve bu sanal ağlar bulut Hizmetleri'nde benzeşim grubu kümeye sabitlenmiş. Bu sanal ağ türü için dağıtımlar sabitlenmiş kümede denenir. Küme kapasitesine yaklaşıyor isteği başarısız olabilir.

## <a name="solutions"></a>Çözümler
1. Yeni bir bulut hizmeti yeniden - bu bölgedeki tüm kümelerin aralarından seçim yapabileceğiniz platform verdiğinden en başarılı olması bu çözüm olasıdır.

   * Yeni bir bulut hizmeti için iş yükü dağıtma  
   * CNAME veya bir kayıt trafiği yeni bulut hizmetine işaret edecek şekilde güncelleştirme
   * Sıfır trafiği eski siteye geçiyor sonra eski bulut hizmeti silebilirsiniz. Bu çözüm, sıfır kapalı kalma süresi maruz.
2. Üretim ve hazırlama yuvası delete - Bu çözüm, var olan DNS adınızı korur ancak uygulamanıza kesinti süresine neden olacak.

   * Üretim ve bulut hizmeti boş, böylece var olan bir bulut hizmetini yuvaları hazırlama silin ve ardından
   * Yeni bir dağıtım varolan bulut hizmeti oluşturun. Bu bölgedeki tüm kümelerde ayırma yeniden dener. Bulut hizmeti bir benzeşim grubuna bağlı değildir emin olun.
3. Ayrılmış IP - Bu çözüm korumak, var olan IP adresi, ancak uygulamanıza kesinti süresine neden olacak.  

   * Powershell kullanarak, varolan dağıtım için bir ReservedIP oluşturma

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Yukarıdaki, hizmetin CSCFG yeni ReservedIP belirtmek emin #2 izleyin.
4. Yeni dağıtımlar - benzeşim grubunun benzeşim grupları artık önerilen kaldırın. #1 yeni bir bulut hizmeti dağıtmak için yukarıdaki adımları izleyin. Bulut hizmeti bir benzeşim grubunda değil emin olun.
5. Bir bölgesel sanal ağa - bakın Dönüştür [benzeşim grupları bölgesel bir sanal ağa (VNet) geçirmek nasıl](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
