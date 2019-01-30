---
title: Anahtar özellikleri ve Azure stack'teki kavramları | Microsoft Docs
description: Temel özellikler ve Azure stack'teki kavramlar hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 09ca32b7-0e81-4a27-a6cc-0ba90441d097
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: b07d8b115b966b9decdfa7379a908da4f9f2ee74
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243918"
---
# <a name="key-features-and-concepts-in-azure-stack"></a>Temel özellikler ve kavramlar Azure Stack'te
Microsoft Azure Stack için yeniyseniz, bu hüküm ve özellik açıklamaları faydalı olabilir.

## <a name="personas"></a>Kişilikler
Kullanıcıların Microsoft Azure Stack, işleci ve kullanıcı için iki çeşit vardır.

* Azure Stack **işleci** Azure Stack yönetme tekliflerini, planları, hizmetler, kotalar ve kullanıcıların kendi Kiracı için kaynak sağlamak için fiyatlandırma yapılandırabilirsiniz. İşleçler kapasitesini yönetmek ve uyarılarını yanıtlama.  
* Azure Stack **kullanıcı** (Kiracı olarak da bilinir) işleci sağlayan hizmetlerini kullanır. Kullanıcılar sağlama, izleme ve bunlar, web uygulamaları, depolama ve sanal makineler gibi abone olduğunuz Hizmetleri yönetin.

## <a name="portal"></a>Portal
Microsoft Azure Stack ile etkileşim kurmanın birincil Yönetim Portalı, kullanıcı portalı ve PowerShell yöntemlerdir.

Azure Stack portalı her ayrı örnekleri, Azure Resource Manager tarafından desteklenir. Operatör Yönetim Portalı, Azure Stack yönetmek ve Kiracı sunumları oluşturma gibi işlemler yapmak için kullanır. (Kiracı portalı olarak da bilinir) kullanıcı portalı için sanal makineler, depolama hesapları ve web apps gibi bulut kaynaklarını kullanım bir Self Servis deneyimi sağlar. Daha fazla bilgi için [Azure Stack yönetici ve Kullanıcı Portalı'nı kullanarak](azure-stack-manage-portals.md).

## <a name="identity"></a>Kimlik 
Azure Stack, kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanır.  

### <a name="azure-active-directory"></a>Azure Active Directory
Azure AD, Microsoft'un bulut tabanlı, çok kiracılı kimlik sağlayıcıdır. Karma senaryo, Azure AD kimlik deposu olarak kullanırsınız.

### <a name="active-directory-federation-services"></a>Active Directory Federasyon Hizmetleri
Azure Stack'i bağlantısız dağıtımları için Active Directory Federasyon Hizmetleri (AD FS) kullanmayı tercih edebilirsiniz. Azure AD ile yaptıkları gibi azure Stack kaynak sağlayıcıları ve diğer uygulamalar çok AD FS ile aynı şekilde çalışır. Azure Stack, kendi Active Directory örneğine ve bir Active Directory Graph API'sini içerir. Azure Stack geliştirme Seti'ni aşağıdaki AD FS senaryoları destekler:

- AD FS kullanarak dağıtım için oturum açın.
- Gizli anahtar Kasası'nda bir sanal makine oluşturun
- Gizli dizileri depolamak/erişmek için bir kasa oluşturun
- Özel bir RBAC rollerini aboneliği oluşturabilir.
- Kaynaklar RBAC rolleri için kullanıcıları atama
- Azure PowerShell aracılığıyla sistem genelinde RBAC rolleri oluşturma
- Azure PowerShell aracılığıyla bir kullanıcı olarak oturum açın
- Hizmet oluşturma ilkeleri için Azure PowerShell oturum açmak için bunları kullanın


## <a name="regions-services-plans-offers-and-subscriptions"></a>Bölgeler, hizmetleri, planlar, teklifler ve abonelikler
Azure Stack'te hizmetler, bölgeler, abonelikler, teklifleri ve planları kullanarak kiracılara teslim edilir. Kiracılar için birden çok teklife abone olabilir. Bir veya daha fazla plan Teklife sahip olabilirsiniz ve bir veya daha fazla hizmet planları olabilir.

![](media/azure-stack-key-features/image4.png)

Örnek hiyerarşisini teklifleri, değişen her bir kiracının abonelik planları ve Hizmetleri.

### <a name="regions"></a>Bölgeler
Azure Stack bölgeleri ölçeklendirme ve yönetim temel bir öğe var. Bir kuruluş, her bölgede birden çok bölgede bulunan kaynaklara sahip olabilir. Bölgeler, farklı hizmet teklifleri de olabilir. Azure Stack geliştirme Seti'ni, yalnızca tek bir bölgede desteklenen ve otomatik olarak adlandırılır *yerel*.

### <a name="services"></a>Hizmetler
Microsoft Azure Stack, çok çeşitli Hizmetleri ve sanal makineler, SQL Server veritabanları, SharePoint, Exchange ve daha fazlası gibi uygulamalar sunmak sağlayıcılarının sağlar.

### <a name="plans"></a>Planlar
Bir veya daha fazla hizmet gruplandırmalarını planları oluşturulabilir. Bir sağlayıcı, kiracılarınıza sunabileceğiniz planlar oluşturun. Böylece kiracılarınız tekliflerinize abone olarak, bu tekliflerin içerdiği planları ve hizmetleri kullanabilir.

Bir plana eklenen her bir hizmet, bulut kapasitesini yönetmenize yardımcı olmak için kota ayarları ile yapılandırılabilir. Kotalar, VM, RAM ve CPU sınırları gibi kısıtlamalara içerebilir ve kullanıcı aboneliği uygulanır. Kotalar konuma göre ayırt edilebilir. Örneğin, bir bölgeden işlem hizmetlerini içeren bir plan iki sanal makine, 4 GB RAM ve 10 CPU çekirdek kotası olabilir.

Bir teklifi oluştururken, Hizmet Yöneticisi dahil edebilirsiniz bir **temel plan**. Bir kiracı söz konusu teklife abone olduğunda, bu temel planlar varsayılan olarak dahil edilir. Bir kullanıcı abone (ve abonelik oluşturdunuz,), kullanıcı bu temel planlar (ile ilgili kotalar) belirtilen tüm kaynak sağlayıcıları erişimi vardır.

Hizmet Yöneticisi de içerebilir **eklenti planları** içinde bir teklif. Eklenti planı varsayılan abonelik olarak dahil edilmez. Eklenti planları, ek planları (kotalar) abonelikleri için abonelik sahibi ekleyen bir teklifi bulunan oluşturulabilir.

### <a name="offers"></a>Teklifler
Teklifleri kiracılara satın almak için sağlayıcıları sunan bir veya daha fazla planları gruplarıdır (abone olmaları). Örneğin, alfa teklif planı A içerebilir içeren bir dizi işlem Hizmetleri ve depolama ve ağ hizmetleri kümesi içeren B planlayın.

Bir teklif bir temel plan kümesi ile birlikte gelir ve hizmet yöneticileri kiracılar aboneliğine ekleyebilmeniz için eklenti planları oluşturabilirsiniz.

### <a name="subscriptions"></a>Abonelikler
Kiracılar tekliflerinizi nasıl satın bir aboneliktir. Bir abonelik teklif kiracıyla birleşimidir. Bir kiracı birden çok teklife abone olabilir. Her abonelik yalnızca bir teklif için geçerlidir. Bir kiracının abonelik planları/Hizmetleri erişebilmek belirleyin.

Abonelikleri düzenleme ve bulut kaynaklarına ve hizmetlerine erişmek sağlayıcıları yardımcı olur.

Yöneticisi, dağıtım sırasında varsayılan sağlayıcı aboneliği oluşturulur. Bu abonelik, Azure Stack yönetmek, daha fazla kaynak sağlayıcıları dağıtmak ve kiracılar için planlar ve teklifler oluşturmak için kullanılabilir. Müşteri iş yüklerini ve uygulamaları çalıştırmak için kullanılmamalıdır. 1804 sürümünden başlayarak, iki ek abonelikler varsayılan sağlayıcı aboneliği tamamlar; bir ölçüm abonelik ve tüketim abonelik. Çekirdek altyapısı, ek kaynak sağlayıcıları ve iş yükleri Yönetimi ayırarak bu eklemeler kolaylaştırır.  

## <a name="azure-resource-manager"></a>Azure Resource Manager
Azure Resource Manager'ı kullanarak, şablona dayalı, bildirim temelli bir modelde altyapı kaynakları çalışabilir.   Bu, dağıtmak ve çözüm bileşenlerinizi yönetmek için kullanabileceğiniz tek bir arabirim sağlar. Tüm bilgiler ve yönergeler için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).

### <a name="resource-groups"></a>Kaynak grupları
Kaynak grupları olan kaynakları, hizmetleri ve uygulamaları koleksiyonları — ve her kaynak sanal makineler, sanal ağlar, genel IP'ler, depolama hesapları ve Web siteleri gibi bir türü vardır. Her kaynak bir kaynak grubu ve kaynak gruplarını mantıksal olarak yardımcı olmak için kaynaklar gibi iş yükü veya konuma göre düzenlemek gerekir. Azure Stack'te planlar ve teklifler gibi kaynakları kaynak gruplarına ayrıca yönetilir.

Farklı [Azure](../azure-resource-manager/resource-group-move-resources.md), Azure Stack kaynakların kaynak grupları arasında taşıyamazsınız. Azure Stack Yönetim Portalı'nda bir kaynağa veya kaynak grubu özelliklerini görüntülediğinizde *taşıma* düğmesidir grileştirilmiş ve kullanılamaz. Ayrıca, kullanımını **kaynak grubunu değiştir** veya **değiştirme abonelik** eylemleri kaynak grubu veya kaynak grubu öğesi özelliklerini de desteklenmez. Tüm çalıştı, işlemleri başarısız olacaktır taşıyın.
 
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Azure Resource Manager ile dağıtım ve uygulamanızı yapılandırmayı tanımlayan bir şablon (JSON biçiminde) oluşturabilirsiniz. Bu şablon, bir Azure Resource Manager şablonu olarak bilinir ve dağıtımı tanımlamanın bildirim temelli bir yöntemini sağlar. Bir şablon kullanarak uygulamanızı yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

## <a name="resource-providers-rps"></a>Kaynak sağlayıcıları (Rp'ler)
Kaynak sağlayıcıları için Azure tabanlı tüm Iaas temelini web hizmetleri ve PaaS Hizmetleri altındadır. Azure Resource Manager hizmetine erişim sağlamak için farklı RPs kullanır.

Dört temel RPs vardır: Ağ, depolama, işlem ve anahtar kasası. Her biri bu RPs yapılandırın ve ilgili kaynaklarını denetlemenize yardımcı olur. Hizmet yöneticileri, yeni özel kaynak sağlayıcılarını da ekleyebilirsiniz.

### <a name="compute-rp"></a>RP işlem
Azure Stack kiracıların kendi sanal makinelerini oluşturmak işlem kaynak sağlayıcısı (CRP) sağlar. CRP, sanal makine uzantıları yanı sıra, sanal makineler oluşturma özelliği içerir. Sanal makine uzantısı hizmeti, Iaas özelliklerini sağlamak için Windows ve Linux sanal makineleri yardımcı olur.  Örnek olarak, CRP'nin bir Linux sanal makinesi sağlama ve VM'yi yapılandırmak için dağıtım sırasında Bash betikleri çalıştırmak için kullanabilirsiniz.

### <a name="network-rp"></a>Ağ RP
Yazılım tanımlı ağ (SDN) ve ağ işlevi sanallaştırma (NFV) özelliklerini özel bulut için bir dizi ağ kaynak Sağlayıcısı'nı (NRP) sunar.  NRP, yazılım yük dengeleyici, genel IP'ler, ağ güvenlik grupları, sanal ağlar gibi kaynakları oluşturmak için kullanabilirsiniz.

### <a name="storage-rp"></a>RP depolama
Depolama RP dört tutarlı Azure depolama hizmetleri sunar: blob, tablo, kuyruk ve hesap yönetimi. Ayrıca, tutarlı Azure depolama hizmetleri hizmet sağlayıcısı yönetimini kolaylaştırmak için bir depolama bulut yönetim hizmeti sunar. Azure depolama, depolama ve büyük miktarlardaki yapılandırılmamış veriler, belgeler ve medya dosyalarını Azure BLOB'ları ile gibi alma esnekliği sağlar ve Azure tabloları verilerle yapılandırılmış NoSQL tabanlı. Azure depolama hakkında daha fazla bilgi için bkz. [Microsoft Azure Storage'a giriş](../storage/common/storage-introduction.md).

#### <a name="blob-storage"></a>Blob depolama
BLOB Depolama, herhangi bir veri kümesi depolar. Blob; bir belge, ortam dosyası veya uygulama yükleyici gibi herhangi bir türde metin veya ikili veri olabilir. Tablo depolama, yapılandırılmış veri kümelerini depolar. Table Storage, yüksek miktarda verinin hızla dağıtılmasını ve verilere hızla erişilebilmesini sağlayan NoSQL anahtar özniteliği veri deposudur. Kuyruk depolama, iş akışı işlemeye ve bulut hizmetlerinin bileşenleri arasında iletişime yönelik güvenilir Mesajlaşma sağlar.

Her blob kapsayıcısı altında düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamaya ilişkin kullanışlı bir yöntem sunar. Bir depolama hesabında herhangi bir sayıda kapsayıcı olabilir ve bir kapsayıcı herhangi bir sayıda depolama hesabının 500 TB kapasite sınırını dolduracak kadar BLOB içerebilir. Blob Storage blok blobları, ekleme blobları ve sayfa blobları (diskler) olmak üzere üç türde blob sunar. Blok blobları bulut nesnelerinin akış ve depolanması için en iyi duruma getirilmiştir ve belge, ortam dosyaları ve yedekler vb. öğelerin depolanması için uygun bir seçenektir. Ekleme blobları blok bloblarına benzer ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bir ekleme blobu yalnızca sonuna yeni bir blok eklenerek güncelleştirilebilir. Ekleme blobları, yeni verilerin yalnızca blobun sonuna yazılması gereken günlüğe kaydetme gibi senaryolar için iyi bir seçenektir. Sayfa blobları Iaas disklerini temsil etmek için optimize edilmiş ve rastgele destekleyen yazar ve 1 TB'ye kadar olabilir. IaaS diskine bağlı bir Azure Virtual Machine ağı, sayfa blobu olarak kaydedilen bir VHD’dir.

#### <a name="table-storage"></a>Table Storage
Tablo depolama, Microsoft'un NoSQL anahtar/öznitelik deposu – olmadan şemaları, geleneksel ilişkisel veritabanlarından farklı bir tasarıma sahiptir. Bu yana olmaması şemaları verilerini depolayan ihtiyaçları, uygulama geliştikçe verilerinizi uyarlamak da kolaylaşır. Table Storage’ın kullanımı son derece kolaydır, böylece geliştiriciler uygulamalarını hızla geliştirebilir. Table Storage bir anahtar öznitelik deposudur; bu, bir tablodaki her değerin türü belirtilmiş bir özellik adıyla depolandığı anlamına gelir. Özellik adı filtreleme ve seçim kriterlerinin belirlenmesi için kullanılabilir. Özellik ve değerlerinin toplamı bir varlığı oluşturur. Tablo depolama olmaması şemalarını,'den itibaren iki varlık aynı tablodaki farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türde olabilir. Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak üzere Table Storage’ı kullanabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

#### <a name="queue-storage"></a>Kuyruk Depolama
Azure Queue depolama birimi, uygulama bileşenleri arasında bulut mesajlaşma özelliği sağlar. Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler.

### <a name="keyvault"></a>KeyVault
KeyVault RP, yönetimi ve parolalar ve sertifikalar gibi gizli denetlenmesini sağlar. Örneğin, bir kiracı yönetici parola veya anahtarlarını VM dağıtımı sırasında sağlamak için KeyVault RP kullanabilirsiniz.

## <a name="high-availability-for-azure-stack"></a>Azure Stack için yüksek kullanılabilirlik
Bir çoklu VM üretim sisteminin azure'da yüksek kullanılabilirlik elde etmek için Vm'leri yerleştirilir bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) , bunları birden çok hata etki alanları ve güncelleme etki alanları arasında yayılır. Azure Stack daha küçük ölçek hata etki alanı bir kullanılabilirlik kümesinde tek bir düğüm ölçek birimi olarak tanımlanır.  

Azure Stack altyapısını zaten hatalara karşı dayanıklı olsa da, bir donanım hatası olursa (Yük Devretme Kümelemesi) temel alınan teknoloji hala bazı kapalı kalma süresi VM'ler için etkilenen bir fiziksel sunucuda artmasına neden olur. Azure Stack, Azure ile tutarlı olacak şekilde en fazla üç hata etki alanı ile bir kullanılabilirlik sahip destekler.

- **Hata etki alanları**. Vm'leri bir kullanılabilirlik kümesine yerleştirilir bunları mümkün olduğunca eşit olarak birden çok hata etki alanları üzerinde (Azure Stack düğüm) yayarak birbirinden fiziksel olarak izole edilmiş olur. Bir donanım hatası varsa, başarısız hata etki alanı Vm'lerden diğer hata etki alanları yeniden, ancak, mümkün olduğunda, aynı kullanılabilirlik kümesindeki diğer vm'lerden ayrı hata etki alanlarında tutulur. Donanım tekrar çevrimiçi olduğunda, yüksek kullanılabilirliği sürdürmek için Vm'leri yeniden Dengelenecek. 
 
- **Güncelleme etki alanları**. Güncelleştirme etki alanlarını kullanılabilirlik kümelerinde yüksek kullanılabilirlik sağlayan başka bir Azure kavramdır. Bir güncelleme etki alanı, aynı anda bakımdan geçirilebilen temel alınan donanım mantıksal grubudur. Aynı güncelleştirme etki alanında bulunan VM'ler, planlanan bakım sırasında birlikte yeniden başlatılır. Kiracılar, bir kullanılabilirlik kümesinde VM'ler oluşturduğunuzda, Azure platformu otomatik olarak Vm'leri bunlar arasında dağıtır güncelleştirme etki alanı. Azure Stack'te kendi temel konak güncelleştirilmeden önce kümedeki çevrimiçi diğer konaklar arasında geçişi, Vm'leri Canlı. Bir konak güncelleştirme sırasında kapalı kalma süresi olmadan Kiracı olduğundan, Azure Stack'te güncelleştirme etki alanı özelliği yalnızca şablon Azure ile uyumluluk için bulunmaktadır. 

## <a name="role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC)
Sistem yetkili kullanıcılar, gruplar ve Hizmetleri için bir abonelik, kaynak grubu veya tek başına bir kaynak düzeyinde rolleri atayarak erişim için RBAC kullanabilirsiniz. Her bir rol, bir kullanıcı, Grup veya hizmet Microsoft Azure Stack kaynaklara sahip erişim düzeyini tanımlar.

Azure RBAC, tüm kaynak türleri için geçerli olan üç temel rolüne sahiptir: Sahip, katkıda bulunan ve okuyucu. Sahip tüm kaynaklara temsilci erişimi başkalarına hakkı dahil olmak üzere tam erişimi vardır. Katkıda bulunan oluşturabilir ve tüm Azure kaynakları türlerini yönetmek, ancak diğerleri için erişim izni veremiyor. Okuyucu, mevcut Azure kaynakları yalnızca görüntüleyebilir. Azure RBAC rolleri kalan belirli bir Azure kaynak yönetimi sağlar. Örneğin, sanal makine Katılımcısı rolü oluşturma ve sanal makinelerin yönetimini sağlar ancak Yönetim sanal ağ veya sanal makine bağlanan bir alt ağ izin vermiyor.

## <a name="usage-data"></a>Kullanım verileri
Microsoft Azure Stack toplar ve tüm kaynak sağlayıcılarını kullanım verilerini toplayan ve bunu Azure'a işleme için Azure ticaret tarafından iletir. Azure Stack'te toplanan kullanım verileri, bir REST API aracılığıyla görüntülenebilir. Bir Azure ile tutarlı Kiracı API'si yanı sıra sağlayıcısı ve sağlayıcı API'leri temsilci tüm Kiracı aboneliklerindeki kullanım verilerini almak için yoktur. Bu veriler, bir dış aracı ya da hizmet için fatura veya geri ödeme ile tümleştirmek için kullanılabilir. Azure ticaret tarafından kullanım işlendikten sonra Azure fatura Portalı'nda görüntülenebilir.

## <a name="next-steps"></a>Sonraki adımlar
[Yönetim temel bilgileri](azure-stack-manage-basics.md)

