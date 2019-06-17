---
title: Node.js - Microsoft Bilişsel hizmetler kullanarak bir konuşma Öğrenici model oluşturma | Microsoft Docs
titleSuffix: Azure
description: Node.js kullanarak bir konuşma Öğrenici modeli oluşturmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: cc071d59a387c8ae4982eacbce6812526f447788
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66388776"
---
# <a name="create-a-conversation-learner-model-using-nodejs"></a>Node.js kullanarak bir konuşma Öğrenici model oluşturma

Konuşma Öğrenici botlar oluşturma karmaşıklığını azaltır. Ancak, bir karma geliştirme iş elle yazılmış kod ve makine öğrenimi botlar yazmak için gereken kod miktarını azaltmak izin verme akışı etkinleştirir. Kullanıcı oturum, denetimi veya bir API isteği deposuna stok kontrolü yapmak gibi modelinizi sabit bazı kısımlarını yine de kodlanmış olabilir. Ancak, durum ve eylem seçimi diğer değişiklikler etki alanı uzmanı veya geliştirici tarafından verilen örnek iletişim kutuları'ndan öğrendiniz.

## <a name="invitation-required"></a>Davet gerekli

*Davetiye proje konuşma Öğrenici erişmek için gereklidir.*

Botunuzun ve machine learning için SDK'sı erişir bir bulut hizmetine eklediğiniz bir SDK proje konuşma Öğrenici oluşur.  Şu anda davet proje konuşma daha yalın bulut hizmetine erişim gerektirir.  Zaten davet yapmadıysanız, [davet isteği](https://aka.ms/conversation-learner-request-invite).  Davetiye almadıysanız, bulut API'sine erişmek mümkün olmayacaktır.

## <a name="prerequisites"></a>Önkoşullar

- Düğüm 8.5.0 veya üzeri ve npm 5.3.0 veya üzeri. Yüklersiniz [ https://nodejs.org ](https://nodejs.org).
  
- LUIS yazarlık anahtarı:

  1. Oturum [ https://www.luis.ai ](https://www.luis.ai).

  2. Sağ üst köşedeki, ardından "ayarlar" şirket adınızı tıklayın

  3. Anahtar yazma elde edilen sayfada gösterilir

  (2 rolü anahtar yazma, LUIS işlevi görür.  İlk olarak, konuşma Öğrenici yazma anahtarı hizmet verecektir.  İkinci olarak, konuşma Öğrenici LUIS varlık ayıklama için kullanır. anahtar yazma LUIS LUIS, sizin adınıza modeller oluşturmak için kullanılır)

- Google Chrome web tarayıcısı. Yüklersiniz [ https://www.google.com/chrome/index.html ](https://www.google.com/chrome/index.html).

- Git. Yüklersiniz [ https://git-scm.com/downloads ](https://git-scm.com/downloads).

- VSCode. Yüklersiniz [ https://code.visualstudio.com/ ](https://code.visualstudio.com/). Bu önerilir, gerekli unutmayın.

## <a name="quick-start"></a>Hızlı başlangıç 

1. Yükleyin ve yapılandırın:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Sırasında `npm install`, bu durum oluşursa bu hatayı yoksayabilirsiniz: `gyp ERR! stack Error: Can't find Python executable`

2. Yapılandırın:

   Adlı bir dosya oluşturun `.env` dizinde `cl-bot-01`.  Dosyanın içeriğini olmalıdır:

   ```
   NODE_ENV=development
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Bot başlatın:

    ```
    npm start
    ```

    Bu genel boş bot çalıştırır `cl-bot-01/src/app.ts`.

3. Tarayıcıyı Aç `http://localhost:3978`

Konuşma Öğrenici kullanmakta olduğunuz ve oluşturabilir ve konuşma Öğrenici modeli öğretin.  

> [!NOTE]
> Başlatma sırasında proje konuşma Öğrenici davetle kullanılabilir.  Varsa `http://localhost:3978/ui` HTTP gösterir `403` hata, yani hesabınız yok davet etti.  Lütfen [davet isteği](https://aka.ms/conversation-learner-request-invite).

## <a name="tutorials-demos-and-switching-between-bots"></a>Öğreticiler, tanıtımlar ve botlar arasında geçiş yapma

Yukarıdaki yönergeleri genel boş bot başlatıldı.  Bir öğretici çalıştırma veya bunun yerine bot gösteri için:

1. Konuşma Öğrenici web kullanıcı arabirimini açın varsa, model listesine dönmek `http://localhost:3978/ui/home`.
    
2. Başka bir bot çalışıyorsa (gibi `npm start` veya `npm run demo-pizza`), durdurun.  UI işlemi durdurun ya da web tarayıcısını kapatın gerekmez.

3. Bir tanıtım bot (yukarıdaki adım 2) komut satırından çalıştırın.  Tanıtımlar şunlardır:

   ```bash
   npm run tutorial-general
   npm run tutorial-entity-detection
   npm run tutorial-session-callbacks
   npm run tutorial-api-calls
   npm run tutorial-hybrid
   npm run demo-password
   npm run demo-pizza
   npm run demo-storage
   ```

4. Zaten değilseniz, konuşma Öğrenici Web Arabirimine chrome'da yükleyerek geçiş `http://localhost:3978/ui/home`. 

5. "İçeri aktarma eğitimler" tıklayın ve konuşma Öğrenici başlattığınız tanıtım için karşılık gelen kullanıcı arabiriminde tanıtım modeli seçin.

Tanıtımları için kaynak dosyaları `cl-bot-01/src/demos`

## <a name="create-a-bot-which-includes-back-end-code"></a>Arka uç kodu içeren bir bot oluşturun

1. Konuşma Öğrenici web kullanıcı arabirimini açın varsa, model listesine dönmek `http://localhost:3978/ui/home`.
    
2. Bir bot çalışıyorsa (gibi `npm run demo-pizza`), durdurun.  UI işlemi durdurun ya da web tarayıcısını kapatın gerekmez.

3. İsterseniz, kodu düzenleme `cl-bot-01/src/app.ts`.

4. Yeniden oluşturun ve bot yeniden başlatın:

    ```bash    
    npm run build
    npm start
    ```

5. Zaten değilseniz, konuşma Öğrenici Web Arabirimine chrome'da yükleyerek geçiş `http://localhost:3978/ui/home`. 

6. Yeni bir konuşma Öğrenici modeli oluşturma kullanıcı Arabiriminde ve öğretim başlatın.

7. Kod değişikliklerini yapmak `cl-bot-01/src/app.ts`, yukarıdaki adım 2 ' başlayarak adımları yineleyin.

## <a name="vscode"></a>VSCode

VSCode içinde var. çalıştırılır "boş bot" yanı sıra, her bir tanıtım için yapılandırmaları `cl-bot-01/src/app.ts`.  Açık `cl-bot-01` VSCode klasöründe.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Bir şablon `.env.example` dosyasını gösterir hangi ortamı değişkenlerini örnekleri yapılandırmak için ayarlanmış.

Ekleyerek, makine üzerinde çalışan diğer hizmetler arasında çakışmaları önlemek için bu bağlantı noktaları ayarlayabilirsiniz bir `.env` projesinin kök dosya:

```bash
cp .env.example .env
```

Bu, botunuzun yerel olarak çalıştırın ve konuşma Öğrenici kullanmaya başlamasını sağlayan standart yapılandırmayı kullanır.  (Daha sonra botunuzun Bot Framework'ü dağıtmak için bu dosyaya yönelik bazı düzenlemeler gerekli olacaktır.)

## <a name="support"></a>Destek

- Etiket sorular [Stack Overflow](https://stackoverflow.com) "microsoft bilişsel" ile
- Bir özellik istemek bizim [User Voice sayfası](https://aka.ms/conversation-learner-uservoice)
- Bir sorun açın bizim [GitHub deposu](https://github.com/Microsoft/ConversationLearner-Samples)

## <a name="contributing"></a>Katkıda bulunan

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Daha fazla bilgi için [Kullanım Kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) sayfasına bakın. Başka sorularınız ya da yorumlarınız varsa bunları [opencode@microsoft.com](mailto:opencode@microsoft.com) adresine gönderebilirsiniz.

## <a name="source-repositories"></a>Kaynak havuzları

- [conversationlearner örnekleri](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-sdk](https://github.com/Microsoft/ConversationLearner-SDK)
- [conversationlearner modelleri](https://github.com/Microsoft/ConversationLearner-Models)
- [conversationlearner kullanıcı arabirimi](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner webchat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Merhaba Dünya](./tutorials/01-hello-world.md)
