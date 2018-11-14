---
title: SSS - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmeti için sık sorulan sorular listesi
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/08/2018
ms.author: tulasim
ms.openlocfilehash: 2e4a5d9b7ee2a1a88bcfe819be6540385458108f
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622372"
---
# <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

## <a name="manage-the-knowledge-base"></a>Bilgi Bankası'nı yönetme

### <a name="i-accidentally-deleted-a-part-of-my-qna-maker-what-should-i-do"></a>Yanlışlıkla sildiğim bir parçası my soru-cevap Oluşturucu, neler yapabilirim? 

Kalıcı soru ve cevap çiftlerini, dosyaları, URL'ler, özel sorular ve yanıtlar, bilgi bankalarından veya Azure kaynakları dahil olmak üzere tüm siler. Bilgi Bankası dışarı aktardığınızdan emin olun **ayarları** bilgi bankanızı herhangi bir bölümünü silmeden önce sayfa. 

### <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>My URL'lerini neden olduğunu / soru-cevap çiftlerini dosyalar ayıklanıyor değil mi?

Soru-cevap Oluşturucu otomatik-geçerli SSS URL'lerinden soru-cevap (soru-cevap) içerikler ayıklama emin olamaz mümkündür. Böyle durumlarda soru-cevap içeriğini bir .txt dosyasına yapıştırma ve aracı, veri alabilen, bkz. Alternatif olarak, bilgi bankanızı düzenleyerek içerik bilgi bankanızı ekleyebileceğiniz [soru-cevap Oluşturucu portalı](https://qnamaker.ai).

### <a name="how-large-a-knowledge-base-can-i-create"></a>Ne kadar büyük bir bilgi bankası oluşturabilirim?

Soru-cevap Oluşturucu hizmeti oluştururken seçtiğiniz SKU, Azure Search'te Bilgi Bankası boyutuna bağlıdır. Okuma [burada](./Tutorials/choosing-capacity-qnamaker-deployment.md) daha fazla ayrıntı için.

### <a name="why-cant-i-see-anything-in-the-drop-down-when-i-try-to-create-a-new-knowledge-base"></a>Yeni Bilgi Bankası oluşturmaya çalıştığınızda neden açılan herhangi bir şey göremiyorum?

Azure'da henüz herhangi bir soru-cevap Oluşturucu hizmeti oluşturmadınız. Okuma [burada](./How-To/set-up-qnamaker-service-azure.md) bunun nasıl yapılacağını öğrenin.

### <a name="how-do-i-share-a-knowledge-base-with-others"></a>Nasıl Bilgi Bankası başkalarıyla paylaşırım?

Soru-cevap Oluşturucu hizmetini düzeyinde çalışır paylaşımı, diğer bir deyişle, hizmetteki tüm bilgi bankalarından paylaşılır. Okuma [burada](./How-To/collaborate-knowledge-base.md) nasıl Bilgi Bankası üzerinde işbirliği yapın.

### <a name="can-you-share-a-kb-with-a-contributor-that-is-not-in-the-same-aad-tenant-to-modify-a-kb"></a>Bir KB bir KB değiştirmek için aynı AAD kiracısında değil bir katkıda bulunan ile paylaşabilir miyim? 

Paylaşımı Azure rol tabanlı erişim denetimi (RBAC) bağlıdır. Dilerseniz, _herhangi_ kaynak başka bir kullanıcı ile Azure'da soru-cevap Oluşturucu da paylaşabilirsiniz.

### <a name="if-you-have-an-app-service-plan-with-5-qnamaker-kbs-can-you-assign-readwrite-rights-to-5-different-users-so-each-of-them-can-access-only-1-qnamaker-kb"></a>Bir App Service planı ile 5 QnAMaker KB'leri varsa. Bunların her biri yalnızca 1 QnAMaker KB erişebilmesi için 5 farklı kullanıcılara Okuma/Yazma hakkı atayabilirim miyim?

QnAMaker hizmetin tamamı, bireysel KB'leri paylaşabilirsiniz.

### <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>İyi bir eşleşme bulunduğunda varsayılan ileti nasıl değiştirebilirim?

Varsayılan ileti ayarları, App Service'te bir parçasıdır.
- Azure portalında uygulama hizmeti kaynağınıza gidin

![qnamaker appservice](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Tıklayarak **ayarları** seçeneği

![qnamaker appservice ayarları](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Değiştirin **DefaultAnswer** ayarı
- App service'ı yeniden başlatın

![qnamaker appservice yeniden başlatma](./media/qnamaker-faq/qnamaker-appservice-restart.png)

### <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Neden SharePoint bağlantımı ayıklanan değil mi?

Araç yalnızca ortak URL'leri ayrıştırır ve kimliği doğrulanmış veri kaynakları şu anda desteklemiyor. Alternatif olarak, dosyayı indirin ve soru ve cevapları ayıklayın için dosya karşıya yükleme seçeneğini kullanın.


### <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>My Bilgi Bankası'na yaptığım güncelleştirmeler bankamda. Neden?

Her düzenleme işleminden bir tablo güncelleştirme, test veya ayar olup olmadığını yayımlanabilmesi için önce kaydedilmesi gerekir. Tıkladığınızdan emin olun **kaydedin ve eğitme** her düzenleme işleminden sonra düğme.

### <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Multimedya ve Bilgi Bankası zengin veriler mu?

Bilgi bankası Markdown’ı destekler. Bununla birlikte, URL'lerden otomatik ayıklama HTML Markdown'a dönüştürme becerisi sınırlıdır. Tam özellikli Markdown kullanmak istiyorsanız, içeriğinizi doğrudan Tablo'da değiştirebilir ya da zengin içerikli bir Bilgi Bankası karşıya yükleyin.

Multimedya, resimler ve videolar gibi şu anda desteklenmiyor.

### <a name="does-qna-maker-support-non-english-languages"></a>Soru-cevap Oluşturucu İngilizce dışındaki dilleri destekler mi?

Daha fazla ayrıntı görmek [desteklenen diller](./Overview/languages-supported.md).

Birden çok dil içerik varsa, her dil için ayrı bir hizmet oluşturmak emin olun.

## <a name="manage-service"></a>Hizmeti yönetme

### <a name="when-should-i-restart-my-app-service"></a>App service ne zaman yeniden başlatmanız gerekir? 

Bilgi Bankası'ndaki sürüm değeri yanında uyarı simgesi olduğunda uygulama hizmetinizi yenileyin **uç nokta anahtarları** üzerinde tablo **kullanıcı ayarları** [sayfa](https://www.qnamaker.ai/UserSettings).

### <a name="when-should-i-refresh-my-endpoint-keys"></a>Uç nokta anahtarlarımı ne zaman yenilemeniz gerekir?

Uç nokta anahtarlarınızı geçirildiğini, şüpheleniyorsanız yenileyin.

### <a name="can-i-use-the-same-azure-search-resource-for-kbs-using-multiple-languages"></a>Aynı Azure Search kaynak KB'leri kullanarak birden çok dil için kullanabilir miyim?

Birden çok dil ve birden çok KB kullanmak için her dil için bir soru-cevap Oluşturucu kaynak oluşturmak kullanıcı vardır. Bu dil başına ayrı bir Azure arama hizmetleri oluşturur. Bir tek bir Azure arama hizmeti farklı dilde KB'leri karıştırma sonuçları düşürülmüş ilgi düzeyi neden olur.

## <a name="integrate-with-other-services-including-bots"></a>Botlar gibi diğer hizmetlerle tümleştirme

### <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Soru-Cevap Oluşturucu’yu kullanmak için Bot Framework’ü kullanmam mı gerekir?

Hayır, soru-cevap Oluşturucu ile Bot Framework kullanmanız gerekmez. Bununla birlikte, soru-cevap Oluşturucu, Azure robot hizmeti'ndeki çeşitli şablonlardan biri olarak sunulur. Bot hizmeti, Microsoft robot altyapısı aracılığıyla, hızla akıllı robot geliştirme sağlar ve bir sunucusuz ortamda çalışır.

### <a name="how-can-i-create-a-bot-with-qna-maker"></a>Soru-cevap Oluşturucu ile bot nasıl oluşturabilirim?

Bölümündeki yönergeleri [bu](./Tutorials/create-qna-bot.md) ile Azure Bot hizmeti Botunuza oluşturulacağını belgeler.

### <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Soru-Cevap Oluşturucu hizmetini web siteme nasıl ekleyebilirim?

Soru-cevap Oluşturucu hizmetini Web sitenize bir web Sohbeti denetimi olarak eklemek için şu adımları izleyin:

1. Yönergeleri takip ederek, SSS Robotu oluşturun [burada](./Tutorials/create-qna-bot.md).
2. Adımları izleyerek web Sohbeti etkinleştirin [burada](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat)

## <a name="data-storage"></a>Veri depolama

### <a name="what-data-is-stored-and-where-is-it-stored"></a>Hangi verilerin depolanır ve nerede depolanır? 

Soru-cevap Oluşturucu hizmetinizi oluşturduğunuzda, bir Azure bölgesi seçildi. Bilgi bankaları ve günlük dosyaları, bu bölgede depolanır. 
