---
title: Operations Management Suite - Azure Logic Apps B2B iletilerini izlemek | Microsoft Docs
description: "B2B iletişimi tümleştirme hesabı ve mantığı uygulamalarınız için Azure günlük analizi ile Operations Management Suite (OMS) içinde izleme"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 128abd504785227c1f27debd329d46d358e6e516
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="track-b2b-communication-in-the-microsoft-operations-management-suite-oms"></a>İzleme B2B iletişimi Microsoft Operations Management Suite (OMS)

İkisi arasındaki B2B iletişim kurduktan sonra iş süreçlerini veya tümleştirme hesabınızı aracılığıyla uygulamaları çalıştıran bu varlıkların birbirleriyle iletileri değiştirebilir. Bu iletiler doğru olarak işlenir, AS2, X12, izleyebilir ve EDIFACT iletileri ile olup olmadığını denetlemek için [Azure günlük analizi](../log-analytics/log-analytics-overview.md) içinde [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Örneğin, iletileri izlemek için bu izleme web tabanlı özellikleri kullanabilirsiniz:

* İleti sayısı ve durumu
* İlgili kaynaklar durumu
* İlgili kaynaklar ile iletileri ilişkilendirmek
* Hataları için ayrıntılı hata açıklaması
* Arama özellikleri

## <a name="requirements"></a>Gereksinimler

* Tanılama Günlüğü ile ayarlanmış bir mantıksal uygulama. Bilgi [bir mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md) ve [bu mantıksal uygulama için günlük kaydını ayarlamak nasıl](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* İzleme ve günlük ile ayarlanan bir tümleştirme hesabı. Bilgi [tümleştirme hesap oluşturmak nasıl](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve [izlemeyi ve o hesap için günlüğe kaydetmeyi nasıl ayarlanacağını](../logic-apps/logic-apps-monitor-b2b-message.md).

* Henüz yapmadıysanız [günlük analizi için tanılama veri yayımlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) OMS içinde.

> [!NOTE]
> Önceki gereksinimlerini karşılamanızın sonra bir çalışma alanı olmalıdır [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). OMS, B2B iletişiminde izlemek için aynı OMS çalışma kullanmanız gerekir. 
>  
> Bir OMS çalışma alanı yoksa, bilgi [bir OMS çalışma alanı oluşturmak nasıl](../log-analytics/log-analytics-get-started.md).

## <a name="add-the-logic-apps-b2b-solution-to-the-operations-management-suite-oms"></a>Operations Management Suite (OMS) Logic Apps B2B çözümü ekleyin

OMS B2B iletileri mantıksal uygulamanızı izlemek için eklemelisiniz **Logic Apps B2B** OMS portalı çözüme. Daha fazla bilgi edinmek [çözümleri için OMS ekleme](../log-analytics/log-analytics-get-started.md).

1. İçinde [Azure portal](https://portal.azure.com), seçin **daha Hizmetleri**. "İçin günlük analizi" araması yapın ve ardından **günlük analizi** aşağıda gösterildiği gibi:

   ![Günlük analizi Bul](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. Altında **günlük analizi**, bulma ve OMS çalışma alanınızı seçin. 

   ![OMS çalışma alanınızı seçin](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Altında **Yönetim**, seçin **OMS portalı**.

   ![OMS portalı seçin](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. OMS giriş sayfası açıldıktan sonra Seç **Çözümleri Galerisi**.    

   ![Çözümleri Galerisi seçin](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. Altında **tüm çözümleri**, bulun ve seçin **Logic Apps B2B**.     

   ![Logic Apps B2B seçin](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. Altında **Logic Apps B2B**, seçin **Ekle**.

   ![Seçin ekleme](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

   Bölme için OMS giriş sayfasında **Logic Apps B2B iletileri** artık görünür. 
   B2B iletilerinizi işlendiğinde bu kutucuğu ileti sayısı güncelleştirir.

   ![OMS giriş sayfası, Logic Apps B2B iletileri döşeme](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-the-operations-management-suite"></a>İleti durumu ve Operations Management Suite ayrıntılarında izleme

1. B2B iletilerinizi işlendikten sonra durumu ve bu iletileri ayrıntılarını görüntüleyebilirsiniz. OMS giriş sayfasında, **Logic Apps B2B iletileri** döşeme.

   ![Güncelleştirilmiş ileti sayısı](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

   > [!NOTE]
   > Varsayılan olarak, **Logic Apps B2B iletileri** döşeme tek bir günde göre verileri gösterir. Farklı bir zaman aralığı için veri kapsamını değiştirmek için sayfanın üst kısmındaki OMS kapsam denetimi seçin:
   > 
   > ![Veri Kapsamı Değiştir](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)
   >

2. İleti sonra durum Panosu görüntülenir, tek bir gün bazında verileri gösteren bir belirli bir ileti türü için daha fazla bilgi görüntüleyebilirsiniz. Bölme için seçin **AS2**, **X12**, veya **EDIFACT**.

   ![İleti durumunu görüntüle](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Seçilen bölme için iletilerinin bir listesi görüntülenir. 
   Her ileti türü özellikleri hakkında daha fazla bilgi için bu ileti özelliği açıklamalar bakın:

   * [AS2 ileti özellikleri](#as2-message-properties)
   * [X12 ileti özelliklerinin](#x12-message-properties)
   * [EDIFACT ileti özellikleri](#EDIFACT-message-properties)

   Örneğin, işte AS2 ileti listesi aşağıdaki gibi görünmelidir:

   ![AS2 iletilerini görüntüleme](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Görüntülemek veya girişleri ve çıkışları belirli iletileri için dışarı aktarmak için bu iletileri seçin ve Seç **karşıdan**. İstendiğinde, .zip dosyasını yerel bilgisayarınıza kaydedin ve ardından bu dosyasını ayıklayın. 

   Ayıklanan klasörü, seçilen her ileti için bir klasör içerir. 
   Onayları ayarlarsanız, ileti klasörünün bildirim ayrıntılarla dosyalarını da içerir. 
   Her ileti klasörün en az bu dosyaları vardır: 
   
   * Giriş yükü ve çıkış yükü ayrıntıları okunabilir dosyaları
   * Girişleri ve çıkışları ile kodlanmış dosyalar

   Her ileti türü için klasör ve dosya adı biçimlerini burada bulabilirsiniz:

   * [AS2 klasör ve dosya adı biçimleri](#as2-folder-file-names)
   * [X12 klasör ve dosya adı biçimleri](#x12-folder-file-names)
   * [EDIFACT klasör ve dosya adı biçimleri](#edifact-folder-file-names)

   ![İleti dosyaları indirme](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Görüntülemek için aynı olan tüm eylemler kimliği, çalışma **günlük arama** sayfasında, bir ileti ileti listesinden seçin.

   Bu eylemler sütun ya da belirli sonuçları Ara sıralayabilirsiniz.

   ![Kimliği aynı eylemleri çalıştır](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Arama sonuçları önceden oluşturulmuş sorguları tercih **Sık Kullanılanlar**.

   * Bilgi [filtreleri ekleyerek sorguları oluşturmak nasıl](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Veya daha fazla bilgi edinmek [günlük aramaları verilerle günlük analizi bulmak nasıl](../log-analytics/log-analytics-log-searches.md).

   * Arama kutusuna sorgu değiştirmek için sütun ve filtre olarak kullanmak istediğiniz değerleri içeren sorguyu güncelleştirin.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Özellik açıklamaları adı için ve biçimleri AS2, X 12 ve EDIFACT iletileri

Her ileti türü için özellik açıklamaları ve indirilen ileti dosyaları için ad biçimleri aşağıda verilmiştir.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>AS2 ileti özellik açıklamaları

Burada, her AS2 ileti için özellik açıklamaları bulunmaktadır.

| Özellik | Açıklama |
| --- | --- |
| Gönderen | Belirtilen Konuk iş ortağı **alma ayarı**, veya ana makine ortak belirtilen **gönderme ayarları** AS2 sözleşmesi için |
| Alıcı | Belirtilen ana bilgisayar iş ortağı **alma ayarı**, veya konuk iş ortağı belirtilen **gönderme ayarları** AS2 sözleşmesi için |
| Logic App | AS2 Eylemler Burada ayarlanan mantıksal uygulama |
| Durum | AS2 ileti durumu <br>Başarı alınan = ya da geçerli bir AS2 iletisi gönderdi. Hiçbir MDN ayarlayın. <br>Başarı alınan = ya da geçerli bir AS2 iletisi gönderdi. MDN ayarlama ve alınan veya MDN gönderilir. <br>Başarısız alınan geçersiz bir AS2 ileti =. Hiçbir MDN ayarlayın. <br>Bekleyen alınan = ya da geçerli bir AS2 iletisi gönderdi. MDN ayarlama ve MDN beklenir. |
| ACK | MDN ileti durumu <br>Kabul alınan = ya da pozitif MDN gönderilir. <br>Bekleyen alma ya da bir MDN göndermek için bekleyen =. <br>Reddedilen alınan = ya da negatif MDN gönderilir. <br>Gerekli değil = MDN ayarlı değil anlaşmasında. |
| Yön | AS2 ileti yönü |
| Bağıntı Kimliği | Tüm tetikleyiciler ve Eylemler bir mantıksal uygulama içinde karşılık gelen kimliği |
| İleti Kimliği | AS2 ileti üstbilgilerini AS2 ileti kimliği |
| Zaman damgası | Ne zaman AS2 eylem ileti işleme süresi |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>İndirilen ileti dosyaları için AS2 adı biçimleri

Her indirilen AS2 ileti klasör ve dosya adı biçimler şunlardır.

| Klasör veya dosya | Ad biçimi |
| :------------- | :---------- |
| İleti klasörü | [Gönderen] \_[Alıcı]\_AS2\_[correlation-ID]\_[ileti kimliği]\_[zaman damgası] |
| Giriş, çıkış ve bildirim dosyaları, ayarlanmışsa | **Giriş yükü**: [Gönderen]\_[Alıcı]\_AS2\_[correlation-ID]\_input_payload.txt </p>**Çıktı yükü**: [Gönderen]\_[Alıcı]\_AS2\_[correlation-ID]\_çıkış\_payload.txt </p></p>**Girişleri**: [Gönderen]\_[Alıcı]\_AS2\_[correlation-ID]\_inputs.txt </p></p>**Çıkış**: [Gönderen]\_[Alıcı]\_AS2\_[correlation-ID]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Özellik açıklamaları X12 iletisi

Her X12 yönelik özellik açıklamalarını İşte ileti.

| Özellik | Açıklama |
| --- | --- |
| Gönderen | Belirtilen Konuk iş ortağı **alma ayarı**, veya ana makine ortak belirtilen **gönderme ayarları** bir X12 için Sözleşmesi |
| Alıcı | Belirtilen ana bilgisayar iş ortağı **alma ayarı**, veya konuk iş ortağı belirtilen **gönderme ayarları** bir X12 için Sözleşmesi |
| Logic App | Mantıksal uygulama burada Eylemler ayarlandığı X12 |
| Durum | X12 durum iletisi <br>Başarı alınan = ya da geçerli X12 gönderilen ileti. Hiçbir işlev ack ayarlayın. <br>Başarı alınan = ya da geçerli X12 gönderilen ileti. İşlev ack ayarlama ve alınan veya işlevsel bir ack gönderilir. <br>Başarısız alınan = ya da geçersiz bir X12 gönderilen ileti. <br>Bekleyen alınan = ya da geçerli X12 gönderilen ileti. İşlev ack ayarlama ve işlevsel bir ack beklenir. |
| ACK | İşlev Ack (997) durumu <br>Kabul alınan = ya da pozitif bir işlev bildirim gönderildi <br>Reddedilen alınan = veya negatif bir işlev bildirim gönderildi <br>Bekleyen bir işlev onayı bekleniyor = ancak alınmadı. <br>İşlevsel bir ack = oluşturulan ancak gönderemez ortağına. <br>Gerekli değil işlevsel = ack ayarlı değil. |
| Yön | X12 ileti yönü |
| Bağıntı Kimliği | Tüm tetikleyiciler ve Eylemler bir mantıksal uygulama içinde karşılık gelen kimliği |
| Msg türü | EDI X 12 ileti türü |
| ICN | X12 Değişim Denetimi numaralı ileti |
| TSCN | İşlem kümesi denetim numarası X12 iletisi |
| Zaman damgası | Zaman zaman X12 eylem işlenen ileti |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 biçimleri için indirilen ileti dosyası adı

Ad biçimler şunlardır her X12 yüklenen ileti klasör ve dosyaları.

| Klasör veya dosya | Ad biçimi |
| :------------- | :---------- |
| İleti klasörü | [Gönderen] \_[Alıcı]\_X12\_[Değişim Denetimi numarası]\_[genel denetim-numarası]\_[işlem-set-denetim-number]\_[zaman damgası] |
| Giriş, çıkış ve bildirim dosyaları, ayarlanmışsa | **Giriş yükü**: [Gönderen]\_[Alıcı]\_X12\_[Değişim Denetimi numarası]\_input_payload.txt </p>**Çıktı yükü**: [Gönderen]\_[Alıcı]\_X12\_[Değişim Denetimi numarası]\_çıkış\_payload.txt </p></p>**Girişleri**: [Gönderen]\_[Alıcı]\_X12\_[Değişim Denetimi numarası]\_inputs.txt </p></p>**Çıkış**: [Gönderen]\_[Alıcı]\_X12\_[Değişim Denetimi numarası]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT ileti özellik açıklamaları

Burada, her EDIFACT ileti için özellik açıklamaları bulunmaktadır.

| Özellik | Açıklama |
| --- | --- |
| Gönderen | Belirtilen Konuk iş ortağı **alma ayarı**, veya ana makine ortak belirtilen **gönderme ayarları** EDIFACT sözleşmesi için |
| Alıcı | Belirtilen ana bilgisayar iş ortağı **alma ayarı**, veya konuk iş ortağı belirtilen **gönderme ayarları** EDIFACT sözleşmesi için |
| Logic App | EDIFACT Eylemler Burada ayarlanan mantıksal uygulama |
| Durum | EDIFACT ileti durumu <br>Başarı alınan = ya da geçerli bir EDIFACT iletisi gönderdi. Hiçbir işlev ack ayarlayın. <br>Başarı alınan = ya da geçerli bir EDIFACT iletisi gönderdi. İşlev ack ayarlama ve alınan veya işlevsel bir ack gönderilir. <br>Başarısız alınan = ya da geçersiz bir EDIFACT ileti gönderildi <br>Bekleyen alınan = ya da geçerli bir EDIFACT iletisi gönderdi. İşlev ack ayarlama ve işlevsel bir ack beklenir. |
| ACK | İşlev Ack (997) durumu <br>Kabul alınan = ya da pozitif bir işlev bildirim gönderildi <br>Reddedilen alınan = veya negatif bir işlev bildirim gönderildi <br>Bekleyen bir işlev onayı bekleniyor = ancak alınmadı. <br>İşlevsel bir ack = oluşturulan ancak gönderemez ortağına. <br>Gerekli değil = işlevsel Ack ayarlı değil. |
| Yön | EDIFACT ileti yönü |
| Bağıntı Kimliği | Tüm tetikleyiciler ve Eylemler bir mantıksal uygulama içinde karşılık gelen kimliği |
| Msg türü | EDIFACT ileti türü |
| ICN | EDIFACT ileti Değişim Denetimi numarası |
| TSCN | EDIFACT iletisi için işlem kümesi denetim numarası |
| Zaman damgası | Ne zaman EDIFACT eylem ileti işleme süresi |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>İndirilen ileti dosyaları için EDIFACT adı biçimleri

Her indirilen EDIFACT ileti klasör ve dosya adı biçimler şunlardır.

| Klasör veya dosya | Ad biçimi |
| :------------- | :---------- |
| İleti klasörü | [Gönderen] \_[Alıcı]\_EDIFACT\_[Değişim Denetimi numarası]\_[genel denetim-numarası]\_[işlem-set-denetim-number]\_[zaman damgası] |
| Giriş, çıkış ve bildirim dosyaları, ayarlanmışsa | **Giriş yükü**: [Gönderen]\_[Alıcı]\_EDIFACT\_[Değişim Denetimi numarası]\_input_payload.txt </p>**Çıktı yükü**: [Gönderen]\_[Alıcı]\_EDIFACT\_[Değişim Denetimi numarası]\_çıkış\_payload.txt </p></p>**Girişleri**: [Gönderen]\_[Alıcı]\_EDIFACT\_[Değişim Denetimi numarası]\_inputs.txt </p></p>**Çıkış**: [Gönderen]\_[Alıcı]\_EDIFACT\_[Değişim Denetimi numarası]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Sonraki adımlar

* [Operations Management Suite B2B iletiler için sorgu](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel İzleme şemaları](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)