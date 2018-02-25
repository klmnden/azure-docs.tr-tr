---
title: "Anahtar özellikleri ve Azure yığınında kavramları | Microsoft Docs"
description: "Anahtar özellikleri ve Azure yığınında kavramları hakkında bilgi edinin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 09ca32b7-0e81-4a27-a6cc-0ba90441d097
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 6c02ec42874e4e3221c53e6d6e85378bbe2e414a
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="key-features-and-concepts-in-azure-stack"></a>Anahtar özellikleri ve Azure yığınında kavramları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Microsoft Azure yığın yeniyseniz, bu hüküm ve özellik açıklamalarını faydalı olabilir.

## <a name="personas"></a>Kişiler
Kullanıcı Microsoft Azure yığın bulut işleci (sağlayıcısı) ve Kiracı (tüketici) iki çeşit vardır.

* A **bulut operatörü** Azure yığın yapılandırabilir ve teklifler, planları, hizmetleri, kotalar ve fiyatlandırma kiracıları için kaynakları sağlamak üzere yönetebilirsiniz.  Bulut operatörleri ayrıca kapasite yönetme ve uyarılara yanıt.  
* A **Kiracı** (bir kullanıcı olarak da bilinir) bulut yöneticisine sunar Hizmetleri kullanır. Kiracılar sağlamak, izlemek ve bunlar, Web uygulamaları, depolama ve sanal makineler gibi abone olduğunuz hizmetleri yönetme.

## <a name="portal"></a>Portal
Microsoft Azure yığın ile etkileşim, birincil Yönetici portalı, kullanıcı portalı ve PowerShell yöntemleridir.

Azure yığın portalları her ayrı örnekleri Azure Kaynak Yöneticisi'nin tarafından desteklenir.  Bir bulut operatörü, Azure yığın yönetmek ve Kiracı tekliflerini oluşturmak gibi şeyler için Yönetici portalı'nı kullanır.  (Kiracı portalı olarak da bilinir) Kullanıcı Portalı sanal makineler, depolama hesapları ve Web uygulamaları gibi bulut kaynaklarının kullanımını bir Self Servis deneyimi sağlar. Daha fazla bilgi için bkz: [Azure yığın yönetici ve kullanıcı portalı kullanarak](azure-stack-manage-portals.md).

## <a name="identity"></a>Kimlik 
Azure yığını, Azure Active Directory (AAD) veya Active Directory Federasyon Hizmetleri (AD FS) bir kimlik sağlayıcısı kullanır.  

### <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory, Microsoft'un bulut tabanlı, çok kiracılı kimlik sağlayıcısıdır.  Çoğu karma senaryolar Azure Active Directory kimlik deposu olarak kullanın.

### <a name="active-directory-federation-services"></a>Active Directory Federasyon Hizmetleri
Azure yığınının bağlantısı kesilmiş dağıtımlar için Active Directory Federasyon Hizmetleri (AD FS) kullanmayı seçebilirsiniz.  Azure Active Directory ile olduğu gibi azure yığını, kaynak sağlayıcıları ve diğer uygulamalar AD FS ile benzer şekilde çalışır. Azure yığın kendi AD FS ve Active Directory örneği ve bir Active Directory grafik API'si içerir. Azure yığın Geliştirme Seti aşağıdaki AD FS senaryoları destekler:

- AD FS kullanarak dağıtım için oturum açın.
- Anahtar kasasına gizli anahtarları ile bir sanal makine oluşturun
- Parolaları depolamak/erişmek için bir kasa oluşturun
- Abonelikte özel RBAC roller oluşturma
- Kaynakları RBAC rollerinde kullanıcılar atayın
- Sistem genelinde RBAC rolleri Azure PowerShell aracılığıyla oluşturma
- Azure PowerShell aracılığıyla bir kullanıcı olarak oturum açın
- Hizmet oluşturma ilkeleri için Azure PowerShell oturum açmak için bunları kullanın


## <a name="regions-services-plans-offers-and-subscriptions"></a>Bölgeler, hizmetleri, planları, teklifleri ve abonelikleri
Azure yığınında Hizmetleri bölgeler, abonelikler, teklifleri ve planları kullanarak kiracılara teslim edilir. Kiracılar için birden çok teklifleri abone olabilir. Bir veya daha fazla plan teklifleri olabilir ve bir veya daha fazla hizmet planları olabilir.

![](media/azure-stack-key-features/image4.png)

Örnek hiyerarşisini teklifleri, değişen her bir kiracının abonelik planları ve Hizmetleri.

### <a name="regions"></a>Bölgeler
Azure yığın bölgeleri, ölçek ve yönetim temel öğesidir. Bir kuruluş, her bölgede kullanılabilir kaynaklar ile birden çok bölgeye olabilir. Bölgeler farklı hizmet teklifleri kullanılabilir da sahip olabilirsiniz. Azure yığın Geliştirme Seti, yalnızca tek bir bölge desteklenir ve otomatik olarak adlı *yerel*.

### <a name="services"></a>Hizmetler
Microsoft Azure yığın çok çeşitli hizmetler ve uygulamalar, sanal makineler, SQL Server veritabanları, SharePoint, Exchange ve daha fazlası gibi sunmak sağlayıcılarının sağlar.

### <a name="plans"></a>Planlar
Bir veya daha fazla hizmet gruplandırmaları planlarının. Bir sağlayıcısı olarak, kiracılarınıza sunmak için planları oluşturun. Böylece kiracılarınız tekliflerinize abone olarak, bu tekliflerin içerdiği planları ve hizmetleri kullanabilir.

Bir plana eklediğiniz her bir hizmet, bulut kapasitesi yönetmenize yardımcı olmak için kota ayarları ile yapılandırılabilir. Kotalar VM, RAM ve CPU sınırları gibi kısıtlamaları içerebilir ve kullanıcı abonelik başına uygulanır. Kotalar konuma göre ayırt. Örneğin, A bölgesinden işlem hizmetleri içeren bir planı iki sanal makine, 4 GB RAM ve 10 CPU çekirdek kotası olabilir.

Bir teklifi oluştururken, Hizmet Yöneticisi dahil edebileceğiniz bir **temel plan**. Bu teklif için bir kiracı abone olduğunda bu temel planları varsayılan olarak dahil edilir. Bir kullanıcı abone olur (ve abonelik oluşturulan olduğunda), kullanıcı bu temel planlarıyla (karşılık gelen kotaları) içinde belirtilen tüm kaynak sağlayıcıları erişebilir.

Hizmet Yöneticisi da içerebilir **eklenti planları** bir teklifte. Eklenti planları Abonelikteki varsayılan olarak dahil edilmez. Eklenti, abonelik sahibi kendi abonelikleri ekleyebilir bir teklif bulunan ek planlar (kotaları) planlarının.

### <a name="offers"></a>Teklifler
Teklifleri satın almak için kiracıların sağlayıcılardan bir veya daha fazla plan bir gruplarıdır (abone). Örneğin, teklif alfa planı A içerebilir içeren bir dizi işlem Hizmetleri ve depolama ve ağ hizmetleri kümesini içeren B planlayın.

Temel plan bir dizi teklif gelir ve hizmet yöneticileri, kiracılar aboneliğine ekleyebileceği eklenti planları oluşturabilirsiniz.

### <a name="subscriptions"></a>Abonelikler
Bir abonelik nasıl kiracılar, teklifleri satın ' dir. Abonelik, bir kiracı teklif ile birleşimidir. Bir kiracı birden çok teklifleri abone olabilir. Her abonelik için yalnızca bir teklif geçerlidir. Bir kiracının abonelikleri erişebilecekleri planları/Hizmetleri belirleyin.

Abonelikler, düzenlemek ve bulut kaynaklarına ve hizmetlerine erişmek sağlayıcıları yardımcı olur.

Yöneticisi, dağıtım sırasında bir varsayılan sağlayıcı abonelik oluşturulur. Bu abonelik Azure yığın yönetmek, daha fazla kaynak sağlayıcıları dağıtmak ve kiracılar için planlar ve teklifleri oluşturmak için kullanılabilir. Müşteri iş yüklerini ve uygulamaları çalıştırmak için kullanılmamalıdır. 

## <a name="azure-resource-manager"></a>Azure Resource Manager
Azure Kaynak Yöneticisi'ni kullanarak altyapı kaynaklarınızı şablona dayalı, bildirim temelli bir model ile çalışabilirsiniz.   Dağıtma ve çözüm bileşenlerini yönetmek için kullanabileceğiniz tek bir arabirim sağlar. Tam bilgi ve yönergeler için bkz: [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).

### <a name="resource-groups"></a>Kaynak grupları
Kaynak grupları olan kaynaklar, hizmetler ve uygulamalar koleksiyonları — ve her bir kaynağın sanal makineler, sanal ağlar, genel IP'ler, depolama hesapları ve Web siteleri gibi bir türe sahip. Her kaynak bir kaynak grubunda olması ve kaynak grupları mantıksal olarak inceleyeceğini kaynaklar gibi iş yükü veya konumu düzenlemek gerekir.  Microsoft Azure yığın içinde planları ve teklifleri gibi kaynakları kaynak gruplarına ayrıca yönetilir.
 
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Azure Resource Manager ile dağıtım ve uygulamanızın yapılandırmasını tanımlayan bir şablon (JSON biçiminde) oluşturabilirsiniz. Bu şablon, Azure Resource Manager şablonu olarak bilinir ve dağıtımı tanımlamanın bildirim temelli bir yolunu sağlar. Bir şablon kullanarak uygulamanızı yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

## <a name="resource-providers-rps"></a>Kaynak sağlayıcıları (RPs)
Kaynak sağlayıcıları için tüm Azure tabanlı Iaas temelini web hizmetleri ve PaaS Hizmetleri altındadır. Azure Resource Manager hizmetine erişim sağlamak için farklı RPs kullanır.

Dört temel RPs vardır: ağ, depolama, hesaplama ve KeyVault. Her bu RPs yapılandırmanıza ve ilgili kaynaklarını denetlemenize yardımcı olur. Hizmet yöneticileri, yeni özel kaynak sağlayıcıları da ekleyebilirsiniz.

### <a name="compute-rp"></a>RP işlem
İşlem kaynak sağlayıcısı (CRP) kendi sanal makineler oluşturmak Azure yığın kiracılar sağlar. CRP sanal makine uzantılarının yanı sıra sanal makineler oluşturma özelliğini içerir. Sanal makine uzantısı hizmeti, Windows ve Linux sanal makineleri için Iaas özelliklerini sağlamaya yardımcı olur.  Örnek olarak, bir Linux sanal makine sağlama ve VM yapılandırmak için dağıtım sırasında Bash betikleri çalıştırmak için CRP kullanabilirsiniz.

### <a name="network-rp"></a>Ağ RP
Ağ kaynak sağlayıcısı (NRP) bir dizi özel bulut için yazılım tanımlı ağ (SDN) ve ağ işlevi sanallaştırma (NFV) özellikleri sunar.  NRP, yazılım yük dengeleyici, genel IP'ler, ağ güvenlik grupları, sanal ağlar gibi kaynakları oluşturmak için kullanabilirsiniz.

### <a name="storage-rp"></a>Depolama RP
Depolama RP dört tutarlı Azure depolama hizmetleri sunar: blob, tablo, kuyruk ve hesap yönetimi. Ayrıca, Azure tutarlı depolama hizmetleri hizmet sağlayıcısı yönetimini kolaylaştırmak için bir depolama bulut Yönetimi hizmet sunar. Azure depolama depolamak ve büyük miktarlarda belgeler ve medya dosyalarını Azure BLOB'ları ile gibi yapılandırılmamış veri almak için esneklik sağlar ve yapılandırılmış NoSQL verileri Azure tablolar ile bağlı. Azure Storage hakkında daha fazla bilgi için bkz: [Microsoft Azure Storage'a giriş](../storage/common/storage-introduction.md).

#### <a name="blob-storage"></a>Blob depolama
BLOB storage herhangi bir veri kümesi depolar. Blob; bir belge, ortam dosyası veya uygulama yükleyici gibi herhangi bir türde metin veya ikili veri olabilir. Table storage yapılandırılmış veri kümelerini depolar. Table Storage, yüksek miktarda verinin hızla dağıtılmasını ve verilere hızla erişilebilmesini sağlayan NoSQL anahtar özniteliği veri deposudur. Kuyruk depolama, iş akışı işleme ve bulut Hizmetleri bileşenleri arasındaki iletişim için güvenilir Mesajlaşma sağlar.

Her blob bir kapsayıcı altında düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamaya ilişkin kullanışlı bir yöntem sunar. Bir depolama hesabının içerebileceği kapsayıcı sayısına ilişkin bir sınırlama yoktur. Bir kapsayıcı, depolama hesabının 500 TB kapasite sınırını dolduracak kadar blob içerebilir. Blob Storage blok blobları, ekleme blobları ve sayfa blobları (diskler) olmak üzere üç türde blob sunar. Blok blobları bulut nesnelerinin akış ve depolanması için en iyi duruma getirilmiştir ve belge, ortam dosyaları ve yedekler vb. öğelerin depolanması için uygun bir seçenektir. Ekleme blobları blok bloblarına benzer ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bir ekleme blobu yalnızca sonuna yeni bir blok eklenerek güncelleştirilebilir. Ekleme blobları, yeni verilerin yalnızca blobun sonuna yazılması gereken günlüğe kaydetme gibi senaryolar için iyi bir seçenektir. Sayfa blobları Iaas disklerini temsil etmek için en iyi duruma getirilir ve rastgele destekleme yazar ve 1 TB'ye kadar olabilir. IaaS diskine bağlı bir Azure Virtual Machine ağı, sayfa blobu olarak kaydedilen bir VHD’dir.

#### <a name="table-storage"></a>Table Storage
Table storage Microsoft'un NoSQL anahtar/öznitelik deposudur – şemaları, geleneksel ilişkisel veritabanlarından farklı yapmadan olmadan bir tasarıma sahiptir. Yetersiz şemaları verilerini depolayan olduğundan, uygulamanızın ihtiyaçları geliştikçe verilerinizi uyarlamak kolaydır. Table Storage’ın kullanımı son derece kolaydır, böylece geliştiriciler uygulamalarını hızla geliştirebilir. Table Storage bir anahtar öznitelik deposudur; bu, bir tablodaki her değerin türü belirtilmiş bir özellik adıyla depolandığı anlamına gelir. Özellik adı filtreleme ve seçim kriterlerinin belirlenmesi için kullanılabilir. Özellik ve değerlerinin toplamı bir varlığı oluşturur. Tablo depolama eksikliği şemalarını itibaren iki varlık aynı tablodaki farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türde olabilir. Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak üzere Table Storage’ı kullanabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

#### <a name="queue-storage"></a>Kuyruk Depolama
Azure Queue depolama birimi, uygulama bileşenleri arasında bulut mesajlaşma özelliği sağlar. Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler.

### <a name="keyvault"></a>KeyVault
KeyVault RP yönetimi ve parolaları ve sertifikaları gibi gizli denetlenmesini sağlar. Örnek olarak, bir kiracı yönetici parolalarını veya anahtarları VM dağıtımı sırasında sağlamak için KeyVault RP kullanın.

## <a name="role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC)
Abonelik, kaynak grubu veya tek başına bir kaynak düzeyinde rolleri atayarak yetkili kullanıcılar, gruplar ve hizmetlere sistem erişim vermek için RBAC kullanabilirsiniz. Her rol, bir kullanıcı, Grup veya hizmet Microsoft Azure yığın kaynaklara sahip erişim düzeyini tanımlar.

Azure RBAC sahip tüm kaynak türleri için geçerli üç temel roller: sahibi, katkıda bulunan ve okuyucu. Sahibi temsilci başkalarına erişimi hakkı dahil olmak üzere tüm kaynaklara tam erişimi vardır. Katkıda bulunan oluşturabilir ve tüm türlerini Azure kaynaklarını yönetmek ancak başkalarına erişim izni veremiyor. Okuyucu yalnızca var olan Azure kaynaklarını görüntüleyebilirsiniz. Azure RBAC rollerin geri kalanı belirli Azure kaynaklarının yönetimini sağlar. Örneğin, sanal makine katılımcı rolü oluşturma ve sanal makinelerin yönetimini sağlar ancak Yönetim sanal ağ veya sanal makine bağlandığı alt ağ izin vermiyor.

## <a name="usage-data"></a>Kullanım verileri
Microsoft Azure yığın toplar ve tüm kaynak sağlayıcıları kullanım verilerini toplayan ve onu Azure'a işleme için Azure commerce iletir. Azure yığında toplanan kullanım verileri bir REST API üzerinden görüntülenebilir. Bir Azure tutarlı Kiracı API'si yanı sıra sağlayıcısı ve sağlayıcı API'leri temsilci Kiracı abonelikler arasında kullanım verilerini almak için yoktur. Bu veriler, bir dış aracı veya faturalama veya geri ödeme hizmeti ile tümleştirmek için kullanılabilir. Kullanım Azure commerce işlendikten sonra Azure fatura Portalı'nda görüntülenebilir.

## <a name="in-development-build-of-azure-stack-development-kit"></a>Azure yığın geliştirme Seti'nin geliştirme, derleme
Geliştirme derlemeleri önce-Azure yığın Geliştirme Seti en son sürümünü değerlendirmek Benimseyenler olanak tanır. Artımlı derlemeler en son ana yayımını temel alan olup olmadıklarını. Ana sürüm birkaç ayda yayımlanacak devam ederken, geliştirme derlemeleri aralıklı önemli sürümler arasında serbest bırakır.

Geliştirme derlemeleri aşağıdaki avantajları sağlar:
- Hata düzeltmeleri
- Yeni Özellikler
- Diğer geliştirmeler

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın dağıtımının önkoşulları](azure-stack-deploy.md)

