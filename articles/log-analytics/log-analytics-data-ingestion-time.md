---
title: Azure Log Analytics veri alımı sürede | Microsoft Docs
description: Farklı Azure Log Analytics veri toplama, gecikme süresini etkileyen faktörleri açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: bwren
ms.openlocfilehash: f40c8ed7eb6bfae958b3b57c4b7d525963ab9741
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955271"
---
# <a name="data-ingestion-time-in-log-analytics"></a>Log analytics'te veri alım zamanı
Azure Log Analytics, binlerce müşteri terabaytlarca veriyi her ay büyüyen bir hızda gönderme yapan bir Azure İzleyici'de büyük ölçekli veri hizmetidir. Genellikle verileri toplandıktan sonra Log Analytics'te kullanılabilir olana kadar geçen süreyi hakkında sorular vardır. Bu makalede, bu gecikme süresini etkileyen faktörleri farklı açıklanmaktadır.

## <a name="typical-latency"></a>Tipik bir gecikme süresi
Gecikme süresi verileri izlenen sistemde oluşturulduğu tarih ve analiz Log analytics'te gelen süreyi ifade eder. Log Analytics'e veri almak için tipik bir gecikme süresi arasında 2 ve 5 dakikadır. Herhangi bir veri belirli gecikme sürelerini faktörler aşağıda açıklanan çeşitli bağlı olarak değişir.


## <a name="factors-affecting-latency"></a>Gecikme süresini etkileyen faktörler
Toplam alım zaman belirli bir veri kümesi aşağıdaki üst düzey alanlarına ayrılabilir. 

- Aracı saati - bir olay bulmak, toplamak ve Log Analytics alma noktası günlük kaydı olarak gönderin. Çoğu durumda, bu işlem bir aracı tarafından işlenir.
- İşlem hattı saati - günlük kaydı işlenecek alım işlem hattının. Bu olay özelliklerini ayrıştırma ve potansiyel olarak hesaplanan bilgi eklemeyi içerir.
- Dizin oluşturma saati – Log Analytics, büyük veri deposuna bir günlük kaydı alabilmek için harcanan süre.

Bu işlemde sunulan farklı gecikme süresi ile ilgili ayrıntılar aşağıda açıklanmıştır.

### <a name="agent-collection-latency"></a>Aracı koleksiyon gecikme süresi
Aracılar ve yönetim çözümleri farklı stratejiler gecikme süresini etkileyebilir bir sanal makine verileri toplamak için kullanın. Belirli bazı örnekler şunlardır:

- Windows olayları, syslog olayları ve performans ölçümlerini hemen toplanır. Linux performans sayaçlarıyla 30 saniyelik aralıklarla yoklama.
- Zaman değiştikten sonra IIS günlükler ve özel günlükleri toplanır. IIS günlükler için bunu tarafından etkilenir [IIS üzerinde yapılandırılmış rollover zamanlaması](log-analytics-data-sources-iis-logs.md). 
- Active Directory çoğaltma çözümü, Active Directory değerlendirmesi çözümü, Active Directory altyapınızın haftalık bir değerlendirme gerçekleştirirken, değerlendirme beş günde gerçekleştirir. Yalnızca değerlendirme tamamlandıktan sonra aracı bu günlükleri toplar.

### <a name="agent-upload-frequency"></a>Aracı karşıya yükleme frekansı
Log Analytics aracısını basit olmasını sağlamak için aracı günlüklerini arabelleğe alır ve bunları düzenli aralıklarla Log Analytics'e gönderir. Karşıya yükleme ve veri türüne bağlı olarak 2 dakika 30 saniye arasında sıklığı değişir. 1 dakika içinde en çok veriyi karşıya yüklendi. Ağ koşulları, bu veriler, Log Analytics alma noktası ulaşmak için gecikme süresini olumsuz yönde etkileyebilir.

### <a name="azure-logs-and-metrics"></a>Azure günlükleri ve ölçümler 
Etkinlik günlüğü verileri Log Analytics'e başlatılabilmek için yaklaşık 5 dakika sürer. Tanılama günlükleri ve ölçüm verilerini Azure hizmete bağlı olarak kullanılabilir hale gelmesi için 1-5 dakika sürebilir. Ardından bir ek 30-60 Log Analytics alımı noktasına gönderilecek saniye günlükleri ve ölçüm verileri için 3 dakika sürer.

### <a name="management-solutions-collection"></a>Yönetim çözümleri toplama
Bazı çözümler verilerine bir Aracıdan toplamaz ve ek gecikme sağlayan bir koleksiyon yöntemi kullanabilir. Bazı çözümler, neredeyse gerçek zamanlı koleksiyon denemeden, düzenli aralıklarla veri toplayın. Belirli örnekler aşağıdakileri içerir:

- Office 365 çözüm, Office 365 Yönetim etkinliği şu anda tüm neredeyse gerçek zamanlı gecikme süresi garanti eder sağlamaz API'sini kullanarak etkinlik günlüklerini yoklar.
- Windows Analytics çözümleri (örneğin güncelleştirme uyumluluğu) veri günlük sıklıkta çözüm tarafından toplanır.

Koleksiyon sıklığının belirlenmesi her bir çözüm için belgelere bakın.

### <a name="pipeline-process-time"></a>İşlem hattı işlem süresi
Log Analytics işlem hattı'nda günlük kayıtları içe alınan sonra geçici depolama birimine Kiracı yalıtımı sağlamak ve veri kaybı olmadığından emin olmak için yazılan. Bu işlem genellikle 5-15 saniye ekler. Bazı yönetim çözümleri, veri toplama daha ağır algoritmalarını uygulayan ve içindeki veri akış içgörülere sahip olun. Örneğin, ağ performansı izleme gelen veri 3 dakikalık aralıklarında etkili bir şekilde 3 dakikalık bir gecikme ekleme toplar. Gecikme süresi ekler, başka bir işlem özel günlükleri işleyen işlemidir. Bazı durumlarda, bu işlem birkaç dakika gecikme dosyalarından aracısı tarafından toplanan günlükleri ekleyebilir.

### <a name="new-custom-data-types-provisioning"></a>Sağlama, yeni özel veri türleri
Öğesinden yeni bir özel veri türü oluşturulduğunda bir [özel günlük](../log-analytics/log-analytics-data-sources-custom-logs.md) veya [veri toplayıcı API'sini](../log-analytics/log-analytics-data-collector-api.md), sistem bir adanmış depolama kapsayıcısı oluşturur. Bu, bu veri türü yalnızca ilk görünümünü oluşan tek seferlik bir ek yüktür.

### <a name="surge-protection"></a>Aşırı yükleme talebiyle koruma
Log Analytics üst önceliğini sistem veri dalgalanmalarına için yerleşik koruma bulunur şekilde müşteri veri kaybı, olduğundan emin olmaktır. Bu, büyük yükü altında bile, sistemin çalışıp tutmak emin emin olmak için arabellek içerir. Normal yük altında bu denetimlerin bir dakikadan az ekleyebilirsiniz, ancak aşırı koşullar ve bunlar veri sağlarken önemli zaman ekleyebilirsiniz hataları güvenlidir.

### <a name="indexing-time"></a>Dizin oluşturma zamanı
Analiz ve Gelişmiş arama özellikleri aksine verilere hemen erişim sağlamaya sağlamaya arasındaki her büyük veri platformu için yerleşik bir denge yoktur. Azure Log Analytics kayıtları milyarlarca üzerinde güçlü sorgular çalıştırın ve birkaç saniye içinde sonuçları almak sağlar. Altyapı ve verileri kendi alımı sırasında önemli ölçüde dönüştüren ve benzersiz compact yapılarını depolar olduğundan bu mümkün olur. Sistem, yeterli miktarı kadar bu yapıları oluşturmak için kullanılabilir veri arabelleğe alır. Arama sonuçlarında günlük kaydı görünmeden önce bu tamamlanması gerekir.

Bu işlem şu anda yaklaşık 5 dakika sürer. daha yüksek veri fiyatları üzerinden ancak daha az zaman veri düşük hacim olduğunda. Bu, birlikte değişkenlik mantığa aykırı görünüyor, ancak bu işlem gecikme süresi yüksek hacimli üretim iş yükleri için en iyi duruma getirilmesi sağlar.



## <a name="checking-ingestion-time"></a>Alım zamanı
Kullanabileceğiniz **sinyal** gecikme verilerini aracılardan kestirmek için tablo. Bir kez sinyal gönderildiğinden bu yana bir dakika, son sistem durumu kaydı geçerli saati arasındaki farkı ideal olarak bir dakika olarak mümkün olduğunca yakın olacaktır.

En yüksek gecikme süresiyle bilgisayarları listelemek için aşağıdaki sorguyu kullanın.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | top 50 by IngestionTime asc

 
Büyük ortamlarda aşağıdaki sorguyu kullanın, toplam bilgisayar sayısı farklı yüzdelerini gecikme sürelerini özetler.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | summarize percentiles(IngestionTime, 50,95,99)



## <a name="next-steps"></a>Sonraki adımlar
* Okuma [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) Log Analytics için.

