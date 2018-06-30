---
title: Ortamınızı Azure günlük analizi ile veri toplamak | Microsoft Docs
description: Bu konu, veri toplamak ve şirket içi veya başka bir bulut ortamında günlük analizi ile barındırılan bilgisayarları izlemek nasıl anlamanıza yardımcı olur.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: a13c83fc0d35be1aec87cb5f2d2b19b0bf27f1bf
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133127"
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Günlük analizi ile ortamınızdaki bilgisayarlardan veri toplama

Azure günlük analizi toplamak ve bulunan Windows veya Linux bilgisayardan verileri hareket:

* [Azure sanal makineleri](log-analytics-quick-collect-azurevm.md) Log Analytics VM uzantısı kullanma 
* Fiziksel sunucularda veya sanal makinelerde olarak veri merkeziniz
* Sanal makineler bir bulut tarafından barındırılan hizmette Amazon Web Hizmetleri (AWS) gibi

Ortamınızda bulunan bilgisayarlar doğrudan bağlanması günlük analizi için veya 2016 System Center Operations Manager 2012 R2'de, bu bilgisayarlarla izlemekte olduğunuz veya sürüm 1801, işlemleri yönetme yönetim grubu ile tümleştirebilir Analytics oturum ve BT hizmet işlemleri işlemlerinizi korumaya devam.  

## <a name="overview"></a>Genel Bakış

![log-Analytics-Agent-Direct-Connect-Diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Çözümleme ve toplanan verileri üzerinde hareket önce ilk yükleme ve aracılar için günlük analizi hizmetine veri göndermek istediğiniz tüm bilgisayarların bağlanmak gerekir. Kurulum, komut satırı kullanılarak, şirket içi bilgisayarları veya ile istenen durum yapılandırması (DSC) Azure Automation aracıları yükleyebilirsiniz. 

Aracı Linux ve Windows için TCP bağlantı noktası 443 giden günlük analizi hizmeti ile iletişim kurar ve bilgisayar Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya proxy sunucusu bağlanırsa gözden [Önkoşullar bölümüne](#prerequisites) için gereken ağ yapılandırması anlayın.  BT güvenlik ilkelerinizi bilgisayarları Internet'e bağlanmak için ağ üzerinde izin vermiyorsa, ayarlayabilirsiniz bir [OMS ağ geçidi](log-analytics-oms-gateway.md) ve aracı günlük analizi için ağ geçidi üzerinden bağlanmak için yapılandırın. Aracı yapılandırma bilgilerini almak ve hangi veri toplama kuralları ve etkinleştirdiğiniz çözümleri bağlı olarak toplanan verileri gönderin. 

System Center 2016 - Operations Manager veya Operations Manager 2012 R2'de, bilgisayarla izliyorsanız veri toplamak ve Hizmeti'ne iletmek ve tarafından izlenmesi için günlük analizi hizmeti ile çok konaklı olabilir [Operations Manager ](log-analytics-om-agents.md). Günlük analizi ile tümleşik bir Operations Manager yönetim grubu tarafından izlenen Linux bilgisayarlar veri kaynakları ve yönetim grubu ile ileri toplanan veriler için yapılandırma almaz. Linux Aracısı'nı yalnızca tek bir çalışma alanına raporlama desteklerken Windows Aracısı en fazla dört çalışma alanları bildirebilirsiniz.  

Yalnızca günlük Analizi'ne bağlamak için aracı Linux ve Windows için değil, ana bilgisayar karma Runbook çalışanı rolü ve değişiklik izleme ve güncelleştirme yönetimi gibi yönetim çözümleri için Azure Otomasyonu de destekler.  Karma Runbook çalışanı rolü hakkında daha fazla bilgi için bkz: [Azure Otomasyon karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri
Aşağıdaki Windows işletim sistemi sürümleri için Windows Aracısı resmi olarak desteklenir:

* Windows Server 2008 Service Pack 1 (SP1) veya daha yenisi
* Windows 7 SP1 ve sonraki sürümler.

> [!NOTE]
> Windows için aracı yalnızca Aktarım Katmanı Güvenliği (TLS) 1.0 ve 1.1 destekler.  

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri
Aşağıdaki Linux dağıtımları resmi olarak desteklenir.  Ancak, Linux Aracısı'nı listelenmeyen diğer dağıtımlar da çalıştırabilirsiniz.  Aksi belirtilmedikçe, tüm ikincil sürümleri listelenen her ana sürümü için desteklenir.  

* Amazon Linux için 2015.09 2012.09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)  
* Oracle Linux 5, 6 ve 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* Debian GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri
Aşağıdaki listeden Linux ve Windows aracı günlük analizi ile iletişim kurmak için gerekli proxy ve güvenlik duvarı yapılandırma bilgilerini bilgileri.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı noktası 443 |Gelen ve giden|Evet |  
|*.oms.opinsights.azure.com |Bağlantı noktası 443 |Gelen ve giden|Evet |  
|*.blob.core.windows.net |Bağlantı noktası 443 |Gelen ve giden|Evet |  
|*.azure-automation.net |Bağlantı noktası 443 |Gelen ve giden|Evet |  


Azure Otomasyon karma Runbook çalışanı bağlanmak ve ortamınızda runbook'ları kullanmak için Otomasyon hizmeti ile kaydetmek için kullanmayı planlıyorsanız, bağlantı noktası numarasını ve bölümünde açıklanan URL'lere erişimi olmalıdır [ağınız için yapılandırma Karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md#network-planning). 

Windows ve Linux Aracısı, bir proxy sunucusu veya OMS ağ geçidi HTTPS protokolünü kullanarak günlük analizi hizmeti ile iletişim kurmasını destekler.  Anonim ve temel kimlik doğrulaması (kullanıcı adı/parola) desteklenir.  Hizmete doğrudan bağlı Windows aracı için proxy yapılandırmasını yüklemesi sırasında belirtilir veya [dağıtımdan sonra](log-analytics-agent-manage.md#update-proxy-settings) Denetim Masası'ndan veya PowerShell ile.  

Linux aracısı için proxy sunucusu yüklemesi sırasında belirtilir veya [yükleme sonrasında](/log-analytics-agent-manage.md#update-proxy-settings) proxy.conf yapılandırma dosyasını değiştirerek.  Linux Aracısı proxy yapılandırma değeri sözdizimi aşağıdaki gibidir:

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
> Gibi özel karakterleri kullanırsanız, "\@" parolanızı, değer yanlış ayrıştırılır çünkü bir proxy bağlantı hatası alıyorsunuz.  Bu sorunu çözmek için bir aracı gibi kullanarak URL Parolada kodlamak [URLDecode](https://www.urldecoder.org/).  

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