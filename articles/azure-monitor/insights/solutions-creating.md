---
title: Azure'da bir yönetimi çözümü oluşturmak | Microsoft Docs
description: Yönetim çözümleri, Azure'da yer alan müşteriler, Log Analytics çalışma alanınıza ekleyebilirsiniz paketlenmiş yönetim senaryolarını içerir.  Bu makale, kendi ortamınızda kullanılacak yönetim çözümleri nasıl oluşturabileceğiniz hakkında ayrıntılar sağlar veya müşterileriniz için kullanılabilir.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ef1af4d3d27bc098341a4de716e293557baa946a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595826"
---
# <a name="design-and-build-a-management-solution-in-azure-preview"></a>Tasarım ve azure'da (Önizleme) bir yönetim çözümü oluşturun
> [!NOTE]
> Şu anda Önizleme aşamasında olan Azure yönetim çözümleri oluşturmak için başlangıç belgeleri budur. Aşağıda açıklanan herhangi bir şema tabi bir değişikliktir.

[Yönetim çözümleri]( solutions.md) müşteriler, Log Analytics çalışma alanınıza ekleyebilirsiniz paket Yönetimi senaryoları sağlar.  Bu makalede, tasarlamak ve oluşturmak için en yaygın gereksinimlerine uygun bir Yönetimi çözümünün temel bir işlemi sunar.  Bu işlemi başlangıç noktası olarak kullanın ve sonra gereksinimleriniz değiştikçe daha karmaşık çözümleri için kavramlar yararlanarak yönetim çözümleri oluşturma konusunda bilginiz varsa.

## <a name="what-is-a-management-solution"></a>Bir yönetim çözümü nedir?

Yönetim çözümleri belirli bir yönetim senaryosunu sağlamak için birlikte çalışan Azure kaynakları içerir.  Olarak uygulanan [Resource Manager şablonlarını](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md) yüklemek ve çözüm yüklendikten sonra içerdiği kaynaklarla yapılandırma ayrıntılarını içerir.

Temel bileşenleri tek tek Azure ortamınızda yazlım Yönetimi çözümünüz için stratejisidir.  Düzgün çalışmasını işlevselliğini açtıktan sonra daha sonra bunları paketleme başlatabilirsiniz bir [yönetim çözüm dosyası]( solutions-solution-file.md). 


## <a name="design-your-solution"></a>Çözümünüzü tasarlayın
Bir yönetim çözümü için en yaygın desen, aşağıdaki diyagramda gösterilmiştir.  Bu düzen farklı bileşenlerin ele alınmıştır aşağıda.

![Yönetim çözümüne genel bakış](media/solutions-creating/solution-overview.png)


### <a name="data-sources"></a>Veri kaynakları
Bir çözüm tasarlamanın ilk adımı, Log Analytics depodan ihtiyaç duyduğunuz verileri belirliyor.  Bu veriler tarafından toplanabilir bir [veri kaynağı](../../azure-monitor/platform/agent-data-sources.md) veya [başka bir çözüm]( solutions.md), veya çözümünüz, toplama işlemini sağlamanız gerekebilir.

Log Analytics deposunda açıklandığı toplanan veri kaynakları birkaç yol vardır [Log analytics'te veri kaynakları](../../azure-monitor/platform/agent-data-sources.md).  Bu, Windows olay günlüğüne olayları içerir veya Syslog tarafından ek olarak performans sayaçları hem Windows hem de Linux istemcileri için oluşturulan.  Azure İzleyici tarafından toplanan Azure kaynaklarından verileri de toplayabilirsiniz.  

Tüm kullanılabilir veri kaynakları erişilebilir değil veri gerektiren sonra kullanabileceğiniz [HTTP veri toplayıcı API'sini](../../azure-monitor/platform/data-collector-api.md) olanak sağlayan bir REST API'sine çağrı yapmadan herhangi bir istemciden Log Analytics depoya veri yazmak.  Özel veri toplama Yönetimi çözümüne en yaygın yolları oluşturmaktır bir [Azure Otomasyonu'nda runbook](../../automation/automation-runbook-types.md) gerekli verileri Azure'a veya dış kaynaklardan toplar ve yazılacak veri toplayıcı API'sini kullanır. Depo.  

### <a name="log-searches"></a>Günlük aramaları
[Günlük aramaları](../../azure-monitor/log-query/log-query-overview.md) ayıklayın ve Log Analytics depodaki verileri analiz etmek için kullanılır.  Bunlar, görünümler ve uyarılar, geçici çözümleme veri deposunda gerçekleştirmesine izin verme yanı sıra tarafından kullanılır.  

Herhangi bir görünüm veya uyarılar tarafından kullanılmayan bile kullanıcıya yardımcı olacağını düşündüğünüz tüm sorguları tanımlamanız gerekir.  Bunlar Portalı'nda kayıtlı aramalar kullanabilecekleri olacaktır ve bunları de içerebilir bir [, liste sorguları görselleştirme bölümü](../../azure-monitor/platform/view-designer-parts.md#list-of-queries-part) özel görünümünüzdeki.

### <a name="alerts"></a>Uyarılar
[Log analytics'teki uyarılar](../../azure-monitor/platform/alerts-overview.md) ile ilgili sorunları belirlemenize [günlük aramaları](#log-searches) karşı depodaki verileri.  Bunlar kullanıcıya bildir ya da otomatik olarak yanıtta bir eylem çalıştırın. Uygulamanızın farklı uyarı koşullarını tanımlamak ve karşılık gelen bir uyarı kuralları, çözüm dosyasına eklenecek gerekir.

Ardından sorun büyük olasılıkla otomatik bir işlem ile düzeltilebilir, bu düzeltmesi yapmak için Azure Automation'da bir runbook genellikle oluşturacaksınız.  Çoğu Azure hizmeti ile yönetilebilir [cmdlet'leri](/powershell/azure/overview) , bu tür işlevleri gerçekleştirmek için runbook yararlanarak.

Dış işlevler bir uyarıya yanıt olarak çözümünüzün gerektirdiği sonra kullanabileceğiniz bir [Web kancası yanıtı](../../azure-monitor/platform/alerts-metric.md).  Bu, uyarıyı bilgi gönderirken bir dış web hizmetini çağırmak sağlar.

### <a name="views"></a>Görünümler
Log Analytics'teki görünümler, Log Analytics deposuna verilerini görselleştirmek için kullanılır.  Her çözüm, genellikle tek bir görünümle içerecek bir [döşeme](../../azure-monitor/platform/view-designer-tiles.md) kullanıcının ana panosunda görüntülenir.  Görünüm herhangi bir sayıda içerebilir [görselleştirme bölümleri](../../azure-monitor/platform/view-designer-parts.md) kullanıcıya toplanan verilerin farklı görselleştirmeler sağlamak için.

[Görünüm Tasarımcısı'nı kullanarak özel görünümlerini oluşturma](../../azure-monitor/platform/view-designer.md) , daha sonra çözüm dosyasına ekleme için dışarı aktarabilirsiniz.  


## <a name="create-solution-file"></a>Çözüm dosyası oluşturma
Yapılandırılmış ve sınanan çözümünüzün bir parçası olacak bileşenleri sonra [çözüm dosyanızı oluşturmak]( solutions-solution-file.md).  Çözüm bileşenlerinde gerçekleştireceksiniz bir [Resource Manager şablonu](../../azure-resource-manager/resource-group-authoring-templates.md) içeren bir [çözüm kaynak]( solutions-solution-file.md#solution-resource) ilişkileri dosyasındaki diğer kaynaklara sahip.  


## <a name="test-your-solution"></a>Çözümünüzü test etme
Çözümünüzü geliştirirken ve çalışma alanınızda test yüklemeniz gerekir.  Kullanılabilir yöntemlerin herhangi biriyle kullanarak bunu yapabilirsiniz [Resource Manager şablonlarını yüklemek ve test](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="publish-your-solution"></a>Çözümünüzü yayımlayın
Tamamlandı ve çözümünüzü test sonra bunu kullanılabilir aşağıdaki kaynaklardan aracılığıyla müşterilere yapabilirsiniz.

- **Azure hızlı başlangıç şablonları**.  [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/) GitHub aracılığıyla topluluk tarafından katkıda bulunulan Resource Manager şablonları kümesidir.  Çözümünüzü kullanılabilir aşağıdaki bilgileri tarafından yapabileceğiniz [katkıda bulunma Kılavuzu](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Azure Market**.  [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/) dağıtmak ve diğer geliştiriciler, ISV'ler, çözümünüzü satma sağlar ve BT uzmanları.  Azure Market'ten çözümünüzü yayımlamak öğrenebilirsiniz [yayımlama ve Azure Marketi'nde Teklif yönetme](../../marketplace/marketplace-publishers-guide.md).



## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir çözüm dosyası oluşturmak]( solutions-solution-file.md) Yönetimi çözümünüz için.
* Ayrıntılarını öğrenmek [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md).
* Arama [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates) farklı Resource Manager şablonu örnekleri için.
