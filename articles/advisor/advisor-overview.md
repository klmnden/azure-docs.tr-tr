---
title: Azure Danışmanı giriş | Microsoft Docs
description: Azure Danışmanı, Azure dağıtımlarınızın iyileştirilmesine yönelik kullanın.
services: advisor
documentationcenter: NA
author: kasparks
manager: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: 747ce8dc4a4e4c8ac1e26e5b051a431176096d71
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015985"
---
# <a name="introduction-to-azure-advisor"></a>Azure Danışmanı giriş

Azure Danışmanı özellikleri hakkında bilgi edinin ve sık sorulan soruların yanıtlarını alın.

## <a name="what-is-advisor"></a>Danışman nedir?
Advisor, Azure dağıtımlarınızın iyileştirilmesine yönelik en iyi uygulamaları izlemenize yardımcı olan kişiselleştirilmiş bir bulut danışmanıdır. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

Danışman ile şunları yapabilirsiniz:
* Get proaktif, eyleme dönüştürülebilir ve kişiselleştirilmiş en iyi uygulama önerileri. 
* Genel Azure azaltacak fırsatlar belirleme gibi performans, güvenlik ve kaynaklarınızın yüksek kullanılabilirliğini artırmak ayırın.
* Önerilen eylemleri satır içi ile öneriler alın.

Danışman aracılığıyla erişebileceğiniz [Azure portalında](https://aka.ms/azureadvisordashboard). Oturum [portalı](https://portal.azure.com), bulun **Advisor** Gezinti Menüsü veya içinde arayın **tüm hizmetleri** menüsü.

Danışman Panosu, tüm Abonelikleriniz için kişiselleştirilmiş önerileri görüntüler.  Belirli bir abonelik ve kaynak türleri için öneriler görüntülemek için filtre uygulayabilirsiniz.  Öneriler, dört kategoriye ayrılır: 

* **Yüksek kullanılabilirlik**: İş açısından kritik uygulamalarınızın sürekliliğini ve emin olmak. Daha fazla bilgi için [Advisor yüksek kullanılabilirlik önerisi](advisor-high-availability-recommendations.md).
* **Güvenlik**: tehditleri ve güvenlik ihlallerini yol açabilecek güvenlik açıklarını algılamak üzere. Daha fazla bilgi için [Danışmanı önerilerini](advisor-security-recommendations.md).
* **Performans**: uygulamalarınızın hızını artırmak için. Daha fazla bilgi için [Danışmanı performans önerilerini](advisor-performance-recommendations.md).
* **Maliyet**: en iyi duruma getirmek ve genel Azure harcamalarınızı azaltmak için. Daha fazla bilgi için [Advisor maliyet önerileri](advisor-cost-recommendations.md).

  ![Advisor öneri türleri](./media/advisor-overview/advisor-dashboard.png)

O kategorideki öneriler listesini görüntülemek için bir kategoriye tıklayın ve hakkında daha fazla bilgi edinmek için bir öneri seçin.  Bir fırsattan yararlanın ya da bir sorunu çözmek için gerçekleştirebileceğiniz eylemleri hakkında da bilgi edinebilirsiniz.

![Advisor öneri kategorisi](./media/advisor-overview/advisor-ha-category-example.png) 

Öneriyi uygulamak bir öneri için önerilen eylemi seçin.  Basit bir arabirim, öneri uygulamak ya da yapıyı yönetmenize yardımcı olan belgeler başvurmak sağlayan açılır.  Bir öneri uygulamak sonra bunu, tanımak Advisor için bir güne kadar sürebilir.

Bir öneriye derhal eylemde düşünmüyorsanız, belirli bir süre için erteleme ya da reddedebilir.  Belirli bir abonelik veya kaynak grubu için öneriler almak istemiyorsanız, yalnızca belirtilen Abonelikleriniz ve kaynak gruplarınız için öneriler oluşturmak için Advisor yapılandırabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-access-advisor"></a>Advisor'ı nasıl erişebilirim?
Danışman aracılığıyla erişebileceğiniz [Azure portalında](https://aka.ms/azureadvisordashboard). Oturum [portalı](https://portal.azure.com), bulun **Advisor** Gezinti Menüsü veya içinde arayın **tüm hizmetleri** menüsü.

Sanal makine kaynak arabirimi aracılığıyla Danışmanı önerilerini de görüntüleyebilirsiniz. Bir sanal makine seçin ve Danışmanı önerilerini menüsünde'na kaydırın. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Advisor'a erişmek hangi izinlerin gerekiyor?
 
Danışman önerileri olarak erişebilir *sahibi*, *katkıda bulunan*, veya *okuyucu* bir abonelik.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Hangi kaynakların Advisor için öneriler sağlar mı?

Advisor, Redis için sanal makineler, kullanılabilirlik kümeleri, uygulama ağ geçitleri, uygulama hizmetleri, SQL Server ve Azure Cache için öneriler sağlar.

### <a name="can-i-postpone-or-dismiss-a-recommendation"></a>Erteleme veya miyim bir öneri Kapat?

Erteleyebilir veya bir öneri kapatmak için tıklayın **Ertele'yi** bağlantı. Bir Ertele'yi dönem veya select belirtebilirsiniz **hiçbir zaman** öneri kapatılamadı.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz:

* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor yüksek kullanılabilirlik önerisi](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)
* [Danışmanı performans önerileri](advisor-performance-recommendations.md)
* [Advisor maliyet önerileri](advisor-cost-recommendations.md)
