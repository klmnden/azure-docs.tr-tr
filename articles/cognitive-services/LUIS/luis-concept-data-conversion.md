---
title: HALUK - Azure veri dönüştürme kavramlarını anlama | Microsoft Docs
description: Tahminleri dil anlama (HALUK) önce utterances nasıl değiştirilebilir öğrenin
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/27/2018
ms.author: v-geberr;
ms.openlocfilehash: 16b0df4b81220885e2c3747470272cee9536e10c
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063570"
---
# <a name="data-conversion-concepts-in-luis"></a>Veri dönüştürme kavramlarını HALUK
HALUK utterances metin utterances tahmin önce konuşulan utterances dönüştürmek için bir yol sağlar. 

## <a name="speech-to-intent-conversion-concepts"></a>Konuşma hedefi dönüştürme kavramları
HALUK metinde konuşma dönüştürülmesi konuşulan utterances bir uç nokta gönderip HALUK tahmin yanıt sağlar. Bir tümleştirilmesi işlemidir [konuşma](https://docs.microsoft.com/azure/cognitive-services/Speech) hizmeti HALUK ile. 

### <a name="key-requirements"></a>Anahtar gereksinimleri
Oluşturmanıza gerek olmayan bir **Bing konuşma API** Bu tümleştirme için anahtar. A **dil anlama** Azure Portal'da oluşturulan anahtarı bu tümleştirme için çalışır. HALUK başlangıç anahtarı kullanmayın, bu tümleştirme için çalışmaz.

### <a name="new-endpoint"></a>Yeni uç nokta 
Bu tümleştirme yeni bir uç noktası oluşturur ve [fiyatlandırma](luis-boundaries.md#key-limits) modeli. Uç noktası aracılığıyla [konuşma SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk), hem konuşulan alabildiğini ve metin utterances tek bir uç nokta olara kullanma olanak sağlar. 

### <a name="quota-usage"></a>Kota kullanımı
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) bilgi. 

### <a name="data-retention"></a>Veri saklama
Konuşma veya metin ise konuşma SDK'sı üzerinden uç bakılmaksızın gönderilen veriler, yalnızca konuşma modelinizi geliştirmek için kullanılır. Bunu modelinizin dışında bir genel kapasite konuşma veya HALUK geliştirmek için kullanılmaz. HALUK uygulama silindiğinde, tutulan verileri de silinir.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşarak metin](luis-tutorial-speech-to-intent.md)

