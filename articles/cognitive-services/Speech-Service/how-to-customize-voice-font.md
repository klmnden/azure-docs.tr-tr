---
title: Özel ses tipi oluşturma
titlesuffix: Azure Cognitive Services
description: Bu makalede metin okuma ses özelleştirme tanınabilir, tür, bir marka ses oluşturmanızı sağlayan bir genel bakıştır.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: panosper
ms.openlocfilehash: 24b98ce8cd2c587f0d39390954eb8a64747ca2ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60654080"
---
# <a name="creating-custom-voice-fonts"></a>Özel ses tipi olarak oluşturma

Metin okuma (TTS) ses özelleştirme markanız için tanınan, tür, tek bir ses oluşturmanıza olanak sağlar: bir *ses tipi.*

Ses tipi oluşturmak için studio kaydını yapabilir ve ilişkili betikler eğitim verileri olarak karşıya yükleyin. Hizmet, ardından kaydınız için ayarlanmış bir benzersiz ses modeli oluşturur. Bu ses tipi konuşma sentezlemek için kullanabilirsiniz.

Az miktarda bir kavram kanıtı için verileri ile başlayabilirsiniz. Ancak daha fazla veri sağlarsanız, daha doğal ve professional, ses çalar.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure hesabı ve konuşma hizmeti için bir abonelik gerekir. [Bir oluşturma](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) henüz yapmadıysanız. Aboneliğiniz, burada gösterildiği gibi özel sesli Portalı'na bağlanın.

1. Oturum [özel sesli portalı](https://customvoice.ai) erişimi uygulamak için kullandığınız aynı Microsoft hesabını kullanarak.

2. Hesap adınızın altında sağ üst kısımdaki, Git **abonelikleri**.

    ![Subscriptions](media/custom-voice/subscriptions.png)

3. Abonelikler sayfasında seçin **mevcut aboneliğe bağlanma**. Konuşma Hizmetleri farklı bölgelerdeki desteklediğini unutmayın. Burada abonelik anahtarınızı oluşturuldu ve anahtarınızı doğru alt portala bağlandığınızdan emin olun bölgeyi denetleyin.  

4. Aşağıdaki örnekte gösterildiği gibi abonelik anahtarınızı tabloya yapıştırın. Her aboneliğin iki anahtarı vardır ve bunlardan birini kullanabilirsiniz.

     ![Abonelik Ekle](media/custom-voice/add-subscription.png)

Başlamaya hazırsınız!

## <a name="prepare-recordings-and-transcripts"></a>Kayıtları ve dökümler hazırlama

Ses dosyaları, ses dosyalarını dökümleri içeren bir metin dosyası ile birlikte bir dizi Ses Eğitim veri kümesi oluşur.

Bu dosya iki yolla hazırlayabilirsiniz. Ya da bir betik yazabilir ve ses beceri tarafından okunur veya herkese ses kullanın ve metin özelliği vardır. İkincisini yapmak, disfluencies "um" ve diğer filler sesleri, atlar, mumbled sözcükleri veya mispronunciations gibi ses dosyalarını düzenleyin.

İyi ses tipi üretmek için kayıtları sessiz bir odada ile yüksek kaliteli bir mikrofon olun. Tutarlı birim oranı, aralık ve konuşma ifadesel veren davranışların gibi konuşma harika bir dijital ses oluşturmak için gerekli.

Üretim kullanımı için bir ses oluşturmak için profesyonel kaydı studio ve ses beceri kullanmanızı öneririz. Daha fazla bilgi için [ses kaydetmek nasıl özel bir ses için örnekleri](record-custom-voice-samples.md).

### <a name="audio-files"></a>Ses dosyaları

Her ses dosyası, tek bir utterance (örneğin, tek bir cümle veya bir iletişim sistemi olan tek bir dönüş) içermelidir. Tüm dosyalar aynı dilde olmalıdır. (Çok dilli özel seslerle desteklenmez.) Ses dosyalarını ayrıca her dosya adı uzantısına sahip benzersiz bir sayısal filename olmalıdır `.wav`.

Ses dosyaları gibi hazırlıklı olmalıdır. Diğer biçimlere desteklenmez ve reddedilir.

| **Özellik** | **Değer** |
| ------------ | --------- |
| Dosya biçimi  | RIFF (.wav)|
| Örnekleme hızı| en az 16. 000 Hz |
| Örnek Biçim| PCM, 16-bit |
| Dosya adı    | Sayısal ile `.wav` uzantısı |
| Arşiv biçimi| .zip      |
| En uzun arşiv boyutu|200 MB|

> [!NOTE]
> .wav dosyaları 16.000 Hertz reddedilir daha düşük bir örnekleme hızı. Bir .zip dosyası farklı bir örnekleme hızı Dalgalar içeriyorsa, yalnızca eşit veya daha yüksek 16.000 Hz aktarılır.
> Portal şu anda içeri aktarmalar .zip 200 MB'a kadar arşivler. Ancak, birden fazla arşivini karşıya yüklenebilir. En fazla izin verilen veri kümeleri 10 .zip ücretsiz aboneliği kullanıcılarını ve standart aboneliği kullanıcıları için 50 dosya sayısıdır.

### <a name="transcripts"></a>Dökümleri

Düz metin dosyası döküm dosyasıdır (ANSI, UTF-8, UTF-8-BOM, UTF-16-LE veya UTF-16 olabilir). Döküm dosyasının her satır bir sekme (kod noktası 9) karakteri oynatırken son ardından ve bir ses dosyasının adı olmalıdır. Boş satır izin verilir.

Örneğin:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

Özel ses sistem metni küçük harfe ve kaldırma fazlalık noktalama dönüştürerek dökümleri normalleştirir. Dökümler ilgili ses kayıtları, doğru döküm % 100 olduğunu önemlidir.

> [!TIP]
> Zaman içinde üretim metin okuma sesleri, konuşma seçin (veya yazma betikler) oluşturma Al fonetik kapsamı hem verimlilik hesap. Sonuçları aşmakta sorun istiyor musunuz? [Özel ses ekibiyle](mailto:speechsupport@microsoft.com) bize sahip hakkında daha fazla inceleyin çıkış bulunacak.

## <a name="upload-your-datasets"></a>Veri kümelerini karşıya yükleme

Ses dosyası Arşiv ve dökümler hazırladıktan sonra bunları aracılığıyla karşıya [özel sesli hizmet portalı](https://customvoice.ai).

> [!NOTE]
> Veri kümeleri, karşıya yüklediğiniz sonra düzenlenemez. Ses dosyalarını bazıları dökümleri içeren unutursanız örneğin veya kazayla yanlış cinsiyet seçin veri kümesinin tamamında yeniden yüklemeniz gerekir. Veri kümesi ve ayarları ayrıntılı bir şekilde karşıya yükleme başlamadan önce denetleyin.

1. Portalda oturum açın.

2. Ana sayfada altında **özel sesli**seçin **veri**.   

    **My ses** tablo görünür. Sesli veri kümeleri henüz yüklemediyseniz, boş olur.

3. Yeni bir veri kümesi karşıya yükleme sayfasını açmak için seçin **verileri içeri aktarma**.

    ![Sesli verileri içeri aktar](media/custom-voice/import-voice-data.png)

4. Sağlanan alanları bir ad ve açıklama girin.

5. Ses tipi olarak sizin için bir yerel ayar seçin. Yerel ayar bilgileri kayıt verilerini ve betikleri dilinin eşleştiğinden emin olun.

6. Cinsiyet konuşmacının ses kullanmakta olduğunuz seçin.

7. Betik ve ses dosyalarını karşıya yüklemek için seçin.

8. Seçin **alma** verilerinizi karşıya yükleme. Büyük veri kümeleri için içeri aktarma, birkaç dakika sürebilir.

> [!NOTE]
> Ücretsiz aboneliği kullanıcıları, aynı anda iki veri kümesi yükleyebilirsiniz. Standart abonelik kullanıcılar aynı anda beş veri kümelerini karşıya yükleyebilirsiniz. Sınıra ulaştıysanız, alma, veri kümelerinden en az birini tamamlanana kadar bekleyin. Daha sonra yeniden deneyin.

Karşıya yükleme tamamlandığında, **ses verilerim** tablo tekrar görünür. Yüklediğiniz veri kümesine karşılık gelen bir giriş görmeniz gerekir.

Veri kümelerini karşıya yükledikten sonra otomatik olarak doğrulanır. Veri doğrulama, bir dizi ses dosyaları, dosya biçimi, boyutu ve örnekleme hızı doğrulamak için denetim içerir. Döküm dosyaları üzerinde denetimleri dosya biçimini doğrulayın ve bazı metin normalleştirme yapın. Konuşma tanıma ile Konuşma transcribed. Daha sonra elde edilen metnini sağladığınız döküm ile karşılaştırılır.

![Ses verilerimi](media/custom-voice/my-voice-data.png)

Aşağıdaki tablo, içeri aktarılan veri kümeleri için işleme durumları gösterir:

| Durum | Anlamı
| ----- | -------
| `NotStarted` | Veri kümeniz alındı ve işlenmek üzere kuyruğa alınır.
| `Running` | Veri kümeniz doğrulanmaktadır.
| `Succeeded` | Veri kümeniz doğrulandı ve şimdi bir ses tipi oluşturmak için kullanılabilir.

Doğrulama tamamlandıktan sonra toplam sayısı her biri, veri kümeleri için eşleşen konuşma gördüğünüz **Utterance** sütun.

Söyleniş puanları ve gürültü düzeyi her kayıtlarınızın denetlemek için bir rapor indirebilirsiniz. Söyleniş puanı aralık 0-100. 70'in altında bir puan genellikle bir konuşma hata veya betik uyuşmazlığı gösterir. Ağır bir Vurgu, Söyleniş puanınız azaltmak ve oluşturulan dijital ses etkiler.

Daha yüksek bir sinyal/gürültü oranına (SNR) daha düşük paraziti ses de gösterir. Genellikle, profesyonel studios, kayıt tarafından 50'den fazla SNR ulaşabilirsiniz. 20 aşağıda bir SNR sesle belirgin paraziti oluşturulan sesinizi de neden olabilir.

Herhangi bir konuşma düşük telaffuz puanları veya zayıf sinyal/gürültü oranları yeniden kaydetmeyi deneyin. Yeniden kayıt yapamazsınız, bu konuşma, veri kümesinden dışlamak.

## <a name="build-your-voice-font"></a>Derleme, ses tipi

Veri kümeniz doğrulandıktan sonra özel ses tipi oluşturmak için kullanabilirsiniz.

1.  İçinde **özel sesli** aşağı açılan menüyü seçin **modelleri**.

    **My ses tiplerini** zaten oluşturmuş olduğunuz tüm özel ses tipi listeleme, tablo görünür.

1. Tablo başlığı altında seçin **oluşturma sesleri**.

    Bir ses tipi oluşturma sayfası görüntülenir. Geçerli yerel ayarı tablosunun ilk satırında gösterilir. Başka bir dilde bir ses oluşturmak için yerel ayarını değiştirin. Ses oluşturmak için kullanılan veri kümeleri için olduğu gibi yerel ayarı aynı olmalıdır.

1. Veri kümeniz yüklediğiniz zaman yaptığınız gibi bir ad ve bu modeli tanımlamanıza yardımcı olması için bir açıklama girin.

    Dikkatli bir şekilde bir ad seçin. Buraya girdiğiniz ad, isteğiniz konuşma sentezi için ses giriş SSML'yi bir parçası olarak belirtmek için kullandığınız ad olacaktır. Yalnızca harf, rakam ve gibi bazı noktalama karakterleri `-`, `_`, ve `(', ')` izin verilir.

    Yaygın **açıklama** alandır modeli oluşturmak için kullanılan veri kümelerinin adlarındaki harflerde kaydetmek için.

1. Kendi ses tipi dinleyicilerinin seçin. Bu veri kümesinin cinsiyet eşleşmelidir.

1. Ses tipi eğitim için kullanmak istediğiniz veri kümelerindeki seçin. Kullandığınız tüm veri kümeleri aynı Konuşmacı olmalıdır.

1. Tıklayın **Oluştur** , ses tipi oluşturmaya başlayın.

    ![Model oluşturma](media/custom-voice/create-model.png)

Yeni modelinizi görünür **My ses tiplerini** tablo.

![My ses tipleri](media/custom-voice/my-voice-fonts.png)

Gösterilen durumu burada gösterildiği gibi bir ses tipi için veri dönüştürme işlemi yansıtır.

| Durum | Anlamı
| ----- | -------
| `NotStarted` | Ses yazı tipi oluşturma isteği, işlenmek üzere kuyruğa alınır.
| `Running` | Kendi ses tipi oluşturuluyor.
| `Succeeded` | Kendi ses tipi oluşturuldu ve dağıtılabilir.

Zaman eğitim işlenen ses veri hacmine bağlı olarak değişir. Tipik bir kez gelen hakkında konuşma yüzlerce 30 dakika ila 20.000 konuşma 40 saat aralığı.

> [!NOTE]
> Ücretsiz aboneliği kullanıcıları, aynı anda bir ses tipi eğitebilirsiniz. Standart abonelik kullanıcılar aynı anda üç ses eğitebilirsiniz. Sınıra ulaştıysanız, en az bir ses yazı tipleri eğitim tamamlanana kadar bekleyin ve işlemi yeniden deneyin.

## <a name="test-your-voice-font"></a>Test, ses tipi

Kendi ses tipi başarıyla oluşturulduktan sonra kullanım için dağıtmadan önce test edebilirsiniz. İçinde **işlemleri** sütunundaki **Test**. Test sayfası için seçilen ses tipi görüntülenir. Ses için herhangi bir testi istekleri henüz göndermediniz, tablo boşsa.

Metin istekleri göndermek için açılan menüyü görüntülemek için seçin **metinle Test** düğmesi tablo başlığı altında. Düz metin veya SSML'yi test isteğinizi gönderebilirsiniz. En büyük giriş boyutuna, SSML'yi istek için tüm etiketleri dahil olmak üzere, 1024 karakterdir. Dilin metin, ses tipi dili ile aynı olması gerekir.

Metin kutusunu doldurarak ve giriş modu onayladıktan sonra seçin **Evet** test isteğinizi göndermek ve test sayfasına geri dönün. Tablo, artık yeni isteğinizi ve Durum sütununda karşılık gelen bir giriş içerir. Bu konuşma sentezlemek için birkaç dakika sürebilir. Durum sütununda ne diyor **başarılı**, metin girişi indirebilirsiniz (bir `.txt` dosyası) ve ses çıkış (bir `.wav` dosyası) ve ikincisi için kalite audition.

## <a name="create-and-use-a-custom-endpoint"></a>Oluşturma ve özel bir uç noktası kullanma

Başarılı bir şekilde oluşturduğunuz ve ses modelinizi test sonra bir özel metin okuma uç noktasında dağıtın. REST API aracılığıyla metin okuma istekleri yaparken sonra genel uç nokta yerine bu uç noktayı kullanın. Özel uç noktanıza yalnızca yazı dağıtmak için kullanılan abonelik tarafından çağrılabilir.

Yeni özel uç nokta oluşturmak için seçin **uç noktaları** gelen **özel sesli** sayfanın üstündeki menü. **My dağıtılan sesleri** sayfası görüntülenirse, geçerli özel sesli uç noktalar, kendi tablosunun varsa. Geçerli yerel ayarı tablosunun ilk satırında yansıtılır. Farklı bir dil için bir dağıtım oluşturmak için görüntülenen yerel ayarı değiştirin. (Bu, dağıtım yapıyorsanız ses eşleşmelidir.)

Yeni bir uç nokta oluşturmak için Seç **dağıtma sesleri** düğmesi. Özel uç noktanıza açıklamasını ve adını girin.

Gelen **abonelik** menüsünde, kullanmak istediğiniz aboneliği seçin. Ücretsiz aboneliği kullanıcıları, dağıtılan bir kerede yalnızca bir model olabilir. Standart abonelik kullanıcıların her biri kendi özel sesli 20 uç oluşturabilir.

![Uç nokta oluşturma](media/custom-voice/create-endpoint.png)

Dağıtılacak model seçtikten sonra seçin **Oluştur**. **My dağıtılan sesleri** sayfası yeniden görüntülenir, artık yeni uç noktanız için bir girişi ile. Bu, yeni bir uç noktayı örneklemek için birkaç dakika sürebilir. Dağıtım durumu olduğunda **başarılı**, uç noktayı kullanıma hazırdır.

![My dağıtılan sesleri](media/custom-voice/my-deployed-voices.png)

Dağıtım durumu olduğunda **başarılı**, uç noktası, dağıtılan ses tipi görünür **My dağıtılan sesleri** tablo. Bir HTTP isteği doğrudan bu URI kullanabilirsiniz.

Çevrimiçi uç noktasını sınama de özel sesli portal kullanılabilir. Uç noktanız test etmek için seçin **test uç noktaları** gelen **özel sesli** açılan menüsü. Uç nokta sayfasını test etme görünür. Dağıtılan bir özel sesli seçin ve (düz metin veya SSML'yi biçimi) metin kutusuna söylenir için metin girin.

> [!NOTE]
> SSML'yi, kullanırken `<voice>` etiket oluşturduğunuzda, özel sesli verdiğiniz adı belirtmeniz gerekir. Düz metin gönderirseniz, özel sesli her zaman kullanılır.

İçinde özel ses tipi konuşulan metnin duymak seçin **Play**.

![Uç nokta test etme](media/custom-voice/endpoint-testing.png)

Özel uç nokta, metin okuma istekleri için kullanılan standart uç nokta işlevsel olarak eşdeğerdir. Bkz: [REST API](rest-apis.md) daha fazla bilgi için.

## <a name="language-support"></a>Dil desteği

Ses özelleştirmesi şu dillerde kullanılabilir:

| Dil | Yerel Ayar |
|----------|--------|
| Çince (ana kara) | zh-CN |
| English (US) | en-US |
| Fransızca  | fr-FR |
| Almanca  | de-DE |
| İtalyanca | İt-IT |

> [!NOTE]
> Fransızca, Almanca ve İtalyanca üslup eğitimi 2000'den fazla konuşma bir veri kümesi ile başlar. Çince-İngilizce dilli modelleri de Konuşma 2000'den fazla veri kümesi ile desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Ses örneklerinizi kaydedin](record-custom-voice-samples.md)
