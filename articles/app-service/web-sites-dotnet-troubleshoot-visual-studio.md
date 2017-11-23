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
ms.openlocfilehash: 1e3aff1898665c834a70e6c49f23e408a508b10a
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme
## <a name="overview"></a>Genel Bakış
Bu öğreticide Visual Studio Araçları, bir web uygulamasında hata ayıklama yardımcı olmak için nasıl kullanılacağı gösterilmiştir [uygulama hizmeti](http://go.microsoft.com/fwlink/?LinkId=529714), çalıştırarak [hata ayıklama modu](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) uzaktan veya uygulama ve web sunucu günlükleri görüntüleme.

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

Öğretici, Visual Studio 2017 kullanmakta olduğunuz varsayar. 

Akış günlükleri yalnızca .NET Framework 4 veya sonrasını hedefleyen uygulamalar için çalışır özelliği.

## <a name="sitemanagement"></a>Web Uygulama Yapılandırması ve Yönetimi
Visual Studio web uygulama yönetimi işlevleri ve yapılandırma ayarlarını bulunan alt kümesine erişim sağlar [Azure portal](http://go.microsoft.com/fwlink/?LinkId=529715). Bu bölümde, kullanarak kullanılabilenleri görürsünüz **Sunucu Gezgini**. En son Azure tümleştirme özelliklerini görmek için denemenin **Cloud Explorer** de. Her iki windows açabilirsiniz **Görünüm** menüsü.

1. Zaten Visual Studio'da Azure oturumunuz açık değil, sağ **Azure** ve Bağlan seçin **Microsoft Azure aboneliği** içinde **Sunucu Gezgini**.

    Hesabınıza erişimi sağlayan bir yönetim sertifikası yüklemek için kullanılan bir alternatiftir. Bir sertifika yüklemeyi seçerseniz, sağ **Azure** düğümünde **Sunucu Gezgini**ve ardından **yönetin ve filtre abonelikleri** bağlam menüsünde. İçinde **Microsoft Azure Aboneliklerini Yönet** iletişim kutusu, tıklatın **sertifikaları** sekmesini ve ardından **alma**. Karşıdan yüklemek ve bir abonelik dosyayı içe aktarmak için yönergeleri izleyin (olarak da adlandırılan bir *.publishsettings* dosyası) Azure hesabınız için.

   > [!NOTE]
   > Abonelik dosya indirme, kaynak kodu dizinlerinizi (örneğin, indirme klasöründe) dışında bir klasöre kaydedin ve alma işlemi tamamlandıktan sonra silin. Abonelik dosyasına erişim kazanır kötü niyetli bir kullanıcı düzenleme, oluşturma ve Azure hizmetlerinizi silin.
   >
   >

    Visual Studio'dan Azure kaynaklarına bağlanma hakkında daha fazla bilgi için bkz: [hesaplarını yönetme, abonelikleri ve yönetici rollerini](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. İçinde **Sunucu Gezgini**, genişletin **Azure** ve genişletin **uygulama hizmeti**.
3. Oluşturduğunuz web uygulamasını içeren kaynak grubunu genişletin [Create bir ASP.NET web uygulaması Azure][app-service-web-get-started-dotnet.md], web app düğümünü sağ tıklatın ve'ı tıklatın **görünüm ayarlarını**.

    ![Sunucu Gezgininde görünümü ayarları](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    **Azure Web uygulaması** sekmesi görüntülenir ve Visual Studio'da bulunan web uygulaması yönetim ve yapılandırma görevleri vardır görebilirsiniz.

    ![Azure Web uygulaması penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    Bu öğreticide, günlüğe kaydetme ve izleme aşağı açılan listeler kullanacaksınız. Uzaktan hata ayıklama kullanacağınız ancak etkinleştirmek için farklı bir yöntem kullanmanız.

    Bu pencere uygulama ayarlarının ve bağlantı dizelerinin kutularında hakkında daha fazla bilgi için bkz: [Azure Web Apps: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

    Bu pencerede yapılamaz bir web uygulaması yönetim görevi gerçekleştirmek istiyorsanız, tıklayın **Yönetim Portalı'nda açmak** Azure portalına bir tarayıcı penceresi açın.

## <a name="remoteview"></a>Server Explorer'da erişim web uygulama dosyaları
Genellikle bir web projesi ile dağıttığınız `customErrors` bayrağı ayarlamak Web.config dosyasında `On` veya `RemoteOnly`, yanlış giden anlamına yararlı hata iletisi bir şey olduğunda elde etmezsiniz. Birçok hata için size tek şey sayfası aşağıdaki dosyalardan birini gibi:

**'/' Uygulamasında sunucu hatası:**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Bir hata oluştu:**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Web sayfasını görüntüleyemiyor**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Sık hatanın nedenini bulmak için en kolay yolu önceki ekran görüntüleri ilk nasıl yapılacağını açıklar ayrıntılı hata iletileri sağlamaktır. Dağıtılmış Web.config dosyasında değişiklik gerektirir. Düzen *Web.config* dosya projede ve projeyi yeniden dağıtın veya oluşturma bir [Web.config dönüştürmesi](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) ve hata ayıklama derlemesi dağıtmak, ancak daha hızlı bir yolu yoktur: içinde **Çözüm Gezgini** , doğrudan görüntüleyin ve kullanarak dosyaları uzak web uygulamasında düzenleyin *uzak görünümü* özelliği.

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

Bu bölümde oluşturduğunuz proje kullanarak uzaktan hata ayıklama gösterilmektedir [Azure][app-service-web-get-started-dotnet.md içinde bir ASP.NET web uygulaması oluştur].

1. Oluşturduğunuz web projesini açın [Azure][app-service-web-get-started-dotnet.md içinde bir ASP.NET web uygulaması oluştur].

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

6. İçinde **profil** aynı profil, kullanılan aşağı açılan listesinden, [Azure][app-service-web-get-started-dotnet.md içinde bir ASP.NET web uygulaması oluştur]. Ardından, Ayarlar'ı tıklatın.

7. İçinde **Yayımla** iletişim kutusunda, tıklatın **ayarları** sekmesini ve sonra değiştirmek **yapılandırma** için **hata ayıklama**ve ardından  **Kaydet**.

    ![Hata ayıklama modunda yayımlama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)

8. **Yayımla**’ta tıklayın. Dağıtımdan sonra biter ve tarayıcınız, web uygulamanızın Azure URL'sine açıldığında, tarayıcıyı kapatın.

9. İçinde **Sunucu Gezgini**, web uygulamanızı sağ tıklayın ve ardından **Attach hata ayıklayıcı**.

    ![Hata ayıklayıcıyı Ekle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    Tarayıcı, Azure'da çalışan giriş sayfanız için otomatik olarak açılır. Hata ayıklama için sunucusu Azure ayarlar 20 saniye veya bunu bekleyin gerekebilir. Bu gecikme, yalnızca ilk kez 48 saatlik süre içinde bir web uygulamasında hata ayıklama modunda çalıştırdığınızda olur. Hata ayıklama aynı dönem yeniden başlattığınızda, gecikme yoktur.

    > [!NOTE] 
    > Hata ayıklayıcıyı başlatma herhangi konusunda sorun yaşıyorsanız, kullanarak yapmak için deneyin **Cloud Explorer** yerine **Sunucu Gezgini**.
    >

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

7. İçinde **Sunucu Gezgini**, genişletin **Azure > uygulama hizmeti > kaynak grubunuz > web uygulamanızı > WebJobs > sürekli**ve ardından sağ **ContosoAdsWebJob**.

8. Tıklatın **hata ayıklayıcısını**.

    ![Hata ayıklayıcıyı Ekle](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    Tarayıcı, Azure'da çalışan giriş sayfanız için otomatik olarak açılır. Hata ayıklama için sunucusu Azure ayarlar 20 saniye veya bunu bekleyin gerekebilir. Bu gecikme, yalnızca ilk kez 48 saatlik süre içinde bir web uygulamasında hata ayıklama modunda çalıştırdığınızda olur. Hata ayıklama aynı dönem yeniden başlattığınızda, gecikme yoktur.

9. Contoso Ads giriş sayfasına açılan web tarayıcısında yeni bir ad oluşturun.

    Bir ad oluşturma tarafından WebJob toplanmış ve işlenen oluşturulması bir kuyruk iletisi neden olur. Ne zaman WebJobs SDK isabetini kod isabet kuyruk iletiyi işlemek için işlevi çağırır.

10. Hata ayıklayıcı kesme noktasında böldüğünde inceleyin ve program bulut çalışırken değişken değerlerini değiştirin. Aşağıdaki çizimde, hata ayıklayıcı geçirilmedi blobInfo nesnesinin içeriğini gösterir `GenerateThumbnail` yöntemi.

     ![hata ayıklayıcı blobInfo nesnesinde](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)

11. Çalışmaya devam edebilmesi için F5 tuşuna basın.

     `GenerateThumbnail` Yöntemi tamamlandıktan küçük resim oluşturma.

12. Tarayıcıda, dizin sayfasını yenileyin ve küçük resim bakın.

13. Visual Studio'da hata ayıklamayı durdurmak için SHIFT + F5 tuşuna basın.

14. İçinde **Sunucu Gezgini**, ContosoAdsWebJob düğümünü sağ tıklatın ve **görünüm Pano**.

15. Azure kimlik bilgilerinizle oturum ve, WebJob için sayfaya gitmek için Web işi adı'ye tıklayın.

     ![ContosoAdsWebJob'ı tıklatın](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Pano gösterir `GenerateThumbnail` son yürütülen işlevi.

     (Bir sonraki tıklattığınızda **görünüm Pano**, oturum açmak zorunda değilsiniz ve tarayıcı, WebJob için doğrudan sayfasına gider.)

16. İşlev yürütme ayrıntılarını görmek için işlev adına tıklayın.

     ![İşlev ayrıntıları](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

İşlevinizi [günlükleri yazdı](https://github.com/Azure/azure-webjobs-sdk/wiki), tıklatın **ToggleOutput** bunları görmek için.

## <a name="notes-about-remote-debugging"></a>Uzaktan hata ayıklama hakkında notlar

* Hata ayıklama modunda üretimde çalışan önerilmez. Üretim web uygulamanız çıkışı için birden fazla sunucu örneğinin ölçeklenmez, hata ayıklama, diğer isteklere yanıt web sunucusu engeller. Hata ayıklayıcı için taktığınızda birden çok web sunucusu örneğine sahip, rastgele bir örneği elde ve sonraki tarayıcı istekleri aynı örneğine gidin sağlamanın bir yolu yoktur. Ayrıca, genellikle üretim için hata ayıklama derlemesi dağıtmak yok ve yayın derlemeleri derleyici iyileştirmelerini satır kaynak kodunuzda neler olduğunu göstermek imkansız yapabilir. Üretim ilgili sorunları gidermek için en iyi uygulama kaynaktır izleme ve web sunucu günlükleri.
* Kesme noktaları uzak uzun durur kaçının hata ayıklama. Azure, yanıt vermeyen bir işlem olarak birkaç dakikadan uzun süre için durduruldu ve öyle kapanır bir işlem değerlendirir.
* Hatalarını ayıkladığınız sırada sunucu bant genişliği ücretleri etkileyebilecek Visual Studio için veri gönderiyor. Bant genişliği oranları hakkında daha fazla bilgi için bkz: [Azure fiyatlandırma](https://azure.microsoft.com/pricing/calculator/).
* Olduğundan emin olun `debug` özniteliği `compilation` öğesinde *Web.config* dosya ayarlanmış true. Ayarlanmış bir hata ayıklama yapı yapılandırması yayımladığınızda, varsayılan olarak true.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Hata ayıklayıcı hata ayıklamak istediğiniz koda adım değil bulursanız, sadece kendi kodumu ayarını değiştirmeniz gerekebilir.  Daha fazla bilgi için bkz: [sadece kendi kodumu atlama sınırla](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Uzaktan hata ayıklama özelliği etkinleştirmek ve 48 saat sonra özelliği otomatik olarak devre dışı bir süreölçer sunucuda başlar. Bu 48 saat sınır güvenlik ve performans nedenleriyle yapılır. İstediğiniz şekilde geri sayıda saatlerinin özelliği kolayca kapatabilirsiniz. Değil etkin olarak ayıklarken devre dışı bırakarak öneririz.
* Hata ayıklayıcı herhangi bir işlem için yalnızca web uygulaması işleminin (w3wp.exe) el ile ekleyebilirsiniz. Visual Studio'da hata ayıklama modunu kullanma hakkında daha fazla bilgi için bkz: [Visual Studio'da hata ayıklamayı](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Tanılama günlükleri'ne genel bakış
Bir Azure web uygulamasında çalışan bir ASP.NET uygulaması günlükleri aşağıdaki türlerini oluşturabilirsiniz:

* **Uygulama izleme günlükleri**<br/>
  Uygulama yöntemlerini çağırarak bu günlükler oluşturur [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) sınıfı.
* **Web sunucu günlükleri**<br/>
  Web sunucusu web uygulaması her HTTP isteği için bir günlük girişi oluşturur.
* **Ayrıntılı hata iletisi günlükleri**<br/>
  Web sunucusu HTML sayfası bazı ek bilgiler başarısız HTTP isteklerini (durum kodu 400 veya daha büyük sonuç istek) oluşturur.
* **İstek izleme günlüklerini başarısız oldu**<br/>
  Web sunucusu, başarısız olan HTTP istekleri için ayrıntılı izleme bilgilerini içeren bir XML dosyası oluşturur. Web sunucusu, aynı zamanda bir tarayıcıda XML biçimine XSL dosyasını sağlar.

Azure etkinleştirmek veya her günlük türü gerektiği gibi devre dışı olanağı sağlar şekilde günlük web uygulama performansını etkiler. Uygulama günlükleri için yalnızca belirli bir önem düzeyi yukarıda günlükleri yazılması belirtebilirsiniz. Yeni bir web uygulaması varsayılan olarak tüm günlük oluştururken devre dışı bırakılır.

Günlükleri dosyalarına yazılır bir *LogFiles* web uygulamasını ve FTP aracılığıyla erişilebilir olan dosya sistemi klasöründe. Ayrıca bir Azure depolama hesabı, Web sunucusu ve uygulama günlükleri yazılabilir. Büyük bir dosya sistemini de mümkün olandan bir depolama hesabındaki günlükler hacmi tutabilirsiniz. Dosya sistemi kullandığınızda en fazla 100 megabaytlık günlükler için sınırlı. (Yalnızca kısa vadeli bekletme için dosya sistemi günlükleri içindir. Azure sınıra ulaşıldıktan sonra yeni değerler için yer açmak için eski günlük dosyaları siler.)  

## <a name="apptracelogs"></a>Oluşturma ve uygulama izleme günlüklerini görüntüle
Bu bölümde, aşağıdaki görevleri yapın:

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
5. Tarayıcının adres çubuğunda eklemek *trace.axd* URL ve (URL için http://localhost:53370/trace.axd benzer) Enter tuşuna basın.
6. Üzerinde **uygulama izleme** sayfasında, **ayrıntıları görüntüle** ilk satırda (BrowserLink satır değil).

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    **İstek ayrıntıları** sayfası görüntülenirse hem de **izleme bilgilerini** eklediğiniz izleme deyimleri çıktısını gördüğünüz bölüm `Index` yöntemi.

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Varsayılan olarak, `trace.axd` yalnızca yerel olarak kullanılabilir. Uzak web uygulamasından kullanılabilmesini isteseydiniz, ekleyebilirsiniz `localOnly="false"` için `trace` öğesinde *Web.config* dosya, aşağıdaki örnekte gösterildiği gibi:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Ancak, etkinleştirme `trace.axd` bir üretim web uygulaması güvenlik nedeniyle önerilmez. Aşağıdaki bölümlerde, bir Azure web uygulamasında izleme günlüklerini okumak için daha kolay bir yolu görürsünüz.

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

    Bu bölümde, etkin ve Azure web uygulaması ayarları kullanarak günlük kaydını devre dışı. Ayrıca, etkinleştirmek ve Web.config dosyasını değiştirerek izleme dinleyicileri devre dışı bırakın. Ancak, Web.config dosyasında değişiklik günlüğünü web uygulama yapılandırması aracılığıyla etkinleştirme, değil yaparken, geri dönüştürmek uygulama etki alanı neden olur. Sorunu yeniden oluşturmak için bir uzun sürüyorsa veya aralıklı, uygulama etki alanı geri dönüştürme "Düzelt" ve yeniden işlem yapılana kadar beklenecek zorlayabilir. Azure tanılama etkinleştirme, uygulama etki alanı geri dönüştürme olmadan hemen hata bilgilerini yakalama başlatmanızı sağlar.

### <a name="output-window-features"></a>Çıktı penceresi özellikleri
**Microsoft Azure günlükleri** sekmesinde **çıkış** penceresinde birkaç düğmeler ve metin kutusu vardır:

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
Web sunucu günlükleri, web uygulaması için tüm HTTP etkinliğini kaydeder. Bunları görmek için **çıkış** penceresinde, web uygulaması için etkinleştirmeli ve bunları izlemek istediğiniz Visual Studio söyleyin.

1. İçinde **Azure Web uygulaması yapılandırmasında** gelen açtığınız sekmesini **Sunucu Gezgini**, Web Server günlüğü için değiştirme **üzerinde**ve ardından **kaydetmek**.

    ![Web sunucusu günlüğü etkinleştirme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. İçinde **çıkış** penceresinde tıklatın **hangi Microsoft Azure günlüklerinin izleneceğini belirt** düğmesi.

    ![Hangi Azure günlüklerinin izleneceğini belirt](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. İçinde **Microsoft Azure günlük seçenekleri** iletişim kutusunda **Web sunucu günlükleri**ve ardından **Tamam**.

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

2. İçinde **çıkış** penceresinde tıklatın **hangi Microsoft Azure günlüklerinin izleneceğini belirt** düğmesi.

3. İçinde **Microsoft Azure günlük seçenekleri** iletişim kutusu, tıklatın **tüm günlükleri**ve ardından **Tamam**.

    ![Tüm günlükler izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)

4. Tarayıcının adres çubuğunda fazladan bir karakter 404 hatası neden URL'si ekleyin (örneğin, `http://localhost:53370/Home/Contactx`), ve Enter tuşuna basın.

    Birkaç saniye sonra Visual Studio'da ayrıntılı hata günlüğü görünür **çıkış** penceresi.

    ![Ayrıntılı hata günlüğü - çıktı penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Control + bir tarayıcıda biçimlendirilmiş günlük çıkışı görmek için bağlantıya tıklayın:

    ![Ayrıntılı hata günlüğü - bir tarayıcı penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

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

<!-- ## <a name="storagelogs"></a>View storage logs
Application tracing logs can also be sent to an Azure storage account, and you can view them in Visual Studio. To do that you'll create a storage account, enable storage logs in the Azure portal, and view them in the **Logs** tab of the **Azure Web App** window.

You can send logs to any or all of three destinations:

* The file system.
* Storage account tables.
* Storage account blobs.

You can specify a different severity level for each destination.

Tables make it easy to view details of logs online, and they support streaming; you can query logs in tables and see new logs as they are being created. Blobs make it easy to download logs in files and to analyze them using HDInsight, because HDInsight knows how to work with blob storage. For more information, see **Hadoop and MapReduce** in [Data Storage Options (Building Real-World Cloud Apps with Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

You currently have file system logs set to verbose level; the following steps walk you through setting up information level logs to go to storage account tables. Information level means all logs created by calling `Trace.TraceInformation`, `Trace.TraceWarning`, and `Trace.TraceError` will be displayed, but not logs created by calling `Trace.WriteLine`.

Storage accounts offer more storage and longer-lasting retention for logs compared to the file system. Another advantage of sending application tracing logs to storage is that you get some additional information with each log that you don't get from file system logs.

1. Right-click **Storage** under the Azure node, and then click **Create Storage Account**.

![Create Storage Account](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. In the **Create Storage Account** dialog, enter a name for the storage account.

    The name must be must be unique (no other Azure storage account can have the same name). If the name you enter is already in use you'll get a chance to change it.

    The URL to access your storage account will be *{name}*.core.windows.net.
2. Set the **Region or Affinity Group** drop-down list to the region closest to you.

    This setting specifies which Azure datacenter will host your storage account. For this tutorial your choice won't make a noticeable difference, but for a production web app you want your web server and your storage account to be in the same region to minimize latency and data egress charges. The web app (which you'll create later) should run in a region as close as possible to the browsers accessing your web app in order to minimize latency.
3. Set the **Replication** drop-down list to **Locally redundant**.
   
    When geo-replication is enabled for a storage account, the stored content is replicated to a secondary datacenter to enable failover to that location in case of a major disaster in the primary location. Geo-replication can incur additional costs. For test and development accounts, you generally don't want to pay for geo-replication. For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).
4. Click **Create**.

    ![New storage account](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. In the Visual Studio **Azure Web App** window, click the **Logs** tab, and then click **Configure Logging in Management Portal**.

     <!-- todo:screenshot of new portal if the VS page link goes to new portal -- >
    ![Configure logging](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    This opens the **Configure** tab in the portal for your web app.
6. In the portal's **Configure** tab, scroll down to the application diagnostics section, and then change **Application Logging (Table Storage)** to **On**.
7. Change **Logging Level** to **Information**.
8. Click **Manage Table Storage**.

    ![Click Manage TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    In the **Manage table storage for application diagnostics** box, you can choose your storage account if you have more than one. You can create a new table or use an existing one.

    ![Manage table storage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. In the **Manage table storage for application diagnostics** box, click the check mark to close the box.
10. In the portal's **Configure** tab, click **Save**.
11. In the browser window that displays the application web app, click **Home**, then click **About**, and then click **Contact**.

     The logging information produced by browsing these web pages is written to the storage account.
12. In the **Logs** tab of the **Azure Web App** window in Visual Studio, click **Refresh** under **Diagnostic Summary**.

     ![Click Refresh](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     The **Diagnostic Summary** section shows logs for the last 15 minutes by default. You can change the period to see more logs.

     (If you get a "table not found" error, verify that you browsed to the pages that do the tracing after you enabled **Application Logging (Storage)** and after you clicked **Save**.)

     ![Storage logs](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Notice that in this view you see **Process ID** and **Thread ID** for each log, which you don't get in the file system logs. You can see additional fields by viewing the Azure storage table directly.
13. Click **View all application logs**.

     The trace log table appears in the Azure storage table viewer.

     (If you get a "sequence contains no elements" error, open **Server Explorer**, expand the node for your storage account under the **Azure** node, and then right-click **Tables** and click **Refresh**.)

     ![Storage logs in table view](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     This view shows additional fields you don't see in any other views. This view also enables you to filter logs by using special Query Builder UI for constructing a query. For more information, see Working with Table Resources - Filtering Entities in [Browsing Storage Resources with Server Explorer](http://msdn.microsoft.com/library/ff683677.aspx).
14. To look at the details for a single row, double-click one of the rows.

     ![Trace table in Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)
 -->
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

4. İçinde [Azure portal](https://portal.azure.com) **ayarları** sayfasında web uygulamanız için **dağıtım kimlik bilgileri**ve ardından yeni bir kullanıcı adı ve parolayı girin.

    ![Yeni FTP kullanıcı adı ve parola](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    > [!NOTE]
    > Oturum açtığınızda, kendisine önekli web uygulaması adı ile tam kullanıcı adını kullanmak zorunda. Örneğin, "myId" bir kullanıcı adı girin ve "Örneğin" ifadesini site ise, "myexample\myid" oturum açın.
    >

5. Altında gösterilen URL'yi yeni bir tarayıcı penceresine gidin **FTP ana bilgisayar adı** veya **FTPS konak adı** içinde **genel bakış** sayfasında web uygulamanız için.

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
Visual Studio'da hata ayıklama modunu kullanma hakkında daha fazla bilgi için bkz: [Visual Studio'da hata ayıklamayı](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) ve [Visual Studio 2010 ile hata ayıklama ipuçları](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

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
  Bu makalede ayrıca eski olduğundan, ancak giriş makalesi kapsamıyordur bazı ek bilgiler içerir.
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

Ayrıca, ASP.NET kullanmanız gerekmez veya `System.Diagnostics` akış izleme Azure'dan günlüğe kaydeder. Günlük hizmeti akış Azure web uygulaması herhangi akışları *.txt*, *.html*, veya *.log* içinde bulduğu dosya *LogFiles* klasör. Bu nedenle, web uygulamasının dosya sistemine Yazar kendi günlük sistem oluşturabilir ve dosyanızı otomatik olarak akışı indirilir ve. Yapmanız gereken tek şey dosyalarında oluşturur yazma uygulama kodu *d:\home\logfiles* klasör.

### <a name="analyzing-web-server-logs"></a>Analiz etme web sunucu günlükleri
Web sunucusu günlüklerini çözümleme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Web sunucusu günlüklerini verilerini görüntülemek için bir aracı (*.log* dosyaları).
* [IIS performans sorunları veya uygulama LogParser kullanarak hataları giderme](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Web sunucusu günlüklerini çözümlemek için kullanabileceğiniz günlük ayrıştırıcısı aracı giriş.
* [LogParser kullanarak Robert McMurray'tarafından blog yazılarını](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [IIS 7.0, IIS 7.5 ve IIS 8.0 HTTP durum kodu](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Başarısız istek izleme günlüklerini analiz etme
Microsoft TechNet Web sitesi içeren bir [kullanarak başarısız istek izleme](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) bölümünde nasıl Bu günlükler kullanılacağını anlamak için yararlı olabilir. Ancak, bu belge çoğunlukla başarısız istek izleme Azure Web uygulamalarında yapamayacağı IIS Yöneticisi'nde yapılandırma üzerinde durulmaktadır.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki
