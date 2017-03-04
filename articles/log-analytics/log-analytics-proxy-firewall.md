---
title: "Azure Log Analytics&quot;te ara sunucu ve güvenlik duvarı ayarlarını yapılandırma | Microsoft Docs"
description: "Aracılarınız veya OMS hizmetlerinizin belirli bağlantı noktalarını kullanmaları gerektiğinde ara sunucu ve güvenlik duvarı ayarlarını yapılandırın."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b55ebd80-efd4-4220-971b-c18aea1b1ab2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/10/2017
ms.author: banders;magoedte
translationtype: Human Translation
ms.sourcegitcommit: 6a527fa303f1e2bd06ac662e545d6b6a1d299fb4
ms.openlocfilehash: cd06dfd498540970dc8ed29650f4d9e3ca57939b
ms.lasthandoff: 02/10/2017


---
# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Log Analytics'te ara sunucu ve güvenlik duvarı ayarlarını yapılandırma
Log Analytics için ara sunucu ve güvenlik duvarı ayarlarını yapılandırmaya yönelik eylemler, kullanmakta olduğunuz aracı türüne göre farklılık gösterir. Kullandığınız aracı türüne göre aşağıdaki bölümleri gözden geçirin.

## <a name="settings-for-the-oms-gateway"></a>OMS Ağ Geçidi ayarları

Aracılarınız İnternet erişimine sahip değilse, kendi ağ kaynaklarını kullanarak verilerini OMS Ağ Geçidine gönderebilir. Ağ Geçidi bu verileri toplar ve aracılarınız adına OMS hizmetine gönderir.

OMS Ağ Geçidi ile iletişim kuran aracıları, tam etki alanı adı ve özel noktası numarasını kullanarak yapılandırın.

OMS Ağ Geçidi, İnternet erişimi gerektirir. OMS Ağ Geçidi için, sahip olduğunuz aracı türlerinde kullandığınız ara sunucu veya güvenlik duvarı ayarlarının aynısını kullanın. OMS Ağ Geçidi hakkında daha fazla bilgi için bkz. [OMS Ağ Geçidini kullanarak bilgisayar ve cihazları OMS’ye bağlama](log-analytics-oms-gateway.md).

## <a name="configure-settings-with-the-microsoft-monitoring-agent"></a>Ayarları Microsoft İzleme Aracısı ile yapılandırma
Microsoft İzleme Aracısının OMS hizmetine bağlanması ve kaydolması için etki alanlarınızın bağlantı noktası numarasına ve URL'lere erişimi olmalıdır. Aracı ile OMS hizmeti arasındaki iletişim için bir ara sunucu kullanıyorsanız uygun kaynakların erişilebilir olduğundan emin olmanız gerekir. İnternet'e erişimi kısıtlamak için güvenlik duvarı kullanıyorsanız OMS'ye erişime izin vermek için güvenlik duvarınızı yapılandırmanız gerekir. Aşağıdaki tablolar OMS'nin ihtiyaç duyduğu bağlantı noktalarını listeler.

| **Aracı Kaynağı** | **Bağlantı Noktaları** | **HTTPS denetlemesini atlama** |
| --- | --- | --- |
| \*.ods.opinsights.azure.com |443 |Evet |
| \*.oms.opinsights.azure.com |443 |Evet |
| \*.blob.core.windows.net |443 |Evet |
| \*.azure-automation.net |443 |Yes |
| ods.systemcenteradvisor.com |443 | |

Denetim Masası'nı kullanarak Microsoft İzleme Aracısı'na ilişkin ara sunucu ayarlarını yapılandırmak için aşağıdaki yordamı kullanabilirsiniz. Yordamı her bir sunucu için kullanmanız gerekecektir. Yapılandırmanız gereken birden çok sunucu olması durumunda, bu işlemi otomatikleştirmek için bir betik kullanmak sizin için daha kolay olabilir. Bu durumda, bir sonraki [Microsoft İzleme Aracısı için ara sunucu ayarlarını betik kullanarak yapılandırma](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script) yordamına bakabilirsiniz.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Denetim Masası'nı kullanarak Microsoft İzleme Aracısı için ara sunucu ayarlarını yapılandırma
1. **Denetim Masası**'nı açın.
2. **Microsoft İzleme Aracısı**'nı açın.
3. **Ara Sunucu Ayarları** sekmesine tıklayın.<br>  
   ![ara sunucu ayarları sekmesi](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)
4. **Ara sunucu kullan**'ı seçin ve gerekiyorsa gösterilen örneğe benzer şekilde URL ile bağlantı noktasını yazın. Ara sunucunuz kimlik doğrulaması gerektiriyorsa ara sunucuya erişmek için kullanıcı adını ve parolayı yazın.

Doğrudan sunuculara bağlanan her aracıya ilişkin ara sunucu ayarlarını belirlemek için çalıştırabileceğiniz bir PowerShell betiği oluşturmak amacıyla aşağıdaki yordamı kullanın.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Betik kullanarak Microsoft İzleme Aracısı için ara sunucu ayarlarını yapılandırma
Aşağıdaki örneği kopyalayın, ortamınıza özgü bilgilerle güncelleştirin, bir PS1 dosya adı uzantısıyla kaydedin ve ardından betiği doğrudan OMS hizmetine bağlanan her bilgisayardan çalıştırın.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)


## <a name="configure-settings-with-operations-manager"></a>Ayarları Operations Manager ile yapılandırma
Bir Operations Manager yönetim grubunun OMS hizmetine bağlanması ve kaydolması için etki alanlarınızın bağlantı noktası numarasına ve URL'lere erişimi olmalıdır. Operations Manager yönetim sunucusu ile OMS hizmeti arasındaki iletişim için bir ara sunucu kullanıyorsanız uygun kaynakların erişilebilir olduğundan emin olmanız gerekir. İnternet'e erişimi kısıtlamak için güvenlik duvarı kullanıyorsanız OMS'ye erişime izin vermek için güvenlik duvarınızı yapılandırmanız gerekir. Operations Manager yönetim sunucusu bir ara sunucunun arkasında olmasa bile, aracıları öyle olabilir. Bu durumda, Güvenlik ve Günlük Yönetimi çözümü verilerinin OMS web hizmetine gönderilebilmesi için ara sunucunun aracılarla aynı şekilde yapılandırılmış olması gerekir.

Operations Manager aracılarının OMS hizmetiyle iletişim kurabilmesi için, Operations Manager altyapınızın (aracılar dahil) doğru ara sunucu ayarları ve sürümüne sahip olması gerekir. Aracılara ilişkin ara sunucu ayarları Operations Manager konsolunda belirtilmiştir. Sürümünüz şunlardan biri olmalıdır:

* Operations Manager 2012 SP1 Güncelleştirme Paketi 7 veya üzeri
* Operations Manager 2012 R2 Güncelleştirme Paketi 3 veya üzeri

Bu görevlere ilişkin bağlantı noktaları aşağıdaki tablolarda listelenmiştir.

> [!NOTE]
> Aşağıdaki kaynaklardan bazıları, her ikisi de OMS'nin önceki sürümleri olan Danışman Öngörüleri ve Operasyonel Öngörüler'den bahseder. Ancak listelenen kaynaklar gelecekte değişecektir.
>
>

Aracı kaynakları ve bağlantı noktalarının listesi aşağıdadır:<br>

| **Aracı kaynağı** | **Bağlantı Noktaları** |
| --- | --- |
| \*.ods.opinsights.azure.com |443 |
| \*.oms.opinsights.azure.com |443 |
| \*.blob.core.windows.net/\* |443 |
| ods.systemcenteradvisor.com |443 |

<br>
Yönetim sunucusu kaynakları ve bağlantı noktalarının listesi aşağıdadır:<br>

| **Yönetim sunucusu kaynağı** | **Bağlantı Noktaları** | **HTTPS denetlemesini atlama** |
| --- | --- | --- |
| service.systemcenteradvisor.com |443 | |
| \*.service.opinsights.azure.com |443 | |
| \*.blob.core.windows.net |443 |Yes |
| data.systemcenteradvisor.com |443 | |
| ods.systemcenteradvisor.com |443 | |
| \*.ods.opinsights.azure.com |443 |Evet |
| \*.azure-automation.net |443 |Yes |

<br>
OMS ve Operations Manager konsolu kaynakları ve bağlantı noktalarının listesi aşağıdadır.<br>

| **OMS ve Operations Manager konsolu kaynağı** | **Bağlantı Noktaları** |
| --- | --- |
| service.systemcenteradvisor.com |443 |
| \*.service.opinsights.azure.com |443 |
| \*.live.com |Bağlantı noktası 80 ve 443 |
| \*.microsoft.com |Bağlantı noktası 80 ve 443 |
| \*.microsoftonline.com |Bağlantı noktası 80 ve 443 |
| \*.mms.microsoft.com |Bağlantı noktası 80 ve 443 |
| login.windows.net |Bağlantı noktası 80 ve 443 |

<br>

Operations Manager yönetim grubunuzu OMS hizmetiyle kaydetmek için aşağıdaki yordamları kullanın. Yönetim grubu ve OMS hizmeti arasında iletişim sorunları yaşıyorsanız OMS hizmetine veri iletimi konusundaki sorunları gidermek için doğrulama yordamlarını kullanın.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>OMS hizmeti uç noktaları için özel durumlar isteme
1. Operations Manager yönetim sunucusu için gerekli kaynakların sahip olabileceğiniz tüm güvenlik duvarları tarafından erişilebilir olduğundan emin olmak için daha önceden sunulmuş olan ilk tabloda yer alan bilgileri kullanın.
2. Operations Manager ve OMS'de bulunan İşletim konsolu için gerekli kaynakların sahip olabileceğiniz tüm güvenlik duvarları tarafından erişilebilir olduğundan emin olmak için daha önceden sunulmuş olan ilk tabloda yer alan bilgileri kullanın.
3. Internet Explorer ile bir ara sunucu kullanıyorsanız bunun yapılandırıldığından ve doğru çalıştığından emin olun. Doğrulamak için güvenli bir web bağlantısı (HTTPS) açabilirsiniz; örneğin [https://bing.com](https://bing.com). Güvenli web bağlantısı bir tarayıcıda çalışmıyorsa büyük olasılıkla bulutta yer alan web hizmetleri içeren Operations Manager yönetim konsolunda da çalışmayacaktır.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>Ara sunucuyu Operations Manager konsolunda yapılandırma
1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. **Operasyonel Öngörüler**'i genişletin ve ardından **Operasyonel Öngörüler Bağlantısı**'nı seçin.<br>  
   ![Operations Manager OMS Bağlantısı](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. OMS Bağlantısı görünümünde, **Ara Sunucuyu Yapılandır**'a tıklayın.<br>  
   ![Operations Manager OMS Bağlantısı Ara Sunucuyu Yapılandırma](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. Operasyonel Öngörüler Ayarlar Sihirbazı: Ara Sunucu, **Operasyonel Öngörüler Web Servisine erişmek için bir ara sunucu kullan**'ı seçin ve ardından bağlantı noktası numarasını içeren URL'yi yazın; örneğin, **http://myproxy:80**.<br>  
   ![Operations Manager OMS ara sunucu adresi](./media/log-analytics-proxy-firewall/proxy-om03.png)

### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Ara sunucunun kimlik doğrulaması gerektirmesi durumunda kimlik bilgilerini belirtme
 Ara sunucu kimlik bilgileri ve ayarları, OMS'ye raporlama yapacak olan yönetilen bilgisayarlara yayılmalıdır. Bu sunucular *Microsoft System Center Advisor İzleme Sunucusu Grubu*'nda olmalıdır. Kimlik bilgileri gruptaki her sunucunun kayıt defterinde şifrelenmiştir.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. **RunAs Yapılandırması** altında, **Profiller**'i seçin.
3. **System Center Advisor Farklı Çalıştır Profili Ara Sunucusu** profilini açın.<br>  
   ![System Center Advisor Farklı Çalıştır Ara Sunucusu profilinin görüntüsü](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Farklı Çalıştır Profili Sihirbazı'nda, bir Farklı Çalıştır hesabı kullanmak için **Ekle**'ye tıklayın. Yeni bir Farklı Çalıştır hesabı oluşturabilir veya mevcut bir hesabı kullanabilirsiniz. Doğrudan ara sunucuya geçiş yapmak için bu hesabın yeterli izinlere sahip olması gerekir.<br>   
   ![Farklı Çalıştır Profili Sihirbazı görüntüsü](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Yönetilecek hesabı belirlemek için, Nesne Araması kutusunu açmak amacıyla **Seçilen bir sınıf, grup veya nesne**'yi seçin.<br>  
   ![Farklı Çalıştır Profili Sihirbazı görüntüsü](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. **Microsoft System Center Advisor İzleme Sunucusu Grubu**'nu arayın ve ardından seçin.<br>  
   ![Nesne Araması kutusunun görüntüsü](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Farklı Çalıştır Hesabı Ekle kutusunu kapatmak için **Tamam**'a tıklayın.<br>  
   ![Farklı Çalıştır Profili Sihirbazı görüntüsü](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Sihirbazı tamamlayın ve değişiklikleri kaydedin.<br>  
   ![Farklı Çalıştır Profili Sihirbazı görüntüsü](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)

### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>OMS yönetim paketlerinin indirildiğini doğrulama
OMS'ye çözümler eklediyseniz bu çözümleri Operations Manager konsolunda **Yönetim** altında yönetim paketleri olarak görüntüleyebilirsiniz. Bunları hızlıca bulabilmek için *System Center Advisor* araması yapın.<br>  
   ![indirilen yönetim paketleri](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png)  <br>  
Operations Manager yönetim sunucusunda şu Windows PowerShell komutunu kullanarak da OMS yönetim paketlerini kontrol edebilirsiniz:

   ```  
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
   ```  

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Operations Manager'ın OMS hizmetine veri gönderdiğini doğrulama
1. Operations Manager yönetim sunucusunda Performans İzleyicisi'ni (perfmon.exe) açın ve **Performans İzleyicisi**'ni seçin.
2. **Ekle**'ye tıklayın ve ardından **Sistem Sağlığı Hizmeti Yönetim Grupları**'nı seçin.
3. **HTTP** ile başlayan tüm sayaçları ekleyin.<br>  
   ![sayaçları ekleme](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Operations Manager yapılandırması doğruysa OMS'de eklediğiniz yönetim paketlerini ve yapılandırılmış günlük toplama ilkesini temel alan olaylar ve diğer veri öğeleri için Sistem Sağlığı Hizmeti Yönetim sayaçlarına ilişkin etkinlikleri göreceksiniz.<br>  
   ![Etkinliği gösteren Performans İzleyicisi](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)

## <a name="next-steps"></a>Sonraki adımlar
* İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).
* Çözümler tarafından toplanan ayrıntılı bilgileri görüntülemek için [günlük aramaları](log-analytics-log-searches.md) hakkında bilgi edinin.

