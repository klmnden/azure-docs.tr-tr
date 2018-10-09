---
title: 'Öğretici: Akustik bir model oluşturma - Özel Konuşma Hizmeti'
titlesuffix: Azure Cognitive Services
description: Bu öğreticide Özel Konuşma Hizmeti ile bir akustik model oluşturmayı öğreneceksiniz.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 72c5a0dfb8f33f273ba850378c1fefeef82b4d7a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220251"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Öğretici: Özel akustik model oluşturma

Bu öğreticide, uygulamanızın tanımasını beklediğiniz konuşma verileri için özel bir akustik model oluşturacaksınız. Özel akustik model oluşturma, uygulamanızın gürültülü bir fabrika veya belirli bir kullanıcı popülasyonu gibi belirli bir ortamda kullanılacak şekilde tasarlanmış olması halinde yararlı olacaktır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Verileri hazırlama
> * Akustik veri kümesini içeri aktarma
> * Özel akustik model oluşturma

Bilişsel Hizmetler hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://cris.ai) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

[Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Bilişsel Hizmetler Abonelikleri) sayfasını açarak Bilişsel Hizmetler hesabınızın bir aboneliğe bağlanmış olduğundan emin olun.

Herhangi bir abonelik listelenmiyorsa **Get free subscription** (Ücretsiz abonelik oluştur) düğmesine tıklayarak Bilişsel Hizmetler'in sizin için bir hesap oluşturmasını sağlayabilirsiniz. Alternatif olarak **Connect existing subscription** (Var olan aboneliğe bağlan) düğmesine tıklayarak Azure portalında oluşturulmuş olan bir Özel Arama Hizmeti aboneliğine bağlanabilirsiniz.

Azure portalında Özel Arama Hizmeti aboneliği oluşturma hakkında bilgi için bkz. [Azure portalında Bilişsel Hizmetler API'si hesabı oluşturma](../../cognitive-services-apis-create-account.md).

## <a name="prepare-the-data"></a>Verileri hazırlama

Akustik modeli belirli bir etki alanına özelleştirmek üzere konuşma verilerinden oluşan bir koleksiyona ihtiyaç duyulur. Bu koleksiyon, konuşma verilerinden oluşan ses dosyası kümesine ve her ses dosyasının transkripsiyonunu içeren bir metin dosyasından oluşur. Ses verilerinin tanıyıcıyı kullanmak istediğiniz senaryoyu temsil etmesi gerekir.

Örnek:

*   Gürültülü bir fabrika ortamındaki konuşmayı daha iyi bir şekilde tanımak istiyorsanız ses dosyaları gürültülü bir fabrika ortamında konuşan kişileri içermelidir.
<a name="Preparing data to customize the acoustic model"></a>
*   Örneğin tek bir kullanıcı için performansı iyileştirmek istiyorsanız FDR’nin Fireside Chats içeriğinin tamamını yazıya dökerek ses dosyalarının yalnızca bu konuşmacıya ait olan birçok örnek içermesini sağlamanız gerekir.

Akustik modeli özelleştirmek üzere kullanılacak akustik veri kümesi iki bölümden oluşur: (1) konuşma verilerini içeren ses dosyası kümesi ve (2) tüm ses dosyalarının transkripsiyonunu içeren bir dosya.

### <a name="audio-data-recommendations"></a>Ses verisi önerileri

*   Veri kümesindeki tüm ses dosyalarının WAV (RIFF) ses biçiminde kaydedilmesi gerekir.
*   Sesin örnekleme oranı 8 kHz veya 16 kHz, örnek değerlerin de sıkıştırılmamış PCM 16 bit işaretli tamsayılar (kısa değerler) olması gerekir.
*   Yalnızca tek kanallı (mono) ses dosyaları desteklenir.
*   Ses dosyalarının uzunluğunun 100 ms ile 1 dakika arasında olması gerekir. İdeal olarak her ses dosyası en az 1 ms sessizlikle başlayıp 100 ms sessizlikle bitmelidir ve genelde bu süre 500 ms ile 1 saniye arasında tutulur.
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
| Örnekleme Oranı | 8000 Hz veya 16000 Hz |
| Kanallar | 1 (mono) |
| Örnek Biçimi | PCM, 16 bit tamsayılar |
| Dosya Süresi | 0,1 saniye < süre < 60 saniye |
| Sessizlik Payı | > 0,1 saniye |
| Arşiv Biçimi | Zip |
| Maksimum Arşiv Boyutu | 2 GB |

### <a name="transcriptions"></a>Transkripsiyonlar

Tüm WAV dosyalarının transkripsiyonları tek bir düz metin dosyasına yerleştirilmelidir. Transkripsiyon dosyasının her satırında ses dosyalarından birinin adı ve transkripsiyonu bulunmalıdır. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır.

  Örnek:
```
  speech01.wav  speech recognition is awesome

  speech02.wav  the quick brown fox jumped all over the place

  speech03.wav  the lazy dog was not amused
```

Transkripsiyon metinleri sistem tarafından işlenebilmesi için normalleştirilecektir. Ancak veriler Özel Konuşma Tanıma Hizmeti'ne yüklenmeden _önce_ kullanıcı tarafından gerçekleştirilmesi gereken bazı önemli normalleştirme adımları vardır. Transkripsiyonlarınızı hazırlarken dile özgü [transkripsiyon yönergeleri](cognitive-services-custom-speech-transcription-guidelines.md) bölümüne bakın.

Aşağıdaki adımlar [Özel Konuşma Tanıma Hizmeti Portalı](https://cris.ai) kullanılarak gerçekleştirilir. 

## <a name="import-the-acoustic-data-set"></a>Akustik veri kümesini içeri aktarma

Ses dosyaları ve transkripsiyonlar hazırlandıktan sonra hizmetin web portalından içeri aktarılabilir.

Bu işlemi gerçekleştirmek için [Özel Konuşma Tanıma Hizmeti Portalı](https://cris.ai)'nda oturum açtığınızdan emin olun. Ardından üst şeritteki “Custom Speech” (Özel Konuşma) açılan menüsüne tıklayıp “Adaptation Data” (Uyarlama Verileri) öğesini seçin. Özel Konuşma Tanıma Hizmeti'ne ilk kez veri yüklüyorsanız “Datasets” (Veri Kümeleri) adlı boş bir tablo görürsünüz. 

"Acoustic Datasets" (Akustik Veri Kümeleri) satırındaki "Import" (İçeri Aktar) düğmesine tıkladığınızda sitede yeni veri kümesi yükleme sayfası açılır.

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-import.png)

_Name_ (Ad) ve _Description_ (Açıklama) metin kutularına uygun girişleri yapın. Bunlar, yüklediğiniz farklı veri kümelerini takip etmek için kullanışlıdır. Ardından “Transcription File” (Transkripsiyon Dosyaları) ve “WAV files” (WAV dosyaları) için “Choose File" (Dosya Seç) öğesine tıklayarak düz metin transkripsiyon dosyanızı ve WAV dosyalarını içeren ZIP arşivini seçin. Bu işlemleri tamamladığınızda verilerinizi yüklemek için “Import” (İçeri Aktar) öğesine tıklayın. Verileriniz yüklenir. Daha büyük veri kümelerinde bu işlem birkaç dakika sürebilir.

Yükleme işlemi tamamlandığında "Acoustic Datasets" (Akustik Veri Kümeleri) tablosuna dönecek ve akustik veri kümenizi gösteren bir giriş göreceksiniz. Bu kümeye benzersiz tanıtıcı (GUID) atanmış olduğuna dikkat edin. Ayrıca verilerin geçerli durumunu gösteren bir durum alanı da bulunur. Durum işlenmek üzere kuyruğa alınmış olduğunda “Waiting” (Beklemede), doğrulama sürecinde “Processing” (İşleniyor), kullanıma hazır olduğunda ise “Complete” (Tamamlandı) olur.

Veri doğrulama aşamasında bir dizi kontrol gerçekleştirilerek ses dosyalarında dosya biçimi, uzunluğu ve örnekleme oranı doğrulama; transkripsiyon dosyalarında ise dosya biçimi doğrulama ve metin normalleştirme işlemleri gerçekleştirilir.

Durum “Complete” (Tamamlandı) olduğunda “Details” (Ayrıntılar) öğesine tıklayarak akustik veri doğrulama raporunu görebilirsiniz. Doğrulama adımında başarılı ve başarısız olan konuşma sayısının yanı sıra başarısız olan konuşmalar hakkında ayrıntılı bilgiler gösterilir. Aşağıdaki örnekte iki WAV dosyası, ses biçimi uygun olmadığından doğrulamadan geçememiştir (bu veri kümesinde biri hatalı örnekleme oranına, diğeri ise hatalı dosya biçimine sahiptir).

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-report.png)

Herhangi bir noktada veri kümesinin Name (Ad) veya Description (Açıklama) alanında değişiklik yapmak istediğinizde “Edit” (Düzenle) bağlantısına tıklayabilirsiniz. Ses dosyalarını veya transkripsiyonları değiştiremeyeceğinizi unutmayın.

## <a name="create-a-custom-acoustic-model"></a>Özel akustik model oluşturma

Akustik veri kümenizin durumu “Complete” (Tamamlandı) olduğunda özel akustik model oluşturmak için kullanabilirsiniz. Bunu yapmak için “Custom Speech” (Özel Konuşma) açılan menüsündeki “Acoustic Models” (Akustik Modeller) öğesine tıklayın. Tüm özel akustik modellerinizi listeleyen "Your models" (Modelleriniz) adlı bir tablo göreceksiniz. İlk kullanımda bu tablo boş olacaktır. Geçerli yerel ayar, tablo başlığında gösterilir. Akustik modeller şu an için yalnızca İngilizce (ABD) dilinde oluşturulabilir.

Yeni bir model oluşturmak için tablo başlığının altındaki “Create New” (Yeni Oluştur) öğesine tıklayın. Yukarıda yaptığınız gibi modeli tanımlamanıza yardımcı olması için bir ad ve açıklama girin. Örneğin "Description" (Açıklama) alanını modeli oluşturmak için kullanılan başlangıç modelini ve akustik veri kümesini kaydetmek için kullanabilirsiniz. Ardından açılan menünden bir “Base Acoustic Model” (Temel Akustik Model) seçimi yapın. Temel model, özelleştirme için kullanacağınız başlangıç noktasıdır. İki temel akustik modelden birini seçebilirsiniz. _Microsoft Search and Dictation AM_; komutlar, arama sorguları veya dikte gibi uygulamaya yönlendirilen konuşmalar için uygundur. _Microsoft Conversational model_, günlük konuşma tarzındaki konuşmaları tanımak için uygundur. Bu konuşma türü genelde başka bir kişiye hitaben yapılır ve çağrı merkezinde veya toplantılarda kullanılır. Kısmi sonuçlar için gecikme süresinin Conversational modellerde, Search ve Dictation modellerine kıyasla daha yüksek olduğunu unutmayın.

Ardından açılan menüyü kullanarak özelleştirmek istediğiniz akustik verilerini seçin.

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-create2.png)

İşleme tamamlandıktan sonra isteğe bağlı olarak yeni modelinizin çevrimdışı testini gerçekleştirebilirsiniz. Bu işlem belirtilen akustik veri kümesinde özelleştirilmiş akustik modeli kullanarak konuşmayı metne dönüştürme değerlendirmesi gerçekleştirecek ve sonuçları bildirecektir. Bu testi gerçekleştirmek için “Accuracy Testing” (Doğruluk Testi) onay kutusunu seçin. Ardından açılan menünden bir dil modeli seçin. Özel dil modeli oluşturmadıysanız açılan listede yalnızca temel dil modelleri bulunur. Kılavuzdaki temel dil modellerinin [açıklamasına](cognitive-services-custom-speech-create-language-model.md) bakın ve en uygun olanı seçin.

Son olarak özel modeli değerlendirmek için kullanmak istediğiniz akustik veri kümesini seçin. Doğruluk testini gerçekleştiriyorsanız modelin performansı hakkında gerçekçi bir izlenim almak için model oluşturma sırasında kullanılandan farklı bir akustik veri kümesi seçmeniz önemlidir. Çevrimdışı testin 1000 konuşmayla sınırlı olduğunu da unutmayın. Akustik veri kümesi bundan daha büyükse yalnızca ilk 1000 konuşma değerlendirilir.

Özelleştirme işlemini çalıştırmaya hazır olduğunuzda “Create” (Oluştur) öğesine tıklayın.

Akustik modeller tablosunda bu yeni modele karşılık gelen yeni bir giriş göreceksiniz. İşlemin durumu tabloda gösterilir. Durumlar “Waiting” (Beklemede), “Processing” (İşleniyor) ve “Complete” (Tamamlandı) olacaktır.

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ses dosyaları ve transkripsiyonlar ile kullanmak için özel bir akustik model geliştirdiniz. Metin dosyaları ile kullanacağınız özel bir dil dosyası oluşturmak için özel bir dil modeli oluşturma öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Özel akustik model oluşturma](cognitive-services-custom-speech-create-language-model.md)
