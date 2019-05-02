---
title: Bir soru-cevap Oluşturucu Bilgi Bankası'na chit sohbet ekleme
titleSuffix: Azure Cognitive Services
description: Bir KB oluşturduğunuzda botunuz için kişisel chit sohbet ekleyerek daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici kolaylaştırır. Soru-cevap Oluşturucu üst chit-sohbet, önceden doldurulmuş bir dizi, KB kolayca eklemenize olanak sağlar.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: a908185f2d8475afb642250d1499cffa539aca4d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64573504"
---
# <a name="add-chit-chat-to-a-knowledge-base"></a>Bilgi Bankası'na Chit sohbet Ekle

Botunuz için Sohbet chit ekleyerek daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici kolaylaştırır. Soru-cevap Oluşturucu chit sohbet özelliği üst chit-sohbet, önceden doldurulmuş bir dizi, Bilgi Bankası (KB) kolayca eklemenize olanak sağlar. Bu botunuzun ait kişilik için bir başlangıç noktası olabilir ve bu saat ve bunları sıfırdan yazmak maliyeti kaydedecek.  

Bu veri kümesi, Professional, kolay ve Witty gibi çoklu kişilikler sesini chit sohbet yaklaşık 100 senaryoları sahiptir. Botunuzun ait ses en çok benzeyen bir kişi seçin. Kullanıcı sorgusu verildiğinde, soru-cevap Oluşturucu en yakın bilinen chit sohbet soru-cevap ile eşleştirmeye çalışır.  

Farklı inancı bazı örnekleri aşağıda verilmiştir. İnancı ayrıntılarıyla birlikte tüm kişilik veri kümelerini görebilirsiniz [burada](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets).

<!-- added quotes so acrolinx doesn't score these sentences -->
|Kullanıcı sorgusu|Profesyonel|Kolay|Şakacısın|
|--|--|--|--|
|`You are awesome`|`I aim to serve.`|`Aw, I'm blushing.`|`Flattery. I like it.`|
|`Are you hungry?`|`I don't need to eat.`|`I only do food for thought.`|`Eating would require a lot of things I don't have. Like a digestive system. And silverware.`|
|`Sing a song`|`I'm afraid I'm not musically inclined.`|`La la la, tra la la. I'm awesome at this.`|`Those who can, do. Those who can't, don't sing.`|
|`Will you marry me?`|`I think it's best if we stick to a professional relationship.`|`Definitely didn't see that coming!`|`Sure. Take me to city hall. See what happens.`|



> [!NOTE]
> Chit sohbet desteği yalnızca İngilizce dilinde şu anda kullanılabilir. 

## <a name="add-chit-chat-during-kb-creation"></a>KB oluşturma sırasında chit sohbet ekleme
Kaynak URL ve dosyaları ekledikten sonra Bilgi Bankası oluşturma sırasında chit sohbet ekleme seçeneği mevcuttur. Sohbet chit temeliniz olarak kullanmak istediğiniz kişilik seçin. Sohbet chit ekleyip chit sohbet desteği, veri kaynaklarınızı zaten varsa istemiyorsanız **hiçbiri**. 
   
![Oluşturma sırasında chit sohbet Ekle](../media/qnamaker-how-to-chit-chat/create-kb-chit-chat.png)

## <a name="add-chit-chat-to-an-existing-kb"></a>Mevcut bir KB olarak Chit sohbet Ekle
KB'ı seçin ve gitmek **ayarları** sayfası. Bir bağlantı ilgili tüm chit sohbet veri kümelerine **.tsv** biçimi. İstediğiniz kişilik indirin ve ardından dosya kaynağı olarak karşıya yükleyin. İndirin ve dosyayı karşıya yüklemeyi zaman biçimi veya meta veri düzenleme olmadan emin olun. 
  
![Sohbet chit mevcut KB olarak Ekle](../media/qnamaker-how-to-chit-chat/add-chit-chat-dataset.png)

## <a name="edit-your-chit-chat-questions-and-answers"></a>Sohbet chit sorular ve yanıtlar Düzenle
KB düzenlediğinizde chit Sohbeti, seçtiğiniz kişilik üzerinde temel için yeni bir kaynağı görürsünüz. Artık sorular değiştirilmiş ekleyebilir veya yanıtlar, gibi başka bir kaynağı ile düzenleyin. 

![Sohbet chit Bankalarıyla Düzenle](../media/qnamaker-how-to-chit-chat/edit-chit-chat.png)

## <a name="add-additional-chit-chat-questions-and-answers"></a>Ek chit sohbet sorularını ve yanıtlarını Ekle
Değil önceden tanımlanmış ayarlanmış olan yeni chit sohbet soru-cevap ekleyebilirsiniz. Zaten chit sohbet kümesinde kapsamında bir soru-cevap çifti çoğaltma değil emin olun. Tüm yeni chit sohbet soru-cevap'ı eklediğinizde, eklenen, **editoryal** kaynak. Derecelendiricisini anlayan chit sohbet olduğundan emin olmak için meta verileri anahtar/değer çifti Ekle "editoryal: chit sohbet", aşağıdaki görüntüde görebileceğiniz gibi:
   
![Sohbet chit Bankalarıyla Ekle](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png)

## <a name="delete-chit-chat-from-an-existing-kb"></a>Mevcut bir KB chit sohbet Sil
KB'ı seçin ve gitmek **ayarları** sayfası. Belirli chit sohbet kaynağınızı seçili kişilik adlı bir dosya olarak listelenir. Bu kaynak dosyası olarak silebilirsiniz.

![KB chit sohbet Sil](../media/qnamaker-how-to-chit-chat/delete-chit-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası içeri aktarma](../Tutorials/migrate-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
