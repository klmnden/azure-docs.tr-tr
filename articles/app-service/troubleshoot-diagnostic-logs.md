---
title: Uygulamalar - Azure App Service için tanılama günlüğünü etkinleştirme
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
ms.custom: seodec18
ms.openlocfilehash: 37455c278d665d05636ec120ca91b76153e53d16
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835728"
---
# <a name="enable-diagnostics-logging-for-apps-in-azure-app-service"></a>Azure App Service'teki uygulamalar için tanılama günlüğünü etkinleştirme
## <a name="overview"></a>Genel Bakış
Azure, hatalarını ayıklamaya yardımcı olmak üzere yerleşik tanılama sağlar bir [App Service uygulaması](https://go.microsoft.com/fwlink/?LinkId=529714). Bu makalede, Azure tarafından günlüğe kaydedilen bilgilere nasıl yanı sıra tanılama günlüğünü etkinleştirme ve uygulamanız için izleme ekleme öğrenin.

Bu makalede [Azure portalında](https://portal.azure.com) ve Azure CLI'yı tanılama günlükleri ile çalışma. Visual Studio kullanarak tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Visual Studio'daki sorun giderme Azure](troubleshoot-dotnet-visual-studio.md).

## <a name="whatisdiag"></a>Web sunucusu tanılama ve uygulama tanılama
App Service, web sunucusunu hem web uygulamasının içinden bilgileri günlüğe kaydetme için tanılama işlevi sağlar. Bu mantıksal olarak ayrılır **web sunucu tanılamalarını** ve **uygulama tanılama**.

### <a name="web-server-diagnostics"></a>Web sunucusu tanılama
Etkinleştirmek veya günlükleri aşağıdaki türde devre dışı bırakabilirsiniz:

* **Ayrıntılı hata günlüğü** -HTTP durum kodu 400 veya üzeri sonuçları herhangi bir istek için ayrıntılı bilgiler. Bu sunucunun döndürülen hata kodu neden belirlemek yardımcı olabilecek bilgiler içerebilir. Bir HTML dosyası her bir hata oluşturulur, uygulamanın dosya sistemindeki ve 50 hataları (dosyalar) kadar korunur. HTML dosyaları sayısı 50'den fazla eski 26 dosyaları otomatik olarak silinir.
* **Başarısız istek izleme** -ayrıntılı bir izleme isteği ve her bir bileşende geçen süre işlemek için kullanılan IIS bileşenlerini de dahil olmak üzere, başarısız isteklerle bilgileri. Site performansı artırmak veya belirli bir HTTP hatası yalıtmak istiyorsanız kullanışlıdır. Bir klasöre her hata için uygulamanın dosya sisteminde oluşturulur. Dosya bekletme ilkeleri, yukarıda günlük ayrıntılı hata ile aynıdır.
* **Web sunucusu günlüğe kaydetme** -kullanarak HTTP işlemleri hakkında bilgi [W3C Genişletilmiş günlük dosyası biçimini](/windows/desktop/Http/w3c-logging). İşlenen isteklerin veya özel bir IP adresinden kaç isteklerdir sayısı gibi genel site ölçümleri belirlerken yararlı olacaktır.

### <a name="application-diagnostics"></a>Uygulama tanılamaları
Uygulama Tanılama web uygulaması tarafından üretilen bilgileri yakalamanıza olanak sağlar. ASP.NET uygulamalarında kullanabileceğiniz [System.Diagnostics.Trace](/dotnet/api/system.diagnostics.trace) uygulama tanılama günlüğüne bilgileri günlüğe kaydetmek için sınıf. Örneğin:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Çalışma zamanında gidermeye yardımcı olması için bu günlükleri alabilirsiniz. Daha fazla bilgi için [Visual Studio'da Azure App Service sorun giderme](troubleshoot-dotnet-visual-studio.md).

Bir uygulamaya içerik yayımlarken app Service dağıtım bilgilerini de kaydeder. Otomatik olarak gerçekleşir ve dağıtım günlüğü için yapılandırma ayarı yok. Dağıtım günlüğü bir dağıtım başarısız olmasının belirlemenize olanak tanır. Örneğin, bir özel dağıtım betiği kullanıyorsanız, betiği neden başarısız olduğunu belirlemek için dağıtım günlüğünü kullanabilirsiniz.

## <a name="enablediag"></a>Tanılamayı etkinleştirme
Tanılamayı etkinleştirmek için [Azure portalında](https://portal.azure.com), uygulamanızın sayfasına gidin ve tıklayın **Ayarları > tanılama günlükleri**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Günlükleri bölümü](./media/web-sites-enable-diagnostic-log/logspart.png)

Etkinleştirdiğinizde **uygulama tanılama**, ayrıca **düzeyi**. Aşağıdaki tabloda her düzeyde içerir günlükleri kategorilerini gösterilmektedir:

| Düzey| Dahil edilen günlük kategorileri |
|-|-|
|**Devre dışı** | None |
|**Hata:** | Hataları, kritik |
|**Uyarı** | Hataları, kritik uyarı|
|**Bilgi** | Uyarı, bilgi, hataları, kritik|
|**ayrıntılı** | İzleme, hata ayıklama, bilgi, uyarı, hata, kritik (tüm kategoriler) |
|-|-|

İçin **uygulama günlüğü**, geçici hata ayıklama amacıyla için dosya sistemi seçeneğini etkinleştirebilirsiniz. Bu seçenek otomatik olarak 12 saat içinde devre dışı bırakır. Günlüklerin yazılacağı bir blob kapsayıcısını seçmek için blob depolama seçeneğinde kapatabilirsiniz.

> [!NOTE]
> Şu anda yalnızca .NET uygulama günlükleri, blob depolamaya yazılabilir. Java, PHP, Node.js, Python uygulama günlükleri, dosya sisteminde (kod değişikliklerini dış depolama birimine günlüklerini yazma izni) olmadan yalnızca depolanabilir.
>
>

İçin **Web sunucusu günlüğü**, seçebileceğiniz **depolama** veya **dosya sistemi**. Seçme **depolama** bir depolama hesabı ve günlüklere yazılır bir blob kapsayıcısı seçmenizi sağlar. 

Dosya sisteminde günlüklerini depolamak, dosyaları FTP tarafından erişilen veya Azure CLI kullanarak bir ZIP arşivi indirilir.

Varsayılan olarak, günlükleri otomatik olarak silinmez (dışında **uygulama günlüğü (dosya sistemi)**). Günlükleri otomatik olarak silmek için ayarlarsınız **saklama dönemi (gün)** alan.

> [!NOTE]
> Varsa, [depolama hesabınızın erişim anahtarlarını yeniden oluştur](../storage/common/storage-create-storage-account.md), güncelleştirilmiş anahtarları kullanmak için kendi günlük yapılandırmasını sıfırlamanız gerekir. Bunu yapmak için:
>
> 1. İçinde **yapılandırma** sekmesinde, ilgili günlük kaydı özelliğini **kapalı**. Ayarlarınızı kaydedin.
> 2. Depolama hesabı blob'u için günlüğe kaydetme, yeniden etkinleştirin. Ayarlarınızı kaydedin.
>
>

Dosya sistemi veya blob depolama, herhangi bir birleşimini aynı anda etkinleştirilebilir ve tek bir günlük düzeyi yapılandırmalara sahip. Örneğin, hataları ve Uyarıları dosya sistemi günlük kaydı ile ayrıntılı bir düzeyde etkinleştirirken uzun vadeli günlüğü çözüm olarak, BLOB depolamaya oturum isteyebilirsiniz.

Depolama konumlarının her ikisinde de aynı temel bilgileri günlüğe kaydedilen olayları sunarken **blob depolama** günlüklerini örnek kimliği ve iş parçacığı kimliği günlüğünüdeğerindendahaayrıntılıbirzamandamgası(değerbiçimi)gibiekbilgi**dosya sistemi**.

> [!NOTE]
> Depolanan bilgi **blob depolama** yalnızca depolama istemcisi veya bu depolama sistemleri ile doğrudan çalışabilmeniz için uygulamanın kullanılarak erişilebilir. Örneğin, Visual Studio 2013, blob depolama keşfetmek için kullanılan bir Depolama Gezgini içerir ve HDInsight blob depolamada depolanan verilere erişebilir. Ayrıca, aşağıdakilerden birini kullanarak Azure depolama alanına erişen bir uygulama yazabilirsiniz [Azure SDK'ları](https://azure.microsoft.com/downloads/).
>

## <a name="download"></a> Nasıl Yapılır: İndirme günlükleri
Uygulama dosya sisteminde depolanmış tanılama bilgilerini FTP kullanarak doğrudan erişilebilir. Ayrıca Azure CLI kullanarak bir ZIP arşivi indirilebilir.

Günlükleri depolanan dizin yapısı aşağıdaki gibidir:

* **Uygulama günlükleri** -/LogFiles/uygulama /. Bu klasör, uygulama günlüğü tarafından üretilen bilgileri içeren bir veya daha fazla metin dosyalarını içerir.
* **Başarısız istek izlemelerin** -/ LogFiles/W3SVC ### /. Bu klasör, XSL dosyası ve bir veya daha fazla XML dosyalarını içerir. XSL dosyası biçimlendirme ve Internet Explorer'da görüntülendiğinde XML dosyalarının içeriğini filtreleme işlevi sağladığından, XML dosyaları gibi aynı dizine XSL dosyası indirme emin olun.
* **Ayrıntılı Hata günlüklerini** -/LogFiles DetailedErrors /. Bu klasör, ortaya çıkan HTTP hataları için kapsamlı bilgiler sağlayan bir veya daha fazla .htm dosyaları içerir.
* **Web sunucusu günlüklerini** -/LogFiles/http/RawLogs. Bu klasör bir veya daha fazla metin dosyaları olarak biçimlendirilmiş kullanarak [W3C Genişletilmiş günlük dosyası biçimini](/windows/desktop/Http/w3c-logging).
* **Dağıtım günlükleri** -/ LogFiles/Git. Bu klasör Azure App Service tarafından kullanılan iç dağıtım işlemleri tarafından oluşturulan günlükleri içeren, hem de Git dağıtımları için günlüğe kaydeder. Ayrıca dağıtım günlükleri D:\home\site\deployments altında bulabilirsiniz.

### <a name="ftp"></a>FTP

Uygulamanızın FTP sunucusu ile FTP bağlantısı açmak için bkz [uygulamanızı FTP/S kullanarak Azure App Service'e dağıtma](deploy-ftp.md).

Uygulamanızın FTP/S sunucusuna bağlandıktan sonra açın **LogFiles** günlük dosyalarının depolandığı klasöre,.

### <a name="download-with-azure-cli"></a>İle Azure CLI'yı indirme
Azure komut satırı arabirimini kullanarak günlük dosyalarını indirmek için yeni bir komut istemi, PowerShell, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

    az webapp log download --resource-group resourcegroupname --name appname

Bu komut adlı bir dosyaya ' appname' adlı uygulama için günlüklere kaydeder **webapp_logs.zip** geçerli dizin.

> [!NOTE]
> Azure CLI'yi yüklemediyseniz veya Azure Aboneliğinizdeki kullanacak şekilde yapılandırmadıysanız, bkz. [Azure CLI kullanmak için nasıl](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Nasıl yapılır: Application ınsights'ta günlüklerini görüntüle
Visual Studio Application Insights, filtreleme ve günlük arama ve günlükleri istekleri ve diğer olaylarla ilişkilendirmek için gereken araçları sağlar.

1. Projenizi Visual Studio'da Application Insights SDK'sını ekleyin.
   * Çözüm Gezgini'nde projenize sağ tıklayın ve Application Insights Ekle'ı seçin. Arabirim içeren bir Application Insights kaynağı oluşturma adım adım yol gösterir. [Daha fazla bilgi](../azure-monitor/app/asp-net.md)
2. İzleme dinleyicisi paketini projenize ekleyin.
   * Projenize sağ tıklayın ve NuGet paketlerini Yönet'i seçin. Seçin `Microsoft.ApplicationInsights.TraceListener` [daha fazla bilgi edinin](../azure-monitor/app/asp-net-trace-logs.md)
3. Projenize yükleyin ve günlük verileri üretmek için çalıştırabilirsiniz.
4. İçinde [Azure portalında](https://portal.azure.com/)yeni Application Insights kaynağınıza göz atın ve Aç **arama**. İstek, kullanım ve diğer telemetriyi birlikte günlük verilerinizi görmeniz gerekir. Bazı telemetri gelmesi birkaç dakika sürebilir: Yenile'ye tıklayın. [Daha fazla bilgi](../azure-monitor/app/diagnostic-search.md)

[Application Insights ile izleme performansıyla ilgili daha fazla bilgi edinin](../azure-monitor/app/azure-web-apps.md)

## <a name="streamlogs"></a> Nasıl Yapılır: Akış günlükleri
Bir uygulama geliştirirken, genellikle günlük kaydı bilgilerini neredeyse gerçek zamanlı görmek yararlı olur. Geliştirme ortamınızı Azure CLI'yı kullanarak günlük kaydı bilgilerini akışını yapabilirsiniz.

> [!NOTE]
> Akıştaki sıralama dışı olaylar sonuçlanabilir günlük dosyasına günlük arabellek bazı türleri yazın. Örneğin, bir kullanıcı bir sayfayı ziyaret ettiğinde oluşan bir uygulama günlük girişi sayfa isteği için karşılık gelen HTTP günlük girişi önce akıştaki görüntülenebilir.
>
> [!NOTE]
> Günlük akışını ayrıca depolanan herhangi bir metin dosyası yazılan bilgi akışları **D:\\giriş\\LogFiles\\**  klasör.
>
>

### <a name="streaming-with-azure-cli"></a>Azure CLI ile akış
Değiştirmek için akış günlük kaydı bilgilerini, yeni bir komut istemi, PowerShell, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

    az webapp log tail --name appname --resource-group myResourceGroup

Bu komut, 'appname' adlı uygulamaya bağlanır ve uygulama günlüğü olaylarını ortaya çıktıkları penceresine bilgi akışı başlatmak. Herhangi bir bilgi /LogFiles dizininde (d:/home/logfiles) depolanan dosyaları .txt, .log veya .htm bitiş yazılan yerel konsola akışla aktarılır.

Hataları gibi belirli olayları filtrelemek için kullanmak **--filtre** parametresi. Örneğin:

    az webapp log tail --name appname --resource-group myResourceGroup --filter Error

HTTP gibi belirli günlük türlerini filtreleyecek şekilde kullanmak **--yolu** parametresi. Örneğin:

    az webapp log tail --name appname --resource-group myResourceGroup --path http

> [!NOTE]
> Azure CLI'yi yüklemediyseniz veya Azure Aboneliğinizdeki kullanacak şekilde yapılandırmadıysanız, bkz. [Azure CLI kullanmak için nasıl](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a> Nasıl Yapılır: Tanılama günlükleri anlama
### <a name="application-diagnostics-logs"></a>Uygulama tanılama günlükleri
Uygulama tanılama günlüklerinin dosya sistemi veya blob depolamaya depoladığınız bağlı olarak, .NET uygulamaları için belirli bir biçimde bilgileri depolar. 

Depolanan veri kümesini temel her iki depolama türlerinde - tarih ve saat olayı, olay türü (bilgi, uyarı, hata) ve olay iletisi üretilen işlem kimliği olayın gerçekleştiği aynıdır. Günlük depolama için dosya sistemini kullanarak günlük dosyalarını neredeyse anında güncelleştirildiğinden bir sorunu gidermek için anında erişim gerektiğinde yararlıdır. Dosyaları önbelleğe alınır ve ardından depolama kapsayıcısına bir zamanlamaya göre Temizlenen için blob depolama, arşivleme amacıyla kullanılır.

**Dosya sistemi**

Dosya sistemine oturum ya da akış kullanılarak alınan her satırı aşağıdaki biçimdedir:

    {Date}  PID[{process ID}] {event type/level} {message}

Örneğin, bir hata olayı, aşağıdaki örneğe benzer görünür:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Dosya sistemi günlük kaydı yalnızca zaman, işlem kimliği, olay düzeyi ve ileti sağlayan üç kullanılabilir yöntemi, en temel bilgileri sağlar.

**Blob depolama**

Blob depolama için oturum açarken, verileri virgülle ayrılmış değerler (CSV) biçiminde depolanır. Olay hakkında daha ayrıntılı bilgi sağlamak için ek alanlar günlüğe kaydedilir. Aşağıdaki özellikler, CSV her satır için kullanılır:

| Özellik adı | Değeri/biçimi |
| --- | --- |
| Tarih |Olayın saat ve tarihi |
| Düzey |Olay düzeyi (uyarı, bilgi, hata) |
| ApplicationName |Uygulama adı |
| Örnek kimliği |Üzerinde olayın gerçekleştiği uygulama örneği |
| EventTickCount |Değer çizgisi biçiminde (daha fazla duyarlık) olayın gerçekleştiği saat ve tarihi |
| EventID |Bu olay, olay kimliği<p><p>Varsayılan olarak hiçbiri belirtilmişse 0 |
| PID |İşlem Kimliği |
| TID |Olay üretilen iş parçacığının iş parçacığı kimliği |
| İleti |Olay ayrıntı iletisi |

Blob olarak depolanan veriler aşağıdaki örneğe benzer olacaktır:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> ASP.NET Core için günlük kaydı kullanılarak gerçekleştirilir [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) bu sağlayıcı mevduatlarını ek günlük dosyaları blob kapsayıcısına sağlayıcısı. Daha fazla bilgi için [ASP.NET Core Azure'da oturum](/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#logging-in-azure).
>
>

### <a name="failed-request-traces"></a>Başarısız istek izlemelerin
Başarısız istek izlemelerin adlı XML dosyalarında depolanan **fr ### .xml**. Oturum bilgilerini daha kolay hale getirmek için bir XSLT stil sayfası adlı **freb.xsl** XML dosyaları ile aynı dizinde sağlanır. XML dosyalarından birini Internet Explorer'da açın, Internet Explorer XSL stil sayfası izleme, aşağıdaki örneğe benzer bilgiler biçimlendirilmiş bir görünümünü sağlamak için kullanır:

![başarısız istek tarayıcıda görüntüleniyor](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

> [!NOTE]
> Portalda uygulamanızın sayfaya gitmek için biçimlendirilmiş başarısız istek izlemelerin görüntülemek için kolay bir yolu var. Sol menüden **Tanıla ve problemleri çözmenize**, ardından aramak **başarısız istek günlükleri izleme**, sonra göz atın ve istediğiniz izleme görüntülemek için simgeye tıklayın.
>

### <a name="detailed-error-logs"></a>Ayrıntılı hata günlükleri
Ayrıntılı Hata günlüklerini oluşmuş HTTP hataları hakkında daha ayrıntılı bilgi sağlayan HTML belgeleridir. Bunlar yalnızca HTML belgeleri olduğundan, bir web tarayıcısı kullanarak görüntülenebilir.

### <a name="web-server-logs"></a>Web sunucu günlükleri
Web sunucusu günlükleri kullanılarak biçimlendirilen [W3C Genişletilmiş günlük dosyası biçimini](/windows/desktop/Http/w3c-logging). Bu bilgileri bir metin düzenleyicisi kullanarak okuyabilir veya gibi yardımcı programlar kullanılarak Ayrıştırılan [günlük ayrıştırıcısı](https://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Azure App Service tarafından üretilen günlükleri desteklemeyen **s-computername**, **s-ip**, veya **cs-version** alanları.
>
>

## <a name="nextsteps"></a> Sonraki adımlar
* [Azure uygulama hizmetleri nasıl izlenir?](web-sites-monitor.md)
* [Azure App Service'te Visual Studio sorunlarını giderme](troubleshoot-dotnet-visual-studio.md)
* [HDInsight uygulama günlüklerine analiz edin](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
