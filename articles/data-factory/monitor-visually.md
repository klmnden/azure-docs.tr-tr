---
title: Azure veri fabrikalarını görsel olarak izleme | Microsoft Docs
description: Azure veri fabrikalarını görsel olarak izleme hakkında bilgi edinin
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: shlo
ms.openlocfilehash: df684860cd3d1b6a002a300682ca4c6398461ba6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60717070"
---
# <a name="visually-monitor-azure-data-factories"></a>Azure veri fabrikalarını görsel olarak izleme
Azure Data Factory, bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir. Azure Data Factory’yi kullanarak, farklı veri depolarından veri alabilen, Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyebilen/dönüştürebilen ve çıktı verilerini iş zekası (BI) uygulamaları tarafından kullanılabilmesi için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilen veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturup zamanlayabilirsiniz.

Bu hızlı başlangıçta, tek satırlık bir kod yazmadan Data Factory işlem hatlarını görsel olarak izleme öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="monitor-data-factory-pipelines"></a>Data Factory işlem hatlarını izleyin

Basit liste görünümü arabirimi ile işlem hattı ve etkinlik çalıştırmalarını izleme. Tüm çalıştırmalar yerel tarayıcının saat diliminde görüntülenir. Saat dilimini değiştirebilirsiniz ve tüm tarih saat alanları seçilen saat dilimine ek bileşen.  

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. Oturum [Azure portalında](https://portal.azure.com/).
3. Azure portalında oluşturulan bir veri fabrikası dikey penceresine gidin ve Data Factory görsel izleme deneyimi başlatmak için 'İzleme ve yönetme' kutucuğa tıklayın.

## <a name="monitor-pipeline-runs"></a>İşlem hattı çalıştırmalarını izleme
Data Factory v2 işlem hatlarınız için her işlem hattı çalıştırmasının gösterildiği liste görünümü. Dahil edilen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem Hattı Adı | İşlem hattının adı. |
| Eylemler | Tek eylem etkinlik çalıştırmalarını görüntülemek kullanılabilir. |
| Başlangıç çalıştırın | İşlem hattı çalıştırma başlangıç tarih saat (GG/AA/YYYY, ss: dd: SS AM/PM) |
| Süre | Çalışma süresi (ss) |
| Tetikleyen | El ile tetikleyici, zamanlama tetikleyicisi |
| Durum | Başarılı, sürüyor, başarısız |
| Parametreler | İşlem hattı çalıştırma parametreleri (ad, değer çiftleri) |
| Hata | İşlem hattı çalıştırma hatası (if/any) |
| Çalışma Kimliği | İşlem hattı çalıştırma kimliği |

![İşlem hattı çalıştırmalarını izleme](media/monitor-visually/pipeline-runs.png)

## <a name="monitor-activity-runs"></a>Etkinlik çalıştırmalarını izleme
Her işlem hattı çalıştırmasına karşılık gelen etkinlik çalıştırmalarının gösterildiği liste görünümü. Tıklayın **'Etkinlik çalıştırmaları'** simgenin altında **'Actions'** sütun etkinliğini görüntülemek için her işlem hattı çalıştırması için çalışır. Dahil edilen sütunlar:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| Etkinlik Adı | İşlem hattı içindeki etkinliğin adı. |
| Etkinlik Türü | Kopyalama, vb. HDInsightSpark, Hdınsighthive etkinliği türü. |
| Başlangıç çalıştırın | Etkinlik çalıştırma başlangıç tarih saat (GG/AA/YYYY, ss: dd: SS AM/PM) |
| Süre | Çalışma süresi (ss) |
| Durum | Başarılı, sürüyor, başarısız |
| Girdi | Etkinlik girişlerinde açıklayan bir JSON dizisi |
| Çıktı | Etkinlik çıktıları açıklayan bir JSON dizisi |
| Hata | Etkinlik çalıştırma hatası (if/any) |

![Etkinlik çalıştırmalarını izleme](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> ' Ye tıklamanız **'Yenile'** işlem hattı ve etkinlik çalıştırmaları listesini yenilemek için üstteki simgesi. Otomatik yenileme şu anda desteklenmiyor.

![Yenile](media/monitor-visually/refresh.png)

## <a name="select-a-data-factory-to-monitor"></a>İzlemek için bir veri fabrikası'nı seçin
Üzerine gelin **Data Factory** sol üstteki simgesi. İzleyebileceğiniz azure aboneliklerinin ve veri fabrikalarının listesini görmek için 'OK' simgesine tıklayın.

![Veri fabrikası seçme](media/monitor-visually/select-datafactory.png)

## <a name="configure-the-list-view"></a>Liste Görünümü yapılandırma

### <a name="apply-rich-ordering-and-filtering"></a>Zengin sıralama ve filtreleme uygulanır

İşlem hattı çalıştırma başlangıcı desc/artan düzende çalıştırır ve filtre ardışık düzen tarafından aşağıdaki sütunlar çalışır:

| **Sütun adı** | **Açıklama** |
| --- | --- |
| İşlem Hattı Adı | İşlem hattının adı. Seçenekler Hızlı filtreler 'Son 24 saat' için 'Son week', 'son 30 gün' eklemek veya özel bir tarih seçin. |
| Başlangıç çalıştırın | İşlem hattı çalıştırması başlatma tarih saat |
| Çalıştırma durumu | Filtre tarafından başarılı oldu, durum - başarısız oldu, devam eden çalıştırır. |

![Filtre](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Sütun ekleme veya kaldırma
Liste Görünümü üst bilgisine sağ tıklayın ve liste görünümünde gösterilmesini istediğiniz sütunları seçin

![Sütunlar](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Sütun genişliklerini ayarlama
Artırın ve liste görünümünde sütun genişliklerini, sütun üst bilgisinin üzerinde bekleyerek azaltın

## <a name="promote-user-properties-to-monitor"></a>İzlemek için kullanıcı özelliklerini Yükselt

İzleyebileceğiniz varlık haline gelebilmesi kullanıcı özelliği olarak herhangi bir işlem hattı etkinliği özelliği yükseltebilirsiniz. Örneğin, yükseltebilirsiniz **kaynak** ve **hedef** özelliklerini, işlem hattındaki kopyalama etkinliği kullanıcı özellikleri. Belirleyebilirsiniz **otomatik oluştur** oluşturulacak **kaynak** ve **hedef** kopyalama etkinliği için kullanıcı özellikleri.

![Kullanıcı özellikleri oluşturma](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Bu gibi durumlarda, en fazla 5 işlem hattı etkinlik özellikleri yalnızca kullanıcı özelliklerini yükseltebilirsiniz.

Kullanıcı özelliklerini oluşturduktan sonra bunları daha sonra izleme liste görünümlerinde izleyebilirsiniz. Kopyalama etkinliği için bir kaynak tablo adı ise, kaynak tablo adı etkinliğinde bir sütun listesi görünümü çalışırken izleyebilirsiniz.

![Etkinlik çalıştırmaları listesi olmadan kullanıcı özellikleri](media/monitor-visually/monitor-user-properties-image2.png)

![Kullanıcı özellikleri için sütunları için etkinlik çalıştırmaları listesi ekleme](media/monitor-visually/monitor-user-properties-image3.png)

![Kullanıcı özellikleri için sütunları ile etkinlik çalıştırmaları Listele](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="rerun-activities-inside-a-pipeline"></a>Bir işlem hattı içindeki etkinlikleri yeniden çalıştırın

Artık bir işlem hattı içindeki etkinlikleri yeniden çalıştırabilirsiniz. Tıklayın **etkinlik çalıştırmalarını görüntülemek** ve ardışık düzeninize, işlem hattını yeniden çalıştırmak istediğiniz hangi noktasından etkinliği seçin.

![Etkinlik çalıştırmalarını görüntüleme](media/monitor-visually/rerun-activities-image1.png)

![Bir etkinlik çalıştırması seçin](media/monitor-visually/rerun-activities-image2.png)

### <a name="view-rerun-history"></a>Geçmişi yeniden görüntüle

Tüm liste görünümünde işlem hattı çalıştırmalarını için yeniden çalıştırma geçmişini görüntüleyebilirsiniz.

![Geçmişi görüntüleme](media/monitor-visually/rerun-history-image1.png)

Ayrıca belirli bir işlem hattı çalıştırması için Geçmişi Görüntüle yeniden kullanabilirsiniz.

![Bir işlem hattı çalıştırma geçmişini görüntüle](media/monitor-visually/rerun-history-image2.png)

## <a name="guided-tours"></a>Kılavuzlu Tur
Sol alt köşesinde 'bilgi simgesine' ve 'Tur, işlem hattı ve etkinlik çalıştırmalarını izleme konusunda adım adım yönergeleri almak için Kılavuzlu' tıklayın.

![Kılavuzlu Tur](media/monitor-visually/guided-tours.png)

## <a name="feedback"></a>Geri Bildirim
Bize çeşitli özellikler veya karşılaşmış sorunları geri bildirim sağlamak için 'Geri' simgesine tıklayın.

![Geri Bildirim](media/monitor-visually/feedback.png)

## <a name="alerts"></a>Uyarılar

Data factory'de desteklenen ölçümler üzerinde uyarılar oluşturabilir. Seçin **İzleyicisi -> Uyarılar ve ölçümler** kullanmaya başlamak için veri fabrikası İzleyicisi sayfasında.

![](media/monitor-visually/alerts01.png)

Yedi dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Uyarı Oluştur

1.  Tıklayın **yeni uyarı kuralı** yeni bir uyarı oluşturmak için.

    ![](media/monitor-visually/alerts02.png)

1.  Kural adı belirtin ve uyarıyı seçin **önem derecesi**.

    ![](media/monitor-visually/alerts03.png)

1.  Uyarı ölçütünü seçin.

    ![](media/monitor-visually/alerts04.png)

    ![](media/monitor-visually/alerts05.png)

1.  Uyarı mantığı yapılandırın. Tüm işlem hatları ve ilgili etkinlikleri için seçilen ölçüm için bir uyarı oluşturabilirsiniz. Ayrıca, belirli bir etkinlik türü, etkinlik adı, işlem hattı adı veya bir başarısızlık türü seçebilirsiniz.

    ![](media/monitor-visually/alerts06.png)

1.  Yapılandırma **e-posta/SMS/anında iletme/ses** uyarı bildirimleri. Oluşturduğunuz veya mevcut bir **eylem grubu** için uyarı bildirimleri.

    ![](media/monitor-visually/alerts07.png)

    ![](media/monitor-visually/alerts08.png)

1.  Uyarı kuralı oluşturun.

    ![](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz [izleme ve işlem hatlarını programlama yoluyla yönetme](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) makalede izleme ve işlem hatlarını yönetme hakkında bilgi edinin.
