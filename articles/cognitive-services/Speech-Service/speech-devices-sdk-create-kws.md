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
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b5ace2e741f900dd4ab7ba6518d0956284af35f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461621"
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

Cihazınızda bir özel Uyandırma sözcük kullanabilmeniz için önce Microsoft özel Uyandırma Word oluşturma hizmetini kullanarak Uyandırma word oluşturmanız gerekir. Bir dosya hizmeti oluşturan bir Uyanma sözcük sağladıktan sonra Uyandırma word Cihazınızda etkinleştirmek için Geliştirme Seti dağıtın.

1. Git [özel konuşma hizmeti portalı](https://cris.ai/).

    ![Özel konuşma hizmeti portalı](media/speech-devices-sdk/wake-word-4.png)

1. Azure Active Directory için Davetiyesi e-posta adresiyle oturum açın.

1. **Özel Uyandırma Word** kullanılamaz, ortak var. alan doğrudan bağlantı olduğundan. Özel konuşma tanıma özelliği bir Azure aboneliği gerektirir, ancak özel Uyandırma Word özelliği değil. Aldığınız varsa **Hayır abonelik bulunamadı.** hata sayfası, yalnızca Değiştir **"abonelikleri? errorMessage = yok % 20Subscriptions % 20found"** ile "**customkws**" URL'si ve isabet girin. URL, bunlardan biri olmalıdır: https://westus.cris.ai/customkws, https://eastasia.cris.ai/customkws veya https://northeurope.cris.ai/customkwsbölgenizde nerede bağlı olarak.

1. Tercih ettiğiniz Uyandırma sözcüğü yazın ve ardından **sözcüğü gönderme**.

    ![Uyandırma sözcük girin](media/speech-devices-sdk/wake-word-5.png)

1. Bu dosyaların oluşturulması birkaç dakika sürebilir. Tarayıcı pencerenizde dönen bir daire görmeniz gerekir. Kısa bir süre sonra bir bilgi çubuğu, .zip dosyasını indirmek isteyen görüntülenir.

1. .Zip dosyasını bilgisayarınıza kaydedin. Bu dosya, geliştirme setine özel Uyandırma word dağıtmak için ihtiyacınız. Özel Uyandırma word dağıtmak için yönergeleri izleyin. [konuşma cihaz SDK'sı ile çalışmaya başlama](speech-devices-sdk-qsg.md).

1. Seçin **oturumu kapatın.**

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için alma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) ve konuşma cihaz SDK'sı için kaydolun.

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı için kaydolun](get-speech-devices-sdk.md)
