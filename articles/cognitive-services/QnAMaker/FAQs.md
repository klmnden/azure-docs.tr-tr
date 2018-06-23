---
title: SSS - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: SSS
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: a6bf32549715d0357771b3f3b0ff72f64788ec20
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354082"
---
# <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>My URL'lerini neden olduğunu / soru-yanıt çiftleri dosyalar ayıklanıyor değil mi?

QnA Maker otomatik-soru ve yanıt (QnA) içerikler geçerli SSS URL'lerden extract olduğunu olamaz mümkündür. Böyle durumlarda, QnA içeriği bir .txt dosyasına yapıştırın ve aracı, işleyebilen, bkz. Alternatif olarak, Bilgi Bankası'na içerik düzenleme şeklinde ekleyebilirsiniz.

## <a name="how-large-a-knowledge-base-can-i-create"></a>Ne kadar büyük bir bilgi bankası oluşturabilirim?

Bilgi Bankası QnA Maker hizmet oluştururken seçtiğiniz SKU, Azure Search'te boyutuna bağlıdır. Okuma [burada](./Tutorials/choosing-capacity-qnamaker-deployment.md) daha fazla ayrıntı için.

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>Yeni Bilgi Bankası oluşturmak çalıştığınızda neden için açılan bir şey görmüyorum?

Azure'da henüz QnA Maker hizmetlerin oluşturmadınız. Okuma [burada](./How-To/set-up-qnamaker-service-azure.md) nasıl.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>Nasıl ı Bilgi Bankası diğer paylaşmak?

QnA Maker hizmet düzeyinde çalışır paylaşımı, yani tüm Bilgi Bankası hizmetlerinde paylaşılır. Okuma [burada](./How-To/collaborate-knowledge-base.md) Bilgi Bankası üzerinde işbirliği yapma.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>İyi bir eşleşme bulunduğunda varsayılan ileti nasıl değiştirebilirim?

Varsayılan ileti uygulama hizmetiniz ayarlarında bir parçasıdır.
- Git App service kaynağınız Azure portalında

![qnamaker uygulama hizmeti](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Tıklayın **ayarları** seçeneği

![qnamaker appservice ayarları](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Değerini değiştirme **DefaultAnswer** ayarı
- Uygulama hizmeti yeniden başlatın

![qnamaker appservice yeniden başlatma](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Neden my SharePoint bağlantı ayıklanan değil mi?

Aracı yalnızca ortak URL'leri ayrıştırır ve kimliği doğrulanmış veri kaynakları şu anda desteklemiyor. Alternatif olarak, dosyayı indirin ve sorular ve yanıtlar ayıklamak için dosya karşıya yükleme seçeneğini kullanın.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>My Bilgi Bankası'na yapılan güncelleştirmeler değil yansıtılır yayımlayın. Neden?

Her düzenleme işlemi tablo güncelleştirme, test veya ayarları olup olmadığını yayımlanmadan önce kaydedilmesi gerekir. Kaydet'i tıklatın ve sonra her düzenleme işlemi düğmesi eğitmek emin olun.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>My uç nokta anahtarları yenilediğinizde?

Geçirildiğini olduğundan şüpheleniyorsanız, uç nokta anahtarlarınızı yenilemeniz gerekir.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Bilgi Bankası destek zengin veri ya da çoklu ortam mu?

Bilgi bankası Markdown’ı destekler. Ancak, URL'lerden otomatik ayıklama HTML Markdown dönüştürme yeteneği sınırlıdır. Tam özellikli Markdown kullanmak istiyorsanız, içeriğinizi doğrudan tablosundaki değiştirmek veya zengin içerikli bir Bilgi Bankası karşıya yükleyin.

Multimedya, görüntüler ve videolar gibi şu anda desteklenmiyor.

## <a name="does-qna-maker-support-non-english-languages"></a>QnA Maker İngilizce dışındaki dilleri destekliyor mu?

Daha fazla ayrıntı görmek [desteklenen diller](./Overview/languages-supported.md).

Birden çok dil içerikten varsa, her dil için ayrı bir hizmet oluşturmak emin olun.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Soru-Cevap Oluşturucu’yu kullanmak için Bot Framework’ü kullanmam mı gerekir?

Hayır, Bot Framework QnA Maker ile kullanmanız gerekmez. Ancak, QnA Maker Azure Bot hizmetinde birkaç şablonlarından birini olarak sunulur. Microsoft Bot çerçevesi aracılığıyla hızlı akıllı bot geliştirme bot hizmetini etkinleştirir ve daha az ortamı sunucu çalıştırır.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>Bir bot QnA Maker ile nasıl oluşturabilirim?

' Ndaki yönergeleri izleyin [bu](./Tutorials/create-qna-bot.md) ile Azure Bot, Bot oluşturmak için belgeleri.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Soru-Cevap Oluşturucu hizmetini web siteme nasıl ekleyebilirim?

QnA Maker hizmeti, Web sitenizdeki bir web Sohbeti denetimi olarak eklemek için aşağıdaki adımları izleyin:

1. Yönergeleri izleyerek SSS bot oluşturma [burada](./Tutorials/create-qna-bot.md).
2. Aşağıdaki adımları izleyerek web Sohbeti etkinleştirmek [burada](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-connect-webchat)


