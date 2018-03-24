---
title: Görsel olarak Azure data factory'leri izleme | Microsoft Docs
description: Azure Data Factory görsel olarak izleneceği hakkında bilgi edinin
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: shlo
ms.openlocfilehash: b67c384ffd04176653ad434d39361ee67dc1ffea
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="visually-monitor-azure-data-factories"></a>Görsel olarak Azure data factory'leri izleme
Azure Data Factory, bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir. Azure Data Factory’yi kullanarak, farklı veri depolarından veri alabilen, Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyebilen/dönüştürebilen ve çıktı verilerini iş zekası (BI) uygulamaları tarafından kullanılabilmesi için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilen veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturup zamanlayabilirsiniz.
Bu Hızlı Başlangıç, veri fabrikası v2 ardışık tek satırlık bir kod yazmak zorunda kalmadan görsel olarak izlemek öğreneceksiniz.
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [İzleyici ve ardışık düzenlerinde Data Factory version1 yönetmek](v1/data-factory-monitor-manage-app.md).

## <a name="monitor-data-factory-v2-pipelines"></a>Veri Fabrikası v2 ardışık izleme

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. Oturum [Azure portal](https://portal.azure.com/).
3. Azure portalında oluşturulan veri fabrikası dikey penceresine gidin ve 'İzleyici & Yönet' kutucuğa tıklayın. Bu ADF v2 görsel izleme deneyimi başlatacak.

## <a name="list-view-monitoring"></a>Liste Görünümü İzleme

İşlem hattını izleme ve etkinlik, bir basit liste görünümü arabirimi ile çalışır. Tüm çalıştırmalar yerel tarayıcının saat diliminde görüntülenir. Saat dilimini değiştirebilirsiniz ve tüm tarih saat alanları seçilen saat dilimine uygun hale getirilir.  

#### <a name="monitoring-pipeline-runs"></a>Ardışık Düzen çalıştırır izleme
Data Factory v2 işlem hatlarınız için her işlem hattı çalıştırmasının gösterildiği liste görünümü. Eklenen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem Hattı Adı | İşlem hattının adı. |
| Eylemler | Tek eylem etkinliğini görüntülemek kullanılabilir çalıştırır. |
| Başlangıç çalıştırın | Tarih saat çalıştırma başlangıç kanal (GG/AA/YYYY, ss: dd: SS AM/PM) |
| Süre | Çalışma süresi (ss) |
| Tetikleyen | El ile tetikleyici, zamanlama tetikleyici |
| Durum | Başarısız oldu, başarılı, sürüyor |
| Parametreler | Ardışık Düzen parametreleri (ad, değer çiftleri) çalıştırın |
| Hata | Ardışık Düzen hatası (Eğer/any) çalıştırma |
| Çalışma Kimliği | Çalıştırma ardışık kimliği |

![İşlem hattı çalıştırmalarını izleme](media/monitor-visually/pipeline-runs.png)

#### <a name="monitoring-activity-runs"></a>İzleme etkinliği çalıştırır
Her işlem hattı çalıştırmasına karşılık gelen etkinlik çalıştırmalarının gösterildiği liste görünümü. Tıklatın **'Etkinlik çalışır'** simgesi altında **'Eylemleri'** etkinliğini görüntülemek için sütun çalıştırmak için her potansiyel çalıştırır. Eklenen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Etkinlik Adı | Etkinliği içinde ardışık düzen adı. |
| Etkinlik Türü | Etkinlik türü yani kopyalama, HDInsightSpark, Hdınsighthive vs. |
| Başlangıç çalıştırın | Etkinlik Başlangıç tarih saat (GG/AA/YYYY, ss: dd: SS AM/PM) |
| Süre | Çalışma süresi (ss) |
| Durum | Başarısız oldu, başarılı, sürüyor |
| Girdi | Etkinlik girişlerinde tanımlayan JSON dizisi |
| Çıktı | Etkinlik çıkışları tanımlayan JSON dizisi |
| Hata | Hata (Eğer/any) Çalıştır etkinliği |

![Etkinlik çalıştırmalarını izleme](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> ' Yi tıklamanız gerekir **'Yenile'** ardışık düzen ve etkinlik çalıştırmalarını listesini yenilemek için üstteki simgesi. Otomatik yenileme şu anda desteklenmiyor.
>

![Yenile](media/monitor-visually/refresh.png)

## <a name="features"></a>Özellikler

#### <a name="rich-ordering-and-filtering"></a>Zengin sıralama ve filtreleme

Çalıştırma Start desc/asc sipariş ardışık düzen çalıştırır ve aşağıdaki sütunlara göre filtre ardışık düzen çalışır:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem Hattı Adı | İşlem hattının adı. Seçenekler Hızlı filtreler 'Son 24 saat' için 'Son week', 'son 30 gün' içerir veya özel bir tarih seçin. |
| Başlangıç çalıştırın | Ardışık Düzen çalıştırma başlangıç tarih saat |
| Çalışma durumu | Filtre durumuna göre yani çalıştıran başarılı, başarısız, devam eden |

![Filtre](media/monitor-visually/filter.png)

#### <a name="addremove-columns-to-list-view"></a>Liste görünümüne sütunları ekleme/kaldırma
Liste Görünümü üstbilgisi sağ tıklayın ve liste görünümünde görünmesini istediğiniz sütunları seçin

![Sütunlar](media/monitor-visually/columns.png)

#### <a name="reorder-column-widths-in-list-view"></a>Liste görünümünde sütun genişliklerini yeniden Sırala
Artırma ve azaltma listesindeki sütun genişliklerini, sütun başlığını gelerek görüntüleyin

#### <a name="select-data-factory"></a>Veri fabrikası seçme
Sol üst 'Veri fabrikası' simgesinde üzerine gelin. İzleyeceğiniz azure abonelikleri ve veri fabrikaları listesini görmek için 'OK' simgesine tıklayın.

![Veri fabrikası seçme](media/monitor-visually/select-datafactory.png)

#### <a name="guided-tours"></a>Kılavuzlu Tur
'Bilgi simgeyi' sol alt ve ardışık düzen ve etkinlik çalışmalarınız izleme hakkında adım adım yönergeler alın ' tur destekli' tıklayın.

![Kılavuzlu Tur](media/monitor-visually/guided-tours.png)

#### <a name="feedback"></a>Geri Bildirim
Bize çeşitli özellikleri veya karşılıklı herhangi bir sorunla görüşlerinizi bildirmek için 'Geri' simgeyi tıklatın.

![Geri Bildirim](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [İzleyici ve ardışık düzen programlı olarak yönetmek](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) makalede izleme ve ardışık düzen yönetme hakkında bilgi edinin
