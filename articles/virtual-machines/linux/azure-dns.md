---
title: Azure'da Linux sanal makineler için DNS ad çözümleme seçenekleri
description: Ad çözümleme sağlanan senaryoları da dahil olmak üzere Azure Iaas, Linux sanal makineler için DNS hizmetleri, karma dış DNS ve Getir bilgisayarınızı kendi DNS sunucusu.
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
ms.openlocfilehash: ae8315b2a484cddc500b5c2dd02a019cb4f46d8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127098"
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>Azure'da Linux sanal makineleri için DNS ad çözümleme seçenekleri
Azure DNS ad çözümlemesi, tek bir sanal ağdaki tüm sanal makineler için varsayılan olarak sağlar. Barındıran Azure üzerindeki sanal makinelerinize kendi DNS hizmetleri yapılandırarak kendi DNS ad çözümlemesi çözüm uygulayabilirsiniz. Aşağıdaki senaryolar sizin durumunuz için çalışan bir seçmenize yardımcı olacaktır.

* [Azure sağlayan ad çözümlemesi](#name-resolution-that-azure-provides)
* [Kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-using-your-own-dns-server)

Kullandığınız ad çözümlemesi türünü nasıl sanal makineler ve rol örnekleri birbirleri ile iletişim kurmak gereken üzerinde bağlıdır.

Aşağıdaki tabloda, senaryolar ve karşılık gelen ad çözümlemesi çözümler gösterilmektedir:

| **Senaryo** | **Çözüm** | **Son eki** |
| --- | --- | --- |
| Rol örnekleri ve aynı sanal ağdaki sanal makineler arasındaki ad çözümlemesi |Azure sağlayan ad çözümlemesi |ana bilgisayar adı veya tam etki alanı adı (FQDN) |
| Rol örnekleri ve farklı sanal ağlarda bulunan sanal makineler arasındaki ad çözümlemesi |Sorguları Azure (DNS proxy) tarafından çözümlemesi için sanal ağlar arasında iletecek müşteri tarafından yönetilen DNS sunucuları. Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| Şirket içi bilgisayarları ve rol örnekleri veya azure'da sanal makineler hizmet adlarını çözümleme |Müşteri tarafından yönetilen DNS sunucuları (örneğin, şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya DNS ikincil bölge aktarımlarını kullanarak eşitlenen). Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| Şirket içi bilgisayarlardan Azure konak adı çözümlemesi |Bir müşteri tarafından yönetilen DNS Ara sunucuya karşılık gelen sanal ağ içinde sorguları iletin. Proxy sunucusunu sorgular çözümlemesi için Azure'a iletir. Bkz: [kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-using-your-own-dns-server). |Yalnızca FQDN |
| İç IP'ler için ters DNS |[Kendi DNS sunucunuzu kullanarak ad çözümlemesi](#name-resolution-using-your-own-dns-server) |yok |

## <a name="name-resolution-that-azure-provides"></a>Azure sağlayan ad çözümlemesi
Genel DNS adlarını çözümlemesini yanı sıra, Azure sanal makineler ve aynı sanal ağdaki rol örnekleri için iç ad çözümleme sağlar. Üzerinde Azure Resource Manager tabanlı sanal ağlar DNS son eki sanal ağ arasında tutarlıdır; FQDN gerekli değildir. Ağ arabirim kartı (NIC) ve sanal makineler için DNS adları atanabilir. Herhangi bir yapılandırma sağlayan Azure ad çözümlemesi gerekli olmasa da tüm dağıtım senaryoları için uygun seçim önceki tabloda görüldüğü gibi değil.

### <a name="features-and-considerations"></a>Özellikler ve dikkat edilmesi gerekenler
**Özellikler:**

* Herhangi bir yapılandırma sağlayan Azure ad çözümlemesi kullanmak için gereklidir.
* Azure sağlayan ad çözümlemesi hizmetini yüksek oranda kullanılabilir. Kendi DNS sunucularınızı, küme oluşturma ve yönetme gerek yoktur.
* Azure sağlayan ad çözümlemesi hizmetini hem şirket içi hem de Azure ana bilgisayar adlarını çözümlemek için kendi DNS sunucularınızı birlikte kullanılabilir.
* Ad çözümlemesi için FQDN gerek kalmadan sanal ağlarda bulunan sanal makineler arasında sağlanır.
* Kullanabileceğiniz dağıtımlarınızı en iyi şekilde açıklayan bir ana bilgisayar adları yerine otomatik olarak oluşturulan adları ile çalışma.

**Dikkate alınacak noktalar:**

* Azure'ı oluşturan DNS son eki değiştirilemez.
* El ile kendi kayıtlarınızı kaydedilemiyor.
* WINS ve NetBIOS desteklenmez.
* Ana bilgisayar adları için DNS ile uyumlu olması gerekir.
    Adları, yalnızca 0-9, a-z, kullanmalıdır ve '-', ve başında veya sonunda bir '-'. RFC 3696 bölüm 2 bakın.
* DNS sorgu trafiği her sanal makine için kısıtlanır. Azaltma, çoğu uygulama etkisi olmaması gerekir.  İstek azaltma gözlemlendiğinde istemci tarafı önbelleğe alma etkin olduğundan emin olun.  Daha fazla bilgi için [sağlayan Azure ad resolution en alma](#getting-the-most-from-name-resolution-that-azure-provides).

### <a name="getting-the-most-from-name-resolution-that-azure-provides"></a>Bu Azure ad çözümlemesi en iyi Başlarken sağlar
**İstemci tarafı önbelleğe alma:**

Bazı DNS sorguları, ağ üzerinden gönderilmez. İstemci tarafı önbelleğe alma, gecikme süresini azaltın ve yerel önbelleğinden alınan yinelenen DNS sorguları çözerek ağ tutarsızlıklar dayanıklılık artırılmasına yardımcı olur. Bir yaşam süresi (kayıt güncellik etkilemeden kayıt için olabildiğince uzun süre saklamak önbellek sağlayan TTL), DNS kayıtları içerir. Sonuç olarak, istemci tarafı önbelleğe alma çoğu durumlar için uygundur.

Bazı Linux dağıtımlarında, varsayılan olarak önbelleğe alma içermez. Olmadığından yerel önbellek zaten denetledikten sonra her bir Linux sanal makine için bir önbellek eklemenizi öneririz.

Birkaç farklı DNS gibi dnsmasq, paketleri önbelleğe alma kullanılabilir. En yaygın dağıtımlarında dnsmasq yüklemek için adımları şunlardır:

**Ubuntu (uses resolvconf)**
  * ("Sudo apt-get install dnsmasq") dnsmasq paketini yükleyin.

**SUSE (kullandığı netconf)** :
1. ("Sudo zypper yükleme dnsmasq") dnsmasq paketini yükleyin.
2. Dnsmasq hizmeti ("systemctl etkinleştir dnsmasq.service") etkinleştirin.
3. ("Systemctl başlangıç dnsmasq.service") dnsmasq hizmetini başlatın.
4. Düzenle "/ etc/sysconfig/ağ/config" NETCONFIG_DNS_FORWARDER değiştirip = "" "dnsmasq" için.
5. Resolv.conf ("netconfig güncelleştirme") önbellek ayarlamak için yerel DNS Çözümleyicisi güncelleştirin.

**CentOS Rogue Wave Software (eski adıyla OpenLogic; NetworkManager kullanır)**
1. ("Sudo yum yüklemesi dnsmasq") dnsmasq paketini yükleyin.
2. Dnsmasq hizmeti ("systemctl etkinleştir dnsmasq.service") etkinleştirin.
3. ("Systemctl başlangıç dnsmasq.service") dnsmasq hizmetini başlatın.
4. "Etki alanı adı sunucuları 127.0.0.1; önüne ekleyin" Ekle "/etc/dhclient-eth0.conf" için.
5. Önbellek yerel DNS Çözümleyicisi ayarlamak için ağ hizmeti ("hizmet ağı restart") yeniden başlatın

> [!NOTE]
> : 'Dnsmasq' yalnızca bir Linux için kullanılabilen birçok DNS Önbellek paketidir. Kullanmadan önce kendi gereksinimlerinize uygunluğu denetleyin ve başka bir önbellek yüklü.
>
>

**İstemci tarafı yeniden deneme**

DNS öncelikle bir UDP protokolüdür. DNS protokolü, UDP protokolünü ileti teslimi garanti etmez olduğundan, yeniden deneme mantığı işler. Her DNS istemcisi (işletim sistemi) farklı yeniden deneme mantığı oluşturan kişinin tercih bağlı olarak ek:

* Windows işletim sistemlerinin bir sonraki ikinci ve daha sonra tekrar diğerinden sonra iki, dört ve başka bir dört saniye yeniden deneyin.
* Varsayılan Linux kurulumu, beş saniyeden sonra deneme.  Bu beş kez bir saniyelik aralıklarla yeniden deneyecek şekilde değiştirmeniz gerekir.  

Örneğin bir Linux sanal makine, 'cat /etc/resolv.conf' ve 'Seçenekler' satırına bakın geçerli ayarlarını denetlemek için:

    options timeout:1 attempts:5

Resolv.conf dosyasını otomatik olarak oluşturuldu ve düzenlenmemelidir. 'Seçenekler' Satır Ekle belirli adımlar, dağıtım göre farklılık gösterir:

**Ubuntu** (resolvconf kullanır)
1. Seçenekleri satırı ' / etc/resolveconf/resolv.conf.d/head'.
2. 'Resolvconf -u' çalıştırmak güncelleştirilecek.

**SUSE** (netconf kullanır)
1. 'Timeout:1 girişimi: 5' eklemek için NETCONFIG_DNS_RESOLVER_OPTIONS = "" da '/ etc/sysconfig/ağ/config' parametresi.
2. ' Netconfig Güncelleştirme ' güncelleştirilecek.

**CentOS Rogue Wave Software (eski adıyla OpenLogic)** (NetworkManager kullanır)
1. Ekle ' RES_OPTIONS "timeout:1 girişimi: 5" =' için '/ etc/sysconfig/Ağ'.
2. 'Ağ yeniden servis' çalıştırmak güncelleştirilecek.

## <a name="name-resolution-using-your-own-dns-server"></a>Kendi DNS sunucunuzu kullanarak ad çözümlemesi
Ad çözümleme ihtiyaçlarınızı dışında Azure sağlayan özellikler gidebilir. Örneğin, sanal ağlar arasında DNS çözümlemesi gerektirebilir. Bu senaryo karşılamak için kendi DNS sunucularınızı kullanabilirsiniz.  

Bir sanal ağdaki DNS sunucuları aynı sanal ağdaki ana bilgisayar adlarını çözümlemek için Azure'nın yinelemeli Çözümleyicileri için DNS sorgularını iletebilir. Örneğin, Azure'da çalışan bir DNS sunucusu kendi DNS bölge dosyalarının için DNS sorgularını yanıtlamak ve diğer tüm sorguları Azure'a iletmek. Sanal makineler, bölge dosyalarının ve Azure'ı (ileticisi) sağlayan bir ana bilgisayar adları, her iki girişlerini görmek bu işlevi etkinleştirir. Azure'nın yinelemeli Çözümleyicileri erişim sanal IP'SİNDEN (168.63.129.16) sağlanır.

Ayrıca DNS iletme sanal ağlar arasında DNS çözümlemesine olanak tanır ve şirket içi makinelerinizin Azure sağlayan bir ana bilgisayar adlarını çözümlemek etkinleştirir. Bir sanal makinenin konak adı çözümlemek için DNS sunucusu sanal makine aynı sanal ağda bulunması gerekir ve azure'a ileriye doğru ana bilgisayar adı sorgularını yapılandırılmış olması. DNS son eki her bir sanal ağ farklı olduğu için çözüm için doğru sanal ağı için DNS sorguları göndermek için koşullu iletme kurallarını kullanabilirsiniz. Aşağıdaki görüntüde, iki sanal ağ ve DNS çözümlemesi bu yöntemi kullanarak sanal ağlar arasında yapılması şirket içi ağı gösterilmektedir:

![Sanal ağlar arasında DNS çözümlemesi](./media/azure-dns/inter-vnet-dns.png)

İç DNS soneki sağlayan Azure ad çözümlemesi kullandığınızda, her bir sanal makine için DHCP kullanarak sağlanır. Kendi ad çözümlemesi çözümünü kullandığınızda, diğer DNS mimarilerin soneki uğratan çünkü bu son ek sanal makineler için sağlanmaz. FQDN DEĞERİNE göre makineler bakın veya sonek üzerindeki sanal makinelerinize yapılandırmak için PowerShell veya API soneki belirlemek için kullanabilirsiniz:

* Azure Resource Manager tarafından yönetilen sanal ağlar için soneki aracılığıyla kullanılabilir [ağ arabirim kartı](https://msdn.microsoft.com/library/azure/mt163668.aspx) kaynak. Ayrıca çalıştırabileceğiniz `azure network public-ip show <resource group> <pip name>` NIC FQDN'sini içeren genel IP, ayrıntılarını görüntülemek için komut

Azure'a sorguları iletme ihtiyaçlarınıza uygun değil, kendi DNS çözüm vermeniz gerekir.  DNS çözümünüzü gerekir:

* Uygun bir ana bilgisayar adı çözümlemesi, örneğin aracılığıyla sağlamak [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md). DDNS kullanırsanız, DNS kaydı atmayı devre dışı bırakmanız gerekebilir. DHCP kiralarını Azure'un çok uzun ve atma DNS kayıtlarını beklenenden önce kaldırabilir.
* Dış etki alanı adlarının çözümlenmesini izin vermek için uygun bir özyinelemeli çözümleme sağlar.
* Erişilebilir (TCP ve UDP bağlantı noktası 53) sunduğu istemcilerden ve Internet erişimine sahip olmalıdır.
* Dış aracıları tarafından teşkil tehditleri azaltmak için Internet'ten erişime karşı korunması.

> [!NOTE]
> En iyi performans için Azure DNS sunucuları sanal makineleri kullanırken, IPv6 devre dışı bırakın ve atama bir [örnek düzeyi genel IP](../../virtual-network/virtual-networks-instance-level-public-ip.md) her bir DNS sunucusu sanal makine için.  
>
>
