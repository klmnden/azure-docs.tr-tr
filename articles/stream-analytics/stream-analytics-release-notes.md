---
title: Azure akış analizi - sürüm notları
description: Bu makalede Azure akış analizi ve karşılık gelen Visual Studio Araçları sürüm geçmişini açıklanmaktadır.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/03/2017
ms.openlocfilehash: b5f6f4f42929127521320e56bcc9b36c324cde89
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="stream-analytics-release-notes"></a>Stream Analytics sürüm notları

## <a name="notes-for-06142017-update-of-stream-analytics-tools-for-visual-studio"></a>Stream Analytics araçları 06/14/2017 güncelleştirme Visual Studio için Notlar
Bu güncelleştirme için Visual Studio Araçları ' dir. Bu sürüm aşağıdaki yeni özellikleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Java komut dosyası Düzenleyicisi desteği |Komut dosyası işlevleri, java oluşturduktan sonra yerel java komut dosyası Düzenleyicisi deneyimi keyfini çıkarabilirsiniz.|
| Görünüm iş yürütme zamanı hata iletisi | İş yürütme sırasında çalışma zamanı hataları varsa, görüntü zaman penceresi ayarlayarak bunları hataları sekmesinde görüntüleyebilirsiniz. Varsayılan olarak son 30 dakika boyunca hata iletilerini gösterir. |
| Yerel test giriş CSV ve Avro desteği | JSON yanı sıra, artık yerel test girdi için CSV ve Avro dosya biçimi kullanabilir.|

## <a name="notes-for-05032017-update-of-stream-analytics"></a>Stream Analytics 03/05/2017 güncelleştirilmesi için Notlar
Bu güncelleştirme için sorun giderme belgelerine sürümüdür.

[Sorun giderme kılavuzu](stream-analytics-troubleshooting-guide.md) ve diğer belgeleri serbest bırakıldı. Bu kılavuzu gözden geçirin ve geri bildirim Hoş Geldiniz.

## <a name="notes-for-04242017-update-of-stream-analytics-tools-for-visual-studio"></a>Stream Analytics araçları 24/04/2017 güncelleştirme Visual Studio için Notlar
Bu güncelleştirme için Visual Studio Araçları ' dir. Bu sürüm aşağıdaki yeni özellikleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Visual Studio'da yerel test sonucu görüntüleyin | Yerel test çıktı sonucu görüntülemek için çıkış konsol penceresinde ENTER tuşuna basın veya kapatın. Sonuç, Visual Studio penceresinde, tablo biçiminde gösterilir. |
| Çıkış JSON biçiminde yerel sonucu | Yerel bir test çalıştırdığınızda, hem JSON hem de CSV dosya biçimleri gibi elde edilen sonucu üretilir. |
| BLOB/tablo depolama girdi/çıktı verilerini Önizleme | Bir blob veya tablo depolama iş görünümünde giriş/çıkış çift tıklayarak, Visual Studio içindeki verileri kolayca önizleyebilirsiniz. |
| Giriş/Çıkış hata iletisini görüntüleme | İşin girişleri veya çıkışları ilgili herhangi bir çalışma zamanı hata varsa, ayrıntılı hata iletisini görmek için Vurgu burada iş diyagramında gösterilir.|


## <a name="notes-for-02012017-release-of-stream-analytics"></a>Stream Analytics 02/01/2017 sürümünün notları
Bu sürüm aşağıdaki güncelleştirmeyi içerir:

| Başlık | Açıklama |
| --- | --- |
| JavaScript kullanıcı tanımlı işlevler (UDF) Tanıtımı |[Java kullanıcı tanımlı işlevler](stream-analytics-javascript-user-defined-functions.md) sorgular oluşturma ek esneklik için kullanıma sunulmuştur. |
| Visual Studio ve akış analizi için giriş araçları |[Visual Studio Araçları](stream-analytics-tools-for-visual-studio.md) artık için hata ayıklama ve büyük yardımcı programı kullanılabilir. |
| Tanılama günlük Tanıtımı |[Tanılama günlük](stream-analytics-job-diagnostic-logs.md) için ek sorun giderme seçenekleri kullanıma sunulmuştur. |
| Giriş Jeo-uzamsal işlevleri |[Jeo-uzamsal işlevleri](http://msdn.microsoft.com/library/mt778980(Azure.100).aspx) genel kullanıma sunulmuştur. |

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Akış analizi, 15/04/2016 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeyi içerir:

| Başlık | Açıklama |
| --- | --- |
| Power BI için genel kullanılabilirlik çıkarır |[Power BI çıkışınızın](stream-analytics-power-bi-dashboard.md) genel kullanıma sunulmuştur. Power BI için 90 günlük yetkilendirme sona erme kaldırılmıştır. Burada yetkilendirme gerekiyor yenilenmesi senaryoları hakkında daha fazla bilgi için bkz: [yetkilendirmeyi yenileyin](stream-analytics-power-bi-dashboard.md#renew-authorization) Power BI panosu oluşturma bölümü. |

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Akış analizi, 03/03/2016 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeyi içerir:

| Başlık | Açıklama |
| --- | --- |
| Yeni akış analizi sorgu dili öğeleri |SAQL artık sahiptir [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN sayfasını"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN sayfasını") ve [regexmatch'İŞLEVİNİN] (https://msdn.microsoft.com/library/azure/mt643891.aspx "Regexmatch'İŞLEVİNİN MSDN sayfasını"). |

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Akış analizi, 12/10/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeyi içerir:

| Başlık | Açıklama |
| --- | --- |
| REST API sürümü güncelleştirme |REST API sürümü 2015-10-01 güncelleştirildi. Ayrıntılar, MSDN üzerinde bulunabilir [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx) ve [akış analizi, Machine Learning tümleştirme](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md). |
| Azure Machine Learning tümleştirme |Azure Machine Learning kullanıcı tanımlı işlevler için destek bu sürüm ile birlikte gelir. Daha fazla bilgi için bkz: [öğretici](stream-analytics-machine-learning-integration-tutorial.md) ve [genel blog duyuru](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx). |

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Akış analizi, 11/12/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeyi içerir:

| Başlık | Açıklama |
| --- | --- |
| Yeni davranışını seçin |Stream Analytics Seç genişletilmişse izin vermek için * özellik erişimcisini iç içe geçmiş kaydı olarak. Daha fazla bilgi için bkz: [ http://msdn.microsoft.com/library/mt622759.aspx ] (http://msdn.microsoft.com/library/mt622759.aspx "karmaşık veri türlerini"). |

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Akış analizi, 22/10/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Ek sorgu dil özellikleri |Akış analizi genişletilmiş sorgu dili aşağıdaki özellikler dahil ederek: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [TAVAN](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [kat](https://msdn.microsoft.com/library/azure/mt605240.aspx), [ Güç](https://msdn.microsoft.com/library/azure/mt605287.aspx), [oturum](https://msdn.microsoft.com/library/azure/mt605290.aspx), [KARE](https://msdn.microsoft.com/library/azure/mt605288.aspx), ve [SQRT](https://msdn.microsoft.com/library/azure/mt605238.aspx). |
| Birleşik kısıtlamaları kaldırıldı |Bu sürüm 15 toplamalar sınırlandırılmasıdır sorguda kaldırır. Şimdi sorgu başına toplamalar sayısına bir sınır yoktur. |
| Eklenen grubu tarafından System.Timestamp özelliği |[GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) işlevi artık ya da window_type için izin verir veya [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx). |
| Dönen ve windows atlaması için eklenen UZAKLIĞI |Varsayılan olarak, [dönen](https://msdn.microsoft.com/library/azure/dn835055.aspx) ve [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) windows karşı süresini sıfır hizalı (1/1/0001 12:00:00 AM UTC). Yeni (isteğe bağlı) parametre `offsetsize` özel bir uzaklık (veya hizalama) belirtebilirsiniz. |

## <a name="notes-for-09292015-release-of-stream-analytics"></a>Akış analizi, 29/09/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Azure IOT paketi genel Önizleme |Akış analizi, Azure IOT paketi genel önizleme olarak dahil edilir. |
| Azure portal tümleştirme |Azure portalında sürekli varlığı yanı sıra, Stream Analytics şimdi tümleşik [Azure portal](https://azure.microsoft.com/overview/preview-portal/). Stream Analytics işlevselliği Önizleme portalında şu anda Power BI yapılandırması ve için gözatma veya yeni bir giriş ve çıkış oluşturma çıktı Azure portalında, tarayıcı içi sorgu testi, desteği olmadan işlevlerinin bir alt kümesini sunulan olduğuna dikkat edin kaynaklar Aboneliklerde erişebilirsiniz. |
| Cosmos DB çıkış desteği |Akış analizi işleri şimdi için çıktı [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). |
| IOT Hub giriş desteği |Akış analizi işleri, artık IOT hub'ları verileri işleyebilen. |
| Zaman damgası tarafından heterojen olayları için |Tek veri akışı zaman damgaları farklı alanlara sahip birden çok olay türünü içeriyorsa, artık kullanabilirsiniz [TIMESTAMP BY](http://msdn.microsoft.com/library/mt573293.aspx) her örneği için farklı bir zaman damgası alanları belirtin ifadelerle. |

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Akış analizi, 10/09/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Powerbı grupları için destek |Diğer Power BI kullanıcılarıyla veri paylaşımını etkinleştirmek için akış analizi işleri şimdi yazabilirler [Powerbı grupları](stream-analytics-define-outputs.md#power-bi) Power BI hesabınızı içinde. |

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Akış analizi, 20/08/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Eklenen son işlevi |[Son](http://msdn.microsoft.com/library/mt421186.aspx) işlevidir şimdi akış analizi işleri, etkinleştirme, belirli bir zaman çerçevesi içinde bir olay akışında en son olay almak kullanılabilir. |
| Yeni dizi işlevleri |Dizi işlevleri [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) ve [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) kullanıma sunulmuştur. |
| Yeni kayıt işlevleri |Kayıt işlevleri [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) ve [getrecordpropertyvalue'öğesinin](http://msdn.microsoft.com/library/mt270220.aspx) kullanıma sunulmuştur. |

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Akış analizi, 07/30/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Power BI kurum Azure kimliğinden ayrılmış kimliği |Bu özellik sağlar [Power BI çıkışı](stream-analytics-power-bi-dashboard.md) ASA işleri herhangi bir Azure hesabı türü (Live ID veya kurum kimliği) altında. Ayrıca, Azure hesabınız için bir kuruluş kimliğine sahip ve Power BI çıkışı yetkilendirmek için farklı bir tane kullanın. |
| Hizmet veri yolu kuyrukları çıkış desteği |[Hizmet veri yolu kuyrukları](stream-analytics-define-outputs.md#service-bus-queues) çıkışları Stream Analytics işlerine hazırdır. |
| Hizmet veri yolu konuları çıkış desteği |[Hizmet veri yolu konuları](stream-analytics-define-outputs.md#service-bus-topics) çıkışları Stream Analytics işlerine hazırdır. |

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Akış analizi, 07/09/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Özel Blob çıkış bölümlendirme |BLOB Depolama çıkışları BLOB'lar yazılır, çıkış ve yapısı ve çıktı veri yolu klasör yapısı biçimi sıklığını belirtmek için bir seçenek artık izin verir. |

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Akış analizi, 03/05/2015 sürüm notları
Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama |
| --- | --- |
| Artan en büyük değer sırası tolerans penceresi |Sırası tolerans penceresi için en büyük boyut artık 59:59 (dd: ss): |
| JSON çıktı biçimi: Ayrılmış satırı veya dizi |Şimdi Blob Depolama veya olay hub'ı ya da bir dizi olarak JSON nesnelerinin veya JSON nesnelerini içeren yeni bir satır ayırma tarafından çıktısını almak için alırken bir seçenek yoktur. |

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Akış analizi, 16/04/2015 sürüm notları
| Başlık | Açıklama |
| --- | --- |
| Azure depolama hesabı yapılandırmasında gecikme |Akış analizi işi ilk kez bir bölgede oluştururken, yeni bir depolama hesabı oluşturun veya bu bölgede Stream Analytics işlerini izleme için var olan bir hesap belirtin istenir. İzleme, izleme depolama hesabı açılan kısa süre önce yapılandırılmış bir gösteren yerine ikinci bir depolama hesabı belirtmek için istemi 30 dakika içinde aynı bölgede başka bir Stream Analytics işi oluşturma yapılandırmada gecikme nedeniyle. Gereksiz bir depolama hesabı oluşturmamak için bu bölgede başka işler sağlamadan önce ilk kez bir bölgede bir işi oluşturduktan sonra 30 dakika bekleyin. |
| Proje yükseltme |Şu anda Stream Analytics Canlı düzenlemeler tanımı veya çalışan bir işi yapılandırmasını desteklemez. Giriş, çıkış, sorgu, Ölçek veya çalışan bir işi yapılandırmasını değiştirmek için önce iş durdurmanız gerekir. |
| Giriş kaynağından çıkarımı yapılan veri türleri |CREATE TABLE deyimi kullanılmazsa, giriş biçiminden türetilmiş giriş türü, örneğin CSV tüm alanları dize. Açıkça alanları türü uyuşmazlığı hatalarını önlemek için CAST işlevi kullanarak sağ türüne dönüştürün. |
| Eksik alanların null değerler olarak yüzdelik |Giriş kaynakta mevcut değil bir alan başvuran çıktı olayını null değerleri sonuçlanır. |
| SELECT deyimi ifadelerle gelmelidir |Sorgunuzda, alt sorgular deyimleri ile tanımlanan SELECT deyimi izlemeniz gerekir. |
| Bellek sorunu |Düzen dışı olayları ve/veya yetersiz bellek, bir işin kaynaklanan çalıştırmak iş durumu, büyük bir miktarını neden olabilir koruma karmaşık sorgular için büyük bir tolerans ile akış analizi işi yeniden başlatın. Başlatma ve durdurma işlemleri iş işlemi günlüklerde görülebilir. Bu davranışı önlemek için birden çok bölüm arasında sorguyu dışarıya ölçeklendirin. Gelecekteki bir sürümde bu sınırlama, bunları yeniden başlatmak yerine etkilenen işlerini performansı önemli tarafından değinilmiştir. |
| Büyük blob girişleri yükü zaman damgası olmadan bellek yetersiz soruna neden olabilir |Zaman damgası alanı zaman damgası tarafından belirtilmezse, Blob depolama biriminden büyük dosyaları kullanma akış analizi işleri çökmesine neden olabilir. Bu sorunu önlemek için her bir blob altında 10 MB boyutunda tutun. |
| SQL veritabanı olay birim sınırlaması |SQL veritabanı bir çıktı hedefi olarak kullanırken, çıktı verilerini yüksek miktarda Stream Analytics işi zaman aşımına neden olabilir. Bu sorunu çözmek için toplamalar veya filtre işleçleri kullanarak çıktı miktarının azaltılmasını veya Azure Blob Depolama veya olay hub'ları bir çıktı hedefi olarak bunun yerine seçin. |
| Power BI veri kümeleri yalnızca bir tablo içerebilir |Powerbı sağlanan bir veri kümesinde birden fazla tablo desteklemiyor. |

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için denemenin [Azure akış analizi Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
