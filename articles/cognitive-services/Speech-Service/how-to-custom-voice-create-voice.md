---
title: Özel bir ses - konuşma hizmetleri oluşturma
titlesuffix: Azure Cognitive Services
description: Verilerinizi yüklemeye hazır olduğunuzda, özel sesli portalına gidin. Oluşturma veya özel sesli bir proje seçin. Projeye sağ dil/bölge ve verileri cinsiyet özellikleri paylaşmalıdır ses eğitiminizde kullanmak istiyorsanız.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 6189ea2866d1c16f994179df0179e29353e6c47d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65410707"
---
# <a name="create-a-custom-voice"></a>Özel ses oluşturma

İçinde [özel ses için verileri hazırlama](how-to-custom-voice-prepare-data.md), özel sesli ve farklı biçim gereksinimlerini eğitmek için kullanabileceğiniz farklı veri türleri açıkladığımız. Verilerinizi hazır olduktan sonra bunları karşıya başlayabilirsiniz [özel sesli portalı](https://aka.ms/custom-voice-portal), özel sesli eğitim API'si aracılığıyla veya. Burada özel bir ses portal üzerinden eğitim adımları açıklanmaktadır.

> [!NOTE]
> Bu sayfa okuma izniniz varsayar [özel sesli ile çalışmaya başlama](how-to-custom-voice.md) ve [özel ses için verileri hazırlama](how-to-custom-voice-prepare-data.md)ve özel ses projesi oluşturmuş oldunuz.

Özel ses için desteklenen dilleri denetleyin: [dil özelleştirmesi](language-support.md#customization).

## <a name="upload-your-datasets"></a>Veri kümelerini karşıya yükleme

Verilerinizi yüklemeye hazır olduğunuzda, Git [özel sesli portalı](https://aka.ms/custom-voice-portal). Oluşturma veya özel sesli bir proje seçin. Projeye sağ dil/bölge ve verileri cinsiyet özellikleri paylaşmalıdır ses eğitiminizde kullanmak istiyorsanız. Örneğin, `en-GB` bir Birleşik Krallık Vurgu ile İngilizce dilinde, sahip ses kayıtlarını gerçekleştirilir.

Git **veri** sekmesine **verileri karşıya yükleme**. Sihirbazda hazırlıklarını tamamladığınızdan eşleşen doğru veri türünü seçin.

Karşıya yüklediğiniz her bir veri kümesi, seçtiğiniz veri türü için gereksinimleri karşılaması gerekir. Intune'ye yüklenmeden önce verilerinizi doğru şekilde biçimlendirmek önemlidir. Bu verileri doğru bir şekilde özel sesli hizmeti tarafından işlenecek sağlar. Git [özel ses için verileri hazırlama](how-to-custom-voice-prepare-data.md) ve verilerinizi iletişiminizin biçimlendirilmiş olduğundan emin olun.

> [!NOTE]
> Ücretsiz aboneliği (F0) kullanıcıları, aynı anda iki veri kümesi yükleyebilirsiniz. Standart aboneliği (S0) kullanıcıları aynı anda beş veri kümelerini karşıya yükleyebilirsiniz. Sınıra ulaştıysanız, alma, veri kümelerinden en az birini tamamlanana kadar bekleyin. Daha sonra yeniden deneyin.

> [!NOTE]
> Maksimum abonelik başına içeri aktarılacak izin verilen veri kümeleri (F0) aboneliği kullanıcıları ve 500 standart aboneliği (S0) kullanıcıları için ücretsiz 10 .zip dosya sayısıdır.

Karşıya yükleme düğmesini yaklaştığınızda veri kümeleri otomatik olarak doğrulanır. Veri doğrulama dizi ses dosyaları, dosya biçimi, boyutu ve örnekleme hızı doğrulamak için denetim içerir. Varsa hataları düzeltin ve yeniden gönderin. Veri alma isteği başarıyla başlatıldığında, az önce yüklediğiniz veri kümesine karşılık gelen veri tablosunda bir giriş görmeniz gerekir.

Aşağıdaki tablo, içeri aktarılan veri kümeleri için işleme durumları gösterir:

| Eyalet | Anlamı |
| ----- | ------- |
| İşleniyor | Veri kümeniz alındı ve işleniyor. |
| Başarılı oldu | Veri kümeniz doğrulandı ve şimdi bir ses model oluşturmak üzere kullanılabilir. |
| Başarısız | Veri işlenirken birçok nedeni, örneğin dosya hataları, veri sorunları veya ağ sorunları nedeniyle başarısız oldu. |

Doğrulama tamamlandıktan sonra toplam sayısı her biri, veri kümeleri için eşleşen konuşma gördüğünüz **konuşma** sütun. Seçtiğiniz veri türü uzun ses Segment gerektiriyorsa, bu sütun yalnızca biz sizin ya da temel alarak, dökümler veya konuşma tanıma hizmeti aracılığıyla segmentlere konuşma yansıtır. Daha fazla veri kümesinin başarıyla içeri aktarıldı konuşma ayrıntılı sonuçlarını ve bunların eşleme Dökümleri gibi görüntülemek için doğrulanmış indirebilirsiniz. İpucu: uzun ses Segment veri işleme tamamlanması bir saat sürebilir.

En-US ve zh-CN veri kümeleri için Söyleniş puanları ve gürültü düzeyi her kayıtlarınızın denetlemek için bir rapor daha da indirebilirsiniz. Söyleniş puanı aralık 0-100. 70'in altında bir puan genellikle bir konuşma hata veya betik uyuşmazlığı gösterir. Ağır bir Vurgu, Söyleniş puanınız azaltmak ve oluşturulan dijital ses etkiler.

Daha yüksek bir sinyal/gürültü oranına (SNR) daha düşük paraziti ses de gösterir. Genellikle, profesyonel studios, kayıt tarafından 50'den fazla SNR ulaşabilirsiniz. 20 aşağıda bir SNR sesle belirgin paraziti oluşturulan sesinizi de neden olabilir.

Herhangi bir konuşma düşük telaffuz puanları veya zayıf sinyal/gürültü oranları yeniden kaydetmeyi deneyin. Yeniden kayıt yapamazsınız, bu konuşma, veri kümesinden dışlamak.

## <a name="build-your-custom-voice-model"></a>Özel ses modelinizi derleme

Veri kümeniz doğrulandıktan sonra özel sesli modelinizi oluşturmak için kullanabilirsiniz.

1.  Gidin **metin okuma > özel sesli > Eğitim**.

2.  Tıklayın **modeli eğitme**.

3.  Ardından, girin bir **adı** ve **açıklama** bu modeli tanımlamanıza yardımcı olacak.

    Dikkatli bir şekilde bir ad seçin. Buraya girdiğiniz ad, isteğiniz konuşma sentezi için ses giriş SSML'yi bir parçası olarak belirtmek için kullandığınız ad olacaktır. Yalnızca harf, rakam ve bazı noktalama karakterleri gibi - \_ve (',') izin verilir. Farklı bir ses modelleri için farklı adlar kullanın.

    Yaygın **açıklama** alandır modeli oluşturmak için kullanılan veri kümelerinin adlarındaki harflerde kaydetmek için.

4.  Gelen **seçin eğitim verilerini** eğitim için kullanmak istediğiniz bir veya birden çok veri kümesi seçin. Bunları göndermeden önce Konuşma sayısını denetleyin. Herhangi bir sayıda en-US ve zh-CN ses modellerine yönelik Konuşma ile başlayabilirsiniz. Diğer ülkeler için bir ses eğitmek için 2. 000'den fazla konuşma seçmeniz gerekir.

    > [!NOTE]
    > Yinelenen ses adları eğitimleri kaldırılacak. Seçtiğiniz veri kümeleri arasında birden fazla .zip dosyasını aynı ses adlarını içerip içermediğini emin olun.

    > [!TIP]
    > Veri kümeleri aynı hoparlör kullanarak kalitesi için gereklidir. Eğitim için gönderdiğiniz veri kümeleri, toplam 6000'den az farklı konuşma birtakım içerdiğinde, istatistiksel parametrik sentezi teknik aracılığıyla sesli modelinizi eğitmek. Burada eğitim verilerinizi bir toplam 6000 ayrı konuşma aşıyor durumda birleştirme sentezi teknik eğitim işlemine kazandırın. Normalde birleştirme teknoloji, daha doğal ve daha yüksek kaliteli sesli sonuca neden olabilir. [Özel ses ekibiyle](mailto:speechsupport@microsoft.com) genel kullanıma eşdeğer bir dijital ses oluşturan son sinir TTS teknolojisine sahip bir model eğitip istiyorsanız [sinir sesleri](language-support.md#neural-voices).

5.  Tıklayın **eğitme** ses modelinizi oluşturmaya başlayın.

Bu yeni modeli oluşturulan eğitim tablo karşılık gelen yeni bir giriş görüntüler. Tablo, ayrıca durumu görüntüler: İşlem, başarılı, başarısız.

Gösterilen durum veri kümenizde sesli modeline dönüştürme işlemidir, burada gösterildiği gibi yansıtır.

| Eyalet | Anlamı |
| ----- | ------- |
| İşleniyor | Ses modelinizi oluşturuluyor. |
| Başarılı oldu | Ses modelinizi oluşturuldu ve dağıtılabilir. |
| Başarısız | Ses modelinizi eğitim birçok nedeni, örneğin görünmeyen veri sorunları veya ağ sorunları nedeniyle başarısız oldu. |

Zaman eğitim işlenen ses veri hacmine bağlı olarak değişir. Tipik bir kez gelen hakkında konuşma yüzlerce 30 dakika ila 20.000 konuşma 40 saat aralığı. Model eğitim başarılı sonra test etmek başlatabilirsiniz.

> [!NOTE]
> Ücretsiz aboneliği (F0) kullanıcıları, aynı anda bir ses tipi eğitebilirsiniz. Standart aboneliği (S0) kullanıcıları, aynı anda üç ses eğitebilirsiniz. Sınıra ulaştıysanız, en az bir ses yazı tipleri eğitim tamamlanana kadar bekleyin ve işlemi yeniden deneyin.

> [!NOTE]
> Ses modelleri abonelik başına düşünürler izin verilen en fazla sayısını 10 modele ücretsiz abonelik (F0) kullanıcılar için ve standart aboneliği (S0) kullanıcıları için 100 ' dir.

## <a name="test-your-voice-model"></a>Ses modelinizi test etme

Kendi ses tipi başarıyla oluşturulduktan sonra kullanım için dağıtmadan önce test edebilirsiniz.

1.  Gidin **metin okuma > özel sesli > test**.

2.  Tıklayın **Ekle test**.

3.  Test etmek istediğiniz bir veya birden çok modeli seçin.

4.  Konuşmaya voice(s) istediğiniz metni girin. Tek seferde birden çok modeli test etmek için seçtiyseniz, aynı metin için farklı modelleri test etmek için kullanılır.

    > [!NOTE]
    > Dilin metin, ses tipi dili ile aynı olması gerekir. Yalnızca başarıyla eğitilen modelleri test edilebilir. Bu adımda yalnızca düz metin desteklenir.

5.  **Oluştur**’a tıklayın.

Test isteğinizi gönderdikten sonra test sayfasına döndürür. Tablo, artık yeni isteğinizi ve Durum sütununda karşılık gelen bir giriş içerir. Bu konuşma sentezlemek için birkaç dakika sürebilir. Durum sütununda ne diyor **başarılı**ses çal veya metin girişi (bir .txt dosyası) indirin ve ses çıkış (.wav dosyası) ve daha kaliteli ikincisi audition.

Test etmek için seçtiğiniz her model ayrıntı sayfasında test sonuçlarını da bulabilirsiniz. Git **eğitim** sekme ve model ayrıntı sayfası girmek için model adına tıklayın.

## <a name="create-and-use-a-custom-voice-endpoint"></a>Oluşturma ve bir özel sesli uç noktası kullanma

Başarılı bir şekilde oluşturduğunuz ve ses modelinizi test sonra bir özel metin okuma uç noktasında dağıtın. REST API aracılığıyla metin okuma istekleri yaparken sonra genel uç nokta yerine bu uç noktayı kullanın. Özel uç noktanıza yalnızca yazı dağıtmak için kullanılan abonelik tarafından çağrılabilir.

Yeni bir özel sesli uç noktası oluşturmak için Git **metin okuma > özel sesli > Dağıtım**. Seçin **uç noktası ekleme** girin bir **adı** ve **açıklama** özel uç noktanız için. Ardından bu uç nokta ile ilişkilendirmek istediğiniz özel sesli modeli seçin.

Tıklattıktan sonra **Ekle** düğme uç noktası tablosunda yeni uç noktanız için bir giriş görürsünüz. Bu, yeni bir uç noktayı örneklemek için birkaç dakika sürebilir. Dağıtım durumu olduğunda **başarılı**, uç noktayı kullanıma hazırdır.

> [!NOTE]
> Ücretsiz aboneliği (F0) kullanıcılar yalnızca bir model dağıtılmış olabilir. Standart aboneliği (S0) kullanıcıları en çok 50 uç noktalar, her biri kendi özel sesli oluşturabilirsiniz.

> [!NOTE]
> Özel sesinizi kullanılacak ses model adını belirtin, bir HTTP isteği doğrudan özel bir URI kullanın ve TTS hizmet kimlik doğrulaması geçirmek için aynı abonelik kullanın.

Uç noktanız dağıtıldıktan sonra uç nokta adına bir bağlantı olarak görünür. Uç nokta, uç nokta URL'si ve örnek kod gibi uç noktanıza özel bilgileri görüntülemek için bağlantıya tıklayın.

Çevrimiçi uç noktasını sınama de özel sesli portal kullanılabilir. Uç noktanız test etmek için seçin **endpoint denetle** gelen **uç noktası ayrıntıları** sayfası. Uç nokta sayfasını test etme görünür. Söylenir için metin girin (ya da düz metin veya [SSML'yi biçimi](speech-synthesis-markup.md) metin kutusuna. İçinde özel ses tipi konuşulan metnin duymak seçin **Play**. Bu test özelliği, özel konuşma sentezi kullanımınıza göre ücretlendirilir.

Özel uç nokta, metin okuma istekleri için kullanılan standart uç nokta işlevsel olarak eşdeğerdir. Bkz: [REST API](rest-text-to-speech.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Kılavuzu: Ses örneklerinizi kaydedin](record-custom-voice-samples.md)
* [Metin okuma API Başvurusu](rest-text-to-speech.md)
