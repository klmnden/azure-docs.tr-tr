---
title: "Azure Stream Analytics tanılama günlükleri ile ilgili sorunları giderme | Microsoft Docs"
description: "Tanılama günlüklerini Microsoft Azure Stream Analytics işlerine çözümlemeyi öğrenin."
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: c9772df2c216d465ca6e90e69bce011969dd4f02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Azure Stream Analytics tanılama günlükleri kullanarak sorun giderme

Bazen, bir Azure akış analizi işi beklenmedik bir şekilde işlemeyi durdurur. Bu tür bir olay sorunlarını giderme olması önemlidir. Olay beklenmeyen sorgu sonucu, aygıtlara bağlantı veya bir beklenmeyen hizmet kesintisi tarafından neden olabilir. Stream Analytics tanılama günlüklerine yardımcı olabilecek oluşur ve kurtarma zamanı azaltmak sorunları nedenini belirlemeye.

## <a name="log-types"></a>Günlük türleri

Akış analizi günlükleri iki tür sunar: 
* [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (her zaman açıktır). Etkinlik günlükleri işleri üzerinde gerçekleştirilen işlemler fikir verir.
* [Tanılama günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (yapılandırılabilir). Tanılama günlükleri daha zengin bir işlemle gerçekleşen her şeyi fikir sağlar. İş silindiğinde tanılama iş oluşturulduğunda, başlangıç ve bitiş günlüğe kaydeder. Bunlar olayları iş güncelleştirildiği ve çalışırken kapsar.

> [!NOTE]
> Azure Storage, Azure Event Hubs'a ve Azure günlük analizi gibi hizmetleri uyumsuz verileri çözümlemek için kullanabilirsiniz. Bu hizmetlere ilişkin fiyatlandırma modeli göre sizden ücret kesilir.
>

## <a name="turn-on-diagnostics-logs"></a>Tanılama günlüklerini Aç

Tanılama günlükleri **kapalı** varsayılan olarak. Tanılama günlüklerini açmak için şu adımları tamamlayın:

1.  Azure portalında oturum açın ve akış iş dikey penceresine gidin. Altında **izleme**seçin **tanılama günlükleri**.

    ![Tanılama günlüklerini dikey gezinme](./media/stream-analytics-job-diagnostic-logs/image1.png)  

2.  Seçin **tanılamayı açın**.

    ![Tanılama günlüklerini Aç](./media/stream-analytics-job-diagnostic-logs/image2.png)

3.  Üzerinde **tanılama ayarları** sayfası için **durum**seçin **üzerinde**.

    ![Tanılama günlükleri için Durumu Değiştir](./media/stream-analytics-job-diagnostic-logs/image3.png)

4.  İstediğiniz arşivleme hedefi (depolama hesabı, olay hub'ı, günlük analizi) olarak ayarlayın. Toplamak istediğiniz günlükleri kategorileri seçin (yürütme, yazma). 

5.  Yeni tanılama yapılandırmayı kaydedin.

Tanılama yapılandırması etkili olması için yaklaşık 10 dakika sürer. Bundan sonra günlükler Başlat yapılandırılmış arşivleme hedef görünmesini (bunlar üzerinde görebilirsiniz **tanılama günlükleri** sayfa):

![Tanılama günlükleri - arşivleme hedefleri dikey gezinme](./media/stream-analytics-job-diagnostic-logs/image4.png)

Tanılama yapılandırma hakkında daha fazla bilgi için bkz: [toplamak ve Azure kaynaklarınızı Tanılama verileri kullanma](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Kategoriler tanılama günlük

Şu anda, tanılama günlüklerini iki kategorisi Yakala:

* **Yazma**. Yazma işlemleri işi ilgili olayları yakalar oturum: Proje oluşturma, ekleme ve silme girdi ekleme ve sorgu güncelleştirme çıkarır ve başlangıç ve işi durduruluyor.
* **Yürütme**. İş yürütme sırasında meydana gelen olayları yakalar:
    * Bağlantı hataları
    * Dahil olmak üzere, veri işleme hatalar:
        * Sorgu tanımı (eşleşmeyen alan türleri ve değerleri, eksik alanlar vb.) için uygun olmayan olaylar
        * İfade değerlendirme hataları
    * Diğer olayları ve hataları

## <a name="diagnostics-logs-schema"></a>Tanılama günlüklerini şeması

Tüm günlükler JSON biçiminde depolanır. Her giriş aşağıdaki ortak alanlarına sahiptir:

Ad | Açıklama
------- | -------
time | Günlük zaman damgası (UTC içinde).
resourceId | İşlemi bir yerde, büyük harflerle sürdü kaynak kimliği. Abonelik kimliği, kaynak grubu ve iş adı içerir. Örneğin,   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMINGJOBS/STREAMANALYTICS/MYSTREAMINGJOB**.
category | Kategori ya da oturum **yürütme** veya **yazma**.
operationName | Oturum açmış işlemin adı. Örneğin, **olayları göndermek: SQL çıktı yazma hatası için mysqloutput**.
durum | İşlem durumu. Örneğin, **başarısız** veya **başarılı**.
düzeyi | Günlük düzeyi. Örneğin, **hata**, **uyarı**, veya **bilgilendirici**.
properties | JSON bir dize olarak serileştirilmiş günlük girişi özgü ayrıntısı. Daha fazla bilgi için aşağıdaki bölümlere bakın.

### <a name="execution-log-properties-schema"></a>Yürütme günlüğü özellikleri şeması

Yürütme günlüklerini Stream Analytics işi yürütme sırasında gerçekleşen olayları hakkında bilgi vardır. Özellikler şeması, olay türüne bağlı olarak değişir. Şu anda, yürütme günlüklerini aşağıdaki türleri vardır:

### <a name="data-errors"></a>Veri hataları

İş verileri işlerken oluşan herhangi bir hata Bu günlükler kategorisindedir. Bu günlükler en sık okunan, veri seri hale getirme, sırasında oluşturulur ve yazma işlemleri. Bu günlükler bağlantı hataları içermez. Bağlantı hataları, genel olaylar olarak kabul edilir.

Ad | Açıklama
------- | -------
Kaynak | İş adı giriş veya hatanın oluştuğu çıktı.
İleti | Şu hata ile ilişkili ileti.
Tür | Hata türü. Örneğin, **DataConversionError**, **CsvParserError**, veya **ServiceBusPropertyColumnMissingError**.
Veriler | Doğru bir şekilde kullanarak hatanın kaynağı bulmak yararlı olan verileri içerir. Boyutuna bağlı olarak kesilmesi için konu.

Bağlı olarak **operationName** değeri, aşağıdaki şema veri hatalar var:
* **Olayları sıralamak**. Seri hale olayları olay okuma işlemleri sırasında oluşur. Bunlar durum, giriş verileri şunlardan biri nedeniyle sorgu şeması karşılamadığı oluşur:
    * *Tür uyumsuzluğu (de) olay sırasında seri*: hataya neden olan alan tanımlar.
    * *Bir olay, geçersiz bir seri hale getirme okuyamıyor*: hatanın oluştuğu konum giriş verilerindeki hakkında bilgileri listeler. Blob giriş, uzaklık ve bir örnek verileri için BLOB adı içerir.
* **Olayları göndermek**. Yazma işlemleri sırasında olaylarından gönderin. Bunlar hataya akış olayını tanımlayın.

### <a name="generic-events"></a>Genel olaylar

Genel olaylar şey kapsar.

Ad | Açıklama
-------- | --------
Hata | (isteğe bağlı) Hata bilgileri. Genellikle, varsa bu özel durum bilgilerini adıdır.
İleti| Günlük iletisi.
Tür | İleti türü. İç kategorilere ayırma hatalarının eşlenir. Örneğin, **JobValidationError** veya **BlobOutputAdapterInitializationFailure**.
Bağıntı Kimliği | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) , iş yürütme benzersiz şekilde tanımlar. İşi durur aynı kadar tüm yürütme günlük girişlerini zamandan işini başlatır **bağıntı kimliği** değeri.

## <a name="next-steps"></a>Sonraki adımlar

* [Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
