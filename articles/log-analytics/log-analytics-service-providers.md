---
title: "Günlük analizi özellikleri hizmet sağlayıcıları | Microsoft Docs"
description: "Günlük analizi yönetilen hizmet sağlayıcıları (MSP'ler), büyük kuruluşlar, bağımsız yazılım satıcıları (ISV) yardımcı olabilir ve barındırma hizmeti sağlayıcıları yönetebilir ve müşterinin şirket içi veya Bulut altyapı sunucularını izleyebilirsiniz."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: richrund
ms.openlocfilehash: 8a67d9a9d345682e9e6c8f5c7779204a038f5f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="log-analytics-features-for-service-providers"></a>Hizmet sağlayıcıları için günlük analizi özellikleri
Günlük analizi, yönetilen hizmet sağlayıcıları (MSP'ler), büyük kuruluşlar, bağımsız yazılım satıcılarının (ISV'ler) ve barındırma hizmeti sağlayıcıları yönetmek ve müşterinin şirket içi veya Bulut altyapı sunucularını izlemek yardımcı olabilir. 

Büyük kuruluşlar özellikle yönetmekten sorumlu merkezi bir BT Ekibi olduğunda bu benzer şekilde hizmet sağlayıcıları ile paylaşmak için birçok farklı iş birimleri BT. Kolaylık olması için bu belgede terimini kullanır *hizmet sağlayıcısı* ancak aynı işlevselliği da kuruluşlar ve diğer müşteriler için kullanılabilir.

## <a name="cloud-solution-provider"></a>Bulut Çözümü Sağlayıcısı
İş ortakları ve parçası olan hizmet sağlayıcıları için [bulut çözümü sağlayıcısı (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) program, günlük analizi Azure hizmetlerini bir CSP abonelikte kullanılabilir biridir. 

Aşağıdaki özellikleri günlük analizi için etkinleştirilmiş olan *bulut çözümü sağlayıcısı* abonelikleri.

Farklı bir *bulut çözümü sağlayıcısı* şunları yapabilirsiniz:

* Günlük analizi çalışma alanları (müşterinin adına) Kiracı aboneliği oluşturun.
* Erişim çalışma alanları kiracılar tarafından oluşturuldu. 
* Ekleyin ve kullanıcı erişimini Azure kullanıcı Yönetimi'ni kullanarak çalışma alanından kaldırın. Bir kiracının çalışma OMS portalında kullanıcı yönetimi altında sayfası açıldığında ayarları kullanılabilir değil
  * Rol tabanlı erişim henüz - kullanıcı vermiş günlük analizi desteklemiyor `reader` izin Azure portalında, OMS Portalı'nda yapılandırma değişiklikleri yapmalarına izin verir

Bir kiracının aboneliğine oturum açmak için Kiracı tanımlayıcı belirtmeniz gerekir. Kiracı sık sık oturum açmak için kullandığınız e-posta adresi son kısmı tanımlayıcısıdır.

* OMS portalında ekleme `?tenant=contoso.com` portalı için URL. Örneğin, `mms.microsoft.com/?tenant=contoso.com`
* PowerShell'de kullanın `-Tenant contoso.com` kullanırken parametresi `Add-AzureRmAccount` cmdlet'i
* Kullandığınızda Kiracı tanımlayıcısı otomatik olarak eklenen `OMS portal` açmak ve seçilen çalışma alanı için OMS portalı oturum açmak için Azure portalından bağlantı

Farklı bir *müşteri* bir bulut çözümü sağlayıcısı şunları yapabilirsiniz:

* Günlük analizi çalışma alanları bir CSP abonelikte oluşturun.
* CSP tarafından oluşturulan erişim çalışma alanları
  * Kullanım `OMS portal` açmak ve seçilen çalışma alanı için OMS portalı oturum açmak için Azure portalından bağlantı
* Görüntüleme ve kullanıcı yönetimi sayfasına ayarlar altında OMS portalında kullanma

> [!NOTE]
> Günlük analizi dahil yedekleme ve Site kurtarma çözümleri bir kurtarma Hizmetleri Kasası'na bağlanabilmelidir değildir ve bir CSP aboneliğindeki yapılandırılamıyor. 
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a>Günlük analizi kullanarak birden çok müşterileri yönetme
Yönettiğiniz her müşteri için günlük analizi çalışma alanı oluşturma önerilir. Günlük analizi çalışma alanı sağlar:

* Verilerin depolanması bir coğrafi konumu. 
* Fatura için ayrıntı düzeyi 
* Veri yalıtımı 
* Benzersiz yapılandırma

Bir çalışma alanı Müşteri başına oluşturarak, her müşterinin veri ayrı tutmak ve ayrıca her bir müşteri kullanımını izlemek kullanabilirsiniz.

Ne zaman ve neden birden çok çalışma alanı oluşturma hakkında daha fazla ayrıntı açıklanmaktadır [analytics oturum erişimini yönetme](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Oluşturma ve yapılandırma müşteri çalışma alanlarının otomatik hale getirilebilir kullanarak [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager şablonları](log-analytics-template-workspace-configuration.md), veya kullanarak [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Resource Manager şablonları kullanımı çalışma yapılandırması oluşturma ve çalışma alanları yapılandırmak için kullanılan bir ana yapılandırma sahip olmanızı sağlar. Müşteriler için çalışma alanları oluşturuldukça bunlar otomatik olarak gereksinimlerinizi için yapılandırıldığından emin olabilirsiniz. Gereksinimlerinizi güncelleştirdiğinizde, şablon güncelleştirilir ve varolan çalışma alanları yeniden. Bu işlem, hatta varolan çalışma yeni standartlarınızı karşılamak sağlar.    

Birden çok günlük analizi çalışma yönetirken, her çalışma alanında varolan gişe sistemiyle tümleştirme öneririz / işletim konsolunu kullanarak [uyarıları](log-analytics-alerts.md) işlevselliği. Varolan sistemlerinizi ile tümleştirerek, destek personeli tanıdık süreçlerinin izlemek devam edebilirsiniz. Günlük analizi düzenli olarak her çalışma alanında, belirttiğiniz Uyarı ölçütleri karşı denetler ve eylem gerekli olduğunda bir uyarı oluşturur.

Veri kişiselleştirilmiş görünümleri kullanma [Pano](../azure-portal/azure-portal-dashboards.md) Azure portalında yeteneği.  

İçin yönetici düzeyi raporları, özetleme verileri çalışma alanları arasında günlük analizi arasında tümleştirme kullanabilirsiniz ve [Powerbı](log-analytics-powerbi.md). Başka bir raporlama sistemi ile tümleştirme gerekiyorsa, arama API kullanabilirsiniz (PowerShell aracılığıyla veya [REST](log-analytics-log-search-api.md)) sorguları çalıştırmak ve arama sonuçlarını dışarı aktarma.

## <a name="next-steps"></a>Sonraki Adımlar
* Oluşturma ve kullanarak çalışma yapılandırılmasını otomatikleştirmek [Resource Manager şablonları](log-analytics-template-workspace-configuration.md)
* Kullanarak çalışma oluşturulmasını otomatik hale [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Kullanım [uyarıları](log-analytics-alerts.md) mevcut sistemlerle tümleştirmek için
* Özet raporları kullanarak oluşturmak [Powerbı](log-analytics-powerbi.md)

