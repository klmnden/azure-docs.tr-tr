---
title: Görüntü işleme API'si hakkında SSS
titlesuffix: Azure Cognitive Services
description: Azure Bilişsel hizmetler görüntü işleme API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: kefre
ms.openlocfilehash: 55b474d0edbb8dc01b9f35d4f8799e53e37862df
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166381"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Görüntü işleme API'si hakkında sık sorulan sorular

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS'de sorularınızın yanıtlarını bulamazsanız, görüntü işleme API'si topluluk üzerinde sorma [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya başvurun [Yardım ve Destek UserVoice hakkında](https://cognitive.uservoice.com/)

-----

**Soru**: *özel etiketleri kullanmak için görüntü işleme API'si eğitim yapabilirsiniz?  Örneğin, resimler cat cinslerinin 'yapay ZEKA Eğitimi' sonra da bir yapay ZEKA istek türünün değerini almak için akışa istiyorum.*

**Yanıt**: Bu işlev şu anda kullanılamıyor. Ancak, Mühendislerimiz, görüntü işleme için bu işlevselliği getirmek için çalışıyor.

-----

**Soru**: *olabilir görüntü işleme internet bağlantısı olmadan yerel olarak kullanılabilir mi?*

**Yanıt**: şu anda bir şirket içi sunmuyoruz veya yerel bir çözüm.

-----

**Soru**: *hangi dilleri ile görüntü işleme desteklenir?*

**Yanıt**: desteklenen diller şunlardır:

| | | Desteklenen Diller | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Danca (da DK)  | Felemenkçe (nl-NL)     | Türkçe           | Fince (fi-FI)            |Fransızca (fr-FR)
| Almanca (de-DE)  | Yunanca (el-GR)     | Macarca (hu-HU) | İtalyanca (it-IT)            | Japonca (ja-JP)
| Korece (ko-KR)  | Norveççe (nb-yok) | Lehçe (pl-PL)    | Portekizce (pt-BR) (pt-PT) | Rusça (ru-RU)
| İspanyolca (es-ES)   | İsveç dili (sv-SV)     | Türkçe (tr-TR)   |                            |

-----

**Soru**: *olabilir görüntü işleme, lisans kalıplarını okumak için kullanılır?*

**Yanıt**: görüntü işleme API'si, OCR ile iyi metin algılama sunar, ancak lisans kalıplarını için şu anda getirilmemiştir. Sürekli OCR otomatik lisans blondan tanıma için özellik istekleri listemize ekledik ve hizmetlerimizi geliştirmek çalışıyoruz.

-----

**Soru:** *hangi diller için el yazısı tanıma desteklenir?*

**Yanıt**: şu anda yalnızca İngilizce dili desteklenmektedir.

-----

**Soru**: *hangi tür yazma yüzeyleri için el yazısı tanıma desteklenir?*

**Yanıt**: teknoloji yüzeyleri, mektup, incelemeyi ve sarı Yapışkan notlar gibi farklı türde çalışır.

-----

**Soru**: *el yazısı tanıma işleminin ne kadar sürer?*

**Yanıt**: metnin uzunluğuna, geçen süreyi bağlıdır. Daha uzun metinler için birkaç saniye sürebilir. Bu nedenle, resimlerdeki el yazısı metni tanı işlemi tamamlandıktan sonra resimlerdeki el yazısı metin işleminin sonucunu Al işlemi kullanarak sonuçları almadan önce beklemeniz gerekebilir.

-----

**Soru**: *nasıl bir satır ortasında bir şapka karakterini kullanarak eklenen el yazısı tanıma teknolojisi tanıtıcı metinleri mu?*

**Yanıt**: Böyle bir metin ayrı bir satırda el yazısı tanıma işlemi tarafından döndürülür.

-----

**Soru**: *nasıl el yazısı tanıma teknolojisini ele çizilmiş sözcükleri veya satırları?*

**Yanıt**: bunları tanınmayan işlemek için birden fazla satır sözcükleri geçildiğinden, el yazısı tanıma işlemi bunları kısımdan devam etmez. Ancak, sözcüklerin tek bir satırı kullanarak aşıldığında, o kesen gürültü kabul edilir ve sözcükleri hala el yazısı tanıma işlemiyle toplanmış.

-----

**Soru**: *hangi metin yönleri için el yazısı tanıma teknolojisini desteklenir?*

**Yanıt**: açılarını yaklaşık 30 derece en fazla 40 dereceye adresindeki yönelik metin toplanmış el yazısı tanıma işlemiyle.

-----
