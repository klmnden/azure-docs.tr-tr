---
title: Azure veri fabrikası eşleme veri akışı incelemek ve veri önizlemesi
description: Azure Data Factory eşleme veri akışı veri akışı meta verilerini (inceleyin) ve hata ayıklama modunda olduğunda (veri Önizleme) veri Önizleme gösteren bir bölme vardır.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: e7d03ecd8ea4aecf1e0cfdd02d3b9bc5300abe6b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213534"
---
# <a name="azure-data-factory-mapping-data-flow-transformation-inspect-tab"></a>Azure veri fabrikası eşleme veri akışını dönüştürme sekmesini inceleyin

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Bölmesini inceleyin](media/data-flow/agg3.png "bölmesini inceleyin")

İnceleme bölmesinde meta verileri, dönüştürme veri akışını bir görünüm sağlar. Sütun sayıları görmek mümkün olacaktır, eklenen sütunlar, veri türleri, sütun sıralamasını ve sütun başvurularının sütunları değiştirildi. "İnceleme" meta verilerinizi salt okunur görünümüdür. Hata ayıklama modu metadate İnceleme bölmesinde görmek için etkin olması gerekmez.

Dönüşümler verilerinizin şekli değiştikçe, meta veriler görürsünüz İnceleme Bölmesi üzerinden flow'a değiştirir. Kaynak dönüşümünüzü tanımlı bir şeması değilse, meta verileri İnceleme bölmede görünür olmayacaktır. Meta veri eksikliği şema değişikliklerini senaryolarda yaygındır.

![Veri önizleme](media/data-flow/datapreview.png "veri önizlemesi")

Veri önizleme dönüştürülmekte olan gibi verilerinizi salt okunur bir görünümünü sağlar bölmesidir. Veri akışınızı doğrulamak için ifadeleri ve dönüştürme çıktısını görüntüleyebilirsiniz. Veri önizlemelerini görmek için switched-on hata ayıklama modunda olması gerekir. Veriler Önizleme Kılavuzu sütunları tıkladığınızda, bir sonraki paneli sağ görürsünüz. Açılır paneli profili İstatistikler seçtiğiniz sütunları gösterir.

![Verilerin önizlemesini grafik](media/data-flow/chart.png "verilerin önizlemesini grafiği")

