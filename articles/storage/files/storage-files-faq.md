---
title: Sık sorulan sorular (SSS) Azure dosyaları için | Microsoft Docs
description: Azure dosyaları hakkında sık sorulan soruların yanıtlarını bulun.
services: storage
author: roygara
ms.service: storage
ms.date: 01/02/2019
ms.author: rogarana
ms.subservice: files
ms.topic: conceptual
ms.openlocfilehash: c32d9954b3c90a5f7e9c5475acdb141f7154cf76
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540352"
---
# <a name="frequently-asked-questions-faq-about-azure-files"></a>Azure dosyaları hakkında sık sorulan sorular (SSS)
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları endüstri standardı erişilebilen bulutta sunar [sunucu ileti bloğu (SMB) Protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx). Azure dosya paylaşımları Windows, Linux ve macOS Bulut veya şirket içi dağıtımlarda eşzamanlı olarak bağlayabilir. Ayrıca verilerin kullanıldığı yakın, hızlı erişim için Azure dosya eşitleme'ı kullanarak Azure dosya paylaşımları Windows Server makinelerinde önbelleğe alabilir.

Bu makalede, Azure dosyaları özellikleri ve işlevleri, Azure dosya eşitleme ile Azure dosyaları kullanımı dahil olmak üzere hakkında sık sorulan sorular yanıtlanmaktadır. Sorunuzun yanıtını görmüyorsanız, aşağıdaki kanalları (sırayla yükselen) üzerinden bize başvurabilirsiniz:

1. Bu makalede Açıklamalar bölümü.
2. [Azure depolama Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft desteği. Azure portalında yeni bir destek isteği oluşturmak için **yardımcı** sekmesinde **Yardım + Destek** düğmesini ve ardından **yeni destek isteği**.

## <a name="general"></a>Genel
* <a id="why-files-useful"></a>
  **Azure dosyaları faydalı oldu?**  
   Fiziksel sunucu, cihaz veya Gereci yükü yönetmekten sorumlu olmadan bulutta dosya paylaşımları oluşturmak için Azure dosyaları'nı kullanabilirsiniz. Ve sıkıcı iş işletim sistemi güncelleştirmelerini uygulama ve hatalı diskleri değiştirme dahil desteklemiyoruz. Azure dosyaları size yardımcı olabilecek senaryoları hakkında daha fazla bilgi için bkz: [Azure dosyaları neden yararlıdır](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>
  **Azure dosyaları'nda dosyalara erişmenin farklı yolları nelerdir?**  
    SMB 3.0 protokolünü kullanarak dosya paylaşımını yerel makinenize bağlayabilir veya Araçlar gibi kullanabileceğiniz [Depolama Gezgini](https://storageexplorer.com/) dosya paylaşımınızdaki dosyalara erişebilirsiniz. Uygulamanızdan Azure Dosya paylaşımındaki dosyalarınıza erişmek için depolama istemci kitaplıkları, REST API'ler, PowerShell veya Azure CLI'yı kullanabilirsiniz.

* <a id="what-is-afs"></a>
  **Azure dosya eşitleme nedir?**  
    Kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için Azure dosya eşitleme'yi kullanabilirsiniz. Azure dosya eşitleme, Windows Server makineleri Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, ağ dosya sistemi (NFS) ve Dosya Aktarımı Protokolü hizmet (FTPS) dahil olmak üzere verilerinizi yerel olarak erişmek için Windows Server üzerinde kullanılabilir olan herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gereken sayıda önbellek olabilir.

* <a id="files-versus-blobs"></a>
  **Verilerim için neden Azure Blob Depolama ve Azure dosya paylaşımını kullanmam gerekir?**  
    Azure dosyaları ve Azure Blob Depolama, her ikisi de sunar yolları büyük miktarda veriyi bulutta depolayabilir, ancak bunlar biraz farklı amaçlar için kullanışlıdır. 
    
    Azure Blob Depolama, yapılandırılmamış verileri depolamak için gereken çok büyük ölçekli, bulutta yerel uygulamalar için yararlıdır. Performans ve ölçek en üst düzeye çıkarmak için Azure Blob Depolama bir basit depolama daha doğru dosya sistemi soyutlamadır. Azure Blob Depolama REST tabanlı istemci kitaplıkları aracılığıyla yalnızca (veya doğrudan REST tabanlı protokolü aracılığıyla) erişebilir.

    Azure dosyaları, özellikle bir dosya sistemidir. Azure dosyaları, bildiğiniz ve sevdiğiniz şirket içi işletim sistemleri ile çalışmanın yıllara ait tüm dosya özetleri sahiptir. Azure Blob Depolama gibi Azure dosyaları REST arabirimi ve REST tabanlı istemci kitaplıkları sağlar. Azure Blob Depolama farklı olarak, Azure dosyaları Azure dosya paylaşımları için SMB erişim sunar. SMB kullanarak dosya sistemine herhangi özel bir sürücü ekleme ya da herhangi bir kod yazmadan olmadan bir Azure dosya paylaşımı doğrudan Windows, Linux veya macOS, şirket içinde veya sanal makineleri, bulutta bağlayabilir. Ayrıca verilerin kullanıldığı yakın, hızlı erişim için Azure dosya eşitleme'ı kullanarak Azure dosya paylaşımlarını şirket içi dosya sunucularında önbelleğe alabilir. 
   
    Azure Blob Depolama ile Azure dosyaları arasındaki farklar hakkında daha ayrıntılı bir açıklaması için bkz [Azure Blob Depolama, Azure dosyaları veya Azure diskleri ne zaman kullanılacağını belirleme](../common/storage-decide-blobs-files-disks.md). Azure Blob Depolama hakkında daha fazla bilgi edinmek için [Blob depolamaya giriş](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Azure diskleri yerine neden bir Azure dosya paylaşımı kullanmam gerekir?**  
    Azure diskleri bir disk yalnızca bir disktir. Azure diskleri değeri almak için Azure'da çalışan bir sanal makineye bir disk eklemeniz gerekir. Azure diskleri, bir şirket içi sunucusunda bir disk için kullanacağınız her şey için kullanılabilir. Bir işletim sistemi diski olarak, bir işletim sistemi için takas alanı veya bir uygulama için ayrılmış depolama olarak kullanabilirsiniz. Bir ilgi çekici Azure diskleri için Azure dosya paylaşımının nerede kullanabileceğinizi aynı yerde kullanmak için bulutta bir dosya sunucusu oluşturmak için kullanılır. Bir dosya sunucusunu Azure sanal Makineler'de dağıtma (NFS protokolü desteği veya premium depolama gibi) ile Azure dosyaları şu anda desteklenmeyen dağıtım seçenekleri gerektirdiğinde, Azure dosya depolama almak için bir yüksek performanslı yoludur. 

    Ancak, arka uç depolama alanı olarak Azure diskleri olan bir dosya sunucusu genellikle çalışan bazı nedenlerden dolayı bir Azure dosya paylaşımı kullanmaktan çok daha pahalıdır. İlk olarak, disk depolama alanı için ödeme yanı sıra, ayrıca, bir veya daha fazla Azure sanal makinelerini çalıştıran gider için ödeme yapmanız gerekir. İkinci olarak, dosya sunucusu çalıştırmak için kullanılan sanal makineleri yönetmeniz gerekir. Örneğin, işletim sistemi yükseltmeleri için sorumlu olursunuz. Önbelleğe alınan şirket içi verilerin nihai olarak gerekiyorsa, son olarak, ayarlama ve yönetme gibi dağıtılmış dosya sistemi çoğaltma (gerçekleşen yapmak DFSR), çoğaltma teknolojilerini olanak aittir.

    Hem Azure dosyaları hem de Azure sanal Makineler'de (Azure diskleri arka uç depolama alanı olarak kullanmanın yanı sıra) barındırılan bir dosya sunucusu en iyi şekilde almak için bir yaklaşım bir dosya sunucusunda bir VM bulut üzerinde barındırılan Azure dosya eşitleme yüklemektir. Azure dosya paylaşımını dosya sunucunuz ile aynı bölgede ise, bulut katmanlama ve birim boş alan yüzdesi (% 99) en ayarı etkinleştirebilirsiniz. Bu, verilerin en az çoğaltma sağlar. NFS Protokolü gerektiren uygulamalar desteği gibi dosya sunucularınızı, istediğiniz tüm uygulamaları da kullanabilirsiniz.

    Azure'da yüksek performanslı ve yüksek oranda kullanılabilir bir dosya sunucusu kurmak için bir seçenek hakkında daha fazla bilgi için bkz. [dağıtma Iaas VM Konuk kümeleri Microsoft Azure'da](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Azure dosyaları ile Azure disklerini arasındaki farklar hakkında daha ayrıntılı açıklaması için bkz: [Azure Blob Depolama, Azure dosyaları veya Azure diskleri ne zaman kullanılacağını belirleme](../common/storage-decide-blobs-files-disks.md). Azure diskleri hakkında daha fazla bilgi için bkz: [Azure yönetilen disklere genel bakış](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>
  **Nasıl Azure dosyaları'nı kullanarak başlayabilirim?**  
   Azure dosyaları ile çalışmaya başlamak kolaydır. İlk olarak, [dosya paylaşımı oluşturma](storage-how-to-create-file-share.md)ve ardından tercih edilen işletim sisteminizi bağlayın: 

  * [Windows bağlama](storage-how-to-use-files-windows.md)
  * [Linux'ta bağlama](storage-how-to-use-files-linux.md)
  * [MacOS bağlama](storage-how-to-use-files-mac.md)

    Üretim dosya paylaşımlarını kuruluşunuzdaki değiştirmek bir Azure dosya paylaşımı dağıtma hakkında daha ayrıntılı kılavuzu için bkz: [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md).

* <a id="redundancy-options"></a>
  **Hangi depolama yedekliliği seçenekleri, Azure dosyaları tarafından destekleniyor mu?**  
    Şu anda Azure dosyaları, yerel olarak yedekli depolama (LRS), bölgesel olarak yedekli depolama (ZRS) ve coğrafi olarak yedekli depolama (GRS) destekler. Okuma erişimli coğrafi olarak yedekli (RA-GRS) depolama gelecekte desteklemeyi planlıyoruz, ancak biz şu anda paylaşmak için zaman çizelgesi yok.

* <a id="tier-options"></a>
  **Hangi depolama katmanları Azure dosyalarında destekleniyor mu?**  
    Azure dosyaları, iki depolama katmanı destekler: premium ve standart. Standart dosya paylaşımları, genel amaç (GPv1 veya GPv2) depolama hesabı oluşturulur ve premium dosya paylaşımlarına dosya deposundan depolama hesaplarında oluşturulur. Nasıl oluşturulacağı hakkında daha fazla bilgi [standart dosya paylaşımları](storage-how-to-create-file-share.md) ve [premium dosya paylaşımları](storage-how-to-create-premium-fileshare.md). 
    
    > [!NOTE]
    > Blob Depolama hesaplarını Azure dosya paylaşımlarını oluşturulamıyor veya *premium* genel amaçlı (GPv1 veya GPv2) depolama hesapları. Standart Azure dosya paylaşımları oluşturulan gereken *standart* genel amaçlı hesaplar yalnızca ve premium, Azure dosya paylaşımlarını yalnızca dosya deposundan depolama hesaplarında oluşturulması gerekir. *Premium* genel amaçlı (GPv1 ve GPv2) depolama hesapları yalnızca premium sayfa blobları için uyumludur. 

* <a id="give-us-feedback"></a>
  **Gerçekten Azure dosyaları'na eklenen belirli bir özellik öğrenmek istiyorum. Ekleyebilir miyim?**  
    Azure dosyaları takım hizmetimiz hakkında sahip olduğunuz tüm geri bildirim duymak ilgileniyor. Özellik istekleri Lütfen oylamak [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! İleri için birçok yeni özellik ile birbirinden bekliyoruz.

## <a name="azure-file-sync"></a>Azure Dosya Eşitleme

* <a id="afs-region-availability"></a>
  **Azure dosya eşitleme için hangi bölgeler desteklenir?**  
    Kullanılabilir bölgelerin listesi bulunabilir [bölge kullanılabilirliği](storage-sync-files-planning.md#region-availability) bölümü Azure dosya eşitleme Planlama Kılavuzu. Sürekli olarak genel olmayan bölge dahil ek bölgeler için destek ekleyeceğiz.

* <a id="cross-domain-sync"></a>
  **Etki alanına katılmış ve etki alanı ile birleşik olmayan sunucuları aynı eşitleme grubunda olabilir mi?**  
    Evet. Bunlar etki alanına katılmış olmasa bile bir eşitleme grubu farklı Active Directory üyeliği olan sunucu uç noktası içerebilir. Bu yapılandırma teknik olarak çalışır, ancak bir sunucu üzerindeki dosya ve klasörleri için tanımlanan erişim denetim listeleri (ACL'ler) eşitleme grubundaki diğer sunucular tarafından uygulanmasını mümkün olmayabilir çünkü bu tipik bir yapılandırma olarak önermiyoruz. En iyi sonuçlar için aynı Active Directory ormanında olan sunucular arasında farklı Active Directory ormanında olan ancak bu güven ilişkisi oluşturan sunucular arasında veya bir etki alanında olmayan sunucular arasında eşitlemeyi öneririz. Bu yapılandırmalar bir karışımını kullanarak kaçınmanızı öneririz.

* <a id="afs-change-detection"></a>
  **Bir dosya my Azure dosya paylaşımı doğrudan SMB kullanarak veya Portalı'nda oluşturdum. Dosya eşitleme grubundaki sunucuların eşitlenecek ne kadar sürüyor?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Yaklaşık aynı anda aynı dosya iki sunucuya değiştirilirse, ne olur?**  
    Azure dosya eşitleme basit çakışma stratejisi kullanır: aynı anda iki sunucuya değişen dosyaları her iki değişiklik saklarız. En kısa süre önce yazılmış değişiklik, özgün dosya adı tutar. Eski dosya "kaynak" makine ve çakışma sayı adına sahiptir. Bu sınıflandırma takip eden: 
   
    \<FileNameWithoutExtension\>-\<MachineName\>\[-#\].\< ext\>  

    Örneğin, eski yazma oluştuğu CentralServer ise CompanyReport.docx ilk ihtilaf CompanyReport CentralServer.docx olur. İkinci çakışma CompanyReport CentralServer 1.docx adlandırılması.

* <a id="afs-storage-redundancy"></a>
  **Coğrafi olarak yedekli depolama için Azure dosya eşitleme destekleniyor mu?**  
    Evet, Azure dosyaları, hem yerel olarak yedekli depolama (LRS) hem de coğrafi olarak yedekli depolama (GRS) destekler. GRS için yapılandırılmış bir hesaptan eşleştirilmiş bölgelerin arasında bir depolama hesabı yük devretme'ı başlattığınızda, Microsoft, yeni bölge yalnızca verilerin yedek olarak davran önerir. Azure dosya eşitleme, yeni birincil bölge ile eşitlemeyi otomatik olarak başlamaz. 

* <a id="sizeondisk-versus-size"></a>
  **Neden olmayan *disk boyutu* özelliği için bir dosya eşleşmesi *boyutu* Azure dosya eşitleme kullandıktan sonra özellik?**  
  Bkz: [anlama bulut Katmanlaması](storage-sync-cloud-tiering.md#sizeondisk-versus-size).

* <a id="is-my-file-tiered"></a>
  **Bir dosya katmanlanmış olup olmadığını nasıl anlayabilirim?**  
  Bkz: [anlama bulut Katmanlaması](storage-sync-cloud-tiering.md#is-my-file-tiered).

* <a id="afs-recall-file"></a>**Kullanmak istediğiniz bir dosya katmanlı. Yerel olarak kullanabilmek için diskten dosyasına nasıl geri çağırma?**  
  Bkz: [anlama bulut Katmanlaması](storage-sync-cloud-tiering.md#afs-recall-file).

* <a id="afs-force-tiering"></a>
  **Bir dosya veya dizin katmanlanmış nasıl zorlarım?**  
  Bkz: [anlama bulut Katmanlaması](storage-sync-cloud-tiering.md#afs-force-tiering).

* <a id="afs-effective-vfs"></a>
  **Nasıl olduğunu *birim boş alanı* birden çok sunucu uç noktaları bir birimde olduğunda yorumlanan?**  
  Bkz: [anlama bulut Katmanlaması](storage-sync-cloud-tiering.md#afs-effective-vfs).

* <a id="afs-files-excluded"></a>
  **Hangi dosya ve klasörleri Azure dosya eşitleme tarafından otomatik olarak dışlanır?**  
    Varsayılan olarak, Azure dosya eşitleme aşağıdaki dosyaları hariç tutar:
  * Desktop.ini
  * Thumbs.DB
  * ehthumbs.db
  * ~$\*.\*
  * \*.laccdb
  * \*.tmp
  * 635D02A9D91C401B97884B82B3BCDAEA.\*

    Aşağıdaki klasörleri de varsayılan olarak tutulur:

  * \System volume Information
  * \$GERİ DÖNÜŞÜM. DEPO
  * \SyncShareState

* <a id="afs-os-support"></a>
  **Azure dosya eşitleme Windows Server 2008 R2, Linux veya ağa bağlı depolama (NAS) cihazımı ile kullanabilir miyim?**  
    Azure dosya eşitleme, şu anda yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 destekler. Şu anda, biz size paylaşabilirsiniz herhangi bir plan yok, ancak müşteri talebine göre ek platformları destek açık duyuyoruz. Bize bildirin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) hangi platformları destekleyecek şekilde göndermemizi istediğinizi.

* <a id="afs-tiered-files-out-of-endpoint"></a>
  **Neden katmanlı dosyaları dışında sunucu uç noktası ad var mı?**  
    Azure dosya eşitleme Aracısı sürüm 3 önce Azure dosya eşitleme taşıma katmanlı dosyaların sunucu uç noktasını dışında fakat sunucu uç noktası ile aynı birimde engellendi. Kopyalama işlemlerini olmayan katmanlı dosya taşır ve için katmanlı taşır diğer birimleri etkilenmez. Dosya Gezgini ve diğer Windows API'leri aynı birimde taşıma işlemlerini (yaklaşık) anlıktır sahip örtük varsayımına yeniden adlandırma işlemleri davranışı için neden oldu. Bu hamle dosya Gezgini yapar veya diğer taşıma yöntemleri ile (örneğin, komut satırından veya PowerShell) Azure dosya eşitleme bulut verileri geri çekme sırasında yanıt vermeyen görünür anlamına gelir. İle başlayarak [Azure dosya eşitleme Aracısı sürüm 3.0.12.0](storage-files-release-notes.md#supported-versions), Azure dosya eşitleme katmanlı bir dosya sunucu uç noktasını dışında taşımanıza olanak sağlayacaktır. Biz, katmanlı sunucu uç noktasını dışında bir katmanlı dosya olarak mevcut dosyaya izin verme ve ardından dosya arka planda geri çağırma tarafından daha önce bahsedilen olumsuz etkileri kaçının. Başka bir deyişle, aynı birim anlık ve dosyayı diske taşıma tamamlandıktan sonra geri çağırma iş yaptığımız taşır. 

* <a id="afs-do-not-delete-server-endpoint"></a>
  **My server (eşitleme, bulut katmanlama, vb.) Azure dosya eşitleme ile ilgili bir sorun yaşıyorum. Kaldırın ve paylaşabilirim my server uç noktası yeniden?**  
    [!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]
    
* <a id="afs-resource-move"></a>
  **Depolama eşitleme hizmeti ve/veya depolama hesabını bir farklı kaynak grubuna veya aboneliğe taşıyabilirim?**  
   Evet, depolama eşitleme hizmeti ve/veya depolama hesabı farklı bir kaynak grubu veya abonelik içinde mevcut Azure AD kiracısı için taşınabilir. Depolama hesabı taşınırsa, depolama hesabına karma dosya eşitleme hizmeti erişmesini gerekir (bkz [olun Azure dosya eşitleme depolama hesabına erişebilir](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure dosya eşitleme desteklemez aboneliği taşımak için farklı bir Azure AD kiracısı.
    
* <a id="afs-ntfs-acls"></a>
  **Azure dosya eşitleme için Azure dosyalarında depolanan verilerle birlikte dizin/dosya düzeyinde NTFS ACL'leri koruyor mu?**

    Şirket içi dosya sunucularını meta veri olarak Azure dosya eşitleme tarafından kalıcı NTFS ACL'ler taşınan. Azure dosyaları desteklemez Azure AD kimlik bilgileriyle kimlik doğrulaması için Azure dosya eşitleme hizmeti tarafından yönetilen dosya paylaşımları.
    
## <a name="security-authentication-and-access-control"></a>Güvenlik, kimlik ve erişim denetimi
* <a id="ad-support"></a>
**Active Directory tabanlı kimlik doğrulaması ve Azure dosyaları tarafından desteklenen erişim denetimi nedir?**  
    
    Evet, Azure dosyaları kimlik tabanlı kimlik doğrulaması ve erişim denetimi Azure Active Directory (Azure AD) ile destekler (Önizleme). Azure dosyaları için SMB üzerinden Azure AD authentication, Azure Active Directory etki alanı paylaşımları, dizinleri ve dosyaları Azure AD kimlik bilgilerini kullanarak erişmek etki alanına katılmış sanal makineleri Etkinleştirme Hizmetleri yararlanır. Daha fazla ayrıntı için [genel bakış, Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme)](storage-files-active-directory-overview.md). 

    Azure dosyaları, erişim denetimi yönetmek için iki ek yollar sunar:

    - Paylaşılan erişim imzaları (SAS), belirli izinlere sahip ve belirtilen zaman aralığı için geçerli olan belirteçleri oluşturmak için kullanabilirsiniz. Örneğin, 10 dakikalık süre sonu belirli bir dosya salt okunur erişimi olan bir belirteç oluşturabilirsiniz. Herkes belirtecin geçerli olduğu halde belirtece sahip salt okunur dosya söz konusu 10 dakika erişebilir. Şu anda paylaşılan erişim imzası anahtarları yalnızca REST API aracılığıyla veya istemci kitaplıkları desteklenmektedir. Depolama hesabı anahtarlarını kullanarak SMB üzerinden Azure dosya paylaşımını bağlama gerekir.

    - Azure dosya eşitleme (Active Directory tabanlı veya yerel olup olmadığını) tüm ACL'lerde veya DACL'leri çoğaltır ve korur için eşitlenen tüm sunucu uç noktaları için. Windows Server Active Directory ile zaten kimliğini doğrulamak için Azure dosya eşitleme Active Directory tabanlı kimlik doğrulaması için tam destek kadar etkili bir stop-boşluk seçenek ve ACL desteği ulaşır.

* <a id="ad-support-regions"></a>
**Azure ad Önizleme SMB üzerinden Azure dosyaları için tüm Azure bölgelerinde kullanılabilir mi?**

    Tüm ortak bölgelerde Önizleme kullanılabilir.

* <a id="ad-support-on-premises"></a>
**Şirket içi makinelerin Azure AD ile kimlik doğrulama (Önizleme) Azure dosyaları SMB üzerinden Azure AD kimlik doğrulamasını destekliyor mu?**

    Hayır, Azure dosyaları, Önizleme sürümünde şirket içi makinelerin Azure AD ile kimlik doğrulamasını desteklemez.

* <a id="ad-support-devices"></a>
**Cihazların Azure AD kimlik bilgilerini kullanarak Azure dosyaları (Önizleme) destek SMB erişimi için SMB üzerinden kimlik doğrulaması yoksa Azure AD alanına katılmış veya Azure AD'ye kayıtlı?**

    Hayır, bu senaryo desteklenmez.

* <a id="ad-support-rest-apis"></a>
**Get/Set/kopyalama dizin/dosya NTFS ACL'leri desteklemek için REST API'ler var mı?**

    Önizleme sürümü REST API'lerini kullanarak Al Ayarla desteklemez, veya dizinleri ve dosyaları için NTFS ACL'leri kopyalayın.

* <a id="ad-vm-subscription"></a>
**Farklı bir abonelikte bir VM'den Azure AD kimlik bilgileriyle Azure dosyaları erişebilirim?**

    Dosya Paylaşımı dağıtıldığı abonelik eklendiği VM'nin etki alanına katılmış olduğundan ve aynı Azure AD kimlik bilgilerini kullanarak Azure dosyaları erişebiliyorsa Azure AD Domain Services dağıtımla aynı Azure AD kiracısı ile ilişkili ise. Sınırlama yok abonelikte uygulanan ancak ilişkili Azure AD Kiracı.    
    
* <a id="ad-support-subscription"></a>
**Azure AD kimlik doğrulaması SMB üzerinden Azure dosyaları için dosya paylaşımı ile ilişkili olduğu birincil kiracıdan farklı bir Azure AD kiracınız ile etkinleştirebilirim?**

    Hayır, Azure dosyaları, yalnızca dosya paylaşımı ile aynı abonelikte bulunan Azure AD kiracısı ile Azure AD tümleştirmeyi destekler. Yalnızca bir aboneliği Azure AD kiracısı ile ilişkili olabilir.

* <a id="ad-linux-vms"></a>
**SMB üzerinden Azure AD kimlik doğrulaması (Önizleme) Azure dosyaları için Linux Vm'lerini destekliyor mu?**

    Hayır, Linux sanal makineleri kimlik doğrulamasını Önizleme sürümünde desteklenmiyor.

* <a id="ad-aad-smb-afs"></a>
**Azure AD kimlik doğrulaması üzerinden Azure dosya eşitleme tarafından yönetilen dosya paylaşımları SMB yeteneklerini yararlanabilir?**

    Hayır, Azure dosyaları Azure dosya eşitleme tarafından yönetilen dosya paylaşımları üzerinde NTFS ACL'leri koruma desteklemez. Gelen ACL'ler taşınan dosyanın dosya sunucuları, Azure dosya eşitleme tarafından kalıcı şirket içi. Azure dosyaları karşı yerel olarak yapılandırılmış tüm NTFS ACL'leri Azure dosya eşitleme hizmeti tarafından üzerine yazılır. Ayrıca, Azure dosyaları Azure dosya eşitleme hizmeti tarafından yönetilen dosya paylaşımları için Azure AD kimlik bilgileriyle kimlik doğrulaması desteklemez.

* <a id="encryption-at-rest"></a>
**My Azure dosya paylaşımı bekleme durumundayken şifrelenir nasıl sağlayabilirsiniz?**  

    Evet. Daha fazla bilgi için [Azure depolama hizmeti şifrelemesi](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>
**Nasıl bir web tarayıcısı kullanarak ı belirli bir dosyaya erişimi verebilir misiniz?**  

    Paylaşılan erişim imzaları, belirli izinlere sahip ve belirtilen zaman aralığı için geçerli olan belirteçleri oluşturmak için kullanabilirsiniz. Örneğin, belirli bir dosyayı belirli bir süre için salt okunur erişim sağlayan bir belirteç oluşturabilirsiniz. Belirtecin geçerli olduğu halde URL'ye sahip olan herkes dosyayı doğrudan bir web tarayıcısından erişebilirsiniz. Paylaşılan erişim imza anahtarı, Depolama Gezgini gibi bir kullanıcı arabiriminden kolayca oluşturabilirsiniz.

* <a id="file-level-permissions"></a>
**Bu, veya paylaşımdaki klasörlere sadece yazılabilir izinleri tanıyabilir salt okunur mi?**  

    SMB kullanarak dosya paylaşımını bağlama, klasör düzeyinde izinler denetime sahip değilsiniz. REST API veya istemci kitaplıkları aracılığıyla paylaşılan erişim imzası oluşturma, paylaşımdaki klasörlere salt okunur veya sadece yazılabilir izinleri belirtebilirsiniz.

* <a id="ip-restrictions"></a>
**Bir Azure dosya paylaşımı için IP kısıtlamalarını uygulayan miyim?**  

    Evet. Depolama hesap düzeyinde Azure dosya paylaşımınızı erişim kısıtlanmış olabilir. Daha fazla bilgi için [Azure depolama güvenlik duvarlarını yapılandırın ve sanal ağları](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>
**Hangi veri uyumluluk ilkeleri, Azure dosyaları destekliyor mu?**  

   Azure dosyaları Azure depolama alanındaki diğer depolama hizmetlerindeki kullanılan aynı depolama mimarisi üzerinde çalışır. Azure dosyaları, diğer Azure depolama hizmetleri içinde kullanılan aynı veri uyumluluk ilkelerini uygular. Azure depolama veri uyumluluğu hakkında daha fazla bilgi için başvurabilirsiniz [Azure depolama uyumluluk teklifi](https://docs.microsoft.com/azure/storage/common/storage-compliance-offerings)ve Git [Microsoft Trust Center](https://microsoft.com/trustcenter/default.aspx).

## <a name="on-premises-access"></a>Şirket içi erişim

* <a id="port-445-blocked"></a>
**ISS veya Azure dosyaları başarısız BT blokları bağlantı noktası 445'cihazımı bağlayın. Ne yapmalıyım?**

    Hakkında bilgi edinebilirsiniz [engellenen geçici çözüm için çeşitli yollar burada 445 bağlantı noktası](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems#cause-1-port-445-is-blocked). Azure dosyaları yalnızca sağlar (şifreleme desteği ile) SMB 3.0 kullanarak bağlantıları gelen bölge veya veri merkezinin dışında. SMB 3.0 protokolü, internet üzerinden kullanmak için çok güvenli kanal şifrelemesini dahil olmak üzere birçok güvenlik özelliği kullanıma sundu. Ancak, 445 bağlantı noktası olası geçmiş nedeniyle daha düşük SMB sürümlerinde bulunan güvenlik açığı nedeniyle engellendi. İdeal durumda, bağlantı noktası için yalnızca SMB 1.0 trafiği engellenmesi gerektiğini ve tüm istemcilerde SMB 1.0 kapatılmalıdır.

* <a id="expressroute-not-required"></a>
**Azure dosya eşitleme kullanmak için şirket içi veya Azure ExpressRoute, Azure dosyaları'na bağlanmak zorunda?**  

    Hayır. ExpressRoute, Azure dosya paylaşımına erişmek için gerekli değildir. Bağlama durumunda bir Azure dosya açın (Bu, SMB iletişim kurmak için kullandığı bağlantı noktası) Internet erişimi için bağlantı noktası 445 (TCP Giden) için tüm gereken şirket, doğrudan paylaşabilirsiniz. Azure dosya eşitleme kullanıyorsanız, gerekli olan bağlantı noktası 443 (TCP Giden) için HTTPS erişim (SMB gerekli). Ancak, *olabilir* ExpressRoute bu erişim seçeneklerden birini kullanın.

* <a id="mount-locally"></a>
**Nasıl ben Azure dosya paylaşımını yerel makineme takabilir miyim?**  

    SMB protokolü kullanarak bağlantı noktası 445 (TCP Giden) açık ve (örneğin, Windows 10 veya Windows Server 2016'yı kullanıyorsanız), istemci SMB 3.0 protokolünü destekleyene dosya paylaşımı bağlayabilir. Bağlantı noktası 445, kuruluşunuzun İlkesi veya ISS'niz tarafından engellenirse, Azure dosya paylaşımına erişmek için Azure dosya eşitleme'ni kullanabilirsiniz.

## <a name="backup"></a>Backup
* <a id="backup-share"></a>
**Nasıl Azure dosyamı yedekleme paylaşırım?**  
    Kullanabileceğiniz düzenli [paylaşım anlık görüntüleri](storage-snapshots-files.md) yanlışlıkla silinmekten karşı koruma için. AzCopy, Robocopy ve bir bağlı dosya paylaşımını yedekleyebilmeniz bir üçüncü taraf yedekleme aracını kullanabilirsiniz. Azure Backup, Azure dosyaları yedeklemesi sunar. Daha fazla bilgi edinin [Azure yedekleme dosya paylaşımlarını Azure Backup tarafından](https://docs.microsoft.com/azure/backup/backup-azure-files).

## <a name="share-snapshots"></a>Paylaşım anlık görüntüleri

### <a name="share-snapshots-general"></a>Paylaşım anlık görüntüleri: Genel
* <a id="what-are-snaphots"></a>
**Dosya Paylaşımı anlık görüntüleri nelerdir?**  
    Dosya paylaşımlarınızı salt okunur bir sürümünü oluşturmak için Azure dosya paylaşımı anlık görüntülerini kullanabilirsiniz. Azure dosyaları, içerik arkanızı önceki bir sürümünü aynı paylaşımına, Azure'da veya şirket içi daha fazla değişiklikler için alternatif bir konuma kopyalamak için de kullanabilirsiniz. Paylaşım anlık görüntüleri hakkında daha fazla bilgi için bkz: [paylaşım anlık görüntüsü genel bakış](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>
**My paylaşım anlık görüntülerini nerede depolanır?**  
    Paylaşım anlık görüntüleri, aynı depolama hesabında dosya paylaşımı olarak depolanır.

* <a id="snapshot-perf-impact"></a>
**Paylaşım anlık görüntüleri kullanmak için herhangi bir performans etkileri var mı?**  
    Paylaşım anlık görüntüleri, herhangi bir performansa sahip değilsiniz.

* <a id="snapshot-consistency"></a>
**Paylaşım anlık görüntüleri, uygulamayla tutarlı misiniz?**  
    Hayır, paylaşım anlık görüntüleri, uygulamayla tutarlı değildir. Kullanıcı bu uygulamadan önce paylaşım anlık paylaşımına yazma işlemlerini boşaltmaya gerekir.

* <a id="snapshot-limits"></a>
**Paylaşım anlık görüntüleri kullanabilir miyim sayısına yönelik sınırlar var mıdır?**  
    Evet. Azure dosyaları en fazla 200 paylaşım anlık görüntüleri tutabilirsiniz. Bu yüzden paylaşım anlık görüntüleri paylaşım başına sınırsız tüm paylaşım anlık görüntüleri tarafından kullanılan toplam alan paylaşımı kotasında sayılır. Depolama hesabı sınırları hala geçerlidir. 200 paylaşım anlık görüntüleri sonra yeni bir paylaşım anlık görüntüleri oluşturmak için eski anlık görüntüleri silmeniz gerekir.

* <a id="snapshot-cost"></a>
**Anlık görüntüleri maliyeti ne kadar paylaşabilir?**  
    Standart işlem ve standart depolama maliyeti anlık görüntüye uygulanır. Doğası gereği artımlı anlık görüntüleridir. Temel anlık görüntü Paylaş ' dir. Sonraki tüm anlık görüntüleri, artımlı ve yalnızca önceki anlık görüntüden fark depolar. Başka bir deyişle faturada görülür değişim değişiklikleri, iş yükü değişim sıklığı en az ise minimum olur. Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/) standart Azure dosyaları'nı fiyatlandırma bilgileri için. Paylaşım anlık görüntüsü tarafından tüketilen boyutu bakmak için faturalanan kapasite ile karşılaştırarak yoludur bugün kapasite kullanılır. Raporlama geliştirmek için Araçlar üzerinde çalışıyoruz.

* <a id="ntfs-acls-snaphsots"></a>
**NTFS ACL'leri dizinleri ve dosyaları paylaşım anlık görüntüleri kalıcı mı?**
    NTFS ACL'lerin dizinlerin ve dosyaların paylaşım anlık görüntüleri kalıcıdır.

### <a name="create-share-snapshots"></a>Paylaşım anlık görüntüsü oluşturma
* <a id="file-snaphsots"></a>
**Tek tek dosya paylaşım anlık görüntüsünü oluşturabilir miyim?**  
    Paylaşım anlık görüntüleri, dosya paylaşımı düzeyinde oluşturulur. Dosya paylaşım anlık görüntüsünden tek tek dosyaları geri yükleyebilirsiniz, ancak dosya düzeyinde paylaşım anlık görüntüleri oluşturamıyor. Ancak, paylaşım düzeyi paylaşım anlık görüntüsü yaptıktan ve belirli bir dosya değişikliklerini nereye paylaşım anlık görüntülerini listelemek istediğiniz altında bunu yapabilirsiniz **önceki sürümler** Windows monte paylaşımında. 
    
    Dosya anlık görüntü özelliği gerekirse, bize [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encrypted-snapshots"></a>
**Şifrelenmiş dosya paylaşımının paylaşım anlık görüntülerini oluşturabilir miyim?**  
    Etkin bekleme sırasında şifreleme içeren Azure dosya paylaşımlarını bir paylaşım anlık görüntüsünü alabilir. Bir şifrelenmiş dosya paylaşımına dosya paylaşım anlık görüntüsünden geri yükleyebilirsiniz. Paylaşımınızın şifrelendiyse, paylaşım anlık görüntüsünü de şifrelenir.

* <a id="geo-redundant-snaphsots"></a>
**My paylaşım anlık görüntüleri, coğrafi olarak yedekli misiniz?**  
    Paylaşım anlık görüntüleri için çekildiği Azure dosya paylaşımı olarak aynı yedeklilik vardır. Hesabınız için coğrafi olarak yedekli depolama seçtiyseniz, ayrıca anlık görüntü paylaşımınızın nedenle eşleştirilmiş bölgede depolanır.

### <a name="manage-share-snapshots"></a>Paylaşım anlık görüntüleri yönetme
* <a id="browse-snapshots-linux"></a>
**My paylaşım anlık görüntüleri linux'taki göz atın?**  
    Azure CLI, oluşturmak, listesinde, göz atın ve Linux'ta paylaşım anlık görüntüleri geri yüklemek için kullanabilirsiniz.

* <a id="copy-snapshots-to-other-storage-account"></a>
**Paylaşım anlık görüntüleri için farklı bir depolama hesabı kopyalayabilir miyim?**  
    Paylaşım anlık görüntülerindeki dosyaları başka bir konuma kopyalayabilirsiniz ancak paylaşım anlık görüntüleri kendilerini kopyalanamıyor.

### <a name="restore-data-from-share-snapshots"></a>Paylaşım anlık görüntülerindeki veri geri yükleme
* <a id="promote-share-snapshot"></a>
**Ben bir paylaşım anlık görüntüsünü temel paylaşımına yükseltebilirsiniz?**  
    Herhangi bir hedef için paylaşım anlık görüntüsünden veri kopyalayabilirsiniz. Temel paylaşım için paylaşım anlık görüntüsü yükseltilemiyor.

* <a id="restore-snapshotted-file-to-other-share"></a>
**Ben veri my paylaşım anlık görüntüsünden farklı bir depolama hesabına geri yükleyebilirim?**  
    Evet. Paylaşım anlık görüntüsü dosyalarını özgün konuma veya aynı depolama hesabını ya da farklı bir depolama hesabı, aynı bölgedeki veya farklı bölgelerde içeren farklı bir konuma kopyalanabilir. Ayrıca, bir şirket içi konuma veya başka bir bulut dosya kopyalayabilirsiniz.    
  
### <a name="clean-up-share-snapshots"></a>Paylaşım anlık görüntüleri ' Temizle
* <a id="delete-share-keep-snapshots"></a>
**Ben my paylaşımını silmeye ancak benim paylaşım anlık görüntüleri silme?**  
    Etkin bir paylaşım anlık görüntüleri, paylaşımında varsa, paylaşımınıza nelze odstranit. Bir API paylaşım birlikte paylaşım anlık görüntüleri silmek için kullanabilirsiniz. Paylaşım anlık görüntüleri hem de Azure portalında paylaşımını silebilirsiniz.

* <a id="delete-share-with-snapshots"></a>
**My paylaşım anlık görüntüleri için depolama Hesabımı silersem ne olur?**  
    Depolama hesabınızı silerseniz, paylaşım anlık görüntülerini de silinir.

## <a name="billing-and-pricing"></a>Faturalama ve fiyatlandırma
* <a id="vm-file-share-network-traffic"></a>
**Ağ ücreti aboneliğe yansıtılan harici bir bant genişliği olarak bir Azure VM ile bir Azure dosya paylaşım sayısı arasında trafiği mu?**  
    Dosya Paylaşımı ve VM aynı Azure bölgesinde varsa, dosya paylaşımı ve sanal makine arasında trafiği için ek ücret yoktur. Dosya Paylaşımı ve sanal makine farklı bölgelerde bulunuyorsa, aralarındaki trafik harici bant genişliği olarak ücretlendirilir.

* <a id="share-snapshot-price"></a>
**Anlık görüntüleri maliyeti ne kadar paylaşabilir?**  
     Önizleme sırasında paylaşım anlık görüntü kapasitesi için ek ücret yoktur. Standart depolama çıkış ve işlem masrafları uygulanır. Genel kullanılabilirlik sonrasında, abonelik kapasitesi ve Paylaşım anlık görüntüleri üzerinde işlemler için ücretlendirilirsiniz.
     
     Paylaşım anlık görüntüleri, doğası gereği artımlı. Temel paylaşım anlık görüntüsüne Paylaşımı ' dir. Tüm sonraki paylaşım anlık görüntüleri, artımlı ve yalnızca önceki paylaşım anlık görüntüsüne fark depolayın. Yalnızca değişen içeriği için faturalandırılırsınız. Veri 100 GiB paylaşımıyla varsa ancak yalnızca 5 GiB son, paylaşım anlık görüntüsü bu yana değişti paylaşım anlık görüntüsüne yalnızca 5 ek GiB tüketir ve 105 GiB için faturalandırılırsınız. İşlem ve standart çıkış ücretlerini hakkında daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="scale-and-performance"></a>Ölçek ve performans
* <a id="files-scale-limits"></a>
**Azure dosyaları ölçek sınırları nelerdir?**  
    Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında bilgi için bkz: [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>
**Hangi boyutları, Azure dosya paylaşımları için kullanılabilir mi?**  
    Azure dosya paylaşımı boyutları (premium ve standart), en fazla 100 tib'a kadar ölçeklendirebilirsiniz. Premium dosya paylaşımları boyutu 100 TiB kadar bir GA teklifi olarak kullanılabilir. 5 TiB kadar standart dosya paylaşımları boyutları, 100 TiB kadar boyutları Önizleme sürümündedir ancak bir GA, teklifi olarak kullanılabilir. Bkz: [sağlamak büyük dosya paylaşımları (standart katman)](storage-files-planning.md#onboard-to-larger-file-shares-standard-tier) bölüm büyük dosyasına ekleme yönergeleri için Planlama Kılavuzu'nun standart katmanda Önizleme paylaşır.

* <a id="open-handles-quota"></a>
**Kaç adet istemcinin aynı dosyayı aynı anda erişebilir miyim?**    
    Tek bir dosya çubuğunda 2.000 açık tanıtıcıları kotası yoktur. 2\.000 açık tanıtıcıları olduğunda kotasına ulaşıldığında belirten bir hata iletisi görüntülenir.

* <a id="zip-slow-performance"></a>
**Ben Azure dosyaları sıkıştırmasını olduğunda performansta yavaştır. Ne yapmalıyım?**  
    Azure dosyaları için çok sayıda dosya aktarmak için AzCopy (Windows için; Linux ve UNIX için önizleme modunda) ya da Azure PowerShell kullanmanızı öneririz. Bu araçlar ağ aktarım için optimize edilmiştir.

* <a id="slow-perf-windows-81-2012r2"></a>
**My Azure dosya paylaşımını Windows Server 2012 R2 veya Windows 8.1 bağladıktan sonra yavaş performansımı neden oluyor?**  
    Azure dosya paylaşımını Windows Server 2012 R2 ve Windows 8.1 üzerinde bağlarken bilinen bir sorun yoktur. Sorun, Windows 8.1 ve Windows Server 2012 R2 için Nisan 2014 toplu güncelleştirme oluşturulmuştur. En iyi performans için Windows Server 2012 R2 ve Windows 8.1 tüm örnekleri bu düzeltme eki uygulanan olduğundan emin olun. (Her zaman Windows Update aracılığıyla Windows düzeltme ekleri de alacaksınız.) Daha fazla bilgi için ilişkili Microsoft Bilgi Bankası makalesine bakın [Windows 8.1 veya Server 2012 R2'den Azure dosyaları eriştiğinizde performansın yavaşlaması](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Özellikler ve diğer hizmetleri ile birlikte çalışabilirlik
* <a id="cluster-witness"></a>
**My Azure dosya paylaşımı olarak kullanmak bir *dosya paylaşımı Tanığını* my Windows Server Yük devretme kümesi için?**  
    Şu anda bu yapılandırmayı bir Azure dosya paylaşımı için desteklenmiyor. Bu Azure Blob Depolama için ayarlama hakkında daha fazla bilgi için bkz. [yük devretme kümesi için bulut tanığı dağıtma](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>
**Azure dosya paylaşımını bir Azure kapsayıcı örneğinde takabilir miyim?**  
    Evet, Azure dosya paylaşımlarını bir kapsayıcı örneği ömrünü ötesinde bilgileri kalıcı hale getirmek istediğinizde iyi bir seçenektir. Daha fazla bilgi için [Azure Container Instances ile bir Azure dosya paylaşımını bağlama](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>
**Yeniden adlandırma işlemi REST API'de var mı?**  
    Şu anda değil.

* <a id="nested-shares"></a>
**İç içe geçmiş paylaşımlarını ayarlayabilirim? Diğer bir deyişle, bir paylaşım kapsamında?**  
    Hayır. Dosya Paylaşımı *olduğu* iç içe paylaşımlar desteklenmez, böylece, bağlayabileceğiniz sanal bir sunucudur.

* <a id="ibm-mq"></a>
**Azure dosyaları ile IBM MQ nasıl kullanabilirim?**  
    IBM, IBM MQ müşterileri IBM hizmeti ile Azure dosyaları yapılandırma yardımcı olan bir belge yayımladı. Daha fazla bilgi için [Microsoft Azure dosyaları hizmeti ile bir IBM MQ çok örnekli kuyruk yöneticisini kurma](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).

## <a name="see-also"></a>Ayrıca bkz.
* [Windows Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
* [Linux'ta Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
* [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md)
