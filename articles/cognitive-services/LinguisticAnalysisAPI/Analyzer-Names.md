---
title: Dil analiz API yapısında adlandırma Çözümleyicisi | Microsoft Docs
description: Adlandırma yapısını Çözümleyicileri için dil analiz API esneklik ve duyarlık izin vermek için nasıl kullandığını öğrenin.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/23/2016
ms.author: lesun
ms.openlocfilehash: 2729b7126e82862660fc8e1a995cc87ae996ea03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351569"
---
# <a name="analyzer-names"></a>Çözümleyici adları

Çözümleyiciler için biraz karmaşık bir adlandırma yapısı Çözümleyicileri ve bir ad ne anlama geldiğini anlamak duyarlık hem esnekliğini tanımak için kullanırız.
Çözümleyici adları dört bölümden oluşur: bir kimliği, bir tür, bir belirtim ve bir uygulama.
Her bileşenin rol aşağıda tanımlanmıştır.

## <a name="id"></a>Kimlik
İlk olarak, bir Çözümleyicisi benzersiz bir kimliği var; bir GUID.
Bu GUID görece nadiren değiştirmeniz gerekir, ancak belirli bir Çözümleyicisi benzersiz şekilde tanımlamak için tek yoludur.

## <a name="kind"></a>Tip
Ardından, her Çözümleyicisi olan bir **türü**.
Çözümleme türü döndürdü ve bu analizi temsil etmek için kullanılan veri yapısını benzersiz olarak tanımlamanız gerekir çok geniş koşullarını tanımlar.
Şu anda, üç farklı tür vardır:
 - [Belirteçler](Sentences-and-Tokens.md)
 - [POS etiketleri](Pos-Tagging.md)
 - [Constituency ağacı](constituency-parsing.md)

## <a name="specification"></a>Belirtimi
Belirli bir tür içinde nasıl belirli olguya analiz edilmesi gereken üzerinde farklı uzmanlar ancak katılmıyorum.
Programlama dillerinin, bu nasıl yapılmalıdır hiçbir açık ve tam tanımı yok.

Örneğin, "Kendisinin gidin alamadık." İngilizce cümlede belirteçleri bulmaya çalıştığınız düşünün
Özellikle, dize "siz" göz önünde bulundurun.
Bir olası yorumlama olduğundan bu iki belirteçlere bölünmesi: "vermedi" ve "değil".
Ardından alternatif cümle "kendisi değil gitmiş" belirteçleri aynı kümesi gerekir.
Diğer bir olasılık, bu belirteçleri "vermedi" ve "ma" olarak bölünmüş düşünelim sağlamaktır.
İkinci belirtecin normalde bir sözcük olarak değerlendirilebilecek değil, ancak bazı durumlarda yararlı olabilir yüzey dize hakkında daha fazla bilgi bu yaklaşım korur.
Veya bu daraltmaya tek bir sözcük belki de düşünülmelidir.

Ne olursa olsun hangi seçim yapıldığında, seçim tutarlı bir şekilde yapılmalıdır.
Bu rolü tam olarak olan bir **belirtimi**: karar vermek için doğru gösterimi olmalıdır.

Çözümleyici çıkışları yalnızca oldukça aynı belirtimine uygun veri karşılaştırılabilir.

## <a name="implementation"></a>Uygulama

Genellikle aynı sonuçları elde girişimi birden çok model vardır, ancak farklı performans özelliklere sahip.
Bir model daha hızlı olabilir ancak daha az doğru; başka farklı bir denge yapabilir.

**Uygulama** Çözümleyicisi adı kısmı bu tür bilgiler, tanımlamak için kullanılan böylece kullanıcılar için gereksinimlerine en uygun Çözümleyicisi'ni seçebilirsiniz.
