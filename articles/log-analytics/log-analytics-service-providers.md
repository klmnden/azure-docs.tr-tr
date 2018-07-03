---
title: Log Analytics için hizmet sağlayıcıları | Microsoft Docs
description: Log Analytics, yönetilen hizmet sağlayıcılarına (msp), büyük kuruluşlar, bağımsız yazılım satıcılarına (ISV) yardımcı olabilir ve barındırma hizmeti sağlayıcılarına müşterinin şirket içi veya Bulut altyapı sunucularını izleme ve yönetme.
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
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: meirm
ms.component: na
ms.openlocfilehash: 13f36f67e76b75176940a0f36121be30ba27d519
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37340874"
---
# <a name="log-analytics-features-for-service-providers"></a>Hizmet sağlayıcıları için log Analytics özellikleri
Log Analytics, yönetilen hizmet sağlayıcılarına (msp), büyük kuruluşlar, bağımsız yazılım satıcılarına (ISV) ve müşterinin şirket içi veya Bulut altyapı sunucularını izleme ve yönetme barındırma hizmeti sağlayıcılarına yardımcı olabilir. 

Büyük kuruluşlar özellikle yönetmekten sorumlu merkezi bir BT ekibiniz olduğunda bu benzer hizmet sağlayıcıları ile paylaşmak için birçok farklı iş birimleri BT. Kolaylık olması için bu belgede terimini kullanır. *hizmet sağlayıcısı* ancak aynı işlevselliği de kuruluşlar ve diğer müşteriler için kullanılabilir.

İş ortakları ve parçası olan hizmet sağlayıcıları için [bulut çözümü sağlayıcısı (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programı, Log Analytics kullanılabilir Azure Hizmetleri biridir [Azure CSP aboneliği](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Hizmet sağlayıcılarına yönelik mimariler

Log Analytics çalışma alanları, akış ve günlüklerin yalıtım denetlemek ve belirli iş gereksinimlerini ele alan bir günlük mimarisi oluşturma yöneticiye bir yöntem sağlar. [Bu makalede](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-manage-access) geçici çalışma alanı yönetimi genel konular açıklanmaktadır. Hizmet sağlayıcıları, ek hususlar vardır.

Log Analytics çalışma alanları ile ilgili hizmet sağlayıcıları için üç olası mimari vardır:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Dağıtılmış - müşteri kiracısında bulunan çalışma alanları günlükleri depolanır 

Bu mimaride, çalışma alanı söz konusu müşterinin tüm günlükleri için kullanılan bir müşterinin Kiracı dağıtılır. Hizmet sağlayıcısı yöneticileri kullanarak bu çalışma alanı için erişim izni verilen [Azure Active Directory'ye Konuk kullanıcılar (B2B)](https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b). Hizmet sağlayıcı Yöneticisi Azure Portalı'nda bu çalışma alanlarının erişebilmesi için müşterinin dizinine geçin gerekecektir.

Bu mimari avantajları şunlardır:
* Müşteri, kendi kullanarak günlükleri erişimi yönetebilir [rol tabanlı erişim](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview).
* Her müşteri için kendi çalışma alanı tutma ve veri capping gibi farklı ayarlara sahip olabilir.
* Müşteriler için arasında yalıtım yasal ve uymalarını zorunlu tutar.
* Müşterinin hizmet aboneliği içinde her çalışma alanı için ücret alınacaktır.
* Tüm kaynaklar, değil yalnızca aracı tabanlı türlerinden günlükleri toplanabilir. Örneğin, Azure denetim.

Bu mimari dezavantajları şunlardır:
* Tek seferde çok sayıda müşteri kiracılar yönetmek hizmet sağlayıcısı için güçtür.
* Hizmet sağlayıcısı yöneticileri müşteri dizinde sağlanması gerekir.
* Hizmet sağlayıcısı müşterilerine arasında verilerini analiz edemiyoruz.

### <a name="2-central---logs-are-stored-in-workspace-located-in-the-service-provider-tenant"></a>2. Orta - günlükleri, hizmet sağlayıcısı kiracısında bulunan çalışma alanında depolanır

Bu mimaride, günlükler, müşterinin kiracılar, ancak yalnızca bir merkezi konumda hizmet sağlayıcısının aboneliklerden biri içinde depolanmaz. Müşterinin Vm'lere yüklü aracıları çalışma alanı kimliği ve gizli anahtarı kullanarak bu çalışma alanına kendi günlükleri göndermek için yapılandırılır.

Bu mimari avantajları şunlardır:
* Çok sayıda müşteri yönetmek ve bunları için çeşitli arka uç sistemleri tümleştirmek kolay bir işlemdir.
* Hizmet sağlayıcısı, günlükler ve işlevleri gibi çeşitli yapıları üzerinde tam sahiplik sahiptir ve sorguları kaydedilir.
* Hizmet sağlayıcısı tüm müşteriler arasında analiz gerçekleştirebilirsiniz.

Bu mimari dezavantajları şunlardır:
* Müşteriler arasındaki verileri ayrı zor olacaktır. Bunu yapmak için en iyi yöntem, bilgisayarın etki alanı adı kullanmaktır.
* Tüm müşterilerden gelen tüm veriler tek bir fatura ve aynı saklama ve yapılandırma ayarları ile aynı bölgede depolanır.
* Azure yapısı ve PaaS Hizmetleri kaynakla aynı kiracıda bu nedenle yönetim çalışma alanına günlükler gönderilemiyor olması için çalışma alanı gibi Azure tanılama ve Azure denetim gerektirir.
* Tüm müşterilerden gelen tüm VM aracıları aynı çalışma alanı kimliği ve anahtarı kullanarak cental çalışma alanına doğrulanır. Belirli bir müşteri günlüklerinden diğer müşterilerin kesintiye uğratmadan engellemek için bir yöntem yoktur.


### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Karma - günlükleri müşteri kiracısında bulunan çalışma alanında depolanır ve bazıları merkezi bir konuma yeniden çekilir.

İki seçenek arasındaki üçüncü mimarisi karışımı. Günlükleri nerededir her müşteri için yerel ilk dağıtılmış mimarisi dayanır ancak günlüklerinin merkezi bir depo oluşturmak için bazı mekanizmasını kullanma. Günlükleri bir kısmı, raporlama ve analiz için merkezi bir konuma çekilir. Bu bölümü, az sayıda veri türleri veya günlük istatistiği gibi etkinliğinin özetini olabilir.

Log Analytics'te merkezi bir konum uygulamak için iki seçenek vardır:

1. Merkezi çalışma alanı: hizmet sağlayıcısı, kiracıda bir çalışma alanı oluşturun ve yararlanan bir betik kullan [sorgu API'si](https://dev.loganalytics.io/) ile [veri koleksiyonu API'sini](log-analytics-data-collector-api.md) için çeşitli çalışma alanlarından verileri getirmek için Merkezi bir konum. Betik dışındaki başka bir seçenek kullanmaktır [Azure Logic App](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview).

2. Power BI olarak merkezi bir konum: çeşitli çalışma alanları için Log Analytics arasındaki tümleştirmeden kullanarak verileri dışarı aktardığınızda, Power BI merkezi konumunuz olarak hareket edebilir ve [Power BI](log-analytics-powerbi.md). 


## <a name="next-steps"></a>Sonraki Adımlar
* Oluşturma ve kullanarak çalışma yapılandırılmasını otomatikleştirmek [Resource Manager şablonları](log-analytics-template-workspace-configuration.md)
* Çalışma alanlarını kullanarak oluşturulmasını otomatikleştirin [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Kullanım [uyarılar](log-analytics-alerts.md) var olan sistemlerle tümleştirmek için
* Özet raporları kullanarak oluşturmak [Power BI](log-analytics-powerbi.md)

