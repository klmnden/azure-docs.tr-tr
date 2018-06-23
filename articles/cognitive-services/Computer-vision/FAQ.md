---
title: Bilgisayar görme API için sık sorulan sorular | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde bilgisayar görme API hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 01/26/2017
ms.author: kefre
ms.openlocfilehash: 5c862dd2fb26a005f4e785673a4e9358ecf9286f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351544"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Bilgisayar görme API sık sorulan sorular
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS sorularınızın yanıtlarını bulamazsanız, üzerinde bilgisayar görme API topluluk isteyen deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya başvurun [Yardım ve Destek UserVoice üzerinde](https://cognitive.uservoice.com/)

-----

**Soru**: *özel etiketler kullanmak için bilgisayar görme API eğitim yapabilirsiniz?  Örneğin, 'AI eğitmek' sonra da bir AI isteğinde türünün değeri almak Kedi cinslerinin resimlerinin akış istiyor.*

**Yanıt**: Bu işlev şu anda kullanılabilir değil. Ancak, bu işlev için bilgisayar görme getirmek için bizim mühendisleri çalışmaktadır.

-----

**Soru**: *olabilir bilgisayar görme internet bağlantısı olmadan yerel olarak kullanılabilir mi?*

**Yanıt**: şu anda bir şirket içi sunuyoruz değil veya yerel çözüm.

-----

**Soru**: *hangi dilleri bilgisayar görme ile desteklenir?*

**Yanıt**: desteklenen diller şunlardır:

| | | Desteklenen Diller | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Danca (da-DK)  | Felemenkçe (nl-NL)     | Türkçe           | Fince (fi-FI)            |Fransızca (fr-FR)
| Almanca (de-DE)  | Yunanca (el-GR)     | Macarca (hu-HU) | İtalyanca (it-IT)            | Japonca (ja-JP)
| Korece (ko-KR)  | Norveççe (nb-Hayır) | Lehçe (pl-PL)    | Portekizce (pt-BR) (pt-PT) | Rusça (ru-RU)
| İspanyolca (es-ES)   | İsveç dili (sv-SV)     | Türkçe (tr-TU)   |                            |

-----

**Soru**: *olabilir bilgisayar görme lisans kalıplarını okumak için kullanılabilir?*

**Yanıt**: görme API OCR iyi metin algılama sağlar, ancak lisans kalıplarını için şu anda getirilmemiştir. Numaralı sürekli hizmetlerimizi geliştirmek ve OCR otomatik lisans kalıbı tanıma için özellik istekleri listemiz eklemiş olduğunuz çalışıyoruz.

-----

**Soru:** *hangi dilleri el yazısı tanıma için desteklenir?*

**Yanıt**: şu anda yalnızca İngilizce desteklenir.

-----

**Soru**: *el yazısı tanıma için hangi tür yüzeyleri yazma desteklenir?*

**Yanıt**: teknolojisi farklı türde yüzeyleri, Beyaz Tahta, incelemeyi ve sarı Yapışkan notlar gibi çalışır.

-----

**Soru**: *el yazısı tanıma işlemi ne kadar sürer?*

**Yanıt**: metnin uzunluğunu, geçen süre bağlıdır. Daha uzun metinler için birkaç saniye sürebilir. Bu nedenle, el yazısıyla yazılmış metni tanı işlemi tamamlandıktan sonra sonuçları el yazısıyla yazılmış metin işlem sonucu alma işlemi kullanarak alabilmeleri beklemeniz gerekebilir.

-----

**Soru**: *nasıl bir satır ortasında bir şapka kullanılarak eklenen el yazısı tanıma teknolojisi tanıtıcı metin mu?*

**Yanıt**: Böyle metin ayrı bir satırda el yazısı tanıma işlemi tarafından döndürülür.

-----

**Soru**: *nasıl el yazısı tanıma teknolojisini ele çizilmiş sözcükleri veya satır?*

**Yanıt**: Tanınmayan işlemek için birden çok satıra sözcükleri geçildiğinden, el yazısı tanıma işlemi bunları almak değil. Ancak, sözcükleri tek bir satır kullanarak geçildiğinden, bu aşma parazit olarak kabul edilir ve sözcükleri hala el yazısı tanıma işlemiyle toplanmış.

-----

**Soru**: *hangi metin yönler el yazısı tanıma teknolojinin desteklenir?*

**Yanıt**: açılarını yaklaşık 30 derece en çok 40 dereceye adresindeki yönlendirilmiş metin toplanma el yazısı tanıma işlemi tarafından.

-----
