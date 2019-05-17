---
title: Video Indexer - Azure hakkında sık sorulan sorular
titlesuffix: Azure Media Services
description: Video Indexer hakkında sık sorulan soruların yanıtlarını alın.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: f20d718d0b1d3bbdf117e502a380897c79a7905f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799512"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu makalede, Video Indexer hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="general-questions"></a>Genel sorular

### <a name="what-is-video-indexer"></a>Video Indexer nedir?

Video Indexer Microsoft Azure Media Services'ın bir parçası olan bir yapay zeka hizmetidir. Video Indexer, birden çok makine öğrenimi modellerini kolayca videosu hakkında ayrıntılı bilgiler almanıza olanak sağlayan bir düzenleme sağlar. Gelişmiş ve doğru bilgiler sağlamak için Video Indexer sağlar birden çok kanalda video kullanın: ses, konuşma ve görsel. Video Indexer'ın ınsights içerik keşfedilme ve erişilme yeni kazanç fırsatları oluşturmak için oluşturma, geliştirme gibi pek çok yolla kullanılabilir veya ınsights kullanan yapı yeni deneyimler. Video Indexer, test etme, yapılandırma ve özelleştirme hesabınızdaki modelleri için web tabanlı bir arabirim sağlar. Geliştiriciler, bir REST tabanlı API, Video Indexer üretim sistemle tümleştirmek için kullanabilir. 

### <a name="what-can-i-do-with-video-indexer"></a>Video Indexer ile ne yapabilirim?

Video Indexer, medya dosyaları üzerinde gerçekleştirebileceği işlemleri bazıları şunlardır:

* Tanımlama ve konuşma ayıklanması ve konuşmacıları belirleyin.
* Tanımlama ve ekran ayıklama video metin.
* Nesneleri bir video dosyasını algılanıyor.
* Markaları belirleyin (örneğin: Bir videoda Microsoft) ses parçalarını ve ekran metin.
* Algılama ve ünlüleri, bir veritabanı ve kullanıcı tanımlı veritabanı dikdörtgenlerini yüzleri tanıma.
* Ayıklama konuları ele alınan ancak mutlaka ses ve video içerikte bahsedilen.
* Kapalı açıklamalı alt yazıları ve alt yazıları ses parçasından oluşturuluyor.

Daha fazla bilgi ve daha fazla Video Indexer özellikleri için bkz: [genel bakış](video-indexer-overview.md).

### <a name="how-do-i-get-started-with-video-indexer"></a>Video Indexer ile nasıl başlayabilirim?

Video Indexer, ücretsiz bir deneme teklifi 600 dakika içinde web tabanlı bir arabirim ve API aracılığıyla 2.400 dakika ile sağlayan içerir. Yapabilecekleriniz [Video Indexer arabirime web tabanlı oturum açma](https://www.videoindexer.ai/) ve bir Azure aboneliğini ayarlama gerek kalmadan ve herhangi bir web kimliği kullanarak kendiniz için deneyin. 

Dizin video ve ses uçarak uygun ölçekte, Video Indexer, ücretli bir Microsoft Azure aboneliğine bağlanabilirsiniz. Fiyatlandırma hakkında daha fazla bilgi bulabilirsiniz [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) sayfası.

Çalışmaya başlama hakkında daha fazla bilgi bulabilirsiniz [başlama](video-indexer-get-started.md).

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>Video Indexer'ı kullanmak için kodlama gerekiyor mu?

Video Indexer web tabanlı bir arabirim değerlendirmek, yapılandırma ve hesabınızı yönetmek için kullanabileceğiniz **kodlama gerekmeden**.  Daha karmaşık uygulamalar geliştirmek hazır olduğunuzda kullanabileceğiniz [Video Indexer API](https://api-portal.videoindexer.ai/) kendi uygulamalarınızı, web siteleri, Video Indexer'ı tümleştirmek için veya [gibi sunucusuz teknolojilerini kullanarak özel iş akışları Azure Logic Apps](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/) veya Azure işlevleri.

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>Video Indexer'ı kullanmak için machine learning becerileri ihtiyacım var?

Hayır, Video Indexer, birden çok makine öğrenimi modellerini bir komut zinciriyle tümleştirmeyi sağlar. Video Indexer aracılığıyla bir ses veya video dosyası dizin oluşturma, paylaşılan bir zaman çizelgesinde becerileri ya da Müşteri'nin yapmanız gereken algoritmaları hakkında bilgi bankasına öğrenme herhangi bir makineye olmadan ayıklanır ınsights tam bir dizi alır.

### <a name="what-media-formats-does-video-indexer-support"></a>Hangi medya biçimleri Video Indexer destek mu?

Video Indexer, en yaygın ortam biçimlerini destekler. Başvurmak [Azure Media Encoder standard biçimleri](https://docs.microsoft.com/azure/media-services/latest/media-encoder-standard-formats) daha fazla ayrıntı için liste.

### <a name="how-to-do-i-upload-a-media-into-video-indexer"></a>Video Indexer ile medya yükleyebilirim yapmak için nasıl?

Web tabanlı Video Indexer portalında dosya karşıya yükleme iletişim kutusu kullanılarak bir medya dosyasını karşıya yükleyebilir veya URL'sini doğrudan işaret ederek barındıran kaynak dosyası (bkz [örnek](https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4)). Medya Ekleme kodu veya bir iFrame kullanarak içerik ana çalışmaz herhangi bir URL (bkz [örnek](https://www.videoindexer.ai/accounts/7e1282e8-083c-46ab-8c20-84cae3dc289d/videos/5cfa29e152/?t=4.11)). Video Indexer API URL ya da bir bayt dizisi aracılığıyla giriş dosyası belirtmenizi gerektirir. API kullanarak bir URL aracılığıyla karşıya 10 GB ile sınırlıdır, ancak bir zaman süresi sınırı yoktur. Daha fazla bilgi için lütfen bu bakın [yapılır kılavuzunda](https://docs.microsoft.com/azure/media-services/video-indexer/upload-index-videos).

### <a name="how-long-does-it-take-video-indexer-to-extract-insights-from-media"></a>Video Indexer'ın medyadan öngörüleri ayıklamak için ne kadar sürer?

Insights sayısını sayısınıdosyasındabulunanherikiVideoIndexerAPIvewebtabanlıVideoIndexerarabiriminikullanarakbirsesveyavideodosyasıdizinioluşturmakiçingerekensüreyidosyauzunluğuvekalitesi,gibibirdençokparametrebağlıdır[ayrılmış birimleri](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview) kullanılabilir olup olmadığını ve [akış uç noktası](https://docs.microsoft.com/azure/media-services/previous/media-services-streaming-endpoints-overview) etkin olup olmadığı. Kendi içeriğinizi birkaç test dosyalarını çalıştırmak ve daha iyi bir fikir almak için ortalama ele öneririz.

### <a name="can-i-create-customized-workflows-to-automate-processes-with-video-indexer"></a>Video Indexer ile işlemleri otomatik hale getirmek için özelleştirilmiş iş akışları oluşturabilir miyim?

Evet, Video Indexer Flow, Logic Apps gibi sunucusuz teknolojilerini tümleştirebileceğiniz ve [Azure işlevleri](https://azure.microsoft.com/services/functions/). Daha fazla ayrıntı bulabilirsiniz [mantıksal uygulama](https://azure.microsoft.com/services/logic-apps/) ve [akış](https://flow.microsoft.com/en-us/) Video Indexer için Bağlayıcılar [burada](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/). 

### <a name="in-which-azure-regions-is-video-indexer-available"></a>Video Indexer, hangi Azure bölgelerinde kullanılabilir?

Hangi Azure bölgeleri Video Indexer kullanılabilir gördüğünüz [bölgeleri](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all) sayfası.

### <a name="what-is-the-sla-for-video-indexer"></a>Video Indexer için SLA'sı nedir?

Azure medya Hizmetleri SLA'sı Video Indexer kapsar ve bulunabilir [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_2/) sayfası. SLA yalnızca hesapları Ücretli Video Indexer için geçerlidir ve ücretsiz deneme sürümü için geçerli değildir.

## <a name="privacy-questions"></a>Gizlilik sorularının

### <a name="are-video-and-audio-files-indexed-by-video-indexer-stored"></a>Video ve ses dosyalarını depolanan Video Indexer tarafından dizine eklenir?

Evet, görüntü Video Indexer Web sitesi veya API'sini kullanarak dizin oluşturucudan dosyayı silmeniz sürece, video ve ses dosyalarını depolanır. Ücretsiz deneme, video ve ses dosyaları için dizin Azure Doğu ABD bölgesinde saklanır. Aksi takdirde, video ve ses dosyalarını Azure aboneliğinizin depolama hesabında depolanır.

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>Ben, Video Indexer Portalı'nda depolanan dosyalarımı silebilir miyim?

Evet, video ve ses dosyaları yanı sıra tüm meta veriler ve içgörüler bunları Video Indexer tarafından Ayıklanan her zaman silebilirsiniz. Bir kez bir dosya tarafından sağlanan Video Indexer, dosyayı silin ve sağlanan Video Indexer, meta veriler ve içgörüler kalıcı olarak kaldırılır. Ancak, yedekleme çözümünüzü Azure depolamada uyguladıysanız, dosya, Azure depolama alanında kalır.

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>Video Indexer Hesabımı kullanıcı erişimi denetleyebilir miyim?

Evet, yalnızca hesap yöneticileri davet ve hesaplarını kişilere davetini iptal yanı ayrıcalıkları düzenleme kimlerin ve salt okunur erişimi olanların atayın.

### <a name="who-has-access-to-my-video-and-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Dizin ve/veya ve meta verileri ve ayıklanan ınsights Video Indexer tarafından depolanan my video ve ses dosyalarına kimler erişebilir?

Genel olarak gizlilik ayarına sahip, video veya ses içeriğini, video veya ses içeriğini ve kendi öngörüleri bağlantısını olan herkes tarafından erişilebilir. Özel olarak gizlilik ayarına sahip, video veya ses içeriğini yalnızca video veya ses içeriğini hesabına davet kullanıcılar tarafından erişilebilir. Gizlilik ayarı içeriğinizi meta verileri ve öngörüleri ayıklayan Video Indexer için de geçerlidir. Video veya ses dosyanızı karşıya yüklediğinizde gizlilik ayarı atayın. Ayrıca, dizin oluşturma sonrasında gizlilik ayarı değiştirebilirsiniz.

### <a name="what-access-does-microsoft-have-to-my-video-or-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Hangi erişim dizin ve/veya ve meta verileri ve ayıklanan ınsights Video Indexer tarafından depolanan video veya ses dosyalarımı Microsoft gerekiyor mu?

Başına [Azure çevrimiçi hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (OST) içeriğinizi tamamen size ait ve Microsoft içerik meta verileri ve Video Indexer içeriğinizi OST ve Microsoft göre ayıklar ınsights yalnızca erişir Gizlilik bildirimi.

### <a name="are-the-custom-models-that-i-build-in-my-video-indexer-account-available-to-other-accounts"></a>Geliştiriyorum özel modelleri Video Indexer Hesabımda diğer hesapları için kullanılabilir?

 Hayır, hesabınızda oluşturduğunuz özel modelleri, başka bir hesap için kullanılamaz. Video Indexer şu anda sayesinde özel derleme [markaları](customize-brands-model-overview.md), [dil](customize-language-model-overview.md), ve [kişi](customize-person-model-overview.md) hesabınızdaki modelleri. Bu modeller yalnızca modelleri oluşturduğunuz hesabı kullanılabilir.
  
### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>Video Indexer: Video Indexer burada kullanmakta olduğum Azure bölgesi içinde tutulan tarafından içeriği dizine?

Evet, içerik ve kendi ınsights birden fazla Azure bölgesini kullanan Azure aboneliğinizde bir el ile yapılandırma olmadığı sürece, Azure bölgesi içinde tutulur. 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>Video Indexer gizlilik ilkesi nedir?

Video Indexer tarafından kapsanan [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement). Gizlilik bildirimi Microsoft işler kişisel verileri açıklar, Microsoft, nasıl işlediğini ve Microsoft hangi amaçla işler. Gizlilik hakkında daha fazla bilgi edinmek için [Microsoft Trust Center](https://www.microsoft.com/trustcenter).

### <a name="what-certifications-does-video-indexer-have"></a>Video Indexer hangi sertifikaları var mı?

Video Indexer, şu anda SOC sertifika yok. Video Indexer'ın sertifika gözden geçirmek için lütfen başvurmak [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings?product=Azure).

## <a name="api-questions"></a>API sorular

### <a name="what-apis-does-video-indexer-offer"></a>Video Indexer hangi API'ler sunar?

Video Indexer'ın API'leri dizin oluşturma, ayıklama meta verileri, varlık yönetimi, çeviri, ekleme, özelleştirme modelleri ve daha fazlasını sağlar. Daha ayrıntılı Video Indexer API kullanma hakkında bilgi bulmak için başvurmak [Video Indexer Geliştirici Portalı](https://api-portal.videoindexer.ai/).

### <a name="what-client-sdks-does-video-indexer-offer"></a>Video Indexer hangi istemci SDK'ları sunuluyor?

Şu anda hiçbir istemci SDK'ları sunulan vardır. Video Indexer ekibi, yakında teslim planları ve SDK'ları üzerinde çalışmaktadır.

### <a name="how-do-i-get-started-with-video-indexers-api"></a>Video Indexer'ın API'sini kullanmaya nasıl başlayabilirim?

İzleyin [Öğreticisi: Video dizinleyici API'sini kullanmaya başlama](video-indexer-use-apis.md).

### <a name="what-is-the-difference-between-the-video-indexer-api-and-the-azure-media-service-v3-api"></a>Video Indexer API ve Azure Media Service v3 API arasındaki fark nedir?

Şu anda bazı çakışıyor Video Indexer API ve Azure medya hizmeti v3 API'si tarafından sunulan özellikler vardır. Her iki services karşılaştırması hakkında daha fazla bilgi bulabilirsiniz [burada](compare-video-indexer-with-media-services-presets.md).

### <a name="what-is-an-api-access-token-and-why-do-i-need-it"></a>Bir API erişim belirteci nedir ve neden gerekir?

Video Indexer API, bir yetkilendirme API ve işlemleri API içerir. Yetkilendirmeleri API size çağrıları içeren erişim belirteci. İşlemler API'sine yapılan her çağrı, çağrının yetkilendirme kapsamına uyan bir erişim belirteci ile ilişkilendirilmelidir.

Erişim belirteçleri, Video Indexer API'ları güvenlik amacıyla kullanmak için gereklidir. Bu, tüm çağrıları, ya da hesabınıza erişim izinlerine sahip olanlar geldiğini sağlar. 

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>Hesap erişim belirtecini, kullanıcı erişim belirteci ve Video erişim belirteci arasındaki fark nedir?

* Hesap düzeyi – hesap düzeyinde erişim belirteçleri, hesap düzeyinde veya video düzeyinde işlemleri sağlar. Örneğin, karşıya video yükleme, tüm videoları liste, video öngörüleri alın.
* Kullanıcı düzeyinde - kullanıcı düzeyinde erişim belirteçleri kullanıcı düzeyinde işlemler gerçekleştirmenize olanak tanır. Örnek: ilişkili hesaplar alma.
* Video düzeyindeki – belirli bir video işlemleri video düzeyinde erişim belirteçleri sağlar. Örnek: video içgörüsü alma, açıklamalı alt yazı indirme, pencere öğesi alma vb.

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>Yeni bir erişim belirteci almak ne sıklıkla ihtiyacım var? Ne zaman erişim belirteçleri süresi dolar mı?

Erişim belirteçleri, her saat yeni bir erişim belirteci oluşturmak gereken şekilde her saat sona erer. 

## <a name="billing-questions"></a>Faturalama soruları

### <a name="how-much-does-video-indexer-cost"></a>Video Indexer nin ücreti ne kadardır?

Video Indexer, dizin içerik giriş süresine göre basit Kullandıkça Öde fiyatlandırma modeli kullanır. Ek ücretleri, kodlama, akış, depolama, ağ kullanımı ve medya ayrılmış birimleri uygulanabilir. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) sayfası.

### <a name="when-am-i-billed-for-using-video-indexer"></a>Video Indexer'ı kullanmak için ne zaman faturalandırılırım?

Bir video dizin oluşturma için gönderilirken, dizin oluşturmanın video analizini mi, ses analizi mi yoksa ikisini birden mi içereceği kullanıcı tarafından belirtilir. Bu, hangi SKU’ların ücretlendirileceğini belirler. İşleme sırasında bir kritik düzey hatası oluşursa yanıt olarak bir hata kodu döndürülür. Bu durumda faturalama gerçekleştirilmez.  Kritik hatalar, koddaki bir hatadan veya hizmetin bir iç bağımlılığında oluşan kritik bir hatadan kaynaklanabilir. Yanlış tanımlama veya içgörü ayıklama gibi hatalar kritik olarak kabul edilmez ve bir yanıt döndürülür. Geçerli (hata kodu olmayan) bir yanıt döndürülen durumlarda faturalama gerçekleşir.
 
### <a name="does-video-indexer-offer-a-free-trial"></a>Video Indexer ücretsiz bir deneme sunar?

Evet, Video Indexer tam hizmet ve API işlevselliğini sağlayan ücretsiz bir deneme sunar. Bir kota videoların web tabanlı bir arabirim kullanıcılar için ve 2.400 dakika API kullanıcılar için 600 dakika yoktur. 

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
