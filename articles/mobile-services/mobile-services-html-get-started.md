<properties
    pageTitle="HTML/JavaScript uygulamaları için Azure Mobile Services’ı Kullanmaya Başlama | Microsoft Azure"
    description="HTML geliştirme için Azure Mobile Services’ı kullanmaya başlamak için bu öğreticiden yararlanın."
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html5"
    ms.devlang="javascript"
    ms.topic="get-started-article" 
    ms.date="07/21/2016"
    ms.author="glenga"/>


# <a name="getting-started"> </a>Mobile Services’ı kullanmaya başlama

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

##Genel Bakış 

Bu öğreticide, bir HTML uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir *Yapılacaklar listesi* uygulaması oluşturacaksınız. Aşağıda bu öğreticinin video sürümünü görüntüleyebilirsiniz. 

> [AZURE.VIDEO mobile-get-started-html]
 
Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![][0]

Bu öğreticiyi tamamlamak HTML uygulamalarına ilişkin tüm Mobile Services öğreticileri için ön koşuldur. PhoneGap/Cordova uygulaması için, bu öğreticinin [PhoneGap/Cordova sürüm](mobile-services-javascript-backend-phonegap-get-started.md)üne bakın.

##Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

+ Yerel bilgisayarınızda aşağıdaki web sunucularından biri çalışıyor olmalıdır:

    +  **Windows**: IIS Express. IIS Express [Microsoft Web Platformu Yükleyicisi] tarafından yüklenir.
    +  **MacOS x**: Python, önceden yüklenmiş olması gerekir.
    +  **Linux**: Python. [Python’un en son sürümünü] yüklemelisiniz.

    Uygulamayı barındırmak için herhangi bir web sunucusunu kullanabilirsiniz, ancak bunlar indirilmiş betikler tarafından desteklenen web sunucularıdır.  

+ HTML5'i destekleyen bir web tarayıcısı.
+ Bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-html%2F"%20target="_blank). 


## <a name="create-new-service"> </a>Yeni bir mobil hizmet oluşturma

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni HTML uygulaması oluşturma

Mobil hizmetinizi oluşturduktan sonra yeni bir uygulama oluşturmak veya mevcut bir uygulamayı mobil hizmetinize bağlamak üzere değiştirmek için Klasik Azure Portalı’ndaki kolay bir hızlı başlangıcı izleyebilirsiniz.

Bu bölümde, mobil hizmetinize bağlanan yeni bir HTML uygulaması oluşturacaksınız.

1.  [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.


2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Windows**’a tıklayın ve **Yeni HTML uygulaması oluştur** seçeneğini genişletin.

    ![][6]

    Burada, mobil hizmetinize bağlanan bir HTML uygulaması barındırmanın üç kolay adımı gösterilmiştir.

    ![][7]

3. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItems tablosu oluştur**’a tıklayın.

4. **Uygulamanızı indirme ve çalıştırma** altında **İndir**’e tıklayın.

    Bu, mobil hizmetinize bağlanan örnek için _Yapılacaklar listesi_ uygulamasın için web sitesi dosyalarını indirir. Sıkıştırılmış dosyayı yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

5. **Yapılandır** sekmesinde `localhost` öğesinin **Çıkış noktaları arası kaynak paylaşma (CORS)** altındaki **Ana bilgisayar adlarından gelen isteklere izin ver** listesinde zaten olduğunu doğrulayın. Listede yoksa, **ana bilgisayar adı** alanına `localhost` yazın ve ardından **Kaydet**’e tıklayın.

    ![][9]

    > [AZURE.IMPORTANT] Hız başlangıç uygulamasını localhost dışında bir web sunucusuna dağıtırsanız, web sunucusunun adını **Ana bilgisayar adlarından gelen isteklere izin ver** listesine eklemelisiniz. Daha fazla bilgi için bkz. [Çıkış noktaları arası kaynak paylaşma](http://msdn.microsoft.com/library/windowsazure/dn155871.aspx).

## HTML uygulamanızı barındırma ve çalıştırma

Bu öğreticinin son aşaması yerel bilgisayarda yeni uygulamanızı barındırmak ve çalıştırmaktır.

1. Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma gidin, bilgisayarınızda dosyaları genişletin ve **server** alt klasöründeki aşağıdaki betiklerden birini başlatın.

    + **launch-windows** (Windows bilgisayarlar)
    + **launch-mac.command** (Mac OS X bilgisayarlar)
    + **launch-linux.sh** (Linux bilgisayarlar)

    > [AZURE.NOTE] Bir Windows bilgisayarda, PowerShell sizden betiği çalıştırmayı istediğinizi onaylamanızı istediğinde `R` yazın. Web tarayıcınız, İnternet'ten indirildiği için betiği çalıştırmamanız konusunda sizi uyarabilir. Bu durumda, tarayıcının betiği yüklemeye devam etmesini istemelisiniz.

    Bu, yerel bilgisayarınızda yeni uygulamayı barındıracak bir web sunucusu başlatır.

2. Uygulamayı başlatmak için bir web tarayıcısında <a href="http://localhost:8000/" target="_blank">http://localhost:8000 /</a> URL’sini açın.

3. Uygulamada, **Yeni görev gir** bölümüne _Öğreticiyi tamamla_ gibi anlamlı bir metin yazın ve ardından**Ekle**’ye tıklayın.

    ![][10]

    Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler uygulamada ikinci sütunda görüntülenir.

    > [AZURE.NOTE] Sorgulamak ve page.js dosyasında bulunan verileri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.

4. [Klasik Azure Portalı] geri dönün, **Veri** sekmesine ve sonra **TodoItems** tablosuna tıklayın.

    ![][11]

    Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.

    ![][12]

## <a name="next-steps"> </a>Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* **[Uygulamanıza kimlik doğrulaması ekleme]**  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* **[Mobile Services HTML/JavaScript Kavramsal Başvurusu Nasıl Yapılır?]**  
  Mobile Services’ı HTML/JavaScript ile kullanma hakkında daha fazla bilgi edinin


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Mobil hizmet örneği tanımlama]:#define-mobile-service-instance
[Sonraki Adımlar]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-html-get-started/mobile-quickstart-completed-html.png

[6]: ./media/mobile-services-html-get-started/mobile-portal-quickstart-html.png
[7]: ./media/mobile-services-html-get-started/mobile-quickstart-steps-html.png

[9]: ./media/mobile-services-html-get-started/mobile-services-set-cors-localhost.png
[10]: ./media/mobile-services-html-get-started/mobile-quickstart-startup-html.png
[11]: ./media/mobile-services-html-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-html-get-started/mobile-data-browse.png


<!-- URLs. -->
[Uygulamanıza kimlik doğrulaması ekleme]: mobile-services-html-get-started-users.md

[Klasik Azure Portalı]: https://manage.windowsazure.com/
[Microsoft Web Platformu Yükleyicisi]:  http://go.microsoft.com/fwlink/p/?LinkId=286333
[Python’un en son sürümünü]: http://go.microsoft.com/fwlink/p/?LinkId=286342
[Mobile Services HTML/JavaScript Kavramsal Başvurusu Nasıl Yapılır?]: mobile-services-html-how-to-use-client-library.md
[Çıkış noktaları arası kaynak paylaşma]: http://msdn.microsoft.com/library/azure/dn155871.aspx
 



<!--HONumber=Aug16_HO1-->


