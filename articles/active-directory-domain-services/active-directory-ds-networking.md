---
title: 'Azure AD etki alanı Hizmetleri: Ağ yönergeleri | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri için ağ konuları
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: maheshu
ms.openlocfilehash: a56413490decc928ff2643213084155ae469871c
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Azure AD etki alanı Hizmetleri için ağ konuları
## <a name="how-to-select-an-azure-virtual-network"></a>Nasıl bir Azure sanal ağı seçin
Aşağıdaki yönergeler Azure AD etki alanı Hizmetleri ile kullanmak için bir sanal ağ seçmenize yardımcı.

### <a name="type-of-azure-virtual-network"></a>Azure sanal ağ türü
* **Kaynak Yöneticisi sanal ağlar**: Azure AD etki alanı Hizmetleri, Azure Resource Manager kullanılarak oluşturulan sanal ağlarda etkinleştirilebilir.
* Klasik Azure sanal ağı Azure AD Etki Alanı Hizmetleri'nde etkinleştiremezsiniz.
* Diğer sanal ağlar Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağa bağlanabilir. Daha fazla bilgi için bkz: [ağ bağlantınızı](active-directory-ds-networking.md#network-connectivity) bölümü.

### <a name="azure-region-for-the-virtual-network"></a>Sanal ağ için Azure bölgesi
* Yönetilen etki alanı sanal ağ aynı Azure bölgesinde dağıtılır, Azure AD etki alanı Hizmetleri hizmeti etkinleştirin seçin.
* Azure AD etki alanı Hizmetleri tarafından desteklenen bir Azure bölgesindeki bir sanal ağı seçin.
* Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.

### <a name="requirements-for-the-virtual-network"></a>Sanal ağ gereksinimleri
* **Azure, iş yükleri için yakınlık**: şu anda barındıran/Azure AD Etki Alanı Hizmetleri'ne erişmesi gereken sanal makineleri barındıracak sanal ağı seçin. İş yüklerinizi yönetilen etki alanı farklı bir sanal ağa dağıtılırsa, sanal ağlara bağlanma seçebilirsiniz.
* **DNS sunucuları özel/Getir kendi**: sanal ağ için yapılandırılmış hiçbir özel DNS sunucusunun olduğundan emin olun. Özel bir DNS sunucusu örneği, sanal ağ dağıtmış olan bir Windows Server VM üzerinde çalışan Windows Server DNS örneğidir. Azure AD etki alanı Hizmetleri değil tümleştirin hiçbir özel DNS sunucularıyla sanal ağ içinde dağıtılabilir.
* **Aynı etki alanı adı mevcut etki alanlarıyla**: Bu sanal ağda kullanılabilir aynı etki alanı adına sahip mevcut bir etki alanına sahip değil emin olun. Örneğin, seçilen sanal ağ üzerinde zaten "contoso.com" adında bir etki alanınız olduğunu varsayın. Daha sonra bir Azure AD etki alanı Hizmetleri yönetilen etki alanı (yani "contoso.com") aynı etki alanı adına bu sanal ağ ile etkinleştirmeyi deneyin. Azure AD Etki Alanı Hizmetleri'ni etkinleştirmeye çalışırken bir hatayla karşılaşırsınız. Bu sanal ağ üzerinde etki alanı adı için ad çakışmaları nedeniyle bu hatasıdır. Bu durumda, Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanınızı ayarlamak için farklı bir ad kullanmanız gerekir. Alternatif olarak, var olan etki alanının sağlanmasını kaldırıp Azure AD Etki Alanı Hizmetleri'ni etkinleştirme işlemiyle devam edebilirsiniz.

> [!WARNING]
> Hizmeti etkinleştirdikten sonra etki alanı Hizmetleri'ni farklı bir sanal ağa taşıyamazsınız.
>
>


## <a name="guidelines-for-choosing-a-subnet"></a>Bir alt ağı seçmeye yönelik yönergeleri

![Önerilen alt ağ tasarımı](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

* Azure AD etki alanı Hizmetleri dağıtmak bir **ayrı ayrılmış bir alt ağ** Azure sanal ağınızın içinde.
* Nsg'ler yönetilen etki alanınız için ayrılmış bir alt ağ için geçerli değildir. Ayrılmış bir alt ağ için Nsg'ler uygulamalısınız olmanız **değil hizmeti için gereken bağlantı noktalarını engellemek ve etki alanınızı yönetmek**.
* Aşırı yönetilen etki alanınız için ayrılmış bir alt ağ içinde kullanılabilir IP adresi sayısını kısıtlamaz. Bu kısıtlama, iki etki alanı denetleyicileri, yönetilen etki alanınız için kullanılabilir bulunmasını hizmet önler.
* **Ağ geçidi alt ağı Azure AD Etki Alanı Hizmetleri'nde etkinleştirmeyin** sanal ağınızın.
* Yönetilen etki alanınızı etkin olduğu alt ağdan giden erişim engellemez.

> [!WARNING]
> İlişkilendirdiğinizde, bir NSG bir alt ağ içinde Azure AD etki alanı Hizmetleri ile etkinleştirildiğinde, Microsoft'un hizmet ve etki alanını yönetme özelliğini bozabilir. Ayrıca, Azure AD kiracınız, yönetilen etki alanınız arasında eşitleme bozulur. **SLA, burada bir NSG engelleyen Azure AD etki alanı Hizmetleri güncelleştirme ve etki alanınızı yönetme uygulanmış olan dağıtımlar için geçerli değildir.**
>
>

## <a name="ports-required-for-azure-ad-domain-services"></a>Azure AD etki alanı Hizmetleri için gereken bağlantı noktaları
Aşağıdaki bağlantı noktalarını hizmetine Azure AD etki alanı Hizmetleri için gerekli olan ve yönetilen etki alanınızı sürdürün. Bu bağlantı noktaları için yönetilen etki alanınız etkinleştirdiğiniz alt engellenmez emin olun.

| Bağlantı noktası numarası | Gerekli mi? | Amaç |
| --- | --- | --- |
| 443 | Zorunlu |Azure AD kiracınıza ile eşitleme |
| 5986 | Zorunlu | Etki alanınızın Yönetimi |
| 3389 | İsteğe bağlı | Etki alanınızın Yönetimi |
| 636 | İsteğe bağlı | Yönetilen etki alanınız için güvenli LDAP (LDAPS) erişim |

**Bağlantı noktası 443 (Azure AD ile eşitleme)**
* Azure AD dizininizi yönetilen etki alanı ile eşitlemek için kullanılır.
* Bu bağlantı noktası, nsg'deki erişmesine izin vermek için zorunludur. Bu bağlantı noktası erişimi olmadan, yönetilen etki alanınızı Azure AD dizini ile eşitlenmiş değil. Kullanıcılar yönetilen etki alanınıza parolalarını yapılan değişiklikler eşitlenmez oturum açmak mümkün olmayabilir.
* Azure IP adresi aralığına ait IP adresleri için bu bağlantı noktasına gelen erişimi kısıtlayabilirsiniz. Azure IP adresi aralığı kuralda gösterilen PowerShell aralığından daha farklı bir aralık olduğuna dikkat edin.

**Bağlantı noktası 5986 (PowerShell uzaktan iletişimi)**
* PowerShell uzaktan iletişimi yönetilen etki alanınızda kullanarak yönetim görevlerini gerçekleştirmek için kullanılır.
* Bu bağlantı noktası, nsg'deki üzerinden erişime izin vermek için zorunludur. Bu bağlantı noktası erişimi olmadan, yönetilen etki alanınızı güncelleştirilmiş, yapılandırılmış, yedeklenen veya izlenen olamaz.
* Aşağıdaki kaynak IP adresleri için bu bağlantı noktasına gelen erişimi kısıtlayabilirsiniz: 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161
* Yönetilen etki alanınız için etki alanı denetleyicileri genellikle bu bağlantı noktasında dinleme değil. Yalnızca bir yönetim veya bakım işlemi yönetilen etki alanı için gerçekleştirilmesi gerektiğinde hizmet yönetilen etki alanı denetleyicilerinde Bu bağlantı noktası açar. İşlemi tamamlanır tamamlanmaz hizmet yönetilen etki alanı denetleyicilerinde Bu bağlantı noktasını kapatır.

**Bağlantı noktası 3389 (Uzak Masaüstü)**
* Yönetilen etki alanınız için etki alanı denetleyicilerine Uzak Masaüstü bağlantıları için kullanılır.
* Bu bağlantı noktası, NSG üzerinden açma isteğe bağlıdır.
* Bu bağlantı noktası da büyük ölçüde yönetilen etki alanınızda devre dışı kalır. PowerShell uzaktan iletişimi kullanarak yönetim ve izleme görevlerini gerçekleştirilen beri bu düzenek sürekli olarak kullanılmaz. Bu bağlantı noktası Microsoft Gelişmiş sorun giderme adımları için yönetilen etki alanınız uzaktan bağlanmak için gereken nadir durumlarda kullanılır. Sorun giderme işlemi tamamlandıktan hemen sonra bağlantı noktası kapalı.

**Bağlantı noktası 636 (güvenli LDAP)**
* İnternet üzerinden yönetilen etki alanınız için güvenli LDAP erişimi etkinleştirmek için kullanılır.
* Bu bağlantı noktası, NSG üzerinden açma isteğe bağlıdır. Yalnızca etkin Internet üzerinden güvenli LDAP erişimi varsa, bağlantı noktasını açın.
* Bu bağlantı noktası üzerinden güvenli LDAP bağlanmak beklediğiniz kaynak IP adresleri için gelen erişimi kısıtlayabilirsiniz.

**Giden erişim** AAD etki alanı hizmetleri yönetmek, yedekleme ve yönetilen etki alanınızı izlemek için çeşitli diğer Azure hizmetleriyle giden erişimi olması gerekir. Yönetilen etki alanınızı etkin olduğu özel alt ağdan giden erişim engellemez.


## <a name="network-security-groups"></a>Ağ Güvenlik Grupları
A [ağ güvenlik grubu (NSG)](../virtual-network/virtual-networks-nsg.md) izin veren veya bir sanal ağ üzerindeki VM örneklerinize ağ trafiğinin reddeden erişim denetimi listesi (ACL) kurallarının bir listesini içerir. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur. Ayrıca, tekil bir VM trafik kısıtlanabilir başka bir NSG doğrudan bu VM ilişkilendirerek.

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Azure AD etki alanı Hizmetleri ile sanal ağlar için örnek NSG
Aşağıdaki tabloda, örnek bir Azure AD etki alanı Hizmetleri yönetilen etki alanına sahip bir sanal ağ için yapılandırabileceğiniz NSG gösterilmektedir. Bu kural, yönetilen etki alanı kalır emin olmak için gerekli bağlantı noktaları üzerinden gelen trafiğe düzeltme eki, güncelleştirilmiş ve Microsoft tarafından izlenen sağlar. Varsayılan 'DenyAll' kural, internet'ten diğer gelen trafik için geçerlidir.

Ayrıca, NSG Internet üzerinden güvenli LDAP erişim kilitlemek nasıl gösterilmektedir. Güvenli LDAP erişim yönetilen etki alanınıza internet üzerinden değil etkinleştirdiyseniz, bu kuralı atla. NSG gelen LDAPS erişim TCP bağlantı noktası IP adreslerinin üzerinden 636 belirtilen kümesinden yalnızca izin veren kurallar kümesini içerir. Belirtilen IP adreslerini Internet üzerinden LDAPS erişime izin verecek şekilde NSG kuralı DenyAll NSG kural daha yüksek önceliğe sahip.

![Örnek internet üzerinden LDAPS erişimi güvenli hale getirmek için NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

**Daha fazla bilgi** - [bir ağ güvenlik grubu oluşturun](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Ağ bağlantısı
Azure AD etki alanı Hizmetleri yönetilen etki alanı yalnızca tek bir sanal ağı Azure içinde etkinleştirilebilir.

### <a name="scenarios-for-connecting-azure-networks"></a>Azure ağları bağlama senaryoları
Aşağıdaki dağıtım senaryoları hiçbirini yönetilen etki alanınızı kullanmak için Azure sanal ağlara bağlanabilir:

#### <a name="use-the-managed-domain-in-more-than-one-azure-virtual-network"></a>Yönetilen etki alanında birden fazla Azure sanal ağı kullan
Diğer Azure sanal ağlar Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz Azure sanal ağa bağlanabilir. Bu VPN/VNet eşleme bağlantısı diğer sanal ağlara dağıtılabilir, iş yükleri ile yönetilen etki alanı kullanmanıza olanak sağlar.

![Klasik sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Resource Manager tabanlı bir sanal ağdaki yönetilen etki alanı kullanın
Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz Azure Klasik sanal ağı için Resource Manager tabanlı bir sanal ağa bağlanabilir. Bu bağlantı, Resource Manager tabanlı sanal ağda dağıtılmış, iş yükleri ile yönetilen etki alanı kullanmanıza olanak sağlar.

![Klasik sanal ağ bağlantısı için kaynak yöneticisi](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Ağ bağlantısı seçenekleri
* **VNet-VNet bağlantıları kullanarak sanal ağ eşlemesi**: sanal ağ eşlemesi mekanizmasıdır aynı bölgedeki iki sanal ağı Azure omurga ağı aracılığıyla birbirine bağlayan bir. Eşleme yapıldıktan sonra, iki sanal ağ tüm bağlantılarda tek bir sanal ağ gibi görünür. Bunlar ayrı kaynaklar olarak yönetilmeye devam eder, ancak bu sanal ağlardaki sanal makineler özel IP adresleri kullanarak birbirleriyle doğrudan iletişim kurabilir.

    ![Eşleme kullanarak sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Daha fazla bilgi - sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md)

* **Siteden siteye VPN bağlantıları kullanarak VNet-VNet bağlantıları**: başka bir sanal ağ (VNet-VNet) sanal ağa bağlanma benzer bir şirket içi site konumuna sanal bir ağa bağlanma. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır.

    ![VPN ağ geçidi kullanarak sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Daha fazla bilgi - VPN ağ geçidi kullanarak sanal ağlara bağlanabilir](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

<br>

## <a name="related-content"></a>İlgili İçerik
* [Azure sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md)
* [Klasik dağıtım modeli için VNet-VNet bağlantı yapılandırma](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Azure ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md)
* [Bir ağ güvenlik grubu oluşturun](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
