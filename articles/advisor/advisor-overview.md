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
ms.openlocfilehash: 96925f251cf4984a11516a962740e19a7b9589dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-advisor"></a>Azure Danışmanı giriş

Azure Danışmanı ve temel işlevleri hakkında bilgi edinin ve sık sorulan soruların yanıtlarını alın.

## <a name="what-is-advisor"></a>Advisor nedir?
Advisor Azure dağıtımlarınızı iyileştirmek için en iyi uygulamaları izleyerek yardımcı olan kişiselleştirilmiş bulut Danışman ' dir. Kaynak yapılandırması ve kullanım telemetri analiz eder ve maliyet verimliliği, performans, yüksek kullanılabilirlik ve Azure kaynaklarınızın güvenliğini geliştirmenize yardımcı olabilir çözümleri önerir.

Advisor ile şunları yapabilirsiniz:
* Get öngörülü, işlem yapılabilir ve öneriler kişiselleştirilmiş en iyi yöntemler. 
* Genel Azure azaltmak için fırsatlarını tanımlayabilmeniz gibi performans, güvenlik ve kaynaklarınızın yüksek kullanılabilirliğini artırmak ayırın.
* Önerilen eylemleri satır içi ile ilgili öneriler alın.

Advisor aracılığıyla erişebilirsiniz [Azure portal](https://aka.ms/azureadvisordashboard). Oturum [portal](https://portal.azure.com)seçin **Gözat**ve ardından kaydırın **Azure Danışmanı**. Advisor Pano seçili abonelik için kişiselleştirilmiş önerileri görüntüler. 

Öneriler dört kategoriye ayrılır: 

* **Yüksek kullanılabilirlik**: emin olun ve iş açısından kritik uygulamalarınızı sürekliliği geliştirin. Daha fazla bilgi için bkz: [Danışmanı yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md).

* **Güvenlik**: Tehditler ve güvenlik ihlallerini yol açabilecek güvenlik açıkları algılamak için. Daha fazla bilgi için bkz: [Danışmanı güvenlik önerileri](advisor-security-recommendations.md).

* **Performans**: uygulamalarınızın hızını artırmak için. Daha fazla bilgi için bkz: [Danışmanı performans önerileri](advisor-performance-recommendations.md).

* **Maliyet**: en iyi duruma getirme ve azaltmak için genel Azure ayırın. Daha fazla bilgi için bkz: [Danışmanı maliyet önerileri](advisor-cost-recommendations.md).

  ![Advisor öneri türleri](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> Advisor önerileri erişmek için öncelikle *aboneliğinizi kaydetmek* Danışmanı ile. Bir abonelik kayıtlı olduğunda bir *abonelik sahibi* Danışmanı Pano başlatır ve tıkladığında **alma önerileri** düğmesi. Bu bir *tek seferlik işlem*. Abonelik kaydedildikten sonra Advisor önerileri olarak erişebilir *sahibi*, *katkıda bulunan*, veya *okuyucu* bir abonelik, bir kaynak grubu ya da belirli bir kaynak için.

Hakkında daha fazla bilgi için bir öneriye tıklayabilirsiniz. Ayrıca bir fırsat yararlanmak veya bir sorunu gidermek üzere gerçekleştirebileceğiniz eylemler hakkında bilgi edinebilirsiniz. 

Advisor satır içi eylemler veya belgelere bağlantılar ile ilgili öneriler sunar. Satır içi eylemi tıklatarak alır, "Destekli kullanıcı gezisine" uygulamak için. Bir belge bağlantıyı tıklatmak eylemi el ile uygulanacak açıklar belgelerine işaret eder. 

Advisor önerileri saatlik güncelleştirir. Bir öneriye derhal eylemde istemiyorsanız, belirli bir süre için uykuda ya da onu yok sayın. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-access-advisor"></a>Advisor nasıl erişirim?
Advisor aracılığıyla erişebilirsiniz [Azure portal](https://aka.ms/azureadvisordashboard). Oturum [portal](https://portal.azure.com)seçin **Gözat**ve ardından kaydırın **Azure Danışmanı**. Advisor Pano seçili abonelik için kişiselleştirilmiş önerileri görüntüler. 

Sanal makine kaynak dikey Advisor önerileri de görüntüleyebilirsiniz. Bir sanal makine seçin ve ardından menüde Danışmanı önerileri için gidin. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Advisor erişmek hangi izinlerin gerekiyor mu?

Advisor önerileri erişmek için öncelikle *aboneliğinizi kaydetmek* Danışmanı ile. Bir abonelik kayıtlı olduğunda bir *abonelik sahibi* Danışmanı Pano başlatır ve tıkladığında **alma önerileri** düğmesi. Bu bir *tek seferlik işlem*. Abonelik kaydedildikten sonra Advisor önerileri olarak erişebilir *sahibi*, *katkıda bulunan*, veya *okuyucu* bir abonelik, bir kaynak grubu ya da belirli bir kaynak için.

### <a name="how-often-are-advisor-recommendations-updated"></a>Ne sıklıkta güncelleştirilmiş Advisor önerileri misiniz?

Advisor önerileri saatlik güncelleştirilir.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Hangi kaynaklara Danışmanı önerileri için sağlar?

Advisor sanal makineler, kullanılabilirlik kümeleri, uygulama ağ geçitleri, uygulama hizmetleri, SQL Server, SQL veritabanları ve Redis önbelleği için öneriler sağlar.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Uykuda veya miyim öneri yok sayın?

Uykuda veya bir öneri kapatmak için **uykuda** düğmesini veya bağlantısını. Erteleme saati dönem veya select belirtebilirsiniz **hiçbir zaman** öneri kapatılamadı.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:

* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Advisor maliyet önerileri](advisor-cost-recommendations.md)
