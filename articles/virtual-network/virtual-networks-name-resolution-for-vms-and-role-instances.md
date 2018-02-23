---
title: "VM'ler ve rol örnekleri için ad çözümlemesi"
description: "Azure Iaas, farklı bulut Hizmetleri, Active Directory ve kendi DNS sunucusu kullanılarak arasındaki karma çözümleri için ad çözümleme senaryoları."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: jdial
ms.openlocfilehash: 5ea3e4ad68fd37ccaa6e081febe4827bdb2e196d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="name-resolution-for-vms-and-role-instances"></a>VM'ler ve rol örnekleri için ad çözümlemesi

Azure Iaas ve PaaS karma çözümleri barındırmak için nasıl kullanacağınız bağlı olarak, sanal makineleri ve birbirleri ile iletişim kurmak için oluşturduğunuz rol örneklerine izin gerekebilir. Bu iletişim, IP adreslerini kullanarak yapılabilir rağmen kolay anımsanacak ve değiştirmeyin adlar kullanmak çok daha kolaydır. 

İç IP adresleri için etki alanı adları çözümlemek rol örnekleri ve Azure üzerinde barındırılan sanal makineleri gerektiğinde, bunlar iki yöntemden birini kullanabilirsiniz:

* [Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution)
* [Kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server) (hangi iletmek için Azure tarafından sağlanan DNS sunucularını sorgular) 

Kullandığınız ad çözümlemesi türünün nasıl VM'ler ve rol örnekleri birbirleri ile iletişim kurmak gereksinimlerine göre değişir. Aşağıdaki tabloda senaryoları ve karşılık gelen ad çözümlemesi çözümleri gösterilmektedir:

| Senaryo | Çözüm | **Suffix** |
| --- | --- | --- |
| Rol örnekleri veya aynı bulut hizmeti ya da sanal ağ içinde yer alan VM'ler arasında ad çözümleme. |[Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution) |ana bilgisayar adı veya FQDN |
| Ad çözümlemesi (Web uygulaması, işlev veya Bot) bir Azure uygulama hizmeti sanal ağ tümleştirmesinin rol örnekleri ya da sanal makineleri kullanarak aynı sanal ağda bulunan. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| Rol örnekleri veya farklı sanal ağlarda yer alan VM'ler arasında ad çözümleme. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| App Service Web Apps alanından ad çözümlemesi aynı sanal ağda bulunan sanal makineleri için. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| App Service Web Apps name resolution farklı bir sanal ağda bulunan sanal makineleri için. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server-for-web-apps). |Yalnızca FQDN |
| Şirket içi bilgisayar hizmeti adları ve rol örnekleri veya azure'da VM çözünürlüğü. |Müşteri tarafından yönetilen DNS sunucuları (şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya bölge aktarımlarının, örneğin kullanarak eşitlenen bir DNS ikincil). Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| Şirket içi bilgisayarlardan Azure ana bilgisayar adları çözünürlüğü. |İletme sorguları bir müşteri tarafından yönetilen DNS proxy sunucusuna karşılık gelen sanal ağ proxy sunucusunu sorgular için Azure çözümlemesi için iletir. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| DNS geriye doğru iç IP için. |[Kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Uygulanamaz |
| Sanal makineler veya sanal ağ içinde olmayan farklı bulut Hizmetleri bulunan rol örnekleri arasında ad çözümleme. |Geçerli değil. VM'ler ve rol örnekleri farklı bulut hizmetleri arasında bağlantı sanal ağ dışında desteklenmiyor. |Uygulanamaz|

## <a name="azure-provided-name-resolution"></a>Azure tarafından sağlanan ad çözümlemesi

Genel DNS adlarını çözümlenmesi yanı sıra, Azure Vm'leri ve aynı sanal ağda veya Bulut hizmeti içinde bulunan rol örnekleri için dahili ad çözümlemesi sağlar. (Ana bilgisayar adı tek başına yeterli olacak şekilde) VM'ler/örnekleri bir bulut hizmetinde aynı DNS sonekine paylaşır ancak FQDN farklı bulut hizmetleri arasında adları çözümlemek için gerektiği şekilde Klasik sanal ağlar farklı bulutta Hizmetleri farklı DNS sonekleri. Resource Manager dağıtım modelinde sanal ağlardaki, DNS son eki (FQDN gerekli olmadığı için) sanal ağ arasında tutarlıdır ve DNS adlarını NIC ve VM'ler için atanabilir. Azure tarafından sağlanan ad çözümlemesi herhangi bir yapılandırma gerektirmez rağmen tüm dağıtım senaryoları için uygun seçim önceki tabloda görüldüğü gibi değil.

> [!NOTE]
> Web ve çalışan rolleri kullanırken, Azure Hizmet Yönetimi REST API kullanarak rol adı ile örnek numarasını temel alan rol örneklerinin iç IP adreslerini de erişebilirsiniz. Daha fazla bilgi için bkz: [Hizmet Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features"></a>Özellikler

* Kullanım kolaylığı: yapılandırma yok Azure tarafından sağlanan ad çözümlemesi kullanmak için gereklidir.
* Azure tarafından sağlanan ad çözümleme hizmeti oluşturmak ve kendi DNS sunucularınızı kümelerini yönetmek için gereken kaydetme yüksek oranda kullanılabilir.
* Kendi DNS sunucularıyla birlikte şirket içi ve Azure ana bilgisayar adları çözümlemek için kullanılabilir.
* Ad çözümlemesi rol örnekleri/VMs aynı bulut hizmeti için bir FQDN gerek kalmadan içindeki arasında sağlanır.
* Ad çözümlemesi için bir FQDN gerek kalmadan Resource Manager dağıtım modeli kullanan sanal ağları VM'ler arasında sağlanır. Klasik dağıtım modelinde sanal ağlar farklı bulut Hizmetleri adlarında çözümlenirken bir FQDN gerektirir. 
* Dağıtımlarınızı, en iyi şekilde açıklayan bir ana bilgisayar adları kullanabilirsiniz otomatik olarak oluşturulan adlarıyla çalışma yerine.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Oluşturulan Azure DNS soneki değiştirilemez.
* Kendi kayıtları el ile kaydettirilemedi.
* WINS ve NetBIOS desteklenmez (Windows Gezgini'nde, VM'ler göremiyorum).
* Ana bilgisayar adları DNS uyumlu olması gerekir. Adları, yalnızca 0-9, a-z kullanmalıdır ve '-' ve başlayamaz veya bitemez bir '-'. RFC 3696 2 bölümüne bakın.
* DNS sorgu trafiğinin her VM için kısıtlanır. Azaltma uygulamaların çoğu etkisi döndürmemelidir. İstek azaltma gözlenir, istemci tarafı önbelleğe alma etkin olduğundan emin olun. Daha fazla bilgi için bkz: [en sağlanan Azure name resolution alma](#Getting-the-most-from-Azure-provided-name-resolution).
* Yalnızca ilk 180 bulut hizmetlerinde VM'ler, her bir sanal ağı Klasik dağıtım modelinde kaydedilir. Bu sınır, Resource Manager dağıtım modellerinde sanal ağlar için geçerli değildir.

## <a name="dns-client-configuration"></a>DNS istemcisi yapılandırması

### <a name="client-side-caching"></a>İstemci tarafı önbelleğe alma

Her DNS sorgusu, ağ üzerinden gönderilmesi gerekiyor. İstemci tarafı önbelleğe alma gecikme süresini azaltmak ve yerel önbelleğinden yinelenen DNS sorgularını çözülerek ağ blips esnekliği geliştirmek yardımcı olur. DNS kayıtlarını içeren bir yaşam süresi (TTL) kayıt yenilik, istemci tarafı önbelleğe alma çoğu durumlar için uygun olacak şekilde etkilemeden kayıt mümkün olduğunca uzun bir süre saklamak önbellek sağlar.

Windows DNS İstemcisi varsayılan yerleşik bir DNS önbelleği sahiptir. Bazı Linux distro'lar varsayılan önbelleğe alma dahil etmeyin. DNS önbelleği (olmadığından yerel bir önbellek zaten denetledikten sonra), her bir Linux VM ekleme önerilir.

Kullanılabilir paketler önbelleğe alma farklı DNS mevcuttur. Örneğin, dnsmasq. Aşağıdaki adımları dnsmasq üzerinde en yaygın distro'lar yükleme listesi:

* **Ubuntu (kullandığı resolvconf)**:
  * Dnsmasq paketiyle yükleme `sudo apt-get install dnsmasq`.
* **SUSE (kullandığı netconf)**:
  * Dnsmasq paketiyle yükleme `sudo zypper install dnsmasq`.
  * Dnsmasq hizmetiyle etkinleştirmek `systemctl enable dnsmasq.service`. 
  * Dnsmasq hizmetiyle Başlat `systemctl start dnsmasq.service`. 
  * Düzen **/etc/sysconfig/network/config** değiştirip *NETCONFIG_DNS_FORWARDER = ""* için *dnsmasq*.
  * Resolv.conf ile güncelleştirme `netconfig update` önbellek yerel DNS Çözümleyicisi ayarlanacak.
* **OpenLogic (NetworkManager kullanır)**:
  * Dnsmasq paketiyle yükleme `sudo yum install dnsmasq`.
  * Dnsmasq hizmetiyle etkinleştirmek `systemctl enable dnsmasq.service`.
  * Dnsmasq hizmetiyle Başlat `systemctl start dnsmasq.service`.
  * Ekleme *etki alanı adı sunucuları 127.0.0.1; başına* için **/etc/dhclient-eth0.conf**.
  * Ağ Hizmeti ile yeniden `service network restart` önbellek yerel DNS Çözümleyicisi ayarlanacak.

> [!NOTE]
> 'Dnsmasq' paket yalnızca Linux için kullanılabilir birçok DNS önbellekleri biridir. Kullanmadan önce belirli ihtiyaçlarınızı kendi uygunluğuna denetleyin ve başka bir önbellek yüklenir.
> 
> 
    
### <a name="client-side-retries"></a>İstemci tarafı yeniden deneme

DNS öncelikle bir UDP protokolüdür. UDP protokolünü ileti teslimi garanti etmez gibi yeniden deneme mantığı DNS protokolün kendini ele alınır. Her DNS istemcisi (işletim sistemi) oluşturanın tercih bağlı olarak farklı yeniden deneme mantığı sergilemesine:

* Windows işletim sistemlerinin bir sonra ikinci ve daha sonra tekrar başka bir 2, 4 ve başka sonra dört saniye yeniden deneyin. 
* Varsayılan Linux kurulumu yeniden denemelerden sonra beş saniyede. Yeniden deneme beş kez 1 saniyelik aralıklarla değiştirmeden, önerilir.

Bir Linux VM üzerinde geçerli ayarları kontrol `cat /etc/resolv.conf`. Bakmak *seçenekleri* satır, örneğin:

```bash
options timeout:1 attempts:5
```

Resolv.conf dosyası genellikle otomatik olarak oluşturulan ve düzenlenmemelidir. Eklemek için belirli adımlar *seçenekleri* satır distro göre farklılık gösterir:

* **Ubuntu** (resolvconf kullanır):
  * Seçenekler satırı ekleyin **/etc/resolveconf/resolv.conf.d/head**.
  * Çalıştırma `resolvconf -u` güncelleştirmek için.
* **SUSE** (netconf kullanır):
  * Ekleme *timeout:1 denemeleri: 5* için **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parametresinde **/etc/sysconfig/network/config**. 
  * Çalıştırma `netconfig update` güncelleştirmek için.
* **OpenLogic** (NetworkManager kullanır):
  * Ekleme *Yankı "timeout:1 denemeleri: 5 Seçenekleri"* için **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  * İle güncelleştirmeniz `service network restart`.

## <a name="name-resolution-using-your-own-dns-server"></a>Kendi DNS sunucu kullanılarak ad çözümleme

### <a name="vms-and-role-instances"></a>VM'ler ve rol örnekleri

Ad çözümleme gereksinimlerinizi Azure tarafından sağlanan örneğin ne zaman özelliklerini Active Directory etki alanları kullanarak Git veya sanal ağlar arasında DNS çözümlemesi gerektiğinde bir dizi durum vardır. Bu senaryolarını kapsamak üzere Azure kendi DNS sunucularını kullanmak yeteneği sağlar.

Bir sanal ağ içinde DNS sunucuları, bu sanal ağ içinde ana bilgisayar adları çözümlemek için Azure'nın özyinelemeli Çözümleyicileri için DNS sorgularını iletebilirsiniz. Örneğin, Azure'da çalışan bir etki alanı denetleyicisi (DC) kendi etki alanları için DNS sorgularını yanıtlamak ve diğer tüm sorgular için Azure iletin. Sorguları iletme (DC) aracılığıyla şirket içi kaynaklara ve Azure tarafından sağlanan ana bilgisayar adları (aracılığıyla ileticisi) görmek sanal makineleri sağlar. Azure'nın özyinelemeli çözümleyiciler erişimi 168.63.129.16 sanal IP sağlanır.

DNS iletme de arası sanal ağ DNS çözümlemesi sağlar ve Azure tarafından sağlanan ana bilgisayar adları çözümlemek şirket içi makinelerinizi sağlar. Bir sanal makinenin ana bilgisayar adı çözümlemek için DNS sunucusu VM aynı sanal ağında bulunmalıdır ve Azure iletme hostname sorguları yapılandırılmış. DNS son eki her sanal ağ farklı olduğundan, doğru sanal ağı çözümlemesi için DNS sorguları göndermek için koşullu iletme kurallarını kullanabilirsiniz. Aşağıdaki görüntü iki sanal ağlar ve sanal arası ağ bu yöntemi kullanarak DNS çözümlemesi yaparken bir şirket içi ağ gösterir. Bir örnek DNS ileticisi kullanılabilir [Azure hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) ve [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Rol örnekleri VM adı "internal.cloudapp.net" DNS soneki ile birlikte kullanan FQDN kullanarak aynı sanal ağ içindeki VM'ler ad çözümlemesi gerçekleştirebilirsiniz. Ancak, bu durumda, ad çözümlemesi yalnızca Rol örneği tanımlanan VM adı varsa başarılı [rolü şeması (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Başka bir sanal ağ VM'ler ad çözümlemesi gerçekleştirmeniz gereken rol örnekleri (FQDN kullanarak **internal.cloudapp.net** soneki) açıklandığı gibi iki sanal ağ arasında iletme özel DNS sunucuları aracılığıyla bunu zorunda Bu bölüm.
>

![Arası sanal ağ DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Azure tarafından sağlanan ad çözümlemesi, bir iç DNS soneki kullanırken (`*.internal.cloudapp.net`) her VM için Azure tarafından sağlanan DHCP. Ana bilgisayar adı kayıt olduğu gibi bu soneki ana bilgisayar adı çözümlemesi sağlar *internal.cloudapp.net* bölgesi. Diğer DNS mimarileri (örneğin, etki alanına katılmış senaryoları) ile uğratan çünkü kendi ad çözümlemesi çözüm kullanırken, bu soneki VM'ler için sağlanmaz. Bunun yerine, Azure çalışmayan bir yer tutucu sağlar (*reddog.microsoft.com*).

Gerekirse, PowerShell veya API kullanarak iç DNS soneki belirlenebilir:

* Resource Manager dağıtım modellerinde sanal ağlar için soneki aracılığıyla kullanılabilir [ağ arabirim kartı](virtual-network-network-interface.md) kaynak veya aracılığıyla [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) cmdlet'i.
* Klasik dağıtım modellerinde soneki aracılığıyla kullanılabilir [alma dağıtım API'si](https://msdn.microsoft.com/library/azure/ee460804.aspx) çağrısı veya aracılığıyla [Get-AzureVM-Debug](/powershell/module/azure/get-azurevm) cmdlet.

Sorgular için Azure iletme gereksinimlerinize göre değil, kendi DNS çözümü sağlamanız gerekir. DNS çözüm gerekir:

* Aracılığıyla uygun ana bilgisayar adı çözümlemesi sağlamak [DDNS](virtual-networks-name-resolution-ddns.md), örneğin. Azure'nın DHCP kiralarını uzun ve atma DNS kaldırabilir DNS kaydı atmayı devre dışı bırakmanız gerekebilir DDNS kullanarak erken kaydediyorsa unutmayın. 
* Dış etki alanı adlarının çözümlemesine izin verecek şekilde uygun özyinelemeli çözümleme sağlar.
* Erişilebilir (TCP ve UDP bağlantı noktası 53), hizmet verdiği istemcilerden olması ve internet erişimine sahip olmalıdır.
* Dış aracıları tarafından teşkil tehditlerin azaltılmasına Internet'ten erişime karşı korunması.

> [!NOTE]
> Azure VM'ler DNS sunucuları olarak kullanırken en iyi performans için IPv6 devre dışı bırakılması gerekir ve bir [örnek düzeyinde ortak IP](virtual-networks-instance-level-public-ip.md) her DNS sunucusuna VM atanmalıdır. Ek Performans Analizi ve Windows Server DNS sunucusu olarak kullanırken en iyi duruma getirme için bkz: [ad çözümlemesi performansını özyinelemeli Windows DNS Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web Apps
App Service Web uygulamasından sonra ek olarak, DNS ileticisi sahip bir özel DNS sunucusu kurma aynı sanal ağdaki sanal makineleri için sanal bir ağa bağlı ad çözümlemesi gerçekleştirmeniz gerekiyorsa, Azure (sanal IP 168.63.129.16) sorguları ileten , aşağıdaki adımları gerçekleştirmeniz gerekir:
* Sanal ağ tümleştirmesinin uygulama hizmeti Web uygulaması için henüz, açıklandığı gibi yapmadıysanız etkinleştirmek [uygulamanızı bir sanal ağ ile tümleştirmek](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Azure portalında Web uygulaması barındırma için uygulama hizmeti planı seçin **eşitleme ağ** altında **ağ**, **sanal ağ tümleştirmesinin**aşağıda gösterildiği gibi Resim:

    ![Web uygulamaları sanal ağ ad çözümlemesi](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Ad çözümlemesi, App Service Web uygulamasından bir sanal ağa bağlı, farklı bir sanal ağ vm'lerinin kullanımını hem de sanal ağlarda özel DNS sunucuları gibi gerektirir:
* Ayrıca sorguları Azure'un özyinelemeli çözümleyici (sanal IP 168.63.129.16) iletebilir bir VM'de, hedef sanal ağınızdaki bir DNS sunucusu ayarlayın. Bir örnek DNS ileticisi kullanılabilir [Azure hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) ve [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Kaynak sanal ağda bir VM'de DNS ileticisi ayarlayın. Bu DNS ileticisi sorguları, hedef sanal ağınıza DNS sunucusuna iletmek üzere yapılandırın.
* Kaynak DNS sunucunuzun Kaynak sanal ağ ayarlarını yapılandırın.
* Bölümündeki yönergeleri izleyerek kaynak sanal ağa bağlamak App Service Web uygulamanız için sanal ağ tümleştirme sağlamak [uygulamanızı bir sanal ağ ile tümleştirmek](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Azure portalında web uygulaması barındırma için uygulama hizmeti planı seçin **eşitleme ağ** altında **ağ**, **sanal ağ tümleştirmesinin**. 

## <a name="specifying-dns-servers"></a>DNS sunucularını belirtme
Kendi DNS sunucularını kullanırken, Azure sanal ağ veya ağ arabirimi (Resource Manager) başına birden çok DNS sunucusu belirtin / bulut hizmeti (Klasik) yeteneği sağlar. Belirtilen bir bulut hizmeti/ağ arabirimi için DNS sunucuları, sanal ağ için belirtilen DNS sunucuları üzerinde önceliği alın.

> [!NOTE]
> Ağ bağlantısı özellikleri, sanal ağ bağdaştırıcısının yerini, DNS sunucusu IP'leri, değil düzenlenmesi gerekir gibi doğrudan Windows VM bunlar hizmeti sırasında silinmesi olarak onarma. 
> 
> 

Resource Manager dağıtım modelini kullanarak, DNS sunucuları Portal, API/şablonları belirtilebilir: [sanal ağ](https://msdn.microsoft.com/library/azure/mt163661.aspx) ve [ağ arabirimi](https://msdn.microsoft.com/library/azure/mt163668.aspx), veya PowerShell: [sanal ağ ](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) ve [ağ arabirimi](/powershell/module/azurerm.network/new-azurermnetworkinterface).

Klasik dağıtım modeli kullanılırken, sanal ağın DNS sunucularını portalda belirtilebilir veya [ *ağ yapılandırması* dosya](https://msdn.microsoft.com/library/azure/jj157100). Bulut Hizmetleri, DNS sunucuları aracılığıyla belirtilen [ *hizmet yapılandırmasını* dosya](https://msdn.microsoft.com/library/azure/ee758710) veya PowerShell ile kullanarak [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Zaten dağıtılmış bir sanal ağ/sanal makine için DNS ayarlarını değiştirirseniz, etkili olabilmesi için her etkilenen VM değişiklikleri için yeniden başlatmanız gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

Resource Manager dağıtım modeli:

* [Bir sanal ağ oluştur veya güncelleştir](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Bir ağ arabirimi kartı güncelle](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)
* [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)

Klasik dağıtım modeli:

* [Azure hizmet yapılandırma şeması](https://msdn.microsoft.com/library/azure/ee758710)
* [Sanal ağ yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100)
* [Bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md)
