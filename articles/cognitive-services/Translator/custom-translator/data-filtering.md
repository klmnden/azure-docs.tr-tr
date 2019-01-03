---
title: Verileri filtreleme - özel Translator
titleSuffix: Azure Cognitive Services
description: Özel bir sistem eğitim için kullanılacak belgeleri gönderdiğinizde, belgeleri işlemek ve filtreleme adımları eğitim için hazırlamak için bir dizi geçeriz.
author: jann-skotdal
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 12/18/2018
ms.author: v-jansko
ms.topic: article
ms.openlocfilehash: bf5dc911fc41cac63c28e5d77434402c04332dfc
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53609323"
---
# <a name="data-filtering"></a>Veri filtreleme 

Özel bir sistem eğitim için kullanılacak belgeleri gönderdiğinizde, belgeleri işlemek ve filtreleme adımları eğitim için hazırlamak için bir dizi geçeriz. Bu adımlar aşağıda açıklanmıştır. Filtreleme bilgi belgeler özel Translator eğitimlerle hazırlamak için kendiniz alabilir adımları yanı sıra özel translator görüntülenen cümle sayısı anlamanıza yardımcı olabilir. 

## <a name="sentence-alignment"></a>Tümce hizalama 
Belgenizi XLIFF, TMX veya HİZALA biçiminde değilse, kaynak ve hedef belgelerinizi, birbiriyle tümce tümce cümleleri özel Translator hizalar. Translator belge hizalama gerçekleştirmez: başka bir dilde, eşleşen belge bulmak için belgelerin adlandırma izler. Belgenin içinde özel Translator karşılık gelen cümlenin başka bir dilde bulmayı dener. Belge kullanan katıştırılmış HTML gibi biçimlendirme etiketleri ile aynı doğrultuda yardımcı olmak için.  

Kaynak cümlelerde sayısı arasında büyük bir tutarsızlık bakın ve hedef tarafı belgeleri, belgenizi ilk başta, veya diğer nedenlerle de alignable paralel olmuş olabilir. Belge ile büyük bir fark çiftlerini (> % 10) üzerinde her yan tümcelerin bunlar gerçekten paralel emin olmak için ikinci bir görünüm garanti. Tümce sayısı gecikmeleri anormal biçimde farklıysa, özel Translator belgenin yanında bir uyarı gösterir.  


## <a name="deduplication"></a>Yinelenenleri kaldırma 
Özel Translator, test ve eğitim verilerini belgelerden ayarlama bulunan cümleleri kaldırır. Kaldırma işlemi dinamik olarak çalıştırın, veri işleme adımı değil eğitim içinde gerçekleşir. Özel Translator cümle sayısı, bu kaldırma işlemi önce proje genel bakış içindeki raporlar.  

## <a name="length-filter"></a>Uzunluğu filtresi 
* Cümleleri iki tarafındaki yalnızca bir sözcük ile kaldırın. 
* Cümleleri 100'den fazla kelimelerin ile kaldırın.  Çince, Japonca, Korece muaf tutulur. 
* Cümleleri az 3 karakter ile kaldırın. Çince, Japonca, Korece muaf tutulur. 
* Cümleleri 2000'den fazla karakter içeren Çince, Japonca, Korece kaldırın. 
* % 1'den az alfa karakterler içeren cümleleri kaldırın. 
* 50'den fazla sözcükler içeren sözlük girişleri kaldırın. 

 
## <a name="white-space"></a>Boşluk 
* Beyaz boşluk karakterleri sekmeler ve tek bir boşluk karakteri ile CR/LF sıraları dahil olmak üzere herhangi bir dizi değiştirin. 
* İçinde cümlenin başında veya sonunda boşluk Kaldır 


## <a name="sentence-end-punctuation"></a>Tümce bitiş noktalama işareti 
Birden çok cümle bitiş noktalama işaretleri, tek bir örnekle değiştirin.  

 
## <a name="japanese-character-normalization"></a>Japonca karakter normalleştirme 
Yinelenen Japonca karakterler Normalleştir: Yarım genişlikte tam genişlikli karakterlere dönüştürür. 

 
## <a name="unescaped-xml-tags"></a>Atlanmayan XML etiketleri 
Dönüşümler atlanmayan etiketleri kaçan etiketi ekleyerek filtreleme: 
* `&lt;` olur `&amp;lt;` 
* `&gt;` olur `&amp;gt;` 
* `&amp;` olur `&amp;amp;` 

 
## <a name="invalid-characters"></a>Geçersiz karakterler 
Özel Translator, U + FFFD Unicode karakter içeren cümleleri kaldırır. ' % S'karakter U + FFFD başarısız bir kodlama dönüştürme gösterir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Bir model eğitip](how-to-train-model.md) özel Translator içinde.
