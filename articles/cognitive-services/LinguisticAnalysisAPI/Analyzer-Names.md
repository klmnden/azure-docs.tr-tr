---
title: Adlandırma yapısını - dil analizi API'si Çözümleyicisi
titlesuffix: Azure Cognitive Services
description: Dilbilimsel analiz API'si Çözümleyicisi adlandırma yapısı, esneklik ve duyarlık için nasıl sağladığını öğrenin.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/23/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: b2e956b099022751712f96ab0b15aa57c752b458
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60405265"
---
# <a name="analyzer-names"></a>Çözümleyici adları

> [!IMPORTANT]
> Dilbilimsel Analiz önizleme sürümü 9 Ağustos 2018 tarihinde kullanımdan kaldırılmıştır. Metin işleme ve analiz için [Azure Machine Learning metin analizi modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) kullanmanızı öneririz.

Her iki esneklik Çözümleyicileri ve bir ad ne anlama geldiğini anlamak duyarlık izin vermek için biraz karmaşık bir adlandırma yapısına Çözümleyicileri için kullanırız.
Çözümleyici adları dört bölümden oluşur: kimlik, bir tür, bir belirtimi ve bir uygulama.
Her bileşenin rol aşağıda tanımlanmıştır.

## <a name="id"></a>Kimlik
İlk olarak, bir çözümleyici benzersiz bir kimlik vardır; bir GUID.
Bu Guıd'lar, nispeten ender olarak değiştirmeniz gerekir, ancak belirli bir çözümleyici benzersiz şekilde tanımlamak için tek yoludur.

## <a name="kind"></a>Tür
Ardından, her Çözümleyicisi olduğu bir **tür**.
Bu analiz türünü döndürdü ve bu analizin temsil etmek için kullanılan veri yapısını benzersiz şekilde tanımlamalıdır çok geniş koşullarını tanımlar.
Şu anda üç farklı tür vardır:
 - [Belirteçler](Sentences-and-Tokens.md)
 - [POS etiketleri](Pos-Tagging.md)
 - [Constituency ağacı](constituency-parsing.md)

## <a name="specification"></a>Belirtimi
Belirli bir tür içinde belirli bir olguya nasıl çözümlenmelidir farklı uzmanlar ancak katılmıyorum.
Programlama dillerden farklı olarak, bu nasıl yapılmalıdır hiçbir NET ve tam tanımı yok.

Örneğin, "He gitmedi." İngilizce cümlede belirteçleri bulmaya çalıştığınız düşünün.
Özellikle, dize, "siz" göz önünde bulundurun.
Olası bir yorumu olduğundan bu iki belirteçlere bölünmesi: "yaptığını" ve "not".
Ardından alternatif cümlenin "He değil gitti" belirteçleri aynı kümesi gerekir.
Bu belirteçleri "yaptığını" ve "Me" olarak ayrılmalıdır söyleyin başka bir olasılıktır.
İkinci belirtecin normalde bir sözcük değerlendirilmeyecektir, ancak bazı durumlarda yararlı olabilir yüzey dize hakkında daha fazla bilgi bu yaklaşım korur.
Veya belki de bu kısaltma tek bir sözcük düşünülmelidir.

Bakılmaksızın hangi seçeneği yapıldığı, seçim tutarlı bir şekilde yapılması.
Bu rolü tam olarak, bir **belirtimi**: karar verirken doğru gösterimi olmalıdır.

Çözümleyici çıkışları yalnızca oldukça aynı belirtimine uyan veriler için karşılaştırılabilir.

## <a name="implementation"></a>Uygulama

Genellikle aynı sonuçları elde etmek için birden çok modeli vardır ancak farklı performans özellikleri olan.
Bir model daha hızlı olabilir ancak daha az doğru; başka farklı bir denge duruma getirebilir.

**Uygulama** Çözümleyicisi adının bir kısmını bu tür bilgiler, tanımlamak için kullanılır, böylece kullanıcılar için kendi gereksinimlerine en uygun Çözümleyicisi'ni seçebilirsiniz.
