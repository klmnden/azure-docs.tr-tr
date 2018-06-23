---
title: Konuşma öğrenen bot - Microsoft Bilişsel hizmetler dağıtma | Microsoft Docs
titleSuffix: Azure
description: Konuşma öğrenen bot dağıtmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 77cc998227d996a6e52b1b5629204da5dc735ede
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353962"
---
# <a name="how-to-deploy-a-conversation-learner-bot"></a>Konuşma öğrenen bot dağıtma

Bu belge, yerel olarak ya da Azure içine bir konuşma öğrenen bot--dağıtmayı açıklar.

## <a name="prerequisite-determine-the-application-id"></a>Önkoşul: uygulama kimliği belirleme 

Konuşma öğrenen UI dışında bir bot çalıştırmak için bot kullanacağı--konuşma öğrenen uygulama kimliği başka bir deyişle, makine öğrenimi modeline konuşma öğrenen bulutta kimliği ayarlamanız gerekir.  (Buna karşın bot konuşma öğrenen kullanıcı Arabirimi aracılığıyla çalıştırırken, kullanıcı arabirimini hangi uygulama kimliği seçer).  

Uygulama Kimliğini almak nasıl şöyledir:

1. Bot ve konuşma öğrenen UI başlatın.  Tam yönergeler için Hızlı Başlangıç Kılavuzu'na bakın; özetlemek için:

    Bir komut penceresinde:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

    Sahiplenilmiş komut penceresinde

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

2. Açık tarayıcı http://localhost:5050 

3. Konuşma öğrenen uygulama Kimliğini almak istediğiniz tıklayın

4. Sol gezinti çubuğunda "ayarlar"'ı tıklatın.

5. "Uygulama Kimliğinizi" GUID sayfanın üstünde görüntülenir.

## <a name="option-1-deploying-a-conversation-learner-bot-to-run-locally"></a>Seçenek 1: yerel olarak çalıştırmak için bir konuşma öğrenen bot dağıtma

Bu yerel makinenize bir bot dağıtır ve Bot Framework öykünücüsü kullanarak erişim nasıl gösterir.

### <a name="configure-your-bot-for-access-outside-the-conversation-learner-ui"></a>Bot konuşma öğrenen UI dışına erişim için yapılandırma

Bir bot yerel olarak çalışırken, uygulama kimliği bot için 's eklemek `.env` dosyası:

    ```
    CONVERSATION_LEARNER_APP_ID=<YOUR_APP_ID>
    ```

Sonra bot başlatın:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

Bot şimdi yerel olarak çalışıyor.  Bot Framework öykünücü ile erişebilirsiniz.

### <a name="download-and-install-the-emulator"></a>Öykünücü yükleyip

    ```
    git clone https://github.com/Microsoft/BotFramework-Emulator
    npm install
    npm run build
    npm start
    ```

### <a name="connect-the-emulator-to-your-bot"></a>Öykünücü, bot Bağlan

1. Öykünücü üst sol, kutusuna "uç nokta URL'nizi" girin, `http://127.0.0.1:3978/api/messages`.  Diğer alanları boş bırakın ve "Bağlan" düğmesine tıklayın.

2. Şimdi, bot ile konuşmaya.

## <a name="option-2-deploy-to-azure"></a>Seçenek 2: Azure'a dağıtma

Benzer diğer bir bot yayımlamak istediğiniz aynı şekilde, konuşma öğrenen bot yayımlayın. Yüksek bir düzeyde kodunuzu barındırılan bir Web sitesine yüklemek, uygun yapılandırma değerlerini ayarlayın ve ardından bot çeşitli kanallar ile kaydedin. Ayrıntılı yönergeler Azure Bot hizmetini kullanarak, bot yayımlama gösteren Bu videoda içindir.

Bot dağıtıldıktan sonra çalıştırdığınız farklı kanallar için bir Azure Bot kanal kaydı kullanarak Facebook, takımları, Skype vb. gibi bağlanabilir. İşlem bkz belgeler için: https://docs.microsoft.com/en-us/bot-framework/bot-service-quickstart-registration

Aşağıda bir konuşma öğrenen Bot Azure'a dağıtmak için adım adım yönergeler verilmiştir.  Bu yönergeleri bot kaynağınız VSTS, GitHub, BitBucket veya OneDrive gibi bulut tabanlı bir kaynak kullanılabilir değil ve sürekli dağıtımı için bot yapılandıracak varsayılır.

1. Azure portalında oturum açın https://portal.azure.com

2. Yeni bir "Web uygulaması Bot" kaynağı oluşturun 

    1. Bot bir ad verin
    2. "Bot şablona", "Node.js" seçin, "Temel" seçin ve ardından "Seç" düğmesini tıklatın
    3. Web uygulaması Bot oluşturmak için "Oluştur"'i tıklatın.
    4. Web uygulaması Bot kaynak oluşturulması bekleyin.

3. Azure portalında oluşturduğunuz Web uygulaması Bot kaynağını düzenleyin.

    1. "Uygulama ayarları" nav öğesi sol tıklayın
    1. "Uygulama ayarları" bölümüne gidin
    2. Bu ayarları ekleyin:

        Ortam değişkeni | değer
        --- | --- 
        CONVERSATION_LEARNER_SERVICE_URI | "https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/"
        CONVERSATION_LEARNER_APP_ID      | Uygulama kimliği GUID, uygulama için "ayarlar" altında konuşma öğrenen UI öğesinden elde >
        LUIS_AUTHORING_KEY               | Bu uygulama için anahtar yazma HALUK
    
    4. Sayfanın üstünde "Kaydet"'i tıklatın
    5. Sol taraftaki Aç "Oluştur" nav öğesi
    6. "Yapılandırma sürekli dağıtımı"'i tıklatın 
    7. Dağıtımları altında "Kurulum" simgesine tıklayın
    8. "Ayarlarını gerekli"'i tıklatın
    9. Bot kodunuzu kullanılabilir olduğu bir kaynak seçin ve kaynak yapılandırın.
