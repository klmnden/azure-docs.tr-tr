---
title: Node.js - Microsoft Bilişsel hizmetler kullanarak bir konuşma öğrenen uygulaması oluşturma | Microsoft Docs
titleSuffix: Azure
description: Node.js kullanarak bir konuşma öğrenen uygulaması oluşturmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: a3a51aa86a30b060c8dc4113da69462904d7df54
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355085"
---
# <a name="create-a-conversation-learner-application-using-nodejs"></a>Node.js kullanarak bir konuşma öğrenen uygulaması oluşturma

Konuşma öğrenen aracılarını oluşturmanın karmaşıklığını azaltır. Bir karma geliştirme iş elle yazılmış kod ve makine aracılarını yazmak için gerekli kod miktarını azaltmak öğrenme izin vererek akış sağlar. Kullanıcı oturum olmadığını denetleme veya mağaza stoğu denetlemek için bir API isteği yapan gibi uygulamanızın belirli sabit bölümleri hala kodlanmış olmalıdır. Ancak, durumu ve eylem seçimi diğer değişiklikler etki alanı uzman veya geliştirici tarafından verilen örnek iletişim kutuları öğrenilebilecek.

## <a name="invitation-required"></a>Gerekli davet

*Davetiye proje konuşma öğrenen erişmek için gereklidir.*

Proje konuşma öğrenen, bot ve machine learning için SDK erişen bir bulut hizmeti eklemek bir SDK oluşur.  Şu anda bir davet proje konuşma Leaner bulut hizmetine erişim gerektirir.  Zaten davet yapmadıysanız, [davetiye isteği](https://aka.ms/conversation-learner-request-invite).  Bir daveti aldınız değil, API bulut erişemiyor olacaktır.

## <a name="prerequisites"></a>Önkoşullar

- Düğüm 8.5.0 veya üstü ve npm 5.3.0 ya da daha yüksek. Yüklemenize [ https://nodejs.org ](https://nodejs.org).
  
- HALUK geliştirme anahtarı:

  1. İçine oturum [ http://www.luis.ai ](http://www.luis.ai).

  2. Adınızı "ayarlar" üzerinde üst sağ tıklayın

  3. Anahtar yazma elde edilen sayfasında gösterilir

  (2 rolleri anahtar yazma, HALUK işlevi görür.  İlk olarak, bu, konuşma anahtar yazma öğrenen hizmet verecektir.  İkinci olarak, konuşma öğrenen HALUK için varlık ayıklama kullanır; anahtar yazma HALUK HALUK sizin adınıza modelleri oluşturmak için kullanılır)

- Google Chrome web tarayıcısı. Yüklemenize [ https://www.google.com/chrome/index.html ](https://www.google.com/chrome/index.html).

- Git. Yüklemenize [ https://git-scm.com/downloads ](https://git-scm.com/downloads).

- VSCode. Yüklemenize [ https://code.visualstudio.com/ ](https://code.visualstudio.com/). Bu, gerekli değil Not önerilir.

## <a name="quick-start"></a>Hızlı başlangıç 

1. Yükleyin ve yapılandırın:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Sırasında `npm install`, bu durum oluşursa bu hatayı göz ardı edebilirsiniz: `gyp ERR! stack Error: Can't find Python executable`

2. Yapılandırın:

   Adlı bir dosya oluşturun `.env` dizininde `cl-bot-01`.  Dosya içeriğini olmalıdır:

   ```
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Bot başlatın:

    ```
    npm start
    ```

    Bu genel boş bot çalıştırır `cl-bot-01/src/app.ts`.

3. Konuşma öğrenen UI çalıştırın:

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

4. Açık tarayıcı http://localhost:5050 

Konuşma öğrenen kullanmakta olduğunuz ve oluşturabilir ve bir konuşma öğrenen modeli öğretir.  

> [!NOTE]
> Başlatma sırasında proje konuşma öğrenen davet tarafından kullanılabilir.  Varsa http://localhost:5050 bir HTTP gösterir `403` hatası, yani hesabınızı olmayan davet.  Lütfen [davetiye isteği](https://aka.ms/conversation-learner-request-invite).

## <a name="tutorials-demos-and-switching-between-bots"></a>Eğitim, gösterileri ve aracılarını arasında geçiş yapma

Yukarıdaki yönergeleri genel boş bot başlatıldı.  Bir öğretici çalıştırmak veya bot yerine gösteri için:

1. Kullanıcı arabirimini açın konuşma öğrenen web varsa, uygulamaların listesine dönmek http://localhost:5050/home.
    
2. Başka bir bot çalışıyorsa (gibi `npm start` veya `npm run demo-pizza`), uygulamayı durdurun.  UI işlemi durdurun ya da web tarayıcısını kapatın gerekmez.

3. Komut satırından (yukarıdaki adım 2) bir tanıtım bot çalıştırın.  Gösterileri şunları içerir:

  ```bash
  npm run tutorial-general
  npm run tutorial-entity-detection
  npm run tutorial-session-callbacks
  npm run tutorial-api-calls
  npm run demo-password
  npm run demo-pizza
  npm run demo-storage
  npm run demo-vrapp
  ```

4. Zaten değilseniz, konuşma öğrenen web Chrome kullanıcı Arabiriminde yükleyerek geçiş http://localhost:5050/home. 

5. "Alma öğretici"'i tıklatın (yalnızca bir kez gerçekleştirilmesi gerekir).  Bu yaklaşık bir dakika sürer ve konuşma öğrenen modelleri için tüm öğreticileri konuşma öğrenen hesabınızda kopyalayacak.

6. Konuşma öğrenen başlattığınız demo karşılık gelen kullanıcı arabirimini demo modelinde tıklayın.

Gösterileri için kaynak dosyaları `cl-bot-01/src/demos`

## <a name="create-a-bot-which-includes-back-end-code"></a>Arka uç kodu içeren bir bot oluşturma

1. Kullanıcı arabirimini açın konuşma öğrenen web varsa, uygulamaların listesine dönmek http://localhost:5050/home.
    
2. Bir bot çalışıyorsa (gibi `npm run demo-pizza`), uygulamayı durdurun.  UI işlemi durdurun ya da web tarayıcısını kapatın gerekmez.

3. İsterseniz, kodda Düzenle `cl-bot-01/src/app.ts`.

4. Yeniden oluşturma ve bot yeniden başlatın:

    ```bash    
    npm run build
    npm start
    ```

5. Zaten değilseniz, konuşma öğrenen web Chrome kullanıcı Arabiriminde yükleyerek geçiş http://localhost:5050/home. 

6. Kullanıcı Arabiriminde yeni bir konuşma öğrenen uygulaması oluşturma ve öğretme başlatın.

7. Kod değişikliklerini yapmak için `cl-bot-01/src/app.ts`, yukarıdaki adım 2 ' başlayarak adımları yineleyin.

## <a name="vscode"></a>VSCode

VSCode içinde var. çalıştırılır yapılandırmaları her tanıtım ve "boş bot" `cl-bot-01/src/app.ts`.  Açık `cl-bot-01` VSCode klasöründe.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Bir şablonu `.env.example` dosyasını gösterir hangi ortam değişkenleri örnekleri yapılandırmak için ayarlanmış.

Ekleyerek, makinede çalışan diğer hizmetler arasındaki çakışmaları önlemek için bu bağlantı noktaları ayarlayabilirsiniz bir `.env` proje kökündeki dosyasına:

```bash
cp .env.example .env
```

Bu, bot yerel olarak çalıştırın ve konuşma öğrenen kullanmaya başlamak sağlayan standart yapılandırma kullanır.  (Daha sonra bot Bot Framework dağıtmak için bu dosyaya bazı düzenlemelere gerekir.)

## <a name="support"></a>Destek

- Etiket soruları [yığın taşması](https://stackoverflow.com) "microsoft bilişsel" ile
- Üzerinde bir özellik isteği bizim [User Voice sayfası](https://aka.ms/conversation-learner-uservoice)
- Bir sorun açmak bizim [github depo](https://github.com/Microsoft/ConversationLearner-Samples)

## <a name="contributing"></a>Katkıda bulunan

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Daha fazla bilgi için [Kullanım Kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) sayfasına bakın. Başka sorularınız ya da yorumlarınız varsa bunları [opencode@microsoft.com](mailto:opencode@microsoft.com) adresine gönderebilirsiniz.

## <a name="source-repositories"></a>Kaynak havuzları

- [conversationlearner örnekleri](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-SDK'sı](https://github.com/Microsoft/ConversationLearner-SDK)
- [conversationlearner modelleri](https://github.com/Microsoft/ConversationLearner-Models)
- [conversationlearner kullanıcı arabirimi](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner webchat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Merhaba Dünya](./tutorials/1-hello-world.md)
