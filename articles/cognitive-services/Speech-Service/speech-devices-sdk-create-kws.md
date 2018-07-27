---
title: Bir özel Uyandırma sözcük oluşturma
description: Bir özel Uyandırma sözcük için konuşma cihaz SDK'sı oluşturuluyor.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 615a901c70fff92141442699ea6e4b8fce1c9ace
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282582"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Konuşma hizmeti ile özel Uyandırma sözcük oluşturma

Cihazınız, her zaman bir Uyanma sözcük (veya tümceciği) dinliyor. Örneğin, "Hey Cortana" Cortana Yardımcısı için bir Uyanma sözcüktür. Uyandırma word kullanıcı diyor, cihaz kullanıcının dikte durdurur kadar tüm sonraki ses buluta göndermeyi başlatır. Uyandırma Word'ün özelleştirme, Cihazınızı ayırt ve marka bilgilerinizi güçlendirmek için etkili bir yoludur.

Bu makalede, özel Uyandırma word cihazınız için nasıl oluşturulduğunu öğrenin.

## <a name="choosing-an-effective-wake-word"></a>Etkili Uyandırma sözcük seçme

Uyandırma word seçerken aşağıdaki yönergeleri göz önünde bulundurun.

* Uyandırma Word'ün İngilizce bir sözcük veya ifade olmalıdır. Söyleyin iki saniyeden daha uzun sürer.

* 4 – 7 hece sözcükleri en iyi çalışır. Örneğin, yalnızca "Merhaba" zayıf bir durumdayken "Hey, bilgisayar" bir iyi Uyandırma sözcük olur.

* Uyandırma sözcükleri ortak İngilizce telaffuz kurallarını izlemelidir.

* Ortak İngilizce telaffuz kurallarını izleyen bir benzersiz veya hatta made-up sözcük hatalı pozitif sonuçları azaltmak. Örneğin, "computerama" iyi Uyandırma word olabilir.

* Ortak bir sözcük seçmeyin. Örneğin, "yemek" ve "kişiler sık sıradan konuşmada söyleyin sözcükler Git". Bunlar, cihazınız için false Tetikleyiciler olabilir.

* Söyleyiş olabilir bir Uyanma sözcük kullanmaktan kaçının. Kullanıcılar, cihazlarını yanıt almak için "doğru" telaffuz bilmesi gerekir. "Beş dokuz sıfır olarak" Örneğin, "509" belirgin, "beş oh dokuz", veya "beş yüz ve dokuz." "R.E.I." olarak telaffuz "R E miyim" veya "Ray." "Canlı" telaffuz [līv] veya [LIV].

* Özel karakterler, simgeler veya basamak kullanmayın. Örneğin, "Git #" ve "20 + kediler" iyi Uyandırma sözcükleri olmaz. Ancak, "NET Git" veya "yirmi kediler artı" işe yarayabilir. Yine de, marka simgeleri kullanın ve uygun telaffuz güçlendirmek için pazarlama ve belgeleri kullanın.

> [!NOTE]
> Bir ticari marka olarak kaydettirilmiş sözcük Uyandırma word'olarak seçerseniz, bu ticari marka sahibi, aksi takdirde kullanmak için ticari marka sahibinden iznine sahip emin olun. Microsoft, Uyandırma word'ün seçiminize göre kaynaklanabilecek yasal sorunları sorumlu değildir.

## <a name="creating-your-wake-word"></a>Uyandırma Word'ün oluşturma

Cihazınızda bir özel Uyandırma sözcük kullanabilmeniz için önce Microsoft özel Uyandırma Word oluşturma hizmetinin kullanarak oluşturmanız gerekir. Bir dosya hizmeti oluşturan bir Uyanma sözcük verdikten sonra daha sonra Uyandırma word Cihazınızda etkinleştirmek için dev Seti üzerine dağıtırsınız.

1. Git [özel konuşma hizmeti portalı](https://cris.ai/).

2. Azure Active Directory Davetiyesi aldığınız e-posta adresi ile yeni bir hesap oluşturun. 

    ![Yeni hesap oluşturun](media/speech-devices-sdk/wake-word-1.png)
 
3.  Oturum açtıktan sonra formu doldurun ve ardından tıklayın **yolculuğunuza başlayın.**

    ![başarıyla oturum açtı](media/speech-devices-sdk/wake-word-3.png)
 
4. **Özel Uyandırma Word** kullanılamaz, ortak var. götüren bir bağlantı olması. ' A tıklayın veya bu bağlantıyı yapıştırabilirsiniz: https://cris.ai/customkws.

    ![Gizli sayfası](media/speech-devices-sdk/wake-word-4.png)
 
6. Uyandırma sözcüğünü yazın, istediğiniz ardından **Gönder** bu.

    ![Uyandırma sözcük girin](media/speech-devices-sdk/wake-word-5.png)
 
7. Bu dosyaların oluşturulması birkaç dakika sürebilir. Tarayıcınızın sekmesinde dönen bir daire görmeniz gerekir. Kısa bir süre sonra indirmek isteyen bir bilgi çubuğu görünür bir `.zip` dosya.

    ![.zip dosyasını alma](media/speech-devices-sdk/wake-word-6.png)

8. Kaydet `.zip` dosyayı bilgisayarınıza. Bölümündeki yönergeleri izleyerek özel Uyandırma word geliştirme setine dağıtmak için bu dosyaya ihtiyacınız [konuşma cihaz SDK'sı ile çalışmaya başlama](speech-devices-sdk-qsg.md).

9. Artık olabilir **oturumu kapatın.**

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için alma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) ve konuşma cihaz SDK'sı için kaydolun.

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı için kaydolun](get-speech-devices-sdk.md)

