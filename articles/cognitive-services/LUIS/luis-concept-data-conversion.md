---
title: HALUK - Azure veri dönüştürme kavramlarını anlama | Microsoft Docs
description: Tahminleri dil anlama (HALUK) önce utterances nasıl değiştirilebilir öğrenin
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 56de113b41be419a5e39d705a22466139c1c29ce
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266116"
---
# <a name="data-conversion-concepts-in-luis"></a>Veri dönüştürme kavramlarını HALUK
HALUK utterances metin utterances tahmin önce konuşulan utterances dönüştürmek için bir yol sağlar. 

## <a name="speech-to-intent-conversion-concepts"></a>Konuşma hedefi dönüştürme kavramları
HALUK metinde konuşma dönüştürülmesi konuşulan utterances bir uç nokta gönderip HALUK tahmin yanıt sağlar. Bir tümleştirilmesi işlemidir [konuşma](https://docs.microsoft.com/azure/cognitive-services/Speech) hizmeti HALUK ile. 

### <a name="key-requirements"></a>Anahtar gereksinimleri
Oluşturmanıza gerek olmayan bir **Bing konuşma API** Bu tümleştirme için anahtar. HALUK anahtar için bu tümleştirme çalışır.

### <a name="new-endpoint"></a>Yeni uç nokta 
Bu tümleştirme yeni bir uç noktası oluşturur ve [fiyatlandırma](luis-boundaries.md#key-limits) modeli. Uç nokta konuşulan her ikisi de alabilir ve metin utterances tek bir uç nokta olara kullanma olanak sağlar. 

### <a name="quota-usage"></a>Kota kullanımı
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) bilgi. 

### <a name="data-retention"></a>Veri saklama
Konuşma veya metin ise bitiş noktasına bakılmaksızın gönderilen veriler, yalnızca konuşma modelinizi geliştirmek için kullanılır. Bunu modelinizin dışında bir genel kapasite konuşma veya HALUK geliştirmek için kullanılmaz. HALUK uygulama silindiğinde, tutulan verileri de silinir.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğretici ile doğru yazım](luis-tutorial-bing-spellcheck.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions