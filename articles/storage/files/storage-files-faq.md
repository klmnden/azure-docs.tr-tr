---
title: "Azure dosyaları için sık sorulan sorular | Microsoft Docs"
description: "Azure dosyaları hakkında sık sorulan soruların yanıtlarını bulun."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/13/2017
ms.author: renash
ms.openlocfilehash: da8ccf35dcc873a5c31842c6eb7bdf72879854c2
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="frequently-asked-questions-about-azure-files"></a>Azure dosyaları hakkında sık sorulan sorular
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları endüstri standardı erişilebilir bulutta sunar [sunucu ileti bloğu (SMB) Protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (ortak Internet dosya sistemi veya CIFS olarak da bilinir). Azure dosya paylaşımları Windows, Linux ve macOS Bulut veya şirket içi dağıtımlar üzerinde aynı anda bağlayabilir. Windows Server makinelerini Azure dosya paylaşımlarında veri kullanıldığı yakın hızlı erişim için Azure dosya eşitleme (Önizleme) kullanarak de önbelleğe alabilir.

Bu makalede Azure dosyaları özellikleri ve işlevleri, Azure dosya eşitleme Azure dosyaları ile kullanımı dahil olmak üzere hakkında sık sorulan sorular yanıtlanmaktadır. Sorunuzun yanıtını görmüyorsanız, bize (sırayla yükselen) aşağıdaki kanallar aracılığıyla başvurabilirsiniz:

1. Bu makalede Açıklamalar bölümüne.
2. [Azure depolama Forumu](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=windowsazuredata).
3. [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft destek. Azure portalında yeni bir destek isteği oluşturmak için **yardımcı** sekmesine **Yardım + Destek** düğmesine tıklayın ve ardından **yeni destek isteği**.

## <a name="general"></a>Genel
* <a id="why-files-useful"></a>**Azure dosyaları faydalı olur?**  
   Azure dosyaları, bir fiziksel sunucu, cihaz veya Gereci yükü yönetmekten sorumlu olmadan buluta, dosya paylaşımları oluşturmak için kullanabilirsiniz. Biz ve sıkıcı iş sizin için işletim sistemi güncelleştirmelerini uygulama ve hatalı diskleri değiştirme gibi yapın. Azure dosyaları size yardımcı olabilecek senaryoları hakkında daha fazla bilgi için bkz: [neden Azure dosyaları yararlıdır](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>**Azure dosyaları dosyalarında erişim için farklı yollar nelerdir?**  
    SMB 3.0 protokolünü kullanarak yerel makinenizde dosya paylaşımı bağlayabilir veya gibi araçları kullanabilirsiniz [Depolama Gezgini](http://storageexplorer.com/) , dosya paylaşımı içindeki dosyalara erişmek için. Uygulamanızdan dosyalarınızı Azure dosya paylaşımına erişmek için depolama istemci kitaplığı, REST API'leri, PowerShell veya Azure CLI kullanabilirsiniz.

* <a id="what-is-afs"></a>**Azure dosya eşitleme nedir?**  
    Azure dosya eşitleme esneklik, performans ve uyumluluk bir şirket içi dosya sunucusunun tanırken kuruluşunuzun dosya paylaşımları Azure dosyalarında merkezileştirmek için kullanabilirsiniz. Azure dosya eşitleme, Windows Server makinelerini hızlı Azure dosya paylaşımınıza önbelleğine dönüştürür. SMB, ağ dosya sistemi (NFS) ve Dosya Aktarım Protokolü Hizmeti (FTPS) dahil olmak üzere, yerel olarak verilerinize erişmek için Windows Server üzerinde kullanılabilir herhangi bir iletişim kuralı kullanabilirsiniz. Dünya genelinde gerektiği kadar önbellekleri olabilir.

* <a id="files-versus-blobs"></a>**Verilerim için neden Azure Blob storage ile Azure dosya paylaşımının kullanmam?**  
    Azure dosyaları ve Azure Blob Depolama, her ikisi de sunar yolları büyük miktarlarda verinin bulutta depolamak, ancak bunlar biraz farklı amaçlar için kullanışlıdır. 
    
    Azure Blob storage yapılandırılmamış verileri depolamak için gereken büyük ölçekli, bulut yerel uygulamalar için kullanışlıdır. Performansı ve ölçeği en üst düzeye çıkarmak için Azure Blob Depolama bir basit depolama doğru dosya sistemi daha soyutlamadır. Azure Blob Depolama yalnızca REST tabanlı istemci kitaplıkları aracılığıyla (veya doğrudan REST tabanlı bir protokol üzerinden) erişebilir.

    Azure dosyaları şu özellikle bir dosya sistemi. Azure dosyaları bildiğiniz ve şirket içi işletim sistemleri ile çalışmanın yılların memnuniyet tüm dosya özetleri sahiptir. Azure Blob storage'gibi bir REST arabirimi ve REST tabanlı istemci kitaplıkları Azure dosyaları sunar. Azure Blob depolamadan farklı olarak, Azure dosyaları Azure dosya paylaşımları için SMB erişim sunar. SMB kullanarak herhangi bir kod yazmadan ya da dosya sistemine herhangi bir özel sürücü ekleme olmadan Azure dosya paylaşımının doğrudan Windows, Linux veya macOS, ya da şirket içi veya Bulut Vm'lerinde, bağlayabilir. Azure dosya paylaşımları şirket içi dosya sunucularındaki verileri kullanıldığı yakın hızlı erişim için Azure dosya eşitleme kullanarak de önbelleğe alabilir. 
   
    Azure dosyaları ile Azure Blob Depolama arasındaki farklar hakkında daha ayrıntılı bir açıklaması için bkz [Azure Blob storage, Azure dosyaları ya da Azure diskleri kullanmaya karar verme](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Azure Blob Depolama hakkında daha fazla bilgi için bkz: [Blob Storage'a giriş](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Azure diskleri yerine neden bir Azure dosya paylaşımı kullanmam?**  
    Bir Azure disklerde basitçe bir disk disktir. Tek başına bir tek başına disk çok kullanışlı değildir. Azure diskleri değeri almak için Azure'da çalışan bir sanal makineye bir disk ekleyin gerekir. Azure diskleri, bir şirket içi sunucu üzerinde bir disk için kullanacağınız her şey için kullanılabilir. Bir işletim sistemi diski, bir işletim sistemi için takas alanı olarak veya bir uygulama için ayrılmış depolama olarak kullanabilirsiniz. Bir ilginç Azure diskler için Azure dosya paylaşımının nerede kullanabilir yerlerde kullanmak için bulutta bir dosya sunucusu oluşturmak için kullanılır. Bir dosya sunucusu, Azure sanal makineleri dağıtma (NFS protokolü desteği ya da premium depolama gibi) tarafından Azure dosyaları şu anda desteklenmemektedir dağıtım seçenekleri gerektirdiğinde Azure dosya depolama alanı almak için bir yüksek performanslı yoludur. 

    Ancak, arka uç depolama alanı olarak Azure diskler ile bir dosya sunucusu genellikle çalışan bazı nedenlerden dolayı bir Azure dosya paylaşımı kullanmaktan çok daha maliyetlidir. İlk olarak, disk depolama için ödeme yanı sıra, aynı zamanda gider çalışan bir veya daha fazla Azure Vm'lerinin ödemeniz gerekir. İkinci olarak, dosya sunucusu çalıştırmak için kullanılan sanal makineleri yönetmeniz gerekir. Örneğin, işletim sistemi yükseltmeleri için sorumlu. Sonuçta verilerin önbelleğe alınmış şirket içi olmasını gerektirir, son olarak, çoğaltma teknolojileriyle gibi dağıtılmış dosya sistemi çoğaltma (gerçekleşen yapmak için DFSR), ayarlamanıza olanak için olmasına.

    En iyi Azure dosyaları ve (Azure diskleri arka uç depolama alanı olarak kullanarak ek olarak) Azure sanal makinelerinde barındırılan bir dosya sunucusu alınırken bir yaklaşım VM bulut üzerinde barındırılan bir dosya sunucusundaki Azure dosya eşitleme yüklemektir. Azure dosya paylaşımı, dosya sunucusu ile aynı bölgede ise, katmanlama ve birim boş alan yüzdesi (% 99) en yüksek düzeye ayarlanıyor bulut etkinleştirebilirsiniz. Bu veriler, en az çoğaltılmasını sağlar. NFS Protokolü gerektiren uygulamalar desteği gibi dosya sunucularınızda ile istediğiniz herhangi bir uygulama da kullanabilirsiniz.

    Azure yüksek performanslı ve yüksek oranda kullanılabilir dosya sunucusu kurmak için bir seçenek hakkında daha fazla bilgi için bkz: [dağıtma Iaas VM Konuk kümeleri Microsoft Azure'da](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Azure dosyaları ile Azure diskler arasındaki farklar daha ayrıntılı bir açıklaması için bkz [Azure Blob storage, Azure dosyaları ya da Azure diskleri kullanmaya karar verme](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Azure diskleri hakkında daha fazla bilgi için bkz: [Azure yönetilen diskleri genel bakış](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>**Azure dosyaları kullanarak çalışmaya nasıl?**  
   Azure dosyaları ile başlamak kolaydır. İlk olarak, [bir dosya paylaşımı oluşturmak](storage-how-to-create-file-share.md)ve tercih edilen işletim sisteminizi Bağla: 

    * [Windows'da bağlama](storage-how-to-use-files-windows.md)
    * [Linux bağlama](storage-how-to-use-files-linux.md)
    * [MacOS içinde bağlama](storage-how-to-use-files-mac.md)

   Üretim dosya paylaşımları, kuruluşunuzda değiştirmek Azure dosya paylaşımının dağıtma hakkında daha ayrıntılı kılavuzu için bkz: [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md).

* <a id="redundancy-options"></a>**Hangi depolama artıklığı seçeneği Azure dosyaları tarafından destekleniyor mu?**  
    Şu anda Azure dosyaları yalnızca yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS) destekler. Bölge olarak yedekli depolama (ZRS) ve okuma erişimli coğrafi olarak yedekli (RA-GRS) depolama gelecekte desteği planlıyoruz, ancak şu anda paylaşmak için zaman çizelgesi bulunmuyor.

* <a id="tier-options"></a>**Hangi depolama katmanları Azure dosyalarında destekleniyor mu?**  
    Azure dosyaları şu anda, yalnızca standart depolama katmanı destekler. Biz, premium depolama ve seyrek erişimli depolama şu anda desteği için paylaşmak için zaman çizelgesi yok. 
    
    > [!NOTE]
    > Azure dosya paylaşımları yalnızca blob depolama hesapları arasından veya premium depolama hesapları arasından oluşturulamıyor.

* <a id="give-us-feedback"></a>**Gerçekten Azure dosyalara eklenen belirli bir özellik görmek istiyorum. Ekleyebilir miyim?**  
    Azure dosyaları takım hizmetimizi hakkında sahip olduğunuz tüm geri bildirim işitme içinde ilgileniyor. Lütfen özellik istekleri oylamak [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! İleri için birçok yeni özellik ile delighting bekliyoruz.

## <a name="azure-file-sync"></a>Azure dosya eşitleme
* <a id="afs-region-availability"></a>**Hangi bölgeleri Azure dosya eşitleme (Önizleme) destekleniyor mu?**  
    Şu anda, Batı ABD, Batı Avrupa, Doğu Avustralya ve Güneydoğu Asya Azure dosya eşitleme kullanılabilir. Biz genel kullanılabilirlik doğru çalışırken daha fazla bölgeler için destek eklenecektir. Daha fazla bilgi için bkz: [bölge kullanılabilirliği](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>**Etki alanına katılmış ve etki alanına katılmış sunucuları aynı eşitleme grubuna sahip olabilir?**  
    Evet. Bunlar etki alanına katılmış olmasa bile bir eşitleme grubu farklı Active Directory üyelikleri, yüklü sunucu uç noktaları içerebilir. Bu yapılandırma teknik kullanılabilse de, bir sunucu üzerindeki dosya ve klasörleri için tanımlanan erişim denetim listelerini (ACL'ler) eşitleme grubundaki diğer sunucular tarafından zorlanacak kuramamış olabilir çünkü bu normal bir yapılandırma önermiyoruz. En iyi sonuçlar için aynı Active Directory ormanında olan sunucular arasında farklı Active Directory ormanlarında olan ancak güven ilişkileri kurulmuş sunucular arasında ya da bir etki alanında olmayan sunucular arasında eşitleniyor öneririz. Bu yapılandırmalar bir karışımını kullanmaktan kaçının öneririz.

* <a id="afs-change-detection"></a>**Bir dosyayı doğrudan my Azure dosya paylaşımı SMB kullanarak veya Portalı'nda oluşturdum. Eşitleme gruptaki sunucular için eşitleme dosyaya ne kadar sürer?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Aynı dosya iki sunucularda yaklaşık aynı zamanda değiştirdiyseniz ne olur?**  
    Azure dosya eşitleme kullanan basit çakışma çözümü stratejisi: biz iki sunucu üzerinde aynı anda değiştirilen dosyalar için her iki değişiklik tutun. En kısa süre önce yazılmış değişiklik özgün dosya adı tutar. Eski dosya "kaynak" makinenin ve adına eklenen çakışma numarası vardır. Bu sınıflandırma izler: 
   
    \<FileNameWithoutExtension\>-\<MachineName\>\[-#\].\< ext\>  

    Örneğin, CentralServer eski yazma oluştuğu ise CompanyReport.docx ilk ihtilaf CompanyReport CentralServer.docx olur. İkinci çakışma CompanyReport CentralServer 1.docx adlı.

* <a id="afs-storage-redundancy"></a>**Coğrafi olarak yedekli depolama için Azure dosya eşitleme destekleniyor mu?**  
    Evet, Azure dosyaları yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS) destekler. Eşleştirilmiş bölgeler arasında GRS bir yük devretme gerçekleşirse yeni bölge verileri yalnızca bir yedek olarak davran öneririz. Azure dosya eşitleme, yeni birincil bölge ile eşitleniyor otomatik olarak başlamaz. 

* <a id="sizeondisk-versus-size"></a>**Neden değil *disk boyutuna* özelliği dosya eşleşmesi için *boyutu* Azure dosya eşitleme kullandıktan sonra özelliği?**  
    Windows dosya Gezgini dosyasının boyutunu temsil etmek için iki özellik sunar: **boyutu** ve **disk boyutuna**. Bu özellikleri dilden çok az anlamları farklılık gösterir. **Boyutu** dosyasının tam boyutunu temsil eder. **Disk boyutu** diskte depolanan dosya akışı boyutunu temsil eder. Bu özelliklerin değerlerini çeşitli nedenlerle sıkıştırma gibi farklı, yinelenen verileri kaldırma kullanın veya Azure dosya eşitleme ile katmanlama bulut. Bir dosyayı bir Azure dosya paylaşımına katmanlı, dosya akışı, Azure dosya paylaşımı ve diskteki depolandığı için disk boyutu sıfır olur. Ayrıca, bir dosyanın kısmen katmanlı (veya kısmen geri çekilen) olması için de mümkündür. Kısmen katmanlı bir dosyada dosya diskte parçasıdır. Dosyaları multimedya oynatıcıları gibi uygulamalar tarafından kısmen okuma veya yardımcı programlarını zip olduğunda bu durum oluşabilir. 

* <a id="is-my-file-tiered"></a>**Bir dosya katmanlı olup olmadığını nasıl anlayabilirim?**  
    Azure dosya paylaşımınıza dosya katmanlı olup olmadığını denetlemek için birkaç yolu vardır:
    
   *  **Dosyanın dosya özniteliklerini denetleyin.**
     Bunu yapmak için bir dosyaya sağ tıklayın, Git **ayrıntıları**ve ardından aşağı kaydırın **öznitelikleri** özelliği. Katmanlı bir dosya kümesi aşağıdaki özniteliklere sahiptir:     
        
        | Öznitelik harf | Öznitelik | Tanım |
        |:----------------:|-----------|------------|
        | A | Arşiv | Yedekleme yazılımı tarafından dosya yedeklenmelidir gösterir. Bu öznitelik her zaman, dosyanın veya katmanlı tam diskte depolanan bağımsız olarak ayarlanır. |
        | P | Seyrek dosya | Dosyası seyrek dosya olduğunu gösterir. Disk akış dosyada çoğunlukla boş olduğunda seyrek dosya NTFS sunan dosyayı verimli kullanım için özel bir türde değil. Bir dosya katmanlı tamamen veya kısmen geri için azure dosya eşitleme seyrek dosyaları kullanır. Tam olarak katmanlı bir dosyada dosya akışı bulutta depolanır. Kısmen geri çekilen bir dosyada dosyasının bu bölümü diskte zaten var. Tam olarak dosyasıysa, diske geri, Azure dosya eşitleme seyrek dosyasından normal bir dosyaya dönüştürür. |
        | L | Yeniden ayrıştırma noktası | Dosya ayrıştırma noktası olduğunu gösterir. Bir dosya sistemi Filtresi tarafından kullanılmak üzere özel bir işaretçi bir ayrıştırma noktasıdır. Azure dosya eşitleme yeniden ayrıştırma noktaları Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyasının depolandığı bulut konumu tanımlamak için kullanır. Bu, kesintisiz erişimi destekler. Kullanıcılar Azure dosya eşitleme kullanılmakta olduğunu bilmeniz gereken olmaz veya Azure dosya paylaşımınıza dosyasında erişmek nasıl. Bir dosya tam olarak çağrılır, Azure dosya eşitleme yeniden ayrıştırma noktası dosyasından kaldırır. |
        | O | Çevrimdışı | Dosyanın içeriğini tümünün veya diske depolanmış olmayan gösterir. Bir dosya tam olarak çağrılır, Azure dosya eşitleme bu öznitelik kaldırır. |

        ![Ayrıntılar sekmesi seçili bir dosya için özellikleri iletişim kutusu](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Bir klasördeki tüm dosyalar için öznitelikler ekleyerek görebilirsiniz **öznitelikleri** dosya Gezgini tablo görüntüsünü alanı. Bunu yapmak için varolan bir sütunu üzerinde sağ tıklayın (örneğin, **boyutu**), select **daha fazla**ve ardından **öznitelikleri** aşağı açılan listeden.
        
   * **Kullanım `fsutil` bir dosyayı yeniden ayrıştırma noktalarında denetlemek için.**
       Önceki seçenek açıklandığı gibi katmanlı bir dosya kümesi'nin üzerine yeniden ayrıştırma her zaman vardır. Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) için özel bir işaretçi bir yeniden ayrıştırma işaretçidir. Bir dosya yükseltilmiş bir komut istemi veya PowerShell penceresinde bir yeniden ayrıştırma noktası olup olmadığını denetlemek için çalıştırın `fsutil` yardımcı programı:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Dosya ayrıştırma noktası varsa, görmeyi bekleyebilirsiniz **yeniden ayrıştırma etiket değeri: 0x8000001e**. Azure dosya eşitleme tarafından sahip olunan yeniden ayrıştırma noktası değer bu onaltılık değeridir. Çıkış dosyanızın Azure dosya paylaşımınızda yolunu temsil yeniden ayrıştırma verilerini de içerir.

        > [!WARNING]  
        > `fsutil reparsepoint` Yardımcı programı komutu yeniden ayrıştırma noktası silme olanağı da vardır. Azure dosya eşitleme mühendislik ekibi, ister sürece bu komutu yürütmek değil. Bu komutu çalıştırmak, veri kaybına neden olabilir. 

* <a id="afs-recall-file"></a>**Kullanmak istediğiniz bir dosya katmanlı. Yerel olarak kullanılacak disk dosyasına nasıl geri çağırma?**  
    Bir dosyayı diske geri çekmek için en kolay yolu dosyayı açmaktır. Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyasını, Azure dosya paylaşımından çaba herhangi bir iş olmadan sorunsuz bir şekilde indirir. Kısmen olabilir dosya türleri için okuma, bir dosyanın açılması multimedya veya .zip dosyaları gibi dosyanın tamamı indirilmedi.

    PowerShell çekilmesine dosyaya zorlamak için de kullanabilirsiniz. Bu seçenek, bir klasördeki tüm dosyaları gibi birden çok dosyaları aynı anda geri çekmek istediğiniz durumlarda yararlı olabilir. Azure dosya eşitleme yüklü olduğu sunucu düğümü için bir PowerShell oturumu açın ve ardından aşağıdaki PowerShell komutlarını çalıştırın:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>**Nasıl t bir dosya veya dizin katmanlı olması zorunlu?**  
    Bulut katmanlama özelliği etkinleştirilmişse, otomatik olarak katmanları dosyaları katmanlama bulut son erişimini kullanarak ve bulut uç noktası üzerinde belirtilen birim boş alan yüzdesi elde etmek için kez değiştirin. Bazı durumlarda, yine de el ile katmanı için bir dosya zorlamak isteyebilirsiniz. Bu uzun süredir yeniden kullanmayı düşünmüyorsanız büyük bir dosya kaydederseniz kullanışlı olabilir ve diğer dosyalar ve klasörler için kullanmak için birim üzerindeki şimdi boş alanı istiyorsunuz. Aşağıdaki PowerShell komutlarını kullanarak katmanlama uygulayabilirsiniz:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-files-excluded"></a>**Otomatik olarak Azure dosya eşitleme tarafından hangi dosya veya klasörleri hariç tutulur?**  
    Varsayılan olarak, Azure dosya eşitleme aşağıdaki dosyaları hariç tutar:
    * Desktop.ini
    * Thumbs.DB
    * ehthumbs.DB
    * ~$\*.\*
    * \*.laccdb
    * \*.tmp
    * 635D02A9D91C401B97884B82B3BCDAEA.\*

    Aşağıdaki klasörlerin de varsayılan olarak tutulur:

    * \System volume Information
    * \$GERİ DÖNÜŞÜM. DEPO
    * \SyncShareState

* <a id="afs-os-support"></a>**Azure dosya eşitleme Windows Server 2008 R2, Linux veya ağa bağlı depolama (NAS) cihazımı ile kullanabilir miyim?**  
    Şu anda yalnızca Windows Server 2012 R2 ve Windows Server 2016 Azure dosya eşitleme destekler. Şu anda biz biz paylaşabilirsiniz planların sunulmadı, ancak müşteri talebe göre ek platformlar desteklemek için açık olarak çalışıyoruz. Konumundaki bize bildirin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) hangi platformları bize desteklemek için istersiniz.

## <a name="security-authentication-and-access-control"></a>Güvenlik, kimlik doğrulama ve erişim denetimi
* <a id="ad-support"></a>**Active Directory tabanlı kimlik doğrulaması ve Azure dosyaları tarafından desteklenen erişim denetimi nedir?**  
    Azure dosyaları erişim denetimini yönetmek için iki seçenek sunar:

    - Paylaşılan erişim imzaları (SAS), belirli izinlere sahip ve belirtilen zaman aralığı için geçerli olan belirteçleri oluşturmak için kullanabilirsiniz. Örneğin, 10 dakikalık süre sonu olan belirli bir dosya salt okunur erişimi olan bir belirteç oluşturabilirsiniz. Herkes belirtecin geçerli olduğu halde belirtece sahip salt okunur dosya bu 10 dakika erişebilir. Şu anda, paylaşılan erişim imzası anahtarları yalnızca REST API aracılığıyla veya istemci kitaplıkları desteklenmektedir. Depolama hesabı anahtarlarını kullanarak Azure dosya paylaşımını SMB üzerinden bağlama gerekir.

    - Azure dosya eşitleme korur ve tüm isteğe bağlı ACL'lerde veya DACL'leri (Active Directory tabanlı veya yerel olup olmadığını) çoğaltır için eşitlenen tüm sunucu uç noktaları için. Windows Server Active Directory ile zaten kimliğini doğrulamak için Azure dosya eşitleme Active Directory tabanlı kimlik doğrulaması için tam destek kadar etkili bir Dur boşluk seçenek ve ACL desteği ulaşan.

    Şu anda Azure dosyaları Active Directory doğrudan desteklemez.

* <a id="encryption-at-rest"></a>**My Azure dosya paylaşımı bekleme sırasında şifrelenir nasıl sağlayabilirsiniz?**  
    Azure depolama hizmeti şifrelemesi, tüm bölgelerde varsayılan olarak etkinleştirilen sürecinde ' dir. Bu bölgeler için şifrelemeyi etkinleştirmek için herhangi bir eylem gerçekleştirmenizi gerekmez. Diğer bölgeler için bkz: [sunucu tarafı şifreleme](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>**Nasıl ı belirli bir dosyaya erişimi bir web tarayıcısı kullanarak sağlayabilir miyim?**  
    Paylaşılan erişim imzaları özel izinlere sahip ve belirtilen zaman aralığı için geçerli olan belirteçleri oluşturmak için kullanabilirsiniz. Örneğin, belirli bir dosya için ayarlanmış bir süre için salt okunur erişim sağlayan bir belirteç oluşturabilirsiniz. Belirtecin geçerli olduğu halde URL sahip olan herkes dosyanın tüm web tarayıcılarından doğrudan erişebilirsiniz. Depolama Gezgini gibi bir kullanıcı Arabirimi bir paylaşılan erişim imzası tuşu kolayca oluşturabilirsiniz.

* <a id="file-level-permissions"></a>**Salt okunur tanıyabilir veya paylaşımdaki klasörlere yalnızca yazma izinleri mı?**  
    SMB kullanılarak dosya paylaşımını bağlama, klasör düzeyinde izinler denetime yok. Ancak, REST API veya istemci kitaplıkları aracılığıyla paylaşılan erişim imzası oluşturursanız, paylaşım klasörlere salt okunur veya sadece yazılabilir izinleri belirtebilirsiniz.

* <a id="ip-restrictions"></a>**Bir Azure dosya paylaşımı için IP kısıtlamalarını uygulamak?**  
    Evet. Azure dosya paylaşımına erişimi depolama hesabı düzeyinde kısıtlanabilir. Daha fazla bilgi için bkz: [Azure depolama güvenlik duvarlarını yapılandırın ve sanal ağlar](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>**Hangi veri uyumluluk ilkeleri, Azure dosyaları destekliyor mu?**  
   Azure dosyaları Azure Storage diğer depolama Hizmetleri'ndeki kullanılan aynı depolama mimarisi üstünde çalışır. Azure dosyaları diğer Azure storage hizmetleri kullanılan veri uyumluluk ilkelerini uygular. Azure Storage veri uyumluluğu hakkında daha fazla bilgi için indirin ve başvurmak [Microsoft Azure veri koruması belge](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409)ve Git [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

## <a name="on-premises-access"></a>Şirket içi erişim
* <a id="expressroute-not-required"></a>**Veya şirket içi Azure dosya eşitleme kullanmak için Azure dosyalarına bağlanmak için Azure ExpressRoute kullanmak zorunda mıyım?**  
    Hayır. ExpressRoute, Azure dosya paylaşımına erişmek için gerekli değildir. Takma durumunda Azure dosya internet erişimiyle (Bu SMB iletişim kurmak için kullandığı bağlantı noktasıdır) bağlantı noktası 445 (TCP Giden) için tüm, gerekli olan şirket olan doğrudan paylaşabilirsiniz. Azure dosya eşitleme kullanıyorsanız, gerekli olan tek şey HTTPS erişim (SMB gerekli) için bağlantı noktası 443 (TCP Giden). Ancak, siz *için* ExpressRoute bu erişim seçeneklerden birini kullanın.

* <a id="mount-locally"></a>**Nasıl ı my yerel makinede bir Azure dosya paylaşımı bağlayabilir?**  
    Bağlantı noktası 445 (TCP Giden) açıksa ve (örneğin, Windows 10 veya Windows Server 2016 kullanıyorsanız), istemci SMB 3.0 protokolünü destekleyen SMB protokolünü kullanarak dosya paylaşımı bağlayabilir. Bağlantı noktası 445, ISS veya kuruluşunuzun İlkesi tarafından engellenirse, Azure dosya paylaşımına erişmek için Azure dosya eşitleme kullanabilirsiniz.

## <a name="backup"></a>Backup
* <a id="backup-share"></a>**I my Azure dosyasını yedekleyin nasıl paylaşma?**  
    Kullanabileceğiniz düzenli [paylaşmak anlık görüntüler (Önizleme)](storage-how-to-use-files-snapshots.md) yanlışlıkla silmeleri karşı koruma için. AzCopy, Robocopy veya bir bağlı dosya paylaşımı yedekleyebilirsiniz bir üçüncü taraf yedekleme aracı de kullanabilirsiniz. 

## <a name="share-snapshots"></a>Paylaşım anlık görüntüler
### <a name="share-snapshots-general"></a>Paylaşma anlık görüntüler: Genel
* <a id="what-are-snaphots"></a>**Dosya Paylaşımı anlık görüntüleri nelerdir?**  
    Azure dosya paylaşımı anlık görüntüler, dosya paylaşımları, salt okunur bir sürümünü oluşturmak için kullanabilirsiniz. Azure dosyaları, alternatif bir konuma Azure ya da şirket içi daha fazla değişiklikler için aynı paylaşımına içeriği geri önceki bir sürümünü kopyalamak için de kullanabilirsiniz. Paylaşım anlık görüntüler hakkında daha fazla bilgi için bkz: [paylaşmak anlık görüntü genel bakış](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>**My paylaşımı anlık görüntüleri depolandığı?**  
    Paylaşım anlık görüntüleri aynı depolama hesabında dosya paylaşımına olarak depolanır.

* <a id="snapshot-perf-impact"></a>**Paylaşım anlık görüntülerini kullanarak tüm performans etkileri var mı?**  
    Paylaşım anlık görüntüleri herhangi performansa sahip değilsiniz.

* <a id="snapshot-consistency"></a>**Paylaşım anlık görüntüleri uygulama tutarlı misiniz?**  
    Hayır, paylaşım anlık görüntüleri uygulama tutarlı değil. Kullanıcı uygulamadan önce paylaşım anlık paylaşımına yazma işlemlerini boşaltmaya gerekir.

* <a id="snapshot-limits"></a>**Kullanabilirim paylaşımı anlık görüntü sayısı sınır var mıdır?**  
    Evet. Azure dosyaları en fazla 200 paylaşımı anlık görüntüleri tutabilirsiniz. Bu yüzden paylaşımı anlık görüntüleri paylaşım başına sınır tüm paylaşım anlık görüntüleri tarafından kullanılan toplam alanı paylaşım kotası doğru sayılmaz. Depolama hesabı sınırları hala geçerlidir. 200 paylaşımı anlık görüntüleri sonra yeni paylaşım anlık görüntüleri oluşturmak için eski anlık görüntüleri silmeniz gerekir.

### <a name="create-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma
* <a id="file-snaphsots"></a>**Tek tek dosyaların paylaşımı anlık görüntü oluşturabilir miyim?**  
    Paylaşım anlık görüntüleri dosya paylaşımı düzeyinde oluşturulur. Tek tek dosyaların dosya paylaşımı anlık görüntüden geri yükleyebilirsiniz, ancak dosya düzeyinde paylaşımı anlık görüntüleri oluşturamıyor. Ancak, paylaşım düzeyi paylaşımı anlık görüntü aldıysanız ve belirli bir dosya burada değişti paylaşımı anlık görüntüleri listelemek istediğiniz eklerseniz altında bunu yapabilirsiniz **önceki sürümler** Windows monte paylaşımında. 
    
    Bir dosya anlık görüntü özelliği gerekiyorsa, bize en bildirin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>**Bir şifrelenmiş dosya paylaşımının paylaşım anlık görüntü oluşturabilir miyim?**  
    Etkin bekleyen şifreleme sahip Azure dosya paylaşımları bir paylaşım anlık görüntüsünü. Bir şifrelenmiş dosya paylaşımına dosya paylaşımı anlık görüntüden geri yükleyebilirsiniz. Paylaşımınıza şifrelenmişse, paylaşım anlık görüntünüz de şifrelenir.

* <a id="geo-redundant-snaphsots"></a>**My paylaşımı anlık görüntüleri coğrafi olarak yedekli misiniz?**  
    Paylaşım anlık görüntüler, bunlar gerçekleştirilen Azure dosya paylaşımı olarak aynı artıklık sağlamak. Coğrafi olarak yedekli depolama hesabınız için seçtiyseniz, ayrıca anlık görüntü paylaşımınıza nedenle eşleştirilmiş bölgede depolanır.

### <a name="manage-share-snapshots"></a>Paylaşım anlık görüntülerini yönetme
* <a id="browse-snapshots-linux"></a>**Linux alınan my paylaşımı anlık göz atabilirsiniz?**  
    Azure CLI 2.0 oluşturmak, liste, göz atın ve Linux paylaşımı anlık görüntüleri geri yüklemek için kullanabilirsiniz.

* <a id="copy-snapshots-to-other-storage-account"></a>**Farklı bir depolama hesabı için paylaşımı anlık görüntüleri kopyalayabilir miyim?**  
    Dosya Paylaşımı anlık görüntülerden başka bir konuma kopyalayabilirsiniz, ancak paylaşım anlık kopyalanamıyor.

### <a name="restore-data-from-share-snapshots"></a>Veri paylaşımı anlık görüntülerden geri yükleme
* <a id="promote-share-snapshot"></a>**Bir paylaşım anlık temel paylaşımına yükseltme?**  
    Diğer bir hedefe paylaşımı anlık görüntüden veri kopyalayabilirsiniz. Bir paylaşım anlık temel paylaşımına yükseltemezsiniz.

* <a id="restore-snapshotted-file-to-other-share"></a>**I veri my paylaşımı anlık görüntüden farklı bir depolama hesabı için geri yükleyebilir miyim?**  
    Evet. Paylaşım anlık görüntü dosyaları özgün konuma veya aynı depolama hesabı veya farklı bir depolama hesabı, aynı bölgede ya da farklı bölgelerde içeren farklı bir konuma kopyalanabilir. Ayrıca, bir şirket içi konumuna veya başka bir bulut dosyalarını kopyalayabilirsiniz.    
  
### <a name="clean-up-share-snapshots"></a>Paylaşım anlık görüntüler Temizle
* <a id="delete-share-keep-snapshots"></a>**Edebilir miyim my paylaşımını silmek ancak my paylaşımı anlık görüntüleri silin değil?**  
    Paylaşımı üzerinde etkin paylaşımı anlık görüntüler varsa paylaşımınıza silemezsiniz. Bir API paylaşımıyla birlikte paylaşımı anlık görüntüleri silmek için kullanabilirsiniz. Ayrıca, hem paylaşım anlık görüntüleri hem de Azure portalında paylaşımı silebilirsiniz.

* <a id="delete-share-with-snapshots"></a>**Depolama Hesabımı silerseniz my paylaşımı anlık görüntüleri ne olur?**  
    Depolama hesabınız silerseniz, paylaşım anlık görüntüleri de silinir.

## <a name="billing-and-pricing"></a>Faturalama ve fiyatlandırma
* <a id="vm-file-share-network-traffic"></a>**Ağ ücreti aboneliği yansıtılan harici bir bant genişliği olarak bir Azure VM ile bir Azure dosya paylaşımı sayısı arasında trafiği mu?**  
    Dosya Paylaşımı ve VM aynı Azure bölgesinde ise, dosya paylaşımı ve VM arasındaki trafik için ek ücret yoktur. Dosya Paylaşımı ve VM farklı bölgelerde bulunuyorsa, aralarındaki trafik harici bant genişliği olarak ücretlendirilirsiniz.

* <a id="share-snapshot-price"></a>**Anlık görüntüler maliyeti ne kadar paylaşmak?**  
     Önizleme sırasında ücretsizdir paylaşımı anlık görüntü kapasitesi. Standart depolama çıkış ve işlem maliyetleri uygulayın. Genel kullanılabilirlik sonrasında, abonelikleri kapasite ve Paylaşım anlık görüntü üzerinde işlemler için sizden ücret alınır.
     
     Doğası gereği artımlı paylaşımı anlık görüntüler. Paylaşım temel paylaşımı anlık görüntüsüdür. Tüm sonraki paylaşımı anlık görüntüleri artımlı ve yalnızca önceki paylaşım anlık görüntü farkı depolayın. Yalnızca değiştirilen içerik için faturalandırılır. Veri 100 Gib'den Paylaş sahip, ancak yalnızca 5 Gib'den son paylaşımı anlık bu yana değişti, yalnızca 5 ek Gib'den paylaşımı anlık görüntü kullanır ve 105 Gib'den için faturalandırılır. İşlem ve standart çıkış ücretlerini hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="scale-and-performance"></a>Ölçek ve performans
* <a id="files-scale-limits"></a>**Azure dosyaları ölçek sınırları nelerdir?**  
    Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında bilgi için bkz: [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>**Azure dosyaları şu anda sunar daha büyük bir dosya paylaşımı ihtiyacım. My Azure dosya paylaşımı boyutunu artırın?**  
    Hayır. Azure dosya paylaşımının en büyük boyutu 5 Tıb ' dir. Şu anda, biz ayarlayamaz sabit bir sınıra budur. 100 TiB paylaşımı boyutunu artırmak için bir çözüm üzerinde çalışıyoruz ancak şu anda paylaşmak için zaman çizelgesi bulunmuyor.

* <a id="open-handles-quota"></a>**Kaç adet istemcinin aynı dosyayı aynı anda erişebilir mi?**   
    Tek dosya üzerinde 2.000 açık tanıtıcıların kotası yoktur. 2.000 açık tanıtıcıların olduğunda kotasına ulaşıldığında belirten bir hata iletisi görüntülenir.

* <a id="zip-slow-performance"></a>**Azure dosyaları sıkıştırmasını zaman my performansı düşüktür. Ne yapmalıyım?**  
    Azure dosyaları için çok sayıda dosya aktarmak için AzCopy (Windows için; Önizleme Linux ve UNIX için) veya Azure PowerShell kullanmanızı öneririz. Bu araçlar ağ aktarımı için optimize edilmiştir.

* <a id="slow-perf-windows-81-2012r2"></a>**My Azure dosya paylaşımı Windows Server 2012 R2 veya Windows 8.1 bağladıktan sonra neden performansta yavaş mı?**  
    Azure dosya paylaşımının Windows Server 2012 R2 ve Windows 8.1 bağlarken bilinen bir sorun yoktur. Sorunu Nisan 2014 toplu güncelleştirme'nin Windows 8.1 ve Windows Server 2012 R2 için oluşturulmuştur. En iyi performansı elde etmek için Windows Server 2012 R2 ve Windows 8.1 tüm örnekleri uygulanan bu düzeltme eki olduğundan emin olun. (Her zaman Windows düzeltme ekleri Windows Update ile de alacaksınız.) Daha fazla bilgi için ilişkili Microsoft Bilgi Bankası makalesine bakın [Windows 8.1 veya Server 2012 R2'den Azure dosyaları eriştiğinizde yavaş performans](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Özellikleri ve diğer hizmetleri ile birlikte çalışabilirlik
* <a id="cluster-witness"></a>**My Azure dosya paylaşımı olarak kullanmak bir *dosya paylaşımı tanığı* my Windows Server Yük devretme kümesi için mi?**  
    Şu anda bu yapılandırma, bir Azure dosya paylaşımı için desteklenmiyor. Bu Azure Blob Depolama için ayarlama hakkında daha fazla bilgi için bkz: [bir yük devretme kümesi için bir bulut tanığı dağıtmak](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>**Azure kapsayıcı örneğinde Azure dosya paylaşımının takabilir miyim?**  
    Evet, Azure dosya paylaşımları bir kapsayıcı örnek ömür ötesinde bilgi kalıcı hale getirmek istediğinizde iyi bir seçenektir. Daha fazla bilgi için bkz: [Azure kapsayıcı örneği ile bir Azure dosya paylaşımının](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>**REST API içinde bir yeniden adlandırma işlemi var mı?**  
    Şu anda değil.

* <a id="nested-shares"></a>**İç içe geçmiş paylaşımları ayarlayabilir miyim? Diğer bir deyişle, bir paylaşım kapsamında?**  
    Hayır. Dosya Paylaşımı *olan* iç içe paylaşımlar desteklenmez, böylece, bağlayabileceğiniz sanal bir sunucudur.

* <a id="ibm-mq"></a>**Azure dosyaları ile IBM MQ nasıl kullanabilirim?**  
    IBM, IBM MQ müşterileri Azure dosyaları IBM hizmeti ile yapılandırma yardımcı olacak bir belge yayımladı. Daha fazla bilgi için bkz: [bir Microsoft Azure dosya hizmeti ile IBM MQ çok örnekli kuyruk yöneticisini kurma](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).

## <a name="see-also"></a>Ayrıca bkz.
* [Windows Azure dosyaları sorun giderme](storage-troubleshoot-windows-file-connection-problems.md)
* [Linux Azure dosyalarında sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)
* [Azure dosya eşitleme (Önizleme) sorunlarını giderme](storage-sync-files-troubleshoot.md)
