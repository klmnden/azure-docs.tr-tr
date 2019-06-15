---
title: Bir sözlük nedir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Bir sözlük listesini tümcecikleri veya cümlelerden (ve çevirileri) her zaman aynı şekilde çevirmek için Microsoft Translator istediğiniz belirten hizalanmış bir belgedir. Sözlükler terim sözlüklerine ya da terim tabanları bazen de çağrılır.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: conceptual
ms.openlocfilehash: 4bf112974befd7063b3da8e2b1c1dcbb7ad55608
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66385235"
---
# <a name="what-is-a-dictionary"></a>Bir sözlük nedir?

Bir sözlük tümcecikleri veya cümleler ve karşılık gelen çevirileri listesini belirten bir hizalanmış belgelerin çiftidir. Sözlükte sağladığınız çeviri kullanarak her zaman kaynak ifade veya cümle, tüm örneklerini çevirmek için Microsoft Translator istediğinizde bir sözlük, eğitim kullanın. Sözlükler bazen terim sözlüklerine ya da terim tabanları çağrılır. Yanılma olarak "Kopyala ve Değiştir" olan tüm koşulları, liste sözlüğü düşünebilirsiniz.

Sözlükler yalnızca tam olarak desteklenen bir Microsoft sinirsel makine çevirisi (NMT) sistemi arkasına dil çiftleri projelerinde çalışır. [Dillerin tam listesini görüntüleyin](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="phrase-dictionary"></a>İfade sözlüğü
Modelinizi eğitim içinde bir ifade sözlüğü eklediğinizde, herhangi bir sözcük veya tümcecik listelenen belirttiğiniz yolla çevrilir. Rest cümlenin zamanki çevrilir. Bir ifade sözlüğü sözlüğü dosyasında kaynak ve hedef aynı çevrilmemiş ifade sağlayarak çevrilmiş olmamalıdır tümcecikleri belirtmek için kullanabilirsiniz.

## <a name="sentence-dictionary"></a>Tümce sözlüğü
Tümce sözlük kaynak cümle için bir tam hedef çeviri belirtmenizi sağlar. Tümce sözlük eşleşmenin gerçekleşmesi için gönderilen tümcenin kaynak sözlük girişinin eşleşmesi gerekir.  Yalnızca bir kısmını cümlenin eşleşen girişi eşleşmiyor.  Eşleşme algılandığında, cümle sözlük hedef girişi döndürülür.

## <a name="dictionary-only-trainings"></a>Yalnızca sözlük eğitimleri
Sözlük verileri kullanarak bir model eğitebilirsiniz. Bunu yapmak için yalnızca sözlük belge (veya birden çok sözlük belge) seçin, model Oluştur'a dokunun ve dahil etmek istediğiniz. Bu yalnızca sözlük eğitim olduğundan, hiçbir en düşük gerekli eğitim cümleler sayısı yoktur. Model, genellikle eğitim standart bir eğitim daha çok daha hızlı tamamlanır.  Elde edilen modelleri, ek olarak, eklediğiniz sözlükleri çeviri için Microsoft temel modelleri kullanır.  Bir test raporu almazsınız.

>[!Note]
>Özel Translator cümle değil sözlük dosyaları, kaynak ve hedef tümcecikleri eşit sayıda olduğunu önemlidir Hizala / cümleler, sözlüğünüzü belgeler ve bunların tam olarak hizalanır.

## <a name="recommendations"></a>Öneriler

- Sözlükleri bir eğitim verileriyle eğitilen bir modelin yerini alamayacak.  Sözlük, temelde bulun ve sözcükleri veya tümceleri değiştirin.  İzin vererek, eğitim malzemesi tam cümle içinde uzmanlardan sistemin genel bir sözlük kullanmaktan daha iyi bir seçimdir.
- İfade sözlüğü kullanılmamalıdır. Bir ifade bir tümce içinde değiştirildiğinde, bağlamı Bu cümle içinde kaybolur veya rest cümlenin çevirmek için sınırlı. İfadesinin sonucu olan veya cümlenin Word'de göre ifade sözlüğü İngilizceye, cümle genel çeviri kalitesini genellikle düşer.
- İfade sözlüğü ürün adları ("Microsoft SQL Server"), tam adlar ("City, Hamburg") veya ("pivot tablo") ürününün özellikleri gibi bileşik isimleri için iyi çalışır. İşe yaramazsa eşit çünkü bunlar genellikle yüksek oranda kaynak veya hedef dilde bükümlü fiiller veya sıfat yanı sıra. Bileşik isimleri dışında hiçbir şeyde tümcecik dictionary girişlerinin kaçının.
- Bir sözlük kullanırken, büyük/küçük harf ve noktalama işaretleri çevirilerinizi içinde büyük/küçük harf ve hedef dosyanızda sağlanan noktalama yansıtır. Büyük/küçük harf ve noktalama işaretleri giriş cümle ve sözlük dosyanızı kaynak cümleleri eşleştiğinden tanımlamak çalışırken göz ardı edilir. Örneğin, bu belirtilen "City, Hamburg" kaynak dosyasında ve "Ciudad de hamburg" hedef dosyasında bir İngilizce sözlüğü kullanılan İspanyolca sisteme eğitilmiş varsayalım. Çeviri "Hamburg city" ifadesini dahil bir tümceyi istediğim, ardından "Hamburg city" "Hamburg City" girişi için Sözlük dosyamı eşleşir ve "Ciudad de hamburg" Benim son çevirisini eşlemek.
- Bir sözcük sözlüğü dosyasında birden çok kez görünürse, sistem her zaman sağlanan en son giriş kullanın. Sözlüğünüz aynı kelimenin birden çok çevirisinde içermemelidir.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [biçimlerden yönergeleri](document-formats-naming-convention.md).
