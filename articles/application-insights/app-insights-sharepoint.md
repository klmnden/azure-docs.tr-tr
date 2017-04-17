---
title: Bir SharePoint sitesini Application Insights ile izleme
description: "Yeni bir izleme anahtarı ile yeni bir uygulamayı izlemeye başlama"
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 2bfe5910-d673-4cf6-a5c1-4c115eae1be0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: f8c71601d033a2897c83a3a64306a39162360946
ms.lasthandoff: 04/07/2017


---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Bir SharePoint sitesini Application Insights ile izleme
Azure Application Insights, uygulamalarınızın kullanılabilirliğini, performansını ve kullanımını izler. Burada, bir SharePoint sitesi için nasıl ayarlayacağınızı öğreneceksiniz.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma
[Azure portalında](https://portal.azure.com) yeni bir Application Insights kaynağı oluşturun. Uygulama türü olarak ASP.NET’i seçin.

![Özellikler'e tıklayın, anahtarı seçin ve ctrl + C tuşlarına basın](./media/app-insights-sharepoint/01-new.png)

Uygulamanızla ilgili performans ve kullanım verilerini açılan dikey pencerede görürsünüz. Azure’da bir sonraki oturum açışınızda bu pencereye dönmek için başlangıç ekranında bunun bir kutucuğunu bulmanız gerekir. Alternatif olarak, pencereyi bulmak için Gözat'a tıklayın.

## <a name="add-our-script-to-your-web-pages"></a>Betiğimizi web sayfalarınıza ekleme
Hızlı Başlangıç’ta web sayfaları için betik alın:

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

Betiği, izlemek istediğiniz her sayfanın &lt;/head&gt; etiketinin hemen önüne ekleyin. Web sayfanızda bir ana sayfa varsa betiği buraya koyabilirsiniz. Örneğin, bir ASP.NET MVC projesinde View\Shared\_Layout.cshtml’ye koyarsınız

Betikte, telemetriyi Application Insights kaynağınıza yönlendiren izleme anahtarı bulunur.

### <a name="add-the-code-to-your-site-pages"></a>Kodu site sayfalarınıza ekleme
#### <a name="on-the-master-page"></a>Ana sayfada
Sitenin ana sayfasını düzenleyebiliyorsanız, bu sayede sitedeki her sayfa için izleme olanağına sahip olursunuz.

Ana sayfayı inceleyin ve SharePoint Designer’ı veya başka bir düzenleyiciyi kullanarak düzenleyin.

![](./media/app-insights-sharepoint/03-master.png)

Kodu </head> etiketinin hemen önüne ekleyin. 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>Veya belirli sayfalarda
Sınırlı sayıda sayfayı izlemek için betiği her sayfaya ayrı ayrı ekleyin. 

Bir web parçası ekleyip kod parçacığını buna ekleyin.

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Uygulamanızla ilgili verileri görüntüleme
Uygulamanızı yeniden dağıtın.

Uygulamanızın [Azure portalındaki](https://portal.azure.com) dikey penceresine dönün.

İlk olaylar Arama’da görünür. 

![](./media/app-insights-sharepoint/09-search.png)

Daha fazla veri bekliyorsanız, birkaç saniye geçtikten sonra Yenile’ye tıklayın.

Kullanıcı, oturum ve sayfa görüntüleme grafiklerini görmek için genel bakış dikey penceresinden **Kullanım analizi**’ne tıklayın:

![](./media/app-insights-sharepoint/06-usage.png)

Daha fazla ayrıntı görmek için herhangi bir grafiğe tıklayın. Örneğin, Sayfa Görüntülemeleri:

![](./media/app-insights-sharepoint/07-pages.png)

Veya Kullanıcılar:

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a>Kullanıcı Kimliğini Yakalama
Standart web sayfası kod parçacığı SharePoint’ten kullanıcı kimliğini yakalamaz, ancak küçük bir değişiklikle bunu yapabilirsiniz.

1. Application Insights’taki Temel Bileşenler açılan penceresinden uygulamanızın izleme anahtarını kopyalayın. 

    ![](./media/app-insights-sharepoint/02-props.png)

1. Aşağıdaki kod parçacığında geçen 'XXXX' ifadesini izleme anahtarıyla değiştirin. 
2. Betiği portaldan edindiğiniz kod parçacığı yerine SharePoint uygulamanıza ekleyin.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that the SP.UserProfiles.js file is loaded before the custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get the current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for the target user. 
    // To get the PersonProperties object for the current user, use the 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load the PersonProperties object and send the request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if the executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if the executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>Sonraki Adımlar
* Sitenizin kullanılabilirliğini izlemek için [web testleri](app-insights-monitor-web-app-availability.md).
* Diğer uygulama türleri için [Application Insights](app-insights-overview.md).

<!--Link references-->



