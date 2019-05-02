---
title: Azure DevTest Labs için Kurumsal başvuru mimarisi
description: Bu makalede, bir kuruluşta Azure DevTest Labs için başvuru Mimarisi Kılavuzu sağlanmaktadır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 73a3d426e9040525b0c631db273e59c49a6a9eb0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705874"
---
# <a name="azure-devtest-labs-reference-architecture-for-enterprises"></a>Kuruluşlar için Azure DevTest Labs başvuru mimarisi
Bu makalede, bir kuruluşta Azure DevTest Labs hakkında temel bir çözümü dağıtmak için başvuru mimarisi sağlar. Aşağıdakileri içerir:
- Azure ExpressRoute aracılığıyla şirket içi bağlantı
- Sanal makinelere uzaktan oturum açmak için Uzak Masaüstü Ağ Geçidi
- Özel yapıtlar için bir yapıt deposu bağlantısı
- Labs'de kullanılan diğer PaaS Hizmetleri

![Başvuru mimarisi diyagramı](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Mimari
Başvuru mimarisini temel öğeleri şunlardır:

- **Azure Active Directory (Azure AD)**: DevTest Labs kullanan [kimlik yönetimi için Azure AD hizmeti](../active-directory/fundamentals/active-directory-whatis.md). Bu iki temel unsur, kullanıcıların üzerinde DevTest Labs temelinde bir ortamda erişmesini dikkate alın:
    - **Kaynak Yönetimi**: Kaynakları yönetmek için Azure portalına erişim sağlar (sanal makineler oluşturun; ortamlar oluşturun; başlatma, durdurma, yeniden başlatın, Sil ve yapıtları; uygulamak ve benzeri). Kaynak yönetimi, rol tabanlı erişim denetimi (RBAC) kullanarak Azure'da gerçekleştirilir. Kullanıcılara rol atama ve kaynak ve erişim düzeyi izinleri ayarlayın.
    - **Sanal makineler (ağ düzeyinde)**: Varsayılan yapılandırmasında, sanal makineler bir yerel yönetici hesabı kullanın. Mevcut bir etki alanı varsa ([Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), bir şirket içi etki alanı veya bulut tabanlı bir etki alanı), makineleri etki alanına katılması. Kullanıcılar Vm'lere bağlanmak için etki alanı tabanlı kimliklerini kullanabilir.
- **Şirket içi bağlantı**: Bizim mimari diyagramında [ExpressRoute](../expressroute/expressroute-introduction.md) kullanılır. Ancak ayrıca bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md). ExpressRoute için DevTest Labs gerekli olmasa da, kuruluşlarda yaygın olarak kullanılır. ExpressRoute, şirket kaynaklarına erişimi gerekiyorsa gereklidir. Yaygın senaryolar şunlardır:
    - Sahip olduğunuz bir şirket içi verileri buluta taşınamaz.
    - Laboratuvar sanal makineleri şirket içi etki alanına katılmayı tercih eder.
    - Tüm ağ trafiğini bulut ortamı güvenlik/uyumluluk için bir şirket içi güvenlik duvarı üzerinden içine ve dışına zorlamak istiyorsanız.
- **Ağ güvenlik grupları**: Bulut ortamı (veya Bulut ortamında) trafiği kısıtlamak için yaygın bir yolu tabanlı kaynak ve hedef IP adresi olduğu kullanmak için bir [ağ güvenlik grubu](../virtual-network/security-overview.md). Örneğin, Laboratuvar ağlara ve şirket ağından çıkıp internet'i trafiğine izin vermek istediğiniz.
- **Uzak Masaüstü Ağ Geçidi**: Kuruluşlar genellikle şirket güvenlik duvarında giden Uzak Masaüstü bağlantılarını engelleyin. DevTest Labs dahil olmak üzere, bulut tabanlı ortamda bağlantıyı etkinleştirmek için birkaç seçenek vardır:
  - Kullanım bir [Uzak Masaüstü Ağ Geçidi](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)ve beyaz liste statik ağ geçidi IP adresini yük dengeleyici.
  - [Tüm gelen RDP trafiğine doğrudan](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) ExpressRoute/için siteden siteye VPN bağlantısı üzerinden. Kuruluşların DevTest Labs dağıtımı planlarken bu işlevselliği ortak bir noktadır.
- **Ağ Hizmetleri (sanal ağlar, alt ağlar)**: [Azure ağı](../networking/networking-overview.md) topolojidir DevTest Labs mimarisinde başka bir anahtar öğesi. Laboratuvar kaynakları iletişim kurabilmesini ve şirket içi ve internet erişimi olup olmadığını kontrol eder. Bizim mimarisi diyagramı, müşterilerin DevTest Labs kullanmasını en yaygın yolları içerir: Tüm laboratuvarlar aracılığıyla bağlanma [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) kullanarak bir [merkez-uç modeli](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) şirket içi ExpressRoute/için siteden siteye VPN bağlantısı için. Ancak nasıl, ağ altyapısını ayarlamak kısıtlaması yok olduklarından DevTest Labs Azure sanal ağ, doğrudan kullanır.
- **DevTest Labs**:  DevTest Labs genel mimarisinin önemli bir parçasıdır. Hizmet hakkında daha fazla bilgi için bkz: [DevTest Labs hakkında](devtest-lab-overview.md).
- **Sanal makineler ve diğer kaynaklara (SaaS, PaaS ve Iaas)**:  DevTest Labs destekleyen diğer Azure kaynakları ile birlikte temel bir iş yükü sanal makinelerdir. DevTest Labs, kolay ve hızlı bir şekilde Azure kaynakları (VM'ler ve diğer Azure kaynakları dahil) erişim sağlamak bir kuruluş sağlar. Azure için erişim hakkında daha fazla bilgi [geliştiriciler](devtest-lab-developer-lab.md) ve [sınayıcılar](devtest-lab-test-env.md).

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs yerleşik kota veya limitlerinizi olması gerekmese de, Laboratuvar tipik işleminde kullanılan diğer Azure kaynakları sahip [abonelik düzeyinde kotalar](../azure-subscription-service-limits.md). Bu nedenle, tipik kurumsal bir dağıtımda, büyük bir dağıtımın DevTest Labs karşılamak üzere birden çok Azure aboneliği gerekir. Kuruluşlar genellikle ulaşan kotaları şunlardır:

- **Kaynak grupları**: Varsayılan yapılandırmasında DevTest Labs her yeni bir sanal makine için bir kaynak grubu oluşturur veya hizmetini kullanarak kullanıcı bir ortam oluşturur. Abonelikleri içerebilir [kadar 980 kaynak grupları](../azure-subscription-service-limits.md#subscription-limits---azure-resource-manager). Bu nedenle, sanal makineler ve ortamlar bir abonelik sınırını olmasıdır. Dikkate almanız gereken iki diğer yapılandırmalar vardır:
    - **[Tüm sanal makineler aynı kaynak grubuna gidin](resource-group-control.md)**: Bu ayar, kaynak grubu sınırı karşılamanıza yardımcı olmakla birlikte, kaynak türü başına kaynak grubu sınırı etkiler.
    - **Kullanılarak paylaşılan genel IP'ler**: Tüm sanal makineler aynı büyüklük ve bölge aynı kaynak grubuna gidin. Bu yapılandırma bir "Orta zemin" kaynak grubu ve kaynak türü başına kaynak grubu kotaları arasında ise sanal makinelerin genel IP adreslerine sahip izin verilir.
- **Kaynak başına kaynak grubunda kaynak türü**: İçin varsayılan sınır [her kaynak türü başına kaynak grubu kaynakları olan 800](../azure-subscription-service-limits.md#resource-group-limits).  Kullanırken *tüm VM'lerin aynı kaynak grubuna gidin* yapılandırma, kullanıcılar isabet Bu abonelik özellikle Vm'lere birçok ek diskler varsa, daha erken sınırlayın.
- **Depolama hesapları**: DevTest labs'deki bir laboratuvara bir depolama hesabı ile birlikte gelir. Azure kotası [sayıdır depolama hesaplarının her Abonelikteki bölge başına 250](../azure-subscription-service-limits.md#storage-limits). DevTest Labs ile aynı bölgede en fazla sayısını da 250'dir.
- **Rol atamaları**: Bir rol ataması, nasıl bir kaynak (sahibi, kaynak, izin düzeyi) bir kullanıcı veya sorumlusu erişim vermek ' dir. Azure'da var olan bir [sınırı abonelik başına 2.000 rolü atamalarının](../azure-subscription-service-limits.md#role-based-access-control-limits). Varsayılan olarak, DevTest Labs hizmet her VM için bir kaynak grubu oluşturur. Sahibi verilen *sahibi* DevTest Labs sanal makine için izin ve *okuyucu* kaynak grubuna izin. Bu şekilde, iki rol atamaları kullanıcılar laboratuar izin verdiğinizde, kullanılan atamaları yanı sıra, oluşturduğunuz her yeni VM kullanır.
- **API okumayı/yazmayı**: Azure ve DevTest Labs REST API'ler, PowerShell, Azure CLI ve Azure SDK'sı dahil olmak üzere, otomatik hale getirmek için çeşitli yollar vardır. Otomasyon sayesinde, başka bir API isteği sınırına: Her abonelik en fazla izin veriyor [12.000 okuma yazma istekleri ve 1.200, saat başına istek](../azure-resource-manager/resource-manager-request-limits.md). DevTest Labs otomatikleştirin, bu sınırı dikkat edin.

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs ile tek bir laboratuvar çalışmak için bir harika yönetim kullanıcı arabirimine sahiptir. Ancak kuruluş, büyük olasılıkla birden çok Azure aboneliği ve birçok labs gerekir. Tutarlı bir şekilde tüm laboratuvarlarınızı değişiklik komut Otomasyon gerektirir. Bazı örnekler ve DevTest Labs dağıtımı için en iyi Yönetim uygulamaları şunlardır:

- **Laboratuvar ayarlarında yapılan değişiklikler**: Tüm dağıtım Labs'de arasında bir belirli Laboratuvar ayarını güncelleştirme yaygın bir senaryodur. Örneğin, yeni bir sanal makine örneğinin boyutu kullanılabilir ve tüm labs izin vermek için güncelleştirilmesi gerekir. PowerShell betikleri, CLI veya REST API'leri kullanarak bu değişiklikleri otomatik hale getirmek idealdir.  
- **Yapıt deposu kişisel erişim belirteci**:  Genellikle, bir Git deposu için kişisel erişim belirteçleri 90 gün içinde bir yıl veya iki yıl süresi dolar. Sürekliliğinin sağlanması için kişisel erişim belirteci genişletmek veya yeni bir tane oluşturmak önemlidir. Ardından yeni bir kişisel erişim belirteci için tüm Laboratuvar uygulamak için Otomasyon kullanın.
- **Bir laboratuvar ayarı değişiklikleri kısıtla**: Genellikle, belirli bir ayarı (Market görüntüleri kullanılmasına izin verme gibi) sınırlı olmalıdır. Bir kaynak türü için değişiklikleri önlemek için Azure İlkesi'ni kullanabilirsiniz. Veya özel bir rol oluşturun ve bu rolü yerine kullanıcılara vermek *sahibi* Laboratuvar için rol. Çoğu ayarlarında Laboratuvar (iç desteği, Laboratuvar duyuru, izin verilen VM boyutları vb.) için bunu yapabilirsiniz.
- **VM'lerin bir adlandırma kuralını uyguladığınızdan gerektiren**: Yöneticileri, bulut tabanlı geliştirme ve test ortamının bir parçası olan sanal makineleri kolayca belirlemek yaygın olarak istersiniz. Kullanarak bunu yapabilirsiniz [Azure İlkesi](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns).

DevTest Labs aynı şekilde yönetilir temel alınan Azure kaynaklarını kullandığına dikkat edin önemlidir: ağ, disk, işlem ve benzeri. Örneğin, Azure İlkesi, bir laboratuvarda oluşturulan sanal makineler için geçerlidir. Azure Güvenlik Merkezi, VM uyumluluğuna bildirebilirsiniz. Ve Azure Backup hizmeti, düzenli yedeklemeler için Vm'leri Laboratuvardaki sağlayabilir.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
Azure DevTest Labs, mevcut kaynaklar Azure'da (işlem, ağ ve benzeri) kullanır. Bu nedenle, otomatik olarak platformda yerleşik güvenlik özelliklerinden faydalanır. Örneğin, yalnızca Kurumsal ağdan kaynaklanan için gelen Uzak Masaüstü bağlantılarını zorunlu tutmak için bir ağ güvenlik grubu Uzak Masaüstü Ağ Geçidi sanal ağa eklemeniz yeterlidir. Laboratuvarlar günlük olarak kullanan takım üyeleri için verdiğiniz izin düzeyini yalnızca ek bir güvenlik önlemidir. En yaygın izinler [ *sahibi* ve *kullanıcı*](devtest-lab-add-devtest-user.md). Bu roller hakkında daha fazla bilgi için bkz: [Azure DevTest Labs'de sahibini ve kullanıcıları ekleme](devtest-lab-add-devtest-user.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu serideki sonraki makaleye bakın: [Azure DevTest Labs altyapınızın ölçeğini](devtest-lab-guidance-scale.md).
