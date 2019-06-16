---
title: Azure veri fabrikası eşleme veri akışı incelemek ve veri önizlemesi
description: Azure Data Factory eşleme veri akışı veri akışı meta verilerini (inceleyin) ve hata ayıklama modunda olduğunda (veri Önizleme) veri Önizleme gösteren bir bölme vardır.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 47cde50e0874f0f73523925b6d1b2f8ee4abaea9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61284105"
---
# <a name="azure-data-factory-mapping-data-flow-transformation-inspect-tab"></a>Azure veri fabrikası eşleme veri akışını dönüştürme sekmesini inceleyin

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Bölmesini inceleyin](media/data-flow/agg3.png "bölmesini inceleyin")

İnceleme bölmesinde meta verileri, dönüştürme veri akışını bir görünüm sağlar. Sütun sayıları görmek mümkün olacaktır, eklenen sütunlar, veri türleri, sütun sıralamasını ve sütun başvurularının sütunları değiştirildi. "İnceleme" meta verilerinizi salt okunur görünümüdür. Hata ayıklama modu metadate İnceleme bölmesinde görmek için etkin olması gerekmez.

Dönüşümler verilerinizin şekli değiştikçe, meta veriler görürsünüz İnceleme Bölmesi üzerinden flow'a değiştirir. Kaynak dönüşümünüzü tanımlı bir şeması değilse, meta verileri İnceleme bölmede görünür olmayacaktır. Meta veri eksikliği şema değişikliklerini senaryolarda yaygındır.

![Veri önizleme](media/data-flow/datapreview.png "veri önizlemesi")

Veri önizleme dönüştürülmekte olan gibi verilerinizi salt okunur bir görünümünü sağlar bölmesidir. Veri akışınızı doğrulamak için ifadeleri ve dönüştürme çıktısını görüntüleyebilirsiniz. Veri önizlemelerini görmek için switched-on hata ayıklama modunda olması gerekir. Veriler Önizleme Kılavuzu sütunları tıkladığınızda, bir sonraki paneli sağ görürsünüz. Açılır paneli profili İstatistikler seçtiğiniz sütunları gösterir.

![Verilerin önizlemesini grafik](media/data-flow/chart.png "verilerin önizlemesini grafiği")

