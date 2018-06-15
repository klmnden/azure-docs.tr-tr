---
title: Toplamak ve Azure günlük analizi, Windows olay günlüklerini analiz edin | Microsoft Docs
description: Windows olay günlüklerini günlük analizi tarafından kullanılan en yaygın veri kaynaklarının biridir.  Bu makalede Windows olay günlüklerini koleksiyonunu ve Ayrıntılar için günlük analizi çalışma alanında oluşturdukları kayıtlarının nasıl yapılandırılacağı açıklanmaktadır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: bwren
ms.openlocfilehash: 7a7deb4d7a287b2e9613e6035a7ffd7bb6f14f9c
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
ms.locfileid: "26782039"
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windows olay günlüğü veri kaynaklarında, günlük analizi
Windows olay günlüklerini en yaygın biri olan [veri kaynakları](log-analytics-data-sources.md) birçok uygulama Windows olay günlüğüne yazma beri Windows aracıları kullanarak veri toplama için.  İzlemeniz gereken uygulamaları tarafından oluşturulan herhangi bir özel günlük belirtmeye ek sistem ve uygulama gibi standart günlüklerindeki olayları toplayabilir.

![Windows olayları](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Yapılandırma Windows olay günlükleri
Windows olay günlüklerini yapılandırma [günlük analizi ayarları veri menüde](log-analytics-data-sources.md#configuring-data-sources).

Günlük analizi ayarlarında belirtilen Windows olay günlüklerini yalnızca olayları toplar.  Bir olay günlüğü günlük adını yazıp'yi tıklatarak ekleyebilirsiniz  **+** .  Her bir günlükteki yalnızca seçilen önem derecelerine sahip olayları toplanır.  Toplamak istediğiniz belirli günlük için önem derecelerine denetleyin.  Filtre olayları için herhangi bir ek ölçüt sağlayamaz.

Bir olay günlüğü adı yazarken, günlük analizi ortak olay günlüğü adlarının öneriler sağlar. Eklemek istediğiniz günlük listede görünmüyorsa, günlüğünün tam adı yazarak hala ekleyebilirsiniz. Olay Görüntüleyicisi'ni kullanarak günlük tam adını bulabilirsiniz. Olay Görüntüleyicisi'nde açın *özellikleri* sayfasında günlüğü ve dizeden kopyalama *tam adı* alan.

![Windows olayları yapılandırın](media/log-analytics-data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Veri toplama
Günlük analizi seçili önem izlenen bir olay günlüğündeki olay oluşturuldu olarak eşleşen her olay toplar.  Aracı onun yerine üzerinden topladığı her olay günlüğüne kaydeder.  Aracı bir süre için çevrimdışı olursa, aracıyı çevrimdışıyken olayları oluşturulmuş olsalar bile sonra günlük analizi olayları son devre dışı kaldığı toplar.  Bu olayları olay günlüğünü aracı çevrimdışı durumdayken üzerine yazmaya uncollected olaylarla sarmalar durumunda değil toplanacak potansiyeli vardır.

>[!NOTE]
>Günlük analizi kaynağından SQL Server tarafından oluşturulan denetim olaylarını toplama olmayan *MSSQLSERVER* anahtar sözcükleri - içeren olay kimliği 18453 *Klasik* veya *denetim başarı* ve anahtar sözcüğü *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Windows olay kayıtlarını özellikleri
Windows olay kayıtlarını sahip bir tür **olay** ve aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayarın adı. |
| EventCategory |Olay kategorisi. |
| EventData |Tüm olay verileri ham biçiminde. |
| Olay Kimliği |Olay sayısı. |
| eventLevel |Önem derecesi sayısal form durumda. |
| EventLevelName |Metin biçiminde olayın önem derecesi. |
| Olay günlüğü |Olay toplandığı olay günlüğü adı. |
| ParameterXml |Olay parametre değerleri XML biçiminde. |
| ManagementGroupName |System Center Operations Manager aracıları için yönetim grubu adı.  Diğer aracıları için bu değer AOI -:<workspace ID> |
| RenderedDescription |Parametre değerleri ile olay açıklaması |
| Kaynak |Olay kaynağı. |
| SourceSystem |Olay toplandığı aracı türü. <br> OpsManager – Windows aracı, ya da doğrudan bağlanın veya Operations Manager yönetilen <br> Linux – tüm Linux aracıları  <br> AzureStorage – Azure tanılama |
| TimeGenerated |Tarih ve saat Windows olay oluşturuldu. |
| UserName |Olayın günlüğe hesabının kullanıcı adı. |

## <a name="log-searches-with-windows-events"></a>Windows olay günlüğü aramalar
Aşağıdaki tabloda, Windows olay kayıtlarını almak günlük arama farklı örnekleri sağlar.

| Sorgu | Açıklama |
|:---|:---|
| Olay |Tüm Windows olayları. |
| Olay &#124; Burada EventLevelName "error" == |Tüm Windows olayları hata önem derecesi. |
| Olay &#124; Kaynak tarafından Count() özetler |Kaynak olayların Windows sayısı. |
| Olay &#124; Burada EventLevelName "error" &#124; == Kaynak tarafından Count() özetler |Sayısı, Windows hata olayları kaynağa göre. |


## <a name="next-steps"></a>Sonraki adımlar
* Diğer toplamak için günlük analizi yapılandırma [veri kaynakları](log-analytics-data-sources.md) çözümleme için.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için.  
* Kullanım [özel alanlar](log-analytics-custom-fields.md) olay kayıtlarını tek tek alanlarına ayrıştırılamıyor.
* Yapılandırma [performans sayaçları koleksiyonunu](log-analytics-data-sources-performance-counters.md) Windows aracılardan gelen.
