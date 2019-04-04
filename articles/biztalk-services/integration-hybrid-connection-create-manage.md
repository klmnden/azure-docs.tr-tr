---
title: Karma bağlantıları oluşturma ve yönetme | Microsoft Docs
description: Karma bağlantı oluşturma, bağlantıyı yönetmek ve karma Bağlantı Yöneticisi'ni yüklemek hakkında bilgi edinin. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 9d659262195fef0cc6871bac409dd5914b70f401
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916130"
---
# <a name="create-and-manage-hybrid-connections"></a>Karma Bağlantıları Oluşturma ve Yönetme

> [!IMPORTANT]
> BizTalk Karma Bağlantılar kullanımdan kalktı ve yerine App Service Karma Bağlantılar kullanıma sunuldu. Var olan BizTalk Karma Bağlantılarınızı nasıl yöneteceğiniz de dahil olmak üzere daha fazla bilgi için bkz. [Azure App Service Karma Bağlantılar](../app-service/app-service-hybrid-connections.md).
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="overview-of-the-steps"></a>Adımlara genel bakış
1. Girerek karma bağlantı oluşturma **ana bilgisayar adı** veya **FQDN** özel ağınızda şirket içi kaynak.
2. Karma bağlantı, Azure web apps veya Azure mobil uygulamaları bağlayın.
3. Şirket içi kaynağınızda karma Bağlantı Yöneticisi'ni yükleyin ve belirli karma bağlantısı. Azure portalını yükleyin ve bağlanmak için bir tek tıklamalı deneyimi sağlar.
4. Karma bağlantılar ve bağlantı anahtarları yönetin.

Bu konu, aşağıdaki adımları listeler. 

> [!IMPORTANT]
> Bir karma bağlantı uç noktası için bir IP adresi ayarlamak mümkündür. Bir IP adresi kullanıyorsanız, olabilir veya şirket içi kaynağa bağlı olarak, istemci ulaşmıyor olabilir. Karma bağlantı, DNS araması yaparsanız istemcide bağlıdır. Çoğu durumda **istemci** uygulama kodunuzun olması. İstemci bir DNS araması yapmazsa (bir etki alanı adı (x.x.x.x) kabul edildiğinde IP adresini çözümlemek deneyin değil), trafik karma bağlantı üzerinden gönderilmez.
> 
> Sahte (kod). Örneğin, tanımladığınız **10.4.5.6** şirket içi ana olarak:
> 
> **Aşağıdaki senaryoyu çalışır:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on premises host`
> 
> **Aşağıdaki senaryoda çalışmaz:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Karma bağlantı oluşturma
Karma bağlantı oluşturulabilir [Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) **veya** kullanarak [BizTalk Services REST API'leri](/previous-versions/azure/reference/dn232347(v=azure.100)). 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

#### <a name="additional"></a>Ek
* Birden çok karma bağlantıları oluşturulabilir. Bkz: [BizTalk Services: Sürümler grafiği](biztalk-editions-feature-chart.md) izin verilen bağlantı sayısı. 
* Her karma bağlantı, bağlantı dizeleri çifti ile oluşturulur: Gönder ve şirket içi DİNLEME anahtarlar uygulama anahtarları. Her birincil ve ikincil anahtar vardır. 

## <a name="LinkWebSite"></a>Azure App Service Web uygulaması veya mobil uygulamanızı bağlayın
Bir Web uygulaması veya mobil uygulama Azure App Service'te mevcut bir karma bağlantı bağlamak için seçin **var olan bir karma bağlantı kullanmak** karma bağlantılar dikey penceresinde. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>Yerinde karma Bağlantı Yöneticisi'ni yükleyin
Karma bağlantı oluşturulduktan sonra şirket içi kaynak üzerinde karma Bağlantı Yöneticisi'ni yükleyin. Azure web apps ya da BizTalk hizmetinizi indirilebilir. 

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
 
[Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) de iyi bir kaynaktır.

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Ek
* Karma Bağlantı Yöneticisi, şu işletim sistemlerine yüklenebilir:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 + ve Windows Management Framework 4.0 + gerekli)
  * Windows Server 2012 (Windows Management Framework 4.0 + gerekli)
  * Windows Server 2012 R2
* Karma Bağlantı Yöneticisi'ni yükledikten sonra aşağıdakiler gerçekleşir: 
  
  * Azure üzerinde barındırılan karma bağlantının birincil uygulama bağlantı dizesini kullanmak için otomatik olarak yapılandırılır. 
  * Şirket içi kaynak, birincil şirket içi bağlantı dizesini kullanmak için otomatik olarak yapılandırılır.
* Karma Bağlantı Yöneticisi geçerli şirket içi bağlantı dizesi, yetkilendirme için kullanmanız gerekir. Geçerli uygulama bağlantı dizesi, Mobile Apps ve Azure Web Apps için yetkilendirme kullanmanız gerekir.
* Karma bağlantılar, karma Bağlantı Yöneticisi'nin başka bir örneğini başka bir sunucuya yükleyerek ölçeklendirebilirsiniz. İlk şirket içi dinleyicisi olarak aynı adresini kullanmak için şirket içi dinleyiciyi yapılandırın. Bu durumda, rastgele dağıtılmış (hepsini bir kez deneme) arasında şirket etkin dinleyiciler trafiğidir. 

## <a name="ManageHybridConnection"></a>Karma bağlantılar'ı yönetme

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) de iyi bir kaynaktır.

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Karma bağlantı dizelerini kopyasını/regenerate

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) de iyi bir kaynaktır.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Karma bağlantı tarafından kullanılan şirket içi kaynakları denetlemek için Grup İlkesi kullanın
1. İndirme [karma Bağlantı Yöneticisi Yönetim Şablonları](https://www.microsoft.com/download/details.aspx?id=42963).
2. Dosyaları ayıklayın.
3. Grup İlkesi değiştiren bilgisayarda aşağıdakileri yapın:  
   
   * Kopyalama. ADMX dosyalarını *%WINROOT%\PolicyDefinitions* klasör.
   * Kopyalama. ADML dosyaları *%WINROOT%\PolicyDefinitions\en-us* klasör.

Kopyalandıktan sonra ilkeyi değiştirmek için Grup İlkesi Düzenleyicisi'ni kullanabilirsiniz.

## <a name="next"></a>Sonraki
[Karma Bağlantıya Genel Bakış](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Microsoft azure'da BizTalk hizmetlerinin yönetilmesi için REST API](/previous-versions/azure/reference/dn232347(v=azure.100))  
[BizTalk Services: Sürümler Grafiği](biztalk-editions-feature-chart.md)  
[BizTalk Hizmeti oluşturma](biztalk-provision-services.md)  
[BizTalk Services: Pano, İzleyici ve ölçek sekmeleri](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
