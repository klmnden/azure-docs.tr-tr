---
title: "Azure Danışmanı giriş | Microsoft Docs"
description: "Azure dağıtımlarınızı iyileştirmek için Azure Danışmanı'nı kullanın."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: a4096b11a828cf6676aa22b11c4dd4d75f3b0286
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="introduction-to-azure-advisor"></a>Azure Danışmanı giriş

Azure Danışmanı anahtar özellikleri hakkında bilgi edinmek ve sık sorulan soruların yanıtlarını alın.

## <a name="what-is-advisor"></a>Advisor nedir?
Advisor Azure dağıtımlarınızı iyileştirmek için en iyi uygulamaları izleyerek yardımcı olan kişiselleştirilmiş bulut Danışman ' dir. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

Advisor ile şunları yapabilirsiniz:
* Get öngörülü, işlem yapılabilir ve öneriler kişiselleştirilmiş en iyi yöntemler. 
* Genel Azure azaltmak için fırsatlarını tanımlayabilmeniz gibi performans, güvenlik ve kaynaklarınızın yüksek kullanılabilirliğini artırmak ayırın.
* Önerilen eylemleri satır içi ile ilgili öneriler alın.

Advisor aracılığıyla erişebilirsiniz [Azure portal](https://aka.ms/azureadvisordashboard). Oturum [portal](https://portal.azure.com), bulun **Danışmanı** Gezinti Menüsü veya içinde arama **tüm hizmetleri** menüsü.

Advisor Pano kişiselleştirilmiş önerileri için tüm aboneliklerinizi görüntüler.  Belirli Abonelikleriniz ve kaynak türleri için öneriler görüntülemek için filtre uygulayabilirsiniz.  Öneriler dört kategoriye ayrılır: 

* **Yüksek kullanılabilirlik**: emin olun ve iş açısından kritik uygulamalarınızı sürekliliği geliştirin. Daha fazla bilgi için bkz: [Danışmanı yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md).
* **Güvenlik**: Tehditler ve güvenlik ihlallerini yol açabilecek güvenlik açıkları algılamak için. Daha fazla bilgi için bkz: [Danışmanı güvenlik önerileri](advisor-security-recommendations.md).
* **Performans**: uygulamalarınızın hızını artırmak için. Daha fazla bilgi için bkz: [Danışmanı performans önerileri](advisor-performance-recommendations.md).
* **Maliyet**: en iyi duruma getirme ve genel Azure harcama azaltmak için. Daha fazla bilgi için bkz: [Danışmanı maliyet önerileri](advisor-cost-recommendations.md).

  ![Advisor öneri türleri](./media/advisor-overview/advisor-dashboard.png)

> [!NOTE]
> Abonelik, bir abonelik Azure Danışmanı'nı kullanmaya *sahibi* Danışmanı panosunu başlatma gerekir.  Bu eylem abonelik Danışmanı ile kaydeder.  Üzerinde herhangi bir abonelik o noktadan itibaren *sahibi*, *katkıda bulunan*, veya *okuyucu* abonelik Danışmanı önerileri erişebilirsiniz. 

Bu kategoride öneriler listesini görüntülemek için bir kategoriye tıklayın ve bir öneri hakkında daha fazla bilgi edinmek için seçin.  Ayrıca bir fırsat yararlanmak veya bir sorunu gidermek üzere gerçekleştirebileceğiniz eylemler hakkında bilgi edinebilirsiniz.

![Advisor öneri kategorisi](./media/advisor-overview/advisor-ha-category-example.png) 

Öneri uygulamak bir öneri için önerilen eylemi seçin.  Basit bir arabirim öneriyi uygulamayı veya uygulama ile yönetmenize yardımcı olan belgelerine başvurun olanak tanıyan açılır.  Bir öneri uygulamaya başladıktan sonra bunu, tanımak Advisor için bir güne kadar sürebilir.

Bir öneriye derhal eylemde istiyorsanız değil, belirli bir süre için erteleyin veya onu yok sayın.  Belirli abonelik veya kaynak grubu için öneriler almak istemiyorsanız, yalnızca belirtilen Abonelikleriniz ve kaynak gruplarınız için öneri oluşturmak amacıyla Danışmanı'nı yapılandırabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-access-advisor"></a>Advisor nasıl erişirim?
Advisor aracılığıyla erişebilirsiniz [Azure portal](https://aka.ms/azureadvisordashboard). Oturum [portal](https://portal.azure.com), bulun **Danışmanı** Gezinti Menüsü veya içinde arama **tüm hizmetleri** menüsü.

Sanal makine kaynak arabirimi aracılığıyla Advisor önerileri de görüntüleyebilirsiniz. Bir sanal makine seçin ve ardından menüde Danışmanı önerileri için gidin. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Advisor erişmek hangi izinlerin gerekiyor mu?

Bir abonelik Danışmanı önerileri almak için aboneliğinizi Advisor ile kaydetmeniz gerekir. Bir abonelik bir abonelik zaman kayıtlı *sahibi* Danışmanı Pano başlatır. Bu tek seferlik bir işlemdir. Abonelik kaydedildikten sonra Advisor önerileri olarak erişebilir *sahibi*, *katkıda bulunan*, veya *okuyucu* bir abonelik.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Hangi kaynaklara Danışmanı önerileri için sağlar?

Advisor sanal makineler, kullanılabilirlik kümeleri, uygulama ağ geçitleri, uygulama hizmetleri, SQL Server, SQL veritabanları ve Redis önbelleği için öneriler sağlar.

### <a name="can-i-postpone-or-dismiss-a-recommendation"></a>Erteleyin veya miyim öneri yok sayın?

Erteleyin veya bir öneri kapatmak için **Ertele'yi** bağlantı. Bir Ertele'yi dönem veya select belirtebilirsiniz **hiçbir zaman** öneri kapatılamadı.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:

* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Advisor maliyet önerileri](advisor-cost-recommendations.md)
