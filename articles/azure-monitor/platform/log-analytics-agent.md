---
title: Azure Log Analytics aracısını log verileri toplama | Microsoft Docs
description: Bu konuda, veri toplamak ve Azure'da barındırılan bilgisayarlarını izlemek nasıl anlamanıza yardımcı olur şirket içinde veya diğer bulut ortamında Log Analytics ile.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: magoedte
ms.openlocfilehash: 081d65f60eab4e2412a5dd14c3a63a18598e3b8a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67146311"
---
# <a name="collect-log-data-with-the-log-analytics-agent"></a>Log Analytics aracısını log verileri toplama

Daha önce Microsoft Monitoring Agent (MMA) veya OMS Linux aracısı olarak da adlandırılan, Azure Log Analytics aracısını kapsamlı yönetimi için şirket içi makinelerde tarafından izlenen bilgisayarlar geliştirilmiştir [System Center Operations Manager ](https://docs.microsoft.com/system-center/scom/)ve buluttaki sanal makineler. Windows ve Linux aracılarını eklemek için bir Azure İzleyici ve Log Analytics çalışma alanınızı yanı sıra benzersiz günlükleri veya izleme çözümünde tanımlanan ölçümleri farklı kaynaklardan toplanan günlük verilerini depolar. 

Bu makalede, aracı, sistem ve ağ gereksinimleri ve farklı dağıtım yöntemlerini ayrıntılı bir genel bakış sağlar.   

## <a name="overview"></a>Genel Bakış

![Log Analytics Aracısı iletişimi diyagramı](./media/log-analytics-agent/log-analytics-agent-01.png)

Çözümleme ve toplanan verilerin hareket önce ilk yükleme ve aracıları için tüm veriler Azure İzleyici'hizmetine göndermek istediğiniz makineleri bağlanmak gerekir. Windows ve Linux için ve Kurulum, komut satırı kullanarak karma bir ortamı veya makineleri ile Desired State Configuration (DSC) Azure automation'da Azure Log Analytics VM uzantısı kullanarak, Azure sanal makinelerinde aracıları yükleyebilirsiniz. 

Linux ve Windows için aracı Azure İzleyici'hizmetine giden TCP bağlantı noktası 443 üzerinden iletişim kurar ve makinenin Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya Ara sunucu üzerinden bağlanıyorsa ağ yapılandırmasını öğrenmek için aşağıdaki gereksinimleri gözden geçirin. Gerekli. BT güvenlik ilkeleriniz bilgisayarların Internet'e bağlanmak için ağ üzerinde izin vermiyorsa, ayarlayabileceğiniz bir [Log Analytics gateway](gateway.md) ve ardından aracıyı Azure İzleyici günlüklerine ağ geçidi üzerinden bağlanmak için yapılandırın. Aracı yapılandırma bilgilerini almak ve veri toplama kuralları bağlı olarak hangi verilerin toplanan ve izleme çözümleri, çalışma alanınızda etkinleştirmiş olmanız gerekir. 

System Center Operations Manager 2012 R2 veya sonraki bir bilgisayar izliyorsanız, veri toplamak ve hizmete iletmek ve tarafından izlenmesi için Azure İzleyici hizmeti ile birden çok girişli olabilir [Operations Manager](../../azure-monitor/platform/om-agents.md). Linux bilgisayarlar ile aracı sistem durumu hizmet bileşenini Windows aracısı yapar ve bilgiler toplanır ve onun adına bir yönetim sunucusu tarafından işleme içermez. Linux bilgisayarları Operations Manager ile izlenen farklı olduğundan, bunlar değil yapılandırmasını alamıyor veya doğrudan veri toplamak ve aracıyla yönetilen bir Windows sistemi gibi yönetim grubu üzerinden iletin. Sonuç olarak, Linux bilgisayarları Operations Manager Raporlama ile bu senaryo desteklenmez ve Linux bilgisayar için yapılandırmanız gereken [bir Operations Manager yönetim grubuna rapor](../platform/agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group) iki bir Log Analytics çalışma alanı adımlar.

Linux Aracısı, yalnızca tek bir çalışma alanına raporlama desteklese de Windows aracı en fazla dört Log Analytics çalışma alanlarını, rapor edebilirsiniz.  

Linux ve Windows için aracı yalnızca Azure İzleyicisi'ne bağlamak için değil, ayrıca karma Runbook çalışanı rolü ve diğer hizmetler gibi barındırmak için Azure Otomasyonu desteklediği [değişiklik izleme](../../automation/change-tracking.md), [güncelleştirmeyönetimi](../../automation/automation-update-management.md), ve [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md). Karma Runbook çalışanı rolü hakkında daha fazla bilgi için bkz. [Azure Otomasyon karma Runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri
Aşağıdaki Windows işletim sistemi sürümleri Windows aracısı için resmi olarak desteklenir:

* Windows Server 2019
* Windows Server 2008 R2, 2012, 2012 R2'de, 2016, sürüm 1709 ve 1803
* Windows 7 SP1 ve üzeri

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri
Bu bölümde, desteklenen Linux dağıtımları hakkında ayrıntılar sağlar.    

Ağustos 2018 tarihinden sonraki sürümleri ile başlayarak, aşağıdaki değişiklikleri destek modelimizi yapıyoruz:  

* Yalnızca sunucu sürümleri desteklenir, istemci değil.  
* Yeni sürümlerini [Azure destekli Linux dağıtımları](../../virtual-machines/linux/endorsed-distros.md) her zaman desteklenir.  
* Listelenen her ana sürümünün tüm ikincil sürümleri desteklenir.
* Üreticinin destek bitiş tarihi geçmiş sürümleri desteklenmez.  
* AMI yeni sürümleri desteklenmez.  
* SSL çalıştıran sürümler 1.x varsayılan olarak desteklenir.

>[!NOTE]
>Bir distro veya destek modelimizi hizalama değil ve şu anda desteklenmeyen bir sürümünü kullanıyorsanız, Microsoft destek sürümleri çatalı aracı ile ilgili Yardım sağlamaz sıkan Bu deponun çatalını oluşturmanız önerilir.

* Amazon Linux 2017.09 (x 64)
* CentOS Linux 6 (x86/x64) ve 7 (x 64)  
* Oracle Linux 6 ve 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) ve 7 (x 64)
* Debian GNU/Linux 8 ve 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) ve 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x 64)

>[!NOTE]
>OpenSSL 1.1.0 yalnızca x86_x64 platformları (64-bit) ve OpenSSL 1.x herhangi bir platformda desteklenmiyor ' den önceki desteklenir.
>

### <a name="agent-prerequisites"></a>Aracı önkoşulları

Aşağıdaki tabloda, aracının yüklü olması desteklenen Linux dağıtımları için gereken paketleri vurgulanmaktadır.

|Gerekli paket |Açıklama |En düşük sürüm |
|-----------------|------------|----------------|
|Glibc |    GNU C Kitaplığı | 2.5-12 
|Openssl    | OpenSSL kitaplıkları | 1.0.x veya 1.1.x |
|Curl | cURL web istemcisi | 7.15.5 |
|Python-ctypes | | 
|PAM | Eklenebilir kimlik doğrulaması modülleri | | 

>[!NOTE]
>Rsyslog veya syslog-ng, syslog iletileri toplamak için gereklidir. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) 5 sürümünde varsayılan syslog daemon'u syslog olay toplaması için desteklenmiyor. Bu dağıtımları sürümünden Syslog verilerini toplamak için rsyslog daemon'u yüklenmeli ve sysklog değiştirmek için yapılandırılmış.

## <a name="tls-12-protocol"></a>TLS 1.2 Protokolü
Azure İzleyici günlüklerine Aktarımdaki verilerin güvenliğini sağlamak üzere en az kullanmak üzere yapılandırmak için önemle öneririz Aktarım Katmanı Güvenliği (TLS) 1.2. TLS/Güvenli Yuva Katmanı (SSL) daha eski sürümleri, savunmasız bulundu ve bunlar yine de şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**.  Ek bilgi için gözden [TLS 1.2 kullanarak güvenli bir şekilde veri gönderen](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri
Azure İzleyici günlüklerine ile iletişim kurmak Linux ve Windows aracısı için gerekli proxy ve güvenlik duvarı yapılandırma bilgileri listesi aşağıdaki bilgileri.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı noktası 443 |Giden|Evet |  
|*.oms.opinsights.azure.com |Bağlantı noktası 443 |Giden|Evet |  
|*.blob.core.windows.net |Bağlantı noktası 443 |Giden|Evet |  
|*.azure-automation.net |Bağlantı noktası 443 |Giden|Evet |  

Azure kamu için gerekli güvenlik duvarı için bilgi [Azure kamu Yönetimi](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Bağlanmak ve ortamınızda runbook'ları kullanmak için Otomasyon hizmetine kaydetmek için Azure Otomasyon karma Runbook çalışanı'nı kullanmayı planlıyorsanız, bağlantı noktası numarası ve bölümünde açıklanan URL'lere erişimi olmalıdır [ağınız için yapılandırma Karma Runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md#network-planning). 

Windows ve Linux Aracısı, proxy sunucusu veya Azure İzleyici'de HTTPS protokolünü kullanarak ağ geçidi Log Analytics ile iletişim kurulurken destekler.  Anonim ve temel kimlik doğrulaması (kullanıcı adı/parola) desteklenir.  Yüklemesi sırasında belirtilen hizmete doğrudan bağlı Windows aracısı için proxy yapılandırmasını veya [dağıtımdan sonra](agent-manage.md#update-proxy-settings) Denetim Masası'ndan veya PowerShell ile.  

Linux aracısı için proxy sunucusu yüklemesi sırasında belirtilen veya [yüklemeden sonra](agent-manage.md#update-proxy-settings) proxy.conf yapılandırma dosyasını değiştirerek.  Linux Aracısı proxy yapılandırması değeri sözdizimi aşağıdaki gibidir:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Ara sunucunuz kimlik doğrulamasından geçmesini gerektirmiyorsa, Linux Aracısı hala bir sanal kullanıcı/parola sağlama gerektirir. Bu, herhangi bir kullanıcı adı veya parola olabilir.

|Özellik| Açıklama |
|--------|-------------|
|Protokol | https |
|kullanıcı | Ara sunucu kimlik doğrulaması için isteğe bağlı bir kullanıcı adı |
|password | Proxy kimlik doğrulaması için isteğe bağlı parola |
|proxyhost | Adresi veya FQDN proxy sunucusu/Log Analytics ağ geçidi |
|port | Proxy sunucusu/Log Analytics ağ geçidi için isteğe bağlı bağlantı noktası numarası |

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Gibi özel karakterleri kullanırsanız "\@" parolanızı, değeri yanlış ayrıştırılır için proxy bağlantı hatası alırsınız.  Bu sorunu geçici olarak çözmek için parola gibi bir araç kullanarak URL kodlama [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Aracıyı yükle ve yapılandır 
Azure aboneliğiniz veya karma ortamda makinelerin doğrudan Azure İzleyici günlüklerine ile bağlanma, gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tabloda, kuruluşunuzda en iyi şekilde çalıştığı belirlemek için her bir yöntemin vurgulanmaktadır.

|Kaynak | Yöntem | Açıklama|
|-------|-------------|-------------|
|Azure VM| -Log Analytics VM uzantısı için [Windows](../../virtual-machines/extensions/oms-windows.md) veya [Linux](../../virtual-machines/extensions/oms-linux.md) Azure CLI kullanarak veya Azure Resource Manager şablonu ile<br>- [Azure portalından el ile](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json). | Uzantı, Azure sanal makinelerinde Log Analytics aracısını yükler ve bunları mevcut bir Azure İzleyici çalışma alanına kaydeder.|
| Hibrit Windows bilgisayarı|- [El ile yükleme](agent-windows.md)<br>- [Azure Otomasyonu DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Azure Stack ile Resource Manager şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Komut satırını veya Azure Automation DSC gibi otomatikleştirilmiş bir yöntem kullanarak Microsoft Monitoring agent yükleme [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), ya da Microsoft dağıttıysanız, Azure Resource Manager şablonu ile Azure Stack, veri merkezinizdeki.| 
| Hibrit Linux bilgisayarı| [El ile yükleme](../../azure-monitor/learn/quick-collect-linux-computer.md)|Github'da barındırılan bir sarmalayıcı betik çağırma Linux için aracıyı yükleyin. | 
| System Center Operations Manager|[Operations Manager'ı Log Analytics ile tümleştirme](../../azure-monitor/platform/om-agents.md) | Operations Manager ve Azure İzleyici günlüklerine arasında bütünleşmeyi yapılandırmadan İleri toplanan verileri Windows bilgisayarlardan bir yönetim grubuna raporlama.|  

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [veri kaynakları](../../azure-monitor/platform/agent-data-sources.md) Windows veya Linux sisteminizden veri toplamak kullanılabilir veri kaynaklarını anlamasına olanak. 

* Hakkında bilgi edinin [oturum sorguları](../../azure-monitor/log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 

* Hakkında bilgi edinin [izleme çözümleri](../../azure-monitor/insights/solutions.md) işlevselliği eklemek için Azure İzleyici ve ayrıca Log Analytics çalışma alanına veri toplayın.
