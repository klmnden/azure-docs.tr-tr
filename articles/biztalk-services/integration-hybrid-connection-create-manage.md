---
title: Karma bağlantıları oluşturma ve yönetme | Microsoft Docs
description: Karma bir bağlantı oluşturmak, bağlantıyı yönetmek ve karma Bağlantı Yöneticisi'ni yükleme hakkında bilgi edinin. MABS, WABS
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
ms.openlocfilehash: 1751d33b5f6f6a506654daedd15bbd75ae271483
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
ms.locfileid: "26628857"
---
# <a name="create-and-manage-hybrid-connections"></a>Karma Bağlantıları Oluşturma ve Yönetme

> [!IMPORTANT]
> BizTalk Karma Bağlantılar kullanımdan kalktı ve yerine App Service Karma Bağlantılar kullanıma sunuldu. Var olan BizTalk Karma Bağlantılarınızı nasıl yöneteceğiniz de dahil olmak üzere daha fazla bilgi için bkz. [Azure App Service Karma Bağlantılar](../app-service/app-service-hybrid-connections.md).

>[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="overview-of-the-steps"></a>Adımlara genel bakış
1. Karma bağlantı girerek oluşturabilirsiniz **ana bilgisayar adı** veya **FQDN** özel ağınızda şirket içi kaynak.
2. Azure web uygulamaları veya Azure mobil uygulamalar için karma bağlantı bağlayın.
3. Şirket içi kaynakta karma Bağlantı Yöneticisi'ni yükleyin ve belirli karma bağlantısı. Azure Portalı'nı yüklemek ve bağlamak için bir tek tıklamalı deneyimi sağlar.
4. Karma bağlantılar ve bağlantı anahtarları yönetin.

Bu konu, aşağıdaki adımları listeler. 

> [!IMPORTANT]
> Bir IP adresi için bir karma bağlantı uç noktasının ayarlamak mümkündür. Bir IP adresi kullanırsanız, olabilir veya şirket içi kaynağa bağlı olarak, istemci ulaşabilir değil. Karma bağlantının DNS araması yaparsanız istemcide bağlıdır. Çoğu durumda, **istemci** uygulama kodunuz. İstemci bir DNS araması yapmıyorsa (Bu bir etki alanı adı (x.x.x.x) değilmiş gibi IP adresini çözümlemeye değil), trafik karma bağlantı üzerinden gönderilmez.
> 
> Sahte (kod) örneğin tanımladığınız **10.4.5.6** , şirket içi ana bilgisayarı olarak:
> 
> **Aşağıdaki senaryoyu çalışır:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Aşağıdaki senaryoyu çalışmıyor:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Karma bağlantı oluşturma
Karma bağlantı oluşturulabilir [Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) **veya** kullanarak [BizTalk Services REST API'leri](https://msdn.microsoft.com/library/azure/dn232347.aspx). 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

#### <a name="additional"></a>Ek
* Birden çok karma bağlantılar oluşturulabilir. Bkz: [BizTalk Services: sürümler grafiği](biztalk-editions-feature-chart.md) izin verilen bağlantı sayısı. 
* Her bir karma bağlantı için bağlantı dizeleri bir çift oluşturulur: uygulama anahtarları DİNLEME, gönderme ve şirket içi anahtarları. Her bir çifti birincil ve ikincil anahtar vardır. 

## <a name="LinkWebSite"></a>Azure App Service Web uygulaması veya mobil uygulama bağlama
Bir Web uygulaması veya mobil uygulama Azure App Service'te mevcut bir karma bağlantı bağlamak için seçin **varolan karma bağlantıyı kullan** karma bağlantılar dikey penceresinde. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>İçi karma Bağlantı Yöneticisi'ni yükleyin
Karma bağlantı oluşturulduktan sonra şirket içi kaynak karma Bağlantı Yöneticisi'ni yükleyin. Azure web uygulamaları ya da BizTalk hizmetinizi indirilebilir. 

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
* Karma Bağlantı Yöneticisi aşağıdaki işletim sistemlerinde yüklenebilir:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 + ve Windows Management Framework 4.0 + gerekli)
  * Windows Server 2012 (Windows Management Framework 4.0 + gerekli)
  * Windows Server 2012 R2
* Karma Bağlantı Yöneticisi'ni yükledikten sonra aşağıdakiler gerçekleşir: 
  
  * Azure üzerinde barındırılan karma bağlantının birincil uygulama bağlantı dizesi kullanmak için otomatik olarak yapılandırılır. 
  * Şirket içi kaynak birincil şirket içi bağlantı dizesi kullanmak için otomatik olarak yapılandırılır.
* Karma Bağlantı Yöneticisi geçerli şirket içi bağlantı dizesi yetkilendirme için kullanmanız gerekir. Azure Web uygulamaları veya Mobile Apps geçerli uygulama bağlantı dizesi yetkilendirme için kullanmanız gerekir.
* Karma bağlantılar, başka bir sunucuya başka bir örneği karma Bağlantı Yöneticisi'nin yükleyerek ölçeklendirebilirsiniz. Aynı adresi ilk şirket içi dinleyicisi olarak kullanmak için şirket içi dinleyicisini yapılandırın. Bu durumda, rastgele dağıtılmış (hepsini bir kez) arasında etkin şirket içi dinleyicileri trafiğidir. 

## <a name="ManageHybridConnection"></a>Karma bağlantılar yönetme

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) de iyi bir kaynaktır.

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopyala/yeniden karma bağlantı dizeleri

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) de iyi bir kaynaktır.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Karma bağlantı tarafından kullanılan şirket içi kaynakları denetlemek için Grup İlkesi kullanın
1. Karşıdan [karma Bağlantı Yöneticisi Yönetim Şablonları](http://www.microsoft.com/download/details.aspx?id=42963).
2. Dosyaları ayıklayın.
3. Grup İlkesi değiştirir bilgisayarda aşağıdakileri yapın:  
   
   * Kopyalama. ADMX dosyalarını *%WINROOT%\PolicyDefinitions* klasör.
   * Kopyalama. ADML dosyaları *%WINROOT%\PolicyDefinitions\en-us* klasör.

Kopyalandıktan sonra ilkeyi değiştirmek için Grup İlkesi Düzenleyicisi'ni kullanabilirsiniz.

## <a name="next"></a>Sonraki
[Karma bağlantılara genel bakış](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Microsoft Azure BizTalk hizmetlerinin yönetilmesi için REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Sürümler Grafiği](biztalk-editions-feature-chart.md)  
[BizTalk hizmeti oluşturma](biztalk-provision-services.md)  
[BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
