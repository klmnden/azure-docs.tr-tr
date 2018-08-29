---
title: Azure Log Analytics - Azure Logic Apps B2B iletilerini izleme | Microsoft Docs
description: Tümleştirme hesapları ve Azure Log Analytics ile Azure Logic Apps B2B iletişimini izleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 06/19/2018
ms.openlocfilehash: 5bf5385824eb9b711a2fee547c29d24d7ef5a01d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125777"
---
# <a name="track-b2b-communication-with-azure-log-analytics"></a>B2B iletişim Azure Log Analytics ile izleme

İkisi arasındaki B2B iletişim kurduktan sonra iş süreçlerine veya tümleştirme hesabınız üzerinden uygulamaları çalıştıran bu varlıkların birbirleriyle iletiler gönderip alabilir. Bu iletiler doğru işlenir, AS2, X12, izleyebilir ve EDIFACT iletileri ile olup olmadığını denetlemek için [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Örneğin, izleme iletileri için bu izleme web tabanlı özellikleri kullanabilirsiniz:

* İleti sayısı ve durumu
* Bildirim durumu
* İletileri bildirimler ile ilişkilendirin
* Hataları için ayrıntılı hata açıklamaları
* Arama özellikleri

## <a name="requirements"></a>Gereksinimler

* Tanılama günlük kaydı ile ayarlanmış bir mantıksal uygulama. Bilgi [bir mantıksal uygulama oluşturma işlemini](quickstart-create-first-logic-app-workflow.md) ve [nasıl ayarlanacağı, mantıksal uygulama için günlüğe kaydetmeyi](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* İzleme ve günlüğe kaydetme ile ayarlanmış bir tümleştirme hesabı. Bilgi [tümleştirme hesabı oluşturma işlemini](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve [izleme ve günlüğe kaydetme için bu hesabı ayarlamak nasıl](../logic-apps/logic-apps-monitor-b2b-message.md).

* Henüz kaydolmadıysanız [Log Analytics için tanılama verilerini yayımlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Önceki gereksinimlerini karşılamanızın sonra bir Log Analytics çalışma alanı olmalıdır. B2B iletişiminizin Log Analytics, izleme için aynı çalışma alanı kullanmanız gerekir. 
>  
> Bir Log Analytics çalışma alanınız yoksa, bilgi [bir Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="add-the-logic-apps-b2b-solution-to-log-analytics"></a>Logic Apps B2B çözümü Log Analytics'e eklemek

Mantıksal uygulamanız için B2B iletilerini izleme log Analytics için eklemelisiniz **Logic Apps B2B** OMS portalındaki çözüm. Daha fazla bilgi edinin [Log Analytics çözümleri ekleme](../log-analytics/log-analytics-quick-create-workspace.md).

1. İçinde [Azure portalında](https://portal.azure.com), seçin **tüm hizmetleri**. "Log analytics için" için arama yapın ve ardından **Log Analytics** burada gösterildiği gibi:

   ![Log Analytics'i bulma](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin. 

   ![Log Analytics çalışma alanınızı seçin](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Altında **Yönetim**, seçin **genel bakış**.

   ![Log Analytics Portalı'nı seçin](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Giriş sayfası açıldıktan sonra seçin **Ekle** Logic Apps B2B çözümü yüklemek için.    
   ![Çözüm Galerisi seçin](media/logic-apps-track-b2b-messages-omsportal/add-b2b-solution.png)

5. Altında **yönetim çözümleri**, bulma ve oluşturma **Logic Apps B2B** çözüm.     
   ![Logic Apps B2B seçin](media/logic-apps-track-b2b-messages-omsportal/create-b2b-solution.png)

   Giriş sayfasında, kutucuğuna **Logic Apps B2B iletileri** görünür. 
   B2B iletilerinizi işlendiğinde bu kutucuk ileti sayısı güncelleştirir.

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-log-analytics"></a>İleti durumu ve ayrıntıları Log analytics'te izleyin

1. B2B iletilerinizi işlendikten sonra durum ve bu iletileri ayrıntılarını görüntüleyebilirsiniz. Genel bakış sayfasında, **Logic Apps B2B iletileri** Döşe.

   ![Güncelleştirilmiş ileti sayısı](media/logic-apps-track-b2b-messages-omsportal/b2b-overview-tile.png)

   > [!NOTE]
   > Varsayılan olarak, **Logic Apps B2B iletileri** kutucuğu, tek bir günde göre verileri gösterir. Farklı bir zaman aralığı için veri kapsamı değiştirmek için sayfanın üst kapsam denetimi seçin:
   > 
   > ![Veri Kapsamı Değiştir](media/logic-apps-track-b2b-messages-omsportal/server-filter.png)
   >

2. İleti sonra durum panosu açılır, tek bir günde göre verileri gösteren bir özel ileti türü için daha fazla ayrıntı görüntüleyebilirsiniz. İçin kutucuğu seçin **AS2**, **X12**, veya **EDIFACT**.

   ![İleti Durumu görüntüle](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Seçilen kutucuğunuzu iletileri listesi görüntülenir. 
   Her ileti türü özellikleri hakkında daha fazla bilgi için bu ileti özellik açıklamaları bakın:

   * [AS2 ileti özellikleri](#as2-message-properties)
   * [İleti özelliklerinin X12](#x12-message-properties)
   * [EDIFACT ileti özellikleri](#EDIFACT-message-properties)

   Örneğin, işte bir AS2 iletisi listesi aşağıdaki gibi görünmelidir:

   ![AS2 iletilerini görüntüleme](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Görüntülemek veya giriş ve çıkışları belirli iletileri dışarı aktarmak için bu iletileri seçip seçin **indirme**. İstendiğinde, .zip dosyasını yerel bilgisayarınıza kaydedin ve ardından bu dosyayı ayıklayın. 

   Ayıklanan klasörü, seçili her ileti için bir klasör içerir. 
   Onayları ayarlarsanız, ileti klasörünün bildirim ayrıntılarla dosyalarını da içerir. 
   Her ileti klasörünün en az bu dosyaları sahiptir: 
   
   * Kullanıcı tarafından okunabilen dosyalar giriş yükü ve çıkış yükü ayrıntıları
   * Giriş ve çıkışları ile kodlanmış dosyalar

   Her ileti türü için klasör ve dosya adı biçimleri burada bulabilirsiniz:

   * [AS2 klasör ve dosya adı biçimleri](#as2-folder-file-names)
   * [X12 klasör ve dosya adı biçimleri](#x12-folder-file-names)
   * [EDIFACT klasör ve dosya adı biçimleri](#edifact-folder-file-names)

   ![İleti dosyaları indirme](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Görüntülemek için aynı olan tüm Eylemler, üzerinde çalıştırma kimliği **günlük araması** sayfasında, bir ileti ileti listesinden seçin.

   Bu eylemler sütun veya arama belirli sonuçları sıralayabilirsiniz.

   ![Aynı eylemleri çalıştırma kimliği](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Arama sonuçları ile önceden oluşturulmuş sorguları tercih **Sık Kullanılanlar**.

   * Bilgi [filtreler ekleyerek sorguları oluşturmak nasıl](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Veya daha fazla bilgi edinin [Log Analytics'te günlük aramaları sahip verileri bulmak nasıl](../log-analytics/log-analytics-log-searches.md).

   * Arama kutusuna bir sorguyu değiştirmek için sütun ve filtre olarak kullanmak istediğiniz değerleri ile sorguyu güncelleştirin.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Özellik açıklamaları ve ad biçimlerinin AS2, X 12 ve EDIFACT iletileri

Her ileti türü için indirilen ileti dosyası adı biçimlerini ve özellik açıklamaları aşağıda verilmiştir.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>AS2 ileti özellik açıklamaları

Her bir AS2 iletisi için özellik açıklamaları aşağıda verilmiştir.

| Özellik | Açıklama |
| --- | --- |
| Gönderen | Belirtilen Konuk iş ortağı **alma ayarı**, ya da konak iş ortağı belirtilen **gönderme ayarları** AS2 anlaşması |
| Alıcı | Belirtilen konak iş ortağı **alma ayarı**, veya konuk iş ortağı belirtilen **gönderme ayarları** AS2 anlaşması |
| Logic App | Mantıksal uygulama AS2 eylemleri nerede ayarlanır |
| Durum | AS2 ileti durumu <br>Başarı alınan = ya da geçerli bir AS2 iletisi gönderdi. Hiçbir MDN ayarlayın. <br>Başarı alınan = ya da geçerli bir AS2 iletisi gönderdi. MDN ayarlama ve alınan veya MDN gönderilir. <br>Başarısız = geçersiz bir AS2 iletisi alındı. Hiçbir MDN ayarlayın. <br>Bekleyen alınan = ya da geçerli bir AS2 iletisi gönderdi. MDN ayarlanır ve MDN beklenmektedir. |
| ACK | MDN iletisini durumu <br>Kabul edilen bir pozitif MDN gönderilen veya alınan =. <br>Bekleyen bir MDN göndermek veya almak için bekliyor =. <br>Reddedilen bir negatif MDN gönderilen veya alınan =. <br>Gerekli değil = MDN ayarlanmadı anlaşmasında. |
| Yön | AS2 ileti yönü |
| Bağıntı Kimliği | Tüm mantıksal uygulama eylemleri ve Tetikleyicileri karşılık gelen kimliği |
| İleti Kimliği | AS2 iletisi başlıklarından AS2 ileti kimliği |
| Zaman damgası | Ne zaman AS2 eylem ileti işleme süresi |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>AS2 ad biçimlerinin indirilen ileti dosyası

Her indirilen AS2 iletisi klasör ve dosya adı biçimleri şunlardır.

| Klasör veya dosya | Ad biçimi |
| :------------- | :---------- |
| İleti klasörü | [Gönderen] \_[Alıcı]\_AS2\_[bağıntı kimliği]\_message-ID\_[timestamp] |
| Giriş, çıkış ve Eğer bildirim dosyalarını, ayarlayın | **Giriş yükü**: [Gönderen]\_[Alıcı]\_AS2\_[bağıntı kimliği]\_input_payload.txt </p>**Çıktı yükü**: [Gönderen]\_[Alıcı]\_AS2\_[bağıntı kimliği]\_çıkış\_payload.txt </p></p>**Girişleri**: [Gönderen]\_[Alıcı]\_AS2\_[bağıntı kimliği]\_inputs.txt </p></p>**Çıkışlar**: [Gönderen]\_[Alıcı]\_AS2\_[bağıntı kimliği]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Özellik açıklamaları X12 iletisi

Her X12 için özellik açıklamaları İşte ileti.

| Özellik | Açıklama |
| --- | --- |
| Gönderen | Belirtilen Konuk iş ortağı **alma ayarı**, ya da konak iş ortağı belirtilen **gönderme ayarları** x X12 için Sözleşmesi |
| Alıcı | Belirtilen konak iş ortağı **alma ayarı**, veya konuk iş ortağı belirtilen **gönderme ayarları** x X12 için Sözleşmesi |
| Logic App | Mantıksal uygulama yeri eylemleri ayarlandığı X12 |
| Durum | X12 durumu iletisi <br>Başarı alınan = ya da geçerli X12 gönderilen ileti. İşlevsel broker'dan bildirim alınmadı ayarlayın. <br>Başarı alınan = ya da geçerli X12 gönderilen ileti. İşlevsel ack ayarlama ve alınan ya da işlevsel bir bildirim gönderilir. <br>Başarısız alınan = ya da geçersiz x X12 gönderilen ileti. <br>Bekleyen alınan = ya da geçerli X12 gönderilen ileti. İşlevsel ack ayarlanır ve işlev bir onayı bekleniyor. |
| ACK | İşlevsel Ack (997) durumu <br>Kabul edilen pozitif bir işlev bildirim gönderilen veya alınan = <br>Reddedilen negatif bir işlev bildirim gönderilen veya alınan = <br>Bekleyen = işlevsel bir onayı bekleniyor ancak alınmadı. <br>İşlevsel bir ack = oluşturulan ancak gönderilemiyor iş ortağına. <br>Gerekli değil işlevsel = ack ayarlı değil. |
| Yön | X12 ileti yönü |
| Bağıntı Kimliği | Tüm mantıksal uygulama eylemleri ve Tetikleyicileri karşılık gelen kimliği |
| İleti türü | EDI X 12 iletisi türü |
| ICN | Değişim denetim numarası X12 için ileti |
| TSCN | İşlem kümesi denetim numarası için X12 iletisi |
| Zaman damgası | Zaman zaman X12 eylem işlenen ileti |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 biçimleri için indirilen ileti dosyası adı.

İşte adı biçimlerini X12 yüklenen her klasör ve dosyaları iletisi.

| Klasör veya dosya | Ad biçimi |
| :------------- | :---------- |
| İleti klasörü | [Gönderen] \_[Alıcı]\_X12\_[değişim denetim numarası]\_[genel-denetim-number zaman]\_[işlem-kümesi-denetim-number]\_[timestamp] |
| Giriş, çıkış ve Eğer bildirim dosyalarını, ayarlayın | **Giriş yükü**: [Gönderen]\_[Alıcı]\_X12\_[değişim denetim numarası]\_input_payload.txt </p>**Çıktı yükü**: [Gönderen]\_[Alıcı]\_X12\_[değişim denetim numarası]\_çıkış\_payload.txt </p></p>**Girişleri**: [Gönderen]\_[Alıcı]\_X12\_[değişim denetim numarası]\_inputs.txt </p></p>**Çıkışlar**: [Gönderen]\_[Alıcı]\_X12\_[değişim denetim numarası]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT iletisi özellik açıklamaları

Her EDIFACT iletisi için özellik açıklamaları aşağıda verilmiştir.

| Özellik | Açıklama |
| --- | --- |
| Gönderen | Belirtilen Konuk iş ortağı **alma ayarı**, ya da konak iş ortağı belirtilen **gönderme ayarları** EDIFACT sözleşmesi için |
| Alıcı | Belirtilen konak iş ortağı **alma ayarı**, veya konuk iş ortağı belirtilen **gönderme ayarları** EDIFACT sözleşmesi için |
| Logic App | Mantıksal uygulama EDIFACT eylemleri nerede ayarlanır |
| Durum | EDIFACT ileti durumu <br>Başarı alınan = ya da geçerli bir EDIFACT iletisi gönderdi. İşlevsel broker'dan bildirim alınmadı ayarlayın. <br>Başarı alınan = ya da geçerli bir EDIFACT iletisi gönderdi. İşlevsel ack ayarlama ve alınan ya da işlevsel bir bildirim gönderilir. <br>Başarısız geçersiz EDIFACT iletisine gönderilen veya alınan = <br>Bekleyen alınan = ya da geçerli bir EDIFACT iletisi gönderdi. İşlevsel ack ayarlanır ve işlev bir onayı bekleniyor. |
| ACK | İşlevsel Ack (997) durumu <br>Kabul edilen pozitif bir işlev bildirim gönderilen veya alınan = <br>Reddedilen negatif bir işlev bildirim gönderilen veya alınan = <br>Bekleyen = işlevsel bir onayı bekleniyor ancak alınmadı. <br>İşlevsel bir ack = oluşturulan ancak gönderilemiyor iş ortağına. <br>Gerekli değil = işlevsel Ack ayarlı değil. |
| Yön | EDIFACT ileti yönü |
| Bağıntı Kimliği | Tüm mantıksal uygulama eylemleri ve Tetikleyicileri karşılık gelen kimliği |
| İleti türü | EDIFACT iletisi türü |
| ICN | EDIFACT iletisi için değişim denetim numarası |
| TSCN | İşlem kümesi denetim numarası için EDIFACT iletisi |
| Zaman damgası | Ne zaman EDIFACT eylemi ileti işleme süresi |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>EDIFACT ad biçimlerinin indirilen ileti dosyası

Her indirilen EDIFACT iletisi klasör ve dosya adı biçimleri şunlardır.

| Klasör veya dosya | Ad biçimi |
| :------------- | :---------- |
| İleti klasörü | [Gönderen] \_[Alıcı]\_EDIFACT\_[değişim denetim numarası]\_[genel-denetim-number zaman]\_[işlem-kümesi-denetim-number]\_[timestamp] |
| Giriş, çıkış ve Eğer bildirim dosyalarını, ayarlayın | **Giriş yükü**: [Gönderen]\_[Alıcı]\_EDIFACT\_[değişim denetim numarası]\_input_payload.txt </p>**Çıktı yükü**: [Gönderen]\_[Alıcı]\_EDIFACT\_[değişim denetim numarası]\_çıkış\_payload.txt </p></p>**Girişleri**: [Gönderen]\_[Alıcı]\_EDIFACT\_[değişim denetim numarası]\_inputs.txt </p></p>**Çıkışlar**: [Gönderen]\_[Alıcı]\_EDIFACT\_[değişim denetim numarası]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Sonraki adımlar

* [Log analytics'te B2B iletileri için sorgulama](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel İzleme şemaları](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)