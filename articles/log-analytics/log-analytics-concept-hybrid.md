---
title: "Ortamınızı Azure günlük analizi ile veri toplamak | Microsoft Docs"
description: "Bu konu, veri toplamak ve şirket içi veya başka bir bulut ortamında günlük analizi ile barındırılan bilgisayarları izlemek nasıl anlamanıza yardımcı olur."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: magoedte
ms.openlocfilehash: 85fde471f0d99b976e319d552c6a031d63854cf4
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Günlük analizi ile ortamınızdaki bilgisayarlardan veri toplama

Azure günlük analizi toplamak ve bulunan Windows veya Linux bilgisayardan verileri hareket:

* [Azure sanal makineleri](log-analytics-quick-collect-azurevm.md) Log Analytics VM uzantısı kullanma 
* Fiziksel sunucularda veya sanal makinelerde olarak veri merkeziniz
* Sanal makineler bir bulut tarafından barındırılan hizmette Amazon Web Hizmetleri (AWS) gibi

Ortamınızda bulunan bilgisayarlar doğrudan bağlanması için günlük analizi ya da System Center Operations Manager 2012 R2 veya 2016 ile bu bilgisayarları izleme zaten varsa, işlemleri yönetme yönetim grubunuz günlük analizi ile tümleştirebilir ve Hizmet işlemleri işlemleri ve stratejisi koruyarak devam edin.  

## <a name="overview"></a>Genel Bakış

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Çözümleme ve toplanan verileri üzerinde hareket önce ilk yükleme ve aracılar için günlük analizi hizmetine veri göndermek istediğiniz tüm bilgisayarların bağlanmak gerekir. Kurulum, komut satırı kullanılarak, şirket içi bilgisayarları veya ile istenen durum yapılandırması (DSC) Azure Automation aracıları yükleyebilirsiniz. 

Aracı Linux ve Windows için TCP bağlantı noktası 443 giden günlük analizi hizmeti ile iletişim kurar ve bilgisayar Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya proxy sunucusu bağlanırsa gözden [aracı kullanmak için bir proxy sunucusu ile yapılandırma veya OMS ağ geçidi](#configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway) hangi yapılandırma değişiklikleri anlamak için uygulanması gerekir. System Center 2016 - Operations Manager veya Operations Manager 2012 R2'de, bilgisayarla izliyorsanız veri toplamak ve Hizmeti'ne iletmek ve tarafından izlenmesi için günlük analizi hizmeti ile çok konaklı olabilir [Operations Manager ](log-analytics-om-agents.md). Günlük analizi ile tümleşik bir Operations Manager yönetim grubu tarafından izlenen Linux bilgisayarlar veri kaynakları ve yönetim grubu ile ileri toplanan veriler için yapılandırma almaz. Linux Aracısı'nı yalnızca tek bir çalışma alanına raporlama desteklerken Windows Aracısı en fazla dört çalışma alanları bildirebilirsiniz.  

Yalnızca günlük analizi bağlanmak için aracı Linux ve Windows için değil, ayrıca Azure Automation ile ana bilgisayar karma Runbook çalışanı rolü ve değişiklik izleme ve güncelleştirme yönetimi gibi yönetim çözümleri bağlanmayı destekler.  Karma Runbook çalışanı rolü hakkında daha fazla bilgi için bkz: [Azure Otomasyon karma Runbook çalışanı](../automation/automation-offering-get-started.md#automation-architecture-overview).  

BT güvenlik ilkelerinizi bilgisayarları Internet'e bağlanmak için ağınızdaki izin vermiyorsa, aracı yapılandırma bilgilerini almak ve etkinleştirdiğiniz çözümüne bağlı olarak toplanan verileri göndermek için OMS ağ geçidine bağlanmak için yapılandırılabilir. Daha fazla bilgi ve günlük analizi hizmetine bir OMS ağ geçidi üzerinden iletişim kurmak için Linux veya Windows Aracısı yapılandırma adımları için bkz: [OMS ağ geçidini kullanarak OMS bilgisayarları bağlamak](log-analytics-oms-gateway.md). 

> [!NOTE]
> Windows için aracı yalnızca Aktarım Katmanı Güvenliği (TLS) 1.0 ve 1.1 destekler.  
> 

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce en düşük sistem gereksinimlerini karşıladığını doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

### <a name="windows-operating-system"></a>Windows işletim sistemi
Aşağıdaki Windows işletim sistemi sürümleri için Windows Aracısı resmi olarak desteklenir:

* Windows Server 2008 Service Pack 1 (SP1) veya daha yenisi
* Windows 7 SP1 ve sonraki sürümler.

#### <a name="network-configuration"></a>Ağ yapılandırması
Aşağıdaki liste Windows aracı günlük analizi ile iletişim kurmak için gerekli proxy ve güvenlik duvarı yapılandırma bilgilerini bilgileri. Günlük analizi hizmeti ağınızdan giden trafiğidir. 

| Aracı Kaynağı | Bağlantı Noktaları | HTTPS denetlemesini atlama|
|----------------|-------|------------------------|
|*.ods.opinsights.azure.com |443 | Evet |
|*.oms.opinsights.azure.com | 443 | Evet | 
|*.blob.core.windows.net | 443 | Evet | 
|*.azure-automation.net | 443 | Evet | 

### <a name="linux-operating-systems"></a>Linux işletim sistemleri
Aşağıdaki Linux dağıtımları resmi olarak desteklenir.  Ancak, Linux Aracısı'nı listelenmeyen diğer dağıtımlar da çalıştırabilirsiniz.  Aksi belirtilmedikçe, tüm ikincil sürümleri listelenen her ana sürümü için desteklenir.  

* Amazon Linux için 2015.09 2012.09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)  
* Oracle Linux 5, 6 ve 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* Debian GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

#### <a name="network-configuration"></a>Ağ yapılandırması
Aşağıdaki listeden Linux aracısının günlük analizi ile iletişim kurmak için gerekli proxy ve güvenlik duvarı yapılandırma bilgilerini bilgileri. Günlük analizi hizmeti ağınızdan giden trafiğidir. 

|Aracı Kaynağı| Bağlantı Noktaları |  
|------|---------|  
|*.ods.opinsights.azure.com | Bağlantı noktası 443|   
|*.oms.opinsights.azure.com | Bağlantı noktası 443|   
|*.blob.core.windows.net | Bağlantı noktası 443|   
|*.azure-automation.net | Bağlantı noktası 443|  

Linux Aracısı, bir proxy sunucusu veya OMS ağ geçidi HTTPS protokolünü kullanarak günlük analizi hizmeti ile iletişim kurmasını destekler.  Anonim ve temel kimlik doğrulaması (kullanıcı adı/parola) desteklenir.  Proxy sunucusu yüklemesi sırasında veya yükleme işleminden sonra proxy.conf yapılandırma dosyasını değiştirerek belirtilebilir.  

Proxy yapılandırma değeri sözdizimi aşağıdaki gibidir:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Proxy sunucunuz kimlik denetimi gerektirmezse Linux Aracısı'nı hala bir sözde kullanıcı/parola sağlayarak gerektirir. Bu, herhangi bir kullanıcı adı veya parola olabilir.

|Özellik| Açıklama |
|--------|-------------|
|Protokol | https |
|kullanıcı | Proxy kimlik doğrulaması için isteğe bağlı kullanıcı adı |
|password | Proxy kimlik doğrulaması için isteğe bağlı parola |
|proxyhost | Adres veya proxy sunucu/OMS ağ geçidi FQDN'sini |
|port | Proxy sunucu/OMS ağ geçidi için isteğe bağlı bağlantı noktası numarası |

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Gibi özel karakterleri kullanırsanız, "@", parolanızı değeri yanlış ayrıştırılır çünkü bir proxy bağlantı hatası alıyorsunuz.  Bu sorunu çözmek için bir aracı gibi kullanarak URL Parolada kodlamak [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Yükleme ve aracı yapılandırma 
Şirket içi bilgisayarları doğrudan Log Analytics'e ile bağlanma, gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tabloda, kuruluşunuzda en iyi çalıştığı belirlemek için her bir yöntemi vurgular.

|Kaynak | Yöntem | Açıklama|
|-------|-------------|-------------|
| Windows bilgisayar|- [El ile yükleme](log-analytics-agent-windows.md)<br>- [Azure Otomasyonu DSC](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Resource Manager şablonu Azure yığını ile](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Komut satırı veya Azure Otomasyonu DSC gibi otomatik bir yöntem kullanarak Microsoft Monitoring Aracısı'nı yüklemenize [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), ya da Microsoft dağıttıysanız, Azure Resource Manager şablonu ile Azure yığını, veri merkezinizdeki.| 
|Linux bilgisayarı| [El ile yükleme](log-analytics-quick-collect-linux-computer.md)|GitHub üzerinde barındırılan bir sarmalayıcı betik çağırma Linux aracısını yükleyin. | 
| System Center Operations Manager|[Operations Manager günlük analizi ile tümleştirme](log-analytics-om-agents.md) | Operations Manager ile günlük analizi iletmek için arasındaki tümleştirmeyi yapılandırma bir yönetim grubuna raporlama Linux ve Windows bilgisayarlardan toplanan verileri.|  

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [veri kaynakları](log-analytics-data-sources.md) Windows veya Linux sisteminizden veri toplamak kullanılabilir veri kaynakları anlamak için. 

* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için. 

* Hakkında bilgi edinin [çözümleri](log-analytics-add-solutions.md) işlevselliği için günlük analizi eklemek ve de OMS depoya veri toplama.