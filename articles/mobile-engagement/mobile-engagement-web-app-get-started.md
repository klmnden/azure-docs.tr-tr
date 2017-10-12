---
title: "Web Apps için Azure Mobile Engagement kullanmaya başlama | Microsoft Belgeleri"
description: "Web Apps için analizler ve anında iletme bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04afe53a-4caf-4c80-bd75-20cc630cd75c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: hero-article
ms.date: 06/01/2016
ms.author: piyushjo
ms.openlocfilehash: abcb04e4e0a3ae4fdba3a4ded20b3846ac3b21e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Web Apps için Azure Mobile Engagement kullanmaya başlama
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda Web Uygulaması kullanımınızı anlamak için Azure Mobile Engagement’ın nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
> Azure Mobile Engagement hizmeti, Mart 2018’de devre dışı bırakılacaktır. Şu anda yalnızca mevcut müşteriler tarafından kullanılabilmektedir. Daha fazla bilgi için bkz. [Mobile Engagement nedir?](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Bu öğretici için aşağıdakiler gereklidir:

* Visual Studio 2015 veya tercih ettiğiniz başka bir düzenleyici
* [Web SDK](http://aka.ms/P7b453)

Bu Web SDK Önizleme modundadır ve şu anda yalnızca Analizi destekleyip, tarayıcı ya da herhangi bir uygulama içi bildirim göndermeyi henüz desteklememektedir. 

> [!NOTE]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a>Web uygulamanız için Mobile Engagement ayarlama
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama
Bu öğreticide, veri toplamak için gereken en küçük grup olan bir "temel tümleştirme" gösterilmektedir.

Tümleştirmeyi göstermek üzere Visual Studio ile temel bir web uygulaması oluşturulacaktır; ancak Visual Studio’nun dışında oluşturulmuş herhangi bir web uygulamasının adımlarını da izleyebilirsiniz. 

### <a name="create-a-new-web-app"></a>Yeni bir Web Uygulaması oluşturma
Aşağıdaki adımlarda Visual Studio 2015 kullanıldığı varsayılmaktadır ancak Visual Studio'nun önceki sürümleri için de benzer adımlar izlenir. 

1. Visual Studio'yu başlatın ve **Giriş** ekranında **Yeni Proje**’yi seçin.
2. Açılır listeden **Web** -> **ASP.Net Web Uygulaması**’nı seçin. Uygulamanın **Ad**, **Konum** ve **Çözüm adı** alanını doldurun ve ardından **Tamam**’a tıklayın.
3. **Şablon seçin** açılır penceresinde **ASP.Net 4.5 Şablonları** altında **Boş**’u seçin ve **Tamam**’a tıklayın. 

Azure Mobile Engagement Web SDK’sını tümleştireceğimiz yeni ve boş bir Windows Web Uygulaması projesi oluşturmuş oldunuz.

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama
1. Çözümünüzde **javascript** adlı yeni bir klasör oluşturun ve **azure-engagement.js** adlı Web SDK JS dosyasını buna ekleyin. 
2. Aşağıdaki kodla birlikte bu javascript klasörüne **main.js** adlı yeni bir dosya ekleyin. Bağlantı dizesini güncelleştirdiğinizden emin olun. Bu `azureEngagement` nesnesi Web SDK yöntemlerine erişmek için kullanılır. 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Js dosyaları ile Visual Studio][1]

## <a name="enable-real-time-monitoring"></a>Gerçek zamanlı izlemeyi etkinleştirme
Veri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir Etkinlik göndermelisiniz. Web uygulaması bağlamında etkinlik bir web sayfasıdır. 

1. Çözümünüzde **home.html** adlı yeni bir sayfa oluşturun ve web uygulamanızın başlangıç sayfası olarak ayarlayın. 
2. Gövde etiketine aşağıdakileri ekleyerek bu sayfada daha önce eklediğimiz iki javascript’i dahil edin. 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. Gövde etiketini EngagementAgent `startActivity` yöntemini çağıracak şekilde güncelleştirin
   
        <body onload="engagement.agent.startActivity('Home')">
4. **home.html** dosyanız bu şekilde görünür
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a>Analizi genişletme
Analiz için şu anda Web SDK ile birlikte kullanabileceğiniz tüm yöntemler aşağıda verilmiştir:

1. Etkinlikler/Web sayfaları:
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. Olaylar
   
        engagement.agent.sendEvent(name, extras);
3. Hatalar
   
        engagement.agent.sendError(name, extras);
4. İşler
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

