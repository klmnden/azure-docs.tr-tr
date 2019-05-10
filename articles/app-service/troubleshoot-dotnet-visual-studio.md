---
title: Visual Studio - Azure App Service'ı kullanarak uygulama sorunlarını giderme
description: Uzaktan hata ayıklama, izleme ve Visual Studio 2013 için yerleşik olarak bulunan günlük Araçları'nı kullanarak bir App Service uygulaması sorunlarını gidermeyi öğrenin.
services: app-service
documentationcenter: .net
author: cephalin
manager: cfowler
editor: ''
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: cbf6a44f1a3210906ec7ab0d04eecb997bc2c470
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412827"
---
# <a name="troubleshoot-an-app-in-azure-app-service-using-visual-studio"></a>Visual Studio kullanarak Azure App Service'te uygulama sorunlarını giderme
## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir uygulamada hata ayıklama yardımcı olmak için Visual Studio araçlarını kullanma gösterilmektedir [App Service](https://go.microsoft.com/fwlink/?LinkId=529714), çalıştırarak [hata ayıklama modu](https://docs.microsoft.com/visualstudio/debugger/) uzaktan veya uygulama günlükleri ve web sunucusu günlüklerini görüntüleyerek.

Şunları öğreneceksiniz:

* Uygulama yönetim işlevleri, Visual Studio'da kullanılabilir.
* Uzak bir uygulamada hızlı değişiklikler yapmak için Visual Studio uzak görünümü kullanma
* Bir proje sırasında uzaktan hata ayıklama modunda çalıştırmak nasıl Azure'da, hem uygulama hem de bir Web işi için çalışıyor.
* Uygulama izleme günlükleri oluşturma ve bunları görüntülemek uygulama çalışırken, bunları oluşturuyor.
* Görüntüleme dahil olmak üzere, web sunucusu günlükleri, ayrıntılı hata iletileri ve başarısız istek izlemeyi.
* Tanılama günlükleri için bir Azure depolama hesabı ve bunları görüntülemek göndermek nasıl.

Visual Studio Ultimate varsa, ayrıca kullanabileceğiniz [IntelliTrace](/visualstudio/debugger/intellitrace) hata ayıklama. Bu öğreticide IntelliTrace kapsamında değildir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici geliştirme ortamı, web projesi ve içinde ayarladığınız App Service uygulaması çalışır [Azure App Service'te bir ASP.NET uygulaması oluşturma](app-service-web-get-started-dotnet-framework.md). WebJobs bölümleri için içinde oluşturduğunuz uygulamayı gerekir [Azure WebJobs SDK ile çalışmaya başlama][GetStartedWJ].

Bu öğreticide gösterilen kod örnekleri bir C# MVC web uygulaması içindir, ancak sorun giderme yordamlarını Visual Basic ve Web Forms uygulamaları için aynıdır.

Bu öğreticide, Visual Studio 2019 kullanmakta olduğunuz varsayılır. 

Akış günlükleri yalnızca .NET Framework 4 veya sonraki sürümlerini hedefleyen uygulamalar için çalışır özellik.

## <a name="sitemanagement"></a>Uygulama Yapılandırması ve Yönetimi
Visual Studio bulunan yapılandırma ayarlarını ve uygulama yönetimi işlevleri bir alt kümesine erişim sağlar [Azure portalında](https://go.microsoft.com/fwlink/?LinkId=529715). Bu bölümde, ne kullanılarak kullanılabilir görürsünüz **Sunucu Gezgini**. En son Azure tümleştirme özellikleri görmek için denemenin **Cloud Explorer** de. Her iki windows açabileceğiniz **görünümü** menüsü.

1. Zaten Visual Studio'da Azure oturumu açmadıysanız, sağ **Azure** Bağlan seçip **Microsoft Azure aboneliği** içinde **Sunucu Gezgini**.

    Hesabınıza erişimi sağlayan bir yönetim sertifikası yüklemek için kullanılan bir alternatiftir. Bir sertifika yüklemeyi seçerseniz, sağ **Azure** düğümünde **Sunucu Gezgini**ve ardından **yönetin ve filtre abonelikleri** bağlam menüsünde. İçinde **Microsoft Azure abonelikleri yönetme** iletişim kutusu, tıklayın **sertifikaları** sekmesine ve ardından **alma**. İndirmek ve ardından bir abonelik dosyası içeri aktarmak için yönergeleri izleyin (olarak da adlandırılan bir *.publishsettings* dosya) Azure hesabınız için.

   > [!NOTE]
   > Bir abonelik dosyası yüklerseniz, kaynak kodu dizinlerinizi (örneğin, indirme klasöründe) dışında bir klasöre kaydedin ve içeri aktarma tamamlandıktan sonra silin. Abonelik dosyasına erişim kazanır kötü niyetli bir kullanıcı düzenleyin, oluşturun ve Azure hizmetlerinizi silin.
   >
   >

    Visual Studio'dan Azure kaynaklarına bağlama hakkında daha fazla bilgi için bkz. [hesaplarını yönetme, abonelikleri ve yönetici rollerini](https://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. İçinde **Sunucu Gezgini**, genişletme **Azure** genişletin **App Service**.
3. Oluşturduğunuz uygulamayı içeren kaynak grubunu genişletin [Azure App Service'te bir ASP.NET uygulaması oluşturma](app-service-web-get-started-dotnet-framework.md)ve ardından uygulama düğümüne sağ tıklatıp **görünüm ayarlarını**.

    ![Sunucu Gezgini'nde ayarlarını görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    **Azure Web uygulaması** sekmesi görünür ve Visual Studio içinde kullanılabilir uygulama yönetimi ve yapılandırma görevlerini burada görebilirsiniz.

    ![Azure Web uygulaması penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    Bu öğreticide, günlüğe kaydetme ve izleme açılan listeler kullanacaksınız. Uzaktan hata ayıklama kullanacaksınız ancak bunu etkinleştirmek için farklı bir yöntem kullanmanız gerekir.

    Bu pencerede uygulama ayarlarının ve bağlantı dizelerinin kutuları hakkında daha fazla bilgi için bkz: [Azure App Service: Nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

    Bu pencerede yapılamaz bir uygulama yönetim görevi gerçekleştirmek isterseniz **Yönetim Portalı'nda açmak** Azure portalında bir tarayıcı penceresi açın.

## <a name="remoteview"></a>Sunucu Gezgini'nde erişim uygulama dosyaları
Genellikle bir web projesi ile dağıttığınız `customErrors` bayrağı ayarlayın Web.config dosyasında `On` veya `RemoteOnly`, yanlış giden anlamına gelen bir yararlı hata iletisi bir şey olduğunda elde etmezsiniz. Birçok hataları size olan aşağıdaki sorguyu biri gibi bir sayfası:

**'/' Uygulamasında sunucu hatası:**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Bir hata oluştu:**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Web sitesi sayfayı görüntüleyemiyor.**

![Faydasız hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Sık en kolay yolu, hatanın nedenini bulmak için yukarıdaki ekran görüntüleri ilk yapmak nasıl yapıldığını açıklayan ayrıntılı hata iletileri etkinleştirmektir. Dağıtılmış Web.config dosyasında değişiklik gerektirir. Düzen *Web.config* dosya projede ve projeyi yeniden dağıtın veya oluşturma bir [Web.config dönüşümü](https://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) ve hata ayıklama derlemesi dağıtmak, ancak daha hızlı bir yolu yoktur: içinde **Çözüm Gezgini** , doğrudan görüntüleyebilir ve kullanarak uzak uygulama dosyalarında düzenlemek *uzak görünümü* özelliği.

1. İçinde **Sunucu Gezgini**, genişletme **Azure**, genişletme **App Service**uygulamanızı bulunan kaynak grubunu genişletin ve ardından uygulamanızı düğümünü genişletin.

    Uygulamanın içerik dosyalarını ve günlük dosyalarını erişmenizi düğümleri görürsünüz.
2. Genişletin **dosyaları** düğüm ve çift *Web.config* dosya.

    ![Web.config dosyasını açın](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio uzak uygulamadan Web.config dosyasını açar ve başlık çubuğunda dosya adının yanında [uzak] gösterir.
3. Aşağıdaki satırı ekleyin `system.web` öğesi:

    `<customErrors mode="Off"></customErrors>`

    ![Edit Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Faydasız hata iletisini gösteren tarayıcıyı yenileyin ve artık aşağıdaki gibi bir ayrıntılı hata iletisi alırsınız:

    ![Ayrıntılı hata iletisi](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (Kırmızı gösterilen satırı ekleyerek gösterilen hata oluşturuldu *Views\Home\Index.cshtml*.)

Web.config dosyasını düzenleme, okuma ve App Service uygulamanızı dosyalarda düzenleme özelliği yapmak daha kolay sorun giderme senaryoları yalnızca bir örneği var.

## <a name="remotedebug"></a>Uzaktan hata ayıklama uygulamaları
Ayrıntılı hata iletisini yeterli bilgi sağlamaz ve yerel olarak hata yeniden oluşturulamıyor, sorunlarını gidermek için başka bir uzaktan hata ayıklama modunda çalıştırmak için yoludur. Kesme noktaları ayarlayın, belleği doğrudan düzenlemezsiniz, kodda adım adım ve hatta kod yolu değiştirebilirsiniz.

Uzaktan hata ayıklama Visual Studio'nun Express sürümlerinde çalışmaz.

Bu bölümde, oluşturduğunuz projenin kullanarak uzaktan hata ayıklamak gösterilmektedir [Azure App Service'te bir ASP.NET uygulaması oluşturma](app-service-web-get-started-dotnet-framework.md).

1. Oluşturduğunuz web projesi açmak [Azure App Service'te bir ASP.NET uygulaması oluşturma](app-service-web-get-started-dotnet-framework.md).

2. Açık *Controllers\HomeController.cs*.

3. Silme `About()` aşağıdaki kodu yerine yöntemi ve ekleme.

``` c#
public ActionResult About()
{
    string currentTime = DateTime.Now.ToLongTimeString();
    ViewBag.Message = "The current time is " + currentTime;
    return View();
}
```

1. [Bir kesme noktası ayarlamak](https://docs.microsoft.com/visualstudio/debugger/) üzerinde `ViewBag.Message` satır.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve **Yayımla**.

1. İçinde **profili** aynı profili, kullanılan aşağı açılan listesinden [Azure App Service'te bir ASP.NET uygulaması oluşturma](app-service-web-get-started-dotnet-framework.md). Ardından, Ayarlar'ı tıklatın.

1. İçinde **Yayımla** iletişim kutusunda, tıklayın **ayarları** sekmesine ve ardından değiştirmek **yapılandırma** için **hata ayıklama**ve ardından  **Kaydet**.

    ![Hata ayıklama modunda yayımlama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)

1. Tıklayın **yayımlama**. Dağıtım tamamlandıktan sonra tarayıcı açılır uygulamanızın Azure URL'sine Tarayıcıyı kapatın.

1. İçinde **Sunucu Gezgini**uygulamanıza sağ tıklayın ve ardından **hata ayıklayıcı iliştirmek**.

    ![Hata ayıklayıcının](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    Tarayıcı, Azure'da çalışan ana sayfanıza otomatik olarak açılır. Azure hata ayıklama sunucusu ayarlar 20 saniye bekleyin gerekebilir. Bu gecikme, yalnızca ilk kez bir 48 saatlik süre içinde bir uygulama üzerinde hata ayıklama modunda çalıştırdığınızda gerçekleşir. Yeniden aynı dönemi içinde hata ayıklamaya başladığınızda, gecikme yoktur.

    > [!NOTE] 
    > Hata ayıklayıcı başlatılıyor herhangi bir sorun varsa, bunu kullanarak yapmaya **Cloud Explorer** yerine **Sunucu Gezgini**.
    >

1. Tıklayın **hakkında** menüsünde.

    Visual Studio kesme noktasına durdurur ve kodu değil, yerel bilgisayarınızda Azure üzerinde çalışıyor.

1. Üzerine `currentTime` zaman değeri görmek için değişkeni.

    ![Azure'da çalışan hata ayıklama modunda görünümü değişkeni](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

    Gördüğünüzde, yerel bilgisayardan farklı bir saat dilimindeki olabilir Azure sunucusu zamandır.

1. İçin yeni bir değer girin `currentTime` "Artık Azure'da çalışıyor" gibi değişken.

1. Çalışmaya devam edebilmesi için F5 tuşuna basın.

     Azure'da çalışan hakkında sayfası currentTime değişkene girdiğiniz yeni değeri görüntüler.

     ![Yeni değerle sayfası hakkında](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a> Uzaktan hata ayıklama WebJobs
Bu bölümde proje ve oluşturduğunuz uygulamanın kullanarak uzaktan hata ayıklama işlemini gösterir [Azure WebJobs SDK ile çalışmaya başlama](https://github.com/Azure/azure-webjobs-sdk/wiki).

Bu bölümde gösterilen özellikler, yalnızca Visual Studio 2013 Update 4 veya daha sonra kullanılabilir.

Uzaktan hata ayıklama yalnızca sürekli WebJobs ile çalışır. Zamanlanmış ve isteğe bağlı Web işleri, hata ayıklamayı desteklemez.

1. Oluşturduğunuz web projesi açmak [Azure WebJobs SDK ile çalışmaya başlama][GetStartedWJ].

2. ContosoAdsWebJob projeyi *Functions.cs*.

3. [Bir kesme noktası ayarlamak](https://docs.microsoft.com/visualstudio/debugger/) ilk deyimde üzerinde `GnerateThumbnail` yöntemi.

    ![kesme noktası Ayarla](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)

4. İçinde **Çözüm Gezgini**, web projesinin (WebJob proje değil) sağ tıklayın ve **Yayımla**.

5. İçinde **profili** aynı profili, kullanılan aşağı açılan listesinden [Azure WebJobs SDK ile çalışmaya başlama](https://github.com/Azure/azure-webjobs-sdk/wiki).

6. Tıklayın **ayarları** sekmesini tıklatıp değiştirme **yapılandırma** için **hata ayıklama**ve ardından **Yayımla**.

    Visual Studio WebJob projeleri ve web dağıtır ve uygulamanız için Azure URL'sini tarayıcınızı açar.

7. İçinde **Sunucu Gezgini**, genişletme **Azure > App Service > kaynak grubunuzun > uygulamanızı > WebJobs > sürekli**ve ardından sağ tıklayarak **ContosoAdsWebJob**.

8. Tıklayın **hata ayıklayıcının**.

    ![Hata ayıklayıcının](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    Tarayıcı, Azure'da çalışan ana sayfanıza otomatik olarak açılır. Azure hata ayıklama sunucusu ayarlar 20 saniye bekleyin gerekebilir. Bu gecikme, yalnızca ilk kez bir 48 saatlik süre içinde bir uygulama üzerinde hata ayıklama modunda çalıştırdığınızda gerçekleşir. Yeniden aynı dönemi içinde hata ayıklamaya başladığınızda, gecikme yoktur.

9. Contoso Ads giriş sayfasına açılan web tarayıcısında, yeni bir ad oluşturun.

    Oluşturma bir ad, webjob'ı tarafından teslim alındı ve işlenen oluşturulması bir kuyruk iletisi neden olur. Ne zaman Web işleri SDK'sı, kesme noktasına kod isabet kuyruk iletisini işlemek için işlevini çağırır.

10. Hata ayıklayıcı, kesme noktasında kesildiğinde inceleyin ve bulut program çalışırken değişken değerleri değiştirin. Aşağıdaki çizimde, hata ayıklayıcı geçildi blobInfo nesnenin içeriğini gösterir. `GenerateThumbnail` yöntemi.

     ![hata ayıklayıcı blobInfo nesnesi](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)

11. Çalışmaya devam edebilmesi için F5 tuşuna basın.

     `GenerateThumbnail` Yöntemi, küçük resim oluşturma işlemi tamamlandığında.

12. Tarayıcıda, dizin sayfasını yenileyin ve küçük resim görürsünüz.

13. Visual Studio'da hata ayıklamayı durdurmak için SHIFT + F5 tuşlarına basın.

14. İçinde **Sunucu Gezgini**ContosoAdsWebJob düğümüne sağ tıklayın ve tıklayın **panoyu görüntüle**.

15. Azure kimlik bilgilerinizle oturum açın ve ardından WebJob adı, WebJob'ınıza sayfasına gitmek için tıklayın.

     ![ContosoAdsWebJob tıklayın](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Gösteren panoyu `GenerateThumbnail` yakın zamanda yürütülen işlevi.

     ('ı sonraki açışınızda **panoyu görüntüle**oturum açmasına gerek yoktur ve tarayıcı WebJob'ınıza doğrudan sayfasına geçer.)

16. İşlev yürütme ayrıntılarını görmek için işlev adına tıklayın.

     ![İşlev ayrıntıları](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

İşlevinizi [günlükleri yazdı](https://github.com/Azure/azure-webjobs-sdk/wiki), tıkladığınız **ToggleOutput** bunları görmek için.

## <a name="notes-about-remote-debugging"></a>Uzaktan hata ayıklama hakkında notlar

* Üretim aşamasında hata ayıklama modunda çalıştırılması önerilmez. Üretim uygulamanızı kullanıma için birden çok sunucu örneği değil ölçeklenirse, hata ayıklama, diğer isteklerine yanıt vermesini web sunucusu engeller. Hata ayıklayıcıyı iliştirmek birden çok web sunucusu örneğine sahip, rastgele bir örneğini almak ve sonraki tarayıcı isteklerini aynı örneğine gittiğinden emin olmak için yolu yoktur. Ayrıca, hata ayıklama derlemesi için üretim genellikle dağıtmanıza gerek yoktur ve sürüm yapıları için derleyici iyileştirmeleri imkansız satır, kaynak kodunda neler olduğunu göstermek yapabileceğiniz. Üretim sorunlarını gidermeye yönelik en iyi kaynak uygulamasıdır izleme ve web sunucusu günlüklerini.
* Uzun kesme noktaları uzak durur önlemek hata ayıklama. Azure, yanıt vermeyen bir işlem olarak birkaç dakikadan fazla durdurulur ve nasıl kapanıyorsa öyle kapanır işlem değerlendirir.
* Hata ayıklarken, sunucu, bant genişliği ücretleri etkileyebilecek Visual Studio için veri gönderiyor. Bant genişliği ücretleri hakkında daha fazla bilgi için bkz. [Azure fiyatlandırma](https://azure.microsoft.com/pricing/calculator/).
* Emin olun `debug` özniteliği `compilation` öğesinde *Web.config* dosya ayarlanmışsa true. Ayarlanmış bir hata ayıklama yapısı yapılandırması yayımladığınızda, varsayılan olarak true.

``` xml
<system.web>
  <compilation debug="true" targetFramework="4.5" />
  <httpRuntime targetFramework="4.5" />
</system.web>
```
* Hata ayıklayıcının hata ayıklamak istediğiniz kodda ilerleyebilmeniz değil olduğunu fark ederseniz, yalnızca kendi kodum ayarı değiştirmeniz gerekebilir.  Daha fazla bilgi için [yalnızca kendi kodum, Visual Studio kullanarak kullanıcı kodunda hata ayıklama Artırılmayacağını](https://docs.microsoft.com/visualstudio/debugger/just-my-code).
* Uzaktan hata ayıklama özelliği etkinleştirmeniz ve 48 saat sonra özelliği otomatik olarak devre dışı Zamanlayıcı sunucuda başlar. Bu 48 saatlik sınırın, güvenlik ve Performans nedeniyle meydana gelir. Bu gibi durumlarda, özellik kolayca istediğiniz geri çok defa kapatabilirsiniz. Değil etkin bir şekilde ayıklarken devre dışı bırakılması önerilir.
* Bu gibi durumlarda, hata ayıklayıcı el ile herhangi bir işlem için yalnızca uygulama işlemi (w3wp.exe) ekleyebilirsiniz. Visual Studio'da hata ayıklama modu kullanma hakkında daha fazla bilgi için bkz. [Visual Studio'da hata ayıklama](/visualstudio/debugger/debugging-in-visual-studio).

## <a name="logsoverview"></a>Tanılama günlüklerine genel bakış
Bir App Service uygulamasında çalışan bir ASP.NET uygulama günlükleri aşağıdaki türlerini oluşturabilirsiniz:

* **Uygulama izleme günlükleri**<br/>
  Yöntemleri çağırarak bu günlükleri uygulamanın oluşturur [System.Diagnostics.Trace](/dotnet/api/system.diagnostics.trace) sınıfı.
* **Web sunucusu günlükleri**<br/>
  Web sunucusu, uygulamayı her HTTP isteği için bir günlük girişi oluşturur.
* **Ayrıntılı hata iletisi günlükleri**<br/>
  Web sunucusu, bazı ek bilgiler başarısız HTTP isteklerini (durum kodu 400 veya üzeri neden istek) ile bir HTML sayfası oluşturur.
* **Başarısız istek izleme günlükleri**<br/>
  Web sunucusu, ayrıntılı izleme bilgilerini başarısız olan HTTP istekleri için bir XML dosyası oluşturur. Web sunucusu, bir tarayıcıda XML biçimlendirme için XSL dosyası da sağlar.

Uygulama performansı etkiler günlüğü, bu nedenle Azure, etkinleştirme veya günlük her türünü gerektiği gibi devre dışı bırakma olanağı sunar. Uygulama günlükleri için yalnızca belirli bir önem düzeyi yukarıda günlükleri yazılması gerektiğini belirtebilirsiniz. Yeni bir uygulama varsayılan olarak tüm günlük oluşturduğunuzda, devre dışı bırakıldı.

Günlük dosyalarına yazılır bir *LogFiles* klasör dosya sistemindeki Uygulama ve FTP aracılığıyla erişilebilir durumdadır. Ayrıca Azure depolama hesabınız için Web sunucusu günlüklerini ve uygulama günlüklerini yazılabilir. Günlükleri bir depolama hesabında dosya sisteminde mümkün olandan daha büyük bir hacmi tutabilirsiniz. Dosya sistemi kullandığınızda en fazla 100 megabaytlık günlükleri sınırlı. (Dosya sistemi yalnızca kısa vadeli bekletme için günlüklerdir. Azure sınıra ulaşıldıktan sonra yenilerini yer açmak için eski günlük dosyaları siler.)  

## <a name="apptracelogs"></a>Oluşturun ve uygulama izleme günlükleri görüntüleyin
Bu bölümde, aşağıdaki görevleri yapın:

* Oluşturduğunuz web projesi izleme deyimleri ekleme [Azure ve ASP.NET kullanmaya başlama](app-service-web-get-started-dotnet-framework.md).
* Projeyi yerel olarak çalıştırdığınızda günlüklerini görüntüleyin.
* Azure'da çalışan uygulama tarafından oluşturulan günlükleri görüntüleyin.

Web işleri günlüklerini nasıl uygulama oluşturulacağı hakkında daha fazla bilgi için bkz. [Azure kuyruk depolama ile çalışmaya nasıl günlükleri yazmak nasıl Web işleri SDK'sı - kullanarak](https://github.com/Azure/azure-webjobs-sdk/wiki). Günlükleri görüntüleme ve Azure'da nasıl depolandıkları denetlemek için aşağıdaki yönergeler, Web işleri tarafından oluşturulan uygulama günlükleri için de geçerlidir.

### <a name="add-tracing-statements-to-the-application"></a>Uygulamayı izleme deyimleri ekleme
1. Açık *Controllers\HomeController.cs*, değiştirin `Index`, `About`, ve `Contact` yöntemleri eklemek için aşağıdaki kod ile `Trace` deyimleri ve `using` for deyimi `System.Diagnostics`:

```c#
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
```

1. Ekleme bir `using System.Diagnostics;` deyimini dosyanın en üstüne.

### <a name="view-the-tracing-output-locally"></a>Yerel olarak izleme çıkışını görüntüleyin
1. Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın.

    Varsayılan izleme dinleyicisi tüm izleme çıktısına Yazar **çıkış** diğer hata ayıklama çıkış penceresi. Aşağıdaki çizim, eklenen izleme deyimleri çıktısında `Index` yöntemi.

    ![Hata ayıklama penceresinde izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    Aşağıdaki adımlarda, hata ayıklama modunda derleme olmadan bir web sayfasında izleme çıkışını görüntülemek gösterilmektedir.
2. Uygulamanın Web.config dosyasını (bir proje klasöründe bulunur) açın ve eklemek bir `<system.diagnostics>` yalnızca kapatmadan önce dosyanın sonuna öğe `</configuration>` öğesi:

``` xml
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
```

`WebPageTraceListener` Görüntülemenizi sağlar izleme çıkış göz atarak `/trace.axd`.
1. Ekleme bir <a href="https://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trace ögesi</a> altında `<system.web>` Web.config dosyasında, aşağıdaki örnek gibi:

``` xml
<trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
```       

1. Uygulamayı çalıştırmak için CTRL+F5'e basın.
1. Tarayıcının adres çubuğuna ekleyin *trace.axd* URL'si ve Enter tuşuna basın (URL benzer `http://localhost:53370/trace.axd`).
1. Üzerinde **uygulama izleme** sayfasında **ayrıntıları** ilk satırdaki (BrowserLink satır değil).

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    **İstek ayrıntıları** sayfası görüntülenirse hem de **izleme bilgilerini** bölümüne eklediğiniz izleme deyimleri çıktısını görürsünüz `Index` yöntemi.

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Varsayılan olarak, `trace.axd` yalnızca yerel olarak kullanılabilir. Bir uzak uygulamasından kullanılabilir hale getirmek isteseydiniz, ekleyebilirsiniz `localOnly="false"` için `trace` öğesinde *Web.config* aşağıdaki örnekte gösterildiği gibi dosya:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Ancak, etkinleştirme `trace.axd` bir üretim ortamında uygulama, güvenlik nedenleriyle önerilmez. Aşağıdaki bölümlerde, App Service uygulama izleme günlükleri okumak için daha kolay bir yolu görürsünüz.

### <a name="view-the-tracing-output-in-azure"></a>Azure'da izleme çıkışını görüntüleyin
1. İçinde **Çözüm Gezgini**, web projesini sağ tıklatıp **Yayımla**.
2. İçinde **Web'i Yayımla** iletişim kutusu, tıklayın **Yayımla**.

    Visual Studio güncelleştirme yayımlar sonra ana sayfanıza bir tarayıcı penceresi açılır (yaramadı Temizle varsayılarak **hedef URL** üzerinde **bağlantı** sekmesi).
3. İçinde **Sunucu Gezgini**, uygulamanıza sağ tıklayıp **akış günlüklerini görüntüle**.

    ![Bağlam menüsündeki akış günlüklerini görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    **Çıkış** penceresi günlük akışı hizmetine bağlandınız ve görüntülemek için bir günlük giden her dakika bildirim satırı ekler gösterir.

    ![Bağlam menüsündeki akış günlüklerini görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. Uygulama giriş sayfası gösteren tarayıcı penceresinde tıklayın **kişi**.

    Birkaç saniye içinde hata düzeyi işlevinin İzleme çıktısı eklediğiniz `Contact` yöntemi görünür **çıkış** penceresi.

    ![Çıktı penceresinde izleme hatası](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Hizmet izleme günlüğü etkinleştirdiğinizde, varsayılan ayarı olduğu için visual Studio hata düzeyi izlemeleri yalnızca göstermez. Yeni bir App Service uygulaması oluşturduğunuzda, önceki ayarlar sayfasını açtığınızda gördüğünüz gibi tüm günlük varsayılan olarak devre dışı bırakılır:

    ![Uygulama oturum kapatma](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Seçiliyken ancak **akış günlüklerini görüntüle**, Visual Studio değiştirmiştir **uygulama Logging(File System)** için **hata**, hata düzeyi günlükleri anlamına gelir Rapor. Tüm izleme günlüklerinizi görmek için bu ayarı değiştirebilirsiniz **ayrıntılı**. Hata düşük bir önem derecesini seçtiğinizde, daha yüksek önem derecesi düzeyleri için tüm günlükleri de raporlanır. Ayrıntılı seçtiğinizde, bu nedenle, aynı zamanda bilgi, uyarı ve Hata günlüklerini görürsünüz.  

5. İçinde **Sunucu Gezgini**uygulamaya sağ tıklayın ve ardından **görünüm ayarlarını** daha önce yaptığınız gibi.
6. Değişiklik **uygulama günlüğü (dosya sistemi)** için **ayrıntılı**ve ardından **Kaydet**.

    ![İzleme düzeyini ayrıntılı olarak ayarlama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
7. Artık gösteren tarayıcı penceresinde, **kişi** sayfasında **giriş**, ardından **hakkında**ve ardından **kişi**.

    Birkaç saniye içinde **çıkış** pencere tüm izleme çıkış gösterir.

    ![Ayrıntılı izleme çıkışı](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    Bu bölümde, etkin ve uygulama ayarlarını kullanarak günlüğe yazma devre dışı. Etkinleştirebilir ve Web.config dosyasını değiştirerek izleme dinleyicilerine devre dışı bırakın. Ancak, Web.config dosyasını değiştirme sırasında uygulama yapılandırması yoluyla günlüğe kaydetmeyi etkinleştirme bunu değil, geri dönüştürmek uygulama etki alanı neden olur. Sorunu yeniden oluşturmak için bir uzun sürerse veya aralıklı, uygulama etki alanı geri dönüştürme "Düzelt" ve yeniden oluşuncaya kadar beklenecek belirlemeye zorlayabilir. Azure'da tanılamayı etkinleştirme, uygulama etki alanı geri dönüştürme olmadan hemen hata bilgilerini yakalama başlatmanıza olanak tanır.

### <a name="output-window-features"></a>Çıkış penceresi özellikleri
**Microsoft Azure günlükleri** sekmesinde **çıkış** penceresinin birkaç düğme ve metin kutusu vardır:

![Düğmeler günlükleri sekmesi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Bu, aşağıdaki işlevleri gerçekleştirir:

* NET **çıkış** penceresi.
* Etkinleştirmek veya sözcük kaydırmayı devre dışı bırakın.
* Başlatın veya günlüklerini İzlemeyi Durdur.
* Hangi günlüklerinin izleneceğini belirt
* Günlükleri indirin.
* Günlükleri bir arama dizesi veya normal bir ifade göre filtreleyin.
* Kapat **çıkış** penceresi.

Visual Studio, bir arama dizesi veya normal ifade girerseniz, istemci günlük bilgileri filtreler. Günlükleri görüntülenecek sonra ölçütleri girebilirsiniz anlamına **çıkış** penceresi değiştirebilirsiniz filtreleme ölçütlerini günlükleri yeniden oluşturmak zorunda kalmadan.

## <a name="webserverlogs"></a>Web sunucusu günlüklerini görüntüle
Web sunucusu günlükleri, uygulama için tüm HTTP etkinliğini kaydeder. Bunları görmek için **çıkış** penceresinde, uygulama için bunları etkinleştirmeli ve bunları izlemek istediğiniz Visual Studio söyleyin.

1. İçinde **Azure Web App yapılandırmasını** gelen açılan sekmesinde **Sunucu Gezgini**, Web sunucusu günlüğü için değiştirme **üzerinde**ve ardından **Kaydet**.

    ![Web sunucusu günlüğü etkinleştir](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. İçinde **çıkış** penceresinde tıklayın **, Microsoft Azure günlüklerinin izleneceğini belirt** düğmesi.

    ![Hangi Azure günlüklerinin izleneceğini belirtin](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. İçinde **Microsoft Azure günlük seçenekleri** iletişim kutusunda **Web sunucu günlükleri**ve ardından **Tamam**.

    ![Web sunucusu günlüklerini izleyin](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. Uygulamayı gösteren tarayıcı penceresinde tıklayın **giriş**, ardından **hakkında**ve ardından **kişi**.

    Uygulama günlükleri genellikle tarafından web sunucusu günlüklerini izleyen ilk görünür. Günlükleri görüntülenecek bir süre beklemeniz gerekebilir.

    ![Çıkış penceresinde Web sunucusu günlükleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Visual Studio kullanarak ilk web sunucusu günlüklerini etkinleştirdiğinizde varsayılan olarak, Azure dosya sistemine günlükler yazar. Alternatif olarak, web sunucusu günlükleri bir depolama hesabındaki blob kapsayıcısına yazılması gerektiğini belirtmek için Azure portalını kullanabilirsiniz.

Yeniden etkinleştirdiğinizde Visual Studio'da günlüğe kaydetme, Visual Studio'da, günlüğe kaydetme, web sunucusu için bir Azure depolama hesabı günlüğü etkinleştirmek için portal'ı kullanın ve ardından devre dışı bırakın, depolama hesabı ayarlarını geri yüklenir.

## <a name="detailederrorlogs"></a>Ayrıntılı hata iletisi günlüklerini görüntüleme
Ayrıntılı hata günlükleri hata yanıt kodları (400 veya üzeri) neden HTTP istekleriyle ilgili bazı ek bilgiler sağlar. Bunları görmek için **çıkış** penceresinde sahip olduğunuz uygulama için bunları etkinleştirmek ve bunları izlemek istediğiniz Visual Studio.

1. İçinde **Azure Web App yapılandırmasını** gelen açılan sekmesinde **Sunucu Gezgini**, değiştirme **ayrıntılı hata iletileri** için **üzerinde**ve ardından tıklayın **Kaydet**.

    ![Ayrıntılı hata iletilerini etkinleştir](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)

2. İçinde **çıkış** penceresinde tıklayın **, Microsoft Azure günlüklerinin izleneceğini belirt** düğmesi.

3. İçinde **Microsoft Azure günlük seçenekleri** iletişim kutusu, tıklayın **tüm günlükler**ve ardından **Tamam**.

    ![Tüm günlüklerini izleyin](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)

4. Tarayıcının adres çubuğunda bir ek karakter bir 404 hatası neden URL'sine ekleyin (örneğin, `http://localhost:53370/Home/Contactx`), ve Enter tuşuna basın.

    Birkaç saniye sonra ayrıntılı hata günlüğünün Visual Studio'da görünür **çıkış** penceresi.

    ![Ayrıntılı hata günlüğü - çıkış penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Control + tarayıcıda biçimlendirilmiş günlük çıktısını görmek için bağlantıya tıklayın:

    ![Ayrıntılı hata günlüğü - tarayıcı penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Dosya sistemi günlükleri indir
İçinde izleyebilirsiniz herhangi bir günlük **çıkış** penceresi de indirilebilir olarak bir *.zip* dosya.

1. İçinde **çıkış** penceresinde tıklayın **akış günlüklerini indir**.

    ![Düğmeler günlükleri sekmesi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Dosya Gezgini açılır, *indirir* seçilen indirilen dosya klasörü.

    ![İndirilen Dosya](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Ayıklama *.zip* dosya ve aşağıdaki klasör yapısına bakın:

    ![İndirilen Dosya](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Uygulama izleme günlükleri bulunduğunuz *.txt* dosyalar *LogFiles\Application* klasör.
   * Web sunucusu günlüklerini bulunduğunuz *.log* dosyalar *LogFiles\http\RawLogs* klasör. Gibi bir araç kullanın [günlük ayrıştırıcısı](https://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) görüntülemek ve bu dosyaları işlemek için.
   * Ayrıntılı hata iletisi günlüklerin içinde *.html* dosyalar *LogFiles\DetailedErrors* klasör.

     ( *Dağıtımları* klasördür yayımlama; kaynak denetimi tarafından oluşturulan dosyaları için Visual Studio yayımlama ile ilgili herhangi bir şey yok. *Git* klasördür kaynak denetimine ilgili izlemeleri için yayımlama ve günlük dosyası akış hizmeti.)  

<!-- ## <a name="storagelogs"></a>View storage logs
Application tracing logs can also be sent to an Azure storage account, and you can view them in Visual Studio. To do that you'll create a storage account, enable storage logs in the Azure portal, and view them in the **Logs** tab of the **Azure Web App** window.

You can send logs to any or all of three destinations:

* The file system.
* Storage account tables.
* Storage account blobs.

You can specify a different severity level for each destination.

Tables make it easy to view details of logs online, and they support streaming; you can query logs in tables and see new logs as they are being created. Blobs make it easy to download logs in files and to analyze them using HDInsight, because HDInsight knows how to work with blob storage. For more information, see **Hadoop and MapReduce** in [Data Storage Options (Building Real-World Cloud Apps with Azure)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

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

     This view shows additional fields you don't see in any other views. This view also enables you to filter logs by using special Query Builder UI for constructing a query. For more information, see Working with Table Resources - Filtering Entities in [Browsing Storage Resources with Server Explorer](https://msdn.microsoft.com/library/ff683677.aspx).
14. To look at the details for a single row, double-click one of the rows.

     ![Trace table in Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)
 -->
## <a name="failedrequestlogs"></a>Başarısız istek izleme günlüklerini görüntüleme
Başarısız istek izleme günlükleri, IIS URL yeniden yazma veya kimlik doğrulama sorunları gibi senaryolarda bir HTTP isteği nasıl işleme ayrıntılarını anlamak ihtiyacınız olduğunda yararlıdır.

App Service uygulamaları, IIS 7.0 kullanılabilir ve sonraki başarısız istek izleme işlevselliği kullanır. Hangi hataların, ancak günlüğe yapılandırmak için IIS ayarlarını için erişiminiz yok. Başarısız istek izlemeyi etkinleştirdiğinizde, tüm hatalar yakalanır.

Visual Studio kullanarak başarısız istek izlemeyi etkinleştirebilirsiniz, ancak Visual Studio'da görüntüleyemezsiniz. Bu günlükler, XML dosyalarıdır. Akış günlüğü hizmeti yalnızca düz metin modunda okunabilir sayılan dosyaları izler: *.txt*, *.html*, ve *.log* dosyaları.

FTP aracılığıyla doğrudan veya bunları yerel bilgisayarınıza indirmek için bir FTP aracını kullanarak yerel olarak sonra bir tarayıcıda, başarısız istek izleme günlükleri görüntüleyebilirsiniz. Bu bölümde, bir tarayıcıda doğrudan görüntüleyebilirsiniz.

1. İçinde **yapılandırma** sekmesinde **Azure Web uygulaması** den açılan pencere **Sunucu Gezgini**, değiştirme **başarısız istek izleme** için **Üzerinde**ve ardından **Kaydet**.

    ![Başarısız istek izlemeyi etkinleştir](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Adres çubuğunda uygulamayı gösteren tarayıcı penceresinin bir ek karakter URL'ye ekleyin ve bir 404 hatası neden için ENTER'a basın.

    Bu oluşturulacak bir başarısız istek izleme günlük neden olur ve aşağıdaki adımları nasıl görüntüleyeceğinizi veya günlüğünü indir gösterin.

3. Visual Studio içinde **yapılandırma** sekmesinde **Azure Web uygulaması** penceresinde tıklayın **açık Yönetim Portalı'nda**.

4. İçinde [Azure portalında](https://portal.azure.com) **ayarları** sayfasında uygulamanız için **dağıtım kimlik bilgileri**ve ardından yeni bir kullanıcı adı ve parolayı girin.

    ![Yeni FTP kullanıcı adı ve parola](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    > [!NOTE]
    > Oturum açtığınızda, tam kullanıcı adı için önek olarak kullanılan uygulama adıyla kullanmak zorunda. Örneğin, bir kullanıcı adı olarak "myId" girin ve "Örneğin" ifadesini site ise, "myexample\myid" oturum açın.
    >

5. Yeni bir tarayıcı penceresinde altında gösterilen URL'yi Git **FTP konak adı** veya **FTPS konak adı** içinde **genel bakış** uygulamanız için sayfa.

6. Daha önce (dahil olmak üzere uygulama adı ön eki için kullanıcı adı) oluşturduğunuz FTP kimlik bilgilerini kullanarak oturum açın.

    Tarayıcı, uygulama kök klasörünü gösterir.

7. Açık *LogFiles* klasör.

    ![LogFiles klasörü aç](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)

8. W3SVC artı bir sayısal değer adlı klasörü açın.

    ![W3SVC klasörü aç](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    Başarısız istek izleme etkinleştirdikten sonra günlüğe kaydedilmiş hataları için XML dosyalarını ve XML biçimlendirmek için bir tarayıcı kullanabileceğiniz XSL dosyası klasör içeriyor.

    ![W3SVC klasörü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)

9. XML dosyası için izleme bilgilerini görmek istediğiniz başarısız istekleri için tıklayın.

    Aşağıdaki çizim bir örnek hata izleme bilgilerini parçası gösterir.

    ![Başarısız istek izlemeyi tarayıcıda](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Sonraki Adımlar
Nasıl Visual Studio, bir App Service uygulaması tarafından oluşturulan günlükleri görüntülemek kolaylaştırır gördünüz. Aşağıdaki bölümler, ilgili konular hakkında daha fazla kaynakların bağlantılarını sağlar:

* App Service'nın sorunlarını giderme
* Visual Studio'da Hata Ayıklama
* Uzaktan Azure'da hata ayıklama
* ASP.NET uygulamalarında izleme
* Web sunucusu günlüklerini analiz etme
* Başarısız istek izleme günlükleri çözümleme
* Bulut hizmetlerinde hata ayıklama

### <a name="app-service-troubleshooting"></a>App Service'nın sorunlarını giderme
Azure App Service'te uygulama sorunlarını giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Uygulamaları izleme](web-sites-monitor.md)
* [Visual Studio 2013 ile Azure App Service'te bellek sızıntılarını araştırma](https://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Yönetilen bellek sorunlarını analiz etmek için Visual Studio özellikleri hakkında Microsoft ALM blog gönderisi.
* [Bilmeniz gereken azure App Service çevrimiçi araçları](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Blog gönderisi Amit Apple tarafından.

Belirli bir sorun giderme sorunuz konusunda yardım için şu forumlarından birinde bir iş parçacığı başlatın:

* [Azure Forumu ASP.NET sitesinde](https://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [MSDN'deki Azure Forumu](https://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](https://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Visual Studio'da Hata Ayıklama
Visual Studio'da hata ayıklama modu kullanma hakkında daha fazla bilgi için bkz. [Visual Studio'da hata ayıklama](/visualstudio/debugger/debugging-in-visual-studio) ve [Visual Studio 2010 ile hata ayıklama ipuçları](https://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Uzaktan Azure'da hata ayıklama
App Service uygulamaları ve WebJobs için uzaktan hata ayıklama hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Uzaktan hata ayıklama Azure App Service'e Giriş](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Uzaktan hata ayıklama içinde uzaktan hata ayıklama Azure App Service bölüm 2 - giriş](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Azure App Service bölüm 3 - çok örnekli ortamına ve GIT üzerinde uzaktan hata ayıklama için giriş](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [WebJobs hata ayıklama (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Bir Azure Web API'si veya Mobile Services arka uç uygulamanızın kullandığı ve gerekiyorsa, hata ayıklamak için bkz: [Visual Studio'da .NET arka ucu hata ayıklama](https://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>ASP.NET uygulamalarında izleme
Internet'te ASP.NET izleme için hiçbir eksiksiz ve güncel tanıtımları vardır. Yapabileceğiniz en iyi olduğu ile çalışmaya başlama MVC yaramadı henüz mevcut ve yeni blog ile ek, Web Forms belirli sorunları odaklanan yazıları için yazılmış eski giriş materyalleri. Aşağıdaki kaynaklar başlatmak için iyi yerler şunlardır:

* [İzleme ve Telemetri (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  E-kitap bölümü Azure bulut uygulamalarında izleme önerileri sunulur.
* [ASP.NET izleme](/previous-versions/dotnet/articles/ms972204(v=msdn.10))<br/>
  Eski ancak yine de konuya temel bir giriş için iyi bir kaynaktır.
* [İzleme dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners)<br/>
  İzleme dinleyicileri hakkında bilgi ancak bahsetmek değil [WebPageTraceListener](/dotnet/api/system.web.webpagetracelistener).
* [İzlenecek yol: ASP.NET izleme System.Diagnostics izleme ile tümleştirme](/previous-versions/b0ectfxd(v=vs.140))<br/>
  Bu makalede ayrıca eski, ancak tanıtım makalede ele alınmamıştır bazı ek bilgiler içerir.
* [ASP.NET MVC Razor görünümleri izleme](https://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Razor görünümleri izleme yanı sıra, post, ayrıca bir MVC uygulamasındaki tüm işlenmeyen özel durumları günlüğe kaydetmek için bir hata filtre oluşturmak nasıl açıklar. Tüm işlenmemiş özel bir Web Forms uygulaması'nda oturum hakkında daha fazla bilgi için Global.asax örneğe bakın [tam bir örnek için hata işleyicilerini](/previous-versions/bb397417(v=vs.140)) MSDN'de. MVC veya Web Forms etkinleştirilmesi için işleme de varsayılan çerçeve sağlar ancak belirli özel durumları günlüğe kaydetmek istiyorsanız, catch ve aşağıdaki örnekte olduğu gibi yeniden oluşturma:

``` c#
try
{
   // Your code that might cause an exception to be thrown.
}
catch (Exception ex)
{
    Trace.TraceError("Exception: " + ex.ToString());
    throw;
}
```

* [Azure komut satırı (artı Glimpse!) günlüğe kaydetme akış Tanılama izleme](https://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Visual Studio'da nasıl hangi Bu öğreticiyi uygulamak için komut satırını kullanmayı gösterir. [Glimpse](https://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) ASP.NET uygulamalarında hata ayıklama için bir araçtır.
* [Web Apps günlüğe kaydetme ve tanılama - David Ebbo ile kullanarak](https://azure.microsoft.com/documentation/videos/azure-web-site-logging-and-diagnostics/) ve [günlükleri - David Ebbo ile Web uygulamalarından akış](https://azure.microsoft.com/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Scott Hanselman ve David Ebbo videosu.

Hata günlüğü için kendi izleme kodu yazmak için alternatif bir açık kaynak günlüğe kaydetme çerçevesi gibi kullanmaktır [ELMAH](https://nuget.org/packages/elmah/). Daha fazla bilgi için [Scott Hanselman'ın blog yazılarını ELMAH hakkında](https://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Ayrıca, ASP.NET kullanmanız gerekmez veya `System.Diagnostics` akış izleme Azure'dan günlüğe kaydeder. Akış günlüğü hizmeti App Service uygulaması tüm akışları *.txt*, *.html*, veya *.log* içinde bulduğu dosya *LogFiles* klasör. Bu nedenle, uygulamanın dosya sistemine yazma kendi günlük sisteminin oluşturabilir ve dosyanızı otomatik olarak akış ve indirildi. Tek yapmanız gereken dosyaları oluşturan yazma uygulama kodu olan *d:\home\logfiles* klasör.

### <a name="analyzing-web-server-logs"></a>Web sunucusu günlüklerini analiz etme
Web sunucusu günlüklerini analiz etme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [LogParser](https://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Web sunucusu günlüklerini verilerini görüntülemek için bir aracı (*.log* dosyaları).
* [IIS performans sorunu veya uygulama LogParser kullanarak hataları giderme](https://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Web sunucusu günlükleri analiz etmek için kullanabileceğiniz günlük ayrıştırıcısı aracı giriş.
* [LogParser kullanarak Robert McMurray'tarafından blog gönderileri](https://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [IIS 7.0, IIS 7.5 ve IIS 8.0 HTTP durum kodu](https://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Başarısız istek izleme günlükleri çözümleme
Microsoft TechNet Web içeren bir [kullanarak başarısız istek izleme](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing) bölümünde, bu günlükleri kullanmayı anlamak için yararlı olabilir. Ancak, bu belge esas olarak başarısız istek izleme Azure App Service'te yapamayacağınız IIS Yöneticisi'nde yapılandırma üzerinde durulmaktadır.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki
