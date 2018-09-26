---
title: Bir özel Uyandırma sözcük oluşturma
description: Bir özel Uyandırma sözcük için konuşma cihaz SDK'sı oluşturmayı öğrenin.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 7ba62ce0cc2d391c96c31795aabaac9c8796f6d5
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165543"
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

Cihazınızda bir özel Uyandırma sözcük kullanabilmeniz için önce Microsoft özel Uyandırma Word oluşturma hizmetini kullanarak Uyandırma word oluşturmanız gerekir. Bir dosya hizmeti oluşturan bir Uyanma sözcük verdikten sonra Uyandırma word Cihazınızda etkinleştirmek için geliştirme Seti'ni dağıtın.

1. Git [özel konuşma hizmeti portalı](https://cris.ai/).

2. Azure Active Directory Davetiyesi aldığınız e-posta adresi ile yeni bir hesap oluşturun. 

    ![Yeni hesap oluşturun](media/speech-devices-sdk/wake-word-1.png)
 
3.  Oturum açtıktan sonra formu doldurun ve ardından **my yolculuğunuza başlayın**.

    ![başarıyla oturum açıldı](media/speech-devices-sdk/wake-word-3.png)
 
4. **Özel Uyandırma Word** kullanılamaz, ortak var. alan doğrudan bağlantı olduğundan. Özel konuşma tanıma özelliği bir Azure aboneliği gerektirir, ancak özel Uyandırma Word özelliği değil. Aldığınız varsa **Hayır abonelik bulunamadı.** hata sayfası, yalnızca Değiştir **"abonelikleri? errorMessage = yok % 20Subscriptions % 20found"** ile "**customkws**" URL'si ve isabet girin. URL, bunlardan biri olmalıdır: https://westus.cris.ai/customkws, https://eastasia.cris.ai/customkws veya https://northeurope.cris.ai/customkwsbölgenizde nerede bağlı olarak.   


    ![Özel sözcük Uyandırma sayfa](media/speech-devices-sdk/wake-word-4.png)
 
6. Tercih ettiğiniz Uyandırma sözcüğü yazın ve ardından **sözcüğü gönderme**.

    ![Uyandırma sözcük girin](media/speech-devices-sdk/wake-word-5.png)
 
7. Bu dosyaların oluşturulması birkaç dakika sürebilir. Tarayıcı pencerenizde dönen bir daire görmeniz gerekir. Kısa bir süre sonra bir bilgi çubuğu, .zip dosyasını indirmek isteyen görüntülenir.

    ![.Zip dosyasını alma](media/speech-devices-sdk/wake-word-6.png)

8. .Zip dosyasını bilgisayarınıza kaydedin. Bu dosya, geliştirme setine özel Uyandırma word dağıtmak için ihtiyacınız. Özel Uyandırma word dağıtmak için yönergeleri izleyin. [konuşma cihaz SDK'sı ile çalışmaya başlama](speech-devices-sdk-qsg.md).

9. Seçin **oturumu kapatın.**

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için alma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) ve konuşma cihaz SDK'sı için kaydolun.

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı için kaydolun](get-speech-devices-sdk.md)

