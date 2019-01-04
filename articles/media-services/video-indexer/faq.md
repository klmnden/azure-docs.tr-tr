---
title: Video Indexer - Azure hakkında sık sorulan sorular
titlesuffix: Azure Media Services
description: Video Indexer hakkında sık sorulan soruların yanıtlarını alın.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 12/19/2018
ms.author: juliako
ms.openlocfilehash: 972858b4bae995b6e9073cf7ee451a317e9f3909
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731575"
---
# <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

Bu makalede, Video Indexer hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="general-questions"></a>Genel Sorular

### <a name="what-is-video-indexer"></a>Video Indexer nedir?

Video Indexer bir Azure Media Services media ses ve video dosyaları için meta veri ayıklama hizmetidir. Sınıflandırma, analiz ve video ve ses içeriği kullanarak Öngörüler elde etmeye daha kolay hale getirmek için zengin bir makine öğrenme algoritmalarını kullanan [önceden tanımlanmış modelleri](video-indexer-overview.md). Insights yararlanan yapı yeni deneyimleri veya yeni kazanç fırsatlarını oluşturma içerik bulunabilirlik ve erişilebilirlik, geliştirme gibi birçok yönden öngörülerden kullanabilirsiniz.

Video Indexer, test etme, yapılandırma ve özelleştirme ve üretim sistemle tümleştirmek, geliştiriciler için REST tabanlı bir API için web tabanlı bir arabirim sağlar.

### <a name="what-can-i-do-with-video-indexer"></a>Video Indexer ile ne yapabilirim?

Video Indexer, aşağıdaki türlerde medya dosyalarını işlemleri gerçekleştirebilirsiniz:

* Tanımlamak ve konuşma ayıklayın ve konuşmacıları belirleyin.
* Tanımlamak ve ekran ayıklayın video metin.
* Tanımlayın ve video dosyası nesneleri.
* Ses parçalarını ve ekran markaları Microsoft gibi bir video metinde belirleyin.
* Algılar ve ünlüleri, bir veritabanı ve kullanıcı tanımlı veritabanı dikdörtgenlerini yüzleri tanır.
* Görsel ve konuşma metne göre ses ve video içeriğinden anahtar kelimeleri ayıklayın.
* Ele alınan ancak mutlaka açıkça görsel ve konuşma metne göre ses ve video içeriğine belirtilen konuları ayıklar.
* Kapalı açıklamalı alt yazıları ve alt yazıları ses parçasından oluşturun.

Daha fazla bilgi için bkz. [Genel Bakış](video-indexer-overview.md).

### <a name="how-do-i-get-started-with-video-indexer"></a>Video Indexer ile nasıl başlayabilirim?

Video Indexer denemek ücretsiz olarak kullanılabilir. Ücretsiz deneme 600 dakika içinde web tabanlı bir arabirim ve 2.400 dakika API aracılığıyla sunar.

Dizin video ve ses uçarak uygun ölçekte, Video Indexer, ücretli bir Microsoft Azure aboneliğine bağlanabilirsiniz. Fiyatlandırma hakkında daha fazla bilgi bulabilirsiniz [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) sayfası.

Çalışmaya başlama hakkında daha fazla bilgi bulabilirsiniz [başlama](video-indexer-get-started.md).

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>Video Indexer'ı kullanmak için kodlama gerekiyor mu?

Video Indexer API ardışık düzeninizi tümleştirme veya Video Indexer portalı hiç kod yazmanız gerekmez. 

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>Video Indexer'ı kullanmak için machine learning becerileri ihtiyacım var?

Hayır, öngörüleri, video ve ses içeriği tamamen becerilerine veya algoritmaları hakkında bilgi bankasına öğrenme herhangi bir makineye olmadan ayıklayabilir. 

### <a name="what-media-formats-does-video-indexer-support"></a>Hangi medya biçimleri Video Indexer destek mu?

Video Indexer, en yaygın ortam biçimlerini destekler. Daha fazla ayrıntı için Azure Medya Kodlayıcısı standart biçim listesine bakın.

### <a name="how-to-do-i-upload-a-media-into-video-indexer"></a>Video Indexer ile medya yükleyebilirim yapmak için nasıl?

Video Indexer web tabanlı portal dosya karşıya yükleme iletişim kutusu kullanılarak bir medya dosyasını karşıya yükleyebilir veya URL'sini doğrudan işaret ederek dosyasını belirtir. Örneğin, `http://contoso.cloudapp.net/videos/example.mp4`. Bir video oynatıcı gibi içeren bir sayfaya bağlantı `http://contoso.cloudapp.net/videos` çalışmaz. Video Indexer API URL ya da bir bayt dizisi aracılığıyla giriş dosyası belirtmenizi gerektirir. Bir URL yükler Not 10 GB ile sınırlıdır, ancak bir zaman süresi sınırı yoktur. Daha fazla bilgi için lütfen bu bakın [yapılır kılavuzunda](upload-index-videos.md).

### <a name="how-long-does-it-take-visual-indexer-to-extract-insights-from-media"></a>Görsel medyadan öngörüleri ayıklamak için dizin oluşturucu ne kadar sürer?

Dosya uzunluğu ve kalitesi, işlem sayısını dosyasında bulunan ınsights sayısı gibi birden çok parametre, her iki Video Indexer API ve web tabanlı Video Indexer arabirimini kullanarak bir ses veya video dosyası dizini oluşturmak için gereken süreyi bağlıdır kullanılabilir, sahip ve akış uç noktası etkinleştirilip etkinleştirilmediğini gösterir. 10 S3 RU etkin varsayılarak, çoğu içerik türleri için biz dizin oluşturma için ½ videonun uzunluğu 1/3 alabilir tahmin edin.  Ancak, kendi içeriğiyle birkaç test dosyalarını çalıştırmak ve daha iyi bir fikir almak için ortalama ele öneririz. 

### <a name="in-which-azure-regions-is-video-indexer-available"></a>Video Indexer, hangi Azure bölgelerinde kullanılabilir?

Hangi Azure bölgeleri Video Indexer kullanılabilir gördüğünüz [bölgeleri](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all) sayfası.

### <a name="what-is-the-sla-for-video-indexer"></a>Video Indexer için SLA'sı nedir?

Ücretsiz deneme Video Indexer için SLA yoktur. Azure medya Hizmetleri SLA kapsar Video Indexer.

Bilgileri bulabilirsiniz [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_0/) sayfası.

## <a name="billing-questions"></a>Faturalandırma Soruları

### <a name="how-much-does-video-indexer-cost"></a>Video Indexer nin ücreti ne kadardır?

Video Indexer, basit bir ödeme giriş içeriğin süresine göre fiyatlandırma modeli kullandığınız kadarını kullanır. Ek ücretleri, kodlama, akış, depolama, ağ kullanımı ve medya ayrılmış birimleri uygulanabilir. Geçerli fiyatlandırma için başvurun [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) sayfası.

### <a name="does-video-indexer-offer-a-free-trial"></a>Video Indexer ücretsiz bir deneme sunar?

Evet, Video Indexer, Doğu ABD Azure bölgesinde barındırılıyor ve tam hizmet ve API işlevselliğini sağlar ve ücretsiz bir deneme sunar. 10 saatliğine videoların Web sitesi kullanıcılar için ve API kullanıcılar için 40 saatlik kota yok. 

## <a name="security-questions"></a>Güvenlik Soruları

### <a name="are-audio-and-video-files-indexed-by-video-indexer-stored"></a>Ses ve video dosyaları, Video Indexer tarafından depolanan dizine eklenir?

Evet, Video Indexer Web sitesi veya API kullanarak Video dizin oluşturucudan dosyayı silmeniz sürece, video ve ses dosyalarını depolanır. Ücretsiz deneme, video ve ses dosyaları için dizin Azure Doğu ABD bölgesinde saklanır. Aksi takdirde, video ve ses dosyalarını Azure aboneliğinizin depolama hesabında depolanır.

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>Ben, Video Indexer Portalı'nda depolanan dosyalarımı silebilir miyim?

Evet, her zaman, video ve ses dosyalarını ve bunun yanı sıra öngörülerini Video dizin oluşturucudan silebilirsiniz. Dosyaları sildikten sonra Video Indexer ve her yerde (Azure bölge Doğu ABD için deneme hesapları ve Azure aboneliğinizin aksi depolama hesabı) dosya Video Indexer depolanan kalıcı olarak kaybolur.

### <a name="who-has-access-to-my-audio-and-video-files-and-that-have-been-indexed-andor-stored-by-video-indexer"></a>Ses ve video dosyalarımı kimlerin erişebileceğini ve, dizin ve/veya Video Indexer tarafından depolanan?

Evet, her zaman, video ve ses dosyalarını ve bunun yanı sıra öngörülerini Video dizin oluşturucudan silebilirsiniz. Dosyaları sildikten sonra Video Indexer ve her yerde (Azure bölge Doğu ABD için deneme hesapları ve Azure aboneliğinizin aksi depolama hesabı) dosya Video Indexer depolanan kalıcı olarak kaybolur.

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>Video Indexer hesabım için kullanıcı erişimi denetleyebilir miyim?

Evet, hesaba davet seçin.

### <a name="who-has-access-to-the-insights-that-were-extracted-from-my-audio-and-video-files-by-video-indexer"></a>Video Indexer tarafından my ses ve video dosyalarından ayıklanan öngörülere erişim kimler?

Genel olarak gizlilik ayarına sahip videolarınızı, videonuzu ve kendi öngörüleri bağlantısını olan herkes tarafından erişilebilir. Özel olarak gizlilik ayarına sahip videolarınızı, yalnızca videonun hesaba davet kullanıcılar tarafından erişilebilir.

### <a name="do-i-still-own-my-content-that-have-been-indexed-and-stored-by-video-indexer"></a>Dizini oluşturulan ve Video Indexer tarafından depolanan İçeriğim kendi?

Evet, başına [Azure çevrimiçi hizmet koşulları](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), içerik sahibi.

### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>Video Indexer: Video Indexer burada kullanmakta olduğum Azure bölgesi içinde tutulan tarafından içeriği dizine?

Birden fazla Azure bölgesini kullanan Azure aboneliğinizde bir el ile yapılandırma olmadığı sürece Evet, içerik ve kendi ınsights Azure bölgesi içinde tutulacak. 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>Video Indexer gizlilik ilkesi nedir?

Video Indexer tarafından kapsanan [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement).

## <a name="api-questions"></a>API sorular

### <a name="what-are-the-differences-in-capability-on-the-video-indexer-website-versus-the-video-indexer-api"></a>Video Indexer API karşı Video Indexer sitesinde özellik farkları nelerdir?

İle [Video Indexer](https://www.videoindexer.com) Web sitesi, video kitaplığınızda arama, ınsights analiz, modelleri özelleştirebilir, hesabınızı yapılandırın ve videolarınızı kolayca düzenleyin. Güçlü ile [Video Indexer REST API](https://api-portal.videoindexer.ai/), herhangi bir altyapısıyla tümleştirin veya doğrudan kendi Web sitenize veya uygulamanıza pencere öğeleri ekleyin.

### <a name="what-apis-does-video-indexer-offer"></a>Video Indexer hangi API'ler sunar?

Video Indexer API'ları kullanma hakkında daha fazla bilgi bulabilirsiniz [Video Indexer Geliştirici Portalı](https://api-portal.videoindexer.ai/)

### <a name="how-do-i-get-started-with-video-indexers-api"></a>Video Indexer'ın API'sini kullanmaya nasıl başlayabilirim?

İzleyin [Öğreticisi: Video dizinleyici API'sini kullanmaya başlama](video-indexer-use-apis.md).

### <a name="what-is-an-access-token"></a>Bir erişim belirteci nedir?

Video Indexer API, bir yetkilendirme API ve işlemleri API içerir. Yetkilendirmeleri API size çağrıları içeren erişim belirteci. İşlemler API'sine yapılan her çağrı, çağrının yetkilendirme kapsamına uyan bir erişim belirteci ile ilişkilendirilmelidir.

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>Hesap erişim belirtecini, kullanıcı erişim belirteci ve Video erişim belirteci arasındaki fark nedir?

Bu çağrı yetkilendirme kapsamı temsil eder.

* Kullanıcı düzeyinde - kullanıcı düzeyinde erişim belirteçleri kullanıcı düzeyinde işlemler gerçekleştirmenize olanak tanır. Örnek: ilişkili hesaplar alma.
* Hesap düzeyi – hesap düzeyinde erişim belirteçleri, hesap düzeyinde veya video düzeyinde işlemleri sağlar. Örnek: karşıya video yükleme, tüm videoları listeleme, video içgörüsü alma vb.
* Video düzeyindeki – belirli bir video işlemleri video düzeyinde erişim belirteçleri sağlar. Örnek: video içgörüsü alma, açıklamalı alt yazı indirme, pencere öğesi alma vb.

Bu belirteçler, salt okunur veya allowEdit belirterek düzenlemeye izin denetleyebilirsiniz = true/false.

### <a name="why-do-i-need-access-tokens-when-using-the-video-indexer-apis"></a>Erişim belirteçleri neden Video Indexer API kullanırken gerekiyor?

Erişim belirteçleri, Video Indexer API'ları güvenlik amacıyla kullanmak için gereklidir. Bu, tüm çağrıları, ya da hesabınıza erişim izinlerine sahip olanlar geldiğini sağlar. 

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>Yeni bir erişim belirteci almak ne sıklıkla ihtiyacım var? Ne zaman erişim belirteçleri süresi dolar mı?

Erişim belirteçleri, her saat yeni bir erişim belirteci oluşturmak gereken şekilde her saat sona erer. 

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](video-indexer-overview.md)
