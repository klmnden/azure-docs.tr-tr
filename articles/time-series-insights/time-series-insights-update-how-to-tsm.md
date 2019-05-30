---
title: Azure zaman serisi öngörüleri önizlemesinde modelleme verileri | Microsoft Docs
description: Azure zaman serisi öngörüleri önizlemesinde modelleme verileri anlayın.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 73384868deb8f0e33b233e363c42a12adbcbe402
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237555"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Azure zaman serisi öngörüleri önizlemesinde modelleme verileri

Bu belge, Azure zaman serisi öngörüleri önizlemesi aşağıdaki zaman serisi modelleri ile çalışacak şekilde açıklar. Bu, birkaç ortak veri senaryoları ayrıntıları.

Güncelleştirme kullanma hakkında daha fazla bilgi edinmek için [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).

## <a name="types"></a>Türleri

### <a name="create-a-single-type"></a>Tek bir tür oluşturma

1. Zaman serisi modelleri Seçici Masası'na gidin ve seçin **türleri** menüsünde. Zaman serisi modelleri türlerinde odaklanmak için Panel'i Daralt.

    [![Tek bir tür oluşturma](media/v2-update-how-to-tsm/portal_one.png)](media/v2-update-how-to-tsm/portal_one.png#lightbox)

1. **Add (Ekle)** seçeneğini belirleyin.
1. Giriş türlerini ilgilidir ve seçin tüm ayrıntıları **Oluştur**. Bu eylem, ortamda türleri oluşturur.

    [![Bir türü Ekle](media/v2-update-how-to-tsm/portal_two.png)](media/v2-update-how-to-tsm/portal_two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Toplu karşıya yükleme, bir veya daha fazla türleri

1. Seçin **karşıya JSON**.
1. Tür yükü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [![JSON karşıya yükleme](media/v2-update-how-to-tsm/portal_three.png)](media/v2-update-how-to-tsm/portal_three.png#lightbox)

### <a name="edit-a-single-type"></a>Tek bir türünü Düzenle

1. Türü seçip **Düzenle**. 
1. Gerekli değişiklikleri yapın ve seçin **Kaydet**.

    [![Türü Düzenle](media/v2-update-how-to-tsm/portal_four.png)](media/v2-update-how-to-tsm/portal_four.png#lightbox)

### <a name="delete-a-type"></a>Bir türünü Sil

1. Türü seçip **Sil**.
1. Hiçbir örnek türleri ile ilişkili ise bu silinir.

    [![Bir türünü Sil](media/v2-update-how-to-tsm/portal_five.png)](media/v2-update-how-to-tsm/portal_five.png#lightbox)

## <a name="hierarchies"></a>Hiyerarşiler

### <a name="create-a-single-hierarchy"></a>Tek bir hiyerarşi oluşturun

1. Zaman serisi modelleri Seçici Masası'na gidin ve seçin **hiyerarşileri** menüsünde. Zaman serisi modelleri Hiyerarşiler odaklanmak için Panel'i Daralt.

    [![Hiyerarşileri seçin](media/v2-update-how-to-tsm/portal_six.png)](media/v2-update-how-to-tsm/portal_six.png#lightbox)

1. **Add (Ekle)** seçeneğini belirleyin.

    [![Bir hiyerarşi Ekle](media/v2-update-how-to-tsm/portal_seven.png)](media/v2-update-how-to-tsm/portal_seven.png#lightbox)

1. Seçin **ekleme düzeyi** sağ bölmede.

    [![Bir düzey Ekle](media/v2-update-how-to-tsm/portal_eight.png)](media/v2-update-how-to-tsm/portal_eight.png#lightbox)

1. Hiyerarşi ayrıntılarını girin ve seçin **Oluştur**.

    [![Bir düzey oluşturun](media/v2-update-how-to-tsm/portal_nine.png)](media/v2-update-how-to-tsm/portal_nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Toplu karşıya yükleme, bir veya daha fazla hiyerarşiler

1. Seçin **karşıya JSON**.
1. Hiyerarşi yükü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [![Toplu karşıya yükleme hiyerarşiler](media/v2-update-how-to-tsm/portal_ten.png)](media/v2-update-how-to-tsm/portal_ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Tek bir hiyerarşiyi Düzenle

1. Hiyerarşi seçip **Düzenle**.
1. Gerekli değişiklikleri yapın ve seçin **Kaydet**.

    [![Tek bir hiyerarşiyi Düzenle](media/v2-update-how-to-tsm/portal_eleven.png)](media/v2-update-how-to-tsm/portal_eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Bir hiyerarşiyi Sil

1. Hiyerarşi seçip **Sil**. 
1. Örnek hiyerarşi ile ilişkili ise bu silinir.

    [![Bir hiyerarşiyi Sil](media/v2-update-how-to-tsm/portal_twelve.png)](media/v2-update-how-to-tsm/portal_twelve.png#lightbox)

## <a name="instances"></a>Örnekler

### <a name="create-a-single-instance"></a>Tek bir örneğini oluşturma

1. Zaman serisi modelleri Seçici Masası'na gidin ve seçin **örnekleri** menüsünde. Zaman serisi modelleri örneklerinde odaklanmak için Panel'i Daralt.

    [![Tek bir örneğini oluşturma](media/v2-update-how-to-tsm/portal_thirteen.png)](media/v2-update-how-to-tsm/portal_thirteen.png#lightbox)

1. **Add (Ekle)** seçeneğini belirleyin.

    [![Bir örnek ekler](media/v2-update-how-to-tsm/portal_fourteen.png)](media/v2-update-how-to-tsm/portal_fourteen.png#lightbox)

1. Örnek ayrıntıları girin, türü ve hiyerarşi ilişkisi seçip **Oluştur**.

### <a name="bulk-upload-one-or-more-instances"></a>Toplu karşıya yükleme, bir veya daha fazla örnek

1. Seçin **karşıya JSON**.
1. Örnekleri yükü içeren dosyayı seçin.

    [![Toplu karşıya yükleme, bir veya daha fazla örnek](media/v2-update-how-to-tsm/portal_fifteen.png)](media/v2-update-how-to-tsm/portal_fifteen.png#lightbox)

1. **Karşıya Yükle**’yi seçin.

### <a name="edit-a-single-instance"></a>Tek bir örnek Düzenle

1. Örnek seçip **Düzenle**. 
1. Gerekli değişiklikleri yapın ve seçin **Kaydet**.

    [![Tek bir örnek Düzenle](media/v2-update-how-to-tsm/portal_sixteen.png)](media/v2-update-how-to-tsm/portal_sixteen.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Zaman serisi modelleri hakkında daha fazla bilgi için okuma [veri modelleme](./time-series-insights-update-tsm.md).

- Önizleme hakkında daha fazla bilgi edinmek için [Azure zaman serisi öngörüleri önizlemesi gezginde verileri görselleştirme](./time-series-insights-update-explorer.md).

- Desteklenen JSON şekilleri hakkında bilgi edinmek için [desteklenen JSON şekilleri](./time-series-insights-send-events.md#json).
