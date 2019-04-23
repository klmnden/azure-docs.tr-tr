---
title: Azure DevTest Labs SSS | Microsoft Docs
description: Azure DevTest Labs hakkında sık sorulan sorulara yanıtlar bulun.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2019
ms.author: spelluru
ms.openlocfilehash: c26418d36271b4d2d39a43eda7e8b23585d69f4a
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60149436"
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs SSS
Azure DevTest Labs hakkında en yaygın soruların yanıtlarını alın.

## <a name="blog-post"></a>Blog gönderisi
DevTest Labs takım blogumuzda 20 Mart 2019'ten itibaren kullanımdan kaldırılmıştır. 

### <a name="where-can-i-track-feature-updates-from-now-on"></a>Burada özellik güncelleştirmeleri artık izleyebilir miyim?
Şu andan itibaren biz özellik güncelleştirmeleri ve bilgilendirici blog gönderilerini Azure blogunda yayınlayarak ve Azure güncelleştirir. Bu blog gönderileri, ayrıca gerekli yerlerde belgelerimize bağlayacaksınız.

Abone [DevTest Labs Azure blogunu](https://azure.microsoft.com/blog/tag/azure-devtest-labs/) ve [DevTest Labs Azure güncelleştirmeleri](https://azure.microsoft.com/updates/?product=devtest-lab) DevTest Labs'de yeni özellikler hakkında bilgi sahibi olmak için.

### <a name="what-happens-to-the-existing-blog-posts"></a>Mevcut blog gönderilerine yapılan ne olacak?
Şu anda (kesinti güncelleştirmeler hariç) geçirme mevcut blog gönderilerine çalışıyoruz bizim [DevTest Labs belgeleri](devtest-lab-overview.md). MSDN blog'u kullanım dışı olduğunda için DevTest Labs belgeleri genel bakış yönlendirilirsiniz. Yeniden yönlendirilen sonra 'Filtresi tarafından' başlığında aradığınız makalesi için arama yapabilirsiniz. Size, tüm gönderileri geçirilen henüz yapmadıysanız, ancak bu ay sonuna kadar yapılması gerekir. 


### <a name="where-do-i-see-outage-updates"></a>Kesinti güncelleştirmeleri nerede görebilirim?
Biz yayınlayarak bizim Twitter tanıtıcısı Bugünden başlayarak kullanarak kesinti güncelleştirmeleri. Bizi kesintiler ve bilinen hatalar en son güncelleştirmeleri almak için Twitter'da takip edin.

### <a name="twitter"></a>Twitter
Bizim Twitter tanıtıcısı: [@azlabservices](https://twitter.com/azlabservices)

## <a name="general"></a>Genel
### <a name="what-if-my-question-isnt-answered-here"></a>Peki sorumun cevabı burada bulamadığınız?
Sorunuzu burada listelenmiyorsa, yanıt bulmanıza yardımcı olabiliriz. Bu nedenle, bize bildirin.

- Bu SSS, sonunda bir soru gönderin. Azure Cache takım ve diğer topluluk üyelerinin bu makaleyle ilgili etkileşim kurun.
- Geniş bir kitleye ulaşmak için bir soru gönderin [Azure DevTest Labs MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). Azure DevTest Labs takım ve diğer topluluk üyelerinin ile etkileşim kurun.
- Özellik istekleri için istek ve fikirleri için gönderme [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="what-is-a-microsoft-account"></a>Microsoft hesabı nedir?
Bir Microsoft hesabı olan neredeyse her şey Microsoft cihazlar ve hizmetler ile yapmak için kullandığınız hesaptır. Bu bir e-posta adresi ve Skype, Outlook.com, OneDrive, Windows phone, Azure ve Xbox Live oturum açmak için kullandığınız parola olur. Tek bir hesap, dosyaları, fotoğraflar, kişiler ve ayarları, tüm cihazlardan izleyebilirsiniz anlamına gelir.
 
> [!NOTE]
> Bir Microsoft hesabı bir Windows Live ID çağrılması için kullanılan

### <a name="why-should-i-use-azure-devtest-labs"></a>Azure DevTest Labs neden kullanmalıyım?
Azure DevTest Labs, takımınızın zamandan ve paradan tasarruf edebilirsiniz. Geliştiriciler, çeşitli farklı tabanlarını kullanarak kendi ortamlarını oluşturabilir. Bunlar ayrıca, yapıtları hızla dağıtın ve uygulamaları yapılandırmak için kullanabilirsiniz. Özel görüntüleri ve formülleri kullanarak sanal makineleri (VM'ler) şablon olarak kaydedin ve kolayca takım arasında yeniden oluşturun. DevTest Labs, yöneticiler israfı azaltın ve bir takımın ortamlarını yönetmek için kullanabilir, Laboratuvar ayrıca çeşitli yapılandırılabilir ilkeler sunar. Bu ilkeler, otomatik kapatma, maliyet eşik, kullanıcı ve en fazla VM boyutu başına en fazla VM içerir. DevTest Labs hakkında daha ayrıntılı açıklama için bkz: [genel bakış](devtest-lab-overview.md) veya [tanıtım videosunu](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs).

### <a name="what-does-worry-free-self-service-mean"></a>"Sorunsuz Self-Servis" ne demektir?
Sorunsuz Self Servis, geliştiricilere ve test edicilere kendi ortamlarını gerektiğinde oluşturmak anlamına gelir. Yöneticiler, DevTest Labs uygun erişimi ayarlayın, israfı en aza indirmek ve maliyetleri denetim yardımcı olduğunu bilmesinin güvenliğe sahip. Yöneticiler, en fazla VM sayısı, hangi VM boyutlarının izin verilir ve VM'ler çalışmaya ve kapatma zaman belirtebilirsiniz. DevTest Labs ayrıca nasıl Laboratuvar kaynaklarını kullanıldığını haberdar olmanıza yardımcı olmak için uyarılar ayarlayın ve maliyetleri izleme kolaylaştırır.

### <a name="how-can-i-use-devtest-labs"></a>DevTest Labs nasıl kullanabilirim?
DevTest Labs, geliştirme gerektirir veya test ortamları ve hızla yeniden oluşturmak veya maliyet tasarrufu ilkeleri kullanarak yönetmek istediğiniz zaman yararlıdır.
Müşterilerimiz için DevTest Labs kullanan bazı senaryolar aşağıda verilmiştir:

- Geliştirme yönetme ve test ortamları tek bir yerde. Maliyetleri azaltmak ve paylaşmak için özel görüntü oluşturma için ilkeleri kullanma takım arasında oluşturur.
- Geliştirme aşamaları boyunca disk durumunu kaydetmek için özel görüntüleri kullanarak bir uygulama geliştirin.
- Maliyet ilerleme durumu ile ilgili olarak izleyin.
- Kalite güvencesi test etmek için yığın test ortamları oluşturun.
- Yapıtlar ve formülleri kolayca yapılandırmak ve çeşitli ortamlarda bir uygulama oluşturmak için kullanın.
- Sonları gerçekleştirilen HACK (işbirliğine dayalı geliştirme veya test çalışma) için sanal makineleri dağıtmak ve olay sona erdiğinde daha sonra kolayca bunları sağlamasını kaldırma.

### <a name="how-am-i-billed-for-devtest-labs"></a>DevTest Labs için nasıl faturalandırılırım?
DevTest Labs ücretsiz bir hizmettir. Laboratuvarları oluşturma ve ilkeleri, şablonlar ve yapıtlar DevTest Labs'de yapılandırma ücretsizdir. Yalnızca VM'ler, depolama hesapları ve sanal ağlar gibi laboratuvarlarınızı kullanılan Azure kaynakları için ödeme yaparsınız. Laboratuvar kaynaklarını maliyeti hakkında daha fazla bilgi için bkz. [Azure DevTest Labs fiyatlandırma](https://azure.microsoft.com/pricing/details/devtest-lab/).

## <a name="security"></a>Güvenlik

### <a name="what-are-the-different-security-levels-in-devtest-labs"></a>Farklı güvenlik düzeylerine DevTest Labs nedir?
Güvenlik erişim, rol tabanlı Access Control tarafından (RBAC) belirlenir. Erişimi nasıl çalıştığını öğrenmek için bu izin, bir rolü olan ve bir kapsam arasındaki farkları öğrenmek için RBAC tarafından tanımlandığı şekilde yardımcı olur.

- **İzni**: Bir izin, belirli bir eylem için tanımlanmış bir erişimdir. Örneğin, bir izin, tüm sanal makineler için okuma erişimi olabilir.
- **Rol**: Bir rol, gruplandırılmış ve bir kullanıcıya atanmış izinler kümesidir. Örneğin, bir abonelik sahibi rolüne sahip bir kullanıcı bir Abonelikteki tüm kaynaklara erişebilir.
- **Kapsam**: Bir kapsam, bir Azure kaynak hiyerarşi içinde düzeyidir. Örneğin, bir kapsam bir kaynak grubu, tek bir laboratuvar veya tüm abonelik olabilir.

DevTest Labs kapsamında, kullanıcı izinlerini tanımlamak rolleri iki tür vardır:

- **Laboratuvar sahibi**: Laboratuvar sahibi Laboratuvardaki tüm kaynaklara erişebilir. Laboratuvar sahibi ilkeleri değiştirebilir, okuma ve yazma için herhangi bir VM, sanal ağı değiştirin ve benzeri.
- **Laboratuvar kullanıcı**: Bir laboratuvar kullanıcı Vm'leri, ilkeleri ve sanal ağlar gibi tüm Laboratuvar kaynaklarını görüntüleyebilir. Ancak, Laboratuvar kullanıcı ilkeleri veya diğer kullanıcılar tarafından oluşturulmuş tüm Vm'leri değiştiremezsiniz.

Ayrıca, DevTest Labs'de özel roller oluşturabilirsiniz. DevTest Labs'de özel roller oluşturma konusunda bilgi almak için bkz: [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Kapsamları hiyerarşik olduğundan, bir kullanıcı belirli bir kapsamda izinlere sahip olduğunda kullanıcı kapsamı içindeki her alt düzey kapsamda Bu izinler otomatik olarak verilir. Örneği için abonelik sahibi rolüne atanmış bir kullanıcı, kullanıcı bir Abonelikteki tüm kaynaklara erişim izni vardır. Bu kaynaklar, VM'ler, sanal ağlar ve Laboratuvarları içerir. Abonelik sahibi, Laboratuvar sahibi rolünü otomatik olarak devralır. Ancak, bunun tersi doğru değildir. Laboratuvar sahibi abonelik düzeyinden daha düşük bir kapsam bir laboratuvar erişebilir. Bu nedenle, Laboratuvar sahibi Vm'leri, sanal ağlar veya Laboratuvar dışında olan diğer kaynakları göremez.

### <a name="how-do-i-define-role-based-access-control-for-my-devtest-labs-environments-to-ensure-that-it-can-govern-while-developerstest-can-do-their-work"></a>Ne miyim tanımlamak rol tabanlı erişim denetimi geliştiriciler ve test çalışmalarını yapabilseniz benim için DevTest Labs, BT'nin emin olmak için ortamlar yöneten?
Kuruluşunuzu ayrıntı bağlıdır ancak geniş bir düzeni vardır.

Merkezi BT ve yalnızca gerekli olanla kendi gerekli düzeyde bir denetiminiz proje ve uygulama ekiplerin olanak sağlar. Genellikle, bu Orta anlamına gelir BT aboneliğin sahibi ve tanıtıcıları çekirdek ağ yapılandırmaları gibi BT işlevleri. Dizi **sahipleri** abonelik küçük olmalıdır. Bu sahipleri, ihtiyaç olduğunda ek sahipleri aday gösterin veya örneğin "ortak IP" abonelik düzeyinde ilkeler uygulayabilirsiniz.

Tier1 veya Katman 2 destek gibi bir abonelik üzerinden erişim gerektiren kullanıcılar bir alt kümesi olabilir. Bu durumda, bu kullanıcılar size öneririz **katkıda bulunan** kullanıcıların kaynaklarını yönetmek, ancak değil kullanıcı erişim sağlamak veya ilkeleri ayarlama böylece erişim.

DevTest Labs kaynak proje/uygulama ekibine yakın çeken sahipler tarafından sahip olunan. Bunlar makine ve gerekli yazılım gereksinimlerini anlama olmasıdır. Birçok kuruluşta bu DevTest Labs kaynak yaygın olarak proje/Geliştirme lideri sahibidir. Bu sahibi kullanıcı ve laboratuvar ortamında ilkelerini yönetebilir ve DevTest Labs ortamdaki tüm sanal makineleri yönetebilir.

Proje/uygulama takım üyeleri eklenmelidir **DevTest Labs kullanıcılar** rol. Bu kullanıcılar, sanal makineler (satır içi Laboratuvar ve abonelik düzeyinde ilkeleri) oluşturabilir. Bunlar, kendi sanal makinelerini de yönetebilirsiniz. Bunlar, diğer kullanıcılara ait sanal makineleri yönetemez.

Daha fazla bilgi için [Azure Kurumsal iskelesi: öngörücü abonelik İdaresi belgeleri](/azure/architecture/cloud-adoption/appendix/azure-scaffold).


### <a name="how-do-i-create-a-role-to-allow-users-to-do-a-specific-task"></a>Kullanıcıların belirli bir görevi gerçekleştirmek bir rolü nasıl oluşturulur?
Özel roller oluşturma ve izinleri için rol atama hakkında kapsamlı bir makale için bkz: [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). Bir rol oluşturur, bir komut dosyası örneği aşağıdadır **DevTest Labs kullanıcısı Gelişmiş**, tüm Vm'leri Laboratuvardaki durdurmak ve başlatmak iznine sahip:


```powershell
$policyRoleDef = Get-AzRoleDefinition "DevTest Labs User"
$policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
$policyRoleDef.Id = $null
$policyRoleDef.Name = "DevTest Labs Advanced User"
$policyRoleDef.IsCustom = $true
$policyRoleDef.AssignableScopes.Clear()
$policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
$policyRoleDef = New-AzRoleDefinition -Role $policyRoleDef  
```

### <a name="how-can-an-organization-ensure-corporate-security-policies-are-in-place"></a>Bir kuruluş Kurumsal güvenlik ilkeleri karşılandığından nasıl sağlayabilirsiniz?
Bir kuruluş, aşağıdaki eylemleri gerçekleştirerek elde edebilirsiniz:

- Geliştirme ve kapsamlı güvenlik ilkesi yayımlama. Kuralları kullanmayla ilişkili kabul edilebilir kullanım ilkesi korumadaki yazılımı, bulut varlıklar. Ayrıca, hangi açıkça ilkeyi ihlal tanımlar.
- Özel bir görüntü, özel yapıtlar ve active directory ile tanımlanan güvenlik bölgesi içinde düzenleme izin veren bir dağıtım işlemi geliştirin. Bu yaklaşım şirket sınırında zorlar ve ortak bir ortam denetimleri kümesini belirler. Geliştirin ve bunların genel işleminin bir parçası güvenli geliştirme yaşam döngüsü izleyin bu denetimleri ortama yönelik bir geliştirici olarak düşünebilirsiniz. Hedefi de aşırı kısıtlayıcı olmayan bir ortamda, Mayıs sağlamaktır geliştirme ancak makul bir dizi denetimi azaltabilir. Laboratuvar sanal makinesi içeren kuruluş biriminde (OU) grup ilkeleri, üretimde bulunan toplam grup ilkelerinin bir alt kümesi olabilir. Alternatif olarak, bunlar düzgün şekilde tanımlanan tüm riskleri azaltmak için ek bir kümesi olabilir.

### <a name="how-can-an-organization-ensure-data-integrity-to-ensure-that-remoting-developers-cant-remove-code-or-introduce-malware-or-unapproved-software"></a>Bir kuruluş, uzak geliştiriciler kodu kaldıramaz veya kötü amaçlı yazılımlardan veya onaylanmamış tanıtmak emin olmak için veri bütünlüğü nasıl sağlayabilirsiniz?
Denetimin dış danışmanların, yükleniciler ya da DevTest Labs'de işbirliği yapmak için uzaktan iletişim'de olan çalışanlar tehdidi azaltmak için birden fazla katman vardır.

İlk adım, daha önce belirtildiği gibi birisi ilkeyi ihlal ettiğinde açıkça sonuçları özetler drafted ve tanımlanmış bir kabul edilebilir kullanım ilkesi olması gerekir.

Uzaktan erişim için denetimlerin ilk katman laboratuvara doğrudan bağlı değil bir VPN bağlantısı üzerinden uzaktan erişim ilkesi uygulamaktır.

İkinci Katman denetimlerin kopyalama önlemek ve Uzak Masaüstü aracılığıyla yapıştırın, Grup İlkesi nesneleri kümesini uygulamaktır. Ağ İlkesi, FTP gibi ortamından giden Hizmetleri ve ortamın dışında RDP Hizmetleri izin vermeyecek şekilde uygulanabilir. Kullanıcı tanımlı yönlendirme tüm Azure ağ trafiğini yeniden şirket içine zorlayabilir, ancak üretim içerik ve oturumları taramak için bir ara sunucu denetlenen sürece veri yüklemeye izin verebilir tüm URL'ler için hesabı alınamadı. Genel IP'ler destekleyen bir dış ağ kaynağı köprüleme izin vermeyecek şekilde DevTest Labs sanal ağ içinde kısıtlı olabilir.

Sonuç olarak, aynı tür kısıtlamaları için tüm olası yöntemlerin çıkarılabilir medya hesabı kuruluş, veya bir gönderi içeriği kabul edebilecek URL'lere arasında uygulanması gerekiyor. Gözden geçirin ve bir güvenlik ilkesi uygulamak için profesyonel güvenlik ile başvurun. Daha fazla öneri için bkz. [Microsoft siber güvenlik](https://www.microsoft.com/security/default.aspx?&WT.srch=1&wt.mc_id=AID623240_SEM_sNYnsZDs).

## <a name="lab-configuration"></a>Laboratuvar yapılandırma

### <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Bir Resource Manager şablonundan bir laboratuvarı nasıl oluşturabilirim?
Sunuyoruz bir [Laboratuvar Azure Resource Manager şablonları GitHub deposunda](https://azure.microsoft.com/resources/templates/101-dtl-create-lab) olarak dağıtabileceğiniz- ya da laboratuvarlarınızı için özel şablonlar oluşturmak için değiştirin. Her şablon, laboratuvarı kendi Azure aboneliğinde olduğu gibi dağıtmak için bir bağlantı vardır. Veya şablonu özelleştirebilirsiniz ve [PowerShell veya Azure CLI kullanarak dağıtma](../azure-resource-manager/resource-group-template-deploy.md).


### <a name="can-i-have-all-virtual-machines-to-be-created-in-a-common-resource-group-instead-having-each-machine-in-its-own-resource-group"></a>Bunun yerine, kendi kaynak grubunda her makineye sahip ortak bir kaynak grubunda oluşturulan tüm sanal makinelerin sahip olabilir miyim? 
Evet, bir laboratuvar sahibi olarak işlemek için kaynak grubu ayırma Laboratuvar ya da izin verebilir veya tüm sanal makineler ortak bir kaynak grubunda belirttiğiniz oluşturduğunuz. 

Farklı bir kaynak grubu senaryosu:
-   DevTest Labs, hızla çalıştırın her ortak/özel IP sanal makinesi için yeni bir kaynak grubu oluşturur.
-   DevTest Labs, aynı boyutta ait paylaşılan IP makineler için bir kaynak grubu oluşturur.

Ortak kaynak grubu senaryosu:
-   Tüm sanal makineler çalışmaya belirttiğiniz ortak kaynak grubunda başlar. Daha fazla bilgi edinin [Laboratuvar için kaynak grubu ayırma](https://aka.ms/RGControl). 

### <a name="how-do-i-maintain-a-naming-convention-across-my-devtest-labs-environment"></a>DevTest Labs ortamımın nasıl bir adlandırma kuralı korunsun mu?
Azure işlemleri için geçerli Kurumsal adlandırma kuralları'nı genişletin ve DevTest Labs'i ortam genelinde tutarlı hale getirmek isteyebilirsiniz. DevTest Labs dağıtırken, belirli bir başlangıç ilkeleri sahip olmasını öneririz. Merkezi bir betiği ve JSON şablonları tutarlılığı zorlamak için bu ilkeleri dağıtırsınız. Abonelik düzeyinde mi uygulanacağına Azure ilkeleri aracılığıyla adlandırma ilkeleri uygulanabilir. Azure İlkesi JSON örneği için bkz. [Azure ilkesi örnekleri](../governance/policy/samples/index.md).

### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Aynı abonelik altında kaç labs oluşturabilirim?
Abonelik başına oluşturulabilecek labs sayısı belirli bir sınırı yoktur. Ancak, abonelik başına kullanılan kaynakları miktarı sınırlıdır. Okuyabilirsiniz [limitler ve kotalar Azure aboneliklerinin](../azure-subscription-service-limits.md) ve [bu sınırları artırmak nasıl](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).


### <a name="how-many-vms-can-i-create-per-lab"></a>Laboratuvar sanal makine sayısı oluşturabilirim?
Laboratuvar oluşturulan VM'ler sayısı belirli bir sınır yoktur. Ancak, abonelik başına kullanılan kaynakların (sanal makine çekirdeklerine, genel IP adresleri ve benzeri) sınırlıdır. Okuyabilirsiniz [limitler ve kotalar Azure aboneliklerinin](../azure-subscription-service-limits.md) ve [bu sınırları artırmak nasıl](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

### <a name="how-do-i-determine-the-ratio-of-users-per-lab-and-the-overall-number-of-labs-that-are-needed-across-an-organization"></a>Laboratuvar ve bir kuruluş genelindeki gerekli labs genel sayısı başına kullanıcı sayısının oranı nasıl belirlerim?
İş birimleri ve aynı geliştirme proje ile ilişkili olan geliştirme gruplar aynı lab ile ilişkili olduğunu öneririz. Her iki grubuna uygulanacak ilkeler, görüntüleri ve kapatma ilkeleri aynı türde sağlar.

Coğrafi sınırlar dikkate almanız gerekebilir. Örneğin, geliştiricilerin Kuzey doğu ABD (ABD), Doğu ABD 2 sağlanan bir laboratuvar kullanabilir. Ve bir kaynak Güney Orta ABD kullanmak için Texas, Dallas, Denver Colorado geliştiriciler yönlendirilebilir. Dış bir üçüncü taraf ile ortak çabaya ise iç geliştiriciler tarafından kullanılmayan bir laboratuvar atanabilir.

Azure DevOps projeleri içinde belirli bir proje için bir laboratuvar de kullanabilirsiniz. Ardından, güvenlik kaynakları her iki kümesine erişim sağlayan belirtilen Azure Active Directory grup olarak uygulanır. Laboratuvara atanan sanal ağ, kullanıcılar birleştirmek için başka bir sınır olabilir.

### <a name="how-can-we-prevent-the-deletion-of-resources-within-a-lab"></a>Nasıl biz bir laboratuvar içindeki kaynakların silinmesini engelleyebilir miyim?
Böylece yalnızca yetkili kullanıcıların kaynakları silmeniz veya Laboratuvar ilkeleri değiştirme Laboratuvar düzeyinde uygun izinleri ayarlamanızı öneririz. Geliştiriciler, içinde yerleştirilmelidir **DevTest Labs kullanıcılar** grubu. Baş geliştirici ya da altyapı sağlama olmalıdır **DevTest Labs sahibi**. Yalnızca iki Laboratuvar sahibi olması önerilir. Bu ilke bozulmalarını önlemek için kod deposu genişletir. Laboratuvar kullanıcıları kaynakları kullanma haklarına sahip ancak Laboratuvar ilkeleri güncelleştirilemiyor. Rolleri ve yerleşik her grup Laboratuvar sahip hakları listeleyen şu makaleye bakın: [Azure DevTest Labs'de sahibini ve kullanıcıları ekleme](devtest-lab-add-devtest-user.md).

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Doğrudan bağlantısını benim laboratuvara nasıl paylaşırım?

1. İçinde [Azure portalında](https://portal.azure.com)Laboratuvar gidin.
2. Kopyalama **Laboratuvar URL** tarayıcınızdan ve Laboratuvar Kullanıcılarınızla paylaşabilirsiniz.

> [!NOTE]
> Bir laboratuvar kullanıcı bir Microsoft hesabı kullanan, ancak kuruluşunuzun Active Directory örneğine üye değil bir dış kullanıcı ise, kullanıcının paylaşılan bağlantı erişmeye çalıştıklarında bir hata iletisi görebilirsiniz. Bir dış kullanıcı bir hata iletisi görürse, adlarının ilk Azure portalının sağ üst köşedeki seçmesini isteyin. Ardından, menü Directory bölümünde Laboratuvar bulunduğu dizine kullanıcı seçebilirsiniz.

## <a name="virtual-machines"></a>Sanal makineler

### <a name="why-cant-i-see-vms-on-the-virtual-machines-page-that-i-see-in-devtest-labs"></a>DevTest Labs'de bkz. sanal makineler sayfasındaki sanal makineleri neden göremiyorum?
DevTest Labs'de bir VM oluşturduğunuzda, size bu VM'ye erişmesine izin verilir. Labs sayfasında hem üzerinde VM görüntüleyebileceğiniz **sanal makineler** sayfası. Atanan kullanıcılar **DevTest Labs sahibi** rol Laboratuvar Laboratuvar içinde oluşturulmuş tüm Vm'leri görebilirsiniz **tüm sanal makineler** sayfası. Ancak, sahip kullanıcılar **DevTest Labs kullanıcısı** rolü değil otomatik olarak verilir diğer kullanıcıların oluşturduğu VM kaynaklarına okuma erişimi. Bu nedenle, bu sanal makineler üzerinde görüntülenmez **sanal makineler** sayfası.


### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Nasıl birden çok VM aynı şablondan tek seferde oluşturabilirim?
Aynı anda birden çok VM aynı şablonu oluşturmak için iki seçeneğiniz vardır:

- Kullanabileceğiniz [Azure DevOps görev uzantımızı](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).
- Yapabilecekleriniz [Resource Manager şablonu oluşturma](devtest-lab-add-vm.md#save-azure-resource-manager-template) bir VM oluştururken ve [Windows PowerShell Resource Manager şablonu dağıtmayı](../azure-resource-manager/resource-group-template-deploy.md).
- Sanal makine oluşturma işlemi sırasında oluşturulacak bir makine birden fazla örneğini belirtebilirsiniz. Sanal makineler birden çok örneğini oluşturma hakkında daha fazla bilgi edinmek için ilgili belge bakın [Laboratuvar sanal makinesi oluşturma](devtest-lab-add-vm.md).

### <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>Mevcut Azure Vm'lerimi my DevTest Labs Laboratuvar nasıl taşırım?
DevTest Labs mevcut Vm'lerinizi kopyalamak için:

1.  Mevcut sanal makinenizin VHD dosyasını kullanarak kopyalama bir [Windows PowerShell Betiği](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/Scripts/CopyVirtualMachines/CopyAzVHDFromVMToLab.ps1).
2.  Oluşturma [özel görüntü](devtest-lab-create-template.md) DevTest Labs Laboratuvarınızı içinde.
3.  Laboratuvarda özel görüntü bir VM oluşturun.

### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Birden çok disk planlamadığım Vm'lerime ne ekleyebilir miyim?

Evet, sanal makineleriniz için birden fazla disk ekleyebilirsiniz.

### <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Test için bir Windows işletim sistemi görüntüsü kullanmak isterseniz bir MSDN aboneliği satın almanız gerekir mi?
Windows istemci işletim sistemi görüntüleri (Windows 7 veya sonraki bir sürümü), geliştirme veya Azure'da test kullanmak için aşağıdaki adımlardan birini uygulayın:

- [Bir MSDN aboneliği satın alma](https://www.visualstudio.com/products/how-to-buy-vs).
- Bir Kurumsal Sözleşme yaptıysanız, bir Azure aboneliği ile oluşturma [Kurumsal geliştirme ve Test teklifi](https://azure.microsoft.com/offers/ms-azr-0148p).

Her MSDN teklifi için Azure KREDİLERİ hakkında daha fazla bilgi için bkz. [Visual Studio aboneleri için aylık Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).


### <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>My Laboratuvardaki tüm Vm'leri silme işleminin nasıl otomatikleştirebilirim?
Laboratuvar sahibi olarak, Vm'leri Azure portalında Laboratuvarınızı öğesinden silebilirsiniz. Ayrıca, tüm VM'lerin bir PowerShell Betiği kullanarak laboratuvarınızda silebilirsiniz. Aşağıdaki örnekte, altında **değiştirmek için değerleri** açıklama, parametre değerlerini değiştirin. Azure portalında Laboratuvar bölmesinden Subscriptionıd labResourceGroup ve labName değerleri alabilir.

```powershell
# Delete all the VMs in a lab.

# Values to change:
$subscriptionId = "<Enter Azure subscription ID here>"
$labResourceGroup = "<Enter lab's resource group here>"
$labName = "<Enter lab name here>"

# Sign in to your Azure account.
Connect-AzAccount

# Select the Azure subscription that has the lab. This step is optional
# if you have only one subscription.
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab that has the VMs that you want to delete.
$lab = Get-AzResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get the VMs from that lab.
$labVMs = Get-AzResource | Where-Object {
          $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
          $_.Name -like "$($lab.Name)/*"}

# Delete the VMs.
foreach($labVM in $labVMs)
{
    Remove-AzResource -ResourceId $labVM.ResourceId -Force
}
```

## <a name="environments"></a>Ortamlar 

### <a name="how-can-i-use-resource-manager-templates-in-my-devtest-labs-environment"></a>DevTest Labs Ortamımın Resource Manager şablonlarını nasıl kullanabilirim?
Bölümünde anlatılan adımları kullanarak DevTest Labs ortamına Resource Manager şablonlarınızı dağıtmak [ortamları özellik DevTest labs'deki](devtest-lab-test-env.md) makalesi. Aslında, Resource Manager şablonlarınızı bir Git deposuna (Azure depoları veya GitHub) denetleyin ve ekleme bir [şablonlarınızın özel depoya](devtest-lab-test-env.md) Laboratuvar için. Bu senaryo, konak geliştirme makineler için DevTest Labs kullanıyorsanız, faydalı olmayabilir ancak üretim temsilcisi bir hazırlık ortamı oluşturuyorsanız yararlı olabilir.

Ayrıca, kaç kullanıcı seçeneği veya Laboratuvar başına sanal makineler yalnızca Laboratuvar ve tüm ortamları (Resource Manager şablonlarını) tarafından yerel olarak oluşturulan sanal makinelerde sayısını sınırlayan hatalarının ayıklanabileceğini belirtmekte yarar.

## <a name="custom-images"></a>Özel Görüntüler

### <a name="how-can-i-set-up-an-easily-repeatable-process-to-bring-my-custom-organizational-images-into-a-devtest-labs-environment"></a>DevTest Labs ortamına özel kuruluş görüntülerim getirmek için bir kolayca yinelenebilir işlemini nasıl ayarlayabilirim?
Bkz. Bu [video görüntü Fabrika desenini](https://sec.ch9.ms/ch9/8e8a/9ea0b8d4-b803-4f23-bca4-4808d9368e8a/dtlimagefactory_mid.mp4). Bu senaryo Gelişmiş bir senaryodur ve sağlanan örnek betikler yalnızca betiklerdir. Herhangi bir değişikliğe ihtiyaç olup, yönetmek ve ortamınızda kullanılan komut dosyaları korumak gerekir.

Bir görüntü fabrikası oluşturma hakkında ayrıntılı bilgi için bkz. [Azure DevTest Labs'de bir özel görüntü fabrikası oluşturma](image-factory-create.md). 

### <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Özel bir görüntü ile bir formül arasındaki fark nedir?
Özel bir görüntü, yönetilen bir görüntü hizmetidir. Formül, ek ayarlarla yapılandırın ve ardından kaydedebilir ve yeniden bir görüntüsüdür. Özel bir görüntü, aynı temel, sabit görüntüsünü kullanarak çeşitli ortamları hızlıca oluşturmak istiyorsanız tercih edilebilir. Formül, bir sanal ağın veya alt ağın bir parçası olarak veya belirli bir boyutta bir VM ile en son parçaları, sanal Makinenizin yapılandırmasını yeniden oluşturmak istiyorsanız daha iyi olabilir. Daha ayrıntılı bir açıklama için bkz. [özel görüntüleri ve formülleri DevTest labs'deki karşılaştırma](devtest-lab-comparing-vm-base-image-types.md).

### <a name="when-should-i-use-a-formula-vs-custom-image"></a>Özel görüntü veya formül ne zaman kullanmalıyım?
Genellikle, bu senaryoda faktör, ekonomik ve yeniden kullanabilirsiniz. Burada birçok kullanıcıları/labs temel görüntünün üstüne yazılım çok sayıda görüntü gerektiren bir senaryo varsa, özel bir görüntü oluşturarak maliyetleri azaltabilir. Bu görüntü bir kez oluşturulduktan anlamına gelir. Bu sanal makine ve Kurulum meydana geldiğinde çalışan sanal makinenin nedeniyle sonucunda oluşan maliyet hazırlık süresini azaltır.

Ancak, ek bir faktörü unutmayın yazılım değişiklikleri sıklığıdır. Çalıştırırsanız günlük oluşturur ve kullanıcılarınızın sanal makinelerde gereken yazılım bir formül yerine özel bir görüntü kullanarak geçirmeniz gerekir.

Daha ayrıntılı bir açıklama için bkz. [özel görüntüleri ve formülleri karşılaştırma](devtest-lab-comparing-vm-base-image-types.md) DevTest labs'deki.

### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Özel görüntüleri oluşturmak için VHD dosyalarını karşıya yükleme işleminin nasıl otomatikleştirebilirim?

Özel görüntüleri oluşturmak için VHD dosyalarını karşıya yükleme otomatikleştirmek için iki seçeneğiniz vardır:

- Kullanım [AzCopy](../storage/common/storage-use-azcopy.md#upload-blobs-to-blob-storage) kopyalayın veya lab ile ilişkili depolama hesabına VHD dosyalarını yükleyin.
- Kullanım [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md). Depolama Gezgini Windows, OS X ve Linux'ta çalışan tek başına bir uygulamadır.

Laboratuvarınızı ile ilişkili hedef depolama hesabını bulmak için:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Sol menüden **kaynak grupları**.
3.  Bulun ve Laboratuvarınızı ile ilişkili kaynak grubunu seçin.
4.  Altında **genel bakış**, depolama hesaplarından birini seçin.
5.  **Bloblar**'ı seçin.
6.  Karşıya yükleme listesinde arayın. Yoksa, 4. adıma geri dönün ve başka bir depolama hesabı deneyin.
7.  Kullanım **URL** , AzCopy komutunda hedef olarak.

Azure Market görüntüsü kendi özel kuruluş görüntü veya ne zaman kullanmalıyım?

### <a name="when-should-i-use-an-azure-marketplace-image-vs-my-own-custom-organizational-image"></a>Azure Market görüntüsü kendi özel kuruluş görüntü veya ne zaman kullanmalıyım?
Azure Market, belirli kaygıları veya kuruluş gereksinimlerine sahip olmadığınız sürece varsayılan olarak kullanılmalıdır. Ortak örnek verilebilir;

- Bir uygulamayı temel görüntüsü bir parçası olarak dahil olacak şekilde gerektiren karmaşık yazılım kurulumu.
- Yükleme ve Kurulum bir uygulamanın Azure Market görüntüsü üzerinde eklenecek işlem süresi verimli bir kullanım olmayan kaç saat sürebilir.
- Geliştiriciler ve test ediciler bir sanal makineyi hızlı bir şekilde erişmesi ve yeni bir sanal makine hazırlık süresini en aza indirmek istiyorsunuz.
- Uyumluluk veya tüm makineler için yapılması gerekenleri yasal koşullar (örneğin, güvenlik ilkeleri).
- Özel görüntüleri kullanarak hafifçe kabul olmamalıdır. Bunlar, artık VHD dosyalarını bu temel alınan temel görüntüleri için gereken yönettiğiniz fazladan karmaşıklık tanıtır. Düzenli olarak bu temel görüntüleri yazılım güncelleştirmeleri ile düzeltmeniz gerekir. Bu güncelleştirmeler, yeni işletim sistemi (OS) güncelleştirmeler ve güncelleştirmeleri ya da yazılım paketi kendisi için gerekli yapılandırma değişikliklerini içerir.

## <a name="artifacts"></a>Yapıtlar

### <a name="what-are-artifacts"></a>Yapıtları nelerdir?
Yapıtlar, en yeni güncelleştirmeleri dağıtmak veya bir VM'ye geliştirme araçlarınızı dağıtmak için kullanabileceğiniz özelleştirilebilir öğeleridir. Sanal Makineyi oluştururken sanal makinenizde yapıtları ekleyin. VM sağlandıktan sonra yapıtları dağıtın ve sanal makinenize yapılandırın. Önceden mevcut olan çeşitli yapıtları kullanılabilir bizim [ortak GitHub deposu](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts). Ayrıca [kendi yapıtları Yazar](devtest-lab-artifact-author.md).

### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>My yapıt VM oluşturma sırasında başarısız oldu. Bunu nasıl giderebilirim?
Başarısız bir yapıt için günlükleri alma hakkında bilgi için bkz: [DevTest labs'deki yapıt hatalarını tanılama nasıl](devtest-lab-troubleshoot-artifact-failure.md).

### <a name="when-should-an-organization-use-a-public-artifact-repository-vs-private-artifact-repository-in-devtest-labs"></a>Ne zaman bir kuruluş, genel yapıt deposuna DevTest Labs özel yapıt deposunda karşılaştırması kullanmalıdır?
[Genel yapıt deposuna](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) en yaygın olarak kullanılan yazılım paketleri başlangıç kümesi sağlar. Bu hızlı dağıtım ile zaman yatırım yapmanıza gerek kalmadan yaygın geliştirici araçları ve eklentileri oluşturmaya yardımcı olur. Kendi özel depoya dağıtmayı tercih edebilirsiniz. Genel ve özel bir depo paralel kullanabilirsiniz. Genel deponun devre dışı bırakmak seçebilirsiniz. Özel bir depo dağıtmak için ölçütleri hakkında önemli noktalar ve aşağıdaki soruları dikkate alınmalıdır:

- Kuruluşun kurumsal lisanslı yazılımı kendi DevTest Labs teklifi kapsamında olan bir gereksinimi var mı? Yanıt Evet ise, özel bir depo oluşturulmalıdır.
- Kuruluş genel sağlama sürecinin bir parçası olarak gerekli olan belirli bir işlemi sağlayan özel yazılım geliştirmek mu? Yanıt Evet ise, özel bir depo dağıtılmalıdır.
- Kuruluşunuzun idare İlkesi yalıtım gerektirir ve dış depolardaki kuruluş doğrudan yapılandırma yönetimi altında olmayan, bir özel yapıt deposu dağıtılmalıdır. Bu işlemin bir parçası, genel deponun bir başlangıç kopyasını kopyalanır ve özel depoya ile tümleşiktir. Böylece kimse kuruluş içinde artık erişebilir sonra genel deponun devre dışı bırakılabilir. Bu yaklaşım, kuruluş kapsamındaki tüm kullanıcılara yalnızca bir kuruluş tarafından onaylanan tek depo olmasını zorlar ve yapılandırma değişikliklerini en aza indirin.


### <a name="should-an-organization-plan-for-a-single-repository-or-allow-multiple-repositories"></a>Bir kuruluş için tek bir depoda plan veya birden çok deposu izin?
Kuruluşunuzun genel idare ve yapılandırma yönetimi stratejisi kapsamında, merkezi bir havuz kullanmanızı öneririz. Birden çok deposu kullandığınızda, yönetilmeyen yazılım siloları zamanla durdurabilir. Merkezi bir depo ile birden çok takımı yapıtlar bu depodan kendi projeleri için kullanabilir. Standartlaştırma, güvenlik, yönetim kolaylığı zorlar ve çalışmalarınızı çoğaltma ortadan kaldırır. Merkezi bir parçası olarak, aşağıdaki eylemleri uygulamalar için uzun vadeli yönetimi ve sürdürülebilirlik önerilir:

- Azure aboneliği kimlik doğrulama ve yetkilendirme için kullandığı aynı Azure Active Directory kiracısı ile Azure depoları ilişkilendirin.
- Adlı bir grup oluşturun `All DevTest Labs Developers` merkezi olarak yönetilen Azure Active Directory'de. Yapıt katkıda herhangi bir geliştirici bu gruba yerleştirilmelidir.
- Aynı Azure Active Directory grubu, Azure depoları depoya ve Laboratuvar erişim sağlamak için kullanılabilir.
- Azure depolarda dallanma veya çatal ayrı bir de geliştirme depoya birincil üretim depodan kullanılmalıdır. İçerik yalnızca ana dala bir çekme isteği ile bir uygun kod gözden geçirdikten sonra eklenir. Kod Gözden Geçiren, değişikliği onayladığında, ana dalın bakım için sorumlu olan, müşteri adayı geliştirici, güncelleştirilmiş kod birleştirir.

## <a name="cicd-integration"></a>CI/CD tümleştirmesi

### <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>DevTest Labs, my CI/CD araç zinciri ile tümleştiriliyor mu?
Azure DevOps kullanıyorsanız, kullanabileceğiniz bir [DevTest Labs görevlerini uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) DevTest labs'deki yayın işlem hattınızı otomatik hale getirmek için. Bu uzantı ile gerçekleştirebileceğiniz görevlerden bazıları şunlardır:

- Oluşturun ve otomatik olarak bir VM dağıtın. VM en son sürümle Azure dosya kopyalama veya PowerShell Azure DevOps Hizmetleri görevleri kullanarak da yapılandırabilirsiniz.
- Daha fazla bilgi için aynı VM'de bir hatayı yeniden oluşturmak için test edildikten sonra otomatik olarak bir VM'nin durumunu yakalayın.
- Sürüm ardışık düzeninin sonunda sanal makine artık gerekli değilse silin.

Aşağıdaki blog teklif rehberlik ve Azure DevOps Hizmetleri Uzantısı kullanma hakkında bilgi gönderir:

- [DevTest Labs ve Azure DevOps uzantısı](integrate-environments-devops-pipeline.md)
- [Azure DevOps Services'dan bir DevTest Labs Laboratuvardaki yeni VM dağıtma](https://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
- [DevTest Labs'de sürekli dağıtımlar için Azure DevOps Services sürüm Yönetimi'ni kullanma](https://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Diğer sürekli tümleştirme (CI) için / sürekli teslim (CD) araç zincirlerinden, aynı senaryoları elde edebileceğiniz dağıtma [Azure Resource Manager şablonları](https://azure.microsoft.com/resources/templates/) kullanarak [Azure PowerShell cmdlet'lerini](../azure-resource-manager/resource-group-template-deploy.md) ve [.NET SDK'ları](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Ayrıca [için DevTest Labs REST API'leri](https://aka.ms/dtlrestapis) araç zincirinizi ile tümleştirmek için.

## <a name="networking"></a>Ağ

### <a name="when-should-i-create-a-new-virtual-network-for-my-devtest-labs-environment-vs-using-an-existing-virtual-network"></a>Yeni bir sanal ağ ve var olan bir sanal ağ kullanarak DevTest Labs ortamım için ne zaman oluşturmalıyım?
Ardından, sanal makinelerinizin altyapınız ile etkileşim kurmak gerekiyorsa, DevTest Labs ortamınız içinde mevcut bir sanal ağ kullanmayı düşünün. ExpressRoute kullanıyorsanız, sanal ağlar en aza indirmek isteyebilirsiniz / alt ağlar Aboneliklerde kullanılmak üzere atanan, IP adres alanı parçalara böylece. 

Sanal ağ eşleme düzeni burada kullanmayı deneyin ([merkez-uç modeli](/architecture/reference-architectures/hybrid-networking/hub-spoke)) çok. Bu yaklaşım, abonelikler arasında vnet/alt ağ iletişimi sağlar. Aksi takdirde, DevTest Labs her ortamın kendi sanal ağı olabilir. 

Vardır [sınırları](../azure-subscription-service-limits.md) abonelik başına sanal ağ sayısı. Bu sınır 100 olarak oluşturulabilir olsa varsayılan süreyi 50 ' dir.

### <a name="when-should-i-use-a-shared-ip-vs-public-ip-vs-private-ip"></a>Özel IP ve genel IP ile paylaşılan bir IP ne zaman kullanmalıyım?
 
Bir siteden siteye VPN veya Express Route kullandığınız makinelerinizi genel internet üzerinden erişilebilir, iç ağ aracılığıyla ve erişilemez olacak şekilde özel IP'ler kullanmayı düşünün.

> [!NOTE]
> Laboratuvar sahibi yanlışlıkla, VM'ler için genel IP adresleri hiç oluşturmasını sağlamak için bu alt ağ ilkesi değiştirebilirsiniz. Abonelik sahibi, genel IP'ler oluşturulmasını önleyen bir abonelik İlkesi oluşturmanız gerekir.

Paylaşılan genel IP'ler kullanırken bir laboratuvarındaki sanal makinelerde bir genel IP adresi paylaşın. Bu yaklaşım, belirli bir aboneliği için genel IP adresleri barındırabileceğiniz ihlal önlemek gerektiğinde yararlı olabilir.

### <a name="how-do-i-ensure-that-development-and-test-virtual-machines-are-unable-to-reach-the-public-internet-are-there-any-recommended-patterns-to-set-up-network-configuration"></a>Nasıl sağlamak, geliştirme ve test sanal makineleri genel internet ulaşamıyor? Ağ Yapılandırması'kurmak için herhangi bir önerilen Düzen var mı?

Evet. Dikkate alınması gereken – gelen ve giden trafiği iki unsur vardır.

- **Gelen trafik** – internet ulaşılamıyor sonra sanal makinenin genel IP adresi yoksa. Hiçbir kullanıcı, genel bir IP adresi oluşturabilir, bir abonelik düzeyi ilkesi ayarlanmış olduğundan emin olun. yaygın bir yaklaşımdır.
- **Giden trafik** – sanal makineler genel İnternet'e doğrudan erişmesini önlemek ve kurumsal bir güvenlik duvarı aracılığıyla trafiği zorlamak istiyorsanız sonra şirket içi trafiği expressroute üzerinden yönlendirebilir veya VPN kullanarak zorlamalı yönlendirme.

> [!NOTE]
> Proxy ayarları olmadan trafiği engelleyen bir proxy sunucunuz varsa, özel durumlar için laboratuvar yapıt depolama hesabı eklemek unutmayın.

Ağ güvenlik grupları, sanal makine ya da alt ağlar için de kullanabilirsiniz. Bu adım, ek bir izin ver / trafiği engellemek için koruma katmanı ekler.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Neden olmayan mevcut sanal ağ düzgün bir şekilde kaydediliyor?
Sanal ağ adı nokta içeren bir olasılıktır. Bu durumda, dönemleri kaldırarak veya bunları çizgilerle değiştirerek deneyin. Ardından, sanal ağ kaydetmeyi yeniden deneyin.

### <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>Ben bir VM'den PowerShell sağladığınızda neden "üst kaynağı bulunamadı" hatası alıyorum?
Bir kaynak başka bir kaynak için bir üst, alt kaynak oluşturmadan önce üst kaynak mevcut olması gerekir. Üst kaynak yoksa, gördüğünüz bir **ParentResourceNotFound** ileti. Üst kaynak üzerinde bir bağımlılık belirtmezseniz, alt kaynak önce üst dağıtılmış olabilir.

Sanal makineleri bir laboratuar ortamında bir kaynak grubu altında alt kaynaklardır. PowerShell kullanarak Vm'leri dağıtmak için Resource Manager şablonları kullandığınızda, PowerShell Betiği belirtilen kaynak grubu adı Laboratuvar tamamlandığında, kaynak grubu adı olmalıdır. Daha fazla bilgi için [yaygın Azure dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md).

### <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Bir VM dağıtımı başarısız olursa daha fazla hata bilgilerini nerede bulabilirim?
Sanal makine dağıtım hataları, etkinlik günlüklerinde yakalanır. Laboratuvar VM etkinlik günlükleri altında bulabilirsiniz **denetim günlükleri** veya **sanal makine tanılama** Laboratuvar VM sayfasında kaynak menüsündeki (VM'den seçtikten sonra sayfanın görünür sanal makineler listem).

Bazı durumlarda, VM dağıtımı başlamadan önce dağıtım hatası oluşur. Sanal makine ile oluşturulan bir kaynak için abonelik sınırı aşıldığında bir örnektir. Bu durumda, hata ayrıntılarını Laboratuvar düzeyinde etkinlik günlüğünde yakalanır. Etkinlik günlükleri alt kısmında bulunan **yapılandırması ve ilkelerini** ayarları. Azure'da günlüklerini etkinlik kullanma hakkında daha fazla bilgi için bkz: [kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/resource-group-audit.md).




