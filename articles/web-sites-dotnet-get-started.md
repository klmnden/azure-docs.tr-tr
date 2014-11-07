<properties pageTitle="ASP.NET için Azure Web sitelerini kullanmaya başlama " metaKeywords="" description="This tutorial shows you how to create an ASP.NET web project in Visual Studio 2013 and deploy it to an Azure Website. In less than 15 minutes you'll have an app up and running in the cloud." metaCanonical="" services="web-sites" documentationCenter=".NET" title="Get started with Azure Websites and ASP.NET" authors="tdykstra"  solutions="" manager="wpickett" editor="mollybos"  />

<tags ms.service="web-sites" ms.workload="web" ms.tgt_pltfrm="na" ms.devlang="dotnet" ms.topic="hero-article" ms.date="09/24/2014" ms.author="tdykstra" />

# Azure Web sitelerini ve ASP.NET'i kullanmaya başlama

Bu öğretici, ASP.NET web uygulaması oluşturmayı ve Visual Studio 2013 veya Web Express için Visual Studio 2013 kullanarak onu bir Azure Web sitesine dağıtmayı gösterir. Bu öğreticide, daha önce Azure veya ASP.NET'i kullanma deneyiminiz olmadığı varsayılmıştır. Bu öğreticiyi tamamladığınızda, bulut üzerinde çalışır durumda basit bir web uygulamanız olacak.

Azure hesabını ücretsiz olarak açabilirsiniz ve önceden bir Visual Studio 2013'ünüz yoksa SDK otomatik olarak Web Express için Visual Studio 2013'ü yükler. Böylece Azure için geliştirme yapmaya tamamen ücretsiz olarak başlayabilirsiniz.
 
Şunları öğreneceksiniz:

* Azure SDK'yı yükleyerek makinenizi Azure geliştirmesi için etkinleştirme.
* Visual Studio ASP.NET web projesi oluşturma ve onu Azure Web sitesine dağıtma.
* Web projesinde değişiklik yapma ve uygulamayı yeniden dağıtma.
* Azure Yönetim Portalı'nı kullanarak web sitenizi izleme ve yönetme.

Aşağıdaki şekilde, tamamlanmış uygulama gösterilmiştir:

![Web sitesi giriş sayfası](./media/web-sites-dotnet-get-started-vs2013/deployedandazure.png)

<div class="wa-note">
  <span class="wa-icon-bulb"></span>
  <h5><a name="note"></a>Bu öğreticiyi tamamlamak için bir Azure hesabınız olması gereklidir:</h5>
  <ul>
    <li><a href="/tr-tr/pricing/free-trial/?WT.mc_id=A261C142F">Azure hesabını ücretsiz olarak açabilirsiniz</a> - Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alırsınız ve bunları bitirdikten sonra bile hesabınızı koruyabilir ve Web siteleri gibi ücretsiz Azure hizmetlerini kullanabilirsiniz.</li>
    <li><a href="/tr-tr/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F">MSDN abonesi avantajlarını etkinleştirebilirsiniz</a> - MSDN aboneliğiniz size her ay ücretli Azure hizmetleri için kullanabileceğiniz krediler kazandırır.</li>
  <ul>
</div>
 
### Öğretici bölümleri

* [Geliştirme ortamını kurma](#set-up-the-development-environment)
* [Visual Studio'da ASP.NET web uygulaması oluşturma](#create-an-aspnet-web-application)
* [Uygulamayı Azure'e dağıtma](#deploy-the-application-to-azure)
* [Değişiklik yapma ve yeniden dağıtma](#make-a-change-and-redeploy)
* [Siteyi yönetim portalında izleme ve yönetme](#monitor-and-manage-the-site-in-the-management-portal)
* [Sonraki adımlar](#next-steps)

[WACOM.INCLUDE [yükleme-sdk-2013-sadece](../includes/install-sdk-2013-only.md)]


## ASP.NET web uygulaması oluşturma

İlk adımınız bir web uygulaması projesi oluşturmaktır. Visual Studio, daha sonra projenizi dağıtacağınız Azure Web sitesini otomatik olarak oluşturur. Aşağıdaki diyagramda bu iki aşamada neler olduğu gösterilmiştir.

![Proje oluşturmayı ve dağıtım aşamalarını gösteren diyagram](./media/web-sites-dotnet-get-started-vs2013/createdeploydiagram.png)

1. Visual Studio 2013'ü veya Web için Visual Studio 2013 Express'i açın.

2. **Dosya** menüsünden, **Yeni Proje**'ye tıklayın.

3. **Yeni Proje** iletişim kutusunda, **C#** > **Web** > **ASP.NET Web Uygulaması**'na tıklayın. Dilerseniz **Visual Basic**'i seçebilirsiniz.

3. Hedef Framework olarak **.NET Framework 4.5** 'in seçildiğinden emin olun.

4. Uygulamaya **MyExample** adını verin ve **Tamam**'a tıklayın.

	![New Project dialog box](./media/web-sites-dotnet-get-started-vs2013/GS13newprojdb.png)

5. **Yeni ASP.NET Projesi** iletişim kutusunda, **MVC** şablonunu seçin. ASP.NET Web Forms ile çalışmayı tercih ederseniz **Web Forms** şablonunu seçebilirsiniz. 

	[MVC ve Web Forms](http://www.asp.net/get-started/websites) web siteleri geliştirmeye yönelik ASP.NET çerçeveleridir. Bu öğreticide bunlardan birini seçebilirsiniz, ancak Web Forms'u seçerseniz daha sonra öğretici sizden *Index.cshtml* dosyasını düzenlemenizi istediğinizde *Default.aspx* dosyasını düzenlemeniz gerekir.

7. **Kimlik Doğrulamayı Değiştir**'e tıklayın. 

	![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started-vs2013/GS13changeauth.png)

6. **Kimlik Doğrulamayı Değiştir** iletişim kutusunda, **Kimlik Doğrulama Yok**'a ve **Tamam**'a tıklayın.

	![No Authentication](./media/web-sites-dotnet-get-started-vs2013/GS13noauth.png)

	Oluşturduğunuz bu örnek uygulama kullanıcıların oturum açmasını sağlamaz. [Sonraki Adımlar](#next-steps) bölümü kimlik doğrulamayı ve yetkilendirmeyi uygulayan bir öğreticiye bağlantı içerir.

5. **Yeni ASP.NET Projesi** iletişim kutusunda, **Azure** altındaki ayarları olduğu gibi bırakın ve sonra **Tamam**'a tıklayın. 

	![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started-vs2013/GS13newaspnetprojdb.png)

	Varsayılan ayarlar Visual Studio'nun web projeniz için bir Azure Web sitesi oluşturacağını belirtir. Öğreticinin sonraki bölümünde bu web projesini yeni oluşturulan web sitesine dağıtacaksınız.

	(Onay işaretinin yazısı **Bulutta ana bilgisayar** veya **Uzak kaynaklar oluştur** olabilir. Her ikisi de aynı etkiye sahiptir.)
	
5. Azure'de henüz oturum açmadıysanız, Visual Studio bunu yapmanızı ister. **Oturum Aç**'a tıklayın.

	![Sign in to Azure](./media/web-sites-dotnet-get-started-vs2013/signin.png)

6. **Azure'de oturum aç** iletişim kutusunda, Azure aboneliğinizi yönetmek için kullandığınız hesabın kimlik bilgilerini ve parolasını girin.
	
	Oturum açtığınızda, **Azure Site Ayarlarını Yapılandır** iletişim kutusu hangi kaynakları oluşturmak istediğinizi sorar.

	![Signed in to Azure](./media/web-sites-dotnet-get-started-vs2013/configuresitesettings.png)

3. Visual Studio varsayılan bir **Site adı** sağlar, Azure bunu uygulama URL'niz için ön ek olarak kullanır. Dilerseniz farklı bir site adı girin.

	URL'nin tamamı, buraya girdikleriniz artı *.azurewebsites.net* ibaresinden oluşur(**Site adı** metin kutusunun yanında gösterildiği gibi). Örneğin, site adı `MyExample6442` ise URL `MyExample6442.azurewebsites.net` olur. URL benzersiz olmalıdır. Başka birisi sizin girdiğinizi daha önce kullanmışsa sağda yeşil onay işareti yerine kırmızı bir ünlem görürsünüz ve farklı bir site adı girmeniz gerekir.

4. **Bölge** açılan listesinde, size en yakın olan konumu seçin.

	Bu ayar, web sitenizin hangi Azure veri merkezinde çalışacağını belirtir. Bu öğretici için herhangi bir bölgeyi seçebilirsiniz, gözle görülen bir fark yaratmayacaktır, ancak bir üretim sitesinde [gecikmeyi](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090) en aza indirmek için web sunucunuzun sitenize erişen tarayıcılara olabildiğince yakın olmasını istersiniz.

5. Veritabanı alanlarını olduğu gibi bırakın.

	Bu öğreticide veritabanı kullanmayacaksınız. Öğreticinin sonundaki [Sonraki Adımlar](#next-steps) bölümü veritabanı kullanmayı gösteren bir öğreticiye bağlantı içerir.

6. **Tamam**'a tıklayın.

	Visual Studio birkaç saniyede web projesini belirttiğiniz klasörde ve web sitesini de belirttiğiniz Azure bölgesinde oluşturur.  

	**Çözüm Gezgini** penceresi yeni projedeki dosya ve klasörleri gösterir.

	![Solution Explorer](./media/web-sites-dotnet-get-started-vs2013/solutionexplorer.png)

	**Web Yayımlama Etkinliği** penceresi sitenin oluşturulduğunu gösterir.

	![Web site created](./media/web-sites-dotnet-get-started-vs2013/GS13sitecreated1.png)

	Ayrıca siteyi Sunucu Gezgini'nde görebilirsiniz.

	![Web site created](./media/web-sites-dotnet-get-started-vs2013/siteinse.png)



## Uygulamayı Azure'e dağıtma

7. **Web Yayımlama Etkinliği** penceresinde, **MyExample uygulamasını şimdi bu siteye yayımla**'ya tıklayın.

	![Web site created](./media/web-sites-dotnet-get-started-vs2013/GS13sitecreated.png)

	Birkaç saniye içinde **Web'i Yayımla** sihirbazı görünür. 

	Visual Studio'nun projenizi Azure'e dağıtması için gereken ayarlar *yayım profili* içine kaydedilmiştir. Sihirbaz bu ayarları inceleyip değiştirmenize olanak tanır.

8. Visual Studio'nun web projesini dağıtmak amacıyla Azure'e bağlanabildiğinden emin olmak için **Web'i Yayımla** sihirbazının **Bağlantı** sekmesinde **Bağlantıyı Doğrula** 'ya tıklayın.

	![Validate connection](./media/web-sites-dotnet-get-started-vs2013/GS13ValidateConnection.png)

	Bağlantı doğrulandığında, **Bağlantıyı Doğrula** düğmesinin yanında yeşil bir onay işareti gösterilir. 

9. **İleri**'ye tıklayın.

	![Successfully validated connection](./media/web-sites-dotnet-get-started-vs2013/GS13ValidateConnectionSuccess.png)

10. **Ayarlar** sekmesinde, **İleri**'ye tıklayın.

	![Settings tab](./media/web-sites-dotnet-get-started-vs2013/GS13SettingsTab.png)

	**Yapılandırma** ve **Dosya Yayımlama Seçenekleri** için varsayılan değerleri kabul edebilirsiniz.

	**Yapılandırma** açılır listesi uzaktan hata ayıklama için bir Hata ayıklama yapısı dağıtmanızı sağlar. [Sonraki Adımlar](#next-steps) bölümü Visual Studio'nun hata ayıklama modunda uzaktan nasıl çalıştırılacağını gösteren bir öğreticiye bağlantı içerir.

	**Dosya Yayımlama Seçenekleri** 'ni genişletirseniz bu öğretici için geçerli olmayan senaryoları uygulamanıza olanak veren çeşitli ayarları görürsünüz:
 
	* Hedefteki ek dosyaları kaldır.
	  
		Sunucudaki projenizde olmayan tüm dosyaları siler. Daha önce farklı bir projeyi dağıttığınız bir siteye proje dağıtıyorsanız buna ihtiyacınız olabilir.

	* Yayımlama sırasında ön derleme yap. 
	 
		Büyük siteler için ilk istek ısınma sürelerini düşürebilir.

	* App_Data klasöründeki dosyaları çıkar. 
	 
		Test amacıyla App_Data'da bazen bir SQL Server veritabanı dosyanız olur ve bunu üretime dağıtmak istemezsiniz.
	
11. **Önizleme** sekmesinde, **Önizlemeyi Başlat**'a tıklayın.

	![StartPreview button in the Preview tab](./media/web-sites-dotnet-get-started-vs2013/GS13Preview.png)

	Bu sekme sunucuya kopyalanacak dosyaların listesini görüntüler. Uygulamayı yayımlamak için önizlemenin görüntülenmesi zorunlu değildir ancak bilinmesi gereken yararlı bir işlevdir.

12. **Yayımla**'ya tıklayın.

	![StartPreview file output](./media/web-sites-dotnet-get-started-vs2013/GS13previewoutput.png)

	Visual Studio dosyaları Azure sunucusuna kopyalama işlemini başlatır.

	**Çıkış** ve **Web Yayımlama Etkinliği** pencereleri hangi dağıtım işlemlerinin yapıldığını gösterir ve dağıtımın başarıyla tamamlandığını raporlar.

	![Output window reporting successful deployment](./media/web-sites-dotnet-get-started-vs2013/PublishOutput.png)

	Dağıtım başarılı olduğunda, varsayılan tarayıcı otomatik olarak dağıtılmış web sitesinin URL'sine açılır ve
	oluşturmuş olduğunuz uygulama artık bulutta çalışmaktadır. Tarayıcı adres çubuğundaki URL, sitenin İnternet'ten yüklendiğini gösterir.

	![Web site running in Azure](./media/web-sites-dotnet-get-started-vs2013/GS13deployedsite.png)

13. Tarayıcıyı kapatın.

## Değişiklik yapma ve yeniden dağıtma

Öğreticinin bu bölümünde, giriş sayfasının **h1** başlığını değiştirecek, değişikliği doğrulamak üzere projeyi geliştirme bilgisayarınızda yerel olarak çalıştıracak ve sonra değişikliği Azure'e dağıtacaksınız.

2. *Görünümler/Giriş/Index.cshtml* veya *.vbhtml* dosyasını **Çözüm Gezgini**'nde açın, **h1** başlığını "ASP.NET" yerine "ASP.NET ve Azure" olarak değiştirin ve dosyayı kaydedin. 

	![MVC index.cshtml](./media/web-sites-dotnet-get-started-vs2013/index.png)

	![MVC h1 change](./media/web-sites-dotnet-get-started-vs2013/mvcandazure.png)

1. Siteyi yerel bilgisayarınızda çalıştırarak güncelleştirilmiş başlığı görmek için CTRL+F5 tuşlarına basın.

	![Web site running locally](./media/web-sites-dotnet-get-started-vs2013/localandazure.png)

	`http://localhost` URL'si yerel bilgisayarınızda çalıştığını gösterir. Varsayılan ayar olarak IIS'nin web uygulaması geliştirme sırasında kullanılmak üzere tasarlanmış basit bir sürümü olan IIS Express'te çalışır.


1. Tarayıcıyı kapatın.

1. **Çözüm Gezgini**'nde, projeye sağ tıklayın ve **Yayımla**'yı seçin.

	![Chooose Publish](./media/web-sites-dotnet-get-started-vs2013/choosepublish.png)

	**Web'i Yayımla** sihirbazının Önizleme sekmesi görünür. Yayımlama ayarlarından birini değiştirmeniz gerektiyse farklı bir sekme seçebilirsiniz, ancak şimdilik tek yapmak istediğiniz aynı ayarlarla yeniden dağıtmaktır.

2. **Web'i Yayımla** sihirbazında, **Yayımla**'ya tıklayın.

	![Click Publish](./media/web-sites-dotnet-get-started-vs2013/clickpublish.png)

	Visual Studio projeyi Azure'e dağıtır ve siteyi varsayılan tarayıcıda açar.

	![Changed site deployed](./media/web-sites-dotnet-get-started-vs2013/deployedandazure.png)

**İpucu:** Daha da hızlı dağıtım için **Web Tek Tık Yayımla** araç çubuğunu etkinleştirebilirsiniz. **Görünüm** > **Araç Çubukları**'na tıklayın ve **Web Tek Tık Yayımla**'yı seçin. Bu araç çubuğu bir profil seçerek yayımlamak üzere bir düğmeye tıklamanızı veya bir düğmeye tıklayarak **Web'i Yayımla** sihirbazını açmanızı sağlar. 

![Web Tek Tık Yayımla Araç Çubuğu](./media/web-sites-dotnet-get-started-vs2013/weboneclickpublish.png)

## Siteyi yönetim portalında izleme ve yönetme

[Azure Yönetim Portalı](/tr-tr/services/management-portal/) yeni oluşturduğunuz web sitesi gibi Azure hizmetlerinizi yönetmenize ve izlemenize olanak veren bir web arabirimidir. Öğreticinin bu bölümünde portalda yapabileceklerinizin bir bölümüne göz atacaksınız.

1. Tarayıcınızda, [http://manage.windowsazure.com]() adresine gidin ve Azure kimlik bilgilerinizle oturum açın.

	Portal, Azure hizmetlerinizin listesini görüntüler.

2. Web sitenizin adına tıklayın.

	![Portal home page with new web site called out](./media/web-sites-dotnet-get-started-vs2013/portalhome.png)
  
3. **Pano** sekmesine tıklayın.

	**Pano** sekmesi kullanım istatistiklerinin genel görünümünü ve sık kullanılan site yönetim işlevlerinin bir bölümüne ait bir bağlantıyı gösterir. **Hızlı Bakış**  altında uygulamanızın giriş sayfasına bir bağlantı da bulursunuz.

	![Portal web site dashboard tab](./media/web-sites-dotnet-get-started-vs2013/portaldashboard.png)
  
	Bu noktada, sitenizde çok fazla trafik olmadığından grafik herhangi bir şey göstermeyebilir. Uygulamanıza giderseniz sayfayı birkaç kez yeniledikten sonra portalın **Pano** sayfasını da yenileyin, bazı istatistiklerin çıktığını göreceksiniz. Daha fazla ayrıntı için **Monitör** sekmesine tıklayabilirsiniz.

4. **Yapılandır** sekmesine tıklayın.

	[Yapılandır](/tr-tr/documentation/articles/web-sites-configure//) sekmesi site için kullanılan .NET sürümünü denetlemenizi, [WebSockets](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/) ve [tanı amaçlı günlüğe kaydetme](/tr-tr/documentation/articles/web-sites-enable-diagnostic-log/) gibi özellikleri etkinleştirmenizi, [bağlantı dizesi değerlerini](/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) ayarlamanızı ve daha birçok özelliği sağlar. 

	![Portal web site configure tab](./media/web-sites-dotnet-get-started-vs2013/portalconfigure.png)
  
5. **Ölçek** sekmesine tıklayın.

	Web siteleri hizmetinin ücretli katmanları için [Ölçek](/tr-tr/documentation/articles/web-sites-scale/) sekmesi, trafikteki değişikliklere göre işlem yapmak için web uygulamanıza hizmet veren makinelerinizin boyut ve sayısını denetlemenizi sağlar.

	Elle ölçeklendirebilir veya otomatik ölçeklendirme ölçütleri veya zamanlamaları yapılandırabilirsiniz.

	![Portal website scale tab](./media/web-sites-dotnet-get-started-vs2013/portalscale.png)

Bunlar yönetim portalının özelliklerinden yalnızca birkaçıdır. Ayrıca yeni web siteleri oluşturabilir, mevcut siteleri silebilir, siteleri durdurup yeniden başlatabilir ve veritabanları ve sanal makineler gibi diğer türlerdeki Azure hizmetlerini yönetebilirsiniz.  

**İpucu:** Önizleme sürümünde, nihayetinde kullanmakta olduğunuz sürümün yerini alacak olan yeni bir yönetim portalı vardır. Daha fazla bilgi için bkz. [Azure Önizleme Portalı](/tr-tr/overview/preview-portal/).

## Sonraki adımlar

Bu öğreticide basit bir web uygulaması oluşturup onu Azure Web sitesine dağıtmayı gördünüz. Bunlar hakkında daha fazla bilgi edinmek için ilgili bazı konu ve kaynakları burada bulabilirsiniz.

* Web projesini dağıtmanın diğer yolları

	Bu öğreticide, tek bir işlemle site oluşturup dağıtmanın en hızlı yolunu gördünüz. Visual Studio kullanarak veya [kaynak denetim sisteminden](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) [otomatik dağıtım](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) aracılığıyla diğer dağıtım yollarının genel bir görünümü için bkz. [Azure Web Sitesi Dağıtma](/tr-tr/documentation/articles/web-sites-deploy/"). 

	Visual Studio, dağıtım işlemini otomatikleştirmenizi sağlayan Windows PowerShell betikleri de oluşturabilir. Daha fazla bilgi için bkz. [Her Şeyi Otomatikleştir (Azure ile Gerçek Yaşamdaki Bulut Uygulamalarını Oluşturma)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

* Visual Studio'da web sitesini yönetme

	**Sunucu Gezgini**'nde yapabileceğiniz site yönetim işlevleri hakkında bilgi için bkz. [Visual Studio'da Azure Web Sitelerinin Sorunlarını Giderme](/tr-tr/develop/net/tutorials/troubleshoot-web-sites-in-visual-studio/).

* Web sitesinin sorununu giderme

	Visual Studio, Azure günlüklerini oluşturuldukları sırada gerçek zamanlı görüntülemeyi kolaylaştıran özellikler sağlar. Azure'de uzaktan hata ayıklama modunu da çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Visual Studio'da Azure Web Sitelerinin Sorunlarını Giderme](/tr-tr/develop/net/tutorials/troubleshoot-web-sites-in-visual-studio/).

* Veritabanı ve yetkilendirme işlevi ekleme

	Veritabanına nasıl erişileceğini ve bazı site işlevlerini yetkili kullanıcılarla sınırlandırmayı gösteren bir öğretici için bkz. [Güvenli bir ASP.NET MVC uygulamasını Üyelik, OAuth ve SQL Veritabanı ile birlikte bir Azure Web sitesine dağıtma](/tr-tr/develop/net/tutorials/web-site-with-sql-database/).

* Özel etki alanı adı ve SSL ekleme

	SSL ve kendi etki alanınızı kullanma hakkında bilgi için (örneğin contoso.azurewebsites.net yerine www.contoso.com) aşağıdaki kaynaklara bakın:

	* [Azure Web sitesi için özel etki alanı adı yapılandırma](/tr-tr/documentation/articles/web-sites-custom-domain-name/). 
	* [Azure web sitesi için HTTPS'yi etkinleştirme](http://azure.microsoft.com/tr-tr/documentation/articles/web-sites-configure-ssl-certificate/)

* Boşta kalma zaman aşımlarından sonra uyandırma bekleme süresine engel olma 

	Varsayılan ayar olarak, web siteleri belirli bir süre boşta kaldıklarında kaldırırlar. Bundan sonraki ilk isteğin sitenin yeniden yüklenmesini beklemesi gerekir. Bu bekleme süresinin olmaması için AlwaysOn özelliğini etkinleştirebilirsiniz. Daha fazla bilgi için [Web Sitelerini Yapılandırma](http://azure.microsoft.com/tr-tr/documentation/articles/web-sites-configure/) konusundaki yapılandırma seçeneklerine bakın.

* Sohbet gibi gerçek zamanlı özellikleri ekleme

	Web siteniz gerçek zamanlı özellikler içerecekse (sohbet hizmeti, oyun, fiyat bandı vb.) [ASP.NET SignalR](http://www.asp.net/signalr) kitaplığını [WebSockets](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/) taşıma yöntemi ile birlikte kullanarak en iyi performansı elde edebilirsiniz. Daha fazla bilgi için bkz. [Windows Azure Web sitelerinde SignalR kullanma](http://www.asp.net/signalr/overview/signalr-20/getting-started-with-signalr-20/using-signalr-with-windows-azure-web-sites). 

* Web uygulamaları için Azure Web Siteleri, Bulut Hizmetleri ve Sanal Makinelerden birini seçme

	Azure'de bu öğreticide gösterildiği gibi Web sitelerinde veya Bulut Hizmetlerinde ya da Sanal Makinelerde web uygulamaları çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Azure Yürütme Modelleri](/tr-tr/develop/net/fundamentals/compute/) ve [Azure Web Siteleri, Bulut Hizmetleri ve Sanal Makineler: Hangisini ne zaman kullanmalı?](/tr-tr/manage/services/web-sites/choose-web-app-service/).
