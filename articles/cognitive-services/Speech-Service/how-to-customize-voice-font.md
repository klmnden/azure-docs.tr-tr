---
title: Özel ses nedir? -Azure Bilişsel hizmetler
description: Tanınabilir, tür, bir marka ses oluşturmanıza olanak sağlayan bu makalede genel bakışlar Microsoft metin okuma ses özelleştirme.
services: cognitive-services
author: noellelacharite
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: 84493ae83515c0458bf5b9e9cf44603300a8b4f7
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284896"
---
# <a name="creating-custom-voice-fonts"></a>Özel ses tipi olarak oluşturma

Microsoft metin okuma (TTS) ses özelleştirme markanız için tanınan, tür, tek bir ses oluşturmanıza olanak sağlar: bir *ses tipi.* 

Ses tipi oluşturmak için studio kaydını yapabilir ve ilişkili betikler eğitim verileri olarak karşıya yükleyin. Hizmet, ardından kaydınız için ayarlanmış bir benzersiz ses modeli oluşturur. Ardından bu ses tipi konuşma sentezlemek için de kullanabilirsiniz. 

Az miktarda bir kavram kanıtı için verileri ile başlayabilirsiniz. Ancak daha fazla veri sağlarsanız, daha doğal ve professional, ses çalar.

Ses özelleştirme ABD İngilizce (en-US) ve ana kara Çince (zh-CN) için kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Metin okuma ses özelleştirme özelliği şu anda özel Önizleme aşamasındadır. [Uygulama formu doldurun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) erişim için kabul edilmesi için.

Ayrıca bir Azure hesabı ve konuşma hizmeti için bir abonelik gerekir. [Bir oluşturma](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) henüz yapmadıysanız. Aboneliğinizi şu şekilde özel sesli Portalı'na bağlanın.

1. Oturum [özel sesli portalı](https://customvoice.ai) uygulamak için kullandığınız erişim için aynı Microsoft hesabını kullanarak.

2. Sağ üst kısımdaki 'Subscriptions' hesap adınızın altında gidin.

    ![Abonelikler](media/custom-voice/subscriptions.png)

3. 'Subscriptions' sayfasında 'Connect mevcut aboneliği' seçin.

     ![Mevcut abonelik'e bağlanma](media/custom-voice/connect-existing-sub.png)

4. Abonelik anahtarınız, aşağıda gösterildiği gibi tablosu içine yapıştırın. Her aboneliğin iki anahtarı vardır ve bunlardan birini kullanabilir.

     ![Abonelik Ekle](media/custom-voice/add-subscription.png)

Başlamaya hazırsınız!

## <a name="prepare-recordings-and-transcripts"></a>Kayıtları ve dökümler hazırlama

Ses dosyaları, tüm bu ses dosyalarını dökümleri içeren bir metin dosyası ile birlikte bir dizi Ses Eğitim veri kümesi oluşur.

Bu dosyalar herhangi bir yönde hazırlayabilirsiniz: ya da bir betik yazabilir ve tarafından sesli talent, okuma veya herkese ses kullanın ve metin özelliği vardır. İkinci durumda, "um" ve diğer filler sesleri, atlar, mumbled sözcükleri veya mispronunciations gibi ses dosyalarını disfluencies düzenleyin.

İyi ses tipi üretmek için kayıtları ile yüksek kaliteli bir mikrofon sessiz bir odada yapılır önemlidir. Tutarlı birim oranı, aralık ve konuşma ifadesel veren davranışların gibi konuşma harika bir dijital ses oluşturmak için gerekli. Üretim kullanımı için bir ses oluşturmak için profesyonel kaydı studio ve ses beceri kullanmanızı öneririz. Daha fazla ayrıntı için [ses kaydetmek nasıl özel bir ses için örnekleri](record-custom-voice-samples.md).

### <a name="audio-files"></a>Ses dosyaları

Her ses dosyası, tek bir utterance (örneğin, tek bir cümle veya bir iletişim sistemi olan tek bir dönüş) içermelidir. Tüm dosyalar (çok dilli özel seslerle desteklenmez) aynı dilde olmalıdır. Ses dosyalarını ayrıca her dosya adı uzantısı ile yapılan benzersiz bir sayısal dosya adı olmalıdır `.wav`.

Ses dosyaları gibi hazırlıklı olmalıdır. Diğer biçimlere desteklenmez ve reddedilir.

| **Özellik** | **Değer** |
| ------------ | --------- |
| Dosya Biçimi  | RIFF (WAV)|
| Örnekleme Oranı| en az 16. 000 Hz |
| Örnek Biçimi| PCM, 16-bit |
| Dosya Adı    | Sayısal ile `.wav` uzantısı |
| Arşiv Biçimi| Zip      |
| Maksimum Arşiv Boyutu|200 MB|

Ses dosyaları kümesini alt dizinler olmadan tek bir klasöre yerleştirin ve tek bir ZIP dosyası arşivi ayarlamak tüm paket.

> [!NOTE]
> 16.000 Hz reddedilir daha düşük bir örnekleme hızı dosyalarıyla Wave. Burada bir zip dosyası farklı bir örnekleme hızı Dalgalar içeren durumlarda, yalnızca eşit veya daha yüksek 16.000 Hz aktarılır.
> Portal şu anda ZIP alır. en fazla 200 MB arşivler. Ancak, birden fazla arşivini yüklenebilir. En fazla izin verilen veri kümeleri aboneliği kullanıcılarını ve 50 Standart abonelik kullanıcılar için ücretsiz 10 ZIP dosyaları sayısıdır.

### <a name="transcripts"></a>Dökümleri

Düz metin dosyası (ANSI/UTF-8/UTF-8-BOM/UTF-16-LE/UTF-16-BE) döküm dosyasıdır. Döküm dosyasının her satır bir sekme (kod noktası 9) karakteri oynatırken son ardından ve bir ses dosyasının adı olmalıdır. Boş satır izin verilir.

Örneğin:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

Özel ses sistem dökümleri için küçük metin dönüştürme ve gereksiz noktalama işaretlerini kaldırarak normalleştirir. Dökümler %100 ilgili ses kayıtları için doğru olduğunu önemlidir.

> [!TIP]
> Ne zaman üretim metin okuma oluşturma, fonetik kapsamı hem verimlilik dikkate konuşma seçin (veya yazma betikler) sesler.

## <a name="upload-your-datasets"></a>Veri kümelerini karşıya yükleme

Ses dosyası Arşiv ve dökümler hazırlandıktan sonra bunları aracılığıyla karşıya [özel sesli hizmet portalı](https://customvoice.ai).

> [!NOTE]
> Veri kümeleri, karşıya yüklediğiniz sonra düzenlenemez. Ses dosyalarını bazıları dökümleri içeren unuttuysanız örneğin veya kazayla yanlış cinsiyet seçin veri kümesinin tamamında yeniden yüklemeniz gerekir. Veri kümesi ve ayarları ayrıntılı bir şekilde karşıya yükleme başlamadan önce denetleyin.

1. Portalda oturum açın.

2. Seçin **veri** özel sesli ana sayfada altında. 

    ![Projelerim](media/custom-voice/my-projects.png)

    Ses verilerim tablo görünür. Henüz hiçbir sesli veri kümelerini karşıya yüklediğiniz değil, boş olur.

3. Tıklayın **verileri içeri aktarma** yeni bir veri kümesi karşıya yükleme sayfasını açın.

    ![Sesli verileri içeri aktar](media/custom-voice/import-voice-data.png)

4. Belirtilen alanları bir ad ve açıklama girin. 

5. Ses tipi olarak sizin için bir yerel ayar seçin. Yerel ayar bilgileri kayıt verilerini ve betikleri dilinin eşleştiğinden emin olun. 

6. Cinsiyet konuşmacının ses kullanmakta olduğunuz seçin.

7. Betik ve ses dosyalarını karşıya yüklemek için seçin. 

8. Tıklayın **alma** verilerinizi karşıya yükleme. Büyük veri kümeleri için içeri aktarma, birkaç dakika sürebilir.

> [!NOTE]
> Ücretsiz aboneliği kullanıcıları, aynı anda iki veri kümesi yükleyebilirsiniz. Standart abonelik kullanıcılar aynı anda beş veri kümelerini karşıya yükleyebilirsiniz. Sınıra ulaştıysanız, en az bir veri kümeleriniz bekleyin içeri aktarma tamamlandıktan, sonra yeniden deneyin.

Karşıya yükleme tamamlandığında, My sesli veri tablosunu yeniden görüntülenir. Just-karşıya veri karşılık gelen bir giriş görmeniz gerekir. 

Veri kümelerini karşıya yükledikten sonra otomatik olarak doğrulanır. Veri doğrulama, bir dizi ses dosyaları, dosya biçimi, boyutu ve örnekleme hızı doğrulamak için denetim içerir. Döküm dosyaları üzerinde denetimleri dosya biçimini doğrulayın ve bazı metin normalleştirme gerçekleştirin. Konuşma tanıma ile Konuşma transcribed ve elde edilen metnini sağladığınız döküm ile karşılaştırılır.

![Ses verilerimi](media/custom-voice/my-voice-data.png)

Aşağıdaki tablo, içeri aktarılan veri kümeleri için işleme durumları gösterir. 

| Durum | Anlamı
| ----- | -------
| `NotStarted` | Veri kümeniz alındı ve işlenmek üzere sıraya alındı
| `Running` | Veri kümeniz doğrulandı
| `Succeeded` | Veri kümeniz doğrulandı ve şimdi bir ses tipi oluşturmak için kullanılabilir

Doğrulama tamamlandıktan sonra her veri kümeleriniz Utterance sütundaki eşleşen konuşma toplam sayısını görebilirsiniz.

Söyleniş puanları ve gürültü düzeyi her kayıtlarınızın denetlemek için bir rapor indirebilirsiniz. Söyleniş puanı aralık 0-100; 70'in altında bir puan genellikle bir konuşma hata veya betik uyuşmazlığı gösterir. Ağır bir Vurgu, Söyleniş puanınız azaltmak ve oluşturulan dijital ses etkiler.

Daha yüksek bir sinyal/gürültü oranına (SNR) daha düşük paraziti ses de gösterir. Genellikle, profesyonel studios aracılığıyla kayıt tarafından 50'den fazla SNR ulaşabilirsiniz. 20 aşağıda bir SNR sesle belirgin paraziti oluşturulan sesinizi de neden olabilir.

Herhangi bir konuşma düşük telaffuz puanları veya zayıf sinyal/gürültü oranları yeniden kaydetmeyi deneyin. Yeniden kayıt mümkün değilse, bu konuşma, veri kümesinden dışlamak.

## <a name="build-your-voice-font"></a>Derleme, ses tipi

Veri kümeniz doğrulandıktan sonra özel ses tipi oluşturmak için kullanabilirsiniz. 

1. Seçin **modelleri** "Özel ses" açılan menüsünde. 
 
    My ses tiplerini tablo, önceden oluşturduğunuz herhangi bir özel ses tipi listesi görüntülenir.

1. Tıklayın **oluşturma sesleri** tablo başlığı altında. 

    Bir ses tipi oluşturma sayfası görüntülenir. Geçerli yerel ayarı tablosunun ilk satırında gösterilir. Başka bir dilde bir ses oluşturmak için yerel ayarını değiştirin. Yerel ayar ses oluşturmak için kullanılan veri kümeleri ile aynı olmalıdır.

1. Veri kümeniz yüklediğiniz zaman yaptığınız gibi bir ad ve bu modeli tanımlamanıza yardımcı olması için bir açıklama girin. 

    Buraya girdiğiniz ad seçerken dikkatli şekilde SSML'yi girişinin parçası olarak isteğinizdeki konuşma sentezi için ses belirtmek için kullandığınız ad olacaktır. Yalnızca harf, rakam ve gibi bazı noktalama karakterleri '-', '_' '(',')' izin verilir.

    Açıklama alanı yaygın bir kullanımı, modeli oluşturmak için kullanılan veri kümelerinin adlarındaki harflerde kaydetmektir.

1. Kendi ses tipi dinleyicilerinin seçin. Bu veri kümesinin cinsiyet eşleşmelidir.

1. Ses tipi eğitim için kullanmak istediğiniz veri kümelerindeki seçin. Kullanılan tüm veri kümeleri aynı Konuşmacı olmalıdır.

1. Tıklayın **Oluştur** , ses tipi oluşturmaya başlayın.

    ![Model oluşturma](media/custom-voice/create-model.png)

Yeni modelinizi My ses tiplerini tabloda görüntülenir. 

![My ses tipleri](media/custom-voice/my-voice-fonts.png)

Gösterilen durumun bir ses tipi için veri dönüştürme işlemi burada gösterildiği gibi yansıtır.

| Durum | Anlamı
| ----- | -------
| `NotStarted` | İsteğiniz ses yazı oluşturma için işlenmek üzere sıraya alındı
| `Running` | Kendi ses tipi oluşturuluyor
| `Succeeded` | Kendi ses tipi oluşturuldu ve dağıtılabilir

Zaman eğitim işlenen ses veri hacmine bağlı olarak değişir. Tipik bir kez gelen hakkında konuşma yüzlerce 30 dakika ila 20.000 konuşma 40 saat aralığı.

> [!NOTE]
> Ücretsiz aboneliği kullanıcıları, aynı anda bir ses tipi eğitebilirsiniz. Standart abonelik kullanıcılar aynı anda üç ses eğitebilirsiniz. Sınıra ulaştıysanız, en az bir ses yazı tipleri eğitim tamamlanana kadar bekleyin ve yeniden deneyin.

## <a name="test-your-voice-font"></a>Test, ses tipi

Kendi ses tipi başarıyla oluşturulduktan sonra kullanım için dağıtmadan önce test edebilirsiniz. Tıklayın **Test** Operations sütunda. Test sayfası için seçilen ses tipi görüntülenir. Ses için herhangi bir testi istekleri henüz göndermediniz, tablo boşsa.

![2. Bölüm My ses tipleri](media/custom-voice/my-voice-fonts2.png)

Tıklayın **metinle Test** düğmesi metni istekleri göndermek için açılan menüyü görüntülemek için tablo başlık altında. Düz metin veya SSML'yi test isteğinizi gönderebilirsiniz. En büyük giriş boyutuna, SSML'yi istek için tüm etiketleri dahil olmak üzere, 1024 karakterdir. Dilin metin, ses tipi dili ile aynı olması gerekir.

![Yazı tipi test ses](media/custom-voice/voice-font-testing.png)

Metin kutusunu doldurarak ve giriş modu onayladıktan sonra **Evet** test isteğinizi göndermek ve test sayfasına geri dönün. Tablo, artık yeni isteğinizi ve şimdi bilinen Durum sütununda karşılık gelen bir giriş içerir. Bu konuşma sentezlemek için birkaç dakika sürebilir. Durum sütununda başarılı okuduğunda, metin girişi indirebilirsiniz (bir `.txt` dosyası) ve ses çıkış (bir `.wav` dosyası) ve ikincisi için kalite audition.

![Ses yazı tipi testi, bölüm 2](media/custom-voice/voice-font-testing2.png)

## <a name="create-and-use-a-custom-endpoint"></a>Oluşturma ve özel bir uç noktası kullanma

Başarıyla oluşturulan ve ses modelinizi test sonra bir özel metin okuma uç noktasında dağıtın. REST API aracılığıyla metin okuma istekleri yaparken sonra genel uç nokta yerine bu uç noktayı kullanın. Özel uç noktanıza yalnızca yazı dağıtmak için kullanılan abonelik tarafından çağrılabilir.

Yeni özel uç nokta oluşturmak için seçin **uç noktaları** sayfanın üstündeki özel sesli menüsünde. My dağıtılan sesleri sayfası, geçerli özel sesli uç noktalar, kendi tablosunun varsa görüntülenir. Geçerli yerel ayarı tablosunun ilk satırında yansıtılır. Farklı bir dil için bir dağıtım oluşturmak için görüntülenen yerel ayarı değiştirin. (Bu, dağıttığınız ses eşleşmelidir.)

Tıklayın **dağıtma sesleri** yeni bir uç noktası oluşturma düğmesi. Özel uç noktanıza açıklamasını ve adını girin.

Abonelik menüsünde, kullanmak istediğiniz aboneliği seçin. Ücretsiz aboneliği kullanıcıları, dağıtılan bir kerede yalnızca bir model olabilir. Standart abonelik kullanıcıların her biri kendi özel sesli 20 uç oluşturabilir.

![Uç nokta oluşturma](media/custom-voice/create-endpoint.png)

Dağıtılacak model seçtikten sonra **Oluştur**. My dağıtılan sesleri sayfası, artık yeni uç noktanız için bir girişi ile yeniden görüntülenir. Bu, yeni bir uç noktayı örneklemek için birkaç dakika sürebilir. Dağıtım durumu başarılı olduğunda, uç noktayı kullanıma hazırdır.

![My dağıtılan sesleri](media/custom-voice/my-deployed-voices.png)

Dağıtım durumu başarılı olduğunda, dağıtılan ses tipi uç noktası My dağıtılan sesleri tabloda görüntülenir. Bir HTTP isteği doğrudan bu URI kullanabilirsiniz.

Çevrimiçi uç noktasını sınama de özel sesli portal kullanılabilir. Uç noktanız test etmek için seçin **test uç noktaları** özel sesli aşağı açılan menüden. Uç nokta sayfasını test etme görünür. Dağıtılan bir özel sesli seçin ve (düz metin veya SSML'yi biçimi) metin kutusuna söylenir için metin girin.

> [!NOTE] 
> SSML'yi, kullanırken `<voice>` etiket oluşturduğunuzda, özel sesli verdiği adı belirtmeniz gerekir.

Tıklayın **Play** içinde özel ses tipi konuşulan metnin duymak.

![Uç nokta test etme](media/custom-voice/endpoint-testing.png)

Özel uç nokta, metin okuma istekleri için kullanılan standart uç nokta işlevsel olarak eşdeğerdir. Bkz: [REST API](rest-apis.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# ' de Konuşma tanıma](quickstart-csharp-dotnet-windows.md)
