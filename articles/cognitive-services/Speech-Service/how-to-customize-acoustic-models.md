---
title: Özel Konuşma Tanıma Hizmeti ile akustik modeli oluşturma - Microsoft Bilişsel Hizmetler
description: Microsoft Bilişsel Hizmetler'de Özel Konuşma Tanıma Hizmeti ile akustik model oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 7f7e008e8fb999ce28cf515fe9af549c309316d4
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285185"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Öğretici: Özel akustik model oluşturma

Özel akustik model oluşturma, uygulamanızın araba gibi belirli bir ortamda, belirli kayıt cihazları veya belirli kullanıcı popülasyonuyla kullanılacak şekilde tasarlanmış olması halinde yararlı olacaktır. Örnek olarak aksanlı konuşma, belirli arka plan görüntüleri veya kayıt için belirli bir mikrofonun kullanılması verilebilir.

Bu sayfada şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Verileri hazırlama
> * Akustik veri kümesini içeri aktarma
> * Özel akustik model oluşturma

Bilişsel Hizmetler hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/try/cognitive-services) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

[Cognitive Services Subscriptions](https://customspeech.ai/Subscriptions) (Bilişsel Hizmetler Abonelikleri) sayfasını açarak Bilişsel Hizmetler hesabınızın bir aboneliğe bağlanmış olduğundan emin olun.

**Connect existing subscription** (Var olan aboneliğe bağlan) düğmesine tıklayarak Azure portalında oluşturulmuş olan bir Özel Konuşma Tanıma Hizmeti aboneliğine bağlanabilirsiniz.

Özel Konuşma Tanıma Hizmeti aboneliği oluşturma hakkında bilgi için [Kullanmaya Başlama](get-started.md) sayfasına bakın.

## <a name="prepare-the-data"></a>Verileri hazırlama

Bir akustik modeli belirli bir etki alanı için özelleştirmek üzere konuşma verilerinden oluşan bir koleksiyona ihtiyaç duyulur. Bu veriler birkaç konuşmadan yüzlerce saatlik konuşmaya kadar değişebilir. Bu koleksiyon, konuşma verilerinden oluşan ses dosyası kümesine ve her ses dosyasının transkripsiyonunu içeren bir metin dosyasından oluşur. Ses verilerinin tanıyıcıyı kullanmak istediğiniz senaryoyu temsil etmesi gerekir.

Örnek:

*   Gürültülü bir fabrika ortamındaki konuşmayı daha iyi bir şekilde tanımak istiyorsanız ses dosyaları gürültülü bir fabrika ortamında konuşan kişileri içermelidir.
*   Örneğin tek bir kullanıcı için performansı iyileştirmek istiyorsanız FDR’nin Fireside Chats içeriğinin tamamını yazıya dökerek ses dosyalarının yalnızca bu konuşmacıya ait olan birçok örnek içermesini sağlamanız gerekir.

Akustik modeli özelleştirmek üzere kullanılacak akustik veri kümesi iki bölümden oluşur: (1) konuşma verilerini içeren ses dosyası kümesi ve (2) tüm ses dosyalarının transkripsiyonunu içeren bir dosya.

### <a name="audio-data-recommendations"></a>Ses verisi önerileri

*   Veri kümesindeki tüm ses dosyalarının WAV (RIFF) ses biçiminde kaydedilmesi gerekir.
*   Sesin örnekleme oranı 8 kHz veya 16 kHz, örnek değerlerin de sıkıştırılmamış PCM 16 bit işaretli tamsayılar (kısa değerler) olması gerekir.
*   Yalnızca tek kanallı (mono) ses dosyaları desteklenir.
*   Ses dosyalarının uzunluğunun 100 ms ile 1 dakika arasında olması gerekir. İdeal olarak her ses dosyası en az 100 ms sessizlikle başlayıp bitmelidir ve genelde bu süre 500 ms ile 1 saniye arasında tutulur.
*   Verilerinizde arka plan gürültüsü varsa daha uzun sessizlik sürelerine sahip örnekler de kullanmanız önerilir. Bu durumda verilerinizde konuşma içeriğinin öncesinde ve/veya sonrasında birkaç saniyelik sessizlik süresi olabilir.
*   Her ses dosyasında tek bir konuşma, başka bir deyişle tek bir cümle, tek bir sorgu veya bir diyaloğun tek bir tarafını içermelidir.
*   Veri kümesindeki her ses dosyası benzersiz bir dosya adına ve “wav” uzantısına sahip olmalıdır.
*   Ses dosyası kümesi alt dizin olmadan tek bir klasöre yerleştirilmeli ve ses dosyası kümesinin tamamı tek bir ZIP dosyası halinde arşivlenmelidir.

> [!NOTE]
> Web portalı aracılığıyla gerçekleştirilen veri içeri aktarma işlemleri şu an için 2 GB ile sınırlı olduğundan bir akustik veri kümesi bu boyutu aşamaz. Bu da 16 kHz kaliteyle yaklaşık 17 saate, 8 kHz ile de yaklaşık 34 saate denk gelir. Ses verilerinin ana gereksinimleri aşağıdaki tabloda özetlenmiştir.
>

| Özellik | Değer |
|---------- |----------|
| Dosya Biçimi | RIFF (WAV) |
| Örnekleme Oranı | 8000 Hz veya 16.000 Hz |
| Kanallar | 1 (mono) |
| Örnek Biçimi | PCM, 16 bit tamsayılar |
| Dosya Süresi | 0,1 saniye < süre < 60 saniye |
| Sessizlik Payı | > 0,1 saniye |
| Arşiv Biçimi | Zip |
| Maksimum Arşiv Boyutu | 2 GB |

## <a name="language-support"></a>Dil desteği

Özel **Konuşmayı Metne Dönüştürme** dil modellerinde aşağıdaki diller desteklenir.

[Desteklenen dillerin](supported-languages.md) tam listesine ulaşmak için tıklayın

### <a name="transcriptions-for-the-audio-dataset"></a>Ses veri kümesi için transkripsiyonlar

Tüm WAV dosyalarının transkripsiyonları tek bir düz metin dosyasına yerleştirilmelidir. Transkripsiyon dosyasının her satırında ses dosyalarından birinin adı ve transkripsiyonu bulunmalıdır. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır.

  Örnek:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Transkripsiyon UTF-8 Bom ile kodlanmış olmalıdır

Transkripsiyon metinleri sistem tarafından işlenebilmesi için normalleştirilecektir. Ancak veriler Özel Konuşma Tanıma Hizmeti'ne yüklenmeden _önce_ kullanıcı tarafından gerçekleştirilmesi gereken bazı önemli normalleştirme adımları vardır. Transkripsiyonlarınızı hazırlarken dile özgü [transkripsiyon yönergeleri](prepare-transcription.md) bölümüne bakın.

Aşağıdaki adımlar [Özel Konuşma Tanıma Hizmeti Portalı](https://customspeech.ai) kullanılarak gerçekleştirilir.

## <a name="import-the-acoustic-data-set"></a>Akustik veri kümesini içeri aktarma

Ses dosyaları ve transkripsiyonlar hazırlandıktan sonra hizmetin web portalından içeri aktarılabilir.

Bu işlemi gerçekleştirmek için [Özel Konuşma Tanıma Hizmeti Portalı](https://customspeech.ai)'nda oturum açtığınızdan emin olun. Ardından üst şeritteki “Custom Speech” (Özel Konuşma) açılan menüsüne tıklayıp “Adaptation Data” (Uyarlama Verileri) öğesini seçin. Özel Konuşma Tanıma Hizmeti'ne ilk kez veri yüklüyorsanız “Datasets” (Veri Kümeleri) adlı boş bir tablo görürsünüz. 

"Acoustic Datasets" (Akustik Veri Kümeleri) satırındaki "Import" (İçeri Aktar) düğmesine tıkladığınızda sitede yeni veri kümesi yükleme sayfası açılır.

![Deneme](media/stt/speech-acoustic-datasets-import.png)

_Name_ (Ad) ve _Description_ (Açıklama) metin kutularına uygun girişleri yapın. Yüklediğiniz farklı veri kümelerini takip etmek için kolay anlaşılır açıklamalar kullanmanız önerilir. Ardından “Transcription File” (Transkripsiyon Dosyaları) ve “WAV files” (WAV dosyaları) için “Choose File" (Dosya Seç) öğesine tıklayarak düz metin transkripsiyon dosyanızı ve WAV dosyalarını içeren ZIP arşivini seçin. Hazırlık işlemlerini tamamladığınızda verilerinizi yüklemek için “Import” (İçeri Aktar) öğesine tıklayın. Verileriniz yüklenir. Daha büyük veri kümelerinde bu işlem birkaç dakika sürebilir.

Yükleme işlemi tamamlandığında "Acoustic Datasets" (Akustik Veri Kümeleri) tablosuna dönecek ve akustik veri kümenizi gösteren bir giriş göreceksiniz. Bu kümeye benzersiz tanımlayıcı (GUID) atanmış olduğuna dikkat edin. Ayrıca verilerin geçerli durumunu gösteren bir durum alanı da bulunur. Durum işlenmek üzere kuyruğa alınmış olduğunda “NotStarted” (Başlatılmadı), doğrulama sürecinde “Running” (Çalışıyor), kullanıma hazır olduğunda ise “Complete” (Tamamlandı) olur.

Veri doğrulama aşamasında bir dizi kontrol gerçekleştirilerek ses dosyalarında dosya biçimi, uzunluğu ve örnekleme oranı doğrulama; transkripsiyon dosyalarında ise dosya biçimi doğrulama ve metin normalleştirme işlemleri gerçekleştirilir.

Durum “Succeeded” (Başarılı) olduğunda “Details” (Ayrıntılar) öğesine tıklayarak akustik veri doğrulama raporunu görebilirsiniz. Doğrulama adımında başarılı ve başarısız olan konuşma sayısının yanı sıra başarısız olan konuşmalar hakkında ayrıntılı bilgiler gösterilir. Aşağıdaki örnekte iki WAV dosyası, ses biçimi uygun olmadığından doğrulamadan geçememiştir (bu veri kümesinde biri hatalı örnekleme oranına, diğeri ise hatalı dosya biçimine sahiptir).

![Deneme](media/stt/speech-acoustic-datasets-report.png)

Herhangi bir noktada veri kümesinin Name (Ad) veya Description (Açıklama) alanında değişiklik yapmak istediğinizde “Edit” (Düzenle) bağlantısına tıklayabilirsiniz. Ses dosyalarını veya transkripsiyonları değiştiremezsiniz.

## <a name="create-a-custom-acoustic-model"></a>Özel akustik model oluşturma

Akustik veri kümenizin durumu “Complete” (Tamamlandı) olduğunda özel akustik model oluşturmak için kullanabilirsiniz. Bunu yapmak için “Custom Speech” (Özel Konuşma) açılan menüsündeki “Acoustic Models” (Akustik Modeller) öğesine tıklayın. Tüm özel akustik modellerinizi listeleyen "Your models" (Modelleriniz) adlı bir tablo göreceksiniz. İlk kullanımda bu tablo boş olacaktır. Geçerli yerel ayar, tablo başlığında gösterilir. Akustik modeller şu an için yalnızca İngilizce (ABD) dilinde oluşturulabilir.

Yeni bir model oluşturmak için tablo başlığının altındaki “Create New” (Yeni Oluştur) öğesine tıklayın. Yukarıda yaptığınız gibi modeli tanımlamanıza yardımcı olması için bir ad ve açıklama girin. Örneğin "Description" (Açıklama) alanını modeli oluşturmak için kullanılan başlangıç modelini ve akustik veri kümesini kaydetmek için kullanabilirsiniz. Ardından açılan menünden bir “Base Acoustic Model” (Temel Akustik Model) seçimi yapın. Temel model, özelleştirme için kullanacağınız başlangıç noktasıdır. İki temel akustik modelden birini seçebilirsiniz. _Microsoft Search ve Dictation AM_; komutlar, arama sorguları veya dikte gibi uygulamaya yönlendirilen konuşmalar için uygundur. _Microsoft Conversational model_, günlük konuşma tarzındaki konuşmaları tanımak için uygundur. Bu konuşma türü genelde başka bir kişiye hitaben yapılır ve çağrı merkezinde veya toplantılarda kullanılır. Kısmi sonuçlar için gecikme süresi Conversational modellerde, Search ve Dictation modellerine kıyasla daha yüksektir.

> [!NOTE]
> Bu senaryoların tümünü hedefleyen yeni bir Universal modeli kullanıma sunuyoruz. Yukarıdaki modeller de genel kullanıma açık kalacaktır.

Ardından açılan menüyü kullanarak özelleştirmek istediğiniz akustik verilerini seçin.

![Deneme](media/stt/speech-acoustic-models-create2.png)

İşleme tamamlandıktan sonra isteğe bağlı olarak yeni modelinizin doğruluk testini gerçekleştirebilirsiniz. Bu işlem belirtilen akustik veri kümesinde özelleştirilmiş akustik modeli kullanarak konuşmayı metne dönüştürme değerlendirmesi gerçekleştirecek ve sonuçları bildirecektir. Bu testi gerçekleştirmek için “Accuracy Testing” (Doğruluk Testi) onay kutusunu seçin. Ardından açılan menünden bir dil modeli seçin. Özel dil modeli oluşturmadıysanız açılan listede yalnızca temel dil modelleri bulunur. Kılavuzdaki temel dil modellerinin [açıklamasına](how-to-customize-language-model.md) bakın ve en uygun olanı seçin.

Son olarak özel modeli değerlendirmek için kullanmak istediğiniz akustik veri kümesini seçin. Doğruluk testini gerçekleştiriyorsanız modelin performansı hakkında gerçekçi bir izlenim almak için model oluşturma sırasında kullanılandan farklı bir akustik veri kümesi seçmeniz önemlidir. Eğitim verilerinin doğruluğunun test edilmesi, uyarlanmış modelin gerçek koşullarda göstereceği performansı değerlendirmenizi sağlamaz. Sonuç fazla iyimser olacaktır. Doğruluk testinin 1000 konuşmayla sınırlı olduğunu da unutmayın. Akustik veri kümesi daha büyükse yalnızca ilk 1000 konuşma değerlendirilir.

Özelleştirme işlemini çalıştırmaya hazır olduğunuzda “Create” (Oluştur) öğesine tıklayın.

Akustik modeller tablosunda bu yeni modele karşılık gelen yeni bir giriş göreceksiniz. İşlemin durumu tabloda gösterilir. Durumlar “Waiting” (Beklemede), “Processing” (İşleniyor) ve “Complete” (Tamamlandı) olacaktır.

![Deneme](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# dilinde konuşmayı algılama](quickstart-csharp-dotnet-windows.md)
- [Git Örnek Verileri](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
