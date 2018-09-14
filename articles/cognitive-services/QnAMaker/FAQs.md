---
title: SSS - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmeti için sık sorulan sorular listesi
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: nstulasi
ms.openlocfilehash: 0af93682c4a1be4de4d92e9c44e10586f740d8bf
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576689"
---
# <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>My URL'lerini neden olduğunu / soru-cevap çiftlerini dosyalar ayıklanıyor değil mi?

Soru-cevap Oluşturucu otomatik-geçerli SSS URL'lerinden soru-cevap (soru-cevap) içerikler ayıklama emin olamaz mümkündür. Böyle durumlarda soru-cevap içeriğini bir .txt dosyasına yapıştırma ve aracı, veri alabilen, bkz. Alternatif olarak, Bilgi Bankası'na bilgi bankanızı düzenleyerek içerik ekleyebilirsiniz.

## <a name="how-large-a-knowledge-base-can-i-create"></a>Ne kadar büyük bir bilgi bankası oluşturabilirim?

Soru-cevap Oluşturucu hizmeti oluştururken seçtiğiniz SKU, Azure Search'te Bilgi Bankası boyutuna bağlıdır. Okuma [burada](./Tutorials/choosing-capacity-qnamaker-deployment.md) daha fazla ayrıntı için.

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>Yeni Bilgi Bankası oluşturmaya çalıştığınızda neden her şey için açılan göremiyorum?

Azure'da henüz herhangi bir soru-cevap Oluşturucu hizmeti oluşturmadınız. Okuma [burada](./How-To/set-up-qnamaker-service-azure.md) bunu nasıl yapacağınız.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>Nasıl Bilgi Bankası diğer paylaşırım?

Soru-cevap Oluşturucu hizmetini düzeyinde çalışır paylaşımı, yani tüm bilgi bankalarından hizmetlerinde paylaşılır. Okuma [burada](./How-To/collaborate-knowledge-base.md) nasıl Bilgi Bankası üzerinde işbirliği yapın.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>İyi bir eşleşme bulunduğunda varsayılan ileti nasıl değiştirebilirim?

Varsayılan ileti ayarları, App Service'te bir parçasıdır.
- Go için Azure portalında uygulama hizmeti kaynağınıza

![qnamaker appservice](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Tıklayarak **ayarları** seçeneği

![qnamaker appservice ayarları](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Değiştirin **DefaultAnswer** ayarı
- App service'ı yeniden başlatın

![qnamaker appservice yeniden başlatma](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Neden SharePoint bağlantımı ayıklanan değil mi?

Araç yalnızca ortak URL'leri ayrıştırır ve kimliği doğrulanmış veri kaynakları şu anda desteklemiyor. Alternatif olarak, dosyayı indirin ve soru ve cevapları ayıklayın için dosya karşıya yükleme seçeneğini kullanın.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>My Bilgi Bankası'na yaptığım güncelleştirmeler bankamda. Neden?

Her düzenleme işleminden tablo güncelleştirme, test veya ayarları olmadığını yayımlanabilmesi için önce kaydedilmesi gerekir. Kaydet'e tıklayın ve her düzenleme işleminden sonra düğme eğitme emin olun.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>Uç nokta anahtarlarımı ne zaman yenilemeniz gerekir?

Geçirildiğini, şüpheleniyorsanız uç nokta anahtarlarınızı yenilemeniz gerekir.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Multimedya ve Bilgi Bankası zengin veriler mu?

Bilgi bankası Markdown’ı destekler. Bununla birlikte, URL'lerden otomatik ayıklama HTML Markdown'a dönüştürme becerisi sınırlıdır. Tam özellikli Markdown kullanmak istiyorsanız, içeriğinizi doğrudan Tablo'da değiştirebilir ya da zengin içerikli bir Bilgi Bankası karşıya yükleyin.

Multimedya, resimler ve videolar gibi şu anda desteklenmiyor.

## <a name="does-qna-maker-support-non-english-languages"></a>Soru-cevap Oluşturucu İngilizce dışındaki dilleri destekler mi?

Daha fazla ayrıntı görmek [desteklenen diller](./Overview/languages-supported.md).

Birden çok dil içerik varsa, her dil için ayrı bir hizmet oluşturmak emin olun.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Soru-Cevap Oluşturucu’yu kullanmak için Bot Framework’ü kullanmam mı gerekir?

Hayır, soru-cevap Oluşturucu ile Bot Framework kullanmanız gerekmez. Bununla birlikte, soru-cevap Oluşturucu, Azure robot hizmeti'ndeki çeşitli şablonlardan biri olarak sunulur. Bot hizmeti, Microsoft robot altyapısı aracılığıyla, hızla akıllı robot geliştirme sağlar ve daha az ortam bir sunucu çalıştırır.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>Soru-cevap Oluşturucu ile bot nasıl oluşturabilirim?

Bölümündeki yönergeleri [bu](./Tutorials/create-qna-bot.md) Azure Bot ile Botunuza oluşturulacağını belgeler.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Soru-Cevap Oluşturucu hizmetini web siteme nasıl ekleyebilirim?

Soru-cevap Oluşturucu hizmetini Web sitenize bir web Sohbeti denetimi olarak eklemek için şu adımları izleyin:

1. Yönergeleri takip ederek, SSS Robotu oluşturun [burada](./Tutorials/create-qna-bot.md).
2. Adımları izleyerek web Sohbeti etkinleştirin [burada](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat)


