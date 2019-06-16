---
title: Microsoft Bilişsel hizmetler - konuşma Öğrenici bot dağıtma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici bot dağıtmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 5522f762f3893f1d67cd3755b1e022f0118cc004
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66385311"
---
# <a name="how-to-deploy-a-conversation-learner-bot"></a>Konuşma Öğrenici bot dağıtma

Bu belgede bir konuşma Öğrenici bot--yerel olarak veya azure'a dağıtmak nasıl açıklanmaktadır.

## <a name="prerequisite-determine-the-model-id"></a>Önkoşul: model kimliği belirleme 

Konuşma Öğrenici UI dışında bir bot çalıştırmak için robot kullanacağı--konuşma Öğrenici model kimliği başka bir deyişle, konuşma Öğrenici buluttaki makine öğrenme modeli kimliği ayarlamanız gerekir.  (Aksine, bot konuşma Öğrenici kullanıcı Arabirimi aracılığıyla çalıştırırken, kullanıcı Arabiriminin hangi model kimliği seçer).  

Model kimliği almak nasıl aşağıda verilmiştir:

1. Botunuzun ve konuşma Öğrenici UI'ı başlatın.  Tam yönergeler için Hızlı Başlangıç Kılavuzu'na bakın; özetlersek:

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

2. Tarayıcıyı Aç `http://localhost:5050` 

3. Konuşma Öğrenici model Kimliğini almak istediğiniz tıklayın

4. Sol gezinti çubuğunda "Ayarlar" üzerinde tıklayın.

5. "Model kimliği" GUID, sayfanın üst kısımda görüntülenir.

## <a name="option-1-deploying-a-conversation-learner-bot-to-run-locally"></a>1\. seçenek: Yerel olarak çalıştırmak için bir konuşma Öğrenici bot dağıtma

Bu yerel makinenize bir bot dağıtır ve Bot Framework öykünücüsü'nü kullanarak nasıl erişeceği gösterilmektedir.

### <a name="configure-your-bot-for-access-outside-the-conversation-learner-ui"></a>Botunuzun konuşma Öğrenici kullanıcı Arabirimi dışından erişim için yapılandırma

Bir bot yerel olarak çalışırken, uygulama kimliği botun ekleme `.env` dosyası:

    ```
    CONVERSATION_LEARNER_MODEL_ID=<YOUR_MODEL_ID>
    ```

Ardından, botunuzun başlatın:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

Bot artık yerel olarak çalışıyor.  Bot Framework öykünücü ile erişebilirsiniz.

### <a name="download-and-install-the-emulator"></a>Öykünücüyü yükleyip

    ```
    git clone https://github.com/Microsoft/BotFramework-Emulator
    npm install
    npm run build
    npm start
    ```

### <a name="connect-the-emulator-to-your-bot"></a>Botunuz için öykünücü bağlanma

1. Girin, sol üst öykünücüsü, kutusuna "uç nokta URL'nizi girin" `http://127.0.0.1:3978/api/messages`.  Diğer alanları boş bırakın ve "Bağlan" düğmesine tıklayın.

2. Şimdi, botunuzun ile konuşmaya.

## <a name="option-2-deploy-to-azure"></a>2\. seçenek: Azure’a dağıtma

Konuşma Öğrenici botunuzun benzer şekilde aynı diğer herhangi bir bot yayımlamak istediğiniz yayımlayın. Yüksek bir düzeyde kodunuzu barındırılan bir Web sitesine yükleme, uygun yapılandırma değerleri ayarlayın ve ardından çeşitli kanallarla bot kaydetme. Ayrıntılı yönergeleri kullanarak Azure Bot hizmeti botunuza yayımlama gösteren Bu videoda verilmiştir.

Bot dağıtıldıktan sonra çalıştırdığınız farklı kanallar için Facebook, Teams, Skype vb. gibi bağlanabilirsiniz. bir Azure Bot kanal kaydı kullanarak. İşlem bkz hakkında daha fazla bilgi için: https://docs.microsoft.com/bot-framework/bot-service-quickstart-registration

Konuşma Öğrenici Bot Azure'a dağıtmak için adım adım yönergeler aşağıda verilmiştir.  Bu yönergeler, bot kaynağınızı Azure DevOps Services, GitHub, BitBucket veya OneDrive gibi bulut tabanlı bir kaynak kullanılabilir ve botunuzun sürekli dağıtım için yapılandırıp yapılandırmayacağınız varsayar.

1. Adresinden Azure portalında oturum açın https://portal.azure.com

2. Yeni bir "Web App Botu" Kaynak Oluştur 

    1. Bot, bir ad verin
    2. "Bot şablona"'a tıklayın, "Node.js" seçin, "Temel"'i seçin ve ardından "Seçin" düğmesine tıklayın
    3. "Web App Botu oluşturmak için Oluştur"'i tıklatın.
    4. Oluşturulacak Web App Botu kaynak için bekleyin.

3. Azure portalında yeni oluşturduğunuz Web App Botu kaynak düzenleyin.

   1. Sol taraftaki gezinti öğesi "Uygulama ayarları" tıklayın
   1. "Uygulama ayarları" bölümüne inin
   2. Bu ayarları ekleyin:

       Ortam değişkeni | value
       --- | --- 
       CONVERSATION_LEARNER_SERVICE_URI | "https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/"
       CONVERSATION_LEARNER_MODEL_ID      | Uygulama kimliği GUID, elde edilen modeli için "ayarlar" altında konuşma Öğrenici arabiriminden >
       LUIS_AUTHORING_KEY               | Bu model için anahtar yazma LUIS
       LUIS_SUBSCRIPTION_KEY            | Yazma kotanızı kullanmaktan kaçınmak yayımlanan botlar önerilir ancak gerekli değildir.
    
   4. Sayfanın üst kısımda "Kaydet"'a tıklayın
   5. Sol taraftaki gezinti öğesi "Derleme" açın
   6. "Üzerinde sürekli dağıtımı Yapılandır" tıklayın 
   7. Dağıtımlar altında "Kurulum" simgesine tıklayın
   8. "Ayarları gerekli" tıklayın
   9. Bot kodunuzu nerede kullanılabilir kaynağını seçin ve kaynak yapılandırın.
