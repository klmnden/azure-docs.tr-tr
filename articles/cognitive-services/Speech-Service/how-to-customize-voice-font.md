---
title: Özel sesli nedir? -Azure Bilişsel hizmetler | Microsoft Docs
description: Tanınabilir, tür, bir marka sesli oluşturmanıza olanak tanıyan Bu makale genel bakışlar Microsoft metin okuma ses özelleştirme.
services: cognitive-services
author: noellelacharite
manager: nolach
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: ad5af799fd46dc51b85432999f986de8cdb056ec
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355096"
---
# <a name="creating-custom-voice-fonts"></a>Özel sesli yazı tipleri oluşturma

Microsoft metin okuma (TTS) sesli özelleştirme tanınabilir, tür, tek bir ses, marka için oluşturmanıza olanak sağlar: bir *sesli yazı tipi.* 

Sesli yazı oluşturmak için studio kaydını yapabilir ve ilişkili betikler eğitim verileri olarak yükleyin. Hizmet, daha sonra kaydınızı olarak ayarlanmış bir benzersiz sesli modeli oluşturur. Ardından bu ses yazı tipini konuşma sentezlemek için de kullanabilirsiniz. 

Kavram kanıtı için verileri küçük miktarda başlayabiliriz. Ancak daha fazla veri sağlarsanız, daha doğal ve professional sesinizi ses.

Sesli özelleştirme ABD İngilizcesi (en-US) ve Anakara Çince (zh-CN) için kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Metin okuma ses özelleştirme özelliği şu anda özel önizlemede değil. [Uygulama formu doldurun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) erişim için kabul edilmesi için.

Şunları da yapmanız gerekir:

* Bir Azure hesabı ([ücretsiz kaydolun](https://azure.microsoft.com/free/ai/) henüz yoksa).

* Konuşma hizmet aboneliği. [Bir tane oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) yapmadıysanız.

    ![Panel oluşturma](media/custom-voice/create-panel.png)

Aboneliğinizi oluşturduktan sonra hızlı başlangıç panelinde ya da genel bakış panelinde Yeni Abonelik iki adet abonelik anahtarı bulabilirsiniz. Her iki anahtarı kullanabilir.

Son olarak, aboneliğiniz gibi özel sesli Portalı'na bağlanın.

1. Oturum [özel sesli portal](https://customvoice.ai) uygulamak için kullandığınız erişim için aynı Microsoft hesabı kullanma.

2. Hesap adı altında 'Subscriptions' sağ üstte gidin.

    ![Abonelikler](media/custom-voice/subscriptions.png)

3. 'Abonelik' sayfasında 'Bağlan varolan abonelik' seçin.

     ![Varolan. abonelik'e bağlanma](media/custom-voice/connect-existing-sub.png)

4. Abonelik anahtarınızı aşağıda gösterildiği gibi tabloya, yapıştırın.

     ![Abonelik Ekle](media/custom-voice/add-subscription.png)

Başlamaya hazırsınız!

## <a name="prepare-recordings-and-transcripts"></a>Kayıtları ve dökümleri hazırlama

Ses Eğitim veri kümesi ses dosyaları, tüm bu ses dosyalarını dökümleri içeren bir metin dosyası ile birlikte bir dizi oluşur.

Bu dosyaları her iki yönde hazırlayabilirsiniz: ya da bir komut dosyası yazma ve ses yeteneğiniz tarafından okuma veya genel kullanıma ses kullanın ve bunları metne transcribe sahip. İkinci durumda, "um" ve diğer dolgu ses atlar, mumbled sözcükleri veya mispronunciations gibi ses dosyalarını disfluencies düzenleyin.

İyi sesli yazı tipi üretmek için kayıtları ile yüksek kaliteli bir mikrofon sessiz bir odada yapılır önemlidir. Tutarlı birim oranı, aralık ve açıklayıcı veren davranışların konuşma konuşarak harika bir dijital ses oluşturmak için gerekli. Ses üretim kullanımı için oluşturmak için profesyonel kaydı studio ve ses yeteneğiniz kullanmanızı öneririz.

### <a name="audio-files"></a>Ses dosyaları

Her ses dosyası (örneğin, tek bir cümle veya tek bir Aç iletişim sisteminin) tek bir utterance içermelidir. Tüm dosyalar (çok dilli özel sesler desteklenmez) aynı dilde olmalıdır. Ses dosyalarını ayrıca her dosya adı uzantısı ile yapılan benzersiz bir sayısal filename olmalıdır `.wav`.

Ses dosyaları gibi hazırlıklı olmalıdır. Diğer biçimlere desteklenmiyor ve reddedilir.

| **Özellik** | **Değer** |
| ------------ | --------- |
| Dosya biçimi  | RIFF (WAV)|
| Örnekleme hızı| 16.000 Hz |
| Kanallar     | 1 (monophonic)  |
| Örnek Biçim| PCM, 16 bit |
| Dosya Adı    | Sayısal ile `.wav` uzantısı |
| Arşiv biçimi| zip      |
| En fazla arşiv boyutu|200 MB|

Ses dosyaları kümesini alt dizinleri olmadan tek bir klasöre yerleştirin ve tek bir ZIP dosyası arşivi olarak ayarlanmış tüm paket.

> [!NOTE]
> Portal şu anda ZIP alır en fazla 200 MB arşivler. Ancak, birden çok arşivini karşıya olabilir. Veri kümeleri izin verilen en fazla 10 ZIP ücretsiz aboneliği kullanıcılarını ve standart abonelik kullanıcılar için 50 dosyaları ' dir.

### <a name="transcripts"></a>Dökümleri

Transcription, düz bir Unicode metin dosyası (UTF-16 little endian) dosyasıdır. Her satırın transcription dosyasının bir sekme (kod noktası 9) karakteri, kendi dökümü son ardından ve bir ses dosyasının adı olması gerekir. Boş satır olmamasına izin verilir.

Örneğin:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

Özel sesli sistem dökümleri metni için küçük dönüştürme ve yabancı noktalama kaldırma normalleştirir. Dökümleri % 100 karşılık gelen ses kayıtlarını doğru olduğunu önemlidir.

> [!TIP]
> Ne zaman üretim okuma oluşturma, ses kapsamı ve verimlilik düşünüldüğünde select utterances (veya yazma komut dosyaları) sesleri.

## <a name="upload-your-datasets"></a>Veri kümeleriniz karşıya yükle

Ses dosyası arşive ve dökümleri hazırladıktan sonra bunları aracılığıyla karşıya [özel sesli hizmet portalı](https://customvoice.ai).

> [!NOTE]
> Karşıya yüklediğiniz sonra veri kümeleri düzenlenemez. Ses dosyalarını bazıları dökümleri dahil unuttuysanız örneğin veya kazayla yanlış cinsiyetiniz seçin tüm veri kümesinin yeniden yüklemeniz gerekir. Veri kümesi ve ayarları tamamen karşıya yükleme başlamadan önce denetleyin.

1. Portalda oturum açın.

2. Seçin **veri** özel sesli ana sayfada altında. 

    ![My projeleri](media/custom-voice/my-projects.png)

    My sesli veri tablosu görüntülenir. Henüz hiçbir voice veri kümeleri karşıya yüklediğiniz değil, boş olur.

3. Tıklatın **veri içeri aktarma** yeni bir veri kümesi yükleme sayfasını açın.

    ![Ses Veri Al](media/custom-voice/import-voice-data.png)

4. Sağlanan alanları bir ad ve açıklama girin. 

5. Sesli yazı tipleri için yerel ayarları seçin. Yerel ayar bilgileri kayıt veriler ve komut dosyaları dilinin eşleştiğinden emin olun. 

6. Ses, kullanmakta olduğunuz Konuşmacı cinsiyetiniz seçin.

7. Komut dosyası ve karşıya yüklemek için ses dosyalarını seçin. 

8. Tıklatın **alma** verilerinizi karşıya yüklemek için. Büyük veri kümeleri için içeri aktarma birkaç dakika sürebilir.

> [!NOTE]
> Ücretsiz abonelik kullanıcılar aynı anda iki veri kümesi yükleyebilirsiniz. Standart abonelik kullanıcılar, beş veri kümeleri aynı anda karşıya yükleyebilir. Sınıra ulaştıysanız, içe aktarma veri kümeleriniz en az biri beklemeniz tamamlandıktan ve sonra tekrar deneyin.

Karşıya yükleme tamamlandığında, My sesli veri tablosu yeniden görüntülenir. Henüz karşıya kümenizi karşılık gelen bir giriş görmeniz gerekir. 

Veri kümeleri, karşıya yükleme otomatik olarak doğrulanır. Veri doğrulama bir dizi denetim kendi dosya biçimi, boyutu ve örnekleme hızını doğrulamak için ses dosyaları içerir. Denetimleri transcription dosyalar üzerinde dosya biçimini doğrulayın ve bazı metin normalleştirmesi. Konuşma tanıma kullanarak utterances transcribed ve sonuçta elde edilen metin sağladığınız dökümü ile karşılaştırılır.

![Sesli verilerimi](media/custom-voice/my-voice-data.png)

Aşağıdaki tabloda işleme için alınan veri kümelerini gösterir. 

| Durum | Anlamı
| ----- | -------
| `NotStarted` | Veri kümenizi alındı ve işlenmek üzere sıraya
| `Running` | Veri kümenizi doğrulandı
| `Succeeded` | Veri kümenizi doğrulandı ve şimdi bir sesli yazı tipi oluşturmak için kullanılabilir

Doğrulama tamamlandıktan sonra her veri kümeleriniz Utterance sütununda eşleşen utterances toplam sayısını görebilirsiniz.

Söyleniş puanlarını ve gürültü düzeyi her kayıtlarınızı denetlemek için bir rapor indirebilirsiniz. Söyleniş puan aralığından 0 ile 100; bir puan 70 aşağıda normalde bir konuşma hata veya komut dosyası uyumsuzluğu gösterir. Yoğun Vurgu telaffuz puan azaltmak ve oluşturulan dijital ses etkileyebilir.

Daha yüksek bir sinyal gürültü oranı (SNR), ses, daha düşük gürültü gösterir. Profesyonel stüdyoları aracılığıyla kaydı tarafından 50 + SNR genellikle ulaşabilirsiniz. Ses 20 aşağıda bir SNR ile oluşturulan sesinizi içinde belirgin gürültü neden olabilir.

Düşük telaffuz puanları veya zayıf sinyal gürültü oranları herhangi utterances yeniden kaydetmeyi deneyin. Yeniden kayıt mümkün değilse, bu utterances, veri kümesinden hariç.

## <a name="build-your-voice-font"></a>Sesli yazı derleme

Veri kümenizi doğrulandıktan sonra özel sesli yazı tipi oluşturmak için kullanabilirsiniz. 

1. Seçin **modelleri** "Özel sesli" açılır menüde. 
 
    Önceden oluşturduğunuz tüm özel sesli yazı tiplerini listeleme My sesli yazı tipleri tablo görünür.

1. Tıklatın **oluşturma sesleri** tablo başlığı altında. 

    Bir sesli yazı tipi oluşturmak için sayfası görüntülenir. Geçerli yerel tablonun ilk satırını gösterilir. Başka bir dilde bir sesli oluşturmak için yerel ayarları değiştirin. Yerel ayar sesli oluşturmak için kullanılan veri kümeleri ile aynı olması gerekir.

1. Veri kümenizi karşıya gibi bir ad ve bu model tanımlamanıza yardımcı olması için açıklama girin. 

    Buraya girdiğiniz ad, sesli konuşma Birleştirici için isteğiniz SSML Giriş bir parçası olarak belirtin, böylece seçmeye için kullandığınız adı olacaktır. Yalnızca harf, rakam ve gibi birkaç noktalama karakterleri '-', '_' '(',')' izin verilir.

    Bir ortak açıklama alanı model oluşturmak için kullanılan veri kümeleri adlarını kaydetmek için kullanılır.

1. Sesli yazı cinsiyetiniz seçin. Bu veri kümesi cinsiyetiniz eşleşmelidir.

1. Sesli yazı tipi eğitim için kullanmak istediğiniz veri kümeleri seçin. Kullanılan tüm veri kümeleri aynı Konuşmacı olması gerekir.

1. Tıklatın **oluşturma** sesli yazı oluşturmaya başlamak için.

    ![Model oluşturma](media/custom-voice/create-model.png)

Yeni modelinizi My sesli yazı tipleri tablosunda görünür. 

![My sesli yazı tipleri](media/custom-voice/my-voice-fonts.png)

Gösterilen durumu aşağıda gösterildiği gibi bir sesli yazı tipi için Veri kümenizi dönüştürme işlemi yansıtır.

| Durum | Anlamı
| ----- | -------
| `NotStarted` | Sesli yazı tipi oluşturma için isteğiniz işlenmek üzere sıraya
| `Running` | Sizin sesli yazı tipi oluşturdu
| `Succeeded` | Sesli yazı oluşturulup oluşturulmadığını ve dağıtılabilir

Zaman eğitim işlenen ses veri hacmi bağlı olarak değişir. Tipik kez gelen hakkında utterances yüzlerce 30 dakika 20.000 utterances için 40 saat aralığı.

> [!NOTE]
> Ücretsiz abonelik kullanıcılar aynı anda iki sesli yazı tipi eğitim yapabilirsiniz. Standart abonelik kullanıcılar aynı anda üç sesleri eğitim yapabilirsiniz. Sınıra ulaştıysanız, en az bir sesli yazı tipleri eğitim tamamlanana kadar bekleyin ve yeniden deneyin.

## <a name="test-your-voice-font"></a>Sesli yazı test

Sesli yazı başarıyla oluşturulduktan sonra kullanım için dağıtmadan önce sınayabilirsiniz. Tıklatın **Test** işlemleri sütun. Test sayfası seçilen ses yazı tipini görüntülenir. Tablonun tüm test istekleri sesi için gönderdiğiniz henüz yapmadıysanız, boş.

![2. Bölüm My sesli yazı tipleri](media/custom-voice/my-voice-fonts2.png)

Tıklatın **Test metinle** düğmesi metni istekleri göndermek için bir açılır menü görüntülemek için tablo başlık altında. Düz metin veya SSML test isteğinizi gönderebilirsiniz. En büyük giriş boyutu, SSML istek için tüm etiketleri de dahil olmak üzere, 1.024 karakterdir. Metin dili sesli yazı dili ile aynı olmalıdır.

![Yazı tipi sınama sesli](media/custom-voice/voice-font-testing.png)

Tıklayın metin kutusuna doldurma ve giriş modunu onaylayan sonra **Evet** test isteğinizi gönderin ve test sayfasına dönmek için. Tablo artık yeni isteğiniz ve şimdi bilinen Durum sütununda karşılık gelen bir giriş içerir. Konuşma sentezlemek için birkaç dakika sürebilir. Durum sütununda başarılı okuduğunda metin girişi yükleyebilirsiniz (bir `.txt` dosyası) ve ses çıkış (bir `.wav` dosyası) ve ikincisi kalitesi için audition.

![Sesli yazı tipi test, bölüm 2](media/custom-voice/voice-font-testing2.png)

## <a name="create-and-use-a-custom-endpoint"></a>Oluşturma ve özel bir uç noktası kullan

Başarıyla oluşturuldu ve ses modelinizi test sonra bir özel metin okuma uç dağıtın. Metin okuma isteklerinin REST API'si aracılığıyla yaparken sonra her zamanki uç noktası yerine bu uç nokta kullanın. Özel uç noktanızı yalnızca yazı tipini dağıtmak için kullanılan abonelik tarafından çağrılabilir.

Yeni bir özel uç noktası oluşturmak için seçtiğiniz **uç noktaları** sayfanın üst kısmındaki özel sesli menüsünden. Dağıtım sayfası, geçerli özel sesli uç noktalar, kendi tablosunun varsa görünür.

Tıklatın **dağıtmak sesleri** yeni bir uç noktası oluşturmak için düğmesi. Uç noktası oluşturma"sayfasında, geçerli yerel tablonun ilk satırını yansıtılır. Farklı bir dil için bir dağıtım oluşturmak için görüntülenen yerel ayarını değiştirin. (Bu, dağıttığınız sesli eşleşmelidir.) Özel uç noktanızı açıklaması ve adını girin.

Abonelik menüsünde kullanmak istediğiniz aboneliği seçin. Ücretsiz abonelik kullanıcıların dağıtılan aynı anda yalnızca bir model olabilir. Standart abonelik kullanıcılar 20 uç noktalar, her biri kendi özel sesli kadar oluşturabilirsiniz.

![Uç noktası oluşturma](media/custom-voice/create-endpoint.png)

Dağıtılacak model seçtikten sonra **oluşturma**. Dağıtım sayfası, artık yeni uç noktanız için bir girişi görüntülenir. Yeni bir uç noktası örneği oluşturmak için birkaç dakika sürebilir. Dağıtım durumunu başarılı olduğunda, uç noktayı kullanıma hazırdır.

![My dağıtılan sesler](media/custom-voice/my-deployed-voices.png)

Dağıtım durumu başarılı olduğunda, dağıtılan sesli yazı uç noktası görünür My dağıtılan sesleri tablo. Bir HTTP isteği doğrudan bu URI kullanabilirsiniz.

Çevrimiçi uç noktası sınama ayrıca özel sesli portalı yoluyla kullanılabilir. Uç noktanız test edilmesini seçerseniz **sınama uç noktaları** özel sesli açılan menüsünden. Sayfa sınama uç noktası görüntülenir. Dağıttığınız bir sesli seçin ve (düz metin veya SSML biçimi içinde) söylenir metni metin kutusuna girin.

> [!NOTE] 
> SSML, kullanırken `<voice>` etiketi verdiğiniz özel sesinizi oluşturduğunuz sırada adı belirtmeniz gerekir.

Tıklatın **Yürüt** özel sesli yazı tipiyle konuşulan metin duymak.

![Uç nokta test etme](media/custom-voice/endpoint-testing.png)

Özel uç noktaya okuma istekleri için kullanılan standart endpoint işlevsel olarak aynıdır. Bkz: [REST API](rest-apis.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
- [C# Konuşma tanıması](quickstart-csharp-windows.md)
