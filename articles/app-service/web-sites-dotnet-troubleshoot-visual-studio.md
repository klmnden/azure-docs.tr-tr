---
title: "Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme"
description: "Uzaktan hata ayıklama, izleme ve Visual Studio 2013 için yerleşik günlük araçlarını kullanarak bir Azure web uygulaması giderileceğini öğrenin."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: e42ff64fdd2be87fc19be267d4e2a29e38f67ef5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme
## <a name="overview"></a>Genel Bakış
Bu öğretici, bir web uygulamasında hata ayıklama yardımcı Visual Studio Araçları nasıl kullanacağınızı gösterir [uygulama hizmeti](http://go.microsoft.com/fwlink/?LinkId=529714), çalıştırarak [hata ayıklama modu](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) uzaktan veya uygulama ve web sunucu günlükleri görüntüleme.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Şunları öğreneceksiniz:

* Azure web uygulaması yönetim işlevleri, Visual Studio'da kullanılabilir.
* Visual Studio uzak görünümü bir uzak web uygulaması hızlı değişiklik yapmak için nasıl kullanılacağını.
* Bir proje sırasında uzaktan hata ayıklama modunda çalıştırmak nasıl Azure üzerinde bir web uygulaması hem bir Web işi çalışıyor.
* Uygulama izleme günlükleri oluşturma ve uygulama çalışırken görüntülemek bunları oluşturuyor.
* Web sunucu günlükleri de dahil olmak üzere, görüntülemek nasıl ayrıntılı hata iletilerini ve başarısız istek izleme.
* Tanılama günlüklerini bir Azure depolama hesabı ve bunları görüntülemek göndermek nasıl.

Visual Studio Ultimate varsa de kullanabilirsiniz [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) hata ayıklama için. Bu öğreticide IntelliTrace kapsamında değildir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici geliştirme ortamı, web projesi ve bölümünde ayarladığınız Azure web uygulaması ile çalışır [Azure ve ASP.NET kullanmaya başlama][GetStarted]. İçin Web işleri bölümleri, oluşturduğunuz uygulama gerekir [Azure WebJobs SDK ile çalışmaya başlama][GetStartedWJ].

Bu öğreticide gösterilen kod örnekleri için bir C# MVC web uygulaması, ancak sorun giderme yordamlarını Visual Basic ve Web Forms uygulamaları için aynı.

Öğretici, Visual Studio 2015 veya 2013 kullanıyorsanız varsayar. Visual Studio 2013 kullanıyorsanız Web işleri özellikleri gerektirir [güncelleştirme 4](http://go.microsoft.com/fwlink/?LinkID=510314) veya sonraki bir sürümü.

Akış günlükleri yalnızca .NET Framework 4 veya sonrasını hedefleyen uygulamalar için çalışır özelliği.

## <a name="sitemanagement"></a>Web Uygulama Yapılandırması ve Yönetimi
Visual Studio web uygulama yönetimi işlevleri ve yapılandırma ayarlarını bulunan alt kümesine erişim sağlar [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). Bu bölümde kullanarak kullanılabilenleri görürsünüz **Sunucu Gezgini**. En son Azure tümleştirme özelliklerini görmek için denemenin **Cloud Explorer** de. Her iki windows açabilirsiniz **Görünüm** menüsü.

1. Zaten Visual Studio'da Azure oturumunuz açık değil,'ı tıklatın **Azure Bağlan** düğmesini **Sunucu Gezgini**.

    Hesabınıza erişimi sağlayan bir yönetim sertifikası yüklemek için kullanılan bir alternatiftir. Bir sertifika yüklemeyi seçerseniz, sağ **Azure** düğümünde **Sunucu Gezgini**ve ardından **yönetin ve filtre abonelikleri** bağlam menüsünde. İçinde **Azure Aboneliklerini Yönet** iletişim kutusu, tıklatın **sertifikaları** sekmesini ve ardından **alma**. Karşıdan yüklemek ve bir abonelik dosyayı içe aktarmak için yönergeleri izleyin (olarak da adlandırılan bir *.publishsettings* dosyası) Azure hesabınız için.

   > [!NOTE]
   > Abonelik dosya indirme, kaynak kodu dizinlerinizi (örneğin, indirme klasöründe) dışında bir klasöre kaydedin ve alma işlemi tamamlandıktan sonra silin. Abonelik dosyasına erişim kazanır kötü niyetli bir kullanıcı düzenleme, oluşturma ve Azure hizmetlerinizi silin.
   >
   >

    Visual Studio'dan Azure kaynaklarına bağlanma hakkında daha fazla bilgi için bkz: [hesaplarını yönetme, abonelikleri ve yönetici rollerini](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. İçinde **Sunucu Gezgini**, genişletin **Azure** ve genişletin **uygulama hizmeti**.
3. Oluşturduğunuz web uygulamasını içeren kaynak grubunu genişletin [Azure ve ASP.NET ile çalışmaya başlama][GetStarted], web app düğümünü sağ tıklatın ve'ı tıklatın **görünüm ayarlarını**.

    ![Sunucu Gezgininde görünümü ayarları](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    **Azure Web uygulaması** sekmesi görüntülenir ve Visual Studio'da bulunan web uygulaması yönetim ve yapılandırma görevleri vardır görebilirsiniz.

    ![Azure Web uygulaması penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    Bu öğreticide, günlüğe kaydetme ve izleme aşağı açılan listeler kullanıyor olmanız. Uzaktan hata ayıklama kullanacağınız ancak etkinleştirmek için farklı bir yöntem kullanmanız.

    Bu pencere uygulama ayarlarının ve bağlantı dizelerinin kutularında hakkında daha fazla bilgi için bkz: [Azure Web Apps: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

    Bu pencerede yapılamaz bir web uygulaması yönetim görevi gerçekleştirmek istiyorsanız, tıklayın **Yönetim Portalı'nda açmak** Azure portalına bir tarayıcı penceresi açın.

## <a name="remoteview"></a>Server Explorer'da erişim web uygulama dosyaları
Genellikle bir web projesi ile dağıttığınız `customErrors` bayrağı ayarlamak Web.config dosyasında `On` veya `RemoteOnly`, yanlış giden anlamına yararlı hata iletisi bir şey olduğunda elde etmezsiniz. İçin birçok hata aldığınız tek şey sayfası aşağıdaki dosyalardan birini gibi.

**'/' Uygulamasında sunucu hatası:**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Bir hata oluştu:**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Web sayfasını görüntüleyemiyor**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Sık hatanın nedenini bulmak için en kolay yolu önceki ekran görüntüleri ilk nasıl yapılacağını açıklar ayrıntılı hata iletileri sağlamaktır. Dağıtılmış Web.config dosyasında değişiklik gerektirir. Düzenleme *Web.config* dosya projede ve projeyi yeniden dağıtın veya oluşturma bir [Web.config dönüştürmesi](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) ve hata ayıklama derlemesi dağıtmak, ancak daha hızlı bir yolu yoktur: içinde **Çözüm Gezgini** doğrudan görüntüleyin ve kullanarak dosyaları uzak web uygulamasında düzenleyin *uzak görünümü* özelliği.

1. İçinde **Sunucu Gezgini**, genişletin **Azure**, genişletin **uygulama hizmeti**, web uygulamanızı bulunan kaynak grubunu genişletin ve ardından web uygulamanız için düğümünü genişletin.

    Web uygulamanızın içerik dosyalarını ve günlük dosyalarını erişmenizi düğümleri bakın.
2. Genişletme **dosyaları** düğümü ve çift *Web.config* dosya.

    ![Web.config dosyasını açın](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio Uzak web uygulamasından Web.config dosyasını açar ve başlık çubuğunda dosya adının yanındaki [Uzaktan] gösterir.
3. Aşağıdaki satırı ekleyin `system.web` öğe:

    `<customErrors mode="Off"></customErrors>`

    ![Web.config Düzenle](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Faydasız hata iletisi gösteren tarayıcıyı yenilemek ve şimdi, aşağıdaki örnek gibi bir ayrıntılı hata iletisi alırsınız:

    ![Ayrıntılı hata iletisi](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (Kırmızı gösterilen satırı ekleyerek gösterilen hata oluşturuldu *Views\Home\Index.cshtml*.)

Web.config dosyasını düzenleme okuma ve Azure web uygulamanızın dosyalarda düzenleme yeteneğini olun daha kolay sorun giderme senaryoları yalnızca bir örneği bulunmaktadır.

## <a name="remotedebug"></a>Uzaktan hata ayıklama web uygulamaları
Ayrıntılı hata iletisi yeterli bilgi sağlamaz ve hata yerel olarak yeniden oluşturulamıyor, sorun giderme için başka bir uzaktan hata ayıklama modunda çalıştırmak için yoludur. Kesme noktalarını ayarlayın, bellek doğrudan işlemek, kod üzerinden adım ve bile kod yolu değiştirin.

Uzaktan hata ayıklama Visual Studio Express sürümlerinde çalışmaz.

Bu bölümde oluşturduğunuz proje kullanarak uzaktan hata ayıklama gösterilmektedir [Azure ve ASP.NET ile çalışmaya başlama][GetStarted].

1. Oluşturduğunuz web projesini açın [Azure ve ASP.NET ile çalışmaya başlama][GetStarted].
2. Açık *Controllers\HomeController.cs*.
3. Silme `About()` aşağıdaki kodu yerine yöntemi ve Ekle.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "The current time is " + currentTime;
            return View();
        }
4. [Bir kesme noktası belirleyerek](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) üzerinde `ViewBag.Message` satır.
5. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve **Yayımla**.
6. İçinde **profil** aynı profil, kullanılan aşağı açılan listesinden, [Azure ve ASP.NET ile çalışmaya başlama][GetStarted].
7. Tıklatın **ayarları** sekmesini tıklatın ve değiştirme **yapılandırma** için **hata ayıklama**ve ardından **Yayımla**.

    ![Hata ayıklama modunda yayımlama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)
8. Dağıtımdan sonra biter ve tarayıcınız, web uygulamanızın Azure URL'sine açıldığında, tarayıcıyı kapatın.
9. İçinde **Sunucu Gezgini**, web uygulamanızı sağ tıklayın ve ardından **Attach hata ayıklayıcı**.

    ![Hata ayıklayıcıyı Ekle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    Tarayıcı, Azure'da çalışan giriş sayfanız için otomatik olarak açılır. Hata ayıklama için sunucusu Azure ayarlar 20 saniye veya bunu bekleyin gerekebilir. Bu gecikme, yalnızca ilk defa bir web uygulamasında hata ayıklama modunda çalıştırdığınızda olur. Sonraki kez var. hata ayıklamayı yeniden başlattığınızda sonraki 48 saat içinde bir gecikme olmayacaktır.

    **Not:** kullanarak yapmak hata ayıklayıcıyı başlatma herhangi bir sorun varsa, deneyin **Cloud Explorer** yerine **Sunucu Gezgini**.
10. Tıklatın **hakkında** menüde.

     Visual Studio kesme noktasına durdurur ve kod Azure içinde değil, yerel bilgisayarınızda çalışıyor.
11. Üzerine gelerek `currentTime` saat değeri görmek için değişkeni.

     ![Hata ayıklama modunda Azure'da çalışan görünümü değişkeni](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     Gördüğünüzde, yerel bilgisayarınızın daha farklı bir saat diliminde olabilir Azure sunucusu saattir.
12. İçin yeni bir değer girin `currentTime` değişken "Şimdi Azure'da çalışan" gibi.
13. Çalışmaya devam edebilmesi için F5 tuşuna basın.

     Azure'da çalışan hakkında sayfa currentTime değişkene girdiğiniz yeni değeri görüntüler.

     ![Yeni değerle sayfası hakkında](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a>Uzaktan hata ayıklama Web işleri
Bu bölümde oluşturduğunuz projeyi ve web uygulamasını kullanarak uzaktan hata ayıklama gösterilmektedir [Azure WebJobs SDK ile çalışmaya başlama](https://github.com/Azure/azure-webjobs-sdk/wiki).

Bu bölümde gösterilen özellikleri yalnızca Visual Studio 2013'te, Update 4 veya daha sonra kullanılabilir.

Uzaktan hata ayıklama yalnızca sürekli Webjob'lar ile çalışır. Zamanlanmış ve isteğe bağlı Webjob'lar, hata ayıklama desteklemez.

1. Oluşturduğunuz web projesini açın [Azure WebJobs SDK ile çalışmaya başlama][GetStartedWJ].
2. ContosoAdsWebJob projeyi açın *Functions.cs*.
3. [Bir kesme noktası belirleyerek](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) işlemindeki ilk deyim üzerinde `GnerateThumbnail` yöntemi.

    ![Kesme noktası ayarlama](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)
4. İçinde **Çözüm Gezgini**, web projesi (Web işi projesinin değil) sağ tıklayın ve **Yayımla**.
5. İçinde **profil** aynı profil, kullanılan aşağı açılan listesinden, [Azure WebJobs SDK ile çalışmaya başlama](https://github.com/Azure/azure-webjobs-sdk/wiki).
6. Tıklatın **ayarları** sekmesini tıklatın ve değiştirme **yapılandırma** için **hata ayıklama**ve ardından **Yayımla**.

    Visual Studio Web işi projeleri ve web dağıtır ve tarayıcınız, web uygulamanızın Azure URL'yi açar.
7. İçinde **Sunucu Gezgini** genişletin **Azure > uygulama hizmeti > kaynak grubunuz > web uygulamanızı > WebJobs > sürekli**ve ardından sağ **ContosoAdsWebJob**.
8. Tıklatın **hata ayıklayıcısını**.

    ![Hata ayıklayıcıyı Ekle](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    Tarayıcı, Azure'da çalışan giriş sayfanız için otomatik olarak açılır. Hata ayıklama için sunucusu Azure ayarlar 20 saniye veya bunu bekleyin gerekebilir. Bu gecikme, yalnızca ilk defa bir web uygulamasında hata ayıklama modunda çalıştırdığınızda olur. Hata ayıklayıcıyı var. Ekle sonraki sefer 48 saat içinde yaparsanız bir gecikme olmayacaktır.
9. Contoso Ads giriş sayfasına açılan web tarayıcısında yeni bir ad oluşturun.

    Bir ad oluşturma tarafından WebJob toplanmış ve işlenen oluşturulması bir kuyruk iletisi neden olur. WebJobs SDK kuyruk iletisi işleyemedi işlevi çağırdığında kodu isabetini karşılaşır.
10. Hata ayıklayıcı kesme noktasında böldüğünde inceleyin ve program bulut çalışırken değişken değerlerini değiştirin. Aşağıdaki çizimde, hata ayıklayıcı GenerateThumbnail yönteme geçirilen blobInfo nesne içeriğini gösterir.

     ![hata ayıklayıcı blobInfo nesnesinde](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
11. Çalışmaya devam edebilmesi için F5 tuşuna basın.

     Küçük resim oluşturma GenerateThumbnail yöntemi tamamlanır.
12. Tarayıcıda, dizin sayfasını yenileyin ve küçük resim bakın.
13. Visual Studio'da hata ayıklamayı durdurmak için SHIFT + F5 tuşuna basın.
14. İçinde **Sunucu Gezgini**, ContosoAdsWebJob düğümünü sağ tıklatın ve **görünüm Pano**.
15. Azure kimlik bilgilerinizle oturum ve, WebJob için sayfaya gitmek için Web işi adı'ye tıklayın.

     ![ContosoAdsWebJob'ı tıklatın](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Pano, son yürütülen GenerateThumbnail işlev gösterir.

     (Bir sonraki tıklattığınızda **görünüm Pano**, oturum açmak zorunda değilsiniz ve tarayıcı, WebJob için doğrudan sayfasına gider.)
16. İşlev yürütme ayrıntılarını görmek için işlev adına tıklayın.

     ![İşlev ayrıntıları](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

İşlevinizi [günlükleri yazdı](https://github.com/Azure/azure-webjobs-sdk/wiki), tıklatın **ToggleOutput** bunları görmek için.

## <a name="notes-about-remote-debugging"></a>Uzaktan hata ayıklama hakkında notlar
* Hata ayıklama modunda üretimde çalışan önerilmez. Üretim web uygulamanız çıkışı için birden fazla sunucu örneğinin ölçeklenmez, hata ayıklama web sunucusu diğer isteklere yanıt vermesini engeller. Hata ayıklayıcı için taktığınızda birden çok web sunucusu örneği varsa, rastgele bir örneği elde edersiniz ve sonraki tarayıcı istekleri için bu örneği gidecek sağlamanın bir yolu yoktur. Ayrıca, genellikle üretim için hata ayıklama derlemesi dağıtmak yok ve yayın derlemeleri derleyici iyileştirmelerini satır kaynak kodunuzda neler olduğunu göstermek imkansız yapabilir. Üretim ilgili sorunları gidermek için en iyi uygulama kaynaktır izleme ve web sunucu günlükleri.
* Kesme noktaları uzak uzun durur kaçının hata ayıklama. Azure, yanıt vermeyen bir işlem olarak birkaç dakikadan uzun süre için durduruldu ve öyle kapanır bir işlem değerlendirir.
* Hatalarını ayıkladığınız sırada sunucu bant genişliği ücretleri etkileyebilecek Visual Studio için veri gönderiyor. Bant genişliği oranları hakkında daha fazla bilgi için bkz: [Azure fiyatlandırma](https://azure.microsoft.com/pricing/calculator/).
* Olduğundan emin olun `debug` özniteliği `compilation` öğesinde *Web.config* dosya ayarlanmış true. Ayarlanmış bir hata ayıklama yapı yapılandırması yayımladığınızda, varsayılan olarak true.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Hata ayıklayıcı hata ayıklamak istediğiniz koda adım olmaz bulursanız, sadece kendi kodumu ayarını değiştirmeniz gerekebilir.  Daha fazla bilgi için bkz: [sadece kendi kodumu atlama sınırla](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Uzaktan hata ayıklama özelliği etkinleştirmek ve 48 saat sonra özelliği otomatik olarak devre dışı bir süreölçer sunucuda başlar. Bu 48 saat sınır güvenlik ve performans nedenleriyle yapılır. İstediğiniz şekilde geri sayıda saatlerinin özelliği kolayca kapatabilirsiniz. Değil etkin olarak ayıklarken devre dışı bırakarak öneririz.
* Hata ayıklayıcı herhangi bir işlem için yalnızca web uygulaması işleminin (w3wp.exe) el ile ekleyebilirsiniz. Visual Studio'da hata ayıklama modunu kullanma hakkında daha fazla bilgi için bkz: [Visual Studio'da hata ayıklamayı](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Tanılama günlükleri'ne genel bakış
Bir Azure web uygulamasında çalışan bir ASP.NET uygulaması günlükleri aşağıdaki türlerini oluşturabilirsiniz:

* **Uygulama izleme günlükleri**<br/>
  Uygulama yöntemlerini çağırarak bu günlükler oluşturur [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) sınıfı.
* **Web sunucu günlükleri**<br/>
  Web sunucusu web uygulaması her HTTP isteği için bir günlük girişi oluşturur.
* **Ayrıntılı hata iletisi günlükleri**<br/>
  Web sunucusu başarısız HTTP isteklerini (Bu durum kodu 400 veya daha büyük sonuç) için bazı ek bilgiler içeren bir HTML sayfası oluşturur.
* **İstek izleme günlüklerini başarısız oldu**<br/>
  Web sunucusu, başarısız olan HTTP istekleri için ayrıntılı izleme bilgilerini içeren bir XML dosyası oluşturur. Web sunucusu, aynı zamanda bir tarayıcıda XML biçimine XSL dosyasını sağlar.

Azure etkinleştirmek veya her günlük türü gerektiği gibi devre dışı olanağı sağlar şekilde günlük web uygulama performansını etkiler. Uygulama günlükleri için yalnızca belirli bir önem düzeyi yukarıda günlükleri yazılması belirtebilirsiniz. Yeni bir web uygulaması varsayılan olarak tüm günlük oluştururken devre dışı bırakılır.

Günlükleri dosyalarına yazılır bir *LogFiles* web uygulamasını ve FTP aracılığıyla erişilebilir olan dosya sistemi klasöründe. Ayrıca bir Azure depolama hesabı, Web sunucusu ve uygulama günlükleri yazılabilir. Büyük bir dosya sistemini de mümkün olandan bir depolama hesabındaki günlükler hacmi tutabilirsiniz. Dosya sistemi kullandığınızda en fazla 100 megabaytlık günlükler için sınırlı. (Yalnızca kısa vadeli bekletme için dosya sistemi günlükleri içindir. Azure sınıra ulaşıldıktan sonra yeni değerler için yer açmak için eski günlük dosyaları siler.)  

## <a name="apptracelogs"></a>Oluşturma ve uygulama izleme günlüklerini görüntüle
Bu bölümde aşağıdaki görevleri gerçekleştirmeniz:

* Oluşturduğunuz web projesine izleme deyimleri ekleme [Azure ve ASP.NET kullanmaya başlama][GetStarted].
* Projeyi yerel olarak çalıştırdığınızda günlüklerine bakın.
* Azure üzerinde çalışan uygulama tarafından oluşturulan günlükleri görüntüleyin.

İçinde Web işleri uygulaması oluşturma hakkında bilgi günlükleri için bkz: [Azure kuyruk depolama ile çalışmaya nasıl WebJobs SDK - günlüklerini yazma izni nasıl kullanarak](https://github.com/Azure/azure-webjobs-sdk/wiki). Günlükleri görüntüleme ve Azure'da nasıl depolandıkları denetlemek için aşağıdaki yönergeleri WebJobs tarafından oluşturulan uygulama günlükleri için de geçerlidir.

### <a name="add-tracing-statements-to-the-application"></a>Uygulama izleme deyimleri ekleme
1. Açık *Controllers\HomeController.cs*ve değiştirme `Index`, `About`, ve `Contact` yöntemleri eklemek için aşağıdaki kod ile `Trace` deyimleri ve `using` bildirimi `System.Diagnostics`:

        public ActionResult Index()
        {
            Trace.WriteLine("Entering Index method");
            ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";
            Trace.TraceInformation("Displaying the Index page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Index method");
            return View();
        }

        public ActionResult About()
        {
            Trace.WriteLine("Entering About method");
            ViewBag.Message = "Your app description page.";
            Trace.TraceWarning("Transient error on the About page at " + DateTime.Now.ToShortTimeString());
            Trace.WriteLine("Leaving About method");
            return View();
        }

        public ActionResult Contact()
        {
            Trace.WriteLine("Entering Contact method");
            ViewBag.Message = "Your contact page.";
            Trace.TraceError("Fatal error on the Contact page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Contact method");
            return View();
        }        
2. Ekleme bir `using System.Diagnostics;` dosyanın en üstüne ifadesine.

### <a name="view-the-tracing-output-locally"></a>Yerel olarak izleme çıktısını görüntüleyin
1. Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın.

    Varsayılan izleme dinleyicisi tüm izleme çıktısı Yazar **çıkış** yanı sıra diğer hata ayıklama çıktı penceresi. Aşağıdaki çizimde eklediğiniz izleme deyimleri çıktısını gösterir `Index` yöntemi.

    ![Hata ayıklama penceresinde izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    Aşağıdaki adımlar, hata ayıklama modunda derleme olmadan bir web sayfasında izleme çıktısını görüntülemek nasıl gösterir.
2. Uygulamanın Web.config dosyasını (proje klasöründe bir) açın ve eklemek bir `<system.diagnostics>` öğesi yalnızca kapatmadan önce dosya sonunda `</configuration>` öğe:

          <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener,
                    System.Web,
                    Version=4.0.0.0,
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    `WebPageTraceListener` Görüntülemenizi sağlar göz atarak çıkış izleme `/trace.axd`.
3. Ekleme bir <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trace ögesi</a> altında `<system.web>` Web.config dosyasında, aşağıdaki örnek gibi:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Uygulamayı çalıştırmak için CTRL+F5'e basın.
5. Tarayıcının adres çubuğunda eklemek *trace.axd* URL ve (URL olacaktır http://localhost:53370/trace.axd için benzer) Enter tuşuna basın.
6. Üzerinde **uygulama izleme** sayfasında, **ayrıntıları görüntüle** ilk satırda (BrowserLink satır değil).

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    **İstek ayrıntıları** sayfası görüntülenirse hem de **izleme bilgilerini** eklediğiniz izleme deyimleri çıktısını gördüğünüz bölüm `Index` yöntemi.

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Varsayılan olarak, `trace.axd` yalnızca yerel olarak kullanılabilir. Uzak web uygulamasından kullanılabilmesini isteseydiniz, ekleyebilirsiniz `localOnly="false"` için `trace` öğesinde *Web.config* dosya, aşağıdaki örnekte gösterildiği gibi:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Ancak, etkinleştirme `trace.axd` bir üretim web uygulaması güvenlik nedenleriyle genellikle önerilmez ve aşağıdaki bölümlerde bir Azure web uygulamasında izleme günlüklerini okumak için daha kolay bir yolu görürsünüz.

### <a name="view-the-tracing-output-in-azure"></a>Azure'da izleme çıktısını görüntüleyin
1. İçinde **Çözüm Gezgini**, web projesine sağ tıklatın ve **Yayımla**.
2. İçinde **Web'i Yayımla** iletişim kutusu, tıklatın **Yayımla**.

    Visual Studio güncelleştirmenizi yayımlar sonra giriş sayfanız için bir tarayıcı penceresi açar (kaydetmedi temizleyin varsayılarak **hedef URL** üzerinde **bağlantı** sekmesi).
3. İçinde **Sunucu Gezgini**, web uygulamanızı sağ tıklatıp **akış günlüklerini görüntüle**.

    ![Bağlam menüsünde akış günlüklerini görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    **Çıkış** penceresi gösterir günlük akış hizmetine bağlanır ve bir bildirim satırı görüntülemek için bir günlük gider her dakika ekler.

    ![Bağlam menüsünde akış günlüklerini görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. Uygulama giriş sayfanız gösteren tarayıcı penceresinde **kişi**.

    Birkaç saniye içinde hata düzeyinden İzleme çıkışı eklediğiniz `Contact` yöntemi görünür **çıkış** penceresi.

    ![Çıktı penceresinde hata izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Hizmet izleme günlüğü etkinleştirdiğinizde, varsayılan ayar olduğu için visual Studio hata düzeyi izlemeleri yalnızca göstermez. Yeni bir Azure web uygulaması oluşturduğunuzda, önceki ayarlar sayfasını açtığınızda gördüğünüz gibi tüm günlük varsayılan olarak devre dışıdır:

    ![Oturum kapatma uygulama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Ancak, seçtiğinizde **akış günlüklerini görüntüle**, Visual Studio otomatik olarak değiştirildi **uygulama Logging(File System)** için **hata**, bildirilen hata düzeyi günlükleri anlamına gelir. Tüm izleme günlüklerini görmek için bu ayarı değiştirebilirsiniz **ayrıntılı**. Hata düşük bir önem düzeyi seçtiğinizde, daha yüksek önem düzeyleri için tüm günlükleri de raporlanır. Ayrıntılı seçtiğinizde, bu nedenle, aynı zamanda bilgi, uyarı ve Hata günlüklerini görürsünüz.  

1. İçinde **Sunucu Gezgini**, web uygulaması sağ tıklayın ve ardından **görünüm ayarlarını** daha önce yaptığınız gibi.
2. Değişiklik **uygulama günlüğü (dosya sistemi)** için **ayrıntılı**ve ardından **kaydetmek**.

    ![İçin ayrıntılı izleme düzeyini ayarlama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. Şimdi gösteren tarayıcı penceresinde, **kişi** sayfasında, **giriş**, ardından **hakkında**ve ardından **kişi**.

    Birkaç saniye içinde **çıkış** penceresi tüm izleme çıktısını gösterir.

    ![Ayrıntılı izleme çıktısı](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    Bu bölümde etkin ve Azure web uygulaması ayarları kullanarak günlük kaydını devre dışı. Ayrıca, etkinleştirmek ve Web.config dosyasını değiştirerek izleme dinleyicileri devre dışı bırakın. Ancak, Web.config dosyasında değişiklik günlüğünü web uygulama yapılandırması aracılığıyla etkinleştirme, değil yaparken, geri dönüştürmek uygulama etki alanı neden olur. Sorunu yeniden oluşturmak için bir uzun sürüyorsa veya aralıklı, uygulama etki alanı geri dönüştürme "Düzelt" ve yeniden işlem yapılana kadar beklenecek zorlayabilir. Hata bilgilerini hemen yakalama başlatabilmeniz Azure tanılama etkinleştirme, bunu yapmaz.

### <a name="output-window-features"></a>Çıktı penceresi özellikleri
**Azure günlükleri** sekmesinde **çıkış** penceresinde birkaç düğmeler ve metin kutusu vardır:

![Düğmeleri günlükleri sekmesi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Bunlar aşağıdaki işlevleri gerçekleştirir:

* Clear **çıkış** penceresi.
* Etkinleştirmek veya sözcük kaydırmayı devre dışı.
* Başlat veya günlüklerini İzlemeyi Durdur.
* Hangi günlüklerinin izleneceğini belirt
* Günlükleri indirin.
* Bir arama dizesi veya normal bir ifade göre günlükleri filtreleyin.
* Kapat **çıkış** penceresi.

Bir arama dizesi veya normal ifade girerseniz, Visual Studio istemci günlük bilgileri filtreler. Günlükleri görüntülenen sonra ölçütleri girebilirsiniz anlamına **çıkış** penceresindeki değiştirebilirsiniz filtreleme ölçütlerini günlükleri yeniden oluşturmak zorunda kalmadan.

## <a name="webserverlogs"></a>Web sunucusu günlüklerini görüntüle
Web sunucu günlükleri, web uygulaması için tüm HTTP etkinliğini kaydeder. Bunları görmek için **çıkış** penceresi sahip web uygulaması için etkinleştirme ve bunları izlemek istediğiniz Visual Studio söyleyin.

1. İçinde **Azure Web uygulaması yapılandırmasında** gelen açtığınız sekmesini **Sunucu Gezgini**, Web Server günlüğü için değiştirme **üzerinde**ve ardından **kaydetmek**.

    ![Web sunucusu günlüğü etkinleştirme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. İçinde **çıkış** penceresinde tıklatın **Azure günlüklerinin izleneceğini belirt** düğmesi.

    ![Hangi Azure günlüklerinin izleneceğini belirt](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. İçinde **Azure günlük seçenekleri** iletişim kutusunda **Web sunucu günlükleri**ve ardından **Tamam**.

    ![İzleyici web sunucu günlükleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. Web uygulaması gösteren tarayıcı penceresinde **giriş**, ardından **hakkında**ve ardından **kişi**.

    Uygulama günlüklerini genellikle web sunucu günlükleri tarafından izlenen önce görünür. Günlükleri görünmesi için bir süre beklemeniz gerekebilir.

    ![Çıktı penceresinde Web sunucu günlükleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Visual Studio kullanarak ilk web sunucu günlükleri etkinleştirdiğinizde varsayılan olarak, Azure dosya sistemine günlükler yazar. Alternatif olarak, bir depolama hesabındaki bir blob kapsayıcısını günlüklerine yazılmış web sunucusu belirtmek için Azure portalını kullanabilirsiniz.

Visual Studio'da günlüğü, yeniden etkinleştirme Visual Studio'da günlüğe kaydetme, web sunucusunu bir Azure depolama hesabı günlüğü etkinleştirmek üzere portalı kullanın ve ardından devre dışı depolama hesap ayarlarınızı geri yüklenir.

## <a name="detailederrorlogs"></a>Ayrıntılı hata iletisi günlükleri görüntüle
Ayrıntılı hata günlükleri hata yanıt kodları (400 veya üstü) neden HTTP istekleriyle ilgili bazı ek bilgiler sağlar. Bunları görmek için **çıkış** penceresinde sahip web uygulaması için etkinleştirme ve bunları izlemek istediğiniz Visual Studio söyleyin.

1. İçinde **Azure Web uygulaması yapılandırmasında** gelen açtığınız sekmesini **Sunucu Gezgini**, değiştirme **ayrıntılı hata iletileri** için **üzerinde**ve ardından **kaydetmek**.

    ![Ayrıntılı hata iletilerini etkinleştir](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)
2. İçinde **çıkış** penceresinde tıklatın **Azure günlüklerinin izleneceğini belirt** düğmesi.
3. İçinde **Azure günlük seçenekleri** iletişim kutusu, tıklatın **tüm günlükleri**ve ardından **Tamam**.

    ![Tüm günlükler izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)
4. Tarayıcının adres çubuğunda fazladan bir karakter 404 hatası neden URL'si ekleyin (örneğin, `http://localhost:53370/Home/Contactx`), ve Enter tuşuna basın.

    Visual Studio birkaç saniye sonra ayrıntılı hata günlüğü görünür **çıkış** penceresi.

    ![Çıktı penceresinde ayrıntılı hata günlüğü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Control + bir tarayıcıda biçimlendirilmiş günlük çıkışı görmek için bağlantıya tıklayın:

    ![Tarayıcı penceresinde ayrıntılı hata günlüğü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Dosya sistem günlüklerini indirin
İçinde izleyebilirsiniz herhangi bir günlük **çıkış** penceresi de indirilebilir olarak bir *.zip* dosyası.

1. İçinde **çıkış** penceresinde tıklatın **akış günlükleri indirmek**.

    ![Düğmeleri günlükleri sekmesi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Dosya Gezgini'ni açar için *indirmeleri* seçili indirilen dosya ile klasör.

    ![İndirilen Dosya](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Extract *.zip* dosya ve aşağıdaki klasör yapısını bakın:

    ![İndirilen Dosya](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Uygulama izleme günlükleri olan *.txt* dosyalar *LogFiles\Application* klasör.
   * Web sunucu günlükleri olan *.log* dosyalar *LogFiles\http\RawLogs* klasör. Bir aracı gibi kullanabilir [günlük ayrıştırıcısı](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) görüntülemek ve bu dosyaları işlemek için.
   * Ayrıntılı hata iletisi günlüklerin içinde *.html* dosyalar *LogFiles\DetailedErrors* klasör.

     ( *Dağıtımları* klasördür kaynak denetim yayımlamayı; tarafından oluşturulan dosyalar için Visual Studio yayımlama ile ilgili herhangi bir şey yok. *Git* klasördür kaynak denetimi ile ilgili izlemeleri için yayımlama ve günlük dosyası akış hizmetine.)  

## <a name="storagelogs"></a>Depolama günlüklerini görüntüle
Uygulama izleme günlükleri, bir Azure depolama hesabı gönderilebilir ve Visual Studio'da görüntüleyebilirsiniz. Bir depolama hesabı oluşturacağız yapmak için Klasik Portalı'nda depolama günlüklerini etkinleştirmek ve görüntülemeye **günlükleri** sekmesinde **Azure Web uygulaması** penceresi.

Herhangi birini veya tümünü üç hedefler için günlüklerini gönderebilirsiniz:

* Dosya sistemi.
* Depolama hesabı tabloları.
* Depolama hesabı BLOB'ları.

Her hedef için farklı önem düzeyi belirtebilirsiniz.

Tabloları çevrimiçi günlükleri ayrıntılarını görüntülemek kolaylaştırır ve akış destekledikleri; tablolardaki günlüklerini sorgular ve bunlar oluşturuldukça yeni günlüklerine bakın. BLOB'ları kolaylaştırır dosyalarında günlükleri indirmek ve çözümlemek için Hdınsight, Hdınsight blob storage ile çalışmak nasıl bildiği için kullanma. Daha fazla bilgi için bkz: **Hadoop ve MapReduce** içinde [veri depolama seçenekleri (yapı gerçek bulut uygulamaları Azure ile)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

Ayrıntı düzeyi ayarlamak dosya sistemi günlükleri şu anda yok; Aşağıdaki adımlar depolama hesabı tablolara gitmek için bilgi düzeyi günlüklerini ayarı aracılığıyla yol. Bilgi düzeyi anlamına gelmektedir çağrılarak oluşturulan tüm günlükleri `Trace.TraceInformation`, `Trace.TraceWarning`, ve `Trace.TraceError` , çağıran oluşturulan günlükleri ancak görüntülenir `Trace.WriteLine`.

Daha fazla depolama ve kalıcı daha uzun bekletme dosya sistemine karşılaştırıldığında günlükleri için depolama hesapları sunar. Uygulama izleme günlükleri depolama alanına göndermeden başka bir avantajı, dosya sistem günlüklerini alma her günlüğü ile bazı ek bilgiler almak olmasıdır.

1. Sağ **depolama** Azure düğümünü ve ardından altında **depolama hesabı oluştur**.

![Depolama hesabı oluşturma](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. İçinde **depolama hesabı oluştur** iletişim kutusunda, depolama hesabı için bir ad girin.

    Adı olmalı benzersiz olmalıdır (başka bir Azure depolama hesabı aynı ada sahip olabilir). Girdiğiniz ad zaten kullanımda olduğunda değiştirmek için bir fırsat elde edersiniz.

    Depolama hesabınıza erişmek için URL *{ad}*. core.windows.net.
2. Ayarlama **bölge veya benzeşim grubunda** size en yakın bölgeyi aşağı açılan liste.

    Bu ayar, hangi Azure veri merkezi depolama hesabınız barındıracak belirtir. Bu öğretici için tercih ettiğiniz bir dikkat çekici fark yapmaz ancak bir üretim web uygulaması için web sunucunuz ve gecikme süresi ve veri çıkış ücretleri en aza indirmek için aynı bölgede olması için depolama hesabınız istiyorsunuz. (Daha sonra oluşturacağınız) web uygulamasını olabildiğince gecikme süresi en aza indirmek için web uygulamanızın erişme tarayıcılar mümkün olduğunca yakın bir bölgede çalıştırmanız gerekir.
3. **Çoğaltma** açılır listesini **Yerel olarak yedekli** olarak ayarlayın.
   
    Bir depolama hesabı için coğrafi çoğaltma etkinleştirildiğinde, birincil konumda önemli bir olağanüstü durum oluşması halinde ikincil bir veri merkezine yük devretme için depolanan içerik bu konuma çoğaltılır. Coğrafi çoğaltma ek ücretlere neden olabilir. Test ve geliştirme hesaplarında genellikle coğrafi çoğaltma için ödeme yapmak istemezsiniz. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma, yönetme veya silme](../storage/common/storage-create-storage-account.md).
4. **Oluştur**'a tıklayın.

    ![Yeni depolama hesabı](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. Visual Studio'da **Azure Web uygulaması** penceresinde tıklatın **günlükleri** sekmesini ve ardından **günlüğü yapılandırma Yönetim Portalı'nda**.

    <!-- todo:screenshot of new portal if the VS page link goes to new portal -->
    ![Günlük tutmayı yapılandırma](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    Bu açılır **yapılandırma** sekmesi, web hizmetiniz için Klasik Portalı'nda.
6. Klasik portalın içinde **yapılandırma** sekmesinde, uygulama Tanılama bölümüne kaydırın ve ardından değiştirmek **uygulama günlüğü (Table Storage)** için **üzerinde**.
7. Değişiklik **günlüğe kaydetme düzeyi** için **bilgi**.
8. Tıklatın **tablo depolama yönetin**.

    ![TableStorage Yönet'e tıklayın](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    İçinde **uygulama tanılama için Tablo depolama yönetmek** kutusunda seçebileceğiniz depolama hesabınızın birden fazla varsa. Yeni bir tablo oluşturmak veya mevcut bir kullanın.

    ![Tablo depolama yönetin](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. İçinde **uygulama tanılama için Tablo depolama yönetmek** kutusuna kutusunu kapatmak için onay işaretine tıklayın.
10. Klasik portalın içinde **yapılandırma** sekmesini tıklatın, **kaydetmek**.
11. Uygulama web uygulaması görüntüleyen tarayıcı penceresinde **giriş**, ardından **hakkında**ve ardından **kişi**.

     Bu web sayfaları göz atarak üretilen günlük kaydı bilgileri depolama hesabına yazılır.
12. İçinde **günlükleri** sekmesinde **Azure Web uygulaması** Visual Studio penceresinde tıklatın **yenileme** altında **tanılama özeti**.

     ![Yenile'yi tıklatın](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     **Tanılama özeti** bölümü varsayılan olarak son 15 dakika için günlükleri gösterir. Daha fazla günlüklerine bakın dönemi değiştirebilirsiniz.

     ("Tablosu bulunamadı" hata iletisi alırsanız, etkin sonra izleme yapmak sayfaları taranan doğrulayın **uygulama günlüğü (Depolama)** ve tıklattığınız sonra **kaydetmek**.)

     ![Depolama günlükleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Bu konuda gördüğünüz bildirimi bkz **işlem kimliği** ve **iş parçacığı kimliği** dosya sistemi günlüklerinde elde etmezsiniz her oturum için. Azure depolama tablosu doğrudan görüntüleyerek ek alanlar görebilirsiniz.
13. Tıklatın **tüm uygulama günlüklerini görüntüle**.

     İzleme günlüğü tablosu Azure depolama tablo Görüntüleyicisi'nde görüntülenir.

     ("Dizi bir öğe içermiyor" bir hata alırsanız, açık **Sunucu Gezgini**, depolama hesabınızın altında düğümünü genişletin **Azure** düğümünü ve ardından sağ tıklayarak **tabloları** tıklatıp **yenileme**.)

     ![Tablo görünümünde depolama günlükleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     Bu görünüm, diğer bir görünümlerde görmüyorum ek alanlar gösterir. Bu görünüm bir sorgu oluşturmak için özel sorgu oluşturucu kullanıcı arabirimini kullanarak günlükleri filtreleyin sağlar. Daha fazla bilgi için bkz. tablo kaynaklarla çalışmak - varlıklarda filtreleme [Sunucu Gezgini ile depolama kaynaklarını gözatma](http://msdn.microsoft.com/library/ff683677.aspx).
14. Tek bir satır ayrıntılarını görmek için satırlardan birinin çift tıklayın.

     ![Sunucu Gezgininde izleme tablosu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>Başarısız istek izleme günlüklerini görüntüle
IIS URL yeniden yazma işlemi veya kimlik doğrulama sorunları gibi senaryolarda bir HTTP isteğinin nasıl işleme ayrıntılarını anlamak gerektiğinde başarısız istek izleme günlüklerini yararlı olur.

Azure web uygulamaları IIS 7.0 ile kullanılabilir ve sonraki aynı başarısız istek izleme işlevini kullanın. Hangi hatalar, ancak günlüğe yapılandırmak için IIS ayarlarını erişimi yok. Başarısız istek izleme etkinleştirdiğinizde, tüm hataları yakalanır.

Visual Studio kullanarak başarısız istek izleme etkinleştirebilirsiniz, ancak Visual Studio'da görüntüleyemezsiniz. Bu günlükler XML dosyalarıdır. Akış günlüğü hizmeti yalnızca düz metin modunda okunabilir sayılan dosyaları izler: *.txt*, *.html*, ve *.log* dosyaları.

Bir tarayıcıda, yerel bilgisayarınıza indirmek için bir FTP aracı kullanarak yerel olarak sonra veya FTP aracılığıyla doğrudan başarısız istek izleme günlüklerini görüntüleyebilirsiniz. Bu bölümde, bir tarayıcıda doğrudan görüntülediğiniz.

1. İçinde **yapılandırma** sekmesinde **Azure Web uygulaması** den açılan pencere **Sunucu Gezgini**, değiştirme **başarısız istek izleme** için **üzerinde**ve ardından **kaydetmek**.

    ![Başarısız istek izlemeyi etkinleştir](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Web uygulaması gösterir tarayıcının adres çubuğundaki URL'ye fazladan bir karakter ekleyin ve 404 hatası neden Enter'ı tıklatın.

    Bu oluşturulacak bir başarısız istek izleme günlüklerini neden olur ve nasıl görüntülemek veya günlük indirmek aşağıdaki adımları gösterir.
3. Visual Studio'da içinde **yapılandırma** sekmesinde **Azure Web uygulaması** penceresinde tıklatın **Yönetim Portalı'nda Aç**.
4. İçinde [Azure Portal](https://portal.azure.com) **ayarları** web uygulamanız için dikey tıklayın **dağıtım kimlik bilgileri**ve ardından yeni bir kullanıcı adı ve parolayı girin.

    ![Yeni FTP kullanıcı adı ve parola](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    ** Oturum açtığınızda için önek web uygulaması adı ile tam kullanıcı adını kullanmak zorunda. Örneğin, "myId" bir kullanıcı adı girin ve "Örneğin" ifadesini site ise, "myexample\myid" oturum açın.
5. Altında gösterilen URL'yi yeni bir tarayıcı penceresine gidin **FTP ana bilgisayar adı** veya **FTPS konak adı** içinde **Web uygulaması** dikey web uygulamanız için.
6. Daha önce (de dahil olmak üzere web uygulama adı, kullanıcı adı için önek) oluşturulan FTP kimlik bilgilerini kullanarak oturum açın.

    Tarayıcı web uygulamasının kök klasörü gösterir.
7. Açık *LogFiles* klasör.

    ![LogFiles klasörünü açın](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)
8. W3SVC artı bir sayısal değer adlı klasörü açın.

    ![W3SVC klasörünü açın](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    Klasör için başarısız istek izleme etkin sonra günlüğe kaydedilmiş hataları XML dosyaları ve bir tarayıcı XML biçimlendirmek için kullanabileceğiniz bir XSL dosyasını içerir.

    ![W3SVC klasörü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)
9. XML dosyası için izleme bilgilerini görmek istediğiniz başarısız istekleri için tıklatın.

    Aşağıdaki çizim bir örnek hata izleme bilgilerinin bir parçası gösterir.

    ![Başarısız istek izleme tarayıcıda](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Sonraki Adımlar
Nasıl Visual Studio Azure web uygulaması tarafından oluşturulan günlükleri görüntülemek kolaylaştırır gördünüz. Aşağıdaki bölümler, ilgili konular hakkında daha fazla kaynaklarına bağlantılar sağlar:

* Azure web uygulaması sorunlarını giderme
* Visual Studio'da hata ayıklama
* Uzaktan hata ayıklama ile Azure
* ASP.NET uygulamalarında izleme
* Analiz etme web sunucu günlükleri
* Başarısız istek izleme günlüklerini analiz etme
* Hata ayıklama bulut Hizmetleri

### <a name="azure-web-app-troubleshooting"></a>Azure web uygulaması sorunlarını giderme
Azure App Service'deki web uygulamaları sorunlarını giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Web uygulamaları izleme](/manage/services/web-sites/how-to-monitor-websites/)
* [Bellek sızıntıları Visual Studio 2013 ile Azure Web uygulamalarında araştırma](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM blog gönderisi yönetilen bellek sorunları çözümlemek için Visual Studio özellikleri hakkında.
* [Azure web apps çevrimiçi araçları hakkında bilmeniz](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Blog gönderisi Amit Apple tarafından.

Belirli bir sorun giderme sorusu ile daha fazla yardım için bir iş parçacığı aşağıdaki forumları birini başlatabilirsiniz:

* [Azure Forumu ASP.NET sitesinde](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [MSDN'deki Azure Forumu](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Visual Studio'da hata ayıklama
Visual Studio'da hata ayıklama modunu kullanma hakkında daha fazla bilgi için bkz: [Visual Studio'da hata ayıklamayı](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) MSDN konusuna ve [Visual Studio 2010 ile hata ayıklama ipuçları](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Uzaktan hata ayıklama ile Azure
Azure web uygulamaları ve Web işleri için uzaktan hata ayıklama hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure uygulama hizmeti Web uygulamalarında uzaktan hata ayıklama giriş](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Uzaktan hata ayıklama içinde uzaktan hata ayıklama Azure App Service Web Apps bölüm 2 - giriş](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Uzaktan hata ayıklama için giriş Azure App Service Web Apps bölümü 3 - çok örnekli ortamı ve GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [Web işleri hata ayıklama (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Bir Azure Web API'si veya Mobile Services arka uç, web uygulamanızın kullanır ve gerekiyorsa, hata ayıklamak için bkz: [Visual Studio .NET arka ucu hata ayıklama](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>ASP.NET uygulamalarında izleme
Internet üzerinde hiçbir kapsamlı ve güncel tanıtımlar ASP.NET izleme için vardır. Yapabileceğiniz en iyi olduğu ile çalışmaya başlama MVC kaydetmedi henüz mevcut ve yeni Web günlüğü ile desteklemek için Web Forms belirli sorunları odaklanan yazıları için yazılmış eski giriş malzemeleri. İyi yerler başlatmak için aşağıdaki kaynaklara şunlardır:

* [İzleme ve Telemetri (Azure ile gerçek bulut uygulamaları derleme)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  E-kitap bölüm Azure bulut uygulamalarında izleme için öneriler.
* [ASP.NET izleme](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Eski ancak yine de konu için temel bir giriş için iyi bir kaynak.
* [İzleme dinleyicileri](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  İzleme dinleyicileri hakkında bilgi ancak Bahsediyor değil [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [İzlenecek yol: ASP.NET izleme System.Diagnostics izleme ile tümleştirme](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  Bu çok eski olduğundan, ancak giriş makalesi kapsamıyordur bazı ek bilgiler içerir.
* [ASP.NET MVC Razor görünümleri izleme](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Razor görünümlerinde izlemenin yanı sıra post ayrıca tüm işlenmeyen özel durumlar bir MVC uygulamasında oturum için bir hata filtresi oluşturma açıklanmaktadır. Global.asax örnekte tüm işlenmeyen özel durumlar bir Web Forms uygulamasında oturum hakkında daha fazla bilgi için bkz [hata işleyicileri için tam bir örnek](http://msdn.microsoft.com/library/bb397417.aspx) konusuna bakın. MVC veya Web Forms, belirli özel durumları günlüğe kaydetmek ancak etkili kendileri için işleme varsayılan framework izin vermek istediğiniz yaparsanız catch ve aşağıdaki örnekte olduğu gibi yeniden oluşturma:

        try
        {
           // Your code that might cause an exception to be thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Azure komut satırı (artı Glimpse'in!) günlük akış Tanılama izleme](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Visual Studio'da nasıl hangi Bu öğretici yapmak için komut satırı kullanmayı gösterir. [Glimpse'in](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) ASP.NET uygulamalarında hata ayıklama için bir araçtır.
* [Web uygulamaları günlüğe kaydetme ve tanılama - David Ebbo ile kullanarak](/documentation/videos/azure-web-site-logging-and-diagnostics/) ve [- David Ebbo ile Web uygulamaları günlüklerinden akış](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Videolar Scott Hanselman ve David Ebbo.

Hata günlüğü için kendi izleme kod yazma alternatif bir açık kaynak günlüğü çerçevesi gibi kullanmaktır [ELMAH](http://nuget.org/packages/elmah/). Daha fazla bilgi için bkz: [Scott Hanselman'ın blog yazılarını hakkında ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Ayrıca, Azure günlüklerinden akış almak istiyorsanız, ASP.NET veya System.Diagnostics izleme kullanmak zorunda değilsiniz unutmayın. Günlük hizmeti akış Azure web uygulaması herhangi akışı sağlanacak *.txt*, *.html*, veya *.log* içinde bulduğu dosya *LogFiles* klasör. Bu nedenle, web uygulamasının dosya sistemine Yazar kendi günlük sistem oluşturabilir ve dosyanızı otomatik olarak akışı ve indirilen. Yapmanız gereken tek şey dosyalarında oluşturur yazma uygulama kodu *d:\home\logfiles* klasör.

### <a name="analyzing-web-server-logs"></a>Analiz etme web sunucu günlükleri
Web sunucusu günlüklerini çözümleme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Web sunucusu günlüklerini verilerini görüntülemek için bir aracı (*.log* dosyaları).
* [IIS performans sorunları veya uygulama LogParser kullanarak hataları giderme](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Web sunucusu günlüklerini çözümlemek için kullanabileceğiniz günlük ayrıştırıcısı aracı giriş.
* [LogParser kullanarak Robert McMurray'tarafından blog yazılarını](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [IIS 7.0, IIS 7.5 ve IIS 8.0 HTTP durum kodu](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Başarısız istek izleme günlüklerini analiz etme
Microsoft TechNet Web sitesi içeren bir [kullanarak başarısız istek izleme](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) nasıl Bu günlükler kullanılacağını anlamak için yararlı olabilecek bölümü. Ancak, bu belge çoğunlukla başarısız istek izleme Azure Web uygulamalarında yapamayacağı IIS Yöneticisi'nde yapılandırma üzerinde durulmaktadır.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki
