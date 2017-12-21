---
title: "En iyi güvenlik uygulamaları, Azure sanal makinesi | Microsoft Docs"
description: "Bu makalede, Azure'da bulunan sanal makinelerde kullanılacak en iyi güvenlik uygulamaları çeşitli sağlar."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 5e757abe-16f6-41d5-b1be-e647100036d8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: yurid
ms.openlocfilehash: db8b0cc58738308116da84f2a45d6507c87f3cde
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="best-practices-for-azure-vm-security"></a>Azure VM Güvenlik için en iyi yöntemler

Bir hizmet (Iaas) senaryoları çoğu altyapıda [Azure sanal makineleri (VM'ler)](https://docs.microsoft.com/azure/virtual-machines/) olan ana iş yükü bulut kullanan kuruluşlar için bilgi işlem. Bu durum özellikle açıktır [karma senaryolar](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) kuruluşlar yavaş iş yüklerinin buluta geçirmek istediğiniz. Bu senaryoda izleyin [Iaas için genel güvenlik konuları](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx)ve tüm Vm'leriniz için en iyi güvenlik yöntemleri uygulayın.

Bu makalede ele çeşitli VM en iyi güvenlik uygulamalarını, her müşterilerimizden aldığımız türetilmiş ve sanal makineler ile doğrudan kendi deneyimlerini.

İyi bir fikir anlaşma üzerinde temel alır ve geçerli Azure platformu özellikleri ile çalışma ve ayarlar özellik. Görüşlerini ve teknolojileri zamanla değişebilir olduğundan, bu makalede düzenli aralıklarla bu değişiklikleri yansıtacak şekilde güncelleştirmeyi planlıyoruz.

En iyi her uygulama için makaleyi açıklanmaktadır:

* Hangi en iyi bir uygulamadır.
* Neden etkinleştirmek iyi bir fikirdir.
* Nasıl etkinleştirmek bilgi edinebilirsiniz.
* Etkinleştirmek başarısız olursa ne.
* Olası alternatifler en iyi deneyim için.

Makaleyi aşağıdaki VM en iyi güvenlik uygulamaları inceler:

* VM kimlik doğrulaması ve erişim denetimi
* VM kullanılabilirlik ve ağ erişim
* Şifreleme zorlayarak VM'ler, kalan verileri koruma
* VM güncelleştirmelerini yönetme
* VM güvenlikle ilgili tutumunuzu yönetme
* VM performansını izleme

## <a name="vm-authentication-and-access-control"></a>VM kimlik doğrulaması ve erişim denetimi

VM korumanın ilk adımı, yalnızca yetkili kullanıcılar yeni Vm'leri ayarlayabilmek sağlamaktır. Kullanabileceğiniz [Azure ilkeleri](../azure-policy/azure-policy-introduction.md) , kuruluşunuzdaki kaynakların kuralları oluşturmak için özelleştirilmiş ilkeler oluşturma ve bu ilkeler, kaynaklara gibi uygulama [kaynak grupları](../azure-resource-manager/resource-group-overview.md).

Doğal bir kaynak grubuna ait VM'ler ilkelerine devralır. Sanal makineleri yönetmek için bu yaklaşım önerilir ancak aynı zamanda erişimi tek tek VM ilkeleri kullanarak denetleyebilirsiniz [rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md).

Resource Manager ilkeleri ve VM erişimi denetlemek için RBAC etkinleştirdiğinizde, genel VM Güvenlik geliştirilmesine yardımcı olun. Aynı kaynak grubuna VM'ler ile aynı yaşam döngüsü birleştirmek öneririz. Kaynak grupları kullanarak, dağıtmak, izlemek ve maliyetleri kaynaklarınız için faturalama yukarı alma. Erişim ve Vm'leri ayarlamak kullanıcıların etkinleştirmek için bir [en az ayrıcalık yaklaşım](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models). Ve kullanıcıların ayrıcalıkları atadığınızda, aşağıdaki yerleşik Azure rolleri kullanmak plan yapın:

- [Sanal makine Katılımcısı](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor): VM'ler, ancak bunlar bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz.
- [Klasik sanal makine Katılımcısı](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor): Klasik dağıtım modeli, ancak sanal makineleri bağlı olmayan sanal ağ veya depolama hesabı kullanılarak oluşturulan sanal makineleri yönetebilirsiniz.
- [Güvenlik Yöneticisi](../active-directory/role-based-access-built-in-roles.md#security-manager): güvenlik bileşenleri, güvenlik ilkeleri ve sanal makineleri yönetebilirsiniz.
- [DevTest Labs kullanıcı](../active-directory/role-based-access-built-in-roles.md#devtest-labs-user): her şeyi görüntüleyip bağlamak, başlatmak, yeniden başlatın ve sanal makineleri kapatın.

Hesapları ve parolaları Yöneticiler arasında paylaşmayın ve parolaları birden çok kullanıcı hesapları veya hizmetleri, özellikle sosyal medya için parolaları veya diğer yönetim dışı etkinlikler arasında yeniden yok. İdeal olarak, kullanmanız gereken [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) , Vm'leri güvenli bir şekilde ayarlamak için şablonlar. Bu yaklaşımı kullanarak, dağıtım seçenekleriniz güçlendirmek ve güvenlik ayarlarını dağıtım boyunca zorlamak.

Veri erişim denetimi RBAC gibi özelliklerinden yararlanarak zorlamaz kuruluşlar, kullanıcılar gerekenden daha fazla ayrıcalıklarını verme. Belirli veri uygunsuz kullanıcı erişimini doğrudan bu verileri tehlikeye atabilir.

## <a name="vm-availability-and-network-access"></a>VM kullanılabilirlik ve ağ erişim

VM, yüksek kullanılabilirlik sağlamak için gereken kritik uygulamaların çalıştırıyorsa, birden çok VM kullanmanızı öneririz. Daha iyi kullanılabilirlik için en az iki VM oluşturma [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md).

[Azure yük dengeleyici](../load-balancer/load-balancer-overview.md) ayrıca yük dengeli sanal makineleri aynı kullanılabilirlik kümesine ait gerektirir. Bu sanal makineleri Internet'ten erişilmesi gerekiyorsa yapılandırmalısınız bir [Internet'e yönelik Yük Dengeleyici](../load-balancer/load-balancer-internet-overview.md).

Sanal makineleri Internet'e açık olduğunda önemlidir, [kontrol ağ güvenlik grupları (Nsg'ler) ile ağ trafiği akışını](../virtual-network/virtual-networks-nsg.md). Nsg'ler alt ağlara uygulanabildiği kaynaklarınızı alt ağa göre gruplandırma ve Nsg'leri alt ağlara uygulayarak sayısını en aza indirebilirsiniz. Düzgün şekilde yapılandırarak yapabilirsiniz Ağ yalıtım bir katman oluşturmak için amacı olan [ağ güvenliği](../best-practices-network-security.md) Azure özellikleri.

Uzaktan erişim için belirli bir VM'yi ve nasıl uzun olan denetim Azure Güvenlik Merkezi'nden tam zamanında (JIT) VM-erişim özelliği de kullanabilirsiniz.

Internet'e VM'ler için ağ erişim kısıtlamalarını zorlamaz kuruluşlar, Uzak Masaüstü Protokolü (RDP) yanılma saldırısı gibi güvenlik risklerine maruz kalır.

## <a name="protect-data-at-rest-in-your-vms-by-enforcing-encryption"></a>Şifreleme zorlayarak, sanal makineleri, kalan verileri koruma

[Bekleyen verileri şifreleme](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) veri gizliliği, uyumluluk ve veri egemenliği doğru zorunlu bir adımdır. [Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md) Windows ve Linux Iaas VM diskleri şifrelemek BT yöneticilerine sağlar. Disk şifrelemesi, endüstri standardı Windows BitLocker özelliği ve işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Linux dm-crypt özelliği birleştirir.

Kurumsal güvenlik ve uyumluluk gereksinimlerini karşılamak için verilerinizi koruma sağlanmasına yardımcı olmak için Disk şifrelemesi uygulayabilirsiniz. Kuruluşunuz, riskleri ilgili yetkisiz veri erişimi azaltmaya yardımcı olmak için şifreleme kullanmayı düşünün. Ayrıca, bunlara hassas verileri yazmadan önce sürücülerinizin şifrelemek öneririz.

Azure depolama hesabınızdaki REST onları korumak için VM veri birimleri şifrelemek emin olun. Şifreleme anahtarları ve gizli anahtarı kullanarak koruma [Azure anahtar kasası](https://azure.microsoft.com/documentation/articles/key-vault-whatis/).

Veri şifrelemeyi zorunlu olmayan kuruluşlar için veri bütünlüğü sorunları daha sunulur. Örneğin, yetkisiz veya standart dışı kullanıcılar bir veri güvenliği aşılmış hesaplarındaki çalmak veya ClearFormat kodlanmış verileri yetkisiz erişim elde. Bu tür riskler için ayırdığınız yanı sıra, endüstri düzenlemelerle uyumlu olması şirketler bunlar tespitlerini kullanan ve kendi veri güvenliğini artırmak için doğru güvenlik denetimlerini kullanarak olduğundan kanıtlamak gerekir.

Disk şifrelemesi hakkında daha fazla bilgi için bkz: [için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler](azure-security-disk-encryption.md).


## <a name="manage-your-vm-updates"></a>VM güncelleştirmelerini yönetme

Tüm şirket içi sanal makineler gibi Azure VM'ler kullanıcı yönetilen yönelik olduğundan, Azure Windows güncelleştirmelerini bunları için gönderme değil. Ancak, otomatik Windows Update ayar etkin olarak bırakmak için teşvik. Başka bir seçenek dağıtmaktır [Windows Server Update Services (WSUS)](https://technet.microsoft.com/windowsserver/bb332157.aspx) veya başka bir uygun güncelleştirme yönetimi ürün başka bir VM veya şirket içi. WSUS ve Windows Update VM'ler güncel kalmasını sağlayın. Ayrıca, tüm Iaas Vm'leri güncel olduğunu doğrulamak için bir tarama ürün kullanmanızı öneririz.

Azure tarafından sağlanan stok görüntüleri, Windows güncelleştirmelerinin son gidiş dahil etmek için düzenli olarak güncelleştirilir. Ancak, görüntüleri dağıtım sırasında geçerli olacağını garanti yoktur. Ortak sürümleri izleyen bir hafif gecikme (en fazla birkaç hafta) mümkün olabilir. Denetleme ve tüm Windows güncelleştirmelerini yükleme ilk adımı, her dağıtım olması gerekir. Bu ölçü, ya da kendi kitaplığınızı gelen görüntüleri dağıtırken uygulamak özellikle önemlidir. Azure Marketi'nde bir parçası olarak sağlanan görüntüleri, varsayılan olarak otomatik olarak güncelleştirilir.

Yazılım güncelleştirme ilkelerini zorunlu olmayan kuruluşlar daha bilinen, daha önce sabit güvenlik açıklarından yararlanma tehditlere maruz kalır. Endüstri düzenlemelerle uyumlu olması bağlı olarak, bu tür tehditleri riski yanı sıra şirketler bunlar tespitlerini kullanan ve bulutta bulunan, iş yükü güvenliğini sağlamaya yardımcı olmak için doğru güvenlik denetimlerini kullanarak olduğundan kanıtlamanız gerekir.

Geleneksel veri merkezlerini ve Azure Iaas için en iyi uygulamaları yazılım güncelleştirme birçok benzerlikler sahip vurgulamak önemlidir. Bu nedenle, VM'ler dahil etmek için geçerli yazılım güncelleştirme ilkelerinizi değerlendirmek öneririz.

## <a name="manage-your-vm-security-posture"></a>VM güvenlikle ilgili tutumunuzu yönetme

Gelişen siber tehditlere ve Vm'lerinizi koruma hızla tehditleri algılayabilir, kaynaklarınıza yetkisiz erişimi önlemeye, uyarılarını tetikler ve hatalı pozitif sonuçları azaltmak izleme kapasitesine zengin gerektirir. Bu tür, bir iş yükü için güvenlik tutumunu ağ erişimi güvenli hale getirmek için güncelleştirme yönetimi, VM'den tüm güvenlik yönlerini oluşur.

Güvenlik yaklaşımı, izlemek için [Windows](../security-center/security-center-virtual-machine.md) ve [Linux VM'ler](../security-center/security-center-linux-virtual-machine.md), kullanın [Azure Güvenlik Merkezi](../security-center/security-center-intro.md). Azure Güvenlik Merkezi'nde, aşağıdaki özellikleri yararlanarak Vm'lerinizi koruma:

* Önerilen yapılandırma kuralları ile işletim sistemi güvenlik ayarlarını uygula
* Tanımlamak ve sistem güvenliğini ve eksik olabilir kritik güncelleştirmeleri yükleyin
* Uç nokta kötü amaçlı yazılımdan koruma koruma önerileri dağıtma
* Disk şifrelemesi doğrula
* Değerlendirmek ve güvenlik açıkları düzeltmek
* Tehditleri algılama

Güvenlik Merkezi, tehditleri için etkin olarak izleyebilir ve olası risklere açığa altında **güvenlik uyarıları**. Bağıntılı tehditleri adlı tek bir görünümde toplanan **güvenlik olayı**.

Güvenlik Merkezi Vm'lerinizi Azure içinde bulunan, olası tehditleri tanımlamak nasıl yardımcı olabileceğini anlamak için aşağıdaki videoyu izleyin:

<iframe src="https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Güçlü güvenlik tutumunu Vm'leri için zorlamaz kuruluşlar tesis edilmiş güvenlik denetimleri aşmak için olası denemeleri yetkisiz kullanıcılar tarafından farkında kalır.

## <a name="monitor-vm-performance"></a>VM performansını izleme

Kaynak kötüye VM işlemleri izin verilenden daha fazla kaynak tüketmesine bir sorun olabilir. VM performans sorunlarını kullanılabilirlik güvenlik ilkesini ihlal eden hizmet kesintisi için yol açabilir. Bu nedenle, bir sorun gerçekleştirildiği sırada VM erişimi değil yalnızca Tepkisel izlemek için kesinlik temelli ancak aynı zamanda bir proaktif olarak normal işlem sırasında ölçülen temel performans karşı.

Çözümleme tarafından [Azure tanılama günlük dosyaları](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), VM kaynaklarınızı izlemek ve performans ve kullanılabilirlik tehlikeye atabilir olası sorunları. Azure tanılama uzantısını Windows tabanlı sanal makinelerin izleme ve tanılama olanakları sağlar. Bu özellikler bir parçası olarak uzantısı dahil olmak üzere etkinleştirebilirsiniz [Azure Resource Manager şablonu](../virtual-machines/windows/extensions-diagnostics-template.md).

Aynı zamanda [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-metrics.md) , kaynağın durumu görünürlük elde etmek için.

VM performans izleme kuruluşlar performans modellerini bazı değişiklikler normal veya anormal olup olmadığını belirleyemiyor. VM normalden daha fazla kaynak tüketen varsa, bu tür bir anomali dış kaynak ya da VM içinde çalışan güvenliği aşılmış bir işlem olası bir saldırı olduğunu gösteriyor olabilir.
