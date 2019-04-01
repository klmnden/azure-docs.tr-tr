---
title: Hizmet sağlayıcıları için Azure İzleyici | Microsoft Docs
description: Azure İzleyici, yönetilen hizmet sağlayıcılarına (msp), büyük kuruluşlar, bağımsız yazılım satıcılarına (ISV) yardımcı olabilir ve barındırma hizmeti sağlayıcılarına müşterinin şirket içi veya Bulut altyapı sunucularını izleme ve yönetme.
services: log-analytics
documentationcenter: ''
author: MeirMen
manager: jochan
editor: ''
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: meirm
ms.openlocfilehash: 97d8d6fac93ebabac8fb319ce2f1ab8719f5f86b
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756576"
---
# <a name="azure-monitor-for-service-providers"></a>Hizmet sağlayıcıları için Azure İzleyici
Log Analytics çalışma alanları Azure İzleyici'de, yönetilen hizmet sağlayıcılarına (msp), büyük kuruluşlar, bağımsız yazılım satıcılarına (ISV) ve müşterinin şirket içi veya Bulut altyapı sunucularını izleme ve yönetme barındırma hizmeti sağlayıcılarına yardımcı olabilir. 

Büyük kuruluşlar özellikle yönetmekten sorumlu merkezi bir BT ekibiniz olduğunda bu benzer hizmet sağlayıcıları ile paylaşmak için birçok farklı iş birimleri BT. Kolaylık olması için bu belgede terimini kullanır. *hizmet sağlayıcısı* ancak aynı işlevselliği de kuruluşlar ve diğer müşteriler için kullanılabilir.

İş ortakları ve parçası olan hizmet sağlayıcıları için [bulut çözümü sağlayıcısı (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programı, Log Analytics, Azure İzleyici'de bulunan Azure Hizmetleri biridir [Azure CSP aboneliklerinde](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Hizmet sağlayıcılarına yönelik mimariler

Log Analytics çalışma alanları, akış ve günlüklerin yalıtım denetlemek ve belirli iş gereksinimlerini ele alan bir günlük mimarisi oluşturma yöneticiye bir yöntem sağlar. [Bu makalede](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) geçici çalışma alanı yönetimi genel konular açıklanmaktadır. Hizmet sağlayıcıları, ek hususlar vardır.

Log Analytics çalışma alanları ile ilgili hizmet sağlayıcıları için üç olası mimari vardır:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Dağıtılmış - müşteri kiracısında bulunan çalışma alanları günlükleri depolanır 

Bu mimaride, bir çalışma alanı söz konusu müşterinin tüm günlükleri için kullanılan bir müşterinin Kiracı dağıtılır. Hizmet sağlayıcısı yöneticileri kullanarak bu çalışma alanı için erişim izni verilen [Azure Active Directory'ye Konuk kullanıcılar (B2B)](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b). Hizmet sağlayıcısı yöneticileri, bu çalışma alanlarının erişebilmesi için Azure portalında, müşterinin dizinine geçin gerekecektir.

Bu mimari avantajları şunlardır:
* Müşteri, kendi kullanarak günlükleri erişimi yönetebilir [rol tabanlı erişim](https://docs.microsoft.com/azure/role-based-access-control/overview).
* Her müşteri için kendi çalışma alanı tutma ve veri capping gibi farklı ayarlara sahip olabilir.
* Müşteriler için arasında yalıtım yasal ve uymalarını zorunlu tutar.
* Müşterinin hizmet aboneliği içinde her çalışma alanı için ücret alınacaktır.
* Tüm kaynaklar, değil yalnızca aracı tabanlı türlerinden günlükleri toplanabilir. Örneğin, Azure denetim günlükleri.

Bu mimari dezavantajları şunlardır:
* Tek seferde çok sayıda müşteri kiracılar yönetmek hizmet sağlayıcısı için güçtür.
* Hizmet sağlayıcısı yöneticileri müşteri dizinde sağlanması gerekir.
* Hizmet sağlayıcısı müşterilerine arasında verilerini analiz edemiyoruz.

### <a name="2-central---logs-are-stored-in-a-workspace-located-in-the-service-provider-tenant"></a>2. Orta - günlükleri, hizmet sağlayıcısı kiracısında bulunan bir çalışma alanında depolanır

Bu mimaride, günlükler, müşterinin kiracılar, ancak yalnızca bir merkezi konumda hizmet sağlayıcısının aboneliklerden biri içinde depolanmaz. Müşterinin Vm'lere yüklü aracıları çalışma alanı kimliği ve gizli anahtarı kullanarak bu çalışma alanına kendi günlükleri göndermek için yapılandırılır.

Bu mimari avantajları şunlardır:
* Çok sayıda müşteriler yönetmek ve bunları için çeşitli arka uç sistemleri tümleştirmek kolay bir işlemdir.
* Hizmet sağlayıcısı, günlükler ve işlevleri gibi çeşitli yapıları üzerinde tam sahiplik sahiptir ve sorguları kaydedilir.
* Hizmet sağlayıcısı tüm müşterilerine analiz gerçekleştirebilirsiniz.

Bu mimari dezavantajları şunlardır:
* Bu mimari yalnızca aracı tabanlı VM verileri için geçerlidir, PaaS, SaaS ve Azure fabric veri kaynakları kapsayan değil.
* Tek bir çalışma alanına birleştirildiğinde müşteriler arasındaki verileri ayrı zor olabilir. Bunu yapmak için en iyi yöntem bilgisayarın tam etki alanı adı (FQDN) kullanmak veya Azure aboneliği kimliği kullanmaktır 
* Tüm müşterilerden gelen tüm veriler tek bir fatura ve aynı saklama ve yapılandırma ayarları ile aynı bölgede depolanır.
* Azure yapısı ve PaaS Hizmetleri kaynak ile aynı kiracıda bu nedenle yönetim çalışma alanına günlükler gönderilemiyor olması için çalışma gibi Azure tanılama ve Azure denetim günlükleri gerektirir.
* Tüm müşterilerden gelen tüm VM aracıları aynı çalışma alanı kimliği ve anahtarı kullanarak merkezi çalışma alanına doğrulanır. Belirli bir müşteri günlüklerinden diğer müşterilerin kesintiye uğratmadan engellemek için bir yöntem yoktur.


### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Karma - günlükleri müşteri kiracısında bulunan çalışma alanında depolanır ve bazıları merkezi bir konuma yeniden çekilir.

İki seçenek arasındaki üçüncü mimarisi karışımı. Günlükleri nerededir her müşteri için yerel ilk dağıtılmış mimarisi dayanır ancak günlüklerinin merkezi bir depo oluşturmak için bazı mekanizmasını kullanma. Günlükleri bir kısmı, raporlama ve analiz için merkezi bir konuma çekilir. Bu bölümü, az sayıda veri türleri veya günlük istatistikler gibi etkinliğinin özetini olabilir.

Merkezi bir konumda günlükler uygulamak için iki seçenek vardır:

1. Merkezi çalışma alanı: Hizmet sağlayıcısı, kiracıda bir çalışma alanı oluşturun ve yararlanan bir betik kullan [sorgu API'si](https://dev.loganalytics.io/) ile [veri koleksiyonu API'sini](../../azure-monitor/platform/data-collector-api.md) bu merkezi bir konuma çeşitli çalışma alanlarından verileri getirmek için. Bir betik dışındaki başka bir seçenek kullanmaktır [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).

2. Power BI olarak merkezi bir konum: Çeşitli çalışma alanları için Log Analytics çalışma alanı arasında tümleştirmeyi kullanarak verileri dışarı aktardığınızda, Power BI merkezi konumunuz olarak hareket edebilir ve [Power BI](../../azure-monitor/platform/powerbi.md). 


## <a name="next-steps"></a>Sonraki Adımlar
* Oluşturma ve kullanarak çalışma yapılandırılmasını otomatikleştirmek [Resource Manager şablonları](template-workspace-configuration.md)
* Çalışma alanlarını kullanarak oluşturulmasını otomatikleştirin [PowerShell](../../azure-monitor/platform/powershell-workspace-configuration.md) 
* Kullanım [uyarılar](../../azure-monitor/platform/alerts-overview.md) var olan sistemlerle tümleştirmek için
* Özet raporları kullanarak oluşturmak [Power BI](../../azure-monitor/platform/powerbi.md)
* Gözden geçirme sürecini [birden çok CSP müşterileri izlemek için Log Analytics ve Power BI'ı yapılandırma](https://docs.microsoft.com/azure/cloud-solution-provider/support/monitor-multiple-customers)
