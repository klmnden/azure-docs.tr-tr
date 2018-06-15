---
title: Azure sanal ağlarda bulunan kaynaklar için ad çözümlemesi | Microsoft Docs
description: Azure Iaas, farklı bulut Hizmetleri, Active Directory ve kendi DNS sunucusu kullanılarak arasındaki karma çözümleri için ad çözümleme senaryoları.
services: virtual-network
documentationcenter: na
author: subsarma
manager: vitinnan
editor: ''
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: subsarma
ms.openlocfilehash: 32d4f72afb4cd18e6b66c52eb78b2fc7b6b75cbd
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31603666"
---
# <a name="name-resolution-for-resources-in-azure-virtual-networks"></a>Azure sanal ağlarda bulunan kaynaklar için ad çözümlemesi

Azure Iaas ve PaaS karma çözümleri barındırmak için nasıl kullanacağınız bağlı olarak, sanal makineleri (VM'ler) ve birbirleri ile iletişim kurmak için bir sanal ağda dağıtılmış başka kaynaklar izin gerekebilir. IP adreslerini kullanarak iletişim etkinleştirebilirsiniz ancak kolay anımsanacak ve değiştirmeyin adlar kullanmak çok daha kolaydır. 

Sanal ağlarda dağıtılan kaynak etki alanı adları iç IP adresleri çözümlemeniz gerekiyorsa, bunlar iki yöntemden birini kullanabilirsiniz:

* [Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution)
* [Ad, kendi DNS sunucusunu kullanır çözümlemesi](#name-resolution-that-uses-your-own-dns-server) (hangi iletmek için Azure tarafından sağlanan DNS sunucularını sorgular) 

Kullandığınız ad çözümlemesi türünün nasıl kaynaklarınızı birbirleri ile iletişim kurmak gereksinimlerine göre değişir. Aşağıdaki tabloda senaryoları ve karşılık gelen ad çözümlemesi çözümleri gösterilmektedir:

> [!NOTE]
> Senaryonuza bağlı olarak, şu anda genel önizlemede olan Azure DNS özel bölgeler özelliğini kullanmak isteyebilirsiniz. Daha fazla bilgi için bkz: [kullanarak Azure DNS özel etki alanları için](../dns/private-dns-overview.md).
>

| **Senaryo** | **Çözüm** | **Son eki** |
| --- | --- | --- |
| Ad çözümlemesi VM'ler arasında aynı sanal ağda veya Azure Cloud Services içinde rol örnekleri aynı bulut hizmetindeki yer. | [Azure DNS özel bölgeler](../dns/private-dns-overview.md) veya [Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution) |ana bilgisayar adı veya FQDN |
| Farklı sanal ağlar, sanal makineleri veya rol örneklerinin farklı bulut hizmetleri arasında ad çözümleme. |[Azure DNS özel bölgeler](../dns/private-dns-overview.md) veya, Azure (DNS proxy) tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme müşteri tarafından yönetilen DNS sunucuları. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| Ad çözümlemesi bir Azure uygulama hizmeti (Web uygulaması, işlev veya Bot) aynı sanal ağdaki sanal ağ tümleştirmesinin rol örnekleri ya da sanal makineleri kullanma. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| Name resolution App Service Web Apps vm'leri aynı sanal ağda. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| Name resolution App Service Web Apps bir sanal ağ içindeki VM'ler için farklı bir sanal ağ içinde. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-that-uses-your-own-dns-server-for-web-apps). |Yalnızca FQDN |
| Şirket içi bilgisayar ve hizmet adlarından VM'ler veya Azure rol örneklerinin çözünürlüğü. |Müşteri tarafından yönetilen DNS sunucuları (şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya bölge aktarımlarının, örneğin kullanarak eşitlenen bir DNS ikincil). Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| Şirket içi bilgisayarlardan Azure ana bilgisayar adları çözünürlüğü. |İletme sorguları bir müşteri tarafından yönetilen DNS proxy sunucusuna karşılık gelen sanal ağ proxy sunucusunu sorgular için Azure çözümlemesi için iletir. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| DNS geriye doğru iç IP için. |[Kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-that-uses-your-own-dns-server). |Uygulanamaz |
| Sanal makineler veya sanal ağ içinde olmayan farklı bulut Hizmetleri bulunan rol örnekleri arasında ad çözümleme. |Geçerli değil. VM'ler ve rol örnekleri farklı bulut hizmetleri arasında bağlantı sanal ağ dışında desteklenmiyor. |Uygulanamaz|

## <a name="azure-provided-name-resolution"></a>Azure tarafından sağlanan ad çözümlemesi

Genel DNS adlarını çözümlenmesi yanı sıra, Azure Vm'leri ve aynı sanal ağda veya Bulut hizmeti içinde bulunan rol örnekleri için dahili ad çözümlemesi sağlar. Tek başına ana bilgisayar adı yeterli olacak şekilde VM'ler ve bulut hizmeti örnekleri aynı DNS sonekine paylaşır. Ancak klasik dağıtım modeli kullanılarak dağıtılan sanal ağlarda farklı bulut Hizmetleri, farklı DNS sonekleri. Bu durumda, farklı bulut hizmetleri arasında adları çözümlemek için FQDN gerekir. Azure Resource Manager dağıtım modeli kullanılarak dağıtılan sanal ağlarda, DNS soneki FQDN gerekli olmadığı için sanal ağ arasında tutarlıdır. DNS adları, hem VM'ler hem de ağ arabirimleri için atanabilir. Azure tarafından sağlanan ad çözümlemesi herhangi bir yapılandırma gerekli olmasa da, önceki tabloda ayrıntılı olarak tüm dağıtım senaryoları için uygun seçeneği değil.

> [!NOTE]
> Bulut kullanarak Hizmetleri web ve çalışan rolleri, rol örnekleri Azure Hizmet Yönetimi REST API kullanarak iç IP adreslerini de erişebilirsiniz. Daha fazla bilgi için bkz: [Hizmet Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/ee460799.aspx). Adres rol adı ile örnek sayısına bağlıdır. 
> 
> 

### <a name="features"></a>Özellikler

Azure tarafından sağlanan ad çözümlemesi aşağıdaki özellikleri içerir:
* Kullanım kolaylığı. Herhangi bir yapılandırma gerekli değildir.
* Yüksek kullanılabilirlik. Oluşturma ve kendi DNS sunucularınızı kümelerini yönetme gerek yoktur.
* Şirket içi ve Azure ana bilgisayar adları çözümlemek için yararlı kendi DNS sunucularıyla birlikte hizmet kullanabilirsiniz.
* VM'ler ve rol örnekleri aynı bulut hizmeti, bir FQDN gerek kalmadan içinde arasında ad çözümlemesini kullanabilir.
* Azure Resource Manager dağıtım modeli, bir FQDN için gerek kalmadan kullanan sanal ağları VM'ler arasında ad çözümlemesini kullanabilir. Farklı bulut Hizmetleri adları çözümlerken Klasik dağıtım modelinde sanal ağlar bir FQDN gerektirir. 
* Dağıtımlarınızı, en iyi şekilde açıklayan ana bilgisayar adlarını kullanabilirsiniz otomatik olarak oluşturulan adlarıyla çalışma yerine.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure tarafından sağlanan ad çözümlemesi kullanırken dikkate alınacak noktalar:
* Oluşturulan Azure DNS soneki değiştirilemez.
* Kendi kayıtları el ile kaydettirilemedi.
* WINS ve NetBIOS desteklenmez. Windows Gezgini'nde, VM'ler göremezsiniz.
* Ana bilgisayar adlarını DNS uyumlu olması gerekir. Adları, yalnızca 0-9, a-z, kullanmalıdır ve '-' ve başlayamaz veya bitemez bir '-'.
* DNS sorgu trafiğinin her VM için kısıtlanır. Azaltma uygulamaların çoğu etkisi döndürmemelidir. İstek azaltma gözlenir, istemci tarafı önbelleğe alma etkin olduğundan emin olun. Daha fazla bilgi için bkz: [DNS istemci yapılandırmasını](#dns-client-configuration).
* Yalnızca ilk 180 bulut hizmetlerinde VM'ler, her bir sanal ağı Klasik dağıtım modelinde kaydedilir. Bu sınır, Azure Kaynak Yöneticisi'nde sanal ağlar için geçerli değildir.

## <a name="dns-client-configuration"></a>DNS istemcisi yapılandırması

Bu bölüm, istemci tarafı önbelleğe alma ve istemci tarafı deneme kapsar.

### <a name="client-side-caching"></a>İstemci tarafı önbelleğe alma

Her DNS sorgusu, ağ üzerinden gönderilmesi gerekiyor. İstemci tarafı önbelleğe alma gecikme süresini azaltmak ve yerel önbelleğinden yinelenen DNS sorgularını çözülerek ağ blips esnekliği geliştirmek yardımcı olur. DNS kayıtlarını kayıt yenilik etkilemeden kayıt mümkün olduğunca uzun bir süre saklamak önbellek sağlayan bir yaşam süresi (TTL) mekanizması içerir. Bu nedenle, istemci tarafı önbelleğe alma çoğu durumlar için uygundur.

Varsayılan Windows DNS istemcisi yerleşik bir DNS önbelleği vardır. Bazı Linux dağıtımları varsayılan önbelleğe alma dahil etmeyin. DNS önbelleği olmadığından yerel bir önbellek zaten fark ederseniz, her bir Linux VM ekleyin.

Farklı DNS (dnsmasq gibi) kullanılabilir önbelleğe alma mevcuttur. En yaygın dağıtımları dnsmasq yükleme şöyledir:

* **Ubuntu (kullandığı resolvconf)**:
  * Dnsmasq paketiyle yükleme `sudo apt-get install dnsmasq`.
* **SUSE (kullandığı netconf)**:
  * Dnsmasq paketiyle yükleme `sudo zypper install dnsmasq`.
  * Dnsmasq hizmetiyle etkinleştirmek `systemctl enable dnsmasq.service`. 
  * Dnsmasq hizmetiyle Başlat `systemctl start dnsmasq.service`. 
  * Düzen **/etc/sysconfig/network/config**, değiştirip *NETCONFIG_DNS_FORWARDER = ""* için *dnsmasq*.
  * Resolv.conf ile güncelleştirme `netconfig update`, önbellek yerel DNS Çözümleyicisi ayarlanacak.
* **OpenLogic (NetworkManager kullanır)**:
  * Dnsmasq paketiyle yükleme `sudo yum install dnsmasq`.
  * Dnsmasq hizmetiyle etkinleştirmek `systemctl enable dnsmasq.service`.
  * Dnsmasq hizmetiyle Başlat `systemctl start dnsmasq.service`.
  * Ekleme *etki alanı adı sunucuları 127.0.0.1; başına* için **/etc/dhclient-eth0.conf**.
  * Ağ Hizmeti ile yeniden `service network restart`, önbellek yerel DNS Çözümleyicisi ayarlanacak.

> [!NOTE]
> Dnsmasq paket yalnızca Linux için kullanılabilir birçok DNS önbellekleri biridir. Kullanmadan önce kendi uygunluğuna belirli gereksinimlerinize ve diğer bir önbellek yüklendiğini denetleyin.
> 
> 
    
### <a name="client-side-retries"></a>İstemci tarafı yeniden deneme

DNS öncelikle bir UDP protokolüdür. Yeniden deneme mantığı, UDP protokolünü ileti teslimi garanti etmez çünkü DNS protokolün kendini ele alınır. Her DNS istemcisi (işletim sistemi) oluşturanın tercih bağlı olarak farklı yeniden deneme mantığı sergilemesine:

* Windows işletim sistemlerinin bir sonra ikinci ve daha sonra tekrar başka sonra iki saniye, dört saniye ve başka bir dört saniye yeniden deneyin. 
* Varsayılan Linux kurulumu yeniden denemelerden sonra beş saniyede. Yeniden deneme belirtimleri beş kez için bir saniyelik aralıklarda değiştirme öneririz.

Bir Linux VM üzerinde geçerli ayarları kontrol `cat /etc/resolv.conf`. Bakmak *seçenekleri* satır, örneğin:

```bash
options timeout:1 attempts:5
```

Resolv.conf dosya genellikle otomatik olarak oluşturulan ve düzenlenmemelidir. Eklemek için belirli adımlar *seçenekleri* satır dağıtım göre farklılık gösterir:

* **Ubuntu** (resolvconf kullanır):
  1. Ekleme *seçenekleri* için satır **/etc/resolveconf/resolv.conf.d/head**.
  2. Çalıştırma `resolvconf -u` güncelleştirmek için.
* **SUSE** (netconf kullanır):
  1. Ekleme *timeout:1 denemeleri: 5* için **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parametresinde **/etc/sysconfig/network/config**. 
  2. Çalıştırma `netconfig update` güncelleştirmek için.
* **OpenLogic** (NetworkManager kullanır):
  1. Ekleme *Yankı "timeout:1 denemeleri: 5 Seçenekleri"* için **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  2. İle güncelleştirmeniz `service network restart`.

## <a name="name-resolution-that-uses-your-own-dns-server"></a>Kendi DNS sunucusunu kullanır ad çözümlemesi

Bu bölüm, VM'ler, rol örnekleri ve web uygulamalarını kapsar.

### <a name="vms-and-role-instances"></a>VM'ler ve rol örnekleri

Ad çözümleme gereksinimlerinizi Azure tarafından sağlanan özellikleri ötesinde geçebilir. Örneğin, size gereken Microsoft Windows Server Active Directory etki alanları kullanmak için sanal ağlar arasında DNS adlarını çözemez. Bu senaryolarını kapsamak üzere Azure kendi DNS sunucularını kullanmak yeteneği sağlar.

Bir sanal ağ içinde DNS sunucuları için özyinelemeli çözümleyiciler Azure DNS sorguları iletebilir. Bu, bu sanal ağ içinde ana bilgisayar adlarını çözümlemek sağlar. Örneğin, Azure'da çalışan bir etki alanı denetleyicisi (DC) kendi etki alanları için DNS sorgularını yanıtlamak ve diğer tüm sorgular için Azure iletin. Sorguları iletme (DC) aracılığıyla şirket içi kaynaklara ve Azure tarafından sağlanan ana bilgisayar adları (aracılığıyla ileticisi) görmek sanal makineleri sağlar. Özyinelemeli çözümleyiciler Azure erişimi 168.63.129.16 sanal IP sağlanır.

DNS iletme aynı zamanda sanal ağlar arasında DNS çözümlemesi sağlar ve Azure tarafından sağlanan ana bilgisayar adlarını çözümlemek şirket içi makinelerinizi sağlar. Bir sanal makinenin ana bilgisayar adı çözümlemek için DNS sunucusu VM aynı sanal ağında bulunmalıdır ve iletme ana bilgisayar adı sorgularını Azure şekilde yapılandırılması. DNS son eki her sanal ağ farklı olduğundan, doğru sanal ağı çözümlemesi için DNS sorguları göndermek için koşullu iletme kurallarını kullanabilirsiniz. Aşağıdaki görüntü iki sanal ağ ve bu yöntemi kullanarak sanal ağlar arasındaki DNS çözümlemesi yaparken bir şirket içi ağ gösterir. Bir örnek DNS ileticisi kullanılabilir [Azure hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) ve [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Rol örneği aynı sanal ağ içindeki VM'ler ad çözümlemesi gerçekleştirebilirsiniz. Bunu VM ana bilgisayar adını oluşur FQDN kullanarak yapar ve **internal.cloudapp.net** DNS soneki. Ancak, bu durumda, ad çözümlemesi yalnızca Rol örneği tanımlanan VM adı varsa başarılı [rolü şeması (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Başka bir sanal ağ VM'ler ad çözümlemesi gerçekleştirmeniz gereken rol örnekleri (kullanarak FQDN **internal.cloudapp.net** soneki) (arasında iletme özel DNS sunucuları bu bölümde açıklanan yöntemi kullanarak bunu yapmanız gerekir iki sanal ağlar).
>

![Sanal ağlar arasında DNS diyagramı](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Azure tarafından sağlanan ad çözümlemesi kullanırken, Azure Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) bir iç DNS soneki sağlar (**. internal.cloudapp.net**) her VM. Ana bilgisayar adı kayıtları olduğundan bu soneki ana bilgisayar adı çözümlemesi sağlar **internal.cloudapp.net** bölgesi. Kendi ad çözümlemesi çözüm kullanırken, diğer DNS mimarileri (örneğin, etki alanına katılmış senaryoları) ile uğratan çünkü bu soneki VM'ler için sağlanmaz. Bunun yerine, Azure çalışmayan bir yer tutucu sağlar (*reddog.microsoft.com*).

Gerekirse, PowerShell veya API kullanarak iç DNS soneki belirleyebilirsiniz:

* Azure Resource Manager dağıtım modellerinde sanal ağlar için soneki aracılığıyla kullanılabilir [ağ arabirimi REST API](/rest/api/virtualnetwork/networkinterfaces/get), [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) PowerShell cmdlet'ini ve [az ağ NIC Göster](/cli/azure/network/nic#az-network-nic-show) Azure CLI komutu.
* Klasik dağıtım modellerinde soneki aracılığıyla kullanılabilir [alma dağıtım API'si](https://msdn.microsoft.com/library/azure/ee460804.aspx) çağrısı veya [Get-AzureVM-Debug](/powershell/module/azure/get-azurevm) cmdlet'i.

Sorgular için Azure iletme gereksinimlerinize göre değil, kendi DNS çözümü sağlamalıdır. DNS çözüm gerekir:

* Uygun konak aracılığıyla ad çözümlemesi sağlamak [DDNS](virtual-networks-name-resolution-ddns.md), örneğin. DDNS kullanıyorsanız, DNS kaydı atmayı devre dışı bırakmanız gerekebilir. Azure DHCP kiralarını uzun ve atma DNS kayıtlarını erken kaldırabilir. 
* Dış etki alanı adlarının çözümlemesine izin verecek şekilde uygun özyinelemeli çözümleme sağlar.
* Erişilebilir (TCP ve UDP bağlantı noktası 53), hizmet verdiği istemcilerden olması ve internet erişimine sahip olmalıdır.
* Dış aracıları tarafından teşkil tehditlerin azaltılmasına Internet'ten erişime karşı korunması.

> [!NOTE]
> DNS sunucuları olarak Azure VM'ler kullanırken en iyi performans için IPv6 devre dışı bırakılması gerekir. A [genel IP adresi](virtual-network-public-ip-address.md) her DNS sunucusuna VM atanmalıdır. Ek Performans Analizi ve Windows Server, DNS sunucusu olarak kullanırken en iyi duruma getirme görmek için [ad çözümlemesi performansını özyinelemeli Windows DNS Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web uygulamaları
Uygulama hizmeti, aynı sanal ağ içindeki VM'ler için sanal bir ağa bağlı kullanılarak oluşturulmuş web uygulamasından ad çözümlemesi gerektiğini varsayalım. Özel bir DNS araması ayarlanmasına ek olarak, Azure (sanal IP 168.63.129.16), sorguları ileten bir DNS ileticisi olan sunucuyu aşağıdaki adımları gerçekleştirin:
1. Web uygulamanız için sanal ağ tümleştirmesinin zaten açıklandığı gibi yapmadıysanız etkinleştirmek [uygulamanızı bir sanal ağ ile tümleştirmek](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Azure portalında web uygulaması barındırma için uygulama hizmeti planı seçin **eşitleme ağ** altında **ağ**, **sanal ağ tümleştirmesinin**.

    ![Sanal ağ ad çözümlemesi ekran görüntüsü](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Uygulama hizmeti, farklı bir sanal ağ içindeki VM'ler için sanal bir ağa bağlı kullanılarak oluşturulmuş web uygulamasından ad çözümlemesi gerçekleştirmeniz gerekiyorsa, hem de sanal ağlarda, aşağıdaki gibi özel DNS sunucularını kullanmak vardır: 
* (Sanal IP 168.63.129.16) azure'da özyinelemeli çözümleyici için ayrıca sorguları iletmek bir VM'de, hedef sanal ağınıza DNS sunucusu ayarlayın. Bir örnek DNS ileticisi kullanılabilir [Azure hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) ve [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Kaynak sanal ağda bir VM'de DNS ileticisi ayarlayın. Bu DNS ileticisi sorguları, hedef sanal ağınıza DNS sunucusuna iletmek üzere yapılandırın.
* Kaynak DNS sunucunuzun Kaynak sanal ağ ayarlarını yapılandırın.
* Bölümündeki yönergeleri izleyerek kaynak sanal ağa bağlamak web uygulamanız için sanal ağ tümleştirme sağlamak [uygulamanızı bir sanal ağ ile tümleştirmek](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Azure portalında web uygulaması barındırma için uygulama hizmeti planı seçin **eşitleme ağ** altında **ağ**, **sanal ağ tümleştirmesinin**. 

## <a name="specify-dns-servers"></a>DNS sunucusu belirtin
Kendi DNS sunucularınızı kullanırken, Azure sanal ağ başına birden çok DNS sunucusu belirleme olanağı sağlar. Ayrıca, ağ arabirimi (Azure Resource Manager) veya Bulut hizmeti (Klasik dağıtım modeli) başına birden çok DNS sunucusu belirtebilirsiniz. Ağ arabirimi veya Bulut hizmeti için belirtilen DNS sunucuları, sanal ağ için belirtilen DNS sunucuları üzerinde önceliği alın.

> [!NOTE]
> DNS sunucusu IP'leri, gibi ağ bağlantısı özellikleri doğrudan Windows VM düzenlenmemelidir. Bunlar hizmeti sırasında silinmesi olmasıdır sanal ağ bağdaştırıcısının yerini, onarma. 
> 
> 

Azure Resource Manager dağıtım modeli kullanılırken, sanal ağ ve ağ arabirimi için DNS sunucuları belirtebilirsiniz. Ayrıntılar için bkz [sanal ağ yönetme](manage-virtual-network.md) ve [bir ağ arabirimi yönetmek](virtual-network-network-interface.md).

Klasik dağıtım modeli kullanırken, Azure portalında sanal ağı için DNS sunucuları belirtebilirsiniz veya [ağ yapılandırma dosyası](https://msdn.microsoft.com/library/azure/jj157100). Bulut Hizmetleri için DNS sunucuları aracılığıyla belirtebilirsiniz [hizmet yapılandırma dosyası](https://msdn.microsoft.com/library/azure/ee758710) veya PowerShell ile kullanarak [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Bir sanal ağ veya zaten dağıtılmış bir sanal makine için DNS ayarlarını değiştirirseniz, etkili olabilmesi için her etkilenen VM değişiklikleri için yeniden başlatmanız gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

Azure Resource Manager dağıtım modeli:

* [Sanal ağ yönetme](manage-virtual-network.md)
* [Bir ağ arabirimi yönetme](virtual-network-network-interface.md)

Klasik dağıtım modeli:

* [Azure hizmet yapılandırma şeması](https://msdn.microsoft.com/library/azure/ee758710)
* [Sanal ağ yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100)
* [Bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md)
