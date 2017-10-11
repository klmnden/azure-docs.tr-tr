---
title: "Azure Mobile Engagement tanıtım uygulamasını | Microsoft Docs"
description: "Karşıdan yükleme konumu, nasıl kullanılacağını ve Azure Mobile Engagement tanıtım uygulamasını kullanmanın avantajları açıklanmıştır"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: f624d5aa-254b-4ad0-96a3-f00e6c3a2c97
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2016
ms.author: piyushjo
ms.openlocfilehash: 8381edb569e19a85c1259f7791b477cfa6e51ea3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement tanıtım uygulamasını
Bir Azure Mobile Engagement tanıtım uygulamasını için yayımlanan **iOS**, **Android**, ve **Windows** platformları kullanışlı kaynaklar bulmak ve Mobile Engagement hakkında daha fazla bilgi için Yardım.

Uygulama, yardımcı olur:

* Kolayca videoları, belgeler, Destek Forumu ve özellik istekleri yükseltmek için yapılması gerekenler gibi Mobile Engagement kaynaklarına yararlı bağlantılar bulma.
* Kendi mobil uygulamalar için fikir almak için Mobile Engagement tarafından desteklenen deneyimi örnek bildirimler.
* Mobile Engagement, kendi uygulamanıza uygulamak nasıl incelemek için bir başvuru uygulaması kullanın. İçin bilgi alabilirsiniz:
  
  * Analytics veri toplar.
  * Uygulama bildirimi senaryolarını türleri gibi gelişmiş *tam ekran Interstitial* veya *açılır*.
  * Anketler ve anketler uygular.
  * Sessiz anında veri ve itme senaryolarında uygulayın.   

## <a name="app-installation"></a>Uygulama yükleme
Bu uygulama aşağıdaki uygulama mağazasında edinilebilir:

* **Windows Evrensel tanıtım uygulamasını**:
  
  * Uygulamaya karşıdan [Windows uygulama mağazası](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
  * Uygulamayı Windows 10 Universal bir uygulama olarak geliştirilmiştir. Kaynak kodu kullanılabilir [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).
* **iOS uygulama gösteri**:
  
  * Uygulamaya karşıdan [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
  * Uygulama iOS Swift geliştirilmiştir. Kaynak kodu kullanılabilir [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).
* **Android tanıtım uygulamasını**:
  
  * Uygulamaya karşıdan [Google Play mağazasına](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
  * Kaynak kodu kullanılabilir [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Windows Evrensel tanıtım uygulamasını][1]

![iOS uygulama gösteri][2]
![Android tanıtım uygulamasını][3]

## <a name="usage"></a>Kullanım
Bu uygulama aşağıdaki yollarla kullanabilirsiniz:

**Cihazınızda uygulamayı (daha önce sağlanan) uygulama mağazası bağlantılarından yükleyin:**

> [!IMPORTANT]
> Bir Azure hesabı veya uygulamaya bağlanmak için bir arka uç gerek gerekmez. Uygulama bağımsız olarak çalışır.
> 
> 

* Cihazınızda uygulamayı sonra Mobile Engagement hakkında yararlı kaynakları bulmak için sol taraftaki menüde bağlantılar yoluyla gidebilirsiniz.
* Ekledik [hizmetin RSS akışı](https://aka.ms/azmerssfeed) bu uygulamaya böylece her zaman en son ürün güncelleştirmelerini hakkında güncelleştirilmiş.
* Her platform için Mobile Engagement tarafından desteklenen bildirim türünü yaşamaya bildirim senaryolar üzerinden gidebilirsiniz. Bu bildirimler yerel olarak--olan yaşadı, Mobile Engagement platformundan bildirimleri göndermeyi aynıdır bildirimler deneyimini göstermek için ekranlar çubuğundaki düğmeler'i tıklatın.

![Windows için uygulama menüsü][4]

![İOS için uygulama menüsünden][5]
![Android için uygulama menüsü][6]

**Kaynak kodu (daha önce sağlanan) GitHub bağlantılarından yükleyin:**

* Kaynak kodu indirdikten sonra ilgili geliştirme ortamında--XCode iOS, Android ve Windows için Visual Studio için Android Studio açın.
* Sonraki izlemeniz gereken bizim [Temel SDK tümleştirme adımları](mobile-engagement-windows-store-dotnet-get-started.md) böylece bu uygulamayı kendi Mobile Engagement arka uç örneğine bağlanamıyor.
  * Uygulamada bir bağlantı dizesi yapılandırmanız gerekir.
  * Ayrıca, uygulamanızı anında iletme bildirimi platformu yapılandırmanız gerekir.
* Mobile Engagement ile uygulama izlenmiş olan fark edeceksiniz. Arka uç bağlandıktan sonra uygulamayı açmak gibi bu nedenle, kullanıcı oturumu, etkinlikleri, olayları ve benzeri görmek kullanabileceksiniz **İzleyici** sekmesi.
* Yerel bildirimler kullanmak yerine kendi Mobile Engagement örneğinden bu uygulamaya bildirimleri gönderebilmesi için olacaktır.
  
  * Bir test cihazı kullanarak Cihazınızı buraya ekleyebilirsiniz **cihaz Kimliğinizi alma** menü öğesi için uygulama. Bu platform arka uç örneğinizle ardından bir test cihazı kaydetmek bir cihaz kimliği verir.
    
    ![Windows aygıt kimliği][7]
    
    ![İOS cihaz kimliği][8]
    ![Android cihaz kimliği][9]

## <a name="key-features-of-the-demo-app"></a>Tanıtım uygulamasını temel özellikleri
* Bu uygulama ile daha önce belirtildiği gibi anahtar tüm kaynakları el ile Mobile Engagement için sahiptir. Soldaki menüden bağlantılar aracılığıyla gidebilirsiniz.
* Her platform için uygulama bildirimler yaşayabilirsiniz. Bu bildirimler olarak teslim edilebilir **yalnızca bildirim**, burada bildirim tıklatılması yalnızca yerel bir uygulama ekran yukarı açar (kullanarak **derin bağlama**)--ya da farklı bir **Web duyuru**, bildirim tıklatıldığında görüntülenecek Mobile Engagement arka uçtan ek HTML içeriğini sağlayabileceğiniz burada.
  
    ![App bildirimler][29]
* İOS, uygulama ya da sistem anında iletme bildirimleri görmek için uygulamaya kapatmanız gerekir. Uygulama ekleme burada bakabilir **eylem düğmeleri**, ister için bu uygulama bildirimi eklenen olanları *geri bildirim* ve *paylaşımı* (Kullanıcı bildirim sağ eylemden yararlanabilmeniz).
  
    ![İOS uygulama bildirimler][11] ![İOS uygulama bildirim görüntüleme][14]
* Android, desteklenen seçenekler çok satırlı metin eklemekte olduğunuz (**büyük metin**) veya bir bildirim görüntüsü (**büyük resmi**) için bildirimi ile birlikte **eylem düğmeleri** (iOS tarafından desteklenen gibi).
  
    ![Android uygulama bildirimlerini][12] ![Android uygulama bildirim ekranı][15]
* Windows 10'bildirimleri PC'de nasıl göründüğünü görebilirsiniz. Bu bildirim ayrıca Windows 10 görüntülenir **bildirim Merkezi**. Ekleme desteği yoktur **eylem düğmeleri** Windows SDK'sındaki anda.
  
    ![Windows uygulama bildirimler][10] ![Windows uygulama görüntüleme][13]
* Her platform için varsayılan "uygulama" bildirimleri yaşayabilirsiniz. Bu iki adımlı bir deneyimdir burada bir **bildirim** penceresi ilk görüntülenir. Tıklattığınızda, bir tam ekran yukarı açılır **duyuru**, aşağıdaki ekran görüntüsünde gösterildiği. Bu Duyurunun içeriğini, Mobile Engagement arka uç örneğinden gelir. SDK bildirimler ve duyuruları için şablon yok. Bunları, bizim logo ve renklendirme olan bu tanıtım uygulamasını gösterildiği gibi kolaylıkla özelleştirebilirsiniz.  
  
    ![Windows uygulama bildirimleri][16]
  
    ![İOS uygulama bildirimleri][17]  ![Android'daki uygulama bildirimleri][18]
  
    **iOS**, **Android**
* Aynı zamanda **kategori** Mobile Engagement'ın bu varsayılan deneyimini özelleştirmek özelliğidir. Tanıtım uygulamasını biz bildirimler deneyimini değiştirmek için iki genel yolu gösterilen. Kategori özelliği Windows SDK'ın henüz desteklenmiyor unutmayın.
  
    **Tam ekran Interstitial:**
  
    ![Uygulama içi bildirim - Interstitial kategorisi][30]
  
    ![İOS Interstitial kategorisi][21]     ![Android'de Interstitial kategorisi][22]
  
    **Açılır bildirimi:**
  
    ![Uygulama içi bildirim - açılır kategorisi][31]
  
    ![İOS açılır bildirim][19]    ![Android açılır bildirimi][20]

**iOS**, **Android**

* Mobile Engagement Ayrıca, uygulama içi bildirim olarak adlandırılan uzmanlaşmış bir türü destekler **yoklamalar**. Bu bölümlenmiş uygulama kullanıcılarınıza hızlı anketler göndermenizi sağlar. Sorular ve aşağıdaki ekran görüntüsünde olduğu gibi her bir soru için seçenekleri ekleyebilirsiniz. Bu daha sonra uygulama kullanıcıya bir uygulama bildirimi görüntülendiğini.   
  
    ![Yoklama bildirimleri][32]
  
    ![Windows'da anket][26]
  
    ![İos'ta anket][27]   ![Android'de anket][28]

**iOS**, **Android**

* Mobile Engagement ayrıca destekler sessiz gönderme **veri gönderme** bildirimleri. Bu bildirimler ile (örneğin, JSON verilerini aşağıdaki örnekte) hizmetinizden veri gönderebilir uygulamanızda işlemek ve bazı adımları uygulayın. Nasıl biz öğeyi fiyat seçmeli olarak veri anında iletme bildirimleri kullanarak değiştirirsiniz bir örnektir.
  
    ![Veri anında iletme bildirimi][33]
  
    ![Windows üzerinde veri anında iletme bildirimi][23]
  
    ![İOS veri anında iletme bildirimi][24]  ![Android veri anında iletme bildirimi][25]

**iOS**, **Android**

> [!NOTE]
> Ayrıntılı adım adım yönergeler için bu bildirimler hiçbirini tıklatarak görüntüleyebileceğiniz **Mobile Engagement platformundan bu bildirimleri göndermek yönergeler için buraya tıklayın** herhangi örnek bildirim ekranında.
> 
> 

[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
