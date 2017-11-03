---
title: "Görsel olarak Azure data factory'leri izleme | Microsoft Docs"
description: "Azure Data Factory görsel olarak izleneceği hakkında bilgi edinin"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: shlo
ms.openlocfilehash: e3ddbb88453b3f5d5f8b4566cf91aadbefd8163f
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="visually-monitor-azure-data-factories"></a>Görsel olarak Azure data factory'leri izleme
Azure Data Factory, bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir. Azure Data Factory’yi kullanarak, farklı veri depolarından veri alabilen, Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyebilen/dönüştürebilen ve çıktı verilerini iş zekası (BI) uygulamaları tarafından kullanılabilmesi için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilen veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturup zamanlayabilirsiniz.
Bu Hızlı Başlangıç, veri fabrikası v2 ardışık tek satırlık bir kod yazmak zorunda kalmadan görsel olarak izlemek öğreneceksiniz.
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [İzleyici ve ardışık düzenlerinde Data Factory version1 yönetmek](v1/data-factory-monitor-manage-app.md).

## <a name="monitor-data-factory-v2-pipelines"></a>Veri Fabrikası v2 ardışık izleme

1. Oturum [Azure portal](https://portal.azure.com/).
2. Azure portalında oluşturulan veri fabrikası dikey penceresine gidin ve 'İzleyici & Yönet' kutucuğa tıklayın. Bu ADF v2 görsel izleme deneyimi başlatacak.

## <a name="list-view-monitoring"></a>Liste Görünümü İzleme

İşlem hattını izleme ve etkinlik, bir basit liste görünümü arabirimi ile çalışır. Tüm çalıştığı yerel tarayıcı saat diliminde gösterilir. Saat dilimini değiştirmek ve tüm tarih/saat alanlarını seçilen zaman dilimine Daya.  

#### <a name="monitoring-pipeline-runs"></a>Ardışık Düzen çalıştırır izleme
Liste görünümü her ardışık düzen birtakım sergileyen veri fabrikası v2 hatlarınızı için çalıştırın. Eklenen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Ardışık Düzen adı | İşlem hattının adı. |
| Eylemler | Tek eylem etkinliğini görüntülemek kullanılabilir çalıştırır. |
| Başlangıç çalıştırın | Tarih saat çalıştırma başlangıç kanal (GG/AA/YYYY, ss: dd: SS AM/PM) |
| Süre | Çalışma süresi (ss) |
| Tarafından tetiklendi | El ile tetikleyici, zamanlama tetikleyici |
| Durum | Başarısız oldu, başarılı, sürüyor |
| Parametreler | Ardışık Düzen parametreleri (ad, değer çiftleri) çalıştırın |
| Hata | Ardışık Düzen hatası (Eğer/any) çalıştırma |
| Kimliği çalıştırın | Çalıştırma ardışık kimliği |

![İşlem hattını izleme çalıştırır](media/monitor-visually/pipeline-runs.png)

#### <a name="monitoring-activity-runs"></a>İzleme etkinliği çalıştırır
Çalıştıran her ardışık düzen karşılık gelen etkinlik çalışması birtakım sergileyen liste görünümü. Tıklatın **'Etkinlik çalışır'** simgesi altında **'Eylemleri'** etkinliğini görüntülemek için sütun çalıştırmak için her potansiyel çalıştırır. Eklenen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Etkinlik adı | Etkinliği içinde ardışık düzen adı. |
| Etkinlik türü | Etkinlik türü yani kopyalama, HDInsightSpark, Hdınsighthive vs. |
| Başlangıç çalıştırın | Etkinlik Başlangıç tarih saat (GG/AA/YYYY, ss: dd: SS AM/PM) |
| Süre | Çalışma süresi (ss) |
| Durum | Başarısız oldu, başarılı, sürüyor |
| Girdi | Etkinlik girişlerinde tanımlayan JSON dizisi |
| Çıktı | Etkinlik çıkışları tanımlayan JSON dizisi |
| Hata | Hata (Eğer/any) Çalıştır etkinliği |

![Monitör etkinliği çalıştırır](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> ' Yi tıklamanız gerekir **'Yenile'** ardışık düzen ve etkinlik çalıştırmalarını listesini yenilemek için üstteki simgesi. Otomatik yenileme şu anda desteklenmiyor.
>

![Yenileme](media/monitor-visually/refresh.png)

## <a name="features"></a>Özellikler

#### <a name="rich-ordering-and-filtering"></a>Zengin sıralama ve filtreleme

Çalıştırma Start desc/asc sipariş ardışık düzen çalıştırır ve aşağıdaki sütunlara göre filtre ardışık düzen çalışır:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Ardışık Düzen adı | İşlem hattının adı. Seçenekler Hızlı filtreler 'Son 24 saat' için 'Son week', 'son 30 gün' içerir veya özel bir tarih seçin. |
| Başlangıç çalıştırın | Ardışık Düzen çalıştırma başlangıç tarih saat |
| Çalışma durumu | Filtre durumuna göre yani çalıştıran başarılı, başarısız, devam eden |

![Filtre](media/monitor-visually/filter.png)

#### <a name="addremove-columns-to-list-view"></a>Liste görünümü için Sütun Ekle/Kaldır
Liste Görünümü üstbilgisi sağ tıklayın ve liste görünümünde görünmesini istediğiniz sütunları seçin

![sütunları](media/monitor-visually/columns.png)

#### <a name="reorder-column-widths-in-list-view"></a>Liste görünümünde sütun genişliklerini yeniden Sırala
Artırma ve azaltma listesindeki sütun genişliklerini, sütun başlığını gelerek görüntüleyin

#### <a name="select-data-factory"></a>Veri Fabrikası seçin
Sol üst 'Veri fabrikası' simgesinde üzerine gelin. İzleyeceğiniz azure abonelikleri ve veri fabrikaları listesini görmek için 'OK' simgesine tıklayın.

![Veri Fabrikası seçin](media/monitor-visually/select-datafactory.png)

#### <a name="guided-tours"></a>Kılavuzlu Tur
'Bilgi simgeyi' sol alt ve ardışık düzen ve etkinlik çalışmalarınız izleme hakkında adım adım yönergeler alın ' tur destekli' tıklayın.

![Kılavuzlu Tur](media/monitor-visually/guided-tours.png)

#### <a name="feedback"></a>Geri Bildirim
Bize çeşitli özellikleri veya karşılıklı herhangi bir sorunla görüşlerinizi bildirmek için 'Geri' simgeyi tıklatın.

![Geri Bildirim](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [İzleyici ve ardışık düzen programlı olarak yönetmek](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) makalede izleme ve ardışık düzen yönetme hakkında bilgi edinin
