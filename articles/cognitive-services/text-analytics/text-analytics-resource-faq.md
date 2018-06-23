---
title: Metin analizi API - Azure Bilişsel hizmetler hakkında sık sorulan sorular | Microsoft Docs
description: Azure üzerinde Microsoft Bilişsel Hizmetleri metin Analytics API ilgili sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: bf82899b4317f0f5ce0f6ca5096dccef7cddd931
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355174"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-api"></a>Metin analizi API hakkında sık sorulan sorular (SSS)

 Kavramları, kod ve metin Analytics API için Microsoft Azure üzerinde Bilişsel hizmetler için ilgili senaryoları hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="can-text-analytics-identify-sarcasm"></a>Metin analizi alaycı düşünceleri belirleyebilir misiniz?

Analiz pozitif negatif düşünceleri ruh algılama yerine ' dir.

Her zaman bazı derecesini düşünceleri analizi imprecision yoktur, ancak hiçbir gizli anlamı veya alt içerik olduğunda en yararlı modelidir. İrony, alaycı düşünceleri, mizah ve benzer şekilde nuanced içerik kültürel bağlamını ve amacı iletmek için normları kullanır. Bu içerik türünü En zorlu arasında analiz etmek için ' dir. Genellikle, Çözümleyicisi ve öznel değerlendirme bir insan tarafından üretilen belirli bir puan arasında büyük farklılık nuanced anlamı içerikle içindir.

## <a name="can-i-add-my-own-training-data-or-models"></a>Kendi eğitim verileri veya modelleri ekleyebilir miyim?

Hayır, modelleri pretrained. Karşıya yüklenen veriler üzerinde kullanılabilir yalnızca işlemlerine Puanlama, anahtar tümcecik ayıklama ve dil algılama. Özel modelleri barındırmayan. İstiyorsanız oluşturmak ve özel makine öğrenimi modellerini konak, göz önünde bulundurun [makine yeteneklerini Microsoft R Server öğrenme](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package).

## <a name="can-i-request-additional-languages"></a>Ek dilleri isteyebilir miyim?

Düşünceleri analiz ve anahtar tümcecik ayıklama için kullanılabilir bir [diller sayıda](text-analytics-supported-languages.md). Doğal dil işleme karmaşıktır ve yeni işlevsellik yayımlanabilmesi için önce önemli sınama gerektirir. Bu nedenle, böylece hiç kimse bir bağımlılık yetişkin için daha fazla zaman gerektiren işlevini alır destek önceden Duyurusu kaçının. 

Sonraki sayfada çalışması için hangi dilleri belirlememize yardımcı olması için oy belirli diller için [kullanıcı sesi](https://cognitive.uservoice.com/forums/555922-text-analytics). 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>Anahtar tümcecik ayıklama neden bazı sözcükleri ancak diğer dönüş?

Anahtar tümcecik ayıklama gerekli olmayan sözcükleri ve tek başına sıfatları ortadan kaldırır. "Muhteşem görünümleri" veya "sisli hava durumu" gibi gelen isim birleşimleri birlikte döndürülür.

Genellikle, çıktı isimleri ve nesneleri tümcenin oluşur. Çıktı en önemli olan ilk deyim ile önem, sırayla listelenir. Önem derecesi metni diğer öğeler bu öğeye ilişkisi veya belirli bir kavram belirtiliyor sayısı tarafından ölçülür.

## <a name="why-does-output-vary-given-identical-inputs"></a>Neden çıktı, aynı girişleri değişiyor mu?

Güncelleştirme küçük değişiklik önemli ya da hizmete sessizce e ise modelleri ve algoritmaları geliştirmeleri bildirilir. Zaman içinde aynı metin giriş sonuçları farklı düşünceleri puan veya anahtar tümcecik çıkış bulabilirsiniz. Bulutta yönetilen makine öğrenme kaynakları kullanarak normal ve kasıtlı bir sonuç budur.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında soru mi? İsteme veya onun için oy göz önünde bulundurun bizim [UserVoice web sitesi](https://cognitive.uservoice.com/forums/555922-text-analytics).

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow: Metin analizi API](https://stackoverflow.com/questions/tagged/text-analytics-api)   
 [StackOverflow: Bilişsel hizmetler](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
