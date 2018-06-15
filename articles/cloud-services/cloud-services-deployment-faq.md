---
title: Dağıtım sorunları için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs
description: Bu makalede, Microsoft Azure bulut Hizmetleri için dağıtım hakkında sık sorulan sorular listelenmektedir.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 05217129d4993514acaf8c717847040584984cb3
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34068913"
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure bulut Hizmetleri için dağıtım sorunlarını: sık sorulan sorular (SSS)

Bu makale için dağıtım sorunları hakkında sık sorulan sorular içerir [Microsoft Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services). Ayrıca başvurabilirsiniz [bulut Hizmetleri VM boyutu sayfa](cloud-services-sizes-specs.md) boyutu bilgi.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Var olan bir dağıtıma üretim yuvasında zaten varsa neden bir bulut hizmeti hazırlama yuvasını bazen dağıtma bir kaynak ayırma hatası ile başarısız oluyor?
Bir bulut hizmeti her iki yuvada dağıtımı varsa, tüm bulut hizmeti için belirli bir kümeyi sabitlenir. Bu, zaten bir dağıtım üretim yuvasında zaten varsa, yeni bir hazırlama dağıtımı yalnızca üretim yuvasına aynı kümedeki ayrılabilen olduğunu anlamına gelir.

Bulut hizmetinizi bulunduğu küme dağıtım isteği karşılamak için yeterli fiziksel işlem kaynaklarını yok ayırma hataları oluşur.

Bu tür ayırma hataları Azaltıcı Yardım için bkz. [bulut hizmeti ayırma hatası: çözümleri](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Neden ölçeklendirmeyi veya bir bulut hizmeti dağıtımı bazen neden ayırma hatası çıkış ölçeklendirme mu?
Bir bulut hizmeti dağıtıldığında, genellikle belirli bir kümeye sabitlenmiş. Bu, var olan bir bulut hizmeti yukarı/giden ölçeklendirme yeni örnekleri aynı kümedeki ayırmalısınız anlamına gelir. Küme kapasitesine yaklaşıyor veya istenen VM boyutu/türü kullanılabilir değilse, istek başarısız olabilir.

Bu tür ayırma hataları Azaltıcı Yardım için bkz. [bulut hizmeti ayırma hatası: çözümleri](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Neden bir bulut hizmeti bir benzeşim grubuna bazen dağıtma ayırma hatasına neden?
Bulut hizmeti bir benzeşim grubuna sabitlenmiş sürece bu bölgedeki herhangi bir küme yapıda tarafından boş bir bulut hizmetinde için yeni bir dağıtım ayrılabilir. Aynı benzeşim grubuna dağıtımları aynı kümede denenir. Küme kapasitesine yaklaşıyor isteği başarısız olabilir.

Bu tür ayırma hataları Azaltıcı Yardım için bkz. [bulut hizmeti ayırma hatası: çözümleri](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Neden olan bir bulut hizmetini yeni bir VM bazen ekleme veya VM boyutunu değiştirme ayırma hatasına neden?
Bir veri merkezinde kümeleri makine türlerinin (örneğin, bir seri, Av2 serisi, D serisi, Dv2 serisi, G serisi, H serisi, vb.) farklı yapılandırmalara sahip. Ancak tüm kümeler mutlaka VM'ler her türlü gerekir. Örneğin, bir A yalnızca serisi küme zaten dağıtılmış bir bulut Hizmeti D serisinin VM eklemeye çalışırsanız, bir ayırma hatası karşılaşırsınız. Değiştirmeye çalışırsanız, bu da (örneğin, bir A serisinden D seriye değiştirme) VM SKU boyutları gerçekleşir.

Bu tür ayırma hataları Azaltıcı Yardım için bkz. [bulut hizmeti ayırma hatası: çözümleri](cloud-services-allocation-failures.md#solutions).

Bölgenizde boyutları denetlemek için bkz: [Microsoft Azure: bölgeye göre ürünleri](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Neden bir bulut hizmeti süre dağıtma abonelik veya hizmeti sınırları/kotaları/kısıtlamaları nedeniyle başarısız oluyor?
Ayrılması için gereken kaynakları varsayılan veya bölge/veri merkezi düzeyinde hizmetiniz için izin verilen en yüksek kota aşarsanız bir bulut hizmeti dağıtımının başarısız olabilir. Daha fazla bilgi için bkz: [bulut Hizmetleri sınırlar](../azure-subscription-service-limits.md#cloud-services-limits).

Aboneliğiniz için portal için geçerli kullanım/kotası izlemek de: Azure portal = > abonelikleri = > \<uygun abonelik > = > "Kullanım + kota".

Kaynak kullanım/kullanımı ile ilgili bilgileri de Azure faturalama API'leri alınabilir. Bkz: [Azure kaynak kullanım API'si (Önizleme)](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Dağıtılan bulut hizmeti VM boyutunu yeniden dağıtmadan nasıl değiştirebilirim?
Dağıtılan bulut hizmeti VM boyutunu yeniden dağıtmadan değiştiremezsiniz. VM boyutu ile yeniden dağıtın yalnızca güncelleştirilebilir CSDEF içinde yerleşik olarak bulunur.

Daha fazla bilgi için bkz: [bir bulut hizmeti güncelleştirmek nasıl](cloud-services-update-azure-service.md).

## <a name="why-am-i-not-able-to-deploy-cloud-services-through-service-management-apis-or-powershell-when-using-azure-resource-manager-storage-account"></a>Neden Azure Resource Manager depolama hesabı kullanırken, hizmet yönetim API'leri veya PowerShell ile bulut Hizmetleri dağıtabilmek için değilim? 

Bulut hizmeti ile Azure Resource Manager modeli doğrudan uyumlu olmayan bir Klasik kaynak olduğundan, Azure Resource Manager depolama hesaplarıyla ilişkilendiremezsiniz. Bazı seçenekler şunlardır: 
 
- REST API aracılığıyla dağıtma.

    Hizmet Yönetimi REST API aracılığıyla dağıttığınızda, hem Klasik hem de Azure Resource Manager depolama hesabı ile çalışacak blob depolama için bir SAS URL'si belirterek sınırlamaya geçici alabilir. 'PackageUrl' özelliği hakkında daha fazla bilgiyi [burada](https://msdn.microsoft.com/library/azure/ee460813.aspx).
  
- Aracılığıyla dağıtma [Azure portal](https://portal.azure.com).

    Bu gelen çalışır [Azure portal](https://portal.azure.com) gibi bir proxy/Azure Resource Manager ve klasik kaynaklar arasında iletişime izin veren dolgusu çağrı geçer. 
 
## <a name="why-does-azure-portal-require-me-to-provide-a-storage-account-for-deployment"></a>Azure portal neden dağıtımı için bir depolama hesabı sağlayın gerektiriyor mu? 

Klasik Portalı'ndaki paketi yönetim API katmanı doğrudan yüklendi, ve ardından API katmanı geçici olarak paketi bir iç depolama dikkate alın.  Bu işlem, API katmanı dosyası yükleme hizmeti tasarlanmadığı için performans ve ölçeklenebilirlik sorunlarının neden olur.  Azure portalında (Resource Manager dağıtım modeli) daha hızlı ve daha güvenilir dağıtımlarda kaynaklanan biz ilk API katmana karşıya ara adım atlanır. 

Maliyet için çok küçük olursa ve aynı depolama hesabındaki tüm dağıtımlar arasında yeniden kullanabilirsiniz. Kullanabileceğiniz [depolama maliyeti hesaplayıcı](https://azure.microsoft.com/pricing/calculator/#storage1) (CSPKG) hizmet paketini karşıya yüklemek için maliyet belirlemek için CSPKG karşıdan yükleyin, ardından CSPKG silin. 
