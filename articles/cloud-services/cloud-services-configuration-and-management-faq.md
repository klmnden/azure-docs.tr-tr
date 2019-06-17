---
title: Yapılandırma ve yönetim sorunları için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs
description: Bu makalede, yapılandırma ve Microsoft Azure bulut Hizmetleri için yönetimi hakkında sık sorulan soruları listelenmektedir.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: genli
ms.openlocfilehash: 85296b4549d7c9499b8d0b815ddf1cd2e85e2b1b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60337434"
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Yapılandırma ve yönetim sorunları Azure bulut Hizmetleri için: Sık sorulan sorular (SSS)

Bu makale için yapılandırma ve yönetim sorunları hakkında sık sorulan sorular içerir [Microsoft Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services). Ayrıca başvurabilirsiniz [bulut Hizmetleri VM boyutu sayfası](cloud-services-sizes-specs.md) boyutu bilgi.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

**Sertifikalar**

- [Bulut hizmeti SSL Sertifikamı sertifika zincirini neden eksik?](#why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete)
- ["Windows Azure Araçları şifreleme sertifikası için uzantıları" amacı nedir?](#what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions)
- [Nasıl ben bir sertifika imzalama isteği (CSR) "RDP-ing" olmadan, örneği oluşturabilir miyim?](#how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance)
- [Bulut Hizmeti Yönetim Sertifikamı süresi doluyor. Yenilemek nasıl?](#my-cloud-service-management-certificate-is-expiring-how-to-renew-it)
- [Ana SSL certificate(.pfx) ve Ara certificate(.p7b) yüklenmesini otomatik hale getirmek nasıl?](#how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b)
- ["Microsoft Azure hizmet yönetimi için MachineKey" Sertifika amacı nedir?](#what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate)

**İzleme ve günlüğe kaydetme**

- [Yönetmek ve uygulamaları izleme yardımcı olan Azure portalında yakında bulut hizmeti özellikleri nelerdir?](#what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications)
- [IIS, günlük dizinine yazma neden durdurulsun mu?](#why-does-iis-stop-writing-to-the-log-directory)
- [WAD, bulut Hizmetleri için günlüğe kaydetmeyi nasıl etkinleştiririm?](#how-do-i-enable-wad-logging-for-cloud-services)

**Ağ yapılandırması**

- [Azure yük dengeleyici için boşta kalma zaman aşımı nasıl ayarlayabilirim?](#how-do-i-set-the-idle-timeout-for-azure-load-balancer)
- [Bulut Hizmetimde nasıl statik bir IP adresi ilişkilendiririm?](#how-do-i-associate-a-static-ip-address-to-my-cloud-service)
- [Azure temel IP'ler/Kimlikleri ve DDOS sağlayan özellikler ve yetenekler nelerdir?](#what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides)
- [HTTP/2 Cloud Services sanal makine etkinleştirme](#how-to-enable-http2-on-cloud-services-vm)

**İzinler**

- [Microsoft bulut hizmeti örnekleri izni olmadan iç mühendisleri Uzak Masaüstü miyim?](#can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission)
- [RDP dosyasını kullanarak bulut hizmeti VM'ye Uzak Masaüstü bağlanamıyorum. Hata takip alın: Bir kimlik doğrulama hatası oluştu (kod: 0x80004005)](#i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005)

**Ölçeklendirme**

- [X ölçek genişletilemiyor örnekleri](#i-cannot-scale-beyond-x-instances)
- [Bellek ölçümlere göre otomatik ölçeklendirmeyi nasıl yapılandırabilirim?](#how-can-i-configure-auto-scale-based-on-memory-metrics)

**Genel**

- ["Nosniff" my Web sitesine nasıl ekleyebilirim?](#how-do-i-add-nosniff-to-my-website)
- [IIS web rolü için nasıl özelleştiririm?](#how-do-i-customize-iis-for-a-web-role)
- [Bulut Hizmetimde kota sınırını nedir?](#what-is-the-quota-limit-for-my-cloud-service)
- [Neden bulut hizmeti sanal Makinem sürücüdeki boş disk alanı çok az gösteriyor mu?](#why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space)
- [Nasıl bir kötü amaçlı yazılımdan koruma uzantısını my Cloud Services için otomatikleştirilmiş bir yolla ekleyebilirim?](#how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way)
- [Cloud Services için sunucu adı belirtme (SNI) etkinleştirme](#how-to-enable-server-name-indication-sni-for-cloud-services)
- [Azure bulut Hizmetimde etiketleri nasıl ekleyebilirim?](#how-can-i-add-tags-to-my-azure-cloud-service)
- [Azure portalında bulut Hizmetimde SDK'sı sürümü görüntülemez. Bu sorunu nasıl alabilirim?](#the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that)
- [Bulut hizmeti için birkaç ay Kapat istiyorsunuz. IP adresi kaybetmeden fatura bulut hizmeti maliyetini azaltmak nasıl?](#i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address)


## <a name="certificates"></a>Sertifikalar

### <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Bulut hizmeti SSL Sertifikamı sertifika zincirini neden eksik?
    
Müşteriler yalnızca yaprak sertifikayı yerine tam sertifika zinciri (yaprak sertifikayı, Ara sertifikaları ve kök sertifika) yüklemenizi öneririz. Yalnızca yaprak sertifikayı yüklediğinizde, CTL walking tarafından sertifika zinciri oluşturmak üzere Windows dayanır. Windows sertifika doğrulamaya çalışırken aralıklı ağ veya DNS sorunlarına Azure ya da Windows Update meydana gelirse, sertifika geçersiz olarak kabul edilebilir. Tam sertifika zinciri yükleyerek bu sorunu önlenebilir. Blog'da [Zincirli SSL sertifikası yükleme](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) bunun nasıl yapılacağı gösterilmektedir.

### <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>"Windows Azure Araçları şifreleme sertifikası için uzantıları" amacı nedir?

Bu sertifikalar, bulut hizmetine bir uzantı eklendiğinde otomatik olarak oluşturulur. En yaygın olarak, bu WAD uzantısı ya da RDP uzantısının, ancak diğerleri gibi kötü amaçlı yazılımdan koruma veya günlük Toplayıcı uzantısı olabilir. Bu sertifikalar, yalnızca şifreleme ve şifresini çözme uzantı özel yapılandırması için kullanılır. Sertifikanın süresi dolmuş olması önemli şekilde sona erme tarihini hiçbir zaman denetlenir. 

Bu sertifikalar yoksayabilirsiniz. Sertifikaları temizlemek isterseniz, tüm silerek deneyebilirsiniz. Azure, kullanımda olan bir sertifika silmeye çalışırsanız bir hata atar.

### <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Nasıl ben bir sertifika imzalama isteği (CSR) "RDP-ing" olmadan, örneği oluşturabilir miyim?

Aşağıdaki kılavuz belgesine bakın:

[Windows Azure Web siteleri (WAWS) ile kullanım için bir sertifika edinme](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

CSR yalnızca bir metin dosyasıdır. Bu makineden oluşturulacak sertifika sonuçta nerede kullanılacak yok. Bu belge, bir App Service için yazılmış olsa da, CSR oluşturmanın geneldir ve bulut Hizmetleri için de geçerlidir.

### <a name="my-cloud-service-management-certificate-is-expiring-how-to-renew-it"></a>Bulut Hizmeti Yönetim Sertifikamı süresi doluyor. Yenilemek nasıl?

Yönetim sertifikaları yenilemek için aşağıdaki PowerShell komutları kullanabilirsiniz:

    Add-AzureAccount
    Select-AzureSubscription -Current -SubscriptionName <your subscription name>
    Get-AzurePublishSettingsFile

**Get-AzurePublishSettingsFile** yeni bir yönetim sertifikası oluşturur **abonelik** > **yönetim sertifikaları** Azure portalında. Yeni sertifika adını "YourSubscriptionNam]-[günün tarihine] - kimlik bilgileri gibi" arar.

### <a name="how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b"></a>Ana SSL certificate(.pfx) ve Ara certificate(.p7b) yüklenmesini otomatik hale getirmek nasıl?

Bu görev bir başlangıç betiği (toplu iş/cmd/PowerShell) kullanarak otomatik hale getirmek ve hizmet tanımı dosyasında başlangıç komut dosyasını kaydedin. Başlangıç betiği ve sertifikayı (.p7b dosyası) başlangıç komut dosyasını aynı dizine proje klasöründe ekleyin.

### <a name="what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate"></a>"Microsoft Azure hizmet yönetimi için MachineKey" Sertifika amacı nedir?

Bu sertifika, Azure Web rolleri üzerinde makine anahtarlarını şifrelemek için kullanılır. Daha fazla bilgi için kullanıma [bu danışma](https://docs.microsoft.com/security-updates/securityadvisories/2018/4092731).

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [Yapılandırma ve bulut hizmeti için başlangıç görevleri çalıştırma](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks)
- [Bulut hizmeti genel başlangıç görevleri](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks-common)

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

### <a name="what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications"></a>Yönetmek ve uygulamaları izleme yardımcı olan Azure portalında yakında bulut hizmeti özellikleri nelerdir?

Uzak Masaüstü Protokolü (RDP) için yeni bir sertifika oluşturma olanağı yakında sunulacaktır. Alternatif olarak, bu betiği çalıştırabilirsiniz:

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 20 48 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```
Konum csdef ve cscfg karşıya blob veya yerel seçin olanağı yakında sunulacaktır. Kullanarak [yeni AzureDeployment](/powershell/module/servicemanagement/azure/new-azuredeployment?view=azuresmps-4.0.0), her bir konum değeri ayarlayabilirsiniz.

Örnek düzeyinde ölçümleri izleme yeteneği. Ek izleme kapasitelerinden kullanılabilir [bulut Hizmetleri'ni izleme nasıl](cloud-services-how-to-monitor.md).

### <a name="why-does-iis-stop-writing-to-the-log-directory"></a>IIS, günlük dizinine yazma neden durdurulsun mu?
Günlük dizinine yazmak için yerel depolama kotası kullanmış olursunuz. Bu sorunu gidermek için işlemlerden birini yapabilirsiniz:
* IIS için tanılamayı etkinleştirin ve blob depolama tanılama düzenli aralıklarla taşıdık.
* Günlük dosyalarını el ile günlüğe kaydetme dizinden kaldırın.
* Yerel kaynaklar için kota sınırını artırın.

Daha fazla bilgi için aşağıdaki belgelere bakın:
* [Azure Depolama’daki tanılama verilerini depolama ve görüntüleme](cloud-services-dotnet-diagnostics-storage.md)
* [Bulut hizmetinde yazma IIS günlükler Durdur](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

### <a name="how-do-i-enable-wad-logging-for-cloud-services"></a>WAD, bulut Hizmetleri için günlüğe kaydetmeyi nasıl etkinleştiririm?
Windows Azure tanılama (WAD) günlüğe kaydetme aşağıdaki seçeneklerle etkinleştirebilirsiniz:
1. [Visual Studio'dan etkinleştir](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)
2. [.NET kodu aracılığıyla etkinleştirme](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics)
3. [PowerShell aracılığıyla etkinleştirme](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell)

Bulut hizmetinizin geçerli WAD ayarlarını almak için kullanabileceğiniz [Get-AzureServiceDiagnosticsExtensions](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell#get-current-diagnostics-extension-configuration) ps cmd veya görüntüleyebilir, "Uzantılar--> bulut Hizmetleri" dikey penceresinden portal üzerinden.


## <a name="network-configuration"></a>Ağ yapılandırması

### <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Azure yük dengeleyici için boşta kalma zaman aşımı nasıl ayarlayabilirim?
Bu gibi hizmet tanımı (csdef) dosyasında zaman aşımını belirtebilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Bkz: [yeni: Yapılandırılabilir boşta kalma zaman aşımı için Azure Load Balancer](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/) daha fazla bilgi için.

### <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Bulut Hizmetimde nasıl statik bir IP adresi ilişkilendiririm?
Bir statik IP adresi ayarlamak için ayrılmış bir IP'yi oluşturmanız gerekir. Bu ayrılmış IP, yeni bir bulut hizmeti veya var olan bir dağıtım ilişkilendirilebilir. Ayrıntılar için aşağıdaki belgelere bakın:
* [Ayrılmış IP adresi oluşturma](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Mevcut bir bulut hizmetinin IP adresini ayırma](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Ayrılmış bir IP'yi yeni bulut hizmetiyle ilişkilendirme](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Çalışan bir geliştirme için ayrılmış bir IP'yi ilişkilendirme](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Hizmet yapılandırma dosyasını kullanarak bir bulut hizmeti için ayrılmış bir IP'yi ilişkilendirme](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

### <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Azure temel IP'ler/Kimlikleri ve DDOS sağlayan özellikler ve yetenekler nelerdir?
Azure veri merkezi fiziksel sunucuları tehditlere karşı korumaya için IP'ler/Kimlikleri sahiptir. Ayrıca, müşterilerin, web uygulaması güvenlik duvarları, ağ güvenlik duvarları, kötü amaçlı yazılımdan koruma, yetkisiz giriş algılama, önleme sistemleri (IDs/IPS) ve diğer üçüncü taraf güvenlik çözümleri dağıtabilirsiniz. Daha fazla bilgi için [varlıkları ve veri koruma ve genel güvenlik standartları ile uyum](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft, sunucuları, ağları ve uygulamaları, tehditleri algılamak için sürekli olarak izler. Azure'nın multipronged tehdit yönetimi yaklaşımını kullanan izinsiz giriş algılama, dağıtılmış hizmet engelleme (DDoS) saldırısını önlemeyi, sızma testi, davranışsal analiz, anomali algılama ve sürekli olarak kendi defense güçlendirmek için machine learning ve risklerini azaltın. Azure için Microsoft Antimalware, Azure bulut Hizmetleri ve sanal makineleri korur. Web uygulaması güvenlik duvarları, ağ güvenlik duvarları, kötü amaçlı yazılımdan koruma, yetkisiz giriş algılama ve önleme sistemleri (IDs/IPS) ve daha fazlası gibi ek olarak, üçüncü taraf güvenlik çözümleri dağıtma seçeneği var.

### <a name="how-to-enable-http2-on-cloud-services-vm"></a>HTTP/2 Cloud Services sanal makine etkinleştirme

HTTP/2 desteği ile Windows 10 ve Windows Server 2016 istemci ve sunucu tarafında gelir. İstemci (tarayıcı) bağlanılıyorsa TLS üzerinden IIS sunucusuna, HTTP/2 TLS uzantıları aracılığıyla belirleyici, ardından sunucu tarafında herhangi bir değişiklik yapmanız gerekmez. Varsayılan olarak TLS üzerinden HTTP/2 kullanımını belirtme h2-14 üst bilgisinin gönderdiğini, olmasıdır. Öte yandan, istemci HTTP/2'ye yükseltmek için yükseltme bir üst bilgi gönderiyorsa, yükseltme çalışır ve zaman ile bir HTTP/2 bağlantı düştüğünden emin olmak için sunucu tarafında aşağıdaki değişikliği yapmanız gerekir. 

1. Regedit.exe'yi çalıştırmak.
2. Kayıt defteri anahtarına gidin: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters.
3. Adlı yeni bir DWORD değeri oluşturun **DuoEnabled**.
4. Değeri 1 olarak ayarlayın.
5. Sunucunuzu yeniden başlatın.
6. Git, **varsayılan Web sitesi** altında **bağlamaları**, yeni oluşturduğunuz otomatik olarak imzalanan sertifika ile yeni bir TLS bağlama oluşturun. 

Daha fazla bilgi için bkz.

- [IIS HTTP/2](https://blogs.iis.net/davidso/http2)
- [Video: Windows 10'da HTTP/2: Tarayıcı, uygulamaları ve Web sunucusu](https://channel9.msdn.com/Events/Build/2015/3-88)
         

Böylece her yeni bir PaaS örneği oluşturulduğunda, yukarıda sistem kayıt defterinde değişiklikler yapmak için bu adımları bir başlangıç görevi otomatik olarak. Daha fazla bilgi için [nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri çalıştırma](cloud-services-startup-tasks.md).

 
Bunu yaptıktan sonra HTTP/2 etkin olup olmadığını veya değil aşağıdaki yöntemlerden birini kullanarak doğrulayabilirsiniz:

- IIS günlükler protokol sürümünde etkinleştirmek ve IIS günlüklerine bakın. Günlüklerde HTTP/2 gösterilir. 
- Internet Explorer/Microsoft edge'de F12 Geliştirici aracı etkinleştirmek ve protokolü doğrulamak için ağ sekmesine gidin. 

Daha fazla bilgi için [HTTP/2 IIS'de](https://blogs.iis.net/davidso/http2).

## <a name="permissions"></a>İzinler

### <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Nasıl miyim bulut Hizmetleri için rol tabanlı erişim uygulayabilir mi?
Bulut Hizmetleri bir Azure Resource Manager tabanlı hizmeti olmadığından rol tabanlı erişim denetimi (RBAC) modelini desteklemiyor.

Bkz: [farklı rolleri Azure anlamak](../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="remote-desktop"></a>Uzak Masaüstü

### <a name="can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission"></a>Microsoft bulut hizmeti örnekleri izni olmadan iç mühendisleri Uzak Masaüstü miyim?
Microsoft Uzak Masaüstü iç mühendisler, bulut hizmetine (e-posta veya yazılı diğer iletişimi) yazılı izni olmadan izin vermez katı bir işlem sahibi ya da kendi yetkilinin izler.

### <a name="i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005"></a>RDP dosyasını kullanarak bulut hizmeti VM'ye Uzak Masaüstü bağlanamıyorum. Hata takip alın: Bir kimlik doğrulama hatası oluştu (kod: 0x80004005)

Azure Active Directory'ye katılmış bir makineden RDP dosyası kullanıyorsanız bu hata oluşabilir. Bu sorunu çözmek için şu adımları izleyin:

1. İndirilen RDP dosyasını sağ tıklayın ve ardından **Düzenle**.
2. Ekleme "&#92;" önce kullanıcı adı öneki. Örneğin, **. \username** yerine **username**.

## <a name="scaling"></a>Ölçeklendirme

### <a name="i-cannot-scale-beyond-x-instances"></a>X ölçek genişletilemiyor örnekleri
Azure aboneliğiniz, kullandığınız çekirdek sayısına limiti vardır. Tüm çekirdek kullandıysanız ölçeklendirme çalışmaz. Bir sınır, 100 çekirdek varsa, örneğin, bu, bulut hizmetinizin 100 A1 boyutta sanal makine örnekleri olabilir veya sanal makine örnekleri 50 A2 boyutu anlamına gelir.

### <a name="how-can-i-configure-auto-scale-based-on-memory-metrics"></a>Bellek ölçümlere göre otomatik ölçeklendirmeyi nasıl yapılandırabilirim?

Bir bulut hizmeti için bellek ölçümlere göre otomatik ölçeklendirme şu anda desteklenmiyor. 

Bu sorunu çözmek için Application ınsights'ı kullanabilirsiniz. Otomatik ölçeklendirme, ölçüm kaynağı olarak Application ınsights'ı destekler ve rol örneği sayısı "Bellek" gibi Konuk ölçüme göre ölçeklendirebilir.  Bulut hizmeti projesi paket dosyanızda (dosyanın *.cspkg) Application Insights'ı yapılandırın ve hizmeti bu feat uygulamak için Azure tanılama uzantısını etkinleştirme gerekir.

Özel ölçüm Application Insights'ın bulut hizmetleri üzerinde otomatik ölçeklendirmeyi yapılandırma yoluyla nasıl hakkında daha fazla ayrıntı için bkz. [otomatik ölçeklendirme ile azure'da özel bir ölçü olarak kullanmaya başlayın](../azure-monitor/platform/autoscale-custom-metric.md)

Azure Tanılama, Cloud Services için Application Insights ile tümleştirme hakkında daha fazla bilgi için bkz. [Application ınsights'a Gönder bulut hizmeti, sanal makine ya da Service Fabric Tanılama verileri](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)

Yaklaşık Cloud Services için Application Insights'ı etkinleştirmek daha fazla bilgi için bkz. [Azure Cloud Services için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices)

Bulut Hizmetleri için Azure tanılama günlüğü etkinleştirme hakkında daha fazla bilgi için bkz. [tanılama ayarlama, Azure bulut Hizmetleri ve sanal makineler için ayarlayın](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)

## <a name="generic"></a>Genel

### <a name="how-do-i-add-nosniff-to-my-website"></a>"Nosniff" my Web sitesine nasıl ekleyebilirim?
MIME türleri algılaması gelen istemcilerin önlemek için bir ayar ekleyin, *web.config* dosya.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

De bu IIS ayarı olarak ekleyebilirsiniz. Aşağıdaki komutla kullanın [genel başlangıç görevleri](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) makalesi.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="how-do-i-customize-iis-for-a-web-role"></a>IIS web rolü için nasıl özelleştiririm?
IIS başlatma betiği kullanma [genel başlangıç görevleri](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) makalesi.

### <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Bulut Hizmetimde kota sınırını nedir?
Bkz: [hizmete özgü sınırlar](../azure-subscription-service-limits.md#subscription-limits).

### <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Neden bulut hizmeti sanal Makinem sürücüdeki boş disk alanı çok az gösteriyor mu?
Bu beklenen bir davranıştır ve uygulamanız için herhangi bir sorun neden olmamalıdır. Günlük kaydı, temelde iki dosyaları normalde kapladığı alan miktarı tüketen Azure PaaS vm'lerde % approot % sürücüsü için açıktır. Ancak temelde, uyumlu olması için birkaç şey vardır, bu olmayan bir-sorunu kapatın.

% Approot % sürücü boyutu değeriyle hesaplanan < .cspkg + maksimum günlük boyutu boyutunu > + boş alan bir kenar boşluğu olarak veya 1,5 GB, hangisi daha büyükse. Sanal makinenizin boyutunu bu hesaplamayı temel bir ilgisi yoktur. (VM boyutu yalnızca geçici C: sürücüsü boyutunu etkiler.) 

% Approot % diske yazmak için desteklenmiyor. Azure VM'ye yazıyorsanız, bunu bir geçici LocalStorage kaynakta yapmalısınız (veya başka bir seçeneği gibi Blob Depolama, Azure dosyaları, vb..). Bu nedenle % approot % klasöründeki boş alan miktarını anlamlı değildir. Uygulamanızı % approot % sürücüye yazıyorsanız emin değilseniz, hizmetiniz birkaç gün boyunca çalıştırın ve ardından karşılaştırma her zaman izin verebilirsiniz "önce" ve "sonra" boyutları. 

Azure % approot % sürücüye hiçbir şey yazmaz. VHD, .cspkg oluşturulur ve Azure VM'ye takılı sonra bu sürücüye yazabilirsiniz gereken tek şey, uygulamasıdır. 

Günlük ayarları yapılandırılamayan, olduğundan, bunu devre dışı bırakamazlar.

### <a name="how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way"></a>Nasıl bir kötü amaçlı yazılımdan koruma uzantısını my Cloud Services için otomatikleştirilmiş bir yolla ekleyebilirim?

Başlangıç görevi PowerShell betiğini kullanarak kötü amaçlı yazılımdan koruma uzantısını etkinleştirebilirsiniz. Adımları uygulamak için bu makaleleri izleyin: 
 
- [Bir PowerShell başlangıç görevi oluşturun](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)
- [Set-AzureServiceAntimalwareExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/Set-AzureServiceAntimalwareExtension?view=azuresmps-4.0.0 )

Kötü amaçlı yazılımdan koruma dağıtım senaryoları ve Portalı'ndan etkinleştirme hakkında daha fazla bilgi için bkz: [kötü amaçlı yazılımdan koruma dağıtım senaryoları](../security/azure-security-antimalware.md#antimalware-deployment-scenarios).

### <a name="how-to-enable-server-name-indication-sni-for-cloud-services"></a>Cloud Services için sunucu adı belirtme (SNI) etkinleştirme

Aşağıdaki yöntemlerden birini kullanarak bulut hizmetlerinde SNI etkinleştirebilirsiniz:

**1. yöntem: PowerShell'i kullanma**

SNI bağlama PowerShell cmdlet'i kullanılarak yapılandırılabilir **yeni WebBinding** , aşağıda gösterildiği gibi bir bulut Hizmeti rol örneği için bir başlangıç görevi:
    
    New-WebBinding -Name $WebsiteName -Protocol "https" -Port 443 -IPAddress $IPAddress -HostHeader $HostHeader -SslFlags $sslFlags 
    
Açıklandığı [burada](https://technet.microsoft.com/library/ee790567.aspx), $sslFlags olarak aşağıdaki değerlerden biri olabilir:

|Değer|Anlamı|
------|------
|0|No SNI|
|1|SNI etkin |
|2 |Merkezi sertifika Store kullanan bağlama olmayan SNI|
|3|Merkezi sertifika kullanan SNI bağlama depolayın |
 
**2. yöntem: Kodu kullanın**

SNI bağlama Ayrıca rol başlatma kodu aracılığıyla bu konuda açıklandığı gibi yapılandırılabilir [blog gönderisi](https://blogs.msdn.microsoft.com/jianwu/2014/12/17/expose-ssl-service-to-multi-domains-from-the-same-cloud-service/):

    
    //<code snip> 
                    var serverManager = new ServerManager(); 
                    var site = serverManager.Sites[0]; 
                    var binding = site.Bindings.Add(“:443:www.test1.com”, newCert.GetCertHash(), “My”); 
                    binding.SetAttributeValue(“sslFlags”, 1); //enables the SNI 
                    serverManager.CommitChanges(); 
    //</code snip> 
    
Yaklaşımlardan birini kullanarak, belirli ana bilgisayar adları için kendi sertifikalarını (*.pfx) önce bir başlangıç görevi kullanılarak rol örneklerinde veya SNI bağlamanızın etkili olması kod aracılığıyla yüklenmesi gerekir.

### <a name="how-can-i-add-tags-to-my-azure-cloud-service"></a>Azure bulut Hizmetimde etiketleri nasıl ekleyebilirim? 

Klasik kaynak bulut hizmetidir. Yalnızca Azure Resource Manager desteği etiketler ile oluşturulan kaynakları. Klasik kaynaklar'bulut hizmeti gibi etiketleri uygulayamazsınız. 

### <a name="the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that"></a>Azure portalında bulut Hizmetimde SDK'sı sürümü görüntülemez. Bu sorunu nasıl alabilirim?

Bu özellik Azure portalında getirme çalışıyoruz. Bu arada, SDK sürümü almak için aşağıdaki PowerShell komutları kullanabilirsiniz:

    Get-AzureService -ServiceName "<Cloud Service name>" | Get-AzureDeployment | Where-Object -Property SdkVersion -NE -Value "" | select ServiceName,SdkVersion,OSVersion,Slot

### <a name="i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address"></a>Bulut hizmeti için birkaç ay Kapat istiyorsunuz. IP adresi kaybetmeden fatura bulut hizmeti maliyetini azaltmak nasıl?

Zaten dağıtılmış bir bulut hizmeti işlem ve depolama kullandığı için faturalandırılır. Azure sanal makineyi olsa bile, bu nedenle, yine de depolama için faturalandırılacaktır. 

Hizmetiniz için IP adresi kaybetmeden faturalarınızı azaltmak için yapabilecekleriniz aşağıda verilmiştir:

1. [IP adresini ayırma](../virtual-network/virtual-networks-reserved-public-ip.md) dağıtımları silmeden önce.  Yalnızca bu IP adresi için faturalandırılırsınız. IP adresi faturalama hakkında daha fazla bilgi için bkz. [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses/).
2. Dağıtımları silin. Gelecekte kullanmak xxx.cloudapp.net silmeyin.
3. Bulut hizmeti aynı ayrılmış IP kullanarak yeniden dağıtmak isterseniz, aboneliğinizde ayrılmış bkz [bulut Hizmetleri ve sanal makineler için ayrılmış IP adresleri](https://azure.microsoft.com/blog/reserved-ip-addresses/).

