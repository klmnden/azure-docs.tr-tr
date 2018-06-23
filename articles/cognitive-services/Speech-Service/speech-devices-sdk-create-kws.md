---
title: Bir özel Uyandırma sözcük oluşturma | Microsoft Docs
description: Özel Uyandırma word için konuşma aygıtları SDK'sı oluşturuluyor.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 2575ed24bb931ca4da05dd6663b976406af590e6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355667"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Konuşma hizmetini kullanarak bir özel Uyandırma sözcük oluşturma

Cihazınızı her zaman bir Uyandırma sözcük (veya tümceciği) dinliyor. Örneğin, "Hey Cortana" Cortana Yardımcısı için bir Uyanma sözcüktür. Kullanıcı uyku modundan çıkarma word istendiğinde cihaz konuşarak kullanıcı durdurur kadar tüm sonraki ses buluta göndermeye başlar. Uyandırma word özelleştirme Cihazınızı ayırt ve marka bilgilerinizi güçlendirmek için etkili bir yoldur.

Bu makalede, cihazınız için bir özel Uyandırma sözcük oluşturmayı öğrenin.

## <a name="choosing-an-effective-wake-word"></a>Etkili Uyandırma word seçme

Uyandırma word seçerken aşağıdaki yönergeleri göz önünde bulundurun.

* Uyandırma word İngilizce bir sözcük veya tümcecik olmalıdır. Bunu söylemek için iki saniyeden daha uzun sürer.

* 4 – 7 hece sözcüklük en iyi çalışır. Örneğin, zayıf bir yalnızca "Hey" iken "Hey, bilgisayar" bir iyi Uyandırma word dosyasıdır.

* Uyandırma sözcükler ortak İngilizce telaffuz kurallarını izlemelidir.

* Ortak İngilizce telaffuz kurallarını izleyen bir benzersiz veya hatta made-up sözcük hatalı pozitif sonuç azaltabilir. Örneğin, "computerama" iyi Uyandırma word olabilir.

* Ortak bir sözcük seçmeyin. Örneğin, "yemek" ve "kişiler sık sıradan konuşmada söyleyin sözcüklerdir gidin". Bunlar, cihazınız için false Tetikleyicileri olabilir.

* Söyleyiş olabilir Uyandırma word kullanmaktan kaçının. Kullanıcılar cihazlarını yanıt almak için "doğru" telaffuz bilmesi gerekir. "Beş dokuz sıfır olarak" Örneğin, "509" belirgin, "beş dokuz", veya "beş yüz ve dokuz." "R.E.I." belirgin olarak "R E ediyorum" veya "Ray." "Live" belirgin [līv] veya [LIV].

* Özel karakterler, simgeler veya rakamları kullanmayın. Örneğin, "Git #" ve "20 + kediler" iyi Uyandırma sözcükler olmayacaktır. Ancak, "sharp Git" veya "yirmi kediler artı" işe yarayabilir. Yine, marka simgeleri kullanın ve uygun telaffuz pekiştirmek için pazarlama ve belgeleri kullanın.

> [!NOTE]
> Bir ticari marka olarak kaydettirilmiş sözcük Uyandırma word olarak seçerseniz, o ticari marka sahibi, aksi takdirde kullanmak için ticari marka sahibinden izniniz emin olun. Microsoft, uyku modundan çıkarma word seçiminize göre ortaya çıkabilecek sorunları yasal sorumlu değildir.

## <a name="creating-your-wake-word"></a>Uyandırma word oluşturma

Bir özel Uyandırma sözcük cihazınızla kullanabilmeniz için önce Microsoft özel Uyandırma Word oluşturma hizmetinin kullanarak oluşturmanız gerekir. Uyandırma word, bir dosya hizmeti üretir verdikten sonra Uyandırma word Cihazınızda etkinleştirmek için Geliştirme Seti üzerine dağıtırsınız.

1. Git [özel konuşma hizmet portalı](https://cris.ai/).

2. Azure Active Directory için daveti aldığınız e-posta adresi ile yeni bir hesap oluşturun. 

    ![Yeni hesap oluştur](media/speech-devices-sdk/wake-word-1.png)
 
3.  Oturum açtıktan sonra formu doldurun ve ardından **gezisine başlatın.**

    ![başarıyla oturum açtı](media/speech-devices-sdk/wake-word-3.png)
 
4. **Özel Uyandırma Word** sayfa ortak olarak kullanılabilir değil; böylece var. götüren bir bağlantı. ' A tıklayın veya bu bağlantıyı yerine yapıştırın: https://cris.ai/customkws.

    ![Gizli sayfa](media/speech-devices-sdk/wake-word-4.png)
 
6. Seçiminizden sonra Uyandırma Word'de türünü **gönderme** onu.

    ![Uyandırma sözcüğü girin](media/speech-devices-sdk/wake-word-5.png)
 
7. Oluşturulacak dosyaları için birkaç dakika sürebilir. Tarayıcınızın sekmesinde dönen bir daire görürsünüz. İndirmek isteyen bir bilgi çubuğu, bir süre sonra görünür bir `.zip` dosyası.

    ![.zip dosyasını alma](media/speech-devices-sdk/wake-word-6.png)

8. Kaydet `.zip` bilgisayarınıza dosya. Bölümündeki yönergeleri izleyerek özel Uyandırma word Geliştirme Seti dağıtmak için bu dosyaya gereksinim [konuşma aygıtları SDK'sı ile çalışmaya başlama](speech-devices-sdk-qsg.md).

9. Şimdi olabilir **oturumu kapatın.**

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için alma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) ve konuşma aygıtları SDK'sı için kaydolun.

> [!div class="nextstepaction"]
> [Konuşma aygıtları SDK'sı için kaydolun](get-speech-devices-sdk.md)

