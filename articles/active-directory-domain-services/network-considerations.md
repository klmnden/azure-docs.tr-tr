---
title: 'Azure AD etki alanı Hizmetleri: Ağ yönergeleri | Microsoft Docs'
description: Azure Active Directory Domain Services ilgili ağ konuları
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2010
ms.author: mstephen
ms.openlocfilehash: 1f21d71bba01eb4bec24dbb558a126ecbbd78bbf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246952"
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Azure AD Domain Services ilgili ağ konuları
## <a name="how-to-select-an-azure-virtual-network"></a>Nasıl bir Azure sanal ağı seçin
Aşağıdaki yönergeler, Azure AD Domain Services ile kullanmak için bir sanal ağ seçin yardımcı.

### <a name="type-of-azure-virtual-network"></a>Azure sanal ağ türü
* **Resource Manager sanal ağları**: Azure AD Domain Services, Azure Resource Manager kullanılarak oluşturulan sanal ağlarda etkinleştirilebilir.
* Azure AD Domain Services klasik bir Azure sanal ağında etkinleştirilemiyor.
* Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağa, diğer sanal ağlara bağlanabilirsiniz. Daha fazla bilgi için [ağ bağlantısı](network-considerations.md#network-connectivity) bölümü.

### <a name="azure-region-for-the-virtual-network"></a>Sanal ağ için Azure bölgesi
* Yönetilen etki alanı sanal ağ aynı Azure bölgesinde dağıtılır, Azure AD Domain Services hizmetinde etkinleştirmek seçin.
* Azure AD Domain Services tarafından desteklenen bir Azure bölgesindeki bir sanal ağ'ı seçin.
* Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.

### <a name="requirements-for-the-virtual-network"></a>Sanal ağ gereksinimleri
* **Azure iş yükleriniz için yakınlık**: Azure AD Etki Alanı Hizmetleri'ne erişmesi gereken sanal makineleri şu anda barındıran/barındıracak olan sanal ağı seçin. Yönetilen etki alanı farklı bir sanal ağa iş yüklerinizi dağıtılırsa, sanal ağları bağlamak de tercih edebilirsiniz.
* **DNS sunucuları özel/Getir kendi**: Sanal ağ için yapılandırılmış hiçbir özel DNS sunucusu olduğundan emin olun. Özel bir DNS sunucusu örneği, Windows Server sanal ağ içinde dağıttığınız Windows Server VM üzerinde çalışan DNS örneğidir. Azure AD Domain Services, sanal ağ içinde dağıtılan özel DNS sunucuları ile tümleşmez.
* **Aynı etki alanı adını mevcut etki alanlarıyla**: Bu sanal ağda bulunan aynı etki alanı adına sahip mevcut bir etki alanına sahip değil emin olun. Örneğin, seçilen sanal ağ üzerinde zaten "contoso.com" adında bir etki alanınız olduğunu varsayın. Daha sonra bir Azure AD Domain Services yönetilen etki alanı (yani "contoso.com") aynı etki alanı adı, sanal ağ ile etkinleştirmeyi deneyin. Azure AD Domain Services'ı etkinleştirmeyi denediğinizde bir hatayla karşılaşırsınız. Bu hata, bu sanal ağ üzerinde etki alanı adı için ad çakışmalarını kaynaklanır. Bu durumda, Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanınızı ayarlamak için farklı bir ad kullanmanız gerekir. Alternatif olarak, var olan etki alanının sağlanmasını kaldırıp Azure AD Etki Alanı Hizmetleri'ni etkinleştirme işlemiyle devam edebilirsiniz.

> [!WARNING]
> Hizmetini etkinleştirdikten sonra etki alanı Hizmetleri'ni farklı bir sanal ağa taşıyamazsınız.
>
>


## <a name="guidelines-for-choosing-a-subnet"></a>Bir alt ağı seçmeye yönelik yönergeleri

![Önerilen alt ağ tasarımı](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

* Azure AD Domain Services'ı dağıtma bir **ayrı ayrılmış alt ağında** Azure sanal ağınız içinde.
* Nsg'ler, yönetilen etki alanınız için ayrılmış bir alt ağ için geçerli değildir. Ayrılmış bir alt ağ için Nsg geçerli emin olmanız **değil servis için gereken bağlantı noktalarını engellemek ve etki alanınızı yönetme**.
* Aşırı yönetilen etki alanınız için ayrılmış bir alt ağ içinde kullanılabilir IP adresi sayısını kısıtlamaz. Bu kısıtlama, hizmet, iki etki alanı denetleyicisi yönetilen etki alanınız için kullanılabilir bulunmasını önler.
* **Ağ geçidi alt ağı, Azure AD Domain Services'ı etkinleştirmeyin** sanal ağ.

> [!WARNING]
> İlişkilendirdiğinizde, bir NSG bir alt ağ, Azure AD Domain Services ile etkinleştirilmişse, Microsoft'un hizmet ve etki alanını yönetme özelliğini bozabilir. Ayrıca, Azure AD kiracınız ile yönetilen etki alanınız arasında eşitlemeyi bozulur. **SLA'sı, burada bir NSG engelleyen Azure AD Domain Services güncelleştirme ve etki alanınızı yönetme uygulanmış olan dağıtımlar için geçerli değildir.**
>
>

## <a name="ports-required-for-azure-ad-domain-services"></a>Azure AD Domain Services için gereken bağlantı noktaları
Aşağıdaki bağlantı noktaları, Azure AD Domain Services hizmetine için gereklidir ve yönetilen etki alanınıza sürdürün. Yönetilen etki alanınıza etkinleştirdiğiniz alt ağ için bu bağlantı noktalarının engellenmediğinden emin olun.

| Bağlantı noktası numarası | Gerekli mi? | Amaç |
| --- | --- | --- |
| 443 | Zorunlu |Azure AD kiracınız ile eşitleme |
| 5986 | Zorunlu | Etki alanınızın Yönetimi |
| 3389 | Zorunlu | Etki alanınızın Yönetimi |
| 636 | İsteğe bağlı | Yönetilen etki alanınıza güvenli LDAP (LDAPS) erişim |

**Bağlantı noktası 443 (Azure AD ile eşitleme)**
* Yönetilen etki alanınızı Azure AD dizininizi eşitlemek için kullanılır.
* Nsg'nizdeki Bu bağlantı noktasına erişime izin vermek için zorunludur. Bu bağlantı noktası erişimi olmadan, yönetilen etki alanınızı Azure AD directory ile eşitlenmiş durumda değil. Kullanıcıların parolalarını değişiklikler, yönetilen Etki Alanınızla eşitlenmez oturum açmak mümkün olmayabilir.
* Azure IP adres aralığına ait IP adresleri için bu bağlantı noktasına gelen erişimi kısıtlayabilirsiniz. Azure IP adresi aralığı kuralda gösterilen PowerShell aralığından daha farklı bir aralık olduğuna dikkat edin.

**Bağlantı noktası 5986 (PowerShell uzaktan iletişimi)**
* PowerShell uzaktan iletişimini kullanarak yönetilen etki alanınızda yönetim görevleri gerçekleştirmek için kullanılır.
* Nsg'nizdeki Bu bağlantı noktası üzerinden erişime izin vermek için zorunludur. Bu bağlantı noktası erişimi olmadan, yönetilen etki alanınıza güncelleştirilmiş, yapılandırılmış, yedeklenen veya izlenen olamaz.
* Herhangi bir yeni etki alanları veya bir Azure Resource Manager sanal ağına sahip etki alanları için aşağıdaki kaynak IP adresleri için bu bağlantı noktasına gelen erişimi kısıtlayabilirsiniz: 52.180.179.108, 52.180.177.87, 13.75.105.168, 52.175.18.134, 52.138.68.41, 52.138.65.157, 104.41.159.212, 104.45.138.161, 52.169.125.119, 52.169.218.0, 52.187.19.1, 52.187.120.237, 13.78.172.246, 52.161.110.169, 52.174.189.149, 40.68.160.142, 40.83.144.56, 13.64.151.161, 52.180.183.67, 52.180.181.39, 52.175.28.111, 52.175.16.141, 52.138.70.93, 52.138.64.115, 40.80.146.22, 40.121.211.60, 52.138.143.173, 52.169.87.10, 13.76.171.84, 52.187.169.156, 13.78.174.255, 13.78.191.178, 40.68.163.143, 23.100.14.28, 13.64.188.43, 23.99.93.197
* Klasik bir sanal ağ ile etki alanları için aşağıdaki kaynak IP adresleri için bu bağlantı noktasına gelen erişimi kısıtlayabilirsiniz: 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209
* Yönetilen etki alanınız için etki alanı denetleyicileri genellikle bu bağlantı noktasında dinleme değil. Yalnızca bir yönetim veya bakım işlemi yönetilen etki alanı için gerçekleştirilecek gerektiğinde hizmet yönetilen etki alanı denetleyicilerinde Bu bağlantı noktasını açar. Hizmet, işlemi tamamlandıktan hemen sonra yönetilen etki alanı denetleyicilerinde Bu bağlantı noktasını kapatır.

**Bağlantı noktası 3389 (Uzak Masaüstü)**
* Yönetilen etki alanınız için etki alanı denetleyicileri için Uzak Masaüstü bağlantıları için kullanılır.
* Aşağıdaki kaynak IP adresleri için gelen erişimi kısıtlayabilirsiniz: 207.68.190.32/27, 13.106.78.32/27, 13.106.174.32/27, 13.106.4.96/27
* Bu bağlantı noktası da büyük ölçüde yönetilen etki alanınızda devre dışı kalır. PowerShell uzaktan iletişimini kullanarak yönetim ve izleme görevlerini yapıldığından, bu mekanizma sürekli olarak kullanılmaz. Bu bağlantı noktası, Microsoft Gelişmiş sorun giderme için yönetilen etki alanınıza uzaktan bağlanmak için gereken nadir durumlarda kullanılır. Sorun giderme işlemi tamamlandıktan hemen sonra bağlantı noktası kapalı.

**Bağlantı noktası 636 (güvenli LDAP)**
* İnternet üzerinden yönetilen etki alanınıza güvenli LDAP erişimini etkinleştirmek için kullanılır.
* Bu bağlantı noktası üzerinden NSG açma isteğe bağlıdır. Yalnızca etkin internet üzerinden güvenli LDAP erişimi varsa, bağlantı noktasını açın.
* Bu bağlantı noktası üzerinden güvenli LDAP bağlanmak beklediğiniz kaynak IP adresleri için gelen erişimi kısıtlayabilirsiniz.


## <a name="network-security-groups"></a>Ağ Güvenlik Grupları
A [ağ güvenlik grubu (NSG)](../virtual-network/virtual-networks-nsg.md) izin veren veya bir sanal ağ, sanal makine örneklerine trafiği reddeden erişim denetimi listesi (ACL) kurallarının bir listesini içerir. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur. Ayrıca, tekil bir VM trafik kısıtlanabilir NSG'yi doğrudan bu VM ile ilişkilendirme tarafından daha fazla.

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Azure AD Domain Services ile sanal ağlar için örnek NSG
Aşağıdaki tabloda örnek bir Azure AD Domain Services yönetilen etki alanı ile bir sanal ağ için yapılandırabileceğiniz NSG gösterilmektedir. Bu kural, yönetilen etki alanında kalır emin olmak için gerekli bağlantı noktaları üzerinden gelen trafiği yama, güncelleştirilen ve Microsoft tarafından izlenebilir sağlar. Varsayılan 'DenyAll' Kural diğer gelen tüm trafiği internet'ten geçerlidir.

Ayrıca, NSG ayrıca internet üzerinden güvenli LDAP erişimini kilitlemek nasıl gösterir. Bu kural, güvenli LDAP erişimi için yönetilen etki alanınıza internet üzerinden etkinleştirmediyseniz atlayın. NSG, TCP bağlantı noktası IP adresleri üzerinden 636 yalnızca belirtilen bir kümesinden gelen LDAPS erişime izin veren kuralları kümesi içerir. Belirtilen IP adresleri internet üzerinden LDAPS erişime izin vermek için NSG kuralı DenyAll NSG kuralı daha yüksek bir önceliğe sahiptir.

![Örnek internet üzerinden LDAPS erişimin güvenliğini sağlamak için NSG](./media/active-directory-domain-services-alerts/default-nsg.png)

**Daha fazla bilgi** - [ağ güvenlik grubu oluşturma](../virtual-network/manage-network-security-group.md).


## <a name="network-connectivity"></a>Ağ bağlantısı
Azure AD Domain Services yönetilen etki alanı, yalnızca azure'da tek bir sanal ağ içinde etkinleştirilebilir.

### <a name="scenarios-for-connecting-azure-networks"></a>Azure ağları bağlama senaryoları
Aşağıdaki dağıtım senaryoları hiçbirini yönetilen etki alanınızı kullanmak için Azure sanal ağları birbirine bağlama:

#### <a name="use-the-managed-domain-in-more-than-one-azure-virtual-network"></a>Birden fazla Azure sanal ağdaki yönetilen etki alanını kullan
Diğer Azure sanal ağlarına Azure AD Domain Services'i etkinleştirdiğiniz Azure sanal ağına bağlanabilir. Bu VPN/VNet eşleme bağlantısı diğer sanal ağlara dağıtılan iş yüklerinizi ile yönetilen etki alanını kullanmanızı sağlar.

![Klasik sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Resource Manager tabanlı bir sanal ağdaki yönetilen etki alanını kullan
Azure AD Domain Services'i etkinleştirdiğiniz Azure Klasik sanal ağ Resource Manager tabanlı bir sanal ağa bağlanabilir. Bu bağlantı, yönetilen etki alanı ile Resource Manager tabanlı sanal ağda dağıtılan iş yüklerinizi kullanmanıza olanak sağlar.

![Klasik sanal ağ bağlantısını için Resource Manager](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Ağ bağlantısı seçenekleri
* **VNet-VNet bağlantıları kullanarak sanal ağ eşlemesi**: Sanal Ağ eşlemesi aynı bölgedeki iki sanal ağı Azure omurga ağı aracılığıyla birbirine bağlayan bir mekanizmadır. Eşleme yapıldıktan sonra, iki sanal ağ tüm bağlantılarda tek bir sanal ağ gibi görünür. Bunlar ayrı kaynaklar olarak yönetilmeye devam eder, ancak bu sanal ağlardaki sanal makineler özel IP adresleri kullanarak birbirleriyle doğrudan iletişim kurabilir.

    ![Eşleme kullanarak sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Daha fazla bilgi - sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md)

* **Siteden siteye VPN bağlantıları kullanarak VNet-VNet bağlantılarında**: Bir sanal ağı başka bir sanal ağa bağlamak (VNet-VNet) bir sanal ağ bir şirket içi site konumuna bağlamakla aynıdır. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır.

    ![VPN ağ geçidi kullanarak sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Daha fazla bilgi - VPN ağ geçidi kullanarak sanal ağları birbirine bağlama](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

<br>

## <a name="related-content"></a>İlgili İçerik
* [Azure sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md)
* [Klasik dağıtım modeli için bir VNet-VNet bağlantısını yapılandırma](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Azure ağ güvenlik grupları](../virtual-network/security-overview.md)
* [Ağ güvenlik grubu oluşturma](../virtual-network/manage-network-security-group.md)
