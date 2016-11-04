---
title: Windows Mağazası JavaScript uygulamaları için Mobile Services kullanmaya başlama | Microsoft Docs
description: JavaScript’te Windows Mağazası geliştirme için Azure Mobile Services’ı kullanmaya başlamak üzere bu öğreticiden yararlanın.
services: mobile-services
documentationcenter: windows
author: ggailey777
manager: erikre
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: javascript
ms.topic: get-started-article
ms.date: 07/21/2016
ms.author: glenga

---
# Mobile Services’ı kullanmaya başlama
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Azure Mobile Apps’te bir Windows uygulaması oluşturma](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).  
> 
> 

Bu öğreticide bir Windows Mağazası JavaScript uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir *Yapılacaklar listesi* uygulaması oluşturacaksınız.  Oluşturacağınız mobil hizmet, sunucu tarafı iş mantığı için JavaScript kullanır. 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F).
* [Windows için Visual Studio 2013 Express]

## Yeni bir mobil hizmet oluşturma
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni bir Windows mağazası uygulaması oluşturma
Mobil hizmetinizi oluşturduktan sonra mobil hizmetinize bağlanan yeni bir Windows Mağazası 8.1 JavaScript uygulaması oluşturmak için klasik Azure portalındaki kolay bir hızlı başlangıcı izleyebilirsiniz.

1. [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.
2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Windows**’a tıklayın ve **Yeni Windows Mağazası uygulaması oluştur** seçeneğini genişletin.
3. Henüz yapmadıysanız, yerel bilgisayarınıza veya sanal makinenize [Visual Studio 2013][Windows için Visual Studio 2013 Express]’i indirin ve yükleyin.
4. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItem tablosu oluştur**’a tıklayın.
5. **Uygulamanızı indirme ve çalıştırma** altında uygulamanız için bir dil seçin ve sonra **İndir**’e tıklayın.
   
    Bu, mobil hizmetinize bağlanan örnek için *Yapılacaklar listesi* uygulaması için projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Windows uygulamanızı çalıştırma
Bu öğreticinin son aşaması yeni uygulamanızı oluşturmak ve çalıştırmaktır.

1. Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma göz atın, bilgisayarınızdaki dosyaları genişletin ve Visual Studio’da çözüm dosyasını açın.
2. Projeyi oluşturmak ve uygulamayı başlatmak için **F5** tuşuna basın.
3. Uygulamada **TodoItem Ekle** bölümüne *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından**Kaydet**’e tıklayın.
   
    Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler uygulamada ikinci sütunda görüntülenir.
4. (İsteğe bağlı) Uygulamayı yeniden çalıştırın ve önceki adımda kaydedilen verilerin uygulama başladıktan sonra mobil hizmetinden yüklendiğine dikkat edin.
5. [Klasik Azure Portalı] geri dönün, **Veri** sekmesine ve sonra **TodoItems** tablosuna tıklayın.
   
    Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.

> [!NOTE]
> Sorgulamak ve default.js dosyasında bulunan verileri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.
> 
> 

## Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre [HTML/JavaScript için Mobile Services istemcisi](mobile-services-html-how-to-use-client-library.md) ile çalışmayı öğrenebilirsiniz. 

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Mobil hizmet örneği tanımlama]:#define-mobile-service-instance
[Sonraki Adımlar]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Windows için Visual Studio 2013 Express]: http://go.microsoft.com/fwlink/?LinkId=257546
[Mobile Services SDK’sı]: http://go.microsoft.com/fwlink/?LinkId=257545
[Klasik Azure Portalı]: https://manage.windowsazure.com/



<!--HONumber=Sep16_HO3-->


