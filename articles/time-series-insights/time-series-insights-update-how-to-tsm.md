---
title: Azure zaman serisi öngörüleri önizlemesinde modelleme verileri | Microsoft Docs
description: Azure zaman serisi öngörüleri önizlemesinde modelleme verileri anlayın.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: df94290c5e62b898b6490c78ef0ae1ee79437240
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64716955"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Azure zaman serisi öngörüleri önizlemesinde modelleme verileri

Bu belge, Azure zaman serisi öngörüleri önizlemesi aşağıdaki zaman serisi modelleri ile çalışacak şekilde açıklar. Bu, birkaç ortak veri senaryoları ayrıntıları.

Güncelleştirme kullanma hakkında daha fazla bilgi edinmek için [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).

## <a name="types"></a>Türler

### <a name="create-a-single-type"></a>Tek bir tür oluşturma

1. Zaman serisi modelleri Seçici Masası'na gidin ve seçin **türleri** menüsünde. Zaman serisi modelleri türlerinde odaklanmak için Panel'i Daralt.

    ![Portal_one][1]

1. **Add (Ekle)** seçeneğini belirleyin.
1. Giriş türlerini ilgilidir ve seçin tüm ayrıntıları **Oluştur**. Bu eylem, ortamda türleri oluşturur.

    ![Portal_two][2]

### <a name="bulk-upload-one-or-more-types"></a>Toplu karşıya yükleme, bir veya daha fazla türleri

1. Seçin **karşıya JSON**.
1. Tür yükü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    ![Portal_three][3]

### <a name="edit-a-single-type"></a>Tek bir türünü Düzenle

Türü seçip **Düzenle**. Gerekli değişiklikleri yapın ve seçin **Kaydet**.

![Portal_four][4]

### <a name="delete-a-type"></a>Bir türünü Sil

Türü seçip **Sil**. Hiçbir örnek türleri ile ilişkili ise bu silinir.

![Portal_five][5]

## <a name="hierarchies"></a>Hiyerarşiler

### <a name="create-a-single-hierarchy"></a>Tek bir hiyerarşi oluşturun

1. Zaman serisi modelleri Seçici Masası'na gidin ve seçin **hiyerarşileri** menüsünde. Zaman serisi modelleri Hiyerarşiler odaklanmak için Panel'i Daralt.

    ![Portal_six][6]

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Portal_seven][7]

1. Seçin **ekleme düzeyi** sağ bölmede.

    ![Portal_eight][8]

1. Hiyerarşi ayrıntılarını girin ve seçin **Oluştur**.

    ![Portal_nine][9]

### <a name="bulk-upload-one-or-more-hierarchies"></a>Toplu karşıya yükleme, bir veya daha fazla hiyerarşiler

1. Seçin **karşıya JSON**.
1. Hiyerarşi yükü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    ![Portal_ten][10]

### <a name="edit-a-single-hierarchy"></a>Tek bir hiyerarşiyi Düzenle

Hiyerarşi seçip **Düzenle**. Gerekli değişiklikleri yapın ve seçin **Kaydet**.

![Portal_eleven][11]

### <a name="delete-a-hierarchy"></a>Bir hiyerarşiyi Sil

Hiyerarşi seçip **Sil**. Örnek hiyerarşi ile ilişkili ise bu silinir.

![Portal_twelve][12]

## <a name="instances"></a>Örnekler

### <a name="create-a-single-instance"></a>Tek bir örneğini oluşturma

1. Zaman serisi modelleri Seçici Masası'na gidin ve seçin **örnekleri** menüsünde. Zaman serisi modelleri örneklerinde odaklanmak için Panel'i Daralt.

    ![Portal_thirteen][13]

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Portal_fourteen][14]

1. Örnek ayrıntıları girin, türü ve hiyerarşi ilişkisi seçip **Oluştur**.

### <a name="bulk-upload-one-or-more-instances"></a>Toplu karşıya yükleme, bir veya daha fazla örnek

1. Seçin **karşıya JSON**.
1. Örnekleri yükü içeren dosyayı seçin.

    ![Portal_fifteen][15]

1. **Karşıya Yükle**’yi seçin.

### <a name="edit-a-single-instance"></a>Tek bir örnek Düzenle

Örnek seçip **Düzenle**. Gerekli değişiklikleri yapın ve seçin **Kaydet**.

![Portal_sixteen][16]

### <a name="delete-an-instance"></a>Örnek silme

Örnek seçip **Sil**. Hiçbir olay örnekleri ile ilişkili ise bu silinir.

## <a name="next-steps"></a>Sonraki adımlar

- Zaman serisi modelleri hakkında daha fazla bilgi için okuma [veri modelleme](./time-series-insights-update-tsm.md).

- Önizleme hakkında daha fazla bilgi edinmek için [Azure zaman serisi öngörüleri önizlemesi gezginde verileri görselleştirme](./time-series-insights-update-explorer.md).

- Desteklenen JSON şekilleri hakkında bilgi edinmek için [desteklenen JSON şekilleri](./time-series-insights-send-events.md#json).

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