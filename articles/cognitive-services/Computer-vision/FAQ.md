---
title: Sık sorulan sorular - görüntü işleme
titlesuffix: Azure Cognitive Services
description: Azure Bilişsel hizmetler görüntü işleme API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 579a47e70f292222914723606d032737b98822bc
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60004882"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Görüntü işleme API'si hakkında sık sorulan sorular

> [!TIP]
> Bu SSS'de sorularınızın yanıtlarını bulamazsanız, görüntü işleme API'si topluluk üzerinde sorma [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya başvurun [Yardım ve Destek UserVoice hakkında](https://cognitive.uservoice.com/)

-----

**Soru**: *Ben özel etiketleri kullanmak için görüntü işleme API'si eğitebilirsiniz?  Örneğin, resimler cat cinslerinin 'yapay ZEKA Eğitimi' sonra da bir yapay ZEKA istek türünün değerini almak için akışa istiyorum.*

**Yanıt**: Bu işlev şu anda kullanılamıyor. Ancak, Mühendislerimiz, görüntü işleme için bu işlevselliği getirmek için çalışıyor.

-----

**Soru**: *Görüntü işleme internet bağlantısı olmadan yerel olarak kullanılabilir mi?*

**Yanıt**: Şu anda bir şirket içi sunmuyoruz veya yerel bir çözüm.

-----

**Soru**: *Görüntü işleme, lisans kalıplarını okumak için kullanılabilir mi?*

**Yanıt**: Görüntü işleme API'si OCR ile iyi metin algılama özelliği sunar, ancak lisans kalıplarını için şu anda getirilmemiştir. Sürekli OCR otomatik lisans blondan tanıma için özellik istekleri listemize ekledik ve hizmetlerimizi geliştirmek çalışıyoruz.

-----

**Soru**: *Hangi tür yazma yüzeyleri için el yazısı tanıma destekleniyor mu?*

**Yanıt**: Teknoloji yüzeyleri, mektup, incelemeyi ve sarı Yapışkan notlar gibi farklı türde çalışır.

-----

**Soru**: *El yazısı tanıma işleminin ne kadar sürer?*

**Yanıt**: Geçen süre miktarını metnin uzunluğuna bağlıdır. Daha uzun metinler için birkaç saniye sürebilir. Bu nedenle, resimlerdeki el yazısı metni tanı işlemi tamamlandıktan sonra resimlerdeki el yazısı metin işleminin sonucunu Al işlemi kullanarak sonuçları almadan önce beklemeniz gerekebilir.

-----

**Soru**: *El yazısı tanıma teknolojisini kullanarak bir giriş işaretini bir satır ortasında eklenen metinleri nasıl ele alınıyor?*

**Yanıt**: Bu metin ayrı bir satırda el yazısı tanıma işlemi tarafından döndürülür.

-----

**Soru**: *El yazısı tanıma teknolojisini çizilmiş sözcükleri veya satırları nasıl işliyor?*

**Yanıt**: Bunları tanınmayan işlemek için birden fazla satır sözcükleri geçildiğinden, el yazısı tanıma işlemi bunları kısımdan devam etmez. Ancak, sözcüklerin tek bir satırı kullanarak aşıldığında, o kesen gürültü kabul edilir ve sözcükleri hala el yazısı tanıma işlemiyle toplanmış.

-----

**Soru**: *Hangi metin yönleri için el yazısı tanıma teknolojisini destekleniyor mu?*

**Yanıt**: Metin açılarını yaklaşık 30 derece en fazla 40 dereceye adresindeki yönelik el yazısı tanıma işlemiyle toplanmış.

-----
