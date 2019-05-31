---
title: Konuşma Öğrenici teknolojileri - Microsoft Bilişsel hizmetler oluşturmaya diğer bot ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici teknolojileri oluşturan diğer bot ile kullanmayı öğrenin.
services: cognitive-services
author: mattm
manager: larsliden
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 07/13/2018
ms.author: nitinme
ms.openlocfilehash: d6af927e395532e43c7cc51c39665e2e42ac6781
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389969"
---
# <a name="how-to-use-conversation-learner-with-other-bot-building-technologies"></a>Konuşma Öğrenici teknolojileri oluşturan diğer bot ile kullanma

Bu öğretici, konuşma Öğrenici teknolojileri oluşturan diğer bot ile kullanma ve bellek (veya durumu) arasında bu teknolojilerden nasıl paylaşılabileceğini kapsar. 

## <a name="video"></a>Video

[![Karma Botlar öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications_Preview)](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications)

## <a name="requirements"></a>Gereksinimler
Bu öğretici, günlük iletişim kutuları, olmayan günlük iletişim Web kullanıcı arabirimini oluşturmak için robot öykünücüyü gerektirir. Bot Framework Öykünücünün kurulumunu hakkında daha fazla bilgi edinilebilir [burada](https://docs.microsoft.com/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0). 

Bu öğreticide, karma Öğreticisi bot çalışıyor olması gerekir:

    npm run tutorial-hybrid

## <a name="details"></a>Ayrıntılar

Konuşma Öğrenici denetiminde olsa da, tüm durum konuşma Öğrenici oturumu göre konuşma Öğrenici'nın bellek Yöneticisi'nde depolanmış olması gerekir. Machine learning durumu konuşma sürücü nasıl belirlemek için kullanır. Bu özellik gereklidir. Dış durum oturumu başladığında çağrılan OnSessionStartCallback de Konuşma Öğrenici uygulamasına geçirilebilir. Oturum sona erdiğinde iç durumu kullanıma OnSessionEndCallback tarafından döndürülebilir.

Ayrıca, bazı başlangıç durumunu alır ve değerler döndüren bir işlev çağrısı konuşma Öğrenici neredeyse düşünebilirsiniz.

Bu örnekte, iki farklı sistemler kullanan bir karma robotun oluşturacak:
1. Konuşma Öğrenici modeli <br/>
    Konuşma öğrenici modeli geçerli oturum tabanlı robotun sonraki eylemi belirlemek için kullanır. Bot bu bölümü bir ilk durumu parça götürür `isOpen` (gösterir bir deponun açık veya kapalı olup olmadığını) ve başka bir parçasının durumunu döndürür `purchaseItem` (bir öğenin kullanıcı adını satın).

2. Eşleşen metin <br />
    Yalnızca belirli dizeleri için gelen metni inceler ve yanıt verir. Bot bu parçası Botlar yöneten diğer depolama mekanizmaları ve CL oturumu başlatmak için sorumludur. Özellikle bu üç değişkenin yönetir: `usingConversationLearner`, `storeIsOpen`, ve `purchaseItem`.

Bu tanıtımda kullanılan model göz alarak başlayalım.

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi "İçeri aktarma eğitimler" tıklayın ve "Öğreticisi-16-HybridBot" adlı modelini seçin.

## <a name="entities"></a>Varlıklar

Varlıkları sayfasını açın ve iki varlık dikkat edin: `isOpen` ve `purchaseItem`

Bu varlıkları nasıl kullanılacağını anlamak için dosyayı açın: `C:\<installedpath>\src\demos\tutorialHybrid.ts` geri çağırmaları aramak için.

Dikkat kodda `OnSessionStartCallback` değerini kopyalar `storeIsOpen` değeri olarak Botbuilder'da konuşma depolamadan `isOpen` konuşma Öğrenici kullanılabilir olacak şekilde varlık. Aşağıdaki kodu bakın:

![](../media/tutorial17_sessionstart.PNG)

Benzer şekilde, kodda `OnSessionEndCallback` varlık değeri kopyalar (oturum öğrenilen etkinlik ve değil yalnızca bir zaman aşımı nedeniyle sonlandırıldı) `purchaseItem` Botbuilder'da depolamaya giden `purchaseItem`. Aşağıdaki kodu bakın:

![](../media/tutorial17_sessionend.PNG)

Artık eylemlerini gözden geçirelim.

## <a name="actions"></a>Eylemler

Model, dört eylem olduğuna dikkat edin.

Eylemler için hedeflenen kurallar aşağıdaki gibidir:

- Varsa `isOpen` varlık olarak ayarlanmışsa, robot "Ne satın etmek istiyorsunuz?" sorar ve depolamak, `puchaseItem` yuvası.
- Varsa `isOpen` , robot, "Ben kapalı Üzgünüz" olmadığını varsayalım, ayarlı değil.
- Diğer iki eylem türlerinin `END_SESSION`.
- END_SESSION eylemi için ConversationLearner konuşma tamamlandığını gösterir.

### <a name="overall-bot-logic"></a>Genel Bot mantığı

Olması durumunda Bot durumun ilk gördüğünüz `usingConversationLearner` bayrağını ayarlayın, biz denetimi konuşma Öğrenici geçirin. Aksi durumda, biz denetimi başka bir şeye geçirin.  Bu örnekte, biz basit metin eşleşen gösteriyor, ancak bu LUIS, soru-cevap oluşturucu ve konuşma Öğrenici bile başka bir örneği gibi başka bir Bot teknolojisi olabilir.

Bir yol açmaya ihtiyacımız ve yakın bir dize yapmamız için deponun "open store" ve "Kapat mağaza" ile karşılaştırın ve "storeIsOpen" bayrağını ayarlayın.

Ardından, konuşma Öğrenici Modelimizi işleme denetim devretmeyi tetiklemek için bir yol gerekir. Biz, "Atölye" dizesi olarak eşleştirme, aşağıdakileri yapın:
- Ayarlama `usingConversationLearner` Botun bellek bayrağı.
- Konuşma Öğrenici Modelimizi "StartSession" yöntemi çağırın.  Bu "hangi başlatacak onSessionStartCallback" tetikleyecek `isOpen` varlık değeri

Aşağıya bakın:

![](../media/tutorial17_useConversationLearner.PNG)

Ayrıca bir "son bu satın alma öğeyi görüntüler geçmişi" metin eşleşme desteklemiyoruz.
Son olarak, başka bir şey belirtilmişse kullanılabilir kullanıcı komutları gösterilir

## <a name="train-dialog"></a>Train iletişim

Bu öğreticide, zaten önceden eğitilmiş modelidir.  Biz, başlangıç etkisini görmek ve uygulamada oturum geri çağırmaları sonlandırmak için tam bot test eder.

## <a name="testing-the-bot"></a>Bot test etme

Tek konuşma daha yalın modeli botlar yalnızca konuşma Öğrenici modeli tarafından ele alınır gösterebilirsiniz gibi bu konuşma Öğrenici Arabiriminde test etmek mümkün olmayacaktır.

### <a name="install-the-bot-framework-emulator"></a>Bot framework öykünücüsü'nü yükleyin

- [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator) kısmına gidin.
- İndirin ve öykünücüyü yükleyin.

### <a name="configure-the-emulator"></a>Öykünücü yapılandırın

- Öykünücü açın ve URL'yi botunuzun çalıştığı aynı bağlantı noktasını hedeflediği emin olun. Büyük olasılıkla: `http://localhost:3978/api/messages`

### <a name="test"></a>Test etme 

#### <a name="scenario-1-store-is-closed"></a>Senaryo 1: Store kapalı
1. 'Atölye' girin. Bu, eşleşen metni tarafından işlenir ve konuşma Öğrenici modele denetim sağlayacak.
2. 'Hello' girin.  Çünkü `isOpen` değeri ayarlanmazsa, robot "Ben kapalı Üzgünüz" yazar ve kullanıcının oturumu sona erdirin.

#### <a name="scenario-2-store-is-open"></a>Senaryo 2: Store Aç
1. 'Store'u açın' girin.  Bu ayarlar `isOpen` true.
1. 'Atölye' girin.
1. 'Hello' girin.  Çünkü `isOpen` değeri ayarı true, robot "Ne satın etmek istiyorsunuz?" Yazar
1. 'Sandalye' girin. varlık olarak CL belleğe 'sandalye' kaydedilecek `purchaseItem`. Son oturum geri çağırma bu değer konuşma deposuna kopyalayan çağrılır.
1. 'Geçmiş' girin.  Bu son haliyle bot 'Satın aldığınız sandalye' Yazar `purchaseItem`.

## <a name="conclusion"></a>Sonuç

Yukarıda öğrendiklerinizi ile Konuşma Öğrenici teknolojisi herhangi bir Bot ile birleştirmek mümkün olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dallandırma ve geri alma](./17-branch-undo.md)
