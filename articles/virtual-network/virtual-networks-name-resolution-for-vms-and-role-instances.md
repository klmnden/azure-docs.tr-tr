---
title: "VM'ler ve rol örnekleri için çözünürlük"
description: "Azure Iaas, farklı bulut Hizmetleri, Active Directory ve kendi DNS sunucusu kullanılarak arasındaki karma çözümleri için çözüm senaryoları adı "
services: virtual-network
documentationcenter: na
author: GarethBradshawMSFT
manager: carmonm
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2016
ms.author: telmos
ms.openlocfilehash: 479cf8cf358d0b242d8ce030d8639b493e4767d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="name-resolution-for-vms-and-role-instances"></a>VM’ler ve Rol Örnekleri için Ad Çözümleme
Azure Iaas ve PaaS karma çözümleri barındırmak için nasıl kullanacağınız bağlı olarak, sanal makineleri ve birbirleri ile iletişim kurmak için oluşturduğunuz rol örneklerine izin gerekebilir. Bu iletişim, IP adreslerini kullanarak yapılabilir rağmen kolay anımsanacak ve değiştirmeyin adlar kullanmak çok daha kolaydır. 

İç IP adresleri için etki alanı adları çözümlemek rol örnekleri ve Azure üzerinde barındırılan sanal makineleri gerektiğinde, bunlar iki yöntemden birini kullanabilirsiniz:

* [Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution)
* [Kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server) (hangi iletmek için Azure tarafından sağlanan DNS sunucularını sorgular) 

Kullandığınız ad çözümlemesi türünün nasıl VM'ler ve rol örnekleri birbirleri ile iletişim kurmak gereksinimlerine göre değişir.

**Aşağıdaki tabloda senaryoları ve karşılık gelen ad çözümlemesi çözümleri gösterilmektedir:**

| **Senaryo** | **Çözüm** | **Son eki** |
| --- | --- | --- |
| Rol örnekleri veya aynı bulut hizmeti ya da sanal ağ içinde yer alan VM'ler arasında ad çözümleme |[Azure tarafından sağlanan ad çözümlemesi](#azure-provided-name-resolution) |ana bilgisayar adı veya FQDN |
| Rol örnekleri veya farklı sanal ağlarda yer alan VM'ler arasında ad çözümleme |Müşteri tarafından yönetilen DNS sunucuları (DNS proxy) Azure tarafından çözümlemesi için sanal ağlar arasındaki sorguları iletme.  Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server) |Yalnızca FQDN |
| Şirket içi bilgisayar hizmeti adları ve rol örnekleri veya azure'da VM çözümleme |Müşteri tarafından yönetilen DNS sunucuları (örneğin şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya bölge aktarımları kullanarak eşitlenen bir DNS ikincil).  Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server) |Yalnızca FQDN |
| Şirket içi bilgisayarlardan Azure ana bilgisayar adı çözümlemesi |İletme sorguları bir müşteri tarafından yönetilen DNS proxy sunucusuna karşılık gelen sanal ağ proxy sunucusunu sorgular için Azure çözümlemesi için iletir. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server) |Yalnızca FQDN |
| İç IP'ler için ters DNS |[Kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server) |yok |
| Sanal makineler veya sanal ağ içinde olmayan farklı bulut Hizmetleri bulunan rol örnekleri arasında ad çözümleme |Geçerli değil. VM'ler ve rol örnekleri farklı bulut hizmetleri arasında bağlantı sanal ağ dışında desteklenmiyor. |yok |

## <a name="azure-provided-name-resolution"></a>Azure tarafından sağlanan ad çözümlemesi
Genel DNS adlarını çözümlenmesi yanı sıra, Azure Vm'leri ve aynı sanal ağda veya Bulut hizmeti içinde bulunan rol örnekleri için dahili ad çözümlemesi sağlar.  (Ana bilgisayar adı tek başına yeterli olacak şekilde) VM'ler/örnekleri bir bulut hizmetinde aynı DNS sonekine paylaşır ancak FQDN farklı bulut hizmetleri arasında adları çözümlemek için gerektiği şekilde Klasik sanal ağlar farklı bulutta Hizmetleri farklı DNS sonekleri.  Resource Manager dağıtım modelinde sanal ağlardaki, DNS son eki (FQDN gerekli olmadığı için) sanal ağ arasında tutarlıdır ve DNS adlarını NIC ve VM'ler için atanabilir. Azure tarafından sağlanan ad çözümlemesi herhangi bir yapılandırma gerektirmez rağmen tüm dağıtım senaryoları için uygun seçeneği yukarıdaki tabloda görüldüğü gibi değil.

> [!NOTE]
> Web ve çalışan rolleri söz konusu olduğunda, Azure Hizmet Yönetimi REST API kullanarak rol adı ile örnek numarasını temel alan rol örneklerinin iç IP adreslerini de erişebilirsiniz. Daha fazla bilgi için bkz: [Hizmet Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features-and-considerations"></a>Özellikler ve ilgili önemli noktalar
**Özellikler:**

* Kullanım kolaylığı: yapılandırma yok Azure tarafından sağlanan ad çözümlemesi kullanmak için gereklidir.
* Azure tarafından sağlanan ad çözümleme hizmeti oluşturmak ve kendi DNS sunucularınızı kümelerini yönetmek için gereken kaydetme yüksek oranda kullanılabilir.
* Kendi DNS sunucularıyla birlikte şirket içi ve Azure ana bilgisayar adları çözümlemek için kullanılabilir.
* Ad çözümlemesi rol örnekleri/VMs aynı bulut hizmeti için bir FQDN gerek kalmadan içindeki arasında sağlanır.
* Ad çözümlemesi Resource Manager dağıtım modeli, FQDN için gerek kalmadan kullanan sanal ağları VM'ler arasında sağlanır. Klasik dağıtım modelinde sanal ağlar farklı bulut Hizmetleri adları çözümlerken FQDN gerektirir. 
* Dağıtımlarınızı, en iyi şekilde açıklayan bir ana bilgisayar adları kullanabilirsiniz otomatik olarak oluşturulan adlarıyla çalışma yerine.

**Dikkate alınacak noktalar:**

* Oluşturulan Azure DNS soneki değiştirilemez.
* Kendi kayıtları el ile kaydettirilemedi.
* WINS ve NetBIOS desteklenmez. (Windows Gezgini'nde, VM'lerin göremiyorum.)
* Ana bilgisayar adları DNS uyumlu olması gerekir (yalnızca 0-9, a-z kullanmanız gerekir ve '-' ve başlayamaz veya bitemez bir '-'. RFC 3696 bölümüne 2 bakın.)
* DNS sorgu trafiğinin her VM için kısıtlanır. Bu, çoğu uygulamayı etkileyen döndürmemelidir.  İstek azaltma gözlenir, istemci tarafı önbelleğe alma etkin olduğundan emin olun.  Daha fazla ayrıntı için bkz: [en sağlanan Azure name resolution alma](#Getting-the-most-from-Azure-provided-name-resolution).
* Yalnızca ilk 180 bulut hizmetlerinde VM'ler, her bir sanal ağı Klasik dağıtım modelinde kaydedilir. Bu sanal ağlarda Resource Manager dağıtım modelleri için geçerli değil.

### <a name="getting-the-most-from-azure-provided-name-resolution"></a>En sağlanan Azure name resolution alma
**İstemci tarafı önbelleğe alma:**

Her DNS sorgusu, ağ üzerinden gönderilmesi gerekiyor.  İstemci tarafı önbelleğe alma gecikme süresini azaltmak ve yerel önbelleğinden yinelenen DNS sorgularını çözülerek ağ blips esnekliği geliştirmek yardımcı olur.  DNS kayıtlarını bir yaşam süresi (istemci tarafı önbelleğe alma çoğu durumlar için uygun olacak şekilde kayıt yenilik etkilemeden kayıt mümkün olduğunca uzun bir süre saklamak önbellek sağlayan TTL) içerir.

Windows DNS İstemcisi varsayılan yerleşik bir DNS önbelleği sahiptir.  Varsayılan olarak önbelleğe alma distro'lar eklemeniz gerekmez, bazı Linux biri (olmadığından yerel bir önbellek zaten denetledikten sonra) her bir Linux VM eklenmesi önerilir.

Bir dizi farklı DNS önbelleğe alma paketleri, örneğin dnsmasq vardır, en yaygın distro'lar üzerinde dnsmasq yüklemek için adımlar şunlardır:

* **Ubuntu (kullandığı resolvconf)**:
  * yalnızca dnsmasq paketini ("sudo get apt yükleme dnsmasq") yükleyin.
* **SUSE (kullandığı netconf)**:
  * ("sudo zypper yükleme dnsmasq") dnsmasq paketini yükle 
  * ("systemctl etkinleştir dnsmasq.service") dnsmasq hizmetini etkinleştirme 
  * ("systemctl başlangıç dnsmasq.service") dnsmasq hizmetini başlatın 
  * Düzenle "/ etc/sysconfig/ağ/config" NETCONFIG_DNS_FORWARDER değiştirip = "" "dnsmasq" için
  * Önbellek ayarlamak için resolv.conf ("netconfig güncelleştirme") yerel DNS Çözümleyicisi güncelleştirme
* **OpenLogic (NetworkManager kullanır)**:
  * ("sudo yum yükleme dnsmasq") dnsmasq paketini yükle
  * ("systemctl etkinleştir dnsmasq.service") dnsmasq hizmetini etkinleştirme
  * ("systemctl başlangıç dnsmasq.service") dnsmasq hizmetini başlatın
  * "başına etki alanı adı sunucuları 127.0.0.1;" Ekle "/etc/dhclient-eth0.conf" için
  * Önbellek yerel DNS Çözümleyicisi ayarlamak için ağ hizmeti ("service ağ restart") yeniden başlatın

> [!NOTE]
> 'Dnsmasq' paket yalnızca Linux için kullanılabilir birçok DNS önbellekleri biridir.  Kullanmadan önce lütfen kendi gereksinimlerinize uygunluğuna denetleyin ve başka bir önbellek yüklenir.
> 
> 

**İstemci tarafı yeniden deneme sayısı:**

DNS öncelikle bir UDP protokolüdür.  UDP protokolünü ileti teslimi garanti etmez gibi yeniden deneme mantığı DNS protokolün kendini ele alınır.  Her DNS istemcisi (işletim sistemi) oluşturucuları tercih bağlı olarak farklı yeniden deneme mantığı sergilemesine:

* Windows işletim sistemlerinin 1 saniye ve daha sonra tekrar başka bir 2, 4 ve başka sonra 4 saniye yeniden deneyin. 
* Varsayılan Linux kurulumu yeniden denemelerden sonra 5 saniye.  5 kez 1 ikinci aralıklarla yeniden denemek için bunu değiştirmek için önerilir.  

Geçerli ayarları Linux VM'de 'kat /etc/resolv.conf' ve 'Seçenekler' satırında görünümünü örneğin denetlemek için:

    options timeout:1 attempts:5

Resolv.conf dosyası genellikle otomatik olarak oluşturulan ve düzenlenmemelidir.  'Seçenekler' satır eklemek için belirli adımlar distro göre farklılık gösterir:

* **Ubuntu** (resolvconf kullanır):
  * Seçenekler satırı ekleyin ' / etc/resolveconf/resolv.conf.d/head' 
  * 'resolvconf -u' Çalıştır güncelleştirmek için
* **SUSE** (netconf kullanır):
  * 'timeout:1 denemeleri: 5' NETCONFIG_DNS_RESOLVER_OPTIONS Ekle = "" içinde '/ etc/sysconfig/ağ/config' parametresi 
  * 'netconfig Güncelleştirme' Çalıştır güncelleştirmek için
* **OpenLogic** (NetworkManager kullanır):
  * 'Yankı "seçenekleri timeout:1 denemeleri: 5" ' eklemek ' / etc/NetworkManager/dispatcher.d/11-dhclient' 
  * 'ağ yeniden hizmet' Çalıştır güncelleştirmek için

## <a name="name-resolution-using-your-own-dns-server"></a>Kendi DNS sunucu kullanılarak ad çözümleme
Ad çözümleme gereksinimlerinizi Azure tarafından sağlanan örneğin ne zaman özelliklerini Active Directory etki alanları kullanarak Git veya sanal ağlar (vnet'ler) arasında DNS çözümlemesi gerektiğinde bir dizi durum vardır.  Bu senaryolarını kapsamak üzere Azure kendi DNS sunucularını kullanmak yeteneği sağlar.  

Bir sanal ağ içinde DNS sunucuları, bu sanal ağ içinde ana bilgisayar adları çözümlemek için Azure'nın özyinelemeli Çözümleyicileri için DNS sorgularını iletebilirsiniz.  Örneğin, bir etki alanı denetleyicisi (DC) Azure'da çalışan kendi etki alanları için DNS sorgularını yanıtlamak ve diğer tüm sorgular için Azure iletebilir.  Bu, hem (DC) aracılığıyla şirket içi kaynakları hem de Azure tarafından sağlanan ana bilgisayar adları (aracılığıyla ileticisi) görmek sanal makineleri sağlar.  Azure'nın özyinelemeli çözümleyiciler erişimi 168.63.129.16 sanal IP sağlanır.

DNS iletme de ağlar arası vnet DNS çözümlemesi sağlar ve Azure tarafından sağlanan ana bilgisayar adları çözümlemek şirket içi makinelerinizi sağlar.  Bir sanal makinenin ana bilgisayar adı çözümlemek için DNS sunucusu VM aynı sanal ağında bulunmalıdır ve Azure iletme hostname sorguları yapılandırılmış.  DNS son eki her bir vnet'teki farklı olduğundan, doğru vnet çözümlemesi için DNS sorguları göndermek için koşullu iletme kurallarını kullanabilirsiniz.  Aşağıdaki görüntü iki sanal ağlar ve bu yöntemi kullanarak ağlar arası vnet DNS çözümlemesi yaparken bir şirket içi ağ gösterir.  Bir örnek DNS ileticisi kullanılabilir [Azure hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) ve [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Ağlar arası vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Azure tarafından sağlanan ad çözümlemesi, bir iç DNS soneki kullanırken (*. internal.cloudapp.net) DHCP kullanarak her bir VM için sağlanır.  Ana bilgisayar adı kayıt internal.cloudapp.net bölgede olduğundan bu ana bilgisayar adı çözümlemesi sağlar.  Diğer DNS mimarileri (örneğin, etki alanına katılmış senaryoları) ile uğratan çünkü kendi ad çözümlemesi çözüm kullanırken, IDN'ler soneki VM'ler için sağlanmaz.  Bunun yerine bir çalışmayan yer tutucu (reddog.microsoft.com) sağlar.  

Gerekirse, PowerShell veya API kullanarak iç DNS soneki belirlenebilir:

* Resource Manager dağıtım modellerinde sanal ağlar için soneki aracılığıyla kullanılabilir [ağ arabirim kartı](https://msdn.microsoft.com/library/azure/mt163668.aspx) kaynak veya aracılığıyla [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) cmdlet'i.    
* Klasik dağıtım modellerinde soneki aracılığıyla kullanılabilir [alma dağıtım API'si](https://msdn.microsoft.com/library/azure/ee460804.aspx) çağrısı veya aracılığıyla [Get-AzureVM-Debug](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet.

Sorgular için Azure iletme gereksinimlerinize göre değil, kendi DNS çözümü sağlamanız gerekir.  DNS çözüm gerekir:

* Uygun ana bilgisayar adı, örn aracılığıyla çözümlemesi [DDNS](virtual-networks-name-resolution-ddns.md).  Azure'nın DHCP kiralarını çok uzun ve atma gibi DNS kaydı atmayı devre dışı bırakmanız gerekebilir DDNS kullanıyorsanız Not DNS kayıtlarını erken kaldırabilirsiniz. 
* Dış etki alanı adlarının çözümlemesine izin verecek şekilde uygun özyinelemeli çözümleme sağlar.
* Erişilebilir (TCP ve UDP bağlantı noktası 53), hizmet verdiği istemcilerden olması ve internet erişimine sahip olmalıdır.
* Dış aracıları tarafından teşkil tehditlerin azaltılmasına Internet'ten erişime karşı korunması.

> [!NOTE]
> Azure VM'ler DNS sunucuları olarak kullanırken en iyi performans için IPv6 devre dışı bırakılması gerekir ve bir [örnek düzeyinde ortak IP](virtual-networks-instance-level-public-ip.md) her DNS sunucusuna VM atanmalıdır.  Windows Server, DNS sunucusu olarak kullanmayı tercih ederseniz [bu makalede](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) ek Performans Analizi ve iyileştirmeler sağlar.
> 
> 

### <a name="specifying-dns-servers"></a>DNS sunucularını belirtme
Kendi DNS sunucularını kullanırken, Azure sanal ağ veya ağ arabirimi (Resource Manager) başına birden çok DNS sunucusu belirtin / bulut hizmeti (Klasik) yeteneği sağlar.  Bir bulut hizmeti/ağ arabirimi için belirtilen DNS sunucularına öncelik sanal ağ için belirtilen üzerinden alın.

> [!NOTE]
> Ağ bağlantısı özellikleri, sanal ağ bağdaştırıcısının yerini, DNS sunucusu IP'leri, değil düzenlenmesi gerekir gibi doğrudan Windows VM bunlar hizmeti sırasında silinmesi olarak onarma. 
> 
> 

Resource Manager dağıtım modelini kullanarak, DNS sunucuları Portal, API/şablonları belirtilebilir ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [NIC](https://msdn.microsoft.com/library/azure/mt163668.aspx)) veya PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [NIC](https://msdn.microsoft.com/library/mt619370.aspx)).

Klasik dağıtım modeli kullanılırken, sanal ağın DNS sunucularını portalda belirtilebilir veya [ *ağ yapılandırması* dosya](https://msdn.microsoft.com/library/azure/jj157100).  Bulut Hizmetleri, DNS sunucuları aracılığıyla belirtilen [ *hizmet yapılandırmasını* dosya](https://msdn.microsoft.com/library/azure/ee758710) veya PowerShell ([New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [!NOTE]
> Zaten dağıtılmış bir sanal ağ/sanal makine için DNS ayarlarını değiştirirseniz, etkili olabilmesi için her etkilenen VM değişiklikleri için yeniden başlatmanız gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Resource Manager dağıtım modeli:

* [Bir sanal ağ oluştur veya güncelleştir](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Bir ağ arabirimi kartı güncelle](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [Yeni-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
* [AzureRmNetworkInterface yeni](https://msdn.microsoft.com/library/mt619370.aspx)

Klasik dağıtım modeli:

* [Azure hizmet yapılandırma şeması](https://msdn.microsoft.com/library/azure/ee758710)
* [Sanal ağ yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100)
* [Bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md) 

