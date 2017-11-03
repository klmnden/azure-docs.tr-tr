---
title: "Karma bağlantıları oluşturma ve yönetme | Microsoft Docs"
description: "Karma bir bağlantı oluşturmak, bağlantıyı yönetmek ve karma Bağlantı Yöneticisi'ni yükleme hakkında bilgi edinin. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 7b8b9072d0e2fd054ca07873c0a9ce772dc2941e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Karma Bağlantıları Oluşturma ve Yönetme

> [!IMPORTANT]
> BizTalk Karma Bağlantılar kullanımdan kalktı ve yerine App Service Karma Bağlantılar kullanıma sunuldu. Var olan BizTalk Karma Bağlantılarınızı nasıl yöneteceğiniz de dahil olmak üzere daha fazla bilgi için bkz. [Azure App Service Karma Bağlantılar](../app-service/app-service-hybrid-connections.md).


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
Web uygulamaları kullanarak Azure portalında karma bağlantı oluşturulabilir **veya** BizTalk Services'ı kullanarak. 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

**BizTalk Services'da karma bağlantılar oluşturmak için**:

1. [Klasik Azure portalında](http://go.microsoft.com/fwlink/p/?LinkID=213885) oturum açın.
2. Sol gezinti bölmesinde seçin **BizTalk Services** ve BizTalk hizmetinizi seçin. 
   
    Mevcut bir BizTalk hizmetini yoksa, şunları yapabilirsiniz [BizTalk hizmeti oluşturma](biztalk-provision-services.md).
3. Seçin **karma bağlantılar** sekmesi:  
   ![Karma bağlantılar sekmesi][HybridConnectionTab]
4. Seçin **karma bir bağlantı oluşturmak** veya seçin **ekleme** görev çubuğunda düğmesi. Aşağıdakileri girin:
   
   | Özellik | Açıklama |
   | --- | --- |
   | Ad |Karma bağlantı adı benzersiz olmalıdır ve BizTalk hizmeti adıyla aynı olamaz. Herhangi bir ad girin, ancak amacı ile belirli. Örneklere şunlar dahildir:<br/><br/>Bordro*SQLServer*<br/>SupplyList*SharepointServer*<br/>Müşteriler*OracleServer* |
   | Ana bilgisayar adı |Tam ana bilgisayar adını girin, yalnızca ana bilgisayar adını veya şirket içi kaynağa IPv4 adresidir. Örneklere şunlar dahildir:<br/><br/>(sqlsunucum)<br/>*(sqlsunucum)*. *Etki alanı*. corp.*şirketiniz*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *Şirketiniz*.com<br/>10.100.10.10<br/><br/>IPv4 adresi kullanırsanız, istemci veya uygulama kodunuz IP adresini çözebilir değil olduğunu unutmayın. Bu konunun başında Not önemli bakın. |
   | Bağlantı noktası |Şirket içi kaynak bağlantı noktası numarasını girin. Örneğin, Web uygulamaları kullanıyorsanız, bağlantı noktası 80 veya 443 numaralı bağlantı noktasını girin. SQL Server kullanıyorsanız, 1433 numaralı bağlantı noktasını girin. |
5. Kurulumu tamamlamak için onay işaretini seçin. 

#### <a name="additional"></a>Ek
* Birden çok karma bağlantılar oluşturulabilir. Bkz: [BizTalk Services: sürümler grafiği](biztalk-editions-feature-chart.md) izin verilen bağlantı sayısı. 
* Her bir karma bağlantı için bağlantı dizeleri bir çift oluşturulur: uygulama anahtarları DİNLEME, gönderme ve şirket içi anahtarları. Her bir çifti birincil ve ikincil anahtar vardır. 

## <a name="LinkWebSite"></a>Azure App Service Web uygulaması veya mobil uygulama bağlama
Bir Web uygulaması veya mobil uygulama Azure App Service'te mevcut bir karma bağlantı bağlamak için seçin **varolan karma bağlantıyı kullan** karma bağlantılar dikey penceresinde. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>İçi karma Bağlantı Yöneticisi'ni yükleyin
Karma bağlantı oluşturulduktan sonra şirket içi kaynak karma Bağlantı Yöneticisi'ni yükleyin. Azure web uygulamaları ya da BizTalk hizmetinizi indirilebilir. BizTalk Services adımlar: 

1. [Klasik Azure portalında](http://go.microsoft.com/fwlink/p/?LinkID=213885) oturum açın.
2. Sol gezinti bölmesinde seçin **BizTalk Services** ve BizTalk hizmetinizi seçin. 
3. Seçin **karma bağlantılar** sekmesi:  
   ![Karma bağlantılar sekmesi][HybridConnectionTab]
4. Görev çubuğunda seçin **şirket içi Kurulum**:  
   ![Şirket içi Kurulumu][HCOnPremSetup]
5. Seçin **yükleme ve yapılandırma** çalıştırmak veya şirket içi sistemde karma Bağlantı Yöneticisi'ni indirin. 
6. Yüklemeyi başlatmak için onay işaretini seçin. 

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
Karma bağlantılar yönetmek için şunları yapabilirsiniz:

* Azure Portalı'nı kullanın ve BizTalk hizmetinize gidin. 
* Kullanım [REST API'leri](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopyala/yeniden karma bağlantı dizeleri
1. [Klasik Azure portalında](http://go.microsoft.com/fwlink/p/?LinkID=213885) oturum açın.
2. Sol gezinti bölmesinde seçin **BizTalk Services** ve BizTalk hizmetinizi seçin. 
3. Seçin **karma bağlantılar** sekmesi:  
   ![Karma bağlantılar sekmesi][HybridConnectionTab]
4. Karma bağlantıyı seçin. Görev çubuğunda seçin **bağlantıyı Yönet**:  
   ![Seçeneklerini yönetin][HCManageConnection]
   
    **Bağlantıyı Yönet** uygulama ve şirket içi bağlantı dizeleri listelenmiştir. Bağlantı dizeleri kopyalayabilir veya bağlantı dizesinde kullanılan erişim tuşunu yeniden oluşturun. 
   
    **Yeniden seçerseniz**, bağlantı dizesi içinde kullanılan paylaşılan erişim anahtarı değiştirilir. Şunları yapın:
   
   * Klasik Azure portalında seçin **anahtarları Eşitle** Azure uygulamasında.
   * Yeniden çalıştırma **şirket içi Kurulum**. Şirket içi Kur'u yeniden çalıştırdığınızda, şirket içi kaynağa güncelleştirilmiş birincil bağlantı dizesi kullanmak için otomatik olarak yapılandırılır.

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
[Klasik Azure portalını kullanarak BizTalk hizmeti oluşturma](biztalk-provision-services.md)  
[BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
