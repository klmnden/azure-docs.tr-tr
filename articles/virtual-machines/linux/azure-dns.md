---
title: Linux sanal makinelerinin Azure DNS ad çözümleme seçenekleri
description: Ad çözümleme sağlanan senaryoları Azure Iaas dahil olmak üzere, Linux sanal makineler için DNS hizmetleri, karma dış DNS ve Getir bilgisayarınızı kendi DNS sunucusu.
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: jeconnoc
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
ms.openlocfilehash: d899e037a144892d6d2c843fec08d63a9e3e514a
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>Linux sanal makinelerinin Azure DNS ad çözümlemesi seçenekleri
Azure DNS ad çözümlemesi tek bir sanal ağdaki tüm sanal makineler için varsayılan olarak sağlar. Azure barındıran sanal makinelerde kendi DNS hizmetleri yapılandırarak kendi DNS ad çözümlemesi çözüm uygulayabilirsiniz. Aşağıdaki senaryolarda durumunuz için çalışan bir seçmenize yardımcı olmalıdır.

* [Azure sağlar ad çözümlemesi](#azure-provided-name-resolution)
* [Kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server)

Kullandığınız ad çözümlemesi türünün nasıl sanal makineler ve rol örnekleri birbirleri ile iletişim kurmak gereksinimlerine göre değişir.

Aşağıdaki tabloda senaryoları ve karşılık gelen ad çözümlemesi çözümleri gösterilmektedir:

| **Senaryo** | **Çözüm** | **Suffix** |
| --- | --- | --- |
| Rol örnekleri ya da sanal makineleri aynı sanal ağda arasındaki ad çözümlemesi |[Azure sağlar ad çözümlemesi](#azure-provided-name-resolution) |ana bilgisayar adı veya tam etki alanı adı (FQDN) |
| Rol örnekleri veya sanal makineleri farklı sanal ağlar arasındaki ad çözümlemesi |Sorguları Azure (DNS proxy) tarafından çözümlemesi için sanal ağlar arasında iletecek müşteri tarafından yönetilen DNS sunucuları. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| Şirket içi bilgisayarları ve rol örnekleri ya da sanal makineleri Azure hizmet adlarından çözümleme |Müşteri tarafından yönetilen DNS sunucuları (örneğin, şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya bölge aktarımları kullanarak eşitlenen bir DNS ikincil). Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| Şirket içi bilgisayarlardan Azure ana bilgisayar adı çözümlemesi |Bir müşteri tarafından yönetilen DNS proxy sunucusuna karşılık gelen sanal ağ içinde sorguları iletin. Proxy sunucusunu sorgular için Azure çözümlemesi için iletir. Bkz: [kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| İç IP'ler için ters DNS |[Kendi DNS sunucu kullanılarak ad çözümleme](#name-resolution-using-your-own-dns-server) |yok |

## <a name="name-resolution-that-azure-provides"></a>Azure sağlar ad çözümlemesi
Genel DNS adlarını çözümlenmesi yanı sıra, Azure sanal makineleri ve aynı sanal ağdaki rol örnekleri için dahili ad çözümlemesi sağlar. Azure Resource Manager tabanlı sanal ağlarda DNS soneki sanal ağ arasında tutarlıdır; FQDN gerekli değildir. DNS adları, ağ arabirimi kartlarıyla (NIC) ve sanal makineleri için atanabilir. Azure sağlar ad çözümlemesi herhangi bir yapılandırma gerektirmez rağmen tüm dağıtım senaryoları için uygun seçeneği yukarıdaki tabloda görüldüğü gibi değil.

### <a name="features-and-considerations"></a>Özellikler ve ilgili önemli noktalar
**Özellikler:**

* Herhangi bir yapılandırma sağlayan Azure ad çözümlemesi kullanmak için gereklidir.
* Azure sağlayan ad çözümleme hizmeti yüksek oranda kullanılabilir. Oluşturma ve kendi DNS sunucularınızı kümelerini yönetme gerek yoktur.
* Azure sağlayan ad çözümleme hizmeti, kendi DNS sunucularının yanı sıra şirket içi ve Azure ana bilgisayar adları çözümlemek için kullanılabilir.
* Ad çözümlemesi, FQDN için gerek kalmadan sanal ağlardaki sanal makineler arasında sağlanır.
* Dağıtımlarınızı en iyi şekilde açıklayan bir ana bilgisayar adları kullanabilirsiniz otomatik olarak oluşturulan adlarıyla çalışma yerine.

**Dikkate alınacak noktalar:**

* Azure oluşturduğu DNS soneki değiştirilemez.
* Kendi kayıtları el ile kaydettirilemedi.
* WINS ve NetBIOS desteklenmez.
* Ana bilgisayar adları DNS uyumlu olması gerekir.
    Adları, yalnızca 0-9, a-z, kullanmalıdır ve '-', ve başlayamaz veya bitemez bir '-'. RFC 3696 bölüm 2 bakın.
* DNS sorgu trafiğinin her sanal makine için kısıtlanır. Azaltma uygulamaların çoğu etkisi döndürmemelidir.  İstek azaltma gözlenir, istemci tarafı önbelleğe alma etkin olduğundan emin olun.  Daha fazla bilgi için bkz: [sağlayan Azure name resolution en alma](#getting-the-most-from-name-resolution-that-azure-provides).

### <a name="getting-the-most-from-name-resolution-that-azure-provides"></a>Bu Azure ad çözümlemesi çoğu alma sağlar
**İstemci tarafı önbelleğe alma:**

Bazı DNS sorgularını ağ üzerinden gönderilmez. İstemci tarafı önbelleğe alma gecikme süresini azaltmak ve esnekliği ağ tutarsızlıklar için yinelenen DNS sorgularını yerel önbelleğinden çözülerek geliştirmek yardımcı olur. Bir yaşam süresi (kayıt yenilik etkilemeden kayıt mümkün olduğunca uzun bir süre saklamak önbellek sağlayan TTL), DNS kayıtlarını içerir. Sonuç olarak, istemci tarafı önbelleğe alma çoğu durumlar için uygundur.

Bazı Linux dağıtımları varsayılan önbelleğe alma dahil etmeyin. Olmadığından yerel bir önbellek zaten denetledikten sonra her bir Linux sanal makine için bir önbellek eklemenizi öneririz.

Birkaç farklı DNS dnsmasq gibi paketleri önbelleğe alma kullanılabilir. En yaygın dağıtımları dnsmasq yüklemek için adımları şunlardır:

**Ubuntu (kullandığı resolvconf)**
  * ("Sudo get apt yükleme dnsmasq") dnsmasq paketini yükleyin.

**SUSE (kullandığı netconf)**:
1. ("Sudo zypper yükleme dnsmasq") dnsmasq paketini yükleyin.
2. ("Systemctl etkinleştir dnsmasq.service") dnsmasq hizmetini etkinleştirin.
3. ("Systemctl başlangıç dnsmasq.service") dnsmasq hizmetini başlatın.
4. Düzenle "/ etc/sysconfig/ağ/config" NETCONFIG_DNS_FORWARDER değiştirip = "" "dnsmasq" için.
5. Önbellek ayarlamak için resolv.conf ("netconfig güncelleştirme") yerel DNS Çözümleyicisi güncelleştirin.

**CentOS yanlış Wave yazılımlar (önceki adıyla OpenLogic; NetworkManager kullanır)**
1. ("Sudo yum yükleme dnsmasq") dnsmasq paketini yükleyin.
2. ("Systemctl etkinleştir dnsmasq.service") dnsmasq hizmetini etkinleştirin.
3. ("Systemctl başlangıç dnsmasq.service") dnsmasq hizmetini başlatın.
4. "Başına etki alanı adı sunucuları 127.0.0.1;" Ekle "/etc/dhclient-eth0.conf" için.
5. Önbellek yerel DNS Çözümleyicisi ayarlamak için ağ hizmeti ("service ağ restart") yeniden başlatın

> [!NOTE]
> : 'Dnsmasq' paket yalnızca Linux için kullanılabilir olan birçok DNS önbellekleri biridir. Kullanmadan önce kendi uygunluğu gereksinimleriniz için denetleyin ve başka bir önbellek yüklü.
>
>

**İstemci tarafı yeniden deneme**

DNS öncelikle bir UDP protokolüdür. UDP protokolünü ileti teslimi garanti etmez çünkü DNS protokolü yeniden deneme mantığı işler. Her DNS istemcisi (işletim sistemi) oluşturanın tercih bağlı olarak farklı yeniden deneme mantığı sergilemesine:

* Windows işletim sistemlerinin bir sonra ikinci ve daha sonra tekrar başka sonra iki, dört ve başka bir dört saniye yeniden deneyin.
* Varsayılan Linux kurulumu yeniden denemelerden sonra beş saniyede.  Beş kez bir saniyelik aralıklarla yeniden denemek için bunu değiştirmeniz gerekir.  

Örneğin bir Linux sanal makine, 'kat /etc/resolv.conf' ve 'Seçenekler' satırında görünüm geçerli ayarlarını denetlemek için:

    options timeout:1 attempts:5

Resolv.conf dosyası otomatik olarak oluşturulan ve düzenlenmemelidir. 'Seçenekler' satır ekleyin belirli adımlar göre dağıtım değişir:

**Ubuntu** (resolvconf kullanır)
1. Seçenekler satırı ekleyin ' / etc/resolveconf/resolv.conf.d/head'.
2. 'Resolvconf -u' Çalıştır güncelleştirmek için.

**SUSE** (netconf kullanır)
1. 'Timeout:1 denemeleri: 5' NETCONFIG_DNS_RESOLVER_OPTIONS Ekle = "" içinde '/ etc/sysconfig/ağ/config' parametresi.
2. 'Netconfig Güncelleştirme' Çalıştır güncelleştirmek için.

**CentOS yanlış Wave yazılımı (önceki adıyla OpenLogic) tarafından** (NetworkManager kullanır)
1. Ekle ' RES_OPTIONS "timeout:1 denemeleri: 5" =' için '/ sysconfig/etc/Ağ'.
2. 'Ağ yeniden hizmet' Çalıştır güncelleştirmek için.

## <a name="name-resolution-using-your-own-dns-server"></a>Kendi DNS sunucu kullanılarak ad çözümleme
Ad çözümleme gereksinimlerinizi dışında Azure sağlayan özellikler gidebilir. Örneğin, DNS çözümlemesi sanal ağlar arasında gerektirebilir. Bu senaryo karşılamak üzere kendi DNS sunucularını kullanabilir.  

Bir sanal ağ içinde DNS sunucuları, özyinelemeli çözümleyiciler Azure aynı sanal ağdaki ana bilgisayar adları çözümlemek için DNS sorgularını iletebilirsiniz. Örneğin, Azure'da çalışan bir DNS sunucusu kendi DNS bölge dosyaları için DNS sorgularını yanıtlamak ve diğer tüm sorguları Azure'a iletin. Sanal makineler, her iki girişlerinizi (ileticisi) Azure sağlayan bir ana bilgisayar adları ve bölge dosyaları görmek bu işlevi etkinleştirir. Özyinelemeli çözümleyiciler Azure erişimi 168.63.129.16 sanal IP sağlanır.

DNS iletme aynı zamanda sanal ağlar arasında DNS çözümlemesi sağlar ve Azure sağlayan bir ana bilgisayar adları çözümlemek şirket içi makinelerinizi sağlar. Bir sanal makinenin ana bilgisayar adı çözümlemek için DNS sunucusu sanal makinesi aynı sanal ağında bulunmalıdır ve Azure iletme hostname sorguları yapılandırılması. DNS son eki her sanal ağ farklı olduğundan, doğru sanal ağı çözümlemesi için DNS sorguları göndermek için koşullu iletme kurallarını kullanabilirsiniz. Aşağıdaki görüntü iki sanal ağ ve DNS çözümlemesi bu yöntemi kullanarak sanal ağlar arasında bulunurken bir şirket içi ağ göstermektedir:

![Sanal ağlar arasında DNS çözümlemesi](./media/azure-dns/inter-vnet-dns.png)

Azure sağlar ad çözümlemesi kullandığınızda, iç DNS soneki DHCP kullanarak her bir sanal makine için sağlanır. Kendi ad çözümlemesi çözümünü kullandığınızda, diğer DNS mimarileri ile soneki uğratan çünkü bu soneki sanal makinelere sağlanmaz. FQDN DEĞERİNE göre makineler başvurun veya sanal makinelerde soneki yapılandırmak için soneki belirlemek için PowerShell veya API kullanabilirsiniz:

* Azure Resource Manager tarafından yönetilen sanal ağlar için soneki aracılığıyla kullanılabilir [ağ arabirim kartı](https://msdn.microsoft.com/library/azure/mt163668.aspx) kaynak. De çalıştırabilirsiniz `azure network public-ip show <resource group> <pip name>` NIC FQDN'sini içerir, genel IP ayrıntılarını görüntülemek için komutu

Sorgular için Azure iletme gereksinimlerinize göre değil, kendi DNS çözümü sağlamanız gerekir.  DNS çözüm gerekir:

* Uygun ana bilgisayar adı çözümlemesi, örneğin aracılığıyla sağlamak [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md). DDNS kullanırsanız, DNS kaydı atmayı devre dışı bırakmanız gerekebilir. DHCP kiralarını Azure çok uzun ve atma DNS kayıtlarını erken kaldırabilirsiniz.
* Dış etki alanı adlarının çözümlemesine izin verecek şekilde uygun özyinelemeli çözümleme sağlar.
* Erişilebilir (TCP ve UDP bağlantı noktası 53), hizmet verdiği istemcilerden olması ve Internet erişimine sahip olmalıdır.
* Dış aracıları tarafından teşkil tehditleri azaltmak için Internet'ten erişime karşı korunması.

> [!NOTE]
> En iyi performans için sanal makineleri Azure DNS sunucularının kullandığınızda, IPv6 devre dışı bırakın ve ata bir [örnek düzeyinde ortak IP](../../virtual-network/virtual-networks-instance-level-public-ip.md) her DNS sunucusu sanal makinesi için.  
>
>
