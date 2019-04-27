---
title: 'Öğretici: Konuşma hizmeti sayesinde akustik model oluşturma'
titlesuffix: Azure Cognitive Services
description: Konuşma Hizmetleri kullanarak Azure'da bir akustik model oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: f2a111558fa3f515b797745dc51e32f625bbd91f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60539154"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Öğretici: Özel akustik model oluşturma

Özel akustik model oluşturma, uygulamanızın araba gibi belirli bir ortamda, belirli kayıt cihazları veya belirli kullanıcı popülasyonuyla kullanılacak şekilde tasarlanmış olması halinde yararlı olacaktır. Örnek olarak aksanlı konuşma, belirli arka plan görüntüleri veya kayıt için belirli bir mikrofonun kullanılması verilebilir.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Verileri hazırlama
> * Akustik veri kümesini içeri aktarma
> * Özel akustik model oluşturma

Azure Bilişsel Hizmetler hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/try/cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Bilişsel Hizmetler Abonelikleri) sayfasını açarak Bilişsel Hizmetler hesabınızın bir aboneliğe bağlanmış olduğundan emin olun.

Azure portalında seçerek oluşturulmuş bir konuşma Hizmetleri abonelik bağlanabilirsiniz **mevcut aboneliğe bağlanma**.

Azure portalında bir konuşma Hizmetleri abonelik oluşturma hakkında daha fazla bilgi için bkz: [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md).

## <a name="prepare-the-data"></a>Verileri hazırlama

Bir akustik modeli belirli bir etki alanı için özelleştirmek üzere konuşma verilerinden oluşan bir koleksiyona ihtiyaç duyulur. Bu koleksiyon birkaç konuşmadan yüzlerce saatlik konuşmaya kadar değişebilir. Koleksiyon, konuşma verilerinden oluşan ses dosyası kümesinden ve her ses dosyasının transkripsiyonunu içeren bir metin dosyasından oluşur. Ses verilerinin tanıyıcıyı kullanmak istediğiniz senaryoyu temsil etmesi gerekir.

Örneğin:

* Gürültülü bir fabrika ortamındaki konuşmayı daha iyi bir şekilde tanımak istiyorsanız ses dosyaları gürültülü bir fabrika ortamında konuşan kişileri içermelidir.
* Örneğin tek bir kullanıcı için performansı iyileştirmek istiyorsanız FDR’nin Fireside Chats içeriğinin tamamını yazıya dökerek ses dosyalarının yalnızca bu konuşmacıya ait olan birçok örnek içermesini sağlamanız gerekir.

Akustik modelin özelleştirilmesi için akustik bir veri kümesi, iki bölümden oluşur: (1 konuşma verileri içeren ses dosyalarını ve (2) içeren tüm ses dosyaları, döküm dosyası kümesi.

### <a name="audio-data-recommendations"></a>Ses verisi önerileri

* Veri kümesindeki tüm ses dosyaları WAV (RIFF) biçiminde depolanmalıdır.
* Sesin örnekleme hızı 8 kilohertz (KHz) veya 16 KHz olmalı ve örnek değerler sıkıştırılmamış, darbe kod modülasyonu (PCM) 16 bit işaretli tamsayılar (kısa değerler) olarak depolanmalıdır.
* Yalnızca tek kanallı (mono) ses dosyaları desteklenir.
* Ses dosyalarının uzunluğu 100 mikro saniye ile 1 dakika arasında değişebilir ancak ideal süre 10-12 saniyedir. İdeal olarak her ses dosyası en az 100 mikro saniye sessizlikle başlayıp bitmelidir ve genelde bu süre 500 mikro saniye ile 1 saniye arasında tutulur.
* Verilerinizde arka plan gürültüsü varsa daha uzun sessizlik sürelerine sahip örnekler de kullanmanızı öneririz. Bu durumda verilerinizde konuşma içeriğinin öncesinde ve/veya sonrasında birkaç saniyelik sessizlik süresi olabilir.
* Her ses dosyasında tek bir konuşma, başka bir deyişle tek bir cümle, tek bir sorgu veya bir diyaloğun tek bir tarafını içermelidir.
* Veri kümesindeki her ses dosyası benzersiz bir dosya adına ve .wav uzantısına sahip olmalıdır.
* Ses dosyası kümesi alt dizin olmadan tek bir klasöre yerleştirilmeli ve ses dosyası kümesinin tamamı tek bir .zip dosyası halinde arşivlenmelidir.

> [!NOTE]
> Web portalı aracılığıyla gerçekleştirilen veri içeri aktarma işlemleri şu an için 2 GB ile sınırlı olduğundan bir akustik veri kümesi bu boyutu aşamaz. Bu boyut 16 KHz kaliteyle kaydedilmiş yaklaşık 17 saatlik sese veya 8 KHz kaliteyle kaydedilmiş yaklaşık 34 saatlik sese denk gelir. Ses verilerinin ana gereksinimleri aşağıdaki tabloda özetlenmiştir:
>

| Özellik | Değer |
|---------- |----------|
| Dosya Biçimi | RIFF (WAV) |
| Örnekleme Oranı | 8000 Hertz (Hz) veya 16.000 Hz |
| Kanallar | 1 (mono) |
| Örnek Biçimi | PCM, 16 bit tamsayılar |
| Dosya Süresi | 0,1 saniye < süre < 12 saniye |
| Sessizlik Payı | > 0,1 saniye |
| Arşiv Biçimi | .zip |
| Maksimum Arşiv Boyutu | 2 GB |

> [!NOTE]
> Dosya adlarında yalnızca Latin alfabesi ve 'dosyaadı.uzantı' biçimi kullanılmalıdır.

## <a name="language-support"></a>Dil desteği

Özel **Konuşmayı Metne Dönüştürme** dil modelleri için desteklenen dillerin tam listesi için bkz. [Konuşma Tanıma Hizmeti için desteklenen diller](language-support.md#speech-to-text).

### <a name="transcriptions-for-the-audio-dataset"></a>Ses veri kümesi için transkripsiyonlar

Tüm WAV dosyalarının transkripsiyonları tek bir düz metin dosyasına yerleştirilmelidir. Transkripsiyon dosyasının her satırında ses dosyalarından birinin adı ve transkripsiyon bulunmalıdır. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır.

  Örneğin:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Transkripsiyon UTF-8 bayt sırası işareti (BOM) ile kodlanmış olmalıdır.

Transkripsiyon metinleri sistem tarafından işlenebilmesi için normalleştirilir. Ancak veriler Özel Konuşma Tanıma Hizmeti'ne yüklenmeden _önce_ kullanıcı tarafından gerçekleştirilmesi gereken bazı önemli normalleştirme adımları vardır. Transkripsiyonlarınızı hazırlarken kullanmanız gereken uygun dil için bkz. [Konuşma Tanıma Hizmeti için transkripsiyon yönergeleri](prepare-transcription.md).

Sonraki bölümde kullanarak adımları [konuşma Hizmetleri portalı](https://cris.ai).

## <a name="import-the-acoustic-dataset"></a>Akustik veri kümesini içeri aktarma

Ses dosyalarını ve transkripsiyonları hazırladıktan sonra hizmetin web portalından içeri aktarabilirsiniz.

Bunları almak için önce için oturum açmadıysanız, sağlayın [konuşma Hizmetleri portalı](https://cris.ai). Ardından şeritteki **Custom Speech** (Özel Konuşma) açılan listesinden **Adaptation Data** (Uyarlama Verileri) öğesini seçin. Özel Konuşma Tanıma Hizmeti'ne ilk kez veri yüklüyorsanız **Datasets** (Veri Kümeleri) adlı boş bir tablo görüntülenir.

**Acoustic Datasets** (Akustik Veri Kümeleri) satırındaki **Import** (İçeri Aktar) düğmesini seçtiğinizde sitede yeni veri kümesi yükleme sayfası açılır.

![Akustik verileri içeri aktarma sayfası](media/stt/speech-acoustic-datasets-import.png)

**Name** (Ad) ve **Description** (Açıklama) kutularına uygun bilgileri girin. Yüklediğiniz farklı veri kümelerini takip etmek için kolay anlaşılır açıklamalar kullanmanız önerilir.

**Transcriptions file (.txt)** (Transkripsiyon dosyaları) ve **Audio files (.zip)** (Ses dosyaları) kutularında **Browse** (Gözat) öğesini seçip düz metin transkripsiyon dosyanızı ve WAV dosyalarının bulunduğu zip arşivini bulun. Hazırlık işlemlerini tamamladığınızda verilerinizi yüklemek için **Import** (İçeri Aktar) öğesini seçin. Verileriniz yüklenir. Büyük veri kümelerinin aktarılması birkaç dakika sürebilir.

Karşıya yükleme tamamlandıktan sonra **Acoustic Datasets** (Akustik Veri Kümeleri) tablosuna dönün. Akustik veri kümenizi gösteren yeni bir giriş görüntülenir. Bu kümeye benzersiz tanımlayıcı (GUID) atanmış olduğuna dikkat edin. Veriler geçerli durumu görüntüler: *NotStarted* işlenmek üzere sıraya alınıyor sırada *çalıştıran* doğrulama gerçekleştiriliyor sırada ve *tam* veri olduğunda kullanıma hazır.

Veri doğrulama aşamasında bir dizi kontrol gerçekleştirilerek ses dosyalarında dosya biçimi, uzunluğu ve örnekleme oranı doğrulama; transkripsiyon dosyalarında ise dosya biçimi doğrulama ve metin normalleştirme işlemleri gerçekleştirilir.

Durum *Succeeded* (Başarılı) olduğunda **Details** (Ayrıntılar) öğesini seçerek akustik veri doğrulama raporunu görebilirsiniz. Doğrulama adımında başarılı ve başarısız olan konuşma sayısının yanı sıra başarısız olan konuşmalar hakkında ayrıntılı bilgiler gösterilir. Aşağıdaki örnekte iki WAV dosyasının ses biçimi hatalı olduğu için doğrulama işlemi başarısız olmuştur. Bu veri kümesinde dosyalardan biri hatalı örnekleme hızına, diğeri ise hatalı dosya biçimine sahiptir.

![Veri uyarlama ayrıntı sayfası](media/stt/speech-acoustic-datasets-report.png)

Veri kümesinin adını veya açıklamasını değiştirmek isterseniz **Edit** (Düzenle) bağlantısına tıklayabilirsiniz. Transkripsiyonları veya ses dosyalarını değiştiremezsiniz.

## <a name="create-a-custom-acoustic-model"></a>Özel akustik model oluşturma

Akustik veri kümenizin durumu *Complete* (Tamamlandı) olduğunda özel akustik model oluşturmak için kullanabilirsiniz. Bunu yapmak için **Custom Speech** (Özel Konuşma) açılan menüsündeki **Acoustic Models** (Akustik Modeller) öğesini seçin. **Your models** (Modelleriniz) adlı listede özel akustik modellerinizin tamamı listelenir. İlk kullanımda bu tablo boş olacaktır. Tablo başlığında geçerli yerel ayar gösterilir. Şu an için yalnızca İngilizce (ABD) dilinde akustik model oluşturabilirsiniz.

Yeni bir model oluşturmak için tablo başlığının altındaki **Create New** (Yeni Oluştur) öğesini seçin. Yukarıda yaptığınız gibi modeli tanımlamanıza yardımcı olması için bir ad ve açıklama girin. Örneğin **Description** (Açıklama) alanını kullanarak modeli oluşturmak için kullandığınız başlangıç modelini ve akustik veri kümesini kaydedebilirsiniz.

Ardından **Base Acoustic Model** (Temel Akustik Model) açılan listesinden bir temel model seçin. Temel model, özelleştirme işlemlerinizin başlangıç noktasıdır. İki temel akustik modelden birini seçebilirsiniz:
* **Microsoft Search and Dictation AM** modeli; komutlar, arama sorguları veya dikte gibi bir uygulamaya yönlendirilen konuşmalar için uygundur.
* **Microsoft Conversational Model**, günlük konuşma tarzındaki konuşmaları tanımak için uygundur. Bu konuşma türü genelde başka bir kişiye hitaben yapılır ve çağrı merkezinde veya toplantılarda kullanılır.

Kısmi sonuçlar için gecikme süresi Conversational modellerde, Search ve Dictation modellerine kıyasla daha yüksektir.

> [!NOTE]
> Bu senaryoların tümünü hedefleyen yeni bir **Universal** modeli kullanıma sunuyoruz. Yukarıdaki modeller de genel kullanıma açık kalacaktır.

Daha sonra **Acoustic Data** (Akustik Verileri) açılan listesinde özelleştirme gerçekleştirmek istediğiniz akustik verilerini seçin.

![Akustik model oluşturma sayfası](media/stt/speech-acoustic-models-create2.png)

İşleme tamamlandıktan sonra isteğe bağlı olarak yeni modelinizin doğruluk testini gerçekleştirebilirsiniz. Bu test belirtilen akustik veri kümesinde özelleştirilmiş akustik modeli kullanarak konuşmayı metne dönüştürme değerlendirmesi gerçekleştirecek ve sonuçları bildirecektir. Bu testi gerçekleştirmek için **Accuracy Testing** (Doğruluk Testi) onay kutusunu seçin. Ardından açılan listeden bir dil modeli seçin. Özel dil modeli oluşturmadıysanız açılan listede yalnızca temel dil modelleri görüntülenir. En uygun dil modeli seçmek için bkz: [Öğreticisi: Özel dil modeli oluşturma](how-to-customize-language-model.md).

Son olarak özel modeli değerlendirmek için kullanmak istediğiniz akustik veri kümesini seçin. Doğruluk testini gerçekleştiriyorsanız modelin performansı hakkında gerçekçi bir izlenim almak için model oluşturma sırasında kullandığınız kümeden farklı bir akustik veri kümesi seçmeniz önemlidir. Eğitim verilerinin doğruluğunun test edilmesi, uyarlanmış modelin gerçek koşullarda göstereceği performansı değerlendirmenizi sağlamaz. Sonuç fazla iyimser olacaktır. Doğruluk testinin 1000 konuşmayla sınırlı olduğunu da unutmayın. Akustik veri kümesi daha büyükse yalnızca ilk 1000 konuşma değerlendirilir.

Özelleştirme işlemini çalıştırmaya hazır olduğunuzda **Create** (Oluştur) öğesini seçin.

Akustik model tablosunda bu yeni modele karşılık gelen yeni bir giriş görüntülenir. Tabloda ayrıca işlemin durumunu görüntüler: *Bekleyen*, *işleme*, veya *tam*.

![Akustik modeller sayfası](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Tanıma Hizmetleri deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# dilinde konuşma tanıma](quickstart-csharp-dotnet-windows.md)
- [Git Örnek Verileri](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
