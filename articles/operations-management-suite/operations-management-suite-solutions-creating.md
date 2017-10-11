---
title: "OMS Yönetimi çözümünde yapı | Microsoft Docs"
description: "Yönetim çözümleri, müşterilerin kendi OMS çalışma alanına ekleyebilirsiniz paketlenmiş yönetim senaryoları sağlayarak Operations Management Suite (OMS) işlevselliği genişletir.  Bu makalede, kendi ortamınızda kullanılacak yönetim çözümleri nasıl oluşturabileceğinizi hakkında ayrıntılar sağlar veya müşterileriniz için kullanılabilir."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ea959f1e95ea46c07eec3afa9d46bfeb72ca3e4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="design-and-build-a-management-solution-in-operations-management-suite-oms-preview"></a>Tasarlama ve bir yönetim çözümü Operations Management Suite (OMS) (Önizleme) içinde oluşturma
> [!NOTE]
> Bu, şu anda önizlemede OMS yönetim çözümleri oluşturmak için başlangıç belgesidir. Aşağıda açıklanan herhangi bir şema değiştirilebilir ' dir.

[Yönetim çözümleri](operations-management-suite-solutions.md) müşteriler kendi OMS çalışma alanına ekleyebilirsiniz paketlenmiş yönetim senaryoları sağlayarak Operations Management Suite (OMS) işlevselliğini genişleten.  Bu makalede, tasarım ve en yaygın gereksinimleri için uygun bir yönetim çözümü oluşturmak için temel işlemi sunar.  Bu işlemi başlangıç noktası olarak kullanın ve gereksinimlerinizi geliştikçe daha karmaşık çözümleri için kavramlar yararlanan yönetim çözümleri oluşturmak için yeni varsa.

## <a name="what-is-a-management-solution"></a>Bir yönetim çözümü nedir?

Yönetim çözümleri OMS ve belirli bir izleme senaryo elde etmek için birlikte çalışan Azure kaynaklarını içerir.  Olarak uygulanan [kaynak Yönetim Şablonları](../azure-resource-manager/resource-manager-template-walkthrough.md) yüklemek ve çözüm yüklendiğinde kapsanan kaynaklarını yapılandırma ayrıntılarını içerir.

Temel stratejisi, Azure ortamınızda bileşenleri tek tek oluşturarak Yönetimi çözümünüzü başlatmaktır.  Düzgün çalışmasını işlevselliği olduktan sonra sonra bunları paketleme başlatabilirsiniz bir [yönetim çözümü dosyası](operations-management-suite-solutions-solution-file.md). 


## <a name="design-your-solution"></a>Çözümünüzü tasarlayın
En yaygın düzeni bir yönetim çözümü için aşağıdaki çizimde gösterilmiştir.  Bu modelde farklı bileşenleri ele alınmıştır aşağıda.

![OMS çözümüne genel bakış](media/operations-management-suite-solutions/solution-overview.png)


### <a name="data-sources"></a>Veri kaynakları
Çözümünü tasarlamanın ilk adımı, günlük analizi depodan gerektiren veri belirliyor.  Bu veri topladığı bir [veri kaynağı](../log-analytics/log-analytics-data-sources.md) veya [başka bir çözüm](operations-management-suite-solutions.md), ya da çözümünüzü toplamak için işlem sağlamanız gerekebilir.

Günlük analizi depoya açıklandığı gibi toplanan veri kaynağı, çeşitli yollarla yok [günlük analizi veri kaynaklarında](../log-analytics/log-analytics-data-sources.md).  Bu olaylar Windows Olay Günlüğü'nde içerir veya Syslog tarafından performans sayaçları yanı sıra Windows ve Linux istemcileri için oluşturulan.  Azure İzleyici tarafından toplanan Azure kaynaklarından verilerini de toplayabilirsiniz.  

Herhangi bir kullanılabilir veri kaynakları erişilebilir değil veri gerektiren sonra kullanabileceğiniz [HTTP veri toplayıcı API](../log-analytics/log-analytics-data-collector-api.md) veri günlük analizi depoya bir REST API'si çağırabilirsiniz herhangi bir istemciden yazma sağlar.  En yaygın bir yönetim çözümüne özel veri koleksiyonunun oluşturmak için yöntemdir bir [Azure Automation runbook](../automation/automation-runbook-types.md) , gerekli verileri Azure ya da dış kaynaklardan toplar ve yazmak için veri toplayıcı API'sini kullanır Depo.  

### <a name="log-searches"></a>Günlük aramalar
[Oturum aramaları](../log-analytics/log-analytics-log-searches.md) ayıklayın ve günlük analizi deposundaki verileri çözümlemek için kullanılır.  Bunlar, görünümler ve geçici veri deposunda çözümlemesi arkasından ek olarak uyarıları tarafından kullanılır.  

Herhangi bir görünüm veya Uyarıları tarafından kullanılmayan olsa bile kullanıcıya yardımcı olacağını düşündüğünüz herhangi bir sorgu tanımlamanız gerekir.  Bu Portalı'nda kayıtlı aramaları kendileri için hazır olur ve bunları da içerebilir bir [listesini, sorgular görselleştirme bölümü](../log-analytics/log-analytics-view-designer-parts.md#list-of-queries-part) özel görünümünüzde.

### <a name="alerts"></a>Uyarılar
[Günlük analizi uyarılarını](../log-analytics/log-analytics-alerts.md) aracılığıyla sorunları belirlemek [oturum aramaları](#log-searches) veri deposunda karşı.  Bunlar kullanıcıya bildir ya da otomatik olarak bir eylem yanıt olarak çalıştırın. Uygulamanız için farklı uyarı koşulları tanımlamak ve çözüm dosyanızdaki karşılık gelen uyarı kuralları içerir.

Sorun büyük olasılıkla otomatik bir işlemle düzeltilebilir varsa, genellikle bir runbook bu düzeltmenin Azure Otomasyonu'nda oluşturacaksınız.  Çoğu Azure Hizmetleri ile yönetilebilir [cmdlet'leri](/powershell/azure/overview) , runbook gibi işlevleri gerçekleştirmek için yararlanan.

Dış işlev yanıt olarak bir uyarı çözümünüzü gerektirir sonra kullanabileceğiniz bir [Web kancası yanıt](../log-analytics/log-analytics-alerts-actions.md).  Bu uyarıdan bilgi gönderirken bir dış web hizmeti çağrısı sağlar.

### <a name="views"></a>Görünümler
Günlük analizi görünümlerde günlük analizi depo verilerini görselleştirmek için kullanılır.  Her çözüm genellikle tek bir görünümle içerecek bir [döşeme](../log-analytics/log-analytics-view-designer-tiles.md) kullanıcının ana panosunda görüntülenir.  Görünüm herhangi bir sayıda içerebilir [görselleştirme bölümleri](../log-analytics/log-analytics-view-designer-parts.md) kullanıcıya toplanan verileri farklı görselleştirmesini sağlamak için.

[Görünüm Tasarımcısı'nı kullanarak özel görünümlerini oluşturma](../log-analytics/log-analytics-view-designer.md) , daha sonra çözüm dosyanızda için içerme dışarı aktarabilirsiniz.  


## <a name="create-solution-file"></a>Çözüm dosyası oluşturma
Yapılandırılmış ve çözümünüzü parçası olacak bileşenleri test sonra [çözüm dosyanızı oluşturun](operations-management-suite-solutions-solution-file.md).  Çözüm bileşenlerinde uygulayacak bir [Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md) içeren bir [çözüm kaynak](operations-management-suite-solutions-solution-file.md#solution-resource) ilişkileri dosyasındaki diğer kaynaklara sahip.  


## <a name="test-your-solution"></a>Çözümünüzü
Çözümünüzü geliştirirken ve çalışma alanınızda test yüklemeniz gerekir.  Kullanılabilir yöntemlerin birini kullanarak bunu yapabilirsiniz [test ve Resource Manager şablonları yükleme](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="publish-your-solution"></a>Çözümünüzü yayımlama
Tamamlandı ve çözümünüzü test sonra bunu aşağıdaki kaynaklardan aracılığıyla müşterilere sunabilirsiniz.

- **Azure hızlı başlangıç şablonları**.  [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/) GitHub aracılığıyla topluluğa katkıda bulunan Resource Manager şablonları kümesidir.  Çözümünüzü kullanılabilir aşağıdaki bilgileri tarafından yapabileceğiniz [katkı Kılavuzu](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Azure Market**.  [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/) dağıtmak ve diğer geliştiriciler, ISV, çözümünüzü satmak sağlar ve BT uzmanları.  Azure Market'ten çözümünüzü yayımlamak öğrenebilirsiniz [yayımlama ve Azure markette bir teklif yönetmek nasıl](../marketplace-publishing/marketplace-publishing-getting-started.md).



## <a name="next-steps"></a>Sonraki adımlar
* Bilgi nasıl [bir çözüm dosyası oluşturma](operations-management-suite-solutions-solution-file.md) Yönetimi çözümünüz için.
* Ayrıntılarını öğrenmek [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).
* Arama [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates) farklı Resource Manager şablonları örneklerini için.