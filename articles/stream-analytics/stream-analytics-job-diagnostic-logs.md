---
title: Azure Stream Analytics, tanılama günlüklerini kullanarak sorun giderme
description: Bu makalede, Azure Stream analytics'te tanılama günlüklerini çözümleme açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: db3c9874676e3240f6896c1e1ff8f873360c20d5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090831"
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Azure Stream Analytics, tanılama günlükleri kullanarak sorun giderme

Bazen, bir Azure Stream Analytics işi beklenmedik bir şekilde işlemeyi durdurur. Bu tür bir olay sorun giderme önemlidir. Olay beklenmeyen sorgu sonucu, bağlantı cihazlara veya tarafından beklenmeyen bir hizmet kesintisinden kaynaklanıyor olabilir. Stream analytics'te tanılama günlükleri yardımcı olabilecek oluşur ve kurtarma süresini kısaltmak, sorunların nedenini tanımlayın.

## <a name="log-types"></a>Günlük türü

Stream Analytics, günlükleri iki tür sunar: 
* [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (her zaman açıktır). Etkinlik günlükleri işler üzerinde gerçekleştirilen işlemler hakkında Öngörüler sunar.
* [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (yapılandırılabilir). Tanılama günlükleri ile bir iş gerçekleşen her şeyi daha zengin Öngörüler sağlar. İş silindiğinde tanılama iş oluşturulduğunda başlangıç ve bitiş günlüğe kaydeder. Bunlar, iş güncelleştirildiğinde ve çalışırken olayları kapsar.

> [!NOTE]
> Azure depolama, Azure Event Hubs ve Azure Log Analytics gibi hizmetler, uyumsuz verileri çözümlemek için kullanabilirsiniz. Bu hizmetler için fiyatlandırma modeline göre ücretlendirilirsiniz.
>

## <a name="turn-on-diagnostics-logs"></a>Tanılama günlüklerini açın

Tanılama günlükleri **kapalı** varsayılan olarak. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları tamamlayın:

1.  Azure portalında oturum açın ve akış işi dikey penceresine gidin. Altında **izleme**seçin **tanılama günlükleri**.

    ![Tanılama günlükleri dikey penceresinde gezinme](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs-monitoring.png)  

2.  Seçin **tanılamayı Aç**.

    ![Stream Analytics tanılama günlüklerini açın](./media/stream-analytics-job-diagnostic-logs/turn-on-diagnostic-logs.png)

3.  Üzerinde **tanılama ayarları** sayfası için **durumu**seçin **üzerinde**.

    ![Değişiklik durumu için tanılama günlükleri](./media/stream-analytics-job-diagnostic-logs/save-diagnostic-log-settings.png)

4.  İstediğiniz arşivleme hedef (depolama hesabı, olay hub'ı Log Analytics'e) ayarlayın. Toplamak istediğiniz günlükleri kategorileri seçin (yürütme, yazma). 

5.  Yeni tanılama yapılandırmayı kaydedin.

Tanılama yapılandırması etkili olması için yaklaşık 10 dakika sürer. Bundan sonra günlükleri başlangıç yapılandırılmış arşivleme hedef görünen (bunlar üzerinde görebilirsiniz **tanılama günlükleri** sayfa):

![Tanılama günlükleri - arşiv hedefleri için dikey penceresinde gezinme](./media/stream-analytics-job-diagnostic-logs/view-diagnostic-logs-page.png)

Tanılama yapılandırma hakkında daha fazla bilgi için bkz. [toplama ve Tanılama verileri Azure kaynaklarınızı kullanma](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Tanılama günlük kategorileri

Şu anda, tanılama günlüklerinin iki kategoriye Yakala:

* **Yazma**. Yakalama işi yazma işlemleri ile ilgili olayları günlüğe: Proje oluşturma, ekleme ve silme girişlerinin ekleme ve güncelleştirme sorgu çıkarır ve başlangıç ve işi durduruluyor.
* **Yürütme**. İş yürütme sırasında gerçekleşen olayları yakalar:
    * Bağlantı hataları
    * Dahil olmak üzere, veri işleme hataları:
        * Sorgu tanımı (eşleşmeyen alan türleri ve değerleri, eksik alanlar vb.) için uygun olmayan olayları
        * İfade değerlendirme hataları
    * Diğer olayları ve hataları

## <a name="diagnostics-logs-schema"></a>Tanılama günlükleri şeması

Tüm günlükler, JSON biçiminde depolanır. Her girişin aşağıdaki ortak dize alanlara sahiptir:

Ad | Açıklama
------- | -------
time | Zaman damgası (UTC) günlüğü.
resourceId | İşlem büyük harf, yerinde geçen kaynak kimliği. Bu abonelik kimliği, kaynak grubunu ve iş adı içerir. Örneğin,   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMINGJOBS/STREAMANALYTICS/MYSTREAMINGJOB**.
category | Kategori ya da oturum **yürütme** veya **yazma**.
operationName | Oturum açmış işlemin adı. Örneğin, **olayları gönderme: SQL çıkış yazma hatası için mysqloutput**.
durum | İşlemin durumu. Örneğin, **başarısız** veya **başarılı**.
düzey | Günlük düzeyi. Örneğin, **hata**, **uyarı**, veya **bilgilendirici**.
properties | Günlük girdisi özgü ayrıntılı olarak serileştirilmiş bir JSON dizesi. Daha fazla bilgi için aşağıdaki bölümlere bakın.

### <a name="execution-log-properties-schema"></a>Yürütme günlüğü özellikleri şeması

Stream Analytics iş yürütme sırasında gerçekleşen olaylar hakkında bilgi yürütme günlükleri vardır. Şema özellikleri, olay türüne bağlı olarak değişir. Şu anda, yürütme günlükleri şu türlerini sunuyoruz:

### <a name="data-errors"></a>Veri hataları

İş verileri işlerken oluşan herhangi bir hata günlükleri Bu kategoride var. Bu günlükler genellikle okuma, oluşturma sırasında yerleşim verilerine seri hale getirme, oluşturulur ve yazma işlemleri. Bu günlükler, bağlantı hataları içermez. Bağlantı hataları genel olayları olarak kabul edilir.

Ad | Açıklama
------- | -------
Kaynak | İş adı, giriş veya hatanın oluştuğu çıktı.
İleti | Hatayla ilişkili ileti.
Tür | Hata türü. Örneğin, **DataConversionError**, **CsvParserError**, veya **ServiceBusPropertyColumnMissingError**.
Veriler | Hatanın kaynağını doğru bir şekilde bulmak yararlı olan veriler içerir. Kesilme tabidir, boyutuna bağlı olarak.

Yapılandırmanıza bağlı olarak **operationName** değeri aşağıdaki şema veri hatalar var:
* **Olayları sıralamak**. Serileştirme olayları olay okuma işlemleri sırasında oluşur. Giriş verileri Bu nedenlerden biri için sorgu şeması karşılamaz bunlar ortaya çıkar:
    * *Tür uyumsuzluğu (de) olay sırasında serileştirmek*: hataya neden olan bir alan tanımlar.
    * *Bir olay, geçersiz bir seri hale getirme okunamıyor*: hatanın oluştuğu giriş verilerinin konumu hakkında bilgi listeler. BLOB adı için blob giriş uzaklığı ve verilerin bir örnek içerir.
* **Olayları gönderme**. Yazma işlemleri sırasında olaylar meydana gönderin. Bunlar, hataya neden olan akış olayı tanımlar.

### <a name="generic-events"></a>Genel olaylar

Genel olaylar, diğer her şeyi kapsar.

Ad | Açıklama
-------- | --------
Hata | (isteğe bağlı) Hata bilgileri. Genellikle, varsa bu özel durum bilgilerini adıdır.
İleti| Günlük iletisi.
Tür | İleti türü. İç hataların kategorilere ayrılması eşlenir. Örneğin, **JobValidationError** veya **BlobOutputAdapterInitializationFailure**.
Bağıntı Kimliği | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) tanımlamasının, iş yürütme. Aynı işi durur bulunana kadar andan itibaren tüm yürütme günlüğü girişleri işini başlatır. **bağıntı kimliği** değeri.

## <a name="next-steps"></a>Sonraki adımlar

* [Stream analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
