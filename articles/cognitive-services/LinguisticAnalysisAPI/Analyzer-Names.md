---
title: Adlandırma yapısını - dil analizi API'si Çözümleyicisi
titlesuffix: Azure Cognitive Services
description: Dilbilimsel analiz API'si Çözümleyicisi adlandırma yapısı, esneklik ve duyarlık için nasıl sağladığını öğrenin.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 03/23/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: f8c7aba2b853f3916c2d01d94d49090a4b3e36a0
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237243"
---
# <a name="analyzer-names"></a>Çözümleyici adları

> [!IMPORTANT]
> Dil analizi önizlemesi, 9 Ağustos 2018 tarihinde kullanımdan. Kullanmanızı öneririz [Azure Machine Learning metin analiz modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) metin işleme ve analiz için.

Her iki esneklik Çözümleyicileri ve bir ad ne anlama geldiğini anlamak duyarlık izin vermek için biraz karmaşık bir adlandırma yapısına Çözümleyicileri için kullanırız.
Çözümleyici adları dört bölümden oluşur: kimlik, bir tür, bir belirtimi ve bir uygulama.
Her bileşenin rol aşağıda tanımlanmıştır.

## <a name="id"></a>Kimlik
İlk olarak, bir çözümleyici benzersiz bir kimlik vardır; bir GUID.
Bu Guıd'lar, nispeten ender olarak değiştirmeniz gerekir, ancak belirli bir çözümleyici benzersiz şekilde tanımlamak için tek yoludur.

## <a name="kind"></a>Tip
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
