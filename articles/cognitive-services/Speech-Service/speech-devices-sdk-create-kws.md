---
title: Bir özel Uyandırma word - konuşma hizmetleri oluşturma
titleSuffix: Azure Cognitive Services
description: Cihazınız, her zaman bir Uyanma sözcük (veya tümceciği) dinliyor. Uyandırma word kullanıcı diyor, kullanıcının dikte durdurur kadar cihaz buluta tüm sonraki ses gönderir. Uyandırma Word'ün özelleştirme, Cihazınızı ayırt ve marka bilgilerinizi güçlendirmek için etkili bir yoludur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 2280af4bf37fdb3cd12482da855f979a9180f0ec
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020537"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>Konuşma hizmeti kullanarak bir özel Uyandırma sözcük oluşturma

Cihazınız, her zaman bir Uyanma sözcük (veya tümceciği) dinliyor. Örneğin, "Hey Cortana" Cortana Yardımcısı için bir Uyanma sözcüktür. Uyandırma word kullanıcı diyor, kullanıcının dikte durdurur kadar cihaz buluta tüm sonraki ses gönderir. Uyandırma Word'ün özelleştirme, Cihazınızı ayırt ve marka bilgilerinizi güçlendirmek için etkili bir yoludur.

Bu makalede, özel Uyandırma word cihazınız için nasıl oluşturulduğunu öğrenin.

## <a name="choose-an-effective-wake-word"></a>Etkili Uyandırma word seçin

Uyandırma word seçtiğinizde aşağıdaki yönergeleri göz önünde bulundurun:

* Uyandırma Word'ün İngilizce bir sözcük veya ifade olmalıdır. Söyleyin iki saniyeden daha uzun sürer.

* 4 ila 7 hece sözcükleri en iyi çalışır. Örneğin, "Hey, bilgisayar" bir iyi Uyandırma sözcüktür. Yalnızca "Merhaba" zayıf bir paroladır.

* Uyandırma sözcükleri ortak İngilizce telaffuz kurallarını izlemelidir.

* Benzersiz bir veya daha yaygın İngilizce telaffuz kurallarını izleyen made-up sözcük hatalı pozitif sonuçları azaltmak. Örneğin, "computerama" iyi Uyandırma word olabilir.

* Ortak bir sözcük seçmeyin. Örneğin, "yemek" ve "kişiler sık sıradan konuşmada söyleyin sözcükler Git". Bunlar, cihazınız için false Tetikleyiciler olabilir.

* Söyleyiş olabilir bir Uyanma sözcük kullanmaktan kaçının. Kullanıcılar, cihazlarını yanıt almak için "doğru" telaffuz bilmesi gerekir. Örneğin, "509" "beş sıfır dokuz" bildirilebilir "beş oh dokuz," veya "beş yüz ve dokuz." "R.E.I." "r-e-i" veya "ışın" bildirilebilir "Canlı" "/līv/" veya "/liv/" bildirilebilir.

* Özel karakterler, simgeler veya basamak kullanmayın. Örneğin, "Git #" ve "20 + kediler" iyi Uyandırma sözcükleri olmaz. Ancak, "NET Git" veya "yirmi kediler artı" işe yarayabilir. Yine de, marka simgeleri kullanın ve uygun telaffuz güçlendirmek için pazarlama ve belgeleri kullanın.

> [!NOTE]
> Bir ticari marka olarak kaydettirilmiş sözcük Uyandırma word'olarak seçerseniz, bu ticari marka olduğunuz veya ticari marka sahibinin Word'ü kullanma iznine sahip emin olun. Microsoft, Uyandırma word'ün seçiminize göre ortaya yasal sorunları sorumlu değildir.

## <a name="create-your-wake-word"></a>Uyandırma Word'ün oluşturma

Cihazınızda bir özel Uyandırma sözcük kullanabilmeniz için önce Microsoft özel Uyandırma Word oluşturma hizmeti ile bir Uyanma word oluşturmak gerekir. Bir dosya hizmeti oluşturan bir Uyanma sözcük sağladıktan sonra Uyandırma word Cihazınızda etkinleştirmek için Geliştirme Seti dağıtın.

1. Git [özel konuşma hizmeti portalı](https://aka.ms/sdsdk-speechportal) ve **oturum** ya da seçin bir konuşma abonelik yoksa [ **bir abonelik oluşturun**](https://go.microsoft.com/fwlink/?linkid=2086754)

    ![Özel konuşma hizmeti portalı](media/speech-devices-sdk/wake-word-4.png)

1. Konumunda [özel Uyandırma Word](https://aka.ms/sdsdk-wakewordportal) sayfa türü Uyandırma Word seçim ve tıklatın **Uyandırma sözcük Ekle**. Bazı sahip olduğumuz [yönergeleri](#choose-an-effective-wake-word) etkili bir anahtar sözcüğü seçmenizi sağlayacak. Şu anda yalnızca en-US dil destekliyoruz.

    ![Uyandırma sözcük girin](media/speech-devices-sdk/wake-word-5.png)

1. Uyandırma Word'ün üç Söyleyiş oluşturulur. İstediğiniz tüm Söyleniş seçebilirsiniz. Ardından **Gönder** Uyandırma word oluşturulacak. Söyleniş satırında geldiğinizde Uyandırma word Lütfen Kaldır var olan bir ilk olarak, değiştirmek istediğiniz Sil simgesini görünür.

    ![Uyandırma Word'ün gözden geçirin](media/speech-devices-sdk/wake-word-6.png)

1. Bu model oluşturulacağını belirtmek için en fazla bir dakika sürebilir. Dosyayı karşıdan yüklemeniz istenir.

    ![Uyandırma Word'ün indirin](media/speech-devices-sdk/wake-word-7.png)

1. .Zip dosyasını bilgisayarınıza kaydedin. Bu dosya, özel Uyandırma word geliştirme setine dağıtmak için ihtiyacınız olacak.

## <a name="next-steps"></a>Sonraki adımlar

Kendi özel Uyandırma word ile test [konuşma cihaz SDK'sı hızlı](https://aka.ms/sdsdk-quickstart).
