---
title: "Bir Azure bulut hizmeti Azure CDN ile tümleştirmek | Microsoft Docs"
description: "Tümleşik bir Azure CDN uç noktasından içerik sunan bir bulut hizmeti dağıtmayı öğrenin"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f2849fe25fd0d5b3dc26598ffba7591cb7433161
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="intro"></a>Bir bulut hizmeti Azure CDN ile tümleştirme
Bir bulut hizmeti, bulut hizmetinin konumundan içerik sunan Azure CDN ile tümleştirilebilir. Bu yaklaşım, aşağıdaki avantajları sunar:

* Kolayca dağıtın ve görüntüleri, komut dosyalarını ve stil sayfalarını bulut hizmetinizin proje dizinlerde güncelleştir
* JQuery veya önyükleme sürümleri gibi bulut hizmetinizde NuGet paketlerini kolayca yükseltme
* Web uygulamanız ve, CDN sunulan içerik tümü aynı Visual Studio arabiriminden yönetmek
* Web uygulamanız ve CDN sunulan içeriğiniz için birleşik dağıtım iş akışı
* ASP.NET paketleme ve küçültme Azure CDN ile tümleştirme

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz
Bu öğreticide şunları öğreneceksiniz nasıl yapılır:

* [Azure CDN uç bulut hizmetiniz ile bütünleşir ve statik içerik Web sayfalarınıza Azure CDN hizmet](#deploy)
* [Bulut hizmetinizde statik içeriği için önbellek ayarlarını yapılandırın](#caching)
* [Denetleyici eylemleri Azure CDN aracılığıyla içerikten hizmet](#controller)
* [Hizmet vermemesini gruplanır ve Visual Studio deneyimi ayıklamasını korurken Azure CDN içeriğinden küçültülmüş](#bundling)
* [Azure CDN çevrimdışı olduğunda geri dönüş komut dosyaları ve CSS yapılandırın.](#fallback)

## <a name="what-you-will-build"></a>Yapı
Varsayılan ASP.NET MVC şablonu kullanarak bir bulut hizmeti Web rolü dağıtacağınızı, görüntü, denetleyici eylem sonuçlarını ve varsayılan JavaScript ve CSS dosyaları gibi tümleşik bir Azure CDN gelen içerik sunmak için kodu ekleyin ve ayrıca geri dönüş yapılandırmak için kod yazma CDN çevrimdışıysa gerektiğinde, paket için mekanizma sundu.

## <a name="what-you-will-need"></a>İhtiyacınız olacak
Bu öğretici aşağıdaki önkoşullar vardır:

* Etkin bir [Microsoft Azure hesabı](/account/)
* Visual Studio 2015 ile birlikte [Azure SDK'sı](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir:
> 
> * Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Web siteleri gibi Azure hizmetlerini kullanabilirsiniz.
> * Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a>Bir bulut hizmeti dağıtma
Bu bölümde, bir bulut hizmeti Web rolü için varsayılan ASP.NET MVC uygulama şablonu Visual Studio 2015'te dağıtın ve yeni bir CDN uç noktası ile tümleştirin. Aşağıdaki yönergeleri izleyin:

1. Visual Studio 2015'te, giderek menü çubuğundan yeni bir Azure bulut hizmeti oluşturma **Dosya > Yeni > Proje > bulut > Azure bulut hizmeti**. Bir ad verin ve tıklatın **Tamam**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. Seçin **ASP.NET Web rolü** tıklatıp  **>**  düğmesi. Tamam'a tıklayın.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. Seçin **MVC** tıklatıp **Tamam**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. Şimdi, bu Web rolü için bir Azure bulut hizmeti yayımlayın. Bulut hizmeti projesine sağ tıklatın ve **Yayımla**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. Henüz Microsoft Azure'da oturum değil,'ı tıklatın **Hesap Ekle...**  açılır tıklatıp **Hesap Ekle** menü öğesi.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. Oturum açma sayfası Azure hesabınızı etkinleştirmek için kullanılan Microsoft hesabıyla oturum açın.
7. Oturum açtınız sonra tıklayın **sonraki**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. Bir bulut hizmeti veya depolama hesabı oluşturmadıysanız varsayılarak, Visual Studio hem de oluşturmanıza yardımcı olur. İçinde **bulut hizmeti oluşturun ve hesabı** iletişim kutusunda, istenen hizmet adını yazın ve istediğiniz bölgeyi seçin. Sonra, **Oluştur**’a tıklayın.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. Yayımlama Ayarları sayfasında yapılandırmasını doğrulayın ve **Yayımla**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > Bulut Hizmetleri için yayımlama işlemi çok uzun sürüyor. Web dağıtımını etkinleştirin tüm rolleri seçeneği için Web rolleri (ancak geçici) hızlı güncelleştirmeleri sağlayarak bulut hizmetiniz çok daha hızlı hata ayıklama yapabilirsiniz. Bu seçenek hakkında daha fazla bilgi için bkz: [yayımlama Azure araçlarını kullanarak bir bulut hizmeti](http://msdn.microsoft.com/library/ff683672.aspx).
   > 
   > 
   
    Zaman **Microsoft Azure etkinlik günlüğü** yayımlama durumu gösterir **tamamlandı**, bulut hizmeti ile tümleşik bir CDN uç noktası oluşturur.
   
   > [!WARNING]
   > Yayımladıktan sonra dağıtılan bulut hizmeti hata ekranı görüntüler varsa, dağıttığınız bulut hizmeti tarafından kullanıldığından, büyük olasılıkla bir [konuk .NET 4.5.2 içermez OS](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Bu sorundan getirebilirler [.NET 4.5.2 bir başlangıç görevi olarak dağıtma](../cloud-services/cloud-services-dotnet-install-dotnet.md).
   > 
   > 

## <a name="create-a-new-cdn-profile"></a>Yeni bir CDN profili oluşturma
CDN profili, CDN uç noktaları koleksiyonudur.  Her bir profil, bir veya daha fazla CDN uç noktası içerir.  CDN uç noktalarınızı İnternet etki alanı, web uygulaması veya başka ölçütlere göre düzenlemek için birden çok profil kullanmak isteyebilirsiniz.

> [!TIP]
> Bu öğretici için kullanmak istediğiniz bir CDN profili varsa, devam [yeni bir CDN uç noktası oluşturma](#create-a-new-cdn-endpoint).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Yeni bir CDN uç noktası oluşturma
**Depolama hesabınız için yeni bir CDN uç noktası oluşturmak için**

1. İçinde [Azure Yönetim Portalı](https://portal.azure.com), CDN profilinize gidin.  Önceki adımda bunu panoya sabitlemiş olabilirsiniz.  Sabitlemediyseniz bunu bulmak için **Gözat**'a, ardından **CDN profilleri**'ne ve uç noktanızı eklemeyi planladığınız profile tıklayabilirsiniz.
   
    CDN profili dikey penceresi görünür.
   
    ![CDN profili][cdn-profile-settings]
2. **Uç Nokta Ekle** düğmesine tıklayın.
   
    ![Uç nokta ekle düğmesi][cdn-new-endpoint-button]
   
    **Uç nokta ekleme** dikey penceresi görünür.
   
    ![Uç nokta ekleme dikey penceresi][cdn-add-endpoint]
3. Bu CDN uç noktası için bir **Ad** girin.  Bu ad, `<EndpointName>.azureedge.net` etki alanındaki önbelleğe alınmış kaynaklarınıza erişmek için kullanılır.
4. İçinde **kaynak türü** açılan listesinde, select *bulut hizmeti*.  
5. İçinde **kaynak ana bilgisayar adı** açılan listesinde, bulut hizmetinizi seçin.
6. Varsayılan değerleri bırakın **kaynak yolu**, **kaynak ana bilgisayar üstbilgisi**, ve **Protokolü/kaynak bağlantı noktası**.  En az bir protokol (HTTP veya HTTPS) belirtmeniz gerekir.
7. Yeni uç noktayı oluşturmak için **Ekle** düğmesine tıklayın.
8. Uç nokta oluşturulduktan sonra, profile yönelik uç noktalar listesinde görünür. Liste görünümünde, kaynak etki alanının yanı sıra, önbelleğe alınan içeriğe erişmek için kullanılacak URL gösterilir.
   
    ![CDN uç noktası][cdn-endpoint-success]
   
   > [!NOTE]
   > Uç nokta hemen kullanılabilir olmaz.  CDN ağ üzerinden yaymak kayıt 90 dakika kadar sürebilir. İçerik CDN kullanılabilir hale gelene kadar hemen CDN etki alanı adını kullanmayı deneyen kullanıcılar durum kodu 404 alabilirsiniz.
   > 
   > 

## <a name="test-the-cdn-endpoint"></a>CDN uç noktasını sınama
Yayımlama durumu olduğunda **tamamlandı**, bir tarayıcı penceresi açın ve gidin  **http://<cdnName>*.azureedge.net/Content/bootstrap.css**. My Kurulum, bu URL'yi şöyledir:

    http://camservice.azureedge.net/Content/bootstrap.css

Hangi CDN uç noktası aşağıdaki kaynak URL'de karşılık gelir:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Ne zaman gezinmek için  **http://*&lt;cdnName >*indirin veya gelen bootstrap.css açmak için bağlı olarak istenir, tarayıcınızdaki.azureedge.net/Content/bootstrap.css** yayımlanan Web uygulamanızdan.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Genel olarak erişilebilir bir URL'de benzer şekilde erişebilirsiniz  **http://*&lt;serviceName >*doğrudan CDN uç noktanız gelen.cloudapp.net/**. Örneğin:

* / Script yolundan .js dosya
* / Content içerik dosyanın yolu
* Herhangi bir denetleyici/eylem
* CDN uç noktanız, sorgu dizeleri içeren herhangi bir URL, sorgu dizesi etkinleştirilirse

Aslında, yukarıdaki yapılandırma ile tüm bulut hizmetinden barındırabilir  **http://*&lt;cdnName >*.azureedge.net/**. İçin giderseniz **http://camservice.azureedge.net/**, ev/dizinden eylem sonucu alıyorum.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Bu, ancak bu her zaman bir tüm bulut hizmeti aracılığıyla Azure CDN sunmak için iyi bir fikir olduğu anlamına gelmez. 

CDN statik teslim iyileştirme önbelleğe almak üzere düşünülmemiştir dinamik varlıklarını teslimat hızlandırmak değil veya CDN varlığı yeni bir sürümü kaynak sunucudan çok sık çekme gerekir bu yana çok sık güncelleştirilir. Bu senaryo için etkinleştirmeniz [dinamik Site hızlandırma](cdn-dynamic-site-acceleration.md) alınabilir olmayan dinamik varlıklarını teslimat hızlandırmak için çeşitli teknikleri kullanan CDN uç noktanız iyileştirmeyi (DSA). 

Dinamik ve statik içeriği karışımını içeren bir siteniz varsa, statik en iyi duruma getirme türü (örneğin, genel web teslim) ile CDN, statik içerik sunmak ve dinamik içerik kaynak sunucudan doğrudan ya da bir CDN uç noktası w aracılığıyla hizmet seçebilir i DSA iyileştirme, bir olay temelinde açık. Bu amaçla, ayrı ayrı içerik dosyaları CDN uç noktasından erişmek nasıl zaten gördünüz. I denetleyici eylemleri Azure CDN aracılığıyla hizmet vermemesini içerikten bulunan belirli bir CDN uç noktası aracılığıyla belirli denetleyici eylemi hizmet nasıl yapacağınızı gösterir.

Bulut hizmetinizde olay temelinde Azure CDN sunmak için hangi içeriğini belirlemek için kullanılan alternatiftir. Bu amaçla, ayrı ayrı içerik dosyaları CDN uç noktasından erişmek nasıl zaten gördünüz. T belirli denetleyici eylemi CDN uç aracılığıyla hizmet nasıl yapacağınızı gösterir [hizmet denetleyici eylemleri Azure CDN aracılığıyla içerikten](#controller).

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Bulut hizmetinizde statik dosyalar için önbellek seçeneklerini yapılandırın
Bulut hizmetinizde Azure CDN tümleştirme ile nasıl CDN uç önbelleğe alınacak statik içerik istediğinizi belirtebilirsiniz. Bunu yapmak için açın *Web.config* (örneğin WebRole1), Web rolünden proje ve ekleme bir `<staticContent>` öğesine `<system.webServer>`. XML süresi 3 gün içinde dolacak önbellek yapılandırır.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Bunu yaptıktan sonra bulut hizmetindeki tüm statik dosyaları CDN önbelleğiniz aynı kuralında gözlemleyin. Önbellek ayarları konusunda daha ayrıntılı denetim için ekleme bir *Web.config* bir klasöre dosya ve ayarlarınızı var. ekleyin. Örneğin, bir *Web.config* dosya *\Content* klasörü ve içeriği aşağıdaki XML ile değiştirin:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Bu ayarı tüm statik dosyaları neden *\Content* 15 gün boyunca önbelleğe alınacak klasörü.

Nasıl yapılandırılacağı hakkında daha fazla bilgi için `<clientCache>` öğesi, bkz: [istemci önbelleği &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

İçinde [hizmet denetleyici eylemleri Azure CDN aracılığıyla içerikten](#controller), ı de CDN önbelleğinde önbellek ayarları denetleyici eylem sonuçlarına yönelik nasıl yapılandırabileceğiniz gösterilir.

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Denetleyici eylemleri Azure CDN aracılığıyla içerikten hizmet
Bulut hizmeti Web rolü Azure CDN ile tümleştirdiğinizde, denetleyici eylemleri için Azure CDN aracılığıyla içerikten hizmet oldukça kolaydır. Bulut hizmet veren dışında doğrudan Azure (yukarıda gösterilen), CDN hizmet [Maarten Balliauw](https://twitter.com/maartenballiauw) eğlenceli ile nasıl yapılacağını gösterir MemeGenerator denetleyicisi [Azure CDNwebüzerindegecikme](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). I yalnızca onu buraya yeniden.

Bulutunuzdaki Küçük yaştaki ALi Norris görüntüde memes oluşturmak istediğiniz hizmet tabanlı varsayalım (tarafından fotoğraf [Alan ışık](http://www.flickr.com/photos/alan-light/218493788/)) şöyle:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Basit bir sahip `Index` derecelerinin görüntüde belirtmek müşterilerin sağlayan eylem için eylem sonrası sonra meme sonra oluşturur. ALi Norris olduğuna göre genel müthiş bir başarı popüler hale için bu sayfayı beklenir. Bu, yarı dinamik içerik Azure CDN ile hizmet veren, iyi bir örnektir.

Bu denetleyici eylemi kurulumu için yukarıdaki adımları izleyin:

1. İçinde *\Controllers* klasör adında yeni bir .cs dosyası oluşturma *MemeGeneratorController.cs* ve içeriğini aşağıdaki kodla değiştirin. Vurgulanan bölümü, CDN adınızla değiştirdiğinizden emin olun.  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. Varsayılan olarak sağ `Index()` eylem ve select **Görünüm Ekle**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. Aşağıdaki ayarları kabul edin ve tıklatın **Ekle**.
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. Yeni *Views\MemeGenerator\Index.cshtml* ve içeriği derecelerinin göndermek için aşağıdaki basit HTML ile değiştirin:
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. Bulut hizmeti yeniden yayımlamanız ve gidin  **http://*&lt;serviceName >*tarayıcınızda.cloudapp.net/MemeGenerator/Index**.

Form değerleri gönderdiğiniz zaman `/MemeGenerator/Index`, `Index_Post` eylem yöntemine döndürür bağlantı `Show` eylem yöntemi ile ilgili giriş tanımlayıcısı. Bağlantıya tıkladığınızda, aşağıdaki kodu ulaşmak:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Ardından, yerel hata ayıklayıcısı ekli, yerel bir yeniden yönlendirme normal hata ayıklama deneyimini alırsınız. Bulut Hizmeti çalışıyorsa, için yönlendirir:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Hangi CDN uç noktanız aşağıdaki kaynak URL'de karşılık gelir:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Daha sonra `OutputCacheAttribute` özniteliği `Generate` yöntemi nasıl eylem sonucu, hangi Azure CDN dokunmaz önbelleğe alınacağını belirtin. Aşağıdaki kod bir önbellek süre sonu 1 saatlik (3.600 saniye) belirtin.

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Benzer şekilde, size herhangi bir denetleyici eylem içerikten yukarı bulut hizmetiniz istenen önbelleğe alma seçeneği ile Azure CDN aracılığıyla hizmet verebilir.

Sonraki bölümde, t, ile birlikte gelen ve küçültülmüş komut dosyaları ve CSS Azure CDN aracılığıyla sunmaya nasıl yapacağınızı gösterir.

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>ASP.NET paketleme ve küçültme Azure CDN ile tümleştirme
Komut dosyaları ve CSS stil sayfaları, seyrek değiştirin ve Azure CDN önbellek prime adaylar. Tüm Web rolü, Azure CDN aracılığıyla hizmet veren, paketleme ve küçültme Azure CDN ile tümleştirmek için kolay bir yoludur. Ancak, ı bunu istemeyebilirsiniz gibi ASP.NET paketleme ve küçültme, istenen develper deneyimini gibi koruma sırasında nasıl yapılacağını gösterir:

* Harika hata ayıklama modu deneyimi
* Basitleştirilmiş Dağıtım
* Komut dosyası/CSS sürüm yükseltme için istemcilere hemen güncelleştirmeleri
* CDN uç noktanız başarısız olduğunda bir geri dönüş mekanizması
* Kod değişikliği simge durumuna küçült

İçinde **WebRole1** oluşturduğunuz proje [Azure CDN uç Azure Web sitesi ile bütünleşir ve Azure CDN Web sayfalarınıza statik içerik sunmanızı](#deploy), açık *App_Start\ BundleConfig.cs* ve bir göz atalım `bundles.Add()` yöntem çağrıları.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

İlk `bundles.Add()` deyimi sanal dizininde betik paket ekler `~/bundles/jquery`. Ardından, açın *görünümler/paylaşılan\_Layout.cshtml* paket etiketi nasıl işlendiğine görmek için. Razor kodunun aşağıdaki satırı bulun yapabiliyor olmanız gerekir:

    @Scripts.Render("~/bundles/jquery")

Bu Razor kod Azure Web rolü çalıştırdığınızda kılacak bir `<script>` etiketi aşağıdakine benzer betik paket için:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Ancak, çalıştırıldığında Visual Studio'da yazarak `F5`, paketteki her komut dosyası ayrı ayrı kılacak (Yukarıdaki durumda yalnızca tek bir betik paketteki dosyasıdır):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Bu, eşzamanlı istemci bağlantıları (paketleme) azaltma ve dosya geliştirme performans (küçültme) üretimde karşıdan yüklenirken geliştirme ortamınızı JavaScript kodunda hata ayıklama olanak sağlar. Azure CDN tümleştirme ile korumak için harika bir özelliktir. İşlenen paket otomatik olarak oluşturulan sürüm dizesi içerdiğinden, ayrıca, bu işlevselliği çoğaltmak istediğiniz şekilde jQuery sürümünüzü NuGet aracılığıyla güncelleştirdiğinizde, istemci tarafında mümkün olan en kısa sürede güncelleştirilebilir.

ASP.NET tümleştirme paketleme ve küçültme ile CDN uç noktanız için aşağıdaki adımları izleyin.

1. Geri *App_Start\BundleConfig.cs*, değişiklik `bundles.Add()` farklı bir kullanılacak yöntemleri [paket Oluşturucusu](http://msdn.microsoft.com/library/jj646464.aspx), bir CDN adresini belirtir. Bunu yapmak için yerini `RegisterBundles` aşağıdaki kod ile yöntemi tanımı:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Değiştirdiğinizden emin olun `<yourCDNName>` Azure CDN adı.
   
    Düz metin olarak ayarladığınız `bundles.UseCdn = true` ve her paket için dikkatli bir şekilde hazırlanmış bir CDN URL eklenir. Örneğin, ilk oluşturucuda kodu:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    aynı sonucu verir:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    Bu oluşturucu, ASP.NET paketleme ve küçültme yerel olarak hata ayıklaması sırasında ayrı komut dosyaları oluşturmak, ancak söz konusu betik erişmek için belirtilen CDN adresini kullanmak için söyler. Ancak, bu dikkatle hazırlanmış CDN URL ile iki önemli özelliklere dikkat edin:
   
   * Bu CDN URL kaynağı `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, bulut hizmetinizin komut dosyası pakette gerçekte sanal dizinin olduğu.
   * CDN Oluşturucusu kullandığımızdan, paket için CDN komut dosyası etiketinin artık işlenmiş URL'de otomatik olarak oluşturulan sürüm dizesi içerir. Önbellek isabetsizliği Azure CDN adresindeki zorlamak için komut dosyası paket değiştiren her zaman bir benzersiz sürüm dizesi el ile oluşturmanız gerekir. Aynı anda bu benzersiz sürüm dizesi paket dağıtıldıktan sonra Azure CDN adresindeki İsabetli Önbellek okuma sayısı en üst düzeye çıkarmak için dağıtımı yaşam sabit kalması gerekir.
   * Sorgu dizesi v < W.X.Y.Z > çeken gelen = *Properties\AssemblyInfo.cs* Web rolü projenizdeki. Derleme sürümü için Azure yayımlama her zaman artırma içeren bir dağıtım iş akışı olabilir. Veya yalnızca değiştirebileceğiniz *Properties\AssemblyInfo.cs* projenize sürüm dizesi joker karakter kullanarak oluşturduğunuz her zaman otomatik olarak artırmak için ' *'. Örneğin:
     
        [derleme: AssemblyVersion("1.0.0.*")]
     
     Bir dağıtım ömrü için benzersiz bir dize oluşturma kolaylaştırmak için diğer bir strateji burada çalışır.
2. Bulut hizmeti yeniden yayımlamanız ve giriş sayfasına erişebilirsiniz.
3. Sayfanın HTML kodunu görüntüleyin. Bulut hizmetinizi değişiklikleri yeniden yayımlamanız her zaman bir benzersiz sürüm dizesi ile işlenen CDN URL'sine görebilmeniz gerekir. Örneğin:  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. Visual Studio'da yazarak, Visual Studio bulut hizmetinde hata ayıklama `F5`.,
5. Sayfanın HTML kodunu görüntüleyin. Böylece Visual Studio'da deneyimi tutarlı bir hata ayıklama sahip tek tek işlenen her komut dosyası hala görürsünüz.  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a>CDN URL'ler için geri dönüş mekanizması
Azure CDN uç noktanız için herhangi bir nedenle başarısız olduğunda, Web sayfanızın JavaScript veya önyükleme yüklenmesi için geri dönüş seçeneği olarak, kaynak Web sunucusuna erişmek akıllı olmasını istiyorsunuz. Sitenizdeki komut dosyalarını ve stil sayfalarını tarafından sağlanan önemli sayfa işlevleri için çok daha ağır ancak CDN kullanılamazlık nedeniyle görüntüleri kaybetmenize ciddi.

[Paket](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) sınıfı adlı bir özellik içerir [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) CDN hatası için geri dönüş mekanizması yapılandırmanıza olanak sağlar. Bu özelliği kullanmak için aşağıdaki adımları izleyin:

1. Web rolü projesinde açmak *App_Start\BundleConfig.cs*, CDN URL'sine her eklediğiniz [paket Oluşturucusu](http://msdn.microsoft.com/library/jj646464.aspx), varsayılan geri dönüş mekanizması eklemek için aşağıdaki vurgulanan değişiklikler Paketleri:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Zaman `CdnFallbackExpression` olan null değil, betik paket başarıyla yüklendi olup olmadığını sınamak ve değilse, paket kaynak Web sunucusundan doğrudan erişmek için HTML'e eklenmiş. Bu özellik, ilgili CDN paket düzgün şekilde yüklenmesini olup olmadığını, testleri bir JavaScript ifadesi ayarlanması gerekir. Her paket test etmek için gerekli ifade içeriği göre farklılık gösterir. Varsayılan paketleri için yukarıdaki:
   
   * `window.jquery`JQuery-{version} .js tanımlanır
   * `$.validator`JQuery.Validate.js tanımlanır
   * `window.Modernizr`modernizer gibi-{version} .js tanımlanır
   * `$.fn.modal`Bootstrap.js tanımlanır
     
     I CdnFallbackExpression için ayarlamamış fark etmiş olabilirsiniz `~/Cointent/css` paket. Şu anda bu olmadığından bir [System.Web.Optimization hatada](https://aspnetoptimization.codeplex.com/workitem/104) , yerleştirir bir `<script>` beklenen yerine geri dönüş CSS etiketini `<link>` etiketi.
     
     Yoktur, ancak iyi bir [stili paket geri dönüş](https://github.com/EmberConsultingGroup/StyleBundleFallback) tarafından sunulan [Ember danışmanlık grup](https://github.com/EmberConsultingGroup).
2. Geçici çözüm için CSS kullanmak için Web rolü projenizin içinde yeni bir .cs dosyası oluşturun *App_Start* adlı bir klasör *StyleBundleExtensions.cs*ve içeriği ile Değiştir [github'dan kodu ](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).
3. İçinde *App_Start\StyleFundleExtensions.cs*, ad alanı, Web rolün adıyla yeniden adlandırın (örneğin **WebRole1**).
4. Geri dönerek `App_Start\BundleConfig.cs` ve son değiştirme `bundles.Add` aşağıdaki vurgulanmış kodu deyimiyle:  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    Komut dosyası için DOM denetlemek için HTML eklemesine aynı fikir bu yeni genişletme yöntemi kullanan bir eşleşen sınıf adı, kural adı ve CSS paket ve kaynağı Web sunucusu geri döner için eşleşme bulunamadı başarısız olursa tanımlanmış kural değer.
5. Bulut hizmeti yeniden yayımlamanız ve giriş sayfasına erişebilirsiniz.
6. Sayfanın HTML kodunu görüntüleyin. Eklenen komut dosyaları aşağıdakine benzer bulmanız gerekir:    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    CSS paket için eklenen kod gelen yalıtılarak işlemi hala içerdiğine dikkat edin `CdnFallbackExpression` satır özelliğinde:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Ancak ilk kısmı bu yana || ifade her zaman true (satırda doğrudan yukarıdaki) döndürür ve document.write() işlevi asla çalışmaz.

## <a name="more-information"></a>Daha Fazla Bilgi
* [Azure içerik teslim ağı (CDN) genel bakış](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [Azure CDN'yi kullanma](cdn-create-new-endpoint.md)
* [ASP.NET paketleme ve küçültme](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
