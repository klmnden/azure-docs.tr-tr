---
title: Azure Data Factory veri akışı taşıma düğümleri
description: Bir Azure Data Factory veri akış diyagramı düğümleri taşıma
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: f600d6113fd52829b7ad22134f3fc208776eadf2
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267164"
---
# <a name="data-factory-data-flow-move-nodes"></a>Veri Fabrikası veri akışı taşıma düğümleri

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Dönüştürme seçenekleri toplama](media/data-flow/agghead.png "toplayıcısı üstbilgisi")

Azure Data Factory, veri akışı tasarım yüzeyine veri akışları yukarıdan aşağıya, soldan sağa, oluşturduğunuz bir "yapı" yüzeyidir. Her dönüştürme bir artı ile bağlı bir araç kutusu yok (+) simgesi. Serbest biçimli DAG ortamında kenarlar aracılığıyla düğümlerini bağlayan yerine iş mantığınıza yoğunlaşır.

Bu nedenle, bir Sürükle ve bırak paradigma "dönüştürme düğümü, Taşı" yoludur gelen değiştirmek için. Bunun yerine, dönüşümler "gelen akış" değiştirerek taşınacaktır.

Azure Data Factory veri akışı, akış veri akışını temsil eder. Dönüştürme ayarlar bölmesinde, bir "Gelen Buhar" alan görürsünüz. Bu, hangi gelen veri akışını, bu dönüştürme besleme bildirir. Gelen Stream adına tıklayarak ve başka bir veri akışı seçerek, grafik dönüştürme düğümünde fiziksel konumunu değiştirebilirsiniz. Geçerli dönüşümü, stream'deki tüm sonraki dönüşümler yanı sıra, ardından yeni konumuna taşınır.

Bir veya daha fazla dönüşümler ile dönüştürme sonraki taşıyorsanız yeni konumda veri akışı yeni bir dal katılır.

Seçtiğiniz düğümünün sonra sonraki hiçbir dönüştürme varsa, yalnızca dönüşüm yeni konumuna taşıyın.
