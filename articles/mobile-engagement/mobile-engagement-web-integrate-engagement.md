---
title: "Azure Mobile Engagement Web SDK tümleştirmesi | Microsoft Docs"
description: "En son güncelleştirmeleri ve Azure Mobile Engagement Web SDK'sı için yordamlar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 7d8eaa180e277741a583522ee62d68f5247b92bb
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Bir web uygulamasına Azure Mobile Engagement tümleştirme
> [!div class="op_single_selector"]
> * [Windows Evrensel](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Bu makaledeki yordamlar analizi ve izleme işlevleri Azure Mobile Engagement web uygulamanızda etkinleştirmek için en basit yolu açıklanmaktadır.

Kullanıcılar, oturumlar, etkinlikleri, kilitlenme ve technicals ilgili tüm İstatistikler işlem için gereken günlüğü raporları etkinleştirme adımlarını izleyin. Olaylar, hatalar ve işleri gibi uygulama bağımlı istatistikler için günlüğü raporları el ile Azure Mobile Engagement API'sini kullanarak etkinleştirmeniz gerekir. Daha fazla bilgi için bilgi [bir web uygulamasında API etiketleme Gelişmiş Mobile Engagement kullanmayı](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Giriş
[Azure Mobile Engagement Web SDK Yükle](http://aka.ms/P7b453).
Mobile Engagement Web SDK tek bir JavaScript dosyası sevk site veya web uygulamanızın her sayfasına eklemek olan azure-engagement.js.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırmadan önce uygulamanız için Mobile Engagement yapılandırmak için yazma bir komut dosyası veya kod parçacığı çalıştırmanız gerekir.
> 
> 

## <a name="browser-compatibility"></a>Tarayıcı uyumluluğu
Mobile Engagement Web SDK'sı, kodlama ve etki alanları arası AJAX istekleri (W3C CORS belirtimi olan) yanı sıra kod çözme yerel JSON kullanır. Aşağıdaki tarayıcılarla uyumludur:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Mobil katılım yapılandırın
Bir genel oluşturan bir komut dosyası yazma `azureEngagement` aşağıdaki örnekteki gibi JavaScript nesne. Sitenizi katları sayfaları olabileceğinden, bu örnek, bu komut dosyası her sayfada dahil olduğunu varsayar. Bu örnekte, JavaScript nesne adlı `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

`connectionString` Uygulamanızı Azure portalında görüntülenen için bir değer.

> [!NOTE]
> `appVersionName`ve `appVersionCode` isteğe bağlıdır. Ancak, böylece analytics sürüm bilgileri işleyebilmesi için bunları yapılandırmanızı öneririz.
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Mobile Engagement betikleri sayfalarınızda içerir
Mobile Engagement betikleri sayfalarınıza aşağıdaki yollardan biriyle ekleyin:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Veya bu:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Diğer ad
Mobile Engagement Web SDK komut dosyası yüklendikten sonra oluşturduğu **katılım** SDK API'leri erişmek için diğer ad. SDK yapılandırmasını tanımlamak için bu diğer ad kullanılamıyor. Bu diğer adı, bu belgede bir başvuru olarak kullanılır.

Varsayılan diğer ad sayfanızdan başka bir genel değişkeni ile çakışırsa, Mobile Engagement Web SDK'sı yükleme önce bu yapılandırmada şu şekilde tanımlayabilirsiniz olduğunu unutmayın:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Temel raporlama
Temel Mobile Engagement ' raporlama, kullanıcılar, oturumlar, etkinlikler ile çökmeler ilgili istatistikler gibi oturum düzeyi istatistikleri ele alınmaktadır.

### <a name="session-tracking"></a>Oturum izleme
Mobile Engagement oturum etkinlikler dizisini bölünür, her bir ad tarafından tanımlanır.

Klasik Web sitesinde, sitenizin her sayfada farklı bir etkinlik bildirmek öneririz. Hangi asla geçerli sayfa değişiklikleri bir Web sitesi veya web uygulaması için etkinliklerini izlemek page içinde daha küçük bir ölçekte gibi isteyebilirsiniz.

Başlat veya geçerli kullanıcı etkinliği değiştirmek için her iki durumda da çağrı `engagement.agent.startActivity` işlevi. Örneğin:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Mobile Engagement sunucunun bir açık oturum üç uygulama sayfası kapatıldıktan sonra dakika içinde otomatik olarak sona erer.

Alternatif olarak, bir oturumu el ile çağırarak sonlandırabilir `engagement.agent.endActivity`. Bu geçerli bir kullanıcı etkinliği 'Boşta.' için ayarlar  Oturum, sürece 10 saniye sonra sona erecek için yeni bir çağrı `engagement.agent.startActivity` oturum sürdürür.

10 saniye arasında bir gecikme genel katılım nesnesinde aşağıdaki gibi yapılandırabilirsiniz:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [!NOTE]
> Kullanamazsınız `engagement.agent.endActivity` içinde `onunload` geri çağırma çünkü bu aşamada AJAX çağrıları yapamazsınız.
> 
> 

## <a name="advanced-reporting"></a>Gelişmiş raporlama
İsteğe bağlı olarak, uygulamaya özgü olaylar, hatalar ve işleri rapor istiyorsanız, Mobile Engagement API'sini kullanmanız gerekir. Mobile Engagement API aracılığıyla erişim `engagement.agent` nesnesi.

Tüm mobil katılım API'sindeki Mobile Engagement'ın gelişmiş özelliklerinden erişebilirsiniz. API makalesinde ayrıntılı [bir web uygulamasında API etiketleme Gelişmiş Mobile Engagement kullanmayı](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>AJAX çağrıları için kullanılan URL'leri özelleştirme
Mobile Engagement Web SDK'sı URL'leri özelleştirebilirsiniz. Örneğin, günlük URL'si (SDK uç noktası için günlük) tanımlanacak yapılandırma bu gibi geçersiz kılabilirsiniz:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

URL işlevlerinizi ile başlayan bir dize döndürecek varsa `/`, `//`, `http://`, veya `https://`, varsayılan düzenini kullanılmaz. Varsayılan olarak, `https://` düzeni, bu URL'ler için kullanılır. Varsayılan şema özelleştirmek istiyorsanız, bu gibi yapılandırma geçersiz kıl:

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
