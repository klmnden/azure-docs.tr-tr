<properties pageTitle="HTML 5 için Azure Mobil Hizmetler uygulamalarını kullanmaya başlama" metaKeywords="" description="Follow this tutorial to get started using Azure Mobile Services for HTML development. " metaCanonical="" services="" documentationCenter="Mobile" title="Get started with Mobile Services" authors="glenga" solutions="" manager="dwrede" editor="" />

<tags ms.service="mobile-services" ms.workload="mobile" ms.tgt_pltfrm="mobile-html" ms.devlang="javascript" ms.topic="hero-article" ms.date="01/01/1900" ms.author="glenga" />


# <a name="getting-started"></a>Mobil Hizmetleri kullanmaya başlama

[WACOM.INCLUDE [mobil-hizmetler-seçicisi-kullanmaya-başlama](../includes/mobile-services-selector-get-started.md)]

<div class="dev-onpage-video-clear clearfix">
<div class="dev-onpage-left-content">
<p>Bu öğretici, Azure Mobil Hizmetleri kullanarak bir HTML uygulamasına bulut tabanlı arka uç hizmeti eklemeyi gösterir. Bu öğreticide, yeni bir mobil hizmet uygulamasıyla birlikte o yeni mobil hizmet içinde uygulama verilerini depolayan basit bir <em>Yapılacaklar listesi</em> uygulaması oluşturacaksınız. Sağdaki klipe tıklayarak bu öğreticinin video sürümünü izleyebilirsiniz.</p>
</div>
<div class="dev-onpage-video-wrapper"><a href="http://go.microsoft.com/fwlink/?LinkId=287040" target="_blank" class="label">öğreticiyi izle</a> <a style="background-image: url('/media/devcenter/mobile/videos/mobile-html-get-started-180x120.png') !important;" href="http://go.microsoft.com/fwlink/?LinkId=287040" target="_blank" class="dev-onpage-video"><span class="icon">Videoyu Oynat</span></a> <span class="time">3:51</span></div>
</div>
 
Tamamlanmış uygulamaya ait bir ekran görüntüsü aşağıda verilmiştir:

![][0]

Bu öğreticiyi tamamlamak HTML uygulamalarına yönelik diğer tüm Mobil Hizmetler öğreticileri için bir önkoşuldur. 

<div class="dev-callout"><strong>Not</strong> <p>Bu öğreticiyi tamamlamak için bir Azure hesabınız olmalıdır. Hesabınız yoksa sadece birkaç dakikada ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. <a href="http://www.windowsazure.com/tr-tr/pricing/free-trial/?WT.mc_id=A0E0E5C02&returnurl=http%3A%2F%2Fwww.windowsazure.com%2Ftr-tr%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-html%2F" target="_blank">Azure Ücretsiz Deneme Sürümü</a>.</p></div>

### Ek gereksinimler

+ Bu öğretici, yerel bilgisayarınızda aşağıdaki web sunucularından birini çalıştırıyor olmanızı gerektirir:

	+  **Windows'ta**: IIS Express. IIS Express, [Microsoft Web Platformu Yükleyicisi] tarafından yüklenir.   
	+  **MacOS X'te**: Python, önceden yüklenmiş olması gerekir.
	+  **Linux'ta**: Python. [Python'un en son sürümünü] yüklemeniz gerekir. 
	
	Uygulamayı barındırmak için istediğiniz web sunucusunu kullanabilirsiniz, ancak bunlar indirilmiş betikler tarafından desteklenen web sunucularıdır.  

+ HTML5'i destekleyen bir web sunucusu.


## <a name="create-new-service"> </a>Yeni mobil hizmet oluşturma

[WACOM.INCLUDE [mobil-hizmetler-oluşturma-yeni-hizmet](../includes/mobile-services-create-new-service.md)]

## <h2><span class="short-header">Yeni uygulama oluşturma</span>Yeni HTML uygulaması oluşturma</h2>

Mobil hizmetinizi oluşturduktan sonra, yeni bir uygulama oluşturmak veya mevcut uygulamayı mobil hizmetinize bağlanacak şekilde değiştirmek için Yönetim Portalı'ndaki kolay hızlı başlangıcı izleyebilirsiniz. 

Bu bölümde, mobil hizmetinize bağlanan yeni bir HTML uygulaması oluşturacaksınız.

1. Yönetim Portalı'nda, **Mobil Hizmetler**'e tıkladıktan sonra yeni oluşturduğunuz mobil hizmete tıklayın.

   
2. Hızlı başlangıç sekmesinde, **Platform seçin** altından **Windows** 'a tıklayın ve **Yeni HTML uygulaması oluştur**'u genişletin.

   	![][6]

   	Bu işlem, mobil hizmetinize bağlı bir HTML uygulaması oluşturup barındırmak için üç kolay adımı görüntüler.

  	![][7]

3. Uygulama verilerinin depolanacağı tabloyu oluşturmak için **Yapılacaklar Öğeleri tablosu oluştur** 'a tıklayın.

4. **Uygulamanızı indirin ve çalıştırın** altından **İndir**'e tıklayın. 

  	Bu işlem, mobil hizmetinize bağlanan örnek _Yapılacaklar listesi_ uygulamasının Web sitesi dosyalarını indirir. Sıkıştırılmış dosyayı yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

5. **Yapılandır** sekmesinde, **Cross-Origin Resource Sharing (CORS)** altında `localhost` seçeneğinin **Ana bilgisayar adlarından isteklere izin ver** listesinde zaten listelendiğini doğrulayın. Listelenmiyorsa **Ana bilgisayar adı** alanında `localhost` yazdıktan sonra **Kaydet**'e tıklayın.

  	![][9]

	<div class="dev-callout"><b>Not</b>
		<p>Hızlı başlangıç uygulamasını localhost dışında bir web sunucusuna dağıtırsanız o web sunucusunun ana bilgisayar adını <strong>Ana bilgisayar adlarından isteklere izin ver</strong> listesine eklemeniz gerekir. Daha fazla bilgi için bkz. <a href="http://msdn.microsoft.com/tr-tr/library/windowsazure/dn155871.aspx" target="_blank">Kaynaklar arası kaynak paylaşımı</a>.</p>
	</div>

## HTML uygulamanızı barındırma ve çalıştırma

Bu öğreticinin son aşaması yeni uygulamanızı yerel bilgisayarınızda barındırıp çalıştırmaktır.

1. Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma gidin, bilgisayarınızdaki dosyaları genişletin ve **server** alt klasöründen aşağıdaki komut dosyalarından birini başlatın.

	+ **launch-windows** (Windows bilgisayarları) 
	+ **launch-mac.command** (Mac OS X bilgisayarları)
	+ **launch-linux.sh** (Linux bilgisayarları)

	<div class="dev-callout"><b>Not</b>
		<p>Windows bilgisayarda, PowerShell bu betiği çalıştırmak istediğinizi onaylamanızı istediğinde `R` yazın. Web tarayıcınız, İnternet'ten indirildiği için bu betiği çalıştırmamanız konusunda sizi uyarabilir. Bu olursa tarayıcının betiği yüklemeye devam etmesini istemelisiniz.</p>
	</div>

	Bu işlem, yerel bilgisayarınızda yeni uygulamayı barındıracak bir web sunucusu başlatır.

2. Uygulamayı başlatmak için bir web tarayıcısında <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> URL'sini açın.

3. Uygulamada, **Yeni görev girin** alanında _Öğreticiyi tamamla_ gibi anlamlı bir metin yazın ve sonra **Ekle**'ye tıklayın.

   	![][10]

   	Bu işlem, Azure'de barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekteki veriler Yapılacaklar Öğeleri tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve bu veriler uygulamada ikinci sütunda görüntülenir.

	<div class="dev-callout"> 
	<b>Not</b> 
   	<p>Veri sorgulamak ve eklemek üzere mobil hizmetinize erişen ve app.js dosyasında bulunan kodu inceleyebilirsiniz.</p> 
 	</div>

4. Yönetim Portalı'na geri dönüp **Veri** sekmesine tıkladıktan sonra **Yapılacaklar Öğeleri** tablosuna tıklayın.

   	![][11]

   	Bunu yapmak uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.

   	![][12]

## <a name="next-steps"> </a>Sonraki Adımlar
Hızlı başlangıcı artık tamamladığınıza göre, Mobil Hizmetler'deki diğer önemli görevleri gerçekleştirmeyi öğrenin: 

* **[Verileri kullanmaya başlama]**
  <br/>Mobil Hizmetleri kullanarak veri depolama ve sorgulama hakkında daha fazla bilgi edinin.
  
* **[HTML uygulamasından özel API çağırma]**
  <br/>HTML uygulamanızı Mobil Hizmetler'de barındırılan özel bir API'ye bağlayın.

* **[Kimlik doğrulama kullanmaya başlama]**
  <br/>Uygulama kullanıcılarınızın kimliğini bir kimlik sağlayıcısı ile doğrulamayı öğrenin.

* **[Mobil Hizmetler HTML/JavaScript Kavramsal Kullanım Başvurusu]**
  <br/>Mobil Hizmetleri HTML/JavaScript ile kullanma hakkında daha fazla bilgi edinin 

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-html-get-started/mobile-quickstart-completed-html.png





[6]: ./media/mobile-services-html-get-started/mobile-portal-quickstart-html.png
[7]: ./media/mobile-services-html-get-started/mobile-quickstart-steps-html.png

[9]: ./media/mobile-services-html-get-started/mobile-services-set-cors-localhost.png
[10]: ./media/mobile-services-html-get-started/mobile-quickstart-startup-html.png
[11]: ./media/mobile-services-html-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-html-get-started/mobile-data-browse.png


<!-- URLs. -->
[Verileri kullanmaya başlama]: /tr-tr/develop/mobile/tutorials/get-started-with-data-html
[Kimlik doğrulama kullanmaya başlama]: /tr-tr/develop/mobile/tutorials/get-started-with-users-html
[Mobil Hizmetler HTML/JavaScript Kavramsal Kullanım Başvurusu]: /tr-tr/documentation/articles/mobile-services-html-call-custom-api 

[Management Portal]: https://manage.windowsazure.com/
[Microsoft Web Platform Installer]:  http://go.microsoft.com/fwlink/p/?LinkId=286333
[latest version of Python]: http://go.microsoft.com/fwlink/p/?LinkId=286342
[HTML uygulamasından özel API çağırma]: /tr-tr/develop/mobile/how-to-guides/work-with-html-js-client
[Cross-origin resource sharing]: http://msdn.microsoft.com/tr-tr/library/windowsazure/dn155871.aspx
