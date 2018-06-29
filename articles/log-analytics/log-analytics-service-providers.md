---
title: Günlük analizi özellikleri hizmet sağlayıcıları | Microsoft Docs
description: Günlük analizi yönetilen hizmet sağlayıcıları (MSP'ler), büyük kuruluşlar, bağımsız yazılım satıcıları (ISV) yardımcı olabilir ve barındırma hizmeti sağlayıcıları yönetebilir ve müşterinin şirket içi veya Bulut altyapı sunucularını izleyebilirsiniz.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: meirm
ms.openlocfilehash: 97e36c624e865010ada67f5163af6d7f03de079f
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096579"
---
# <a name="log-analytics-features-for-service-providers"></a>Hizmet sağlayıcıları için günlük analizi özellikleri
Günlük analizi, yönetilen hizmet sağlayıcıları (MSP'ler), büyük kuruluşlar, bağımsız yazılım satıcılarının (ISV'ler) ve barındırma hizmeti sağlayıcıları yönetmek ve müşterinin şirket içi veya Bulut altyapı sunucularını izlemek yardımcı olabilir. 

Büyük kuruluşlar özellikle yönetmekten sorumlu merkezi bir BT Ekibi olduğunda bu benzer şekilde hizmet sağlayıcıları ile paylaşmak için birçok farklı iş birimleri BT. Kolaylık olması için bu belgede terimini kullanır *hizmet sağlayıcısı* ancak aynı işlevselliği da kuruluşlar ve diğer müşteriler için kullanılabilir.

İş ortakları ve parçası olan hizmet sağlayıcıları için [bulut çözümü sağlayıcısı (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) günlük analizi programdır bulunan Azure hizmetlerinden birini [Azure CSP abonelik](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Hizmet sağlayıcıları için mimariler

Günlük analizi çalışma alanları yöneticinin yalıtım günlüklerinin ve akışını denetlemek ve belirli iş gereksinimlerini ele günlük mimarisi oluşturmak için bir yöntem sağlar. [Bu makalede](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-manage-access) çalışma yönetim geçici genel konular açıklanmaktadır. Hizmet sağlayıcıları dikkat edilecek diğer noktalar vardır.

Günlük analizi çalışma alanları ile ilgili hizmet sağlayıcıları için üç olası mimariler vardır:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Dağıtılmış - müşterinin Kiracı içinde yer alan çalışma alanlarını günlükleri depolanır 

Bu mimaride, bu müşterinin tüm günlükleri için kullanılan bir müşterinin Kiracı çalışma dağıtılır. Hizmet sağlayıcısı yöneticileri kullanarak bu çalışma alanı için erişim izni verilen [Azure Active Directory Konuk kullanıcılar (B2B)](https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b). Hizmet sağlayıcı Yöneticisi Azure Portalı'nda bu çalışma alanları erişebilmeleri için müşterinin dizinine geçin gerekecektir.

Bu mimarisinin avantajları şunlardır:
* Müşteri, kendi kullanarak günlüklerini erişimi yönetebilirsiniz [rol tabanlı erişim](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview).
* Her bir müşteri bekletme ve veri capping gibi kendi çalışma alanı için farklı ayarlara sahip olabilir.
* Müşteriler için arasında yalıtım yasal düzenleme ve uyumluluk.
* Müşteri'nin aboneliğinize her çalışma alanı için ücret alınacaktır.
* Günlükleri tüm kaynakları, değil yalnızca Aracısı tabanlı türlerinden toplanabilir. Örneğin, Azure denetleme.

Bu mimari dezavantajları şunlardır:
* Aynı anda çok sayıda müşteri kiracılar yönetmek hizmet sağlayıcısı için daha zordur.
* Hizmet sağlayıcısı yöneticilerin müşteri dizinde sağlanması gerekmez.
* Hizmet sağlayıcısı müşterilerine arasında verileri çözümlemek olamaz.

### <a name="2-central---logs-are-stored-in-workspace-located-in-the-service-provider-tenant"></a>2. Orta - günlükleri, servis sağlayıcı kiracısı'nda bulunan çalışma alanı depolanır

Bu mimaride günlükleri, Müşteri'nin kiracılar, ancak yalnızca bir merkezi konumda hizmet sağlayıcısının aboneliklerin biri içinde depolanmaz. Müşteri'nin Vm'lere yüklü aracıları çalışma alanı kimliği ve gizli anahtarı kullanarak bu çalışma alanına günlükleri göndermek için yapılandırılır.

Bu mimarisinin avantajları şunlardır:
* Müşteriler çok sayıda yönetmek ve çeşitli arka uç sistemleri tümleştirme kolaydır.
* Hizmet sağlayıcısı günlükleri ve işlevleri gibi çeşitli yapılarını üzerinde tam sahiplik sahiptir ve sorguları kaydedilir.
* Hizmet sağlayıcısı analytics tüm müşterilerin gerçekleştirebilirsiniz.

Bu mimari dezavantajları şunlardır:
* Müşterileri arasında veri ayrı zor olacaktır. Bunu yapmak için en iyi yöntem, bilgisayarın etki alanı adı kullanmaktır.
* Tüm müşterilerden gelen tüm verileri tek bir fatura ve aynı saklama ve yapılandırma ayarları ile aynı bölgede depolanır.
* Azure tanılama ve Azure denetim gerektiren aynı Kiracı kaynak olarak bu nedenle merkezi çalışma alanına günlükler gönderilemiyor olması için çalışma gibi hizmetleri Azure doku ve PaaS.

### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Karma - günlükleri, müşterinin Kiracı içinde yer alan çalışma alanında depolanır ve bazıları merkezi bir konuma alınır.

İki seçenekten üçüncü mimarisi karışımı. Günlükleri olduğu her müşteri için yerel ilk dağıtılmış mimarisi dayanır, ancak günlükler merkezi bir havuz oluşturmak için bazı mekanizmasını kullanarak. Günlükleri bir kısmı, raporlama ve analiz için merkezi bir konuma alınır. Bu bölümü, az sayıda veri türleri veya günlük istatistiği gibi etkinliğinin özetini olabilir.

Günlük analizi merkezi bir konum uygulamak için iki seçenek vardır:

1. Merkezi çalışma: hizmet sağlayıcısı kendi Kiracı çalışma alanı oluşturma ve yararlanan bir komut dosyası kullanarak [sorgu API](https://dev.loganalytics.io/) ile [veri koleksiyonu API](log-analytics-data-collector-api.md) verileri çeşitli çalışma alanlarından için getirmek için Merkezi bir konum. Komut dosyası dışında başka bir seçenek kullanmaktır [Azure mantıksal uygulama](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview).

2. Merkezi bir konum olarak Powerbı: çeşitli çalışma alanları için günlük analizi arasındaki tümleştirmeyi kullanarak veri verdiğinizde, Power BI merkezi konumu olarak görev yapabilir ve [Power BI](log-analytics-powerbi.md). 


## <a name="next-steps"></a>Sonraki Adımlar
* Oluşturma ve kullanarak çalışma yapılandırılmasını otomatikleştirmek [Resource Manager şablonları](log-analytics-template-workspace-configuration.md)
* Kullanarak çalışma oluşturulmasını otomatik hale [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Kullanım [uyarıları](log-analytics-alerts.md) mevcut sistemlerle tümleştirmek için
* Özet raporları kullanarak oluşturmak [Power BI](log-analytics-powerbi.md)

