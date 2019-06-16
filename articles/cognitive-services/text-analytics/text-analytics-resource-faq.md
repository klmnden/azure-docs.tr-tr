---
title: Metin analizi API'si hakkında sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Metin analizi API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: a85fa543a6b26a5ea6452ce99fb91dc1ce465db7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60827961"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-cognitive-service"></a>Metin analizi Bilişsel hizmet hakkında sık sorulan sorular (SSS)

 Kavramları, kod ve senaryolar için Azure üzerinde Microsoft Bilişsel hizmetler metin analizi API'si için ilgili hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="can-text-analytics-identify-sarcasm"></a>Metin analizi alaycılığa belirleyebilir misiniz?

Analiz, pozitif ve negatif yaklaşım ruh algılama yerine ' dir.

Her zaman bir ölçüde yaklaşım analizi kesinlik eksikliği olduğu halde gizli anlam ya da alt içeriğine olduğunda model en kullanışlıdır. İrony, alaycılığa anıları ve benzer şekilde incelikli içerik kültürel bağlam ve amacını iletmek için normları kullanır. Bu tür içerik, En zorlu arasında analiz etmek için kullanılır. Genellikle, incelikli bir anlama sahip içerik Çözümleyicisi ve bir insan tarafından öznel bir değerlendirmesi tarafından oluşturulan belirli bir puan arasında en fazla uyumsuzluk içindir.

## <a name="can-i-add-my-own-training-data-or-models"></a>Kendi eğitim verileri veya model ekleyebilir miyim?

Hayır, modelleri kullanan. Karşıya yüklenen veri çubuğunda kullanılabilir işlemleri yalnızca Puanlama anahtar ifade ayıklama ve dil algılama. Özel modelleri barındırmayın. İstediğiniz, oluşturmak ve özel makine öğrenimi modellerini barındırmak göz önünde bulundurun [makine öğrenimi özellikleri Microsoft R Server'da](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package).

## <a name="can-i-request-additional-languages"></a>İlave diller isteyebilir miyim?

Yaklaşım analizi ve anahtar ifade ayıklama için kullanılabilir bir [seçin dil sayısı](text-analytics-supported-languages.md). Doğal dil işleme, karmaşık ve önemli yeni işlevler yayımlanabilmesi için önce test edilmesini gerektirir. Bu nedenle, hiç bir bağımlılık yetişkin daha çok zaman gerektiren işlevselliğini alır, böylece destek önceden Duyurusu kaçının. 

Sonraki çalışmak için hangi diller belirlememize yardımcı olmak için belirli diller için üzerinden oy [User Voice](https://cognitive.uservoice.com/forums/555922-text-analytics). 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>Anahtar ifade ayıklama neden bazı sözcükler, ancak diğerlerini döndürür?

Anahtar ifade ayıklama, gerekli olmayan sözcükler ve tek başına sıfat ortadan kaldırır. "Muhteşem görünümleri" veya "kafanız karışık hava durumu" gibi bir sıfat-ad birleşimleri birlikte döndürülür.

Genellikle, çıkış isimleri ve nesneleri cümlenin oluşur. Çıkış en önemli işlemin ilk deyim ile önem sırasına göre listelenir. Önem derecesi, belirli bir kavram belirtilen sayıda veya diğer öğelere metin o öğenin ilişki tarafından ölçülür.

## <a name="why-does-output-vary-given-identical-inputs"></a>Neden çıkış, aynı giriş değişiyor?

Güncelleştirme küçük değişiklik büyük veya hizmete sessizce e ise modeller ve algoritmalar geliştirmeleri bildirilir. Zaman içinde aynı metin giriş sonuçları farklı yaklaşım puanını veya anahtar tümcecik çıkış bulabilirsiniz. Bulutta yönetilen bir makine öğrenme kaynakları kullanarak normal ve kasıtlı bir sonuç budur.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında sorunuz var mı? İsteme veya bunun için oy göz önünde bulundurun bizim [UserVoice web sitesi](https://cognitive.uservoice.com/forums/555922-text-analytics).

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow: Metin Analizi API’si](https://stackoverflow.com/questions/tagged/text-analytics-api)   
 [StackOverflow: Bilişsel hizmetler](https://stackoverflow.com/questions/tagged/microsoft-cognitive)
