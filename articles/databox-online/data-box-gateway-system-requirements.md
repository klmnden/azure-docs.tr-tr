---
title: Microsoft Azure veri kutusu ağ geçidi sistem gereksinimleri | Microsoft Docs
description: Yazılım ve Azure veri kutusu ağ geçidi için ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 10/17/2018
ms.author: alkohli
ms.openlocfilehash: da22c09a227069af0eeb42ab67a59189ae494185
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256681"
---
# <a name="azure-data-box-gateway-system-requirements-preview"></a>Azure veri kutusu ağ geçidi sistem gereksinimleri (Önizleme)

Bu makalede, Microsoft Azure veri kutusu ağ geçidi çözümünüz için ve Azure veri kutusu ağ geçidine bağlanma istemcileri için önemli sistem gereksinimlerini açıklar. Veri kutusu ağ geçidi dağıtın ve ardından geri gerekirse dağıtım ve sonraki işlemi sırasında başvurduğu önce bilgileri dikkatlice gözden öneririz.

Veri kutusu ağ geçidi sanal cihaz için sistem gereksinimleri şunlardır:

- **Konaklar için yazılım gereksinimleri** -desteklenen platformlar, yerel yapılandırma kullanıcı Arabirimi için tarayıcılar, SMB istemcileri ve cihaz için bağlanan konaklar için tüm ek gereksinimleri açıklanmaktadır.
- **Cihaz için ağ gereksinimleri** -sanal cihazın işlemi için herhangi bir ağ gereksinimleri hakkında bilgi sağlar.

> [!IMPORTANT]
> Data Box Gateway, Önizleme aşamasındadır. Bu çözümü dağıtmadan önce lütfen [Önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 

## <a name="specifications-for-the-virtual-device"></a>Sanal cihaz özellikleri

Temel alınan bir konak sistemi veri kutusu ağ geçidi için sanal cihazınızın sağlamak için aşağıdaki kaynakları ayırmanız kuramıyor:

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Sanal işlemciler (çekirdekler)   | En az 4 |            
| Bellek  | En az 8 GB|
| Kullanılabilirlik|Tek düğüm|
| Diskler| İşletim sistemi diski: 250 GB <br> Veri diski: 2 TB en, ölçülü kaynak sağlanan ve SSD'ler ile desteklenir|
| Ağ arabirimleri|1 veya daha çok sanal ağ arabirimi|


## <a name="supported-os-for-clients-connected-to-device"></a>Cihaz için bağlanan istemciler için desteklenen işletim sistemi

İstemciler veya veri kutusu ağ geçidine bağlı konaklar için desteklenen işletim sistemlerinin bir listesi aşağıda verilmiştir.

| **İşletim sistemi/platform** | **Sürümleri** |
| --- | --- |
| Windows Server |2012 R2 <br> 2016 |
| Windows |8, 10 |
| SUSE Linux |Kuruluş Sunucusu 12 (x86_64)|
| Ubuntu |16.04.3 LTS|
| CentOS | 7.0 |

## <a name="supported-protocols-for-clients-accessing-device"></a>Cihaz erişen istemciler için Desteklenen protokoller

|**Protokol** |**Sürümleri**   |**Notlar**  |
|---------|---------|---------|
|SMB    | 2.X 3.X      | SMB 1 desteklenmiyor.|
|NFS     | V3 hem de V4        |         |

## <a name="supported-virtualization-platforms-for-device"></a>Cihaz desteklenen sanallaştırma platformları

| **İşletim sistemi/platform**  |**Sürümleri**   |**Notlar**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016  |         |
|VMware ESXi     | 6.0 <br> 6.5        |VMware araçları desteklenmez.         |


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Veri kutusu ağ geçidi için desteklenen depolama hesaplarının listesi aşağıda verilmiştir.

| **Depolama hesabı** | **Notlar** |
| --- | --- |
| Klasik | Standart |
| Genel Amaçlı  |Standart; V1 ve V2 desteklenir. Sık ve seyrek erişimli katmanları desteklenir. |


## <a name="supported-storage-types"></a>Desteklenen depolama türleri

Veri kutusu ağ geçidi için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Dosya biçimi** | **Notlar** |
| --- | --- |
| Azure blok blobu | |
| Azure sayfa blobu  | |
| Azure Dosyaları | |

## <a name="supported-browsers-for-local-web-ui"></a>Yerel web kullanıcı Arabirimi ile desteklenen tarayıcılar

Sanal cihaz için yerel web kullanıcı Arabirimi için desteklenen tarayıcıların listesi aşağıda verilmiştir.

|Tarayıcı  |Sürümler  |Ek gereksinimler/Notlar  |
|---------|---------|---------|
|Google Chrome   |En son sürümü         |         |
|Microsoft Edge    | En son sürümü        |         |
|Internet Explorer     | En son sürümü        |         |
|FireFox    |En son sürümü         |         |


## <a name="networking-requirements"></a>Ağ gereksinimleri

Aşağıdaki tabloda, SMB, Bulut ve yönetim trafiği için izin vermek için güvenlik duvarını açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* hangi gelen istemci istekleri erişimden Cihazınızı yönü belirtir. *Çıkış* veya *giden* hangi veri kutusu ağ geçidi Cihazınızı gönderir dışarıdan, veri dağıtımı dışında yön ifade eder: Örneğin, Internet'e giden.

| Bağlantı noktası yok.| Daraltma veya genişletme | Bağlantı noktası kapsamı| Gerekli|   Notlar                                                             |                                                                                     |
|--------|---------|----------|--------------|----------------------|---------------|
| TCP 80 (HTTP)|Çıkan|WAN |Hayır|Giden bağlantı noktası, güncelleştirmeleri almak için Internet erişimi için kullanılır. <br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur. |                          
| TCP 443 (HTTPS)|Çıkan|WAN|Evet|Giden bağlantı noktası, bulut veri erişimi için kullanılır.<br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur.|   
| UDP 53 (DNS)|Çıkan|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca bir Internet tabanlı bir DNS sunucusu kullanıyorsanız bu bağlantı noktası gereklidir.<br>Yerel DNS sunucusunu kullanmanızı öneririz. |
| UDP 123 (NTP)|Çıkan|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca bir Internet tabanlı bir NTP sunucusu kullanıyorsanız bu bağlantı noktası gereklidir.  |
| UDP 67 (DHCP)|Çıkan|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca bir DHCP sunucusu kullanıyorsanız bu bağlantı noktası gereklidir.  |
| TCP 80 (HTTP)|İçinde|LAN|Evet|Bu, yerel yönetim için cihazda yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. <br>HTTP üzerinden yerel UI erişme, HTTPS için otomatik olarak yönlendirir.  | 
| TCP 443 (HTTPS)|İçinde|LAN|Evet|Bu, yerel yönetim için cihazda yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. | 

## <a name="url-patterns-for-firewall-rules"></a>URL desenleri için güvenlik duvarı kuralları

Ağ yöneticileri genellikle gelen filtrelemek için URL desenleri ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. Veri kutusu ağ geçidi cihazınız ve veri kutusu ağ geçidi hizmeti Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucularına gibi diğer Microsoft uygulamaları bağlıdır. Bu uygulamalarla ilişkili URL desenleri, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenleri değiştirebilirsiniz anlamak önemlidir. Buna karşılık, izleme ve güvenlik duvarı kuralları, veri kutusu ağ geçidi olarak ve gerektiğinde güncelleştirme için ağ yöneticisine da gerekir.

Veri kutusu liberally çoğu zaman sabit IP adresleri, ağ geçidi temel giden trafik için güvenlik duvarı kurallarınızın ayarlamanızı öneririz. Ancak, güvenli bir ortam oluşturmak için gereken gelişmiş güvenlik duvarı kurallarını ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> - (Kaynak) IP'ler cihaz her zaman tüm bulut özellikli ağ arabirimleri için ayarlanması gerekir.
> - IP'ler ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).

|     URL deseni                                                                                                                                                                                                                                                                                                                                                                                                                                       |     Bileşen/işlevi                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://*.databoxedge.azure.com/*<br>https://*.servicebus.windows.net/*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                        |    Azure veri kutusu ağ geçidi hizmeti<br>Azure Service Bus<br>Kimlik Doğrulama Hizmeti    |
|    http://*.Backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                   |    Cihaz etkinleştirme                                                                                    |
|    http://crl.microsoft.com/pki/*   http://www.microsoft.com/pki/*                                                                                                                                                                                                                                                                                                                                                                                    |    Sertifika iptal etme                                                                               |
|    https://*.core.windows.net/* https://*. data.microsoft.com http://*. msftncsi.com                                                                                                                                                                                                                                                                                                                                                                |    Azure depolama hesapları ve izleme                                                                |
|    http://windowsupdate.microsoft.com<br>http://*. windowsupdate.microsoft.com<br>https://*. windowsupdate.microsoft.com<br>http://*. update.microsoft.com<br>https://*. update.microsoft.com<br>http://*. windowsupdate.com<br>http://download.microsoft.com<br>http://*. download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://*. ws.microsoft.com<br>https://*. ws.microsoft.com<br>http://*.MP.microsoft.com        |    Microsoft Update sunucularına                                                                             |
|    http://*.deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                             |    Akamai CDN                                                                                           |
|    https://*.partners.extranet.microsoft.com/*                                                                                                                                                                                                                                                                                                                                                                                                        |    Destek paketi                                                                                      |
|    http://*.Data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                        |    Telemetri hizmeti, Windows müşteri deneyimini ve tanılama telemetrisi güncelleştirmesi bakın      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                         |



## <a name="internet-bandwidth"></a>Internet bant genişliği

Veri kutusu ağ geçidi cihazlarınız için kullanılabilir minimum Internet bant genişliği için aşağıdaki gereksinimler geçerlidir.

- Data Box Gateway'inize her zaman ayrılmış 20 Mb/sn İnternet bant genişliği (veya daha fazlası) sağlanıyor. Bu bant genişliği başka hiçbir uygulamayla paylaşılmamalıdır. 
- Veri kutusu ağ geçidinizi ayrılmış 32 MB/sn Internet bant genişliği (veya daha fazla) sahip ağ kapasitesi azaltma kullanırken.

## <a name="next-step"></a>Sonraki adım

* [Azure veri kutusu ağ geçidi dağıtma](data-box-gateway-deploy-prep.md)

