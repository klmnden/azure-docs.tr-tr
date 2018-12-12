---
title: Veri modelleme Azure zaman serisi öngörüleri | Microsoft Docs
description: Azure zaman serisi Öngörülerinde modelleme verileri anlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: dc6244b6e263d3fb963d40b2f0c626cdfa9ecff8
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873470"
---
# <a name="data-modeling-in-azure-time-series-insights"></a>Azure zaman serisi Öngörülerinde modelleme verileri

Bu belgede ile nasıl çalışılacağı açıklanmaktadır **zaman serisi modelleri** aşağıdaki Azure Time Series Insights (Önizleme). Bu, birkaç ortak veri senaryoları ayrıntıları.

Okuma [(Önizleme) Azure TSI Gezgini](./time-series-insights-update-explorer.md) güncelleştirme gezinme hakkında daha fazla bilgi için makalede.

## <a name="types"></a>Türler

### <a name="how-to-create-a-single-type"></a>Tek bir tür oluşturma

1. TSM model Seçici bölmenin başlığı olarak başlatın ve menüden türlerini seçin. Ardından, TSM türlerinde odaklanmak için Panel'i Daralt:

    ![portal_one][1]

1. **Ekle**'ye tıklayın.
1. Tüm Ayrıntılar türlerine ilişkin giriş ve tıklayın **Oluştur**. Bunun yapılması türleri ortamda oluşturmanız gerekir:

    ![portal_two][2]

### <a name="how-to-bulk-upload-one-or-more-types"></a>Toplu olarak bir veya daha fazla türlerini karşıya yükle

1. Tıklayarak **JSON karşıya**.
1. Tür yükünü içeren bu dosyayı seçin.
1. Tıklayarak **karşıya yükleme**

    ![portal_three][3]

### <a name="how-to-edit-a-single-type"></a>Tek bir tür düzenleme

* Türünü seçin ve tıklayın **Düzenle** düğmesi. Gerekli değişiklikleri yapın ve tıklayın **Kaydet**:

    ![portal_four][4]

### <a name="how-to-delete-a-type"></a>Bir tür silme

* Türünü seçin ve tıklayın **Sil** düğmesi. Hiçbir örnek türlerine ilişkiliyse, silinecek:

    ![portal_five][5]

## <a name="hierarchies"></a>Hiyerarşiler

### <a name="how-to-create-a-single-hierarchy"></a>Tek bir hiyerarşi oluşturma

1. TSM model Seçici bölmenin başlığı olarak başlatın ve hiyerarşileri menüden seçim yapın. Ardından, TSM türlerinde odaklanmak için Panel'i Daralt:

    ![portal_six][6]

1. Tıklayarak **Ekle**

    ![portal_seven][7]

1. Tıklayarak **ekleme düzeyi** sağ bölmede:

    ![portal_eight][8]

1. Hiyerarşi ayrıntılarını girin ve tıklayın **Oluştur**:

    ![portal_nine][9]

### <a name="how-to-bulk-upload-one-or-more-hierarchies"></a>Toplu olarak hiyerarşi bir veya daha fazla karşıya yükleme

1. Tıklayarak **karşıya JSON**.
1. Hiyerarşi yükünü içeren dosyayı seçin.
1. Tıklayarak **karşıya**:

    ![portal_ten][10]

### <a name="how-to-edit-a-single-hierarchy"></a>Tek bir hiyerarşi düzenleme

* Hiyerarşiyi seçin ve tıklayın **Düzenle** düğmesi. Gerekli değişiklikleri yapın ve tıklayın **Kaydet**:

    ![portal_eleven][11]

### <a name="how-to-delete-a-hierarchy"></a>Nasıl bir hiyerarşiyi Sil

* Hiyerarşiyi seçin ve tıklayın **Sil** düğmesi. Hiyerarşi için ilişkili hiçbir örneği silinir.

    ![portal_twelve][12]

## <a name="instances"></a>Örnekler

### <a name="how-to-create-a-single-instance"></a>Tek bir örneğini oluşturma

1. TSM model Seçici bölmenin başlığı olarak başlatın ve örnekleri menüden seçim yapın. Ardından, TSM türlerinde odaklanmak için Panel'i Daralt:

    ![portal_thirteen][13]

1. Tıklayarak **ekleme**:

    ![portal_fourteen][14]

1. Örnek ayrıntıları girin, türü ve hiyerarşi ilişkisini seçin ve tıklayın **Oluştur**.

### <a name="how-to-bulk-upload-one-or-more-instances"></a>Toplu olarak bir veya daha fazla karşıya yükleme

1. Tıklayarak **karşıya JSON**.
1. Örnekleri yükünü içeren dosyayı seçin:

    ![portal_fifteen][15]

1. Tıklayarak **karşıya**.

### <a name="how-to-edit-a-single-instance"></a>Tek bir örneği düzenlemek nasıl

* Örneğini seçin ve tıklayın **Düzenle** düğmesi. Gerekli değişiklikleri yapın ve tıklayın **Kaydet**:

    ![portal_sixteen][16]

### <a name="how-to-delete-an-instance"></a>Örneğini silme

* Örneğini seçin ve tıklayın **Sil** düğmesi. Hiçbir olay örnekleri ilişkili silinir.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [veri modelleme](./time-series-insights-update-tsm.md) hakkında daha fazla bilgi için **zaman serisi modelleri**.

(Önizleme) Azure TSI Gezgini görünümü [makale](./time-series-insights-update-explorer.md) önizlemesi hakkında daha fazla bilgi için.

Desteklenen JSON şekilleri hakkında bilgi edinmek [desteklenen JSON şekilleri](./time-series-insights-send-events.md#json).

<!-- Images -->
[1]: media/v2-update-how-to-tsm/portal_one.png
[2]: media/v2-update-how-to-tsm/portal_two.png
[3]: media/v2-update-how-to-tsm/portal_three.png
[4]: media/v2-update-how-to-tsm/portal_four.png
[5]: media/v2-update-how-to-tsm/portal_five.png
[6]: media/v2-update-how-to-tsm/portal_six.png
[7]: media/v2-update-how-to-tsm/portal_seven.png
[8]: media/v2-update-how-to-tsm/portal_eight.png
[9]: media/v2-update-how-to-tsm/portal_nine.png
[10]: media/v2-update-how-to-tsm/portal_ten.png
[11]: media/v2-update-how-to-tsm/portal_eleven.png
[12]: media/v2-update-how-to-tsm/portal_twelve.png
[13]: media/v2-update-how-to-tsm/portal_thirteen.png
[14]: media/v2-update-how-to-tsm/portal_fourteen.png
[15]: media/v2-update-how-to-tsm/portal_fifteen.png
[16]: media/v2-update-how-to-tsm/portal_sixteen.png