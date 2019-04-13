---
title: Bir kuruluş için Azure DevTest Labs için başvuru mimarisi
description: Bu makale, bir kuruluşta bir başvuru Mimarisi Kılavuzu için Azure DevTest Labs sağlar.
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
ms.openlocfilehash: 61e76369a4d73bd171c9e5c2462b3f261681ba00
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59551390"
---
# <a name="azure-devtest-labs---reference-architecture-for-an-enterprise"></a>Azure DevTest Labs - kuruluş için başvuru mimarisi
Bu makalede, bir kuruluşta Azure DevTest Labs dayalı bir çözümü dağıtmak için bir başvuru mimarisi sağlar. Bu, sanal makinelerine uzaktan oturum açma Uzak Masaüstü Ağ geçidi, özel yapıtlar ve Laboratuvar içinde kullanılan diğer PaaS Hizmetleri için bir yapıt deposu bağlantısı Express Route üzerinden şirket içi bağlantıyı içerir.

![Başvuru mimarisi](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Mimari
Başvuru mimarisindeki temel öğeleri şunlardır:

- **Azure Active Directory (AAD)**: Azure DevTest Labs kullanan [kimlik yönetimi için Azure Active Directory Hizmeti](../active-directory/fundamentals/active-directory-whatis.md). DevTest Labs hakkında kullanıcılara göre bir ortama erişimi sağlama konusunda dikkate alınması gereken iki önemli nokta vardır:
    - **Kaynak Yönetimi**:  Kaynakları yönetmek için Azure portalına erişim sağlar (sanal makineler oluşturma, ortamları, Başlat/Durdur/yeniden/delete/applyartifacts vb.). Roble tabanlı erişim denetimi (RBAC) kullanarak azure'da ve kullanıcı, ayarı kaynak ve erişim düzeyi izinleri için rol ataması uygulama tarafından gerçekleştirilir.
    - **Sanal makineler (ağ düzeyinde)**:  Varsayılan yapılandırmasında, sanal makineler bir yerel yönetici hesabı kullanın.  Mevcut bir etki alanı varsa ([AAD etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-overview.md), bir şirket içi etki alanı veya bulut tabanlı bir etki alanı), makineleri etki alanına katılması. Birleştirilmiş sonra kullanıcıların etki alanı tabanlı kimliklerini Vm'lere bağlanmak için kullanırsınız.
- **Şirket içi bağlantı**: Yukarıdaki mimari diyagramında [Express Route](../expressroute/expressroute-introduction.md) i kullanılmıştır, ancak ayrıca bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md). DevTest Labs için zorunlu olsa da, kuruluşlarda yaygın olarak görülür. Yalnızca şirket kaynaklarına bağlanmak için bir neden olup olmadığını zorunludur. Yaygın nedenleri şunlardır: 
    - Şirket içi verileri buluta henüz taşınamayan
    - Laboratuvar sanal makineleri şirket içi etki alanına tercihi
    - Tüm ağ trafiğini bulut ortamı güvenlik/uyumluluk nedenleriyle bir şirket içi güvenlik duvarı üzerinden içine ve dışına zorlama
- **Ağ güvenlik grupları**: Bulut ortamı (veya Bulut ortamında) trafiği kısıtlamak için yaygın bir yolu tabanlı kaynak ve hedef IP adresi olduğu kullanmak için bir [ağ güvenlik grubu](../virtual-network/security-overview.md). Örneğin, yalnızca Laboratuvar ağlara ve şirket ağından gelen ağ trafiğine izin verme.
- **Uzak Masaüstü Ağ Geçidi**:  Kuruluşlar genellikle şirket güvenlik duvarında giden Uzak Masaüstü bağlantılarını engelleyin. DevTest Labs bulut tabanlı ortamında bağlantıyı etkinleştirmek için kullanma gibi birkaç seçenek vardır bir [Uzak Masaüstü Ağ Geçidi](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture) (ağ geçidi yük dengeleyici için statik IP beyaz liste) veya [gelen tüm yönlendirme RDP trafiğine](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) Express Route/Site siteden siteye VPN bağlantısı üzerinden. Bir ortak kuruluş DevTest Labs'de bir dağıtımı planlarken noktadır.
- **Azure ağ (sanal ağlar, alt ağlar)**:  [Azure ağı](../networking/networking-overview.md) başka bir anahtar öğesi genel DevTest Labs mimarisinde topolojidir. Bu kaynaklardan (veya değil), şirket içi erişim (veya etkinleştirmezsiniz) iletişim ve internet'e (veya etkinleştirmezsiniz) erişmek için labs sağlar. Müşteriler DevTest Labs kullanarak en yaygın yolu mimari diyagramını içeren (üzerinden bağlanan tüm labs [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md) kullanarak bir [merkez-uç modeli](/architecture/reference-architectures/hybrid-networking/hub-spoke) Express Route/Site siteden siteye VPN bağlantısı için ağ altyapısını kurma konusunda herhangi bir kısıtlama yok doğrudan şirket içine), ancak DevTest Labs beri Azure ağ kullanır.
- **DevTest Labs**:  DevTest Labs genel mimarisinin önemli bir parçasıdır. Hizmet hakkında bilgi edinmek için [DevTest Labs hakkında](devtest-lab-overview.md).
- **Sanal makineler ve diğer kaynaklara (SaaS, PaaS ve Iaas)**:  DevTest Labs tarafından desteklenen anahtar iş yüklerinin girişlerden diğer Azure kaynakları birlikte sanal makineler.  DevTest Labs, kolay ve hızlı (sanal makineler ve diğer Azure kaynakları dahil), Azure kaynaklarına erişim vermek bir kuruluş için getirir.  Azure için erişim hakkında daha fazla bilgi [geliştiriciler](devtest-lab-developer-lab.md) ve [sınayıcılar](devtest-lab-test-env.md).

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs herhangi bir yerleşik kotaları veya sınırları olması gerekmese de, diğer Azure kaynakları tipik kullanılan bir laboratuvar işlemlerini sahip [abonelik düzeyinde kotalar](../azure-subscription-service-limits.md). Sonuç olarak, tipik kurumsal bir dağıtımda, büyük bir dağıtımın DevTest Labs karşılamak üzere birden çok Azure aboneliğiniz olmalıdır. En yaygın olarak kurumların ulaşıldı kotalar şunlardır:

- **Kaynak grupları**:  DevTest Labs, varsayılan yapılandırmasında, her yeni bir sanal makine veya hizmetini kullanarak bir kullanıcının oluşturduğu bir ortam için bir kaynak grubu oluşturur. Abonelikleri içerebilir bir [980 kaynak gruplarının en yüksek](../azure-subscription-service-limits.md#subscription-limits---azure-resource-manager), bu sınır, sanal makineler ve ortamlar bir abonelik sayısı üst sınırı olacak şekilde. Dikkate almanız gereken iki yapılandırmaları vardır:
    - **[Tüm sanal makineler aynı kaynak grubuna gidin](resource-group-control.md)**:  Kaynak grubu sınırı ile yardımcı olmakla birlikte, kaynak grubu başına kaynak türü etkiler.
    - **Kullanılarak paylaşılan genel IP'ler**:  Tüm sanal makineler aynı büyüklük ve aynı bölgede aynı kaynak grubuna gidin. Genel IP adreslerine sahip sanal makineler izin veriliyorsa bu bir 'Orta zemin' kaynak grubu kotaları ve kaynak grubu kotalarını başına kaynak türü arasında olur. 
- **Kaynak başına kaynak grubunda kaynak türü**: İçin varsayılan sınır [kaynaklar kaynak grubuna kaynak türü başına olan 800](../azure-subscription-service-limits.md#resource-group-limits).  Kullanımı, tüm sanal makineler, aynı kaynak grubu yapılandırması için özellikle Vm'lere birçok ek diskler varsa, kullanıcılar bu abonelik sınırını çok çabuk ulaşırsınız.
- **Depolama hesapları**: Bir depolama hesabı ve Azure kotası için DevTest labs'deki bir laboratuvara birlikte [sayıdır depolama hesaplarının her Abonelikteki bölge başına 250](../azure-subscription-service-limits.md#storage-limits). DevTest Labs ile aynı bölgede sayısı için üst sınırı 250 olduğunu anlamına gelir.
- **Rol atamaları**: Bir rol ataması, bir kaynak (sahibi, kaynak, izin düzeyi) bir kullanıcı veya sorumlusu erişimi vermeniz şeklidir. Azure'da var. bir [sınırı abonelik başına 2000 rol ataması](../azure-subscription-service-limits.md#role-based-access-control-limits). DevTest Labs hizmetinde (varsayılan yapılandırma), her VM için bir kaynak grubu oluşturur ve sahibi verilecek **sahibi** DevTest Labs sanal makine için izin ve **okuyucu** izni kaynak grubu.  Bu şekilde, oluşturulan her yeni VM rol atamaları vermiş Laboratuvar için kullanıcılar olduğunda oluşturulan iki ek rol atamaları kullanır.
- **API okumayı/yazmayı**: Azure ve DevTest Labs'i otomatik hale getirmek için (REST API'ler, PowerShell, CLI, Azure SDK'sı ve benzeri) çeşitli yolları vardır ve Otomasyon yoluyla, başka bir API isteği sınırına mümkündür. Her abonelik en fazla izin veriyor [12000 okuma yazma istekleri ve 1200, saat başına istek](../azure-resource-manager/resource-manager-request-limits.md).  DevTest Labs otomatikleştirirken dikkat edilmesi gereken bir sınırı var.

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs, tek bir lab ile çalışırken bir harika bir yönetici kullanıcı arabirimine sahiptir. Bir kuruluşta var. büyük olasılıkla birden fazla Azure aboneliğiniz birçok. Bu komut dosyası Otomasyon labs kullanarak tutarlı bir şekilde değişiklik gerektirdiğini anlamına gelir.  Bazı örnekler ve DevTest Labs dağıtımını yönetmek için en iyi yöntemleri şunlardır:

- **Laboratuvar ayarlarında yapılan değişiklikler**: Dağıtımdaki tüm labs arasında belirli Laboratuvar ayarı güncelleştirme yaygın bir senaryodur. Örneğin, yeni bir sanal makine örneğinin boyutu kullanılabilir ve tüm labs izin vermek için güncelleştirilmesi gerekir.  Azure PowerShell betikleri, Azure CLI veya REST API'leri kullanarak bu değişiklikleri otomatik hale getirmek idealdir.  
- **Yapıt deposu kişisel erişim belirteci**:  Genellikle, bir Git deposu için kişisel erişim belirteçleri (90 gün, 1 yıl, 2 yıllık) süresi. Sürekliliğinin sağlanması için kişisel erişim belirteci genişletmek veya yeni bir tane oluşturun ve yeni bir kişisel erişim belirteci için tüm Laboratuvar uygulamak için Otomasyon kullanmak önemlidir.
- **Bir laboratuvar ayarı değişiklikleri kısıtlama**:  Bu durum genellikle kısıtlı olması gereken belirli bir ayarı (örneğin, izin Market kullanılacak görüntüleri) olduğunda. Bu Azure İlkesi (Bu kaynak türü için değişiklikleri engelleyerek) yoluyla yapılabilir veya özel bir rol oluşturarak ve bu rol için laboratuvar 'owner' yerine verme. (İç desteği, Laboratuvar duyuru, izin verilen VM boyutları vb.) Laboratuvardaki ayarların çoğu için yapılabilir
- **Vm'leri bir adlandırma kuralını takip gerektiren**: Bulut tabanlı geliştirme ve test ortamının bir parçası olan sanal makineleri kolayca belirlemek için ortak bir istektir. Bunu [Azure İlkesi ile](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns).

DevTest Labs, Azure aynı şekilde yönetilir temel kaynakları (ağ, disk, bilgi işlem, vb.) azure'da kullandığına dikkat edin önemlidir.  Örneğin, Azure İlkesi bir laboratuvarda oluşturulan sanal makineler için geçerlidir. Azure Güvenlik Merkezi, VM uyumluluğuna bildirebilirsiniz. Azure Backup hizmeti, düzenli yedeklemeler VM'ler için laboratuvar sağlayın ve benzeri. 

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
Azure DevTest Labs, mevcut kaynaklar Azure'da (işlem, ağ ve benzeri) kullanır ve böylece tüm harika güvenlik özellikleri yerleşik platforma otomatik olarak yararlanır. Örneğin, yalnızca Kurumsal ağdan kaynaklanan için tüm gelen Uzak Masaüstü bağlantıları güvenli hale getirmek için Uzak Masaüstü Ağ Geçidi sanal ağ için ağ güvenlik grubu eklemek kadar kolaydır. Yalnızca ek güvenlik için Azure DevTest Labs laboratuvarlar günlük olarak kullanacağınız takım üyeleri için sağlanan izin düzeyini noktadır.  Verilen genel izinler ["Sahip" ve "DevTest Labs kullanıcısı"](devtest-lab-add-devtest-user.md). Bu roller hakkında daha fazla bilgi için bkz: [Azure DevTest Labs'de sahibini ve kullanıcıları ekleme](devtest-lab-add-devtest-user.md).

## <a name="next-steps"></a>Sonraki Adımlar
Bu serideki sonraki makaleye bakın: [Azure DevTest Labs altyapınızın ölçeğini](devtest-lab-guidance-scale.md)
