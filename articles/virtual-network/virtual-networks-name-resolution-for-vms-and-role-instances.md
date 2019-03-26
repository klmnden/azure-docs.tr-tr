---
title: Azure sanal ağlarda bulunan kaynaklar için ad çözümlemesi
titlesuffix: Azure Virtual Network
description: Azure Iaas, farklı bulut Hizmetleri, Active Directory ve kendi DNS sunucunuzu kullanarak arasında karma çözümler için çözüm senaryoları adlandırın.
services: virtual-network
documentationcenter: na
author: subsarma
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/25/2019
ms.author: subsarma
ms.openlocfilehash: ea15468722fcf1b9e2649236ef4dd05549d8f460
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418746"
---
# <a name="name-resolution-for-resources-in-azure-virtual-networks"></a>Azure sanal ağlarda bulunan kaynaklar için ad çözümlemesi

Azure Iaas, PaaS ve karma çözümler barındırmak için nasıl kullanacağınız bağlı olarak, sanal makineleri (VM'ler) ve birbirleri ile iletişim kurmak için bir sanal ağda dağıtılan diğer kaynaklara izin gerekebilir. IP adresleri kullanarak iletişim etkinleştirebilirsiniz olsa da, kolay anımsanacak ve değiştirmeyin adlar kullanmak çok daha kolaydır. 

Etki alanı adları iç IP adreslerine çözümleyebilir sanal ağlara dağıtılan kaynaklara gereksinim duyduğunuzda, bunlar iki yöntemden biri kullanabilirsiniz:

* [Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution)
* [Ad çözümlemesi kendi DNS sunucunuzu kullanan](#name-resolution-that-uses-your-own-dns-server) (hangi iletmek için Azure tarafından sağlanan DNS sunucularını sorgular)

Kullandığınız ad çözümlemesi türünü nasıl kaynaklarınızı birbirleri ile iletişim kurmak gereken üzerinde bağlıdır. Aşağıdaki tabloda, senaryolar ve karşılık gelen ad çözümlemesi çözümler gösterilmektedir:

> [!NOTE]
> Senaryonuza bağlı olarak, genel Önizleme aşamasında olan Azure DNS özel bölgeleri özelliğini kullanmak isteyebilirsiniz. Daha fazla bilgi için bkz. [Azure DNS'yi özel etki alanları için kullanma](../dns/private-dns-overview.md).
>

| **Senaryo** | **Çözüm** | **Son eki** |
| --- | --- | --- |
| VM'ler arasında ad çözümlemesine aynı bulut hizmetindeki rol örnekleri aynı sanal ağ veya Azure bulut Hizmetleri bulunur. | [Azure DNS özel bölgeleri](../dns/private-dns-overview.md) veya [Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution) |Ana bilgisayar adı veya FQDN |
| Farklı sanal ağlardaki sanal makineleri veya rol örneğini farklı bulut hizmetleri arasında ad çözümlemesine. |[Azure DNS özel bölgeleri](../dns/private-dns-overview.md) veya, müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasında sorguları iletme. Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| Ad çözümlemesi bir Azure App Service'in (Web uygulaması, işlev veya Bot) rol örneklerini veya Vm'leri aynı sanal ağdaki sanal ağ tümleştirmesi kullanarak. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasında sorguları iletme. Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| App Service Web Apps aynı sanal ağdaki ad çözünürlüğünü vm'lere. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasında sorguları iletme. Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| Ad çözünürlüğünü App Service Web Apps bir sanal ağdaki VM'ler için farklı bir sanal ağ içinde. |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasında sorguları iletme. Kendi DNS sunucunuzu kullanarak ad çözümleme konusuna bakın. |Yalnızca FQDN |
| Sanal makineleri veya rol örneklerini azure'da şirket içi bilgisayar ve hizmet adlarının çözümlenmesini. |DNS sunucuları (şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya DNS ikincil bölge aktarımlarını, örneğin kullanarak eşitlenen) müşteri tarafından yönetilen. Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| Şirket içi bilgisayarlardan Azure konak adı çözümlemesi. |Bir müşteri tarafından yönetilen DNS proxy sunucusu karşılık gelen sanal ağ içinde sorguları, proxy sunucusu sorguları çözümlemesi için Azure'a iletir. Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-that-uses-your-own-dns-server). |Yalnızca FQDN |
| İç IP'ler için ters DNS. |[Kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-that-uses-your-own-dns-server). |Uygulanamaz |
| Vm'leri ya da farklı bulut Hizmetleri, sanal ağ içinde yer alan rol örneklerinin arasındaki ad çözümlemesi. |Geçerli değildir. VM'ler ve rol örneğini farklı bulut hizmetleri arasında bağlantı, bir sanal ağ dışında desteklenmiyor. |Uygulanamaz|

## <a name="azure-provided-name-resolution"></a>Azure tarafından sağlanan ad çözümlemesi

Genel DNS adı çözümlemesini yanı sıra Azure Vm'leri ve aynı sanal ağ veya Bulut hizmetinin içinde bulunan rol örnekleri için iç ad çözümleme sağlar. VM'ler ve bulut hizmetinde örnekleri aynı DNS sonekine paylaşarak, tek başına ana bilgisayar adı yeterli sağlayın. Ancak klasik dağıtım modeli kullanılarak dağıtılan sanal ağlarda farklı bulut Hizmetleri farklı DNS sonekleri olması. Bu durumda, farklı bulut hizmetleri arasında adlarını çözümlemek için FQDN gerekir. Azure Resource Manager dağıtım modeli kullanılarak dağıtılan sanal ağlarda DNS son eki FQDN gerekli olmadığı için sanal ağ arasında tutarlıdır. DNS adları hem VM'ler hem de ağ arabirimlerinin atanabilir. Azure tarafından sağlanan ad çözümlemesi herhangi bir yapılandırma gerektirmez rağmen önceki tabloda ayrıntılı olarak tüm dağıtım senaryoları için uygun seçim değildir.

> [!NOTE]
> Web ve çalışan rolleri kullanılarak bulut Hizmetleri, Azure Hizmet Yönetimi REST API'sini kullanarak rol örneği iç IP adreslerini de erişebilirsiniz. Daha fazla bilgi için [Hizmet Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/ee460799.aspx). Adres, rol adı ve örnek sayısına göre belirlenmektedir. 
>
>

### <a name="features"></a>Özellikler

Azure tarafından sağlanan ad çözümlemesi aşağıdaki özellikleri içerir:
* Kullanım kolaylığı. Yapılandırma gerekmiyor.
* Yüksek kullanılabilirlik. Kendi DNS sunucularınızı, küme oluşturma ve yönetme gerek yoktur.
* Hem şirket içi hem de Azure ana bilgisayar adlarını çözümlemek için yararlı kendi DNS sunucularıyla birlikte hizmet kullanabilirsiniz.
* VM'ler ve rol örnekleri aynı bulut hizmetindeki bir FQDN gerek kalmadan arasında ad çözümlemesine kullanabilirsiniz.
* Ad çözümlemesi için bir FQDN gerek kalmadan Azure Resource Manager dağıtım modelini kullanan sanal ağlardaki sanal makineler arasında kullanabilirsiniz. Farklı bulut hizmetlerinde adları çözümlerken Klasik dağıtım modelinde sanal ağlar bir FQDN gerektirir. 
* Dağıtımlarınızı, en iyi şekilde açıklayan bir ana bilgisayar adları kullanabilirsiniz yerine otomatik olarak oluşturulan adları ile çalışma.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure tarafından sağlanan ad çözümlemesi kullanırken dikkate alınacak noktalar:
* Azure tarafından oluşturulan DNS son eki değiştirilemez.
* El ile kendi kayıtlarınızı kaydedilemiyor.
* WINS ve NetBIOS desteklenmez. Vm'lerinizi Windows Gezgini'nde göremiyorum.
* Ana bilgisayar adları, DNS ile uyumlu olması gerekir. Adları, yalnızca 0-9, a-z, kullanmalıdır ve '-' ve başlayamaz veya bitemez bir '-'.
* DNS sorgu trafiği her VM için kısıtlanır. Azaltma, çoğu uygulama etkisi olmaması gerekir. İstek azaltma gözlemlendiğinde istemci tarafı önbelleğe alma etkin olduğundan emin olun. Daha fazla bilgi için [DNS istemci yapılandırması](#dns-client-configuration).
* Yalnızca ilk 180 bulut hizmetlerindeki VM'ler, Klasik dağıtım modelinde her bir sanal ağ için kaydedilir. Bu sınır, Azure Resource Manager'da sanal ağlar için geçerli değildir.
* Azure DNS IP adresi: 168.63.129.16. Bu statik bir IP adresi ve değişmez.

## <a name="dns-client-configuration"></a>DNS istemcisi yapılandırması

Bu bölüm, istemci tarafı önbelleğe alma ve istemci tarafı deneme kapsar.

### <a name="client-side-caching"></a>İstemci tarafı önbelleğe alma

Her DNS sorgusu, ağ üzerinden gönderilmesi gerekir. İstemci tarafı önbelleğe alma, gecikme süresini azaltın ve yerel önbelleğinden alınan yinelenen DNS sorguları çözerek ağ blips dayanıklılık artırılmasına yardımcı olur. DNS kayıtlarını kayıt güncellik etkilemeden kayıt için olabildiğince uzun süre saklamak önbellek sağlayan bir yaşam süresi (TTL) mekanizması içerir. Bu nedenle, istemci tarafı önbelleğe alma çoğu durumlar için uygundur.

Yerleşik DNS önbelleğini varsayılan Windows DNS istemcisi vardır. Bazı Linux dağıtımlarında, varsayılan olarak önbelleğe alma içermez. Olmadığından yerel önbellek zaten bulursanız, her bir Linux VM için bir DNS önbelleği ekleyin.

Bir dizi farklı DNS önbelleğe alma (dnsmasq gibi) kullanılabilir paketler vardır. En yaygın dağıtımlarında dnsmasq yükleneceği açıklanmıştır:

* **Ubuntu (uses resolvconf)**:
  * Dnsmasq paketi yükleme `sudo apt-get install dnsmasq`.
* **SUSE (kullandığı netconf)**:
  * Dnsmasq paketi yükleme `sudo zypper install dnsmasq`.
  * Dnsmasq hizmetiyle etkinleştirme `systemctl enable dnsmasq.service`. 
  * Dnsmasq hizmetle başlar `systemctl start dnsmasq.service`. 
  * Düzen **/etc/sysconfig/network/config**, değiştirip *NETCONFIG_DNS_FORWARDER = ""* için *dnsmasq*.
  * Resolv.conf ile güncelleştirme `netconfig update`, önbellek yerel DNS Çözümleyicisi ayarlanacak.
* **OpenLogic (NetworkManager kullanır)**:
  * Dnsmasq paketi yükleme `sudo yum install dnsmasq`.
  * Dnsmasq hizmetiyle etkinleştirme `systemctl enable dnsmasq.service`.
  * Dnsmasq hizmetle başlar `systemctl start dnsmasq.service`.
  * Ekleme *etki alanı adı sunucuları 127.0.0.1; önüne ekleyin* için **/etc/dhclient-eth0.conf**.
  * Ağ Hizmeti ile yeniden `service network restart`, önbellek yerel DNS Çözümleyicisi ayarlanacak.

> [!NOTE]
> Dnsmasq paket yalnızca Linux için kullanılabilen birçok DNS Önbellek biridir. Kullanmadan önce kendi gereksinimlerinize uygunluğu ve başka bir önbellek yüklü olduğunu denetleyin.
>
>
    
### <a name="client-side-retries"></a>İstemci tarafı yeniden deneme

DNS öncelikle bir UDP protokolüdür. UDP protokolünü ileti teslimi garanti etmez olduğundan, yeniden deneme mantığı DNS protokolün kendisini ele alınır. Her DNS istemcisi (işletim sistemi) oluşturan kişinin tercih bağlı olarak farklı yeniden deneme mantığını gösteren:

* Windows işletim sistemlerinin bir sonraki ikinci ve daha sonra tekrar diğerinden sonra dört saniye ve başka bir dört saniye iki saniye yeniden deneyin. 
* Varsayılan Linux kurulumu, beş saniyeden sonra deneme. Yeniden deneme özellikleri için beş kez, bir saniyelik aralıklarla değiştirilmesi önerilir.

Geçerli ayarları ile bir Linux sanal makinesinde kontrol `cat /etc/resolv.conf`. Bakmak *seçenekleri* satır, örneğin:

```bash
options timeout:1 attempts:5
```

Resolv.conf dosyasını genellikle otomatik olarak oluşturuldu ve düzenlenmemelidir. Ekleme için belirli adımlar *seçenekleri* satır farklılık göre dağıtım:

* **Ubuntu** (resolvconf kullanılır):
  1. Ekleme *seçenekleri* için satır **/etc/resolvconf/resolv.conf.d/tail**.
  2. Çalıştırma `resolvconf -u` güncelleştirilecek.
* **SUSE** (netconf kullanılır):
  1. Ekleme *timeout:1 girişimi: 5* için **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parametresinde **/etc/sysconfig/network/config**.
  2. Çalıştırma `netconfig update` güncelleştirilecek.
* **OpenLogic** (uses NetworkManager):
  1. Ekleme *Yankı "timeout:1 girişimi: 5 Seçenekleri"* için **/etc/NetworkManager/dispatcher.d/11-dhclient**.
  2. İle güncelleştirme `service network restart`.

## <a name="name-resolution-that-uses-your-own-dns-server"></a>Kendi DNS sunucunuzu kullanan ad çözümlemesi

Bu bölümde, VM'ler ve rol örnekleri web uygulamalarını kapsar.

### <a name="vms-and-role-instances"></a>VM'ler ve rol örnekleri

Ad çözümleme ihtiyaçlarınızı, Azure tarafından sunulan özelliklerin ötesine geçebilir. Örneğin, ihtiyacınız olabilecek Microsoft Windows Server Active Directory etki alanlarını kullanmak için sanal ağlar arasında DNS adlarını çözemez. Bu senaryolarınızı kapsaması amacıyla Azure kendi DNS sunucularınızı kullanmanıza olanak sağlar.

Bir sanal ağdaki DNS sunucularının, azure'da özyinelemeli Çözümleyicileri için DNS sorguları iletebilirsiniz. Bu, söz konusu sanal ağ içinde ana bilgisayar adlarını çözümlemek sağlar. Örneğin, Azure'da çalışan bir etki alanı denetleyicisi (DC), etki alanları için DNS sorgularını yanıtlamak ve diğer tüm sorguları Azure'a iletmek. Sorguları iletme (DC) aracılığıyla şirket içi kaynaklar ve Azure tarafından sağlanan ana bilgisayar adları (aracılığıyla ileticisi) görmek VM'lerin sağlar. Azure'da özyinelemeli Çözümleyicileri erişim sanal IP'SİNDEN (168.63.129.16) sağlanır.

DNS iletme de sanal ağlar arasında DNS çözümlemesi sağlar ve Azure tarafından sağlanan ana bilgisayar adlarını çözümlemek şirket içi makinelerinizi sağlar. Bir sanal makinenin ana bilgisayar adını çözümlemek için DNS sunucusu VM aynı sanal ağda bulunması gerekir ve ileriye doğru ana bilgisayar adı sorgularını azure'a şekilde yapılandırılması. DNS son eki her bir sanal ağ farklı olduğu için çözüm için doğru sanal ağı için DNS sorguları göndermek için koşullu iletme kurallarını kullanabilirsiniz. Aşağıdaki görüntüde, iki sanal ağ ve DNS çözümlemesi bu yöntemi kullanarak sanal ağlar arasında yapılması şirket içi ağı gösterilmektedir. Bir örnek DNS ileticisi kullanılabilir [Azure hızlı başlangıç şablonları galeri](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) ve [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Bir rol örneği aynı sanal ağda VM ad çözümlemesi gerçekleştirebilirsiniz. Bunu sanal makinenin ana bilgisayar adını oluşur FQDN kullanarak yapar ve **internal.cloudapp.net** DNS soneki. Ancak, bu durumda, ad çözümlemesi yalnızca Rol örneği tanımlanan VM adı varsa, başarılı [rol şeması (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/jj156212.aspx).
> <Role name="<role-name>" vmName="<vm-name>">
>
> Başka bir sanal ağda VM ad çözümlemesi gerçekleştirmek için gereken rol örnekleri (kullanarak FQDN **internal.cloudapp.net** soneki) (arasında iletme özel DNS sunucuları bu bölümde açıklanan yöntemi kullanarak bunu gerekir iki sanal ağ).
>

![Sanal ağlar arasında DNS diyagramı](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Azure tarafından sağlanan ad çözümlemesi kullanırken Azure dinamik konak Yapılandırma Protokolü (DHCP) bir iç DNS soneki sağlar (**. internal.cloudapp.net**) her VM için. Ana bilgisayar adı kayıtları olduğundan bu son ek ana bilgisayar adı çözümlemesi sağlayan **internal.cloudapp.net** bölge. Kendi ad çözümlemesi çözümünüzü kullanılırken, diğer DNS mimarileri (örneğin, etki alanına katılmış senaryoları) ile uğratan çünkü bu son ek Vm'lere sağlanmaz. Bunun yerine, Azure işlevsiz bir yer tutucu sağlar (*reddog.microsoft.com*).

Gerekirse, PowerShell veya API kullanarak iç DNS soneki belirleyebilirsiniz:

* Azure Resource Manager dağıtım modellerindeki Sanal Ağları için soneki aracılığıyla kullanılabilir [ağ arabirimi REST API](https://docs.microsoft.com/rest/api/virtualnetwork/networkinterfaces), [Get-AzNetworkInterface](/powershell/module/az.network/get-aznetworkinterface) PowerShell cmdlet'ini ve [ az ağ nic show](/cli/azure/network/nic#az-network-nic-show) Azure CLI komutu.
* Klasik dağıtım modellerinde soneki aracılığıyla kullanılabilir [alma dağıtım API](https://msdn.microsoft.com/library/azure/ee460804.aspx) arayın veya [Get-AzureVM-hata ayıklama](/powershell/module/servicemanagement/azure/get-azurevm) cmdlet'i.

Azure'a sorguları iletme ihtiyaçlarınıza uygun değil, kendi DNS çözüm sağlamanız gerekir. DNS çözümünüzü gerekir:

* Uygun konak aracılığıyla ad çözümlemesi sağlamak [DDNS](virtual-networks-name-resolution-ddns.md), örneğin. DDNS kullanıyorsanız, DNS kaydı atmayı devre dışı bırakmanız gerekebilir. Azure DHCP kiralarını uzun ve atma DNS kayıtlarını beklenenden önce kaldırabilir. 
* Dış etki alanı adlarının çözümlenmesini izin vermek için uygun bir özyinelemeli çözümleme sağlar.
* Erişilebilir (TCP ve UDP bağlantı noktası 53) sunduğu istemcilerden ve internet erişimine sahip olmalıdır.
* Dış aracıları tarafından teşkil tehditleri azaltmak için internet'ten erişime karşı korunması.

> [!NOTE]
> DNS sunucuları olarak Azure Vm'leri kullanırken en iyi performans için IPv6 devre dışı bırakılması gerekir. A [genel IP adresi](virtual-network-public-ip-address.md) her DNS sunucusu VM'sine atanmalıdır. Ek Performans Analizi ve DNS sunucunuz, Windows Server kullanırken en iyi duruma getirme görmek için [adı özyinelemeli bir Windows Server 2012 R2'dns çözümlemesi performansını](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web uygulamaları
App Service, Vm'leri aynı sanal ağdaki bir sanal ağa bağlı kullanılarak oluşturulan web uygulamanıza ilişkin ad çözümlemesi gerçekleştiremez gerektiğini varsayalım. Ayarlanmasına ek olarak, bir özel DNS sorguları (sanal IP'SİNDEN (168.63.129.16)), Azure'a ileten DNS ileticisi olan sunucuyu aşağıdaki adımları gerçekleştirin:
1. Web uygulamanız için sanal ağ tümleştirmesi zaten açıklandığı yapılmaması durumunda etkinleştirme [uygulamanızı bir sanal ağ ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Azure portalında web uygulamasını barındırmak için App Service planı seçin **eşitleme ağ** altında **ağ**, **sanal ağ tümleştirmesi**.

    ![Sanal ağ adı çözümlemesi ekran görüntüsü](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

App Service, sanal makineleri farklı bir sanal ağdaki bir sanal ağa bağlı kullanılarak oluşturulan web uygulamanıza ilişkin ad çözümlemesi gerçekleştirmeniz gerekiyorsa, her iki sanal ağda özel DNS sunucularını kullanacak şekilde vardır:

* Bir VM'de (sanal IP'SİNDEN (168.63.129.16)) azure'da özyinelemeli çözümleyici için sorguları da iletebilir, hedef sanal ağınızda bir DNS sunucusu ayarlayın. Bir örnek DNS ileticisi kullanılabilir [Azure hızlı başlangıç şablonları galeri](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) ve [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Bir VM'de kaynak sanal ağdaki DNS ileticisi ayarlayın. Bu DNS ileticisi hedef sanal ağınızdaki DNS sunucusu sorguları iletecek şekilde yapılandırın.
* Kaynak DNS sunucunuzun, kaynak sanal ağınızın ayarlarını yapılandırın.
* Web uygulamanızın bölümündeki yönergeleri izleyerek kaynak sanal ağa bağlamak sanal ağ tümleştirmesi etkinleştirme [uygulamanızı bir sanal ağ ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Azure portalında web uygulamasını barındırmak için App Service planı seçin **eşitleme ağ** altında **ağ**, **sanal ağ tümleştirmesi**.

## <a name="specify-dns-servers"></a>DNS sunucusu belirtin
Kendi DNS sunucularınızı kullanırken, Azure sanal ağ başına birden çok DNS sunucusu belirtme olanağı sağlar. Birden çok DNS sunucusu, ağ arabirimi (Azure Resource Manager için) veya Bulut hizmetinden (Klasik dağıtım modeli için) başına de belirtebilirsiniz. Bir ağ arabirimi veya Bulut hizmeti için belirtilen DNS sunucuları, sanal ağ için belirtilen DNS sunucuları üzerinde önceliği alın.

> [!NOTE]
> DNS sunucusu IP'leri, gibi ağ bağlantısı özellikleri, doğrudan Windows Vm'lerinizde düzenlenmemelidir. Bunlar hizmet sırasında silinmesi olmasıdır sanal ağ bağdaştırıcısı yerini, onarımı.
>
>

Azure Resource Manager dağıtım modeli kullanılırken bir sanal ağ ve ağ arabirimi için DNS sunucuları belirtebilirsiniz. Ayrıntılar için bkz [sanal ağ yönetme](manage-virtual-network.md) ve [bir ağ arabirimi yönetmek](virtual-network-network-interface.md).

> [!NOTE]
> Sanal ağınız için özel DNS sunucusu için iyileştirilmiş, en az bir DNS sunucusu IP adresi belirtmeniz gerekir; Aksi takdirde, sanal ağ yapılandırma yoksay ve bunun yerine Azure tarafından sağlanan DNS kullanın.
>
>

Azure portalında Klasik dağıtım modeli kullanılırken, sanal ağın DNS sunucuları belirtebilirsiniz veya [ağ yapılandırma dosyası](https://msdn.microsoft.com/library/azure/jj157100). Bulut Hizmetleri için DNS sunucuları aracılığıyla belirtebilirsiniz [hizmet yapılandırma dosyası](https://msdn.microsoft.com/library/azure/ee758710) veya PowerShell kullanarak [New-AzureVM](/powershell/module/servicemanagement/azure/new-azurevm).

> [!NOTE]
> Bir sanal ağ veya zaten dağıtılmış bir sanal makine için DNS ayarlarını değiştirirseniz, etkili olması için etkilenen her VM için değişiklikleri yeniden başlatmanız gerekir.
>
>

## <a name="next-steps"></a>Sonraki adımlar

Azure Resource Manager dağıtım modeli:

* [Sanal ağ yönetme](manage-virtual-network.md)
* [Bir ağ arabirimi ile yönetme](virtual-network-network-interface.md)

Klasik dağıtım modeli:

* [Azure hizmet yapılandırma şeması](https://msdn.microsoft.com/library/azure/ee758710)
* [Virtual Network yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100)
* [Bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md)