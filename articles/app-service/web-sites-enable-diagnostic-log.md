---
title: Azure App Service'te web apps için tanılama günlüğünü etkinleştirme
description: Tanılama günlüğünü etkinleştirme ve uygulamanız için izleme ekleme yanı sıra Azure tarafından günlüğe kaydedilen bilgilere nasıl öğrenin.
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
ms.openlocfilehash: 5cd56abd02c55dbf72c92ed070f9988fae2b6762
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365263"
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Azure App Service'te web apps için tanılama günlüğünü etkinleştirme
## <a name="overview"></a>Genel Bakış
Azure, hatalarını ayıklamaya yardımcı olmak üzere yerleşik tanılama sağlar bir [App Service web uygulaması](http://go.microsoft.com/fwlink/?LinkId=529714). Bu makalede, Azure tarafından günlüğe kaydedilen bilgilere nasıl yanı sıra tanılama günlüğünü etkinleştirme ve uygulamanız için izleme ekleme öğrenin.

Bu makalede [Azure portalında](https://portal.azure.com), tanılama günlükleri ile çalışmak için Azure PowerShell ve Azure komut satırı arabirimi (Azure CLI). Visual Studio kullanarak tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Visual Studio'daki sorun giderme Azure](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Web sunucusu tanılama ve uygulama tanılama
App Service web apps için web sunucusunu hem web uygulamasının içinden bilgileri günlüğe kaydetme tanılama işlevi sağlar. Bu mantıksal olarak ayrılır **web sunucu tanılamalarını** ve **uygulama tanılama**.

### <a name="web-server-diagnostics"></a>Web sunucusu tanılama
Etkinleştirmek veya günlükleri aşağıdaki türde devre dışı bırakabilirsiniz:

* **Ayrıntılı hata günlüğü** -ayrıntılı hata bilgileri belirten bir hata (durum kodu 400 veya üzeri) HTTP durum kodları için. Bu sunucunun döndürülen hata kodu neden belirlemek yardımcı olabilecek bilgiler içerebilir.
* **Başarısız istek izleme** -ayrıntılı bir izleme isteği ve her bir bileşende geçen süre işlemek için kullanılan IIS bileşenlerini de dahil olmak üzere, başarısız isteklerle bilgileri. Site performansı artırmak veya döndürülecek belirli bir HTTP hatası neden olan yalıtmak çalışıyorsanız yararlı olur.
* **Web sunucusu günlüğe kaydetme** -kullanarak HTTP işlemleri hakkında bilgi [W3C Genişletilmiş günlük dosyası biçimini](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). İşlenen isteklerin veya özel bir IP adresinden kaç isteklerdir sayısı gibi genel site ölçümleri belirlerken yararlı olacaktır.

### <a name="application-diagnostics"></a>Uygulama tanılamaları
Uygulama Tanılama web uygulaması tarafından üretilen bilgileri yakalamanıza olanak sağlar. ASP.NET uygulamalarında kullanabileceğiniz [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) uygulama tanılama günlüğüne bilgileri günlüğe kaydetmek için sınıf. Örneğin:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Çalışma zamanında gidermeye yardımcı olması için bu günlükleri alabilirsiniz. Daha fazla bilgi için [Visual Studio'daki sorun giderme Azure web Apps'e](web-sites-dotnet-troubleshoot-visual-studio.md).

Bir web uygulamasına içerik yayımlarken app Service web Apps'e dağıtım bilgilerini ayrıca oturum. Otomatik olarak gerçekleşir ve dağıtım günlüğü için yapılandırma ayarı yok. Dağıtım günlüğü bir dağıtım başarısız olmasının belirlemenize olanak tanır. Örneğin, bir özel dağıtım betiği kullanıyorsanız, betiği neden başarısız olduğunu belirlemek için dağıtım günlüğünü kullanabilirsiniz.

## <a name="enablediag"></a>Tanılamayı etkinleştirme
Tanılamayı etkinleştirmek için [Azure portalında](https://portal.azure.com), web uygulamanızın sayfasına gidin ve tıklayın **Ayarları > tanılama günlükleri**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Günlükleri bölümü](./media/web-sites-enable-diagnostic-log/logspart.png)

Etkinleştirdiğinizde **uygulama tanılama**, ayrıca **düzeyi**. Bu ayar için yakalanan bilgilerin filtrelemenize olanak sağlar. **bilgilendirici**, **uyarı**, veya **hata** bilgileri. Bu ayarın **ayrıntılı** uygulama tarafından üretilen tüm bilgileri günlüğe kaydeder.

> [!NOTE]
> Web.config dosyasını değiştirme aksine, uygulama Tanılama'yı etkinleştirme ya da tanılama günlük düzeylerini değiştirme uygulamanın içinde çalıştığı uygulama etki alanı dönüştürülmeyeceği.
>
>

İçin **uygulama günlüğü**, geçici hata ayıklama amacıyla için dosya sistemi seçeneğini etkinleştirebilirsiniz. Bu seçenek otomatik olarak 12 saat içinde devre dışı bırakır. Günlüklerin yazılacağı bir blob kapsayıcısını seçmek için blob depolama seçeneğinde kapatabilirsiniz.

İçin **Web sunucusu günlüğü**, seçebileceğiniz **depolama** veya **dosya sistemi**. Seçme **depolama** bir depolama hesabı ve günlüklere yazılır bir blob kapsayıcısı seçmenizi sağlar. 

Dosya sisteminde günlüklerini depolamak, dosyaları FTP tarafından erişilen veya Azure PowerShell veya Azure komut satırı arabirimi (Azure CLI) kullanarak bir ZIP arşivi indirilir.

Varsayılan olarak, günlükleri otomatik olarak silinmez (dışında **uygulama günlüğü (dosya sistemi)**). Günlükleri otomatik olarak silmek için ayarlarsınız **saklama dönemi (gün)** alan.

> [!NOTE]
> Varsa, [depolama hesabınızın erişim anahtarlarını yeniden oluştur](../storage/common/storage-create-storage-account.md), güncelleştirilmiş anahtarları kullanmak için kendi günlük yapılandırmasını sıfırlamanız gerekir. Bunu yapmak için:
>
> 1. İçinde **yapılandırma** sekmesinde, ilgili günlük kaydı özelliğini **kapalı**. Ayarlarınızı kaydedin.
> 2. Depolama hesabının blob veya tablo için günlüğe kaydetme, yeniden etkinleştirin. Ayarlarınızı kaydedin.
>
>

Dosya sistemi, tablo depolama veya blob depolama, herhangi bir birleşimini aynı anda etkinleştirilebilir ve tek bir günlük düzeyi yapılandırmalara sahip. Örneğin, hataları ve Uyarıları dosya sistemi günlük kaydı ile ayrıntılı bir düzeyde etkinleştirirken uzun vadeli günlüğü çözüm olarak, BLOB depolamaya oturum isteyebilirsiniz.

Aynı temel bilgileri günlüğe kaydedilen olayları, tüm üç depolama konumları sunarken **tablo depolama** ve **blob depolama** daha örnek kimliği ve iş parçacığı kimliği gibi ek bilgilerini günlüğe kaydetme günlüğe kaydetme için daha ayrıntılı timestamp (değer biçimi) **dosya sistemi**.

> [!NOTE]
> Depolanan bilgi **tablo depolama** veya **blob depolama** yalnızca depolama istemcisi veya bu depolama sistemleri ile doğrudan çalışabilmeniz için uygulamanın kullanılarak erişilebilir. Örneğin, Visual Studio 2013 tablo veya blob depolama keşfetmek için kullanılan bir Depolama Gezgini içerir ve HDInsight, blob depolamada depolanan verilere erişebilir. Ayrıca, aşağıdakilerden birini kullanarak Azure depolama alanına erişen bir uygulama yazabilirsiniz [Azure SDK'ları](https://azure.microsoft.com/downloads/).
>
> [!NOTE]
> Tanılama Azure Powershell'den da etkinleştirilebilir kullanarak **Set-AzureWebsite** cmdlet'i. Azure PowerShell yüklü değil veya Azure Aboneliğinizdeki kullanacak şekilde yapılandırmadıysanız, bkz. [yüklemek ve Azure PowerShell yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.6.0).
>
>

## <a name="download"></a> Nasıl yapılır: indirme günlükleri
Tanılama bilgileri için web uygulaması dosya sistemi depolanan FTP kullanarak doğrudan erişilebilir. Ayrıca Azure PowerShell veya Azure komut satırı arabirimi kullanarak bir ZIP arşivi indirilebilir.

Günlükleri depolanan dizin yapısı aşağıdaki gibidir:

* **Uygulama günlükleri** -/LogFiles/uygulama /. Bu klasör, uygulama günlüğü tarafından üretilen bilgileri içeren bir veya daha fazla metin dosyalarını içerir.
* **Başarısız istek izlemelerin** -/ LogFiles/W3SVC ### /. Bu klasör, XSL dosyası ve bir veya daha fazla XML dosyalarını içerir. XSL dosyası biçimlendirme ve Internet Explorer'da görüntülendiğinde XML dosyalarının içeriğini filtreleme işlevi sağladığından, XML dosyaları gibi aynı dizine XSL dosyası indirme emin olun.
* **Ayrıntılı Hata günlüklerini** -/LogFiles DetailedErrors /. Bu klasör, ortaya çıkan HTTP hataları için kapsamlı bilgiler sağlayan bir veya daha fazla .htm dosyaları içerir.
* **Web sunucusu günlüklerini** -/LogFiles/http/RawLogs. Bu klasör bir veya daha fazla metin dosyaları olarak biçimlendirilmiş kullanarak [W3C Genişletilmiş günlük dosyası biçimini](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Dağıtım günlükleri** -/ LogFiles/Git. Bu klasör Azure web uygulamaları tarafından kullanılan iç dağıtım işlemleri tarafından oluşturulan günlükleri içeren, hem de Git dağıtımları için günlüğe kaydeder. Ayrıca dağıtım günlükleri D:\home\site\deployments altında bulabilirsiniz.

### <a name="ftp"></a>FTP

Uygulamanızın FTP sunucusu ile FTP bağlantısı açmak için bkz [uygulamanızı FTP/S kullanarak Azure App Service'e dağıtma](app-service-deploy-ftp.md).

Web uygulamanızın FTP/S sunucusuna bağlandıktan sonra açın **LogFiles** günlük dosyalarının depolandığı klasöre,.

### <a name="download-with-azure-powershell"></a>Azure PowerShell ile indirin
Günlük Dosyaları indirmek için Azure PowerShell'in yeni bir örneğini başlatın ve aşağıdaki komutu kullanın:

    Save-AzureWebSiteLog -Name webappname

Bu komut tarafından belirtilen web uygulaması için günlüklere kaydeder **-adı** parametresi adlı bir dosyaya **logs.zip** geçerli dizin.

> [!NOTE]
> Azure PowerShell yüklü değil veya Azure Aboneliğinizdeki kullanacak şekilde yapılandırmadıysanız, bkz. [yüklemek ve Azure PowerShell yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.6.0).
>
>

### <a name="download-with-azure-command-line-interface"></a>Azure komut satırı arabirimi ile indirin
Azure komut satırı arabirimini kullanarak günlük dosyalarını indirmek için yeni bir komut istemi, PowerShell, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

    az webapp log download --resource-group resourcegroupname --name webappname

Bu komut adlı bir dosyaya ' webappname' adlı web uygulaması için günlüklere kaydeder **diagnostics.zip** geçerli dizin.

> [!NOTE]
> Azure komut satırı arabirimi (Azure CLI) yüklü değil veya Azure Aboneliğinizdeki kullanacak şekilde yapılandırmadıysanız, bkz. [Azure CLI kullanmak için nasıl](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Nasıl yapılır: görünüm Application Insights'ta günlüğe kaydeder
Visual Studio Application Insights, filtreleme ve günlük arama ve günlükleri istekleri ve diğer olaylarla ilişkilendirmek için gereken araçları sağlar.

1. Projenizi Visual Studio'da Application Insights SDK'sını ekleyin.
   * Çözüm Gezgini'nde projenize sağ tıklayın ve Application Insights Ekle'ı seçin. Arabirim içeren bir Application Insights kaynağı oluşturma adım adım yol gösterir. [Daha fazla bilgi](../application-insights/app-insights-asp-net.md)
2. İzleme dinleyicisi paketini projenize ekleyin.
   * Projenize sağ tıklayın ve NuGet paketlerini Yönet'i seçin. Seçin `Microsoft.ApplicationInsights.TraceListener` [daha fazla bilgi edinin](../application-insights/app-insights-asp-net-trace-logs.md)
3. Projenize yükleyin ve günlük verileri üretmek için çalıştırabilirsiniz.
4. İçinde [Azure portalında](https://portal.azure.com/)yeni Application Insights kaynağınıza göz atın ve Aç **arama**. İstek, kullanım ve diğer telemetriyi birlikte günlük verilerinizi görmeniz gerekir. Bazı telemetri gelmesi birkaç dakika sürebilir: Yenile'ye tıklayın. [Daha fazla bilgi](../application-insights/app-insights-diagnostic-search.md)

[Application Insights ile izleme performansıyla ilgili daha fazla bilgi edinin](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a> Nasıl yapılır: Stream günlükleri
Bir uygulama geliştirirken, genellikle günlük kaydı bilgilerini neredeyse gerçek zamanlı görmek yararlı olur. Geliştirme ortamınızı Azure PowerShell veya Azure komut satırı arabirimini kullanarak günlüğe bilgi akışını yapabilirsiniz.

> [!NOTE]
> Akıştaki sıralama dışı olaylar sonuçlanabilir günlük dosyasına günlük arabellek bazı türleri yazın. Örneğin, bir kullanıcı bir sayfayı ziyaret ettiğinde oluşan bir uygulama günlük girişi sayfa isteği için karşılık gelen HTTP günlük girişi önce akıştaki görüntülenebilir.
>
> [!NOTE]
> Günlük akışını ayrıca depolanan herhangi bir metin dosyası yazılan bilgi akışları **D:\\giriş\\LogFiles\\**  klasör.
>
>

### <a name="streaming-with-azure-powershell"></a>Azure PowerShell ile akış
Değiştirmek için akış günlük kaydı bilgilerini, Azure PowerShell'in yeni bir örneği başlatın ve aşağıdaki komutu kullanın:

    Get-AzureWebSiteLog -Name webappname -Tail

Bu komut tarafından belirtilen web uygulamasına bağlar **-adı** parametresi ve web uygulama günlüğü olaylarını ortaya çıktıkları PowerShell penceresi için bilgi akışı başlatmak. Herhangi bir bilgi /LogFiles dizininde (d:/home/logfiles) depolanan dosyaları .txt, .log veya .htm bitiş yazılan yerel konsola akışla aktarılır.

Hataları gibi belirli olayları filtrelemek için kullanmak **-ileti** parametresi. Örneğin:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

HTTP gibi belirli günlük türlerini filtreleyecek şekilde kullanmak **-yolu** parametresi. Örneğin:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

Kullanılabilir yollarının listesini görmek için - ListPath parametresini kullanın.

> [!NOTE]
> Azure PowerShell yüklü değil veya Azure Aboneliğinizdeki kullanacak şekilde yapılandırmadıysanız, bkz. [Azure PowerShell kullanmak için nasıl](http://azure.microsoft.com/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Azure komut satırı arabirimi ile akış
Değiştirmek için akış günlük kaydı bilgilerini, yeni bir komut istemi, PowerShell, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

    az webapp log tail --name webappname --resource-group myResourceGroup

Bu komut, 'webappname' adlı web uygulamasına bağlanır ve web uygulama günlüğü olaylarını ortaya çıktıkları penceresine bilgi akışı başlatmak. Herhangi bir bilgi /LogFiles dizininde (d:/home/logfiles) depolanan dosyaları .txt, .log veya .htm bitiş yazılan yerel konsola akışla aktarılır.

Hataları gibi belirli olayları filtrelemek için kullanmak **--filtre** parametresi. Örneğin:

    az webapp log tail --name webappname --resource-group myResourceGroup --filter Error

HTTP gibi belirli günlük türlerini filtreleyecek şekilde kullanmak **--yolu** parametresi. Örneğin:

    az webapp log tail --name webappname --resource-group myResourceGroup --path http

> [!NOTE]
> Azure komut satırı arabirimi yüklü değil veya Azure Aboneliğinizdeki kullanacak şekilde yapılandırmadıysanız, bkz. [nasıl için Azure komut satırı arabirimi kullanan](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a> Nasıl yapılır: Tanılama günlükleri anlama
### <a name="application-diagnostics-logs"></a>Uygulama tanılama günlükleri
Uygulama tanılama günlüklerinin dosya sistemi, tablo depolama veya blob depolama depoladığınız bağlı olarak, .NET uygulamaları için belirli bir biçimde bilgileri depolar. Temel depolanan veriler aynı depolama üç arasında - tarih ve saat olayı, olay türü (bilgi, uyarı, hata) ve olay iletisi üretilen işlem kimliği olayın gerçekleştiği kümesidir.

**Dosya sistemi**

Dosya sistemine oturum ya da akış kullanılarak alınan her satırı aşağıdaki biçimdedir:

    {Date}  PID[{process ID}] {event type/level} {message}

Örneğin, bir hata olayı, aşağıdaki örneğe benzer görünür:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Dosya sistemi günlük kaydı yalnızca zaman, işlem kimliği, olay düzeyi ve ileti sağlayan üç kullanılabilir yöntemi, en temel bilgileri sağlar.

**Tablo depolama**

Tablo depolama için oturum açarken, ek özellikler olay hakkında daha ayrıntılı bilgi yanı sıra tablo depolanan verileri aramayı kolaylaştırmak için kullanılır. Tabloda depolanan her varlık (satır) için aşağıdaki özellikleri (sütunları) kullanılır.

| Özellik adı | Değeri/biçimi |
| --- | --- |
| PartitionKey |Olayın yyyyMMddHH biçimindeki tarih/saat |
| RowKey |Bu varlık benzersiz olarak tanımlayan bir GUID değeri |
| Zaman damgası |Olayın saat ve tarihi |
| EventTickCount |Değer çizgisi biçiminde (daha fazla duyarlık) olayın gerçekleştiği saat ve tarihi |
| ApplicationName |Web uygulaması adı |
| Düzey |Olay düzeyi (uyarı, bilgi, hata) |
| EventID |Bu olay, olay kimliği<p><p>Varsayılan olarak hiçbiri belirtilmişse 0 |
| Örnek kimliği |Örnek web uygulamasının bile oluştu |
| PID |İşlem Kimliği |
| TID |Olay üretilen iş parçacığının iş parçacığı kimliği |
| İleti |Olay ayrıntı iletisi |

**Blob depolama**

Blob depolama için oturum açarken, verileri virgülle ayrılmış değerler (CSV) biçiminde depolanır. Tablo depolama, benzer olay hakkında daha ayrıntılı bilgi sağlamak için ek alanlar kaydedilir. Aşağıdaki özellikler, CSV her satır için kullanılır:

| Özellik adı | Değeri/biçimi |
| --- | --- |
| Tarih |Olayın saat ve tarihi |
| Düzey |Olay düzeyi (uyarı, bilgi, hata) |
| ApplicationName |Web uygulaması adı |
| Örnek kimliği |Olayın oluştuğu web uygulaması örneği |
| EventTickCount |Değer çizgisi biçiminde (daha fazla duyarlık) olayın gerçekleştiği saat ve tarihi |
| EventID |Bu olay, olay kimliği<p><p>Varsayılan olarak hiçbiri belirtilmişse 0 |
| PID |İşlem Kimliği |
| TID |Olay üretilen iş parçacığının iş parçacığı kimliği |
| İleti |Olay ayrıntı iletisi |

Blob olarak depolanan veriler aşağıdaki örneğe benzer olacaktır:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> Günlük ilk satırını sütun üst bilgilerini bu örnekte gösterilen şekilde içerir.
>
>

### <a name="failed-request-traces"></a>Başarısız istek izlemelerin
Başarısız istek izlemelerin adlı XML dosyalarında depolanan **fr ### .xml**. Oturum bilgilerini daha kolay hale getirmek için bir XSLT stil sayfası adlı **freb.xsl** XML dosyaları ile aynı dizinde sağlanır. XML dosyalarından birini Internet Explorer'da açın, Internet Explorer XSL stil sayfası izleme, aşağıdaki örneğe benzer bilgiler biçimlendirilmiş bir görünümünü sağlamak için kullanır:

![başarısız istek tarayıcıda görüntüleniyor](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Ayrıntılı hata günlükleri
Ayrıntılı Hata günlüklerini oluşmuş HTTP hataları hakkında daha ayrıntılı bilgi sağlayan HTML belgeleridir. Bunlar yalnızca HTML belgeleri olduğundan, bir web tarayıcısı kullanarak görüntülenebilir.

### <a name="web-server-logs"></a>Web sunucu günlükleri
Web sunucusu günlükleri kullanılarak biçimlendirilen [W3C Genişletilmiş günlük dosyası biçimini](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Bu bilgileri bir metin düzenleyicisi kullanarak okuyabilir veya gibi yardımcı programlar kullanılarak Ayrıştırılan [günlük ayrıştırıcısı](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Azure web uygulamaları tarafından üretilen günlükleri desteklemeyen **s-computername**, **s-ip**, veya **cs-version** alanları.
>
>

## <a name="nextsteps"></a> Sonraki adımlar
* [Web uygulamalarını izleme](web-sites-monitor.md)
* [Visual Studio'da Azure web apps sorunlarını giderme](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analiz web uygulaması günlüklerinin HDInsight içinde](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
>
>
