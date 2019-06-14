---
title: Dağıtım sorunları için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs
description: Bu makalede, Microsoft Azure Cloud Services dağıtımı hakkında sık sorulan soruları listelenmektedir.
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
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 08d74f866fe28a4c424ba504795b4a22f09785ca
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60337332"
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure Cloud Services dağıtım sorunları: Sık sorulan sorular (SSS)

Bu makale için dağıtım sorunları hakkında sık sorulan sorular içerir [Microsoft Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services). Ayrıca başvurabilirsiniz [bulut Hizmetleri VM boyutu sayfası](cloud-services-sizes-specs.md) boyutu bilgi.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Mevcut bir dağıtımı üretim yuvasında zaten varsa neden bir bulut hizmeti bazen hazırlama yuvasına dağıtma kaynak ayırma hatası ile başarısız oluyor?
Bir bulut hizmeti ya da yuvasında bir dağıtım varsa, tüm bulut hizmeti için belirli bir küme sabitlenir. Bu, zaten bir dağıtım üretim yuvasında zaten varsa, yeni bir hazırlama dağıtımı yalnızca aynı kümedeki üretim yuvasına olarak ayrılabilen anlamına gelir.

Bulut hizmetinizin bulunduğu küme dağıtım isteğinizi karşılamak için yeterli fiziksel hesaplama kaynağı yok ayırma hataları ortaya çıkar.

Tür ayırma hatalarını giderme hakkında Yardım için bkz. [bulut hizmeti ayırma hatası: Çözümleri](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Artırma veya azaltma giden bir bulut hizmeti dağıtımı bazen ayırma hataya neden neden mu?
Bir bulut hizmeti dağıtılırken, genellikle belirli bir kümeye sabitlenebilir. Başka bir deyişle, var olan bir bulut hizmeti yukarı/out ölçeklendirme, aynı kümede yeni örnekleri ayırmanız gerekir. Kümenin kapasitesine yaklaşıyor veya istenen VM boyutuna/türüne kullanılabilir değilse, istek başarısız olabilir.

Tür ayırma hatalarını giderme hakkında Yardım için bkz. [bulut hizmeti ayırma hatası: Çözümleri](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Neden bir bulut hizmetinde bir benzeşim grubuna bazen dağıtma ayırma hatasına neden?
Bulut hizmeti bir benzeşim grubuna Sabitlenen sürece bu bölgedeki herhangi bir küme dokusunda tarafından boş bir bulut hizmetinde yeni bir dağıtım ayrılabilir. Dağıtımları için aynı benzeşim grubu, aynı kümede denenecek. Kümenin kapasitesine yaklaşıyor istek başarısız.

Tür ayırma hatalarını giderme hakkında Yardım için bkz. [bulut hizmeti ayırma hatası: Çözümleri](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Neden yeni bir VM için mevcut bir bulut hizmeti bazen ekleme veya VM boyutunu değiştirme ayırma hatasına neden?
Bir veri merkezi içinde kümeler makine türleri (örneğin, bir serisi, Av2 serisi, D serisi, Dv2 serisi, G serisi, H serisi, vb.) farklı yapılandırmaları olabilir. Ancak tüm kümeler VM'lerin her türlü olması gerekir. Örneğin, bir A serisi-yalnızca kümesinde zaten dağıtılmış bir bulut hizmeti için D serisi VM eklemeyi denerseniz, bir ayırma hatası karşılaşırsınız. Değiştirmeye çalışırsanız, bu da (örneğin, bir A serisinden bir D serisine değiştirme) VM SKU boyutları gerçekleşir.

Tür ayırma hatalarını giderme hakkında Yardım için bkz. [bulut hizmeti ayırma hatası: Çözümleri](cloud-services-allocation-failures.md#solutions).

Bölgenizde boyutlarına denetlemek için bkz: [Microsoft Azure: Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Neden bir bulut hizmeti süre dağıtma abonelik veya hizmet sınırlamaları/kotalar/kısıtlamaları nedeniyle başarısız oluyor?
Ayrılacak gerekli kaynakları varsayılan veya bölge/veri merkezi düzeyinde hizmetiniz için izin verilen en yüksek kota aşarsanız, bir bulut hizmeti dağıtımının başarısız olabilir. Daha fazla bilgi için [Cloud Services sınırlar](../azure-subscription-service-limits.md#azure-cloud-services-limits).

Ayrıca, aboneliğinizi portal için geçerli kullanım/kota izlemek: Azure portalında = > abonelikler = > \<uygun abonelik > = > "Kullanım + kota".

Kaynak kullanım/kullanımı ile ilgili bilgileri de Azure faturalandırma API'leri alınabilir. Bkz: [Azure kaynak kullanım API'si (Önizleme)](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Dağıtılan bulut hizmeti VM boyutunu yeniden dağıtmaya gerek kalmadan nasıl değiştirebilirim?
Yeniden dağıtmaya gerek kalmadan, dağıtılmış bir bulut hizmetinde VM boyutunu değiştiremezsiniz. VM boyutu, yalnızca bir yeniden dağıtma ile güncelleştirilebilir CSDEF yerleşiktir.

Daha fazla bilgi için [bir bulut hizmeti güncelleştirme](cloud-services-update-azure-service.md).

## <a name="why-am-i-not-able-to-deploy-cloud-services-through-service-management-apis-or-powershell-when-using-azure-resource-manager-storage-account"></a>Neden Azure Resource Manager depolama hesabı kullanılırken Hizmet Yönetim API'leri veya PowerShell aracılığıyla bulut hizmetlerini dağıtımında gönderemiyorum? 

Bulut hizmeti Azure Resource Manager modeliyle doğrudan uyumlu olmayan bir Klasik kaynak olduğundan, Azure Resource Manager depolama hesapları ile ilişkilendiremezsiniz. Bazı seçenekler şunlardır: 
 
- REST API aracılığıyla dağıtma.

    Hizmet Yönetimi REST API'si dağıttığınızda, blob depolama, hem Klasik hem de Azure Resource Manager depolama hesabı ile çalışması için bir SAS URL'sini belirterek sınırlamayı alabilir. 'PackageUrl' özelliği hakkında daha fazla bilgiyi [burada](/previous-versions/azure/reference/ee460813(v=azure.100)).
  
- Aracılığıyla dağıtma [Azure portalında](https://portal.azure.com).

    Bu çalışır [Azure portalında](https://portal.azure.com) çağrı proxy/Azure Resource Manager ve klasik kaynaklar arasında iletişime izin veren dolgu üzerinden geçtikçe. 
 
## <a name="why-does-azure-portal-require-me-to-provide-a-storage-account-for-deployment"></a>Azure portalında neden dağıtım için bir depolama hesabı sağlayın gerektiriyor mu? 

Klasik Portalı'nda paket yönetim API katmana doğrudan karşıya yüklendi ve ardından API katmanı geçici olarak paket bir iç depolama hesabına koyabilirsiniz.  Bu işlem API katmanı bir dosya karşıya yükleme hizmeti tasarlanmadığı için performans ve ölçeklenebilirlik sorunlarına neden olur.  Azure portalında (Resource Manager dağıtım modeli) daha hızlı ve daha güvenilir dağıtımlarda kaynaklanan biz ilk API katmanı için karşıya adım atlanır. 

Maliyet, olduğu gibi çok küçüktür ve aynı depolama hesabındaki tüm dağıtımlar arasında yeniden kullanabilirsiniz. Kullanabileceğiniz [depolama maliyet hesaplayıcı](https://azure.microsoft.com/pricing/calculator/#storage1) hizmet paketini (CSPKG) karşıya yüklemek için maliyet belirlemek için CSPKG indirin ve ardından CSPKG silebilirsiniz. 
