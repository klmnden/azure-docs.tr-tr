---
title: Azure Log Analytics aracısını ile karma bir ortamda veri toplama | Microsoft Docs
description: Bu konuda veri toplamak ve şirket içi veya Log Analytics ile diğer bulut ortamında barındırılan bilgisayarlarını izlemek nasıl anlamanıza yardımcı olur.
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
ms.date: 10/15/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 9decd861ff20a45939f700eef99245b6555829f8
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319753"
---
# <a name="collect-data-in-a-hybrid-environment-with-log-analytics-agent"></a>Log Analytics aracısını ile karma bir ortamda veri topla

Azure Log Analytics, toplamak ve üzerinde çalışan Windows veya Linux işletim sistemi çalıştıran bilgisayarlardan veri işlem:

* [Azure sanal makineleri](log-analytics-quick-collect-azurevm.md) Log Analytics VM uzantısını kullanma 
* Veri merkezinizi fiziksel sunucularda veya sanal makinelerde olarak
* Bulutta barındırılan bir hizmetteki Amazon Web Services (AWS) gibi sanal makineler

Ortamınızda barındırılan bilgisayarları doğrudan Log Analytics'e bağlı olarak ya da bu bilgisayarlar System Center Operations Manager 2012 R2 veya sonraki bir sürümü zaten izliyorsanız, Operations Manager'da yönetim grubunuzu Log Analytics ile tümleştirebilirsiniz ve BT hizmet işlemleri işlemlerinizi koruma devam eder.  

## <a name="overview"></a>Genel Bakış

![log-Analytics-Agent-Direct-Connect-Diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Çözümleme ve toplanan verilerin hareket önce ilk yükleme ve aracıları için Log Analytics hizmetine veri göndermek istediğiniz tüm bilgisayarların bağlanmak gerekir. Kurulum, komut satırı kullanarak şirket içi bilgisayarlarınızı veya ile Desired State Configuration (DSC) Azure automation'da aracıları yükleyebilirsiniz. 

Linux ve Windows için aracı TCP bağlantı noktası 443 üzerinden giden Log Analytics hizmetiyle iletişim kurar ve Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya proxy sunucusu bilgisayarı bağlandığı ağ yapılandırmasını öğrenmek için aşağıdaki gereksinimleri gözden geçirin Gerekli.  BT güvenlik ilkeleriniz bilgisayarların Internet'e bağlanmak için ağ üzerinde izin vermiyorsa, ayarlayabileceğiniz bir [OMS ağ geçidi](log-analytics-oms-gateway.md) ve aracının Log analytics'e ağ geçidi üzerinden bağlanmak için yapılandırın. Aracı yapılandırma bilgilerini almak ve hangi veri toplama kuralları ve çözümleri etkinleştirdiğiniz bağlı olarak toplanan veriler gönderme. 

Bilgisayarın System Center Operations Manager 2012 R2 veya üzeri izliyorsanız, veri toplamak ve hizmete iletmek ve tarafından izlenmesi için Log Analytics hizmeti ile birden çok girişli olabilir [Operations Manager](log-analytics-om-agents.md). Log Analytics ile tümleşik bir Operations Manager yönetim grubu tarafından izlenen Linux bilgisayarlar için veri kaynakları ve İleri toplanan verileri yönetim grubu yapılandırması almazsınız. Linux Aracısı, yalnızca tek bir çalışma alanına raporlama desteklese de Windows aracı en fazla dört çalışma alanlarını, rapor edebilirsiniz.  

Log Analytics'e bağlanmak için yalnızca Linux ve Windows için aracı değilse, karma Runbook çalışan rolü ana bilgisayar ve değişiklik izleme ve güncelleştirme yönetimi gibi yönetim çözümleri için Azure Otomasyonu da destekler.  Karma Runbook çalışanı rolü hakkında daha fazla bilgi için bkz. [Azure Otomasyon karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri
Aşağıdaki Windows işletim sistemi sürümleri Windows aracısı için resmi olarak desteklenir:

* Windows Server 2008 Service Pack 1 (SP1) veya üzeri
* Windows 7 SP1 ve üzeri.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri
Bu bölümde, desteklenen Linux dağıtımları hakkında ayrıntılar sağlar.    

Ağustos 2018 tarihinden sonraki sürümleri ile başlayarak, aşağıdaki değişiklikleri destek modelimizi yapıyoruz:  

* Yalnızca sunucu sürümleri desteklenir, istemci değil.  
* Yeni sürümlerini [Azure destekli Linux dağıtımları](../virtual-machines/linux/endorsed-distros.md) her zaman desteklenir.  
* Listelenen her ana sürümünün tüm ikincil sürümleri desteklenir.
* Üreticinin destek bitiş tarihi geçmiş sürümleri desteklenmez.  
* AMI yeni sürümleri desteklenmez.  
* SSL çalıştıran sürümler 1.x varsayılan olarak desteklenir.

Bir distro veya destek modelimizi hizalama değil ve şu anda desteklenmeyen bir sürümünü kullanıyorsanız, Microsoft destek sürümleri çatalı aracı ile ilgili Yardım sağlamaz sıkan Bu deponun çatalını oluşturmanız önerilir.

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

## <a name="tls-12-protocol"></a>TLS 1.2 Protokolü
Log analytics'e Aktarımdaki verilerin güvenliğini sağlamak üzere en az kullanmak üzere yapılandırmak için önemle öneririz Aktarım Katmanı Güvenliği (TLS) 1.2. TLS/Güvenli Yuva Katmanı (SSL) daha eski sürümleri, savunmasız bulundu ve bunlar yine de şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**.  Ek bilgi için gözden [TLS 1.2 kullanarak güvenli bir şekilde veri gönderen](log-analytics-data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri
Linux ve Windows Aracısı Log Analytics ile iletişim kurmak gerekli proxy ve güvenlik duvarı yapılandırma bilgileri listesi aşağıdaki bilgileri.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı noktası 443 |Gelen ve giden|Evet |  
|*.oms.opinsights.azure.com |Bağlantı noktası 443 |Gelen ve giden|Evet |  
|*.blob.core.windows.net |Bağlantı noktası 443 |Gelen ve giden|Evet |  
|*.azure-automation.net |Bağlantı noktası 443 |Gelen ve giden|Evet |  


Bağlanmak ve ortamınızda runbook'ları kullanmak için Otomasyon hizmetine kaydetmek için Azure Otomasyon karma Runbook çalışanı'nı kullanmayı planlıyorsanız, bağlantı noktası numarası ve bölümünde açıklanan URL'lere erişimi olmalıdır [ağınız için yapılandırma Karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md#network-planning). 

Windows ve Linux Aracısı, proxy sunucusu veya OMS Log Analytics hizmeti HTTPS protokolünü kullanarak ağ geçidi ile iletişim kurulurken destekler.  Anonim ve temel kimlik doğrulaması (kullanıcı adı/parola) desteklenir.  Yüklemesi sırasında belirtilen hizmete doğrudan bağlı Windows aracısı için proxy yapılandırmasını veya [dağıtımdan sonra](log-analytics-agent-manage.md#update-proxy-settings) Denetim Masası'ndan veya PowerShell ile.  

Linux aracısı için proxy sunucusu yüklemesi sırasında belirtilen veya [yüklemeden sonra](log-analytics-agent-manage.md#update-proxy-settings) proxy.conf yapılandırma dosyasını değiştirerek.  Linux Aracısı proxy yapılandırması değeri sözdizimi aşağıdaki gibidir:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Ara sunucunuz kimlik doğrulamasından geçmesini gerektirmiyorsa, Linux Aracısı hala bir sanal kullanıcı/parola sağlama gerektirir. Bu, herhangi bir kullanıcı adı veya parola olabilir.

|Özellik| Açıklama |
|--------|-------------|
|Protokol | https |
|kullanıcı | Ara sunucu kimlik doğrulaması için isteğe bağlı bir kullanıcı adı |
|password | Proxy kimlik doğrulaması için isteğe bağlı parola |
|proxyhost | Adresi veya FQDN proxy sunucusu/OMS ağ geçidi |
|port | Proxy sunucusu/OMS ağ geçidi için isteğe bağlı bağlantı noktası numarası |

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Gibi özel karakterleri kullanırsanız "\@" parolanızı, değeri yanlış ayrıştırılır için proxy bağlantı hatası alırsınız.  Bu sorunu geçici olarak çözmek için parola gibi bir araç kullanarak URL kodlama [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Aracıyı yükle ve yapılandır 
Şirket içi bilgisayarları doğrudan Log Analytics ile bağlama, gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tabloda, kuruluşunuzda en iyi şekilde çalıştığı belirlemek için her bir yöntemin vurgulanmaktadır.

|Kaynak | Yöntem | Açıklama|
|-------|-------------|-------------|
| Windows bilgisayar|- [El ile yükleme](log-analytics-agent-windows.md)<br>- [Azure Otomasyonu DSC](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Azure Stack ile Resource Manager şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Komut satırını veya Azure Automation DSC gibi otomatikleştirilmiş bir yöntem kullanarak Microsoft Monitoring agent yükleme [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), ya da Microsoft dağıttıysanız, Azure Resource Manager şablonu ile Azure Stack, veri merkezinizdeki.| 
|Linux bilgisayarı| [El ile yükleme](log-analytics-quick-collect-linux-computer.md)|Github'da barındırılan bir sarmalayıcı betik çağırma Linux için aracıyı yükleyin. | 
| System Center Operations Manager|[Operations Manager'ı Log Analytics ile tümleştirme](log-analytics-om-agents.md) | Operations Manager ve Log Analytics iletmek arasında bütünleşmeyi yapılandırmadan bir yönetim grubuna raporlama Linux ve Windows bilgisayarlardan toplanan verileri.|  

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [veri kaynakları](log-analytics-data-sources.md) Windows veya Linux sisteminizden veri toplamak kullanılabilir veri kaynaklarını anlamasına olanak. 

* Hakkında bilgi edinin [günlük aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 

* Hakkında bilgi edinin [çözümleri](log-analytics-add-solutions.md) Log Analytics'e işlev eklemek ve ayrıca OMS deposuna veri toplayın.
