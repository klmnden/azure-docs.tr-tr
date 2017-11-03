---
title: "Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme"
description: "Azure tarafından günlüğe kaydedilen bilgi erişmek nasıl yanı sıra tanılama günlük kaydını etkinleştirmek ve araçları uygulamanıza eklemek nasıl öğrenin."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 42644bd97443e827b523503989b0b25d3d48028c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme
## <a name="overview"></a>Genel Bakış
Azure hata ayıklamaya yardımcı olması için yerleşik tanılama sağlayan bir [App Service web uygulaması](http://go.microsoft.com/fwlink/?LinkId=529714). Bu makalede, Azure tarafından günlüğe kaydedilen bilgi erişmek nasıl yanı sıra tanılama günlük kaydını etkinleştirmek ve araçları uygulamanıza eklemek nasıl öğreneceksiniz.

Bu makalede kullanan [Azure Portal](https://portal.azure.com), tanılama günlükleri ile çalışmak için Azure PowerShell ve Azure komut satırı arabirimi (Azure CLI). Visual Studio kullanarak tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz: [Visual Studio'da Azure sorun giderme](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Web sunucusu tanılama ve uygulama tanılama
App Service web uygulamalarının web sunucusu ve web uygulamasının içinden bilgileri günlüğe kaydetme için tanılama işlevsellik sağlar. Bu mantıksal olarak ayrılmış **web sunucusu tanılama** ve **uygulama tanılama**.

### <a name="web-server-diagnostics"></a>Web sunucu tanıları
Etkinleştirmek veya günlükleri şu tür devre dışı bırakabilirsiniz:

* **Hata günlüğü ayrıntılı** -ayrıntılı hata bilgileri (durum kodu 400 veya daha büyük) hatası olduğunu gösteren HTTP durum kodları için. Bu neden sunucu döndürülen hata kodu belirlemek yardımcı olabilecek bilgiler içerebilir.
* **Başarısız istek izleme** -ayrıntılı izleme istek ve her bileşenin geçen süre işlemek için kullanılan IIS bileşenlerini de dahil olmak üzere, başarısız istekler hakkında bilgi. Bu site performansını artırabilir ya da döndürülecek belirli bir HTTP hata neden olan yalıtmak çalıştığınız durumlarda yararlı olabilir.
* **Web sunucusu günlüğe kaydetme** -kullanarak HTTP işlemler hakkında bilgi [W3C Genişletilmiş günlük dosyası biçimi](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Bu, belirli bir IP adresinden işlenen isteklerin ya da kaç istek sayısı gibi genel site ölçümleri belirlerken yararlıdır.

### <a name="application-diagnostics"></a>Uygulama tanılama
Uygulama Tanılama web uygulama tarafından üretilen bilgileri yakalamanıza olanak sağlar. ASP.NET uygulamaları kullanabileceğiniz [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) bilgi uygulama tanılama günlüğüne sınıfı. Örneğin:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Çalışma zamanında, sorun giderme konusunda yardımcı olmak için bu günlükleri alabilirsiniz. Daha fazla bilgi için bkz: [Visual Studio'daki sorun giderme Azure web uygulamaları](web-sites-dotnet-troubleshoot-visual-studio.md).

Bir web uygulaması için içerik yayımladığınızda, app Service web apps dağıtım bilgileri de oturum açın. Bu otomatik olarak gerçekleşir ve dağıtım günlüğü için herhangi bir yapılandırma ayarları. Dağıtım günlüğü neden bir dağıtımı başarısız belirlemenize olanak tanır. Örneğin, bir özel dağıtım komut dosyası kullanıyorsanız, komut dosyası neden başarısız olduğunu belirlemek için dağıtım günlüğü kullanabilirsiniz.

## <a name="enablediag"></a>Tanılama etkinleştirme
Tanılama'etkinleştirmek için [Azure Portal](https://portal.azure.com), web uygulamanız için dikey penceresine gidin ve tıklatın **Ayarları > tanılama günlükleri**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Günlükleri bölümü](./media/web-sites-enable-diagnostic-log/logspart.png)

Etkinleştirdiğinizde **uygulama tanılama** ayrıca tercih **düzeyi**. Bu ayar için yakalanan bilgilerin filtrelemenizi sağlar **bilgilendirme**, **uyarı** veya **hata** bilgi. Bu ayar **ayrıntılı** uygulama tarafından üretilen tüm bilgileri kaydeder.

> [!NOTE]
> Web.config dosyasını değiştirme farklı olarak, uygulama Tanılama'yı etkinleştirme ya da tanılama günlük düzeylerini değiştirme içinde uygulamanın çalıştığı uygulama etki alanı dönüştürülmeyeceği.
>
>

İçinde [Klasik portal](https://manage.windowsazure.com) Web uygulaması **yapılandırma** seçebileceğiniz sekmesinde **depolama** veya **dosya sistemi** için **web sunucusu günlüğü**. Seçme **depolama** bir depolama hesabı ve günlüklere yazılır bir blob kapsayıcısını seçmenize olanak sağlar. İçin tüm diğer günlükler **site tanılama** yalnızca dosya sistemine yazılır.

[Klasik portal](https://manage.windowsazure.com) Web uygulaması **yapılandırma** sekmesinde de uygulama tanılama için ek ayarlar vardır:

* **Dosya sistemi** -web uygulama dosya sistemi için uygulama tanılama bilgilerini depolar. Bu dosyalar FTP tarafından erişilen veya Zip arşivini Azure PowerShell veya Azure komut satırı arabirimi (Azure CLI) kullanılarak indirilir.
* **Tablo depolama** -belirtilen Azure depolama hesabı ve tablo adı uygulama tanılama bilgilerini depolar.
* **BLOB Depolama** -belirtilen Azure depolama hesabı ve blob kapsayıcısında uygulama tanılama bilgilerini depolar.
* **Saklama dönemi** -varsayılan olarak, günlükleri otomatik olarak silinir değil **blob depolama**. Seçin **ayarlamak bekletme** ve otomatik olarak günlükleri silmek isterseniz, günlükleri tutulacağı gün sayısı girin.

> [!NOTE]
> Varsa, [depolama hesabınızın erişim anahtarlarını yeniden](../storage/common/storage-create-storage-account.md), güncelleştirilmiş anahtarları kullanmak için ilgili günlük yapılandırmasını sıfırlamanız gerekir. Bunu yapmak için:
>
> 1. İçinde **yapılandırma** sekmesinde, ilgili günlük özelliğini ayarlamak **devre dışı**. Ayarınız kaydedin.
> 2. Depolama hesabı blob veya tablo için günlüğe kaydetme, yeniden etkinleştirin. Ayarınız kaydedin.
>
>

Dosya sistemi, tablo depolama veya blob depolama herhangi bir bileşimini aynı anda etkinleştirilebilir ve ayrı Günlük düzeyi yapılandırmaları vardır. Örneğin, hataları ve Uyarıları ile ayrıntılı bir düzeyde dosya sistemi günlük etkinleştirirken uzun vadeli günlük bir çözüm, blob depolama için oturum isteyebilirsiniz.

Tüm üç depolama konumları aynı temel bilgileri günlüğe kaydedilen olayları için sunarken **tablo depolama** ve **blob depolama** oturum örnek kimliği, iş parçacığı kimliği ve günlük tutmayı'den daha ayrıntılı bir zaman damgası (onay biçimi) gibi ek bilgileri **dosya sistemi**.

> [!NOTE]
> İçinde depolanan bilgileri **tablo depolama** veya **blob depolama** yalnızca bir depolama istemcisi veya doğrudan bu depolama sistemleri ile çalışabilirsiniz uygulamanın kullanılarak erişilebilir. Örneğin, Visual Studio 2013 tablo veya blob depolama keşfetmek için kullanılan bir Depolama Gezgini içerir ve Hdınsight blob storage'da depolanan verilere erişebilir. Ayrıca Azure Storage birini kullanarak erişen bir uygulama yazabilirsiniz [Azure SDK'ları](/downloads/#).
>
> [!NOTE]
> Tanılama Azure Powershell'den de etkinleştirilebilir kullanarak **kümesi AzureWebsite** cmdlet'i. Azure PowerShell yüklü değil ya da Azure aboneliğinizi kullanacak şekilde yapılandırmadıysanız, bkz: [Azure PowerShell kullanmak için nasıl](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

## <a name="download"></a>Nasıl yapılır: indirme günlükleri
Web uygulaması dosya sistemine depolanan tanılama bilgileri FTP kullanarak doğrudan erişilebilir. Ayrıca Azure PowerShell veya Azure komut satırı arabirimi kullanarak Zip arşivini indirilebilir.

Günlükleri depolanmış dizin yapısı aşağıdaki gibidir:

* **Uygulama günlüklerini** -/LogFiles/uygulama /. Bu klasör uygulama günlüğü tarafından üretilen bilgileri içeren bir veya daha fazla metin dosyalarını içerir.
* **Başarısız istek izlemelerin** -/ LogFiles/W3SVC ### /. Bu klasör bir XSL dosyası ve bir veya daha fazla XML dosyalarını içerir. XSL dosyasını biçimlendirme ve Internet Explorer'da görüntülendiğinde XML dosyaları içeriğini filtreleme işlevselliği sağladığından XML dosyaları gibi aynı dizine XSL dosyasını karşıdan emin olun.
* **Ayrıntılı Hata günlüklerini** -/LogFiles/DetailedErrors /. Bu klasör oluşan HTTP hataları için kapsamlı bilgi sağlayan bir veya daha fazla .htm dosyalarını içerir.
* **Web sunucu günlükleri** -/LogFiles/http/RawLogs. Bu klasör içeriyor ya da daha fazla metin dosyaları olarak biçimlendirilmiş kullanarak [W3C Genişletilmiş günlük dosyası biçimi](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Dağıtım günlükleri** -/ LogFiles/Git. Bu klasör Azure web uygulamaları tarafından kullanılan iç dağıtım işlemler tarafından oluşturulan günlükleri içeren, aynı zamanda Git dağıtımları için günlüğe kaydeder.

### <a name="ftp"></a>FTP
FTP kullanarak tanılama bilgilerine erişmek için ziyaret **Pano** , web uygulamanızın [Klasik portal](https://manage.windowsazure.com). İçinde **Hızlı Bakış** bölümünde, kullanmak **FTP tanılama günlükleri** FTP kullanarak günlük dosyalarına erişmek için bağlantı. **Dağıtım/FTP kullanıcısı** girişi FTP sitesine erişmek için kullanılması gereken kullanıcı adını listeler.

> [!NOTE]
> Varsa **dağıtım/FTP kullanıcısı** girişi ayarlanmamışsa, veya bu kullanıcı için parolanızı unuttuysanız, yeni bir kullanıcı ve parola kullanarak oluşturabilirsiniz **sıfırlama dağıtım kimlik bilgileri** bağlamak **Hızlı Bakış** bölümünü **Pano**.
>
>

### <a name="download-with-azure-powershell"></a>Azure PowerShell ile indirme
Günlük Dosyaları indirmek için Azure PowerShell yeni bir örneğini başlatın ve aşağıdaki komutu kullanın:

    Save-AzureWebSiteLog -Name webappname

Bu günlükler tarafından belirtilen web uygulaması için kaydeder **-adı** adlı bir dosyaya parametre **logs.zip** geçerli dizin.

> [!NOTE]
> Azure PowerShell yüklü değil ya da Azure aboneliğinizi kullanacak şekilde yapılandırmadıysanız, bkz: [Azure PowerShell kullanmak için nasıl](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="download-with-azure-command-line-interface"></a>Azure komut satırı arabirimi ile indirme
Azure komut satırı arabirimini kullanarak günlük dosyalarını indirmek için yeni bir komut istemi, PowerShell, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

    azure site log download webappname

Bu günlükler adlı bir dosyaya ' webappname' adlı web uygulamasının kaydeder **diagnostics.zip** geçerli dizin.

> [!NOTE]
> Azure komut satırı arabirimi (Azure CLI) yüklü değil ya da Azure aboneliğinizi kullanacak şekilde yapılandırmadıysanız, bkz: [Azure CLI kullanma nasıl](../cli-install-nodejs.md).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Nasıl yapılır: View Application Insights'ta günlüğe kaydeder
Visual Studio Application Insights filtreleme ve günlükleri arama ve günlükleri istekleri ve diğer olaylarla ilişkilendirme için araçlar sağlar.

1. Projeniz Visual Studio'da Application Insights SDK ekleyin.
   * Çözüm Gezgini'nde, projenize sağ tıklayın ve Application Insights Ekle'ı seçin. Application Insights kaynağı oluşturma dahil adımlarda size kılavuzluk. [Daha fazla bilgi](../application-insights/app-insights-asp-net.md)
2. İzleme dinleyicisi paketini projenize ekleyin.
   * Projenize sağ tıklayın ve NuGet paketlerini Yönet'i seçin. Seçin `Microsoft.ApplicationInsights.TraceListener` [daha fazla bilgi edinin](../application-insights/app-insights-asp-net-trace-logs.md)
3. Projenizi karşıya yükleme ve günlük verileri oluşturmak için çalıştırın.
4. İçinde [Azure Portal](https://portal.azure.com/), yeni Application Insights kaynağınıza göz atın ve Aç **arama**. İstek, kullanım ve diğer telemetri ile birlikte günlük verilerinizi görürsünüz. Birkaç telemetri gelmesi birkaç dakika sürebilir: Yenile'yi tıklatın. [Daha fazla bilgi](../application-insights/app-insights-diagnostic-search.md)

[Application Insights ile izleme performansı hakkında daha fazla bilgi edinin](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a>Nasıl yapılır: akış günlükleri
Bir uygulama geliştirirken, genellikle neredeyse gerçek zamanlı günlük bilgileri görmek yararlı olacaktır. Bu günlük kaydı bilgileri Azure PowerShell veya Azure komut satırı arabirimi kullanarak geliştirme ortamınıza akış tarafından gerçekleştirilebilir.

> [!NOTE]
> Günlük arabellek bazı türleri akış bozuk olayları sonuçlanabilir günlük dosyasına yazar. Örneğin, bir kullanıcı bir sayfayı ziyaret ettiğinde oluşan bir uygulama günlük girişi sayfa isteği için karşılık gelen HTTP günlük girişi önce akışında görüntülenebilir.
>
> [!NOTE]
> Günlük akış de akış depolanan herhangi bir metin dosyası için yazılan bilgilerin **D:\\ev\\LogFiles\\**  klasör.
>
>

### <a name="streaming-with-azure-powershell"></a>Azure PowerShell ile akış
Günlük kaydı bilgileri akış, Azure PowerShell yeni bir örneğini başlatmak ve aşağıdaki komutu kullanmak için:

    Get-AzureWebSiteLog -Name webappname -Tail

Bu tarafından belirtilen web uygulamasına bağlanır **-Name** parametre ve web uygulamasında günlüğü olaylarını ortaya çıktığında PowerShell penceresinde bilgileri akış başlayın. /LogFiles dizininde (d:/home/günlük dosyaları) depolanan .txt, .log veya .htm bitiş dosyaları yazılan herhangi bir bilgi yerel konsola akışı.

Hataları gibi belirli olayları filtrelemek için kullanmak **-ileti** parametresi. Örneğin:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

HTTP gibi belirli günlük türleri filtrelemek için kullanmak **-yol** parametresi. Örneğin:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

Kullanılabilir yolların listesini görmek için - ListPath parametresini kullanın.

> [!NOTE]
> Azure PowerShell yüklü değil ya da Azure aboneliğinizi kullanacak şekilde yapılandırmadıysanız, bkz: [Azure PowerShell kullanmak için nasıl](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Azure komut satırı arabirimi ile akış
Günlük kaydı bilgileri akış, yeni komut istemi, PowerShell, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin için:

    azure site log tail webappname

Bu, 'webappname' adlı web uygulamasına bağlanmak ve web uygulamasında günlüğü olaylarını ortaya çıktığında penceresine bilgi akış başlayın. /LogFiles dizininde (d:/home/günlük dosyaları) depolanan .txt, .log veya .htm bitiş dosyaları yazılan herhangi bir bilgi yerel konsola akışı.

Hataları gibi belirli olayları filtrelemek için kullanmak **--filtre** parametresi. Örneğin:

    azure site log tail webappname --filter Error

HTTP gibi belirli günlük türleri filtrelemek için kullanmak **--yolu** parametresi. Örneğin:

    azure site log tail webappname --path http

> [!NOTE]
> Azure komut satırı arabirimi yüklü değil ya da Azure aboneliğinizi kullanacak şekilde yapılandırmadıysanız, bkz: [nasıl için Azure komut satırı arabirimi kullanmak](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a>Nasıl yapılır: Tanılama günlükleri anlama
### <a name="application-diagnostics-logs"></a>Uygulama tanılama günlükleri
Uygulama tanılama günlükleri dosya sistemi, tablo depolama veya blob depolama depoladığınız bağlı olarak, .NET uygulamaları için belirli bir biçimde bilgileri depolar. Temel depolanan verileri aynı tüm üç depolama türlerine - tarih ve saat olayı, olay türü (bilgi, uyarı, hata) ve olay iletisi üretilen işlem kimliği olayın gerçekleştiği kümesidir.

**Dosya sistemi**

Dosya sistemine oturum veya akış kullanılarak alınan her bir satır şu biçimde olacaktır:

    {Date}  PID[{process id}] {event type/level} {message}

Örneğin, bir hata olayı aşağıdakine benzer görünür:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Dosya sistemine günlüğü yalnızca zaman, işlem kimliği, olay düzeyi ve ileti sağlayan üç kullanılabilir yöntemleri, en temel bilgileri sağlar.

**Tablo depolama**

Tablo depolama için oturum açarken ek özellikler Tablo yanı sıra olay hakkında daha ayrıntılı bilgi depolanan veri arama kolaylaştırmak için kullanılır. Aşağıdaki özellikleri (sütunları) tabloda depolanan her varlık (satır) için kullanılır.

| Özellik adı | Değer/biçimi |
| --- | --- |
| PartitionKey |Tarih/saat yyyyMMddHH biçiminde olay |
| RowKey |Bu varlık benzersiz olarak tanımlayan bir GUID değeri |
| zaman damgası |Olayın saat ve tarihi |
| EventTickCount |Değer çizgilerinin biçiminde (büyük duyarlık) olayın gerçekleştiği saat ve tarihi |
| ApplicationName |Web uygulaması adı |
| Düzey |Olay düzeyi (uyarı, bilgi örneğin hata) |
| Olay Kimliği |Bu olayın olay kimliği<p><p>Varsayılanları hiçbiri belirtilmişse 0 |
| örnek kimliği |Hatta oluştu. web uygulaması örneği |
| PID |İşlem kimliği |
| komutu |Olay üretilen iş parçacığı iş parçacığı kimliği |
| İleti |Olay Ayrıntısı iletisi |

**Blob depolama**

Blob depolama için oturum açarken verileri virgülle ayrılmış değerler (CSV) biçiminde depolanır. Table storage benzer olay hakkında daha ayrıntılı bilgi sağlamak için ek alanlar kaydedilir. Aşağıdaki özellikler, CSV her satır için kullanılır:

| Özellik adı | Değer/biçimi |
| --- | --- |
| Tarih |Olayın saat ve tarihi |
| Düzey |Olay düzeyi (uyarı, bilgi örneğin hata) |
| ApplicationName |Web uygulaması adı |
| örnek kimliği |Olayın oluştuğu web uygulaması örneği |
| EventTickCount |Değer çizgilerinin biçiminde (büyük duyarlık) olayın gerçekleştiği saat ve tarihi |
| Olay Kimliği |Bu olayın olay kimliği<p><p>Varsayılanları hiçbiri belirtilmişse 0 |
| PID |İşlem kimliği |
| komutu |Olay üretilen iş parçacığı iş parçacığı kimliği |
| İleti |Olay Ayrıntısı iletisi |

Blob içinde depolanan verileri aşağıdakine benzer görünür:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> Günlüğün ilk satırı sütun başlığı Bu örnekte belirtildiği şekilde içerir.
>
>

### <a name="failed-request-traces"></a>İstek izlemelerin başarısız oldu
Başarısız istek izlemelerin adlı XML dosyalarında saklanır **fr ### .xml**. Günlüğe kaydedilen bilgileri görüntülemek kolaylaştırmak için bir XSL stil adlı **freb.xsl** XML dosyaları ile aynı dizinde sağlanır. XML dosyalarından birini Internet Explorer'da açma XSL stil sayfası izleme bilgilerini biçimlendirilmiş bir görünümünü sağlamak için kullanır. Bu, aşağıdakine benzer görünecektir:

![tarayıcıda görüntülenen başarısız istek](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Ayrıntılı hata günlükleri
Ayrıntılı Hata günlüklerini oluşan HTTP hataları daha ayrıntılı bilgi sağlayan HTML belgelerdir. Bunlar yalnızca HTML belgeleri olduğundan, bir web tarayıcısı kullanarak görüntülenebilir.

### <a name="web-server-logs"></a>Web sunucu günlükleri
Web sunucu günlükleri kullanılarak biçimlendirilmiş [W3C Genişletilmiş günlük dosyası biçimi](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Bu bilgileri bir metin düzenleyicisi kullanarak okunabilir veya yardımcı programlarını kullanarak Ayrıştırılan [günlük ayrıştırıcısı](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Azure web uygulamaları tarafından oluşturulan günlükleri desteklemeyen **s-computername**, **s-ip**, veya **cs-version** alanları.
>
>

## <a name="nextsteps"></a> Sonraki adımlar
* [Web uygulamaları izleme](web-sites-monitor.md)
* [Visual Studio'daki Azure web uygulamaları sorunlarını giderme](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analiz web uygulama günlüklerini Hdınsight'ta](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
>
>

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](http://go.microsoft.com/fwlink/?LinkId=529714)
* Yeni portalı eski portalına değiştirme kılavuzu için bkz: [Azure portalında gezinme için başvuru](http://go.microsoft.com/fwlink/?LinkId=529715)
