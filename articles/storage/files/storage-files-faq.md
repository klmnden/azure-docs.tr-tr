---
title: "Azure dosyaları hakkında sık sorulan sorular | Microsoft Docs"
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
ms.openlocfilehash: 91c46ea0897f83811cfa5c1a8b67677caff14569
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="frequently-asked-questions-about-azure-files"></a>Azure dosyaları hakkında sık sorulan sorular
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları, endüstri standardı erişilebilir bulutta sunar [sunucu ileti bloğu (SMB) Protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (ortak Internet dosya sistemi veya CIFS olarak da bilinir). Azure Dosya paylaşımları Windows, Linux ve macOS bulut ve şirket içi dağıtımları tarafından aynı anda bağlanabilir. Ayrıca, Azure dosya paylaşımları Azure dosya eşitleme (Önizleme) ile Windows sunucularında verileri nerede kullanılıyor yakın hızlı erişim için önbelleğe alınabilir.

Bu makalede Azure dosyaları özellikleri ve işlevleri, Azure dosya eşitleme dahil olmak üzere hakkında sık sorulan soruların bazıları ele alınacaktır. Yanıt sorunuzun yanıtını burada görmüyorsanız, (sipariş yükselen içinde) aşağıdaki kanallar aracılığıyla Bize Ulaşın çekinmeyin:

1. Bu makalede Açıklamalar bölümüne.
2. [Azure depolama Forumu](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: yeni bir destek servis talebi oluşturmak için gezinme "Yardım + destek için" sekmesinde Azure portalda ve "Yeni destek isteği"'i tıklatın.

## <a name="general"></a>Genel
* <a id="why-files-useful"></a>**Azure dosyaları neden yararlıdır?**  
   Azure dosyaları dosya paylaşımları bir fiziksel sunucu veya cihaz/Gereci yükü yönetmek zorunda kalmadan bulutta oluşturmanıza olanak sağlar. Bu işletim sistemi güncelleştirmelerini uygulama ve hatalı disklerin yerini daha az zaman harcayabilir - biz ve sıkıcı bu çalışmaların tamamını için bunu gelir. Azure dosyaları ile yardımcı senaryoları hakkında daha fazla bilgi için bkz: [neden Azure dosyaları yararlıdır](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>**Azure dosyaları dosyalarında erişim için farklı yollar nelerdir?**  
    SMB 3.0 protokolünü kullanarak yerel makinenizde dosya paylaşımı bağlayabilir veya araçlarını kullanın ister [Depolama Gezgini](http://storageexplorer.com/) , dosya paylaşımı içindeki dosyalara erişmek için. Uygulamanızdan dosyalarınızı Azure dosya paylaşımına erişmek için depolama istemci kitaplığı, REST API'leri, PowerShell veya CLI kullanabilirsiniz.

* <a id="what-is-afs"></a>**Azure dosya eşitleme nedir?**  
    Azure dosya eşitleme esneklik, performans ve uyumluluk bir şirket içi dosya sunucusunun vermeden kuruluşunuzun dosya paylaşımları Azure dosyalarında merkezileştirmenizi sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

* <a id="files-versus-blobs"></a>**Verilerim için neden bir Azure dosya paylaşımı Azure Blob Depolama karşı kullanmam?**  
    Azure dosyaları ve Azure Blob Depolama hem bulutta büyük miktarlarda veri depolamak için bir yol sağlar, ancak biraz farklı amaçlar için yararlıdır. Azure Blob storage yapılandırılmamış verileri depolamak için gereken büyük ölçekli, bulut yerel uygulamalar için kullanışlıdır. Performansı ve ölçeği en üst düzeye çıkarmak için Azure Blob Depolama bir basit depolama doğru dosya sistemi daha soyutlamadır. Ayrıca, Azure Blob Depolama yalnızca REST tabanlı istemci kitaplıkları (veya doğrudan REST tabanlı bir protokol üzerinden) erişebilir.

    Azure dosyaları diğer yandan özellikle aradığı tüm bilmeniz ve şirket içi işletim sistemlerinin yıl memnuniyet dosya özetleri ile bir dosya sistemi olması. Azure Blob Depolama alanı gibi Azure dosyaları bir REST arabirimi ve REST tabanlı istemci kitaplıkları sağlar, ancak Azure Blob depolamadan farklı olarak, Azure dosyaları Azure dosya paylaşımları için SMB erişimini de sunar. Bu, doğrudan bir Azure dosya paylaşımı Windows, Linux veya macOS, şirket içi veya Bulut Vm'lerinde, herhangi bir kod yazmak veya herhangi bir özel sürücü dosya sistemine eklemek zorunda kalmadan bağlayabilir anlamına gelir. Ayrıca, Azure dosya paylaşımları şirket içi dosya sunucularındaki verileri nerede kullanılıyor yakın hızlı erişim için Azure dosya eşitleme kullanarak önbelleğe alınmış olabilir. 
   
    Azure dosyaları ile Azure Blob Depolama arasındaki farklar hakkında daha ayrıntılı bir tartışma için bkz: [Azure Blob storage, Azure dosyaları ya da Azure diskleri kullanmaya karar verme](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Azure Blob Depolama hakkında daha fazla bilgi için bkz: [Blob Storage'a giriş](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Azure diskleri yerine bir Azure dosya paylaşımı neden kullanabilir?**  
    Azure diski yalnızca, bir disk olmasıdır. Bir tek başına disk başına değer Azure diski dışında almak çok kullanışlı - değil, Azure'da çalışan sanal makine eklemek zorunda. Azure diskleri için herhangi bir şey kullanılabilir ve her şeyi bir disk için bir şirket içi sunucuda kullanmanız: bir işletim sistemi diski olarak ayrılmış depolama birimi olarak veya bir işletim sistemi için bir uygulama için değiştirme alanı. Bir ilginç Azure diskler için tam olarak aynı kullanımda yerleştirir buluta bir dosya sunucusu oluşturmak için kullanılır, Azure dosya paylaşımı kullanabilir. Azure VM'de bir dosya sunucusu dağıtma (NFS protokolü desteği ya da premium depolama gibi) şu anda Azure dosyaları tarafından desteklenen dağıtım seçenekleri gerektirdiğinde Azure dosya depolama alanı almak için harika, yüksek performanslı bir yoludur. 

    Diğer taraftan, arka uç olarak Azure disklerle çalıştıran bir dosya sunucu depolama genellikle çeşitli nedenlerle bir Azure dosya paylaşımı kullanmaktan çok daha pahalı olacaktır. İlk olarak, disk depolama için ödeme yanı sıra, aynı zamanda gider çalışan bir veya daha fazla Azure Vm'lerinin ödemeniz gerekir. İkinci olarak, işletim sistemi yükseltme, vb. için sorumlu olan gibi dosya sunucusu olarak çalıştırmak için kullanılan sanal makineleri yönetmeniz gerekir. Sonuçta şirket içi verileri önbelleğe gerektiriyorsa, son olarak, bunu size ayarlamak ve gerçekleşen yapmak için çoğaltma teknolojileri (örneğin, dağıtılmış dosya sistemi çoğaltma) yönetmek için bağlıdır.

    En iyi Azure dosyaları ve arka uç depolama alanı olarak Azure disklerle Azure vm'lerinde barındırılan bir dosya sunucusu almak için ilginç bir yaklaşım ise, bulut VM barındırılan dosya sunucunuzda Azure dosya eşitleme yüklemek için. Azure dosya paylaşımı, dosya sunucusu ile aynı bölgede ise, etkinleştirebilirsiniz katmanlama Bulut ve birim boş alan yüzdesi maksimum (% 99) olarak ayarlayın. Bu veriler, en az çoğaltılmasını verirken, dosya sunucularınızda karşı böyle herhangi bir şey NFS Protokolü gerektiren hangi uygulamaların destek kullanmanıza olanak sağlar.

    Yüksek performanslı ve yüksek oranda kullanılabilir bir dosya sunucusuna Azure ayarlama yollarından biri hakkında daha fazla bilgi için bkz: [Microsoft Azure Iaas VM Konuk kümesi dağıtma](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Azure dosyaları ile Azure diskler arasındaki farklar hakkında daha ayrıntılı bir tartışma için bkz: [Azure Blob storage, Azure dosyaları ya da Azure diskleri kullanmaya karar verme](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Azure diskleri hakkında daha fazla bilgi için bkz: [Azure yönetilen diskleri genel bakış](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>**Azure dosyaları kullanarak çalışmaya nasıl?**  
    Azure dosyaları ile çalışmaya başlama kolaydır: yalnızca [bir dosya paylaşımı oluşturmak](storage-how-to-create-file-share.md) ve bağlama, tercih edilen işletim sistemi: 

    - [Windows üzerinde bağlama](storage-how-to-use-files-windows.md)
    - [Linux'ta bağlama](storage-how-to-use-files-linux.md)
    - [MacOS üzerinde bağlama](storage-how-to-use-files-mac.md)

    Üretim dosya paylaşımları, kuruluşunuzda değiştirmek daha kapsamlı kılavuzu için bir Azure dosya paylaşımı dağıtma hakkında bkz: [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md).

* <a id="redundancy-options"></a>**Hangi depolama artıklığı seçeneği Azure dosyaları tarafından destekleniyor mu?**  
    Azure dosyaları şu anda yalnızca yerel olarak yedekli depolama (LRS) ve destekler coğrafi olarak yedekli depolama (GRS) şu anda. Biz bölge olarak yedekli depolama (ZRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) gelecekte desteği planlayın, ancak şu anda paylaşmak için zaman çizelgesi yok.

* <a id="tier-options"></a>**Hangi depolama katmanları, şu anda Azure dosyaları tarafından destekleniyor mu?**  
    Azure dosyaları şu anda yalnızca destekler standart depolama katmanı. Şu anda premium ve seyrek erişimli desteği için paylaşmak için zaman çizelgesi sahip değilsiniz. Yalnızca Blob Depolama hesapları ya da Premium depolama hesapları Azure dosya paylaşımları oluşturamayacağınızı unutmayın.

* <a id="give-us-feedback"></a>**Gerçekten görmek istiyorum *x* özelliği, ekleyebilir Azure dosyalara eklenen?**  
    Azure dosyaları takım hizmetimizi hakkında sahip olduğunuz tüm geri bildirim işitme içinde kesinlikle ilgileniyor. Lütfen özellik istekleri oylamak [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! İleri için birçok yeni özellik ile delighting bekliyoruz.

## <a name="azure-file-sync"></a>Azure dosya eşitleme
* <a id="afs-region-availability"></a>**Hangi bölgeler şu anda Azure dosya eşitleme (Önizleme) destekleniyor mu?**  
    Azure dosya eşitleme Batı ABD, Batı Avrupa, Doğu Avustralya ve Güneydoğu Asya şu anda kullanılamıyor. Biz genel kullanılabilirlik çalışırken ek bölgeler için destek ekleyeceğiz. Ek bilgi için bkz: [bölge kullanılabilirliği](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>**Aynı eşitleme grubunda etki alanına katılmış ve etki alanına katılmış sunucuları olabilir?**  
    Evet, bir eşitleme grubu sunucu etki alanına katılmış olmaması planlayacak farklı Active Directory üyeliği olan uç noktaları içerebilir. Yapılandırma teknik olarak çalışırken, bir sunucudaki dosyalar/klasörler için tanımlanmış ACL'lere eşitleme grubundaki diğer sunucular tarafından zorunlu mümkün olmayabilir gibi bu normal bir yapılandırma olarak önermiyoruz. En iyi sonuçlar için aynı Active Directory orman, farklı Active Directory ormanlarındaki oluşturulan güven ilişkileri olan sunucuları veya sunucuları değil bir etki alanı ancak olmayan bir karışımını Yukarıdakilerin tümü, her iki sunucu arasında eşitleniyor öneririz.

* <a id="afs-change-detection"></a>**Bir dosyayı doğrudan my Azure dosya paylaşımı SMB üzerinden veya portal üzerinden oluşturdum. Dosya eşitleme gruptaki sunucular için eşitlenen kadar ne kadar?** 
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Aynı dosya iki sunucularda yaklaşık aynı zamanda değiştirildi ne olur?**  
    Azure dosya eşitleme kullanan basit çakışma çözümleme stratejisi: biz hem değişiklikleri tutmak. En kısa süre önce yazılmış özgün dosya adı tutar. Eski dosya 'source' makine ve bu sınıflandırma adıyla eklenen çakışma numarası sahiptir: 
   
    `<FileNameWithoutExtension>-<MachineName>[-#].<ext>`  

    Örneğin, ilk ihtilaf `CompanyReport.docx` olacak `CompanyReport-CentralServer.docx` varsa `CentralServer` eski yazma oluştuğu değil. İkinci çakışma olarak tanımlanması `CompanyReport-CentralServer-1.docx`.

* <a id="afs-storage-redundancy"></a>**Coğrafi olarak yedekli depolama (GRS), Azure dosya eşitleme için destekleniyor mu?**  
    Evet, hem yerel olarak yedekli depolama (LRS) hem de coğrafi olarak yedekli depolama (GRS) Azure dosya eşitleme tarafından desteklenir. GRS arasında bir yük devretme durumunda eşleştirilmiş bölgeler, yeni bölge verileri yalnızca bir yedek olarak davranma öneririz. Azure dosya eşitleme, yeni birincil bölge ile eşitleniyor otomatik olarak başlatılmaz. 

* <a id="sizeondisk-versus-size"></a>**Neden değil *disk boyutuna* özelliği dosya eşleşmesi için *boyutu* Azure dosya eşitleme kullandıktan sonra özelliği?**  
    Windows dosya Gezgini iki özellikleri kullanıma sunan **boyutu** ve **disk boyutuna** dosyasının boyutunu temsil etmek için. Bu özellikleri anlamları dilden çok az farklılıklar; **Boyutu** dosyasının tam boyutunu temsil ederken **disk boyutuna** gerçekten diskte depolanan dosya akışı boyutunu temsil eder. Bunlar çeşitli nedenlerle sıkıştırma gibi farklı, yinelenen verileri kaldırmayı veya bu örnekte, Azure dosya eşitleme ile katmanlama bulut. Bir dosya katmanlı, Azure dosya paylaşımınıza dosya akışı depolanmadığından bir Azure dosya paylaşımı için disk boyutu sıfır olur disk üzerinde. Ayrıca bir dosyanın kısmen katmanlı (veya kısmen geri çekilen) olması için olası dosyasının bu bölümü yani diskte. Bu durum dosyaları kısmen multimedya oynatıcıları gibi uygulamaları veya sıkıştırma yardımcı programlar tarafından okunan ortaya çıkar. 

* <a id="is-my-file-tiered"></a>**Bir dosya katmanlı olmadığını nasıl anlayabilirim?**  
    Bir dosyayı Azure dosyanızı katmanlı olmadığını denetlemek için çeşitli yollar paylaşmak vardır:
    
    1. **Dosyanın dosya özniteliklerini denetleyin.** Bunu yapmak için bir dosyaya sağ tıklayın, gitmek **ayrıntıları** ve ekranı aşağı kaydırarak **öznitelikleri** özelliği. Katmanlı bir dosya kümesi aşağıdaki özniteliklere sahip:     
        
        | Öznitelik harf | Öznitelik | Tanım |
        |:----------------:|-----------|------------|
        | A | Arşiv | Yedekleme yazılımı tarafından dosya yedeklenmelidir gösterir. Bu öznitelik her zaman dosyanın veya katmanlı tam diskte depolanan bağımsız olarak ayarlanır. |
        | P | Seyrek dosya | Dosyanın dosya disk akışını çoğunlukla boş olduğunda, NTFS sunar dosyayı verimli kullanım için özel bir tür bir seyrek dosya olduğunu gösterir. Tam olarak katmanlı, kendi dosya akışı yani yalnızca bulutta depolanır veya kısmen geri çekilen, seçeneğidir; yani dosyasının bu bölümü zaten diskte olan bir dosya olduğu için azure dosya eşitleme seyrek dosyaları kullanır. Tam olarak dosyasıysa, diske geri, Azure dosya eşitleme, seyrek dosyasından normal bir dosyaya dönüştürür. |
        | L | Yeniden ayrıştırma noktası | Dosyanın dosya sistemi Filtresi tarafından kullanılmak üzere özel bir işaretçi bir yeniden ayrıştırma noktası olduğunu gösterir. Azure dosya eşitleme bulutta dosyasının depolandığı Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) tanımlamak için yeniden ayrıştırma noktalarını kullanır. Bu, Azure dosya eşitleme kullanılan bilmenize gerek bile, son kullanıcı ya da Azure dosya paylaşımınızı dosyasında erişmek nasıl olmadan sorunsuz erişim sağlar. Bir dosya tam olarak çağrılır, Azure dosya eşitleme yeniden ayrıştırma noktası dosyasından kaldırır. |
        | O | Çevrimdışı | Dosyanın içeriğini tümünün veya diske depolanmış olmayan gösterir. Bir dosya tam olarak çağrılır, Azure dosya eşitleme bu öznitelik kaldırır. |

        ![Ayrıntılar sekmesi seçili sahip bir dosya için Özellikler iletişim kutusu](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Bir klasördeki dosyaların tümünü özniteliklerini ekleyerek görmek mümkündür **öznitelikleri** dosya Exporer tablo görüntüsünü alanı. Bunu yapmak için (örneğin, boyut), varolan bir sütunla seçin sağ **daha fazla** seçip **öznitelikleri** aşağı açılan listeden.
        
    2. **Kullanım `fsutil` bir dosyayı yeniden ayrıştırma noktalarında denetlemek için.** Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) ayarlamak için önceki seçeneğinin belirtildiği gibi bir katmanlı dosya her zaman bir yeniden ayrıştırma noktası veya özel bir işaretçi sahip olacaktır. Bir dosya ile noktası yeniden ayrıştırma olup olmadığını kontrol edebilirsiniz `fsutil` yardımcı programı yükseltilmiş bir komut istemi veya PowerShell oturumu:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Dosya ayrıştırma noktası varsa, görmelisiniz **yeniden ayrıştırma etiket değeri: 0x8000001e**. Azure dosya eşitleme tarafından sahip olunan yeniden ayrıştırma noktası değer bu onaltılık değeridir. Çıkış dosyanızın Azure dosyanızdaki yolunu temsil eden veri paylaşımı yeniden ayrıştırma de içerir.

        > [!Warning]  
        > `fsutil reparsepoint` Yardımcı programı komutu yeniden ayrıştırma noktası silme olanağı da içerir. İçin Azure dosya eşitleme mühendislik ekibi tarafından belirtilmedikçe bu komutu yürütmek - bunun nedenle veri kaybına neden olabilir. 

* <a id="afs-recall-file"></a>**Kullanmak istediğiniz bir dosya... nasıl ı yerel olarak kullanılmak üzere disk için geri çağırma katmanlı?**  
    Bir dosyayı diske geri çekmek için en kolay yolu, açmaktır. Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) sorunsuz bir şekilde dosyayı Azure dosya paylaşımından işlerinizi yapmak gerek kalmadan indirir. Kısmen olabilir dosya türleri için tüm dosya indirme multimedya gibi okuma veya ZIP dosyaları, bir dosyayı açma oluşturmayacaktır.

    Aynı zamanda olası zorla çekilmesine için bir dosya olan PowerShell kullanarak. Örneğin, bu aynı anda (örneğin, klasördeki tüm dosyalar bir) birçok dosyaları geri çekmek istediğiniz durumlarda yararlı olabilir. Azure dosya eşitleme yüklü olduğu sunucu düğümü için bir PowerShell oturumu açın ve aşağıdaki PowerShell komutlarını çalıştırın:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>**Nasıl t bir dosya veya dizin katmanlı olması zorlayabilir miyim?**  
    Etkinleştirildiğinde, bulut katmanlama özelliğini otomatik olarak son erişimini kullanarak dosyaları katmanı ve bulut uç noktası üzerinde belirtilen birim boş alan yüzdesi elde etmek için saatleri değiştirme, ancak, bazen bir dosyayı el ile katmanı zorlamak istediğiniz. Örneğin, bu uzun süredir yeniden kullanmak ve diğer dosyalar/klasörler, birimdeki boş alan şimdi istiyorsanız düşünmüyorsanız büyük bir dosya kaydederseniz yararlı olabilir. Aşağıdaki PowerShell komutları ile katmanlama uygulayabilirsiniz:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-files-excluded"></a>**Otomatik olarak Azure dosya eşitleme tarafından hangi dosya veya klasörleri hariç tutulur?**
    Varsayılan olarak, Azure dosya eşitleme aşağıdaki dosyaları hariç tutar:
    
    - Desktop.ini
    - Thumbs.DB
    - ehthumbs.DB
    - ~$\*.\*
    - \*.laccdb
    - \*.tmp
    - 635D02A9D91C401B97884B82B3BCDAEA.\*

    Aşağıdaki klasörlerin de varsayılan olarak tutulur:

    - \System volume Information
    - \$GERİ DÖNÜŞÜM. DEPO
    - \SyncShareState

* <a id="afs-os-support"></a>**Linux, Windows Server 2008 R2 ile Azure dosya eşitleme kullanmak ister misiniz veya NAS cihazımı ile - Azure dosya eşitleme, destekliyor mu?**
    Bugün, Azure dosya eşitleme yalnızca Windows Server 2012 R2 ve Windows Server 2016 destekler. Şu anda biz biz paylaşabilirsiniz planların sunulmadı, ancak açık ve müşteri talebe göre ek platformları destekleyecek biçimde ilgilenen çalışıyoruz. Lütfen bize bildirin istediğinizi etmemizi desteklemek için hangi platformlar [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

## <a name="security-authentication-and-access-control"></a>Güvenlik, kimlik doğrulama ve erişim denetimi
* <a id="ad-support"></a>**Active Directory tabanlı kimlik doğrulaması ve Azure dosyaları tarafından desteklenen erişim denetimi nedir?**  
    Azure dosyaları iki yolla erişim denetimini yönetmek için şunları yapabilirsiniz:

    - SAS kullanarak, belirtilen zaman aralığı için geçerli olan özel izinlere sahip belirteçler oluşturabilirsiniz. Örneğin, 10 dakika süre sonu ile belirli bir dosya salt okunur erişimi olan bir belirteç oluşturabilirsiniz. Herkes geçerli olduğu halde bu belirtece sahip salt okunur dosya bu 10 dakika erişebilir. SAS anahtarları yalnızca REST API veya istemci kitaplıkları aracılığıyla bugün desteklenir, SMB üzerinden Azure dosya paylaşımı ile depolama hesabı anahtarlarını bağlamanız gerekir.

    - Azure dosya eşitleme korumak ve tüm sunucu uç noktaları için eşitlenen tüm ACL'ler (AD tabanlı veya yerel) çoğaltılır. Windows Server Active Directory ile zaten kimlik doğrulaması için ACL desteği ulaşan ve AD tabanlı kimlik doğrulaması için Azure dosya eşitleme harika bir Dur boşluk ölçü kadar tam destek sağlayabilirsiniz.

    Azure dosyaları, doğrudan şu anda Active Directory desteklemez.

* <a id="encryption-at-rest"></a>**Şifrelenmiş çalışmıyorken my Azure dosya paylaşımıdır nasıl sağlayabilirsiniz?**  
    Çalışmıyorken şifreleme varsayılan olarak tüm bölgelerde etkinleştiriliyor sürecinde ' dir. Bu bölgeler için şifrelemeyi etkinleştirmek için bir şey yapmanız gerekmez. Diğer bölgeler için lütfen bakın [sunucu tarafı şifrelemesi](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>**Bir web tarayıcısı kullanarak belirli bir dosyaya erişimi nasıl sağlayabilir miyim?**  
    SAS kullanarak, belirtilen zaman aralığı için geçerli olan özel izinlere sahip belirteçler oluşturabilirsiniz. Örneğin, belirli bir süre için belirli bir dosyaya salt okunur erişim sunan bir belirteç oluşturabilirsiniz. Geçerli olduğu halde bu url sahip olan herkes dosya tüm web tarayıcılarından doğrudan erişebilirsiniz. SAS anahtarları, Depolama Gezgini gibi bir kullanıcı arabiriminden kolayca oluşturulabilir.

* <a id="file-level-permissions"></a>**Salt okunur tanıyabilir veya paylaşımdaki klasörlere yalnızca yazma izinleri mı?**  
    Dosya paylaşımını SMB aracılığıyla bağlamanız halinde izinler üzerinde bu düzeyde bir denetiminiz bulunmaz. Ancak, REST API veya istemci kitaplıkları aracılığıyla paylaşılan erişim imzası (SAS) oluşturarak bunu gerçekleştirebilirsiniz.

* <a id="ip-restrictions"></a>**Bir Azure dosya paylaşımı için IP kısıtlamalarını uygulamak?**  
    Evet, bir depolama hesabı düzeyinde, Azure dosya paylaşımına erişim kısıtlanmış olabilir. Daha fazla bilgi için lütfen bkz bkz [Azure depolama güvenlik duvarlarını yapılandırın ve sanal ağlar](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>**Azure dosyaları için desteklenen veri uyumluluk ilkeleri nelerdir?**  
   Azure dosyaları aynı Azure Storage diğer depolama Hizmetleri'nde depolama mimarisine üzerinde çalışır ve aynı veri uyumluluk ilkelerini uygular. Azure Storage veri uyumluluğu hakkında daha fazla bilgi indirmek ve başvurulacak [Microsoft Azure veri koruması belge](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409) ve [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

## <a name="on-premises-access"></a>Şirket içi erişim
* <a id="expressroute-not-required"></a>**Veya şirket içi Azure dosya eşitleme kullanmak için Azure dosyalarına bağlanmak için Azure ExpressRoute kullanmak zorunda mıyım?**  
    Hayır, ExpressRoute bir Azure dosya paylaşımına erişmek için gerekli değildir. Tüm gerekli olan şirket içi, doğrudan bir Azure dosya paylaşımına bağlanması ise, bağlantı noktası 445 (TCP Giden) (Bu SMB iletişim kurar bağlantı noktasıdır) Internet erişimi için açık olduğunu. Azure dosya eşitleme kullanıyorsanız, tüm bağlantı noktası 443 (TCP Giden) için HTTPS erişim (SMB gerekli) gerekli. Ancak, ExpressRoute isterseniz ya da erişim seçeneği ile kullanılabilir.

* <a id="mount-locally"></a>**Nasıl ı my yerel makinede bir Azure dosya paylaşımı bağlayabilir?**  
    Bağlantı noktası 445 (TCP Giden) olduğu ve istemcinizi SMB 3.0 protokolünü destekleyen sürece SMB protokolü ile dosya paylaşımı bağlayabilir (örneğin, Windows 10 veya Windows Server 2016 kullanmakta olduğunuz). Bağlantı noktası 445, ISS veya kuruluşunuzun İlkesi tarafından engellenirse, Azure dosya paylaşımına erişmek için Azure dosya eşitleme kullanabilirsiniz.

## <a name="backup"></a>Backup
* <a id="backup-share"></a>**I my Azure dosyasını yedekleyin nasıl paylaşma?**  
    Kullanabileceğiniz düzenli [paylaşmak anlık görüntüler (Önizleme)](storage-how-to-use-files-snapshots.md) yanlışlıkla silmeleri karşı koruma için. AzCopy, RoboCopy, veya bir bağlı dosya paylaşımı yedekleyebilirsiniz yedekleme aracı bir 3. taraf kullanabilirsiniz. 

## <a name="share-snapshots"></a>Paylaşım anlık görüntüler
### <a name="share-snapshots---general"></a>Paylaşım anlık görüntüleri - genel
* <a id="what-are-snaphots"></a>**Dosya Paylaşımı anlık görüntüsü nedir?**  
    Azure dosya paylaşımı anlık görüntüler, dosya paylaşımları salt okunur bir sürümleri oluşturmanıza olanak sağlar. Ayrıca, aynı veya alternatif bir konuma Azure veya şirket içi başka değişiklik için içeriği geri daha eski bir sürümü kopyalamanıza olanak sağlar. Paylaşım anlık görüntü hakkında daha fazla bilgi için bkz [paylaşmak anlık görüntü genel bakış](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>**My paylaşımı anlık görüntüleri depolandığı?**  
    Paylaşım anlık görüntüleri aynı depolama hesabında dosya paylaşımına olarak depolanır.

* <a id="snapshot-perf-impact"></a>**Tüm performans etkileri var mı?**  
    Paylaşım anlık görüntüleri herhangi performansa sahip değilsiniz.

* <a id="snapshot-consistency"></a>**Paylaşım anlık görüntüleri uygulama tutarlı misiniz?**  
    Hayır. Paylaşım anlık görüntüleri uygulama olmayan tutarlı. Kullanıcı uygulamadan önce paylaşım anlık paylaşımına yazma işlemlerini boşaltmaya gerekecektir.

* <a id="snapshot-limits"></a>**Paylaşım anlık görüntü sayısı herhangi bir sınır var mıdır?**  
    Azure dosyaları koruyabilirsiniz 200 paylaşımı anlık görüntü sınırı yoktur. Bu yüzden paylaşımı anlık görüntüleri tüm paylaşım anlık görüntüleri tarafından kullanılan toplam alanı paylaşım başına no doğru paylaşım kotası sayılmaz. Depolama hesabı sınırları hala geçerlidir. 200 paylaşımı anlık görüntüleri sonra eski anlık görüntüleri yeni paylaşım anlık görüntüleri oluşturmak için silinmesi gerekir.

### <a name="create-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma
* <a id="file-snaphsots"></a>**Tek tek dosyaların paylaşımı anlık görüntü oluşturabilir miyim?**  
    Paylaşım anlık görüntüleri dosya paylaşımı düzeyinde oluşturulur. Tek tek dosyaların dosya paylaşımı anlık görüntüden geri yükleyebilirsiniz, ancak dosya düzeyinde paylaşımı anlık görüntüleri oluşturamıyor. Ancak, paylaşım düzeyi paylaşımı anlık görüntü aldıysanız ve belirli bir dosya değiştiği paylaşımı anlık görüntüleri listelemek istediğiniz eklerseniz, bir Windows takılı paylaşımında "Önceki sürümleri" deneyiminde bunu yapabilirsiniz. Lütfen bir dosya anlık görüntü özelliği gereksinimi varsa bilgilendirin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>**Şifrelenmiş dosya paylaşımı anlık görüntüleri oluşturabilir miyim?**  
    Şifreleme sahip bir Azure dosya paylaşımları paylaşımı anlık görüntüsünü etkin REST. Bir şifrelenmiş dosya paylaşımına dosya paylaşımı anlık görüntüden geri yükleyebilirsiniz. Paylaşımınıza şifrelenmişse, paylaşım anlık görüntünüz de şifrelenir.

* <a id="geo-redundant-snaphsots"></a>**My paylaşımı anlık görüntüleri coğrafi olarak yedekli misiniz?**
    Paylaşım anlık görüntü için bunlar gerçekleştirilen Azure dosya paylaşımı olarak aynı artıklık sahip olur. Hesabınız için coğrafi olarak yedekli depolama (GRS) seçtiyseniz paylaşımı anlık görüntünüz nedenle eşleştirilmiş bölgede depolanır.

### <a name="manage-share-snapshots"></a>Paylaşım anlık görüntülerini yönetme
* <a id="browse-snapshots-linux"></a>**Linux alınan my paylaşımı anlık göz atabilirsiniz?**  
    Azure CLI 2.0 oluşturma, listeleme, göz atın ve Linux üzerinde geri yüklemek için kullanabilirsiniz.

* <a id="copy-snapshots-to-other-storage-account"></a>**Farklı bir depolama hesabı için paylaşımı anlık görüntüleri kopyalayabilir miyim?**  
    Başka bir konuma ancak değil paylaşımı anlık görüntüleri kendilerini paylaşımı anlık görüntülerden dosyalarını kopyalayabilirsiniz.

### <a name="restore-data-from-share-snapshots"></a>Veri paylaşımı anlık görüntülerden geri yükleme
* <a id="promote-share-snapshot"></a>**Bir paylaşım anlık temel paylaşımına yükseltme?**  
    Yalnızca bir paylaşım anlık görüntüden veri herhangi istenen hedefe kopyalayabilirsiniz. Ancak bir paylaşım anlık temel paylaşımına yükseltemezsiniz.

* <a id="restore-snapshotted-file-to-other-share"></a>**I veri my paylaşımı anlık görüntüden farklı bir depolama hesabı için geri yükleyebilir miyim?**  
    Evet. Paylaşım anlık görüntü dosyaları özgün ya da aynı/farklı depolama hesabı aynı veya farklı bölgelerde içeren farklı bir konuma kopyalanır. Dosyaları, şirket içi veya başka bir bulut için de kopyalayabilirsiniz.    
  
### <a name="cleanup-share-snapshot"></a>Temizleme paylaşımı anlık görüntü
* <a id="delete-share-keep-snapshots"></a>**I my paylaşımını silmek ancak anlık görüntülerin paylaştığı değil?**
    Paylaşımı üzerinde etkin paylaşımı anlık görüntüler varsa paylaşımınıza silmek mümkün olmaz. Bir API paylaşımıyla birlikte paylaşımı anlık görüntüleri silmek için kullanılabilir ve aynı de Azure Portalı'ndan elde edebilirsiniz.

* <a id="delete-share-with-snapshots"></a>**Depolama Hesabımı silerseniz my paylaşımı anlık görüntüleri ne olur?**
    Depolama hesabınız silerseniz, paylaşım anlık görüntüleri de silinir.

## <a name="billing-and-pricing"></a>Faturalama ve fiyatlandırma
* <a id="vm-file-share-network-traffic"></a>**Bir Azure VM ile bir Azure dosya paylaşımı arasındaki ağ trafiği ücreti aboneliği yansıtılan harici bir bant genişliği olarak say mu?**  
    Dosya Paylaşımı ve VM aynı Azure bölgesinde varsa, aralarındaki trafik ücretsizdir. Bunlar farklı bölgelerde bulunuyorsa, aralarındaki trafik harici bant genişliği olarak ücretlendirilir.

* <a id="share-snapshot-price"></a>**Anlık görüntüler maliyeti ne kadar paylaşmak?**
     Önizleme paylaşımı sırasında anlık görüntü kapasite ücret olmaz. Standart depolama çıkış ve işlem maliyetleri eşiği uygulayın. Genel kullanılabilirlik sonrasında, kapasite ve Paylaşım anlık görüntü hareketleri ücretlendirilir.
     
     Doğası gereği artımlı paylaşımı anlık görüntüler. Paylaşım temel paylaşımı anlık görüntüsüdür. Tüm sonraki paylaşımı anlık görüntüleri artımlı ve yalnızca önceki paylaşım anlık görüntüden fark depolar. Yalnızca değiştirilen içerik için faturalandırılır. Veri 100 Gib'den Paylaş varsa ancak yalnızca 5 Gib'den, son paylaşımı anlık görüntü sonrası değişti paylaşımı anlık görüntü yalnızca 5 ek Gib'den kullanır ve Faturalanan yalnızca 105 olacaktır Gib'den. Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/) işlem ve standart çıkış ücretlerini hakkında daha fazla bilgi için.

## <a name="scale-and-performance"></a>Ölçek ve performans
* <a id="files-scale-limits"></a>**Azure dosyaları ölçek sınırları nelerdir?**  
    Azure dosyaları ölçeklenebilirlik ve performans hedefleri hakkında daha fazla bilgi için bkz: [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>**Azure dosyaları şu anda... my Azure dosya paylaşımı boyutunu artırmak sunar daha büyük bir dosya paylaşımı ihtiyacım var?**  
    Hayır, Azure dosya paylaşımı en büyük boyutu 5 Tıb yöneliktir. Şu anda biz ayarlayamaz bir sabit sınır budur. Biz 100 TIBS paylaşımı boyutunu artırmak için bir çözüm üzerinde çalışır, ancak şu anda paylaşmak için zaman çizelgesi yok.

* <a id="open-handles-quota"></a>**Kaç adet istemcinin aynı dosyayı aynı anda erişebilir mi?**  
    Tek bir dosya 2000 açık tanıtıcıların kota yoktur. 2000 açık tanıtıcıların oluşturduktan sonra bir hata, hatalarla kotasına ulaşıldı.

* <a id="zip-slow-performance"></a>**Performansta Azure dosyalarıyla dosyaların sıkıştırmasını çalışılırken yavaş. Ne yapmalıyım?**  
    Azure dosyalarıyla çok sayıda dosya aktarmak için ağ aktarımı için bu araçları için iyileştirilmiş gibi AzCopy (Windows, Linux/Unix için Önizleme) veya Azure Powershell kullanmanızı öneririz.

* <a id="slow-perf-windows-81-2012r2"></a>**I Windows Server 2012 R2 veya Windows 8.1 my Azure dosya paylaşımı bağlı ve performansta neden yavaş - nedir?**  
    Bir Azure dosya paylaşımı Windows Server 2012 R2 ve Windows 8.1, Windows 8.1 ve Windows Server 2012 R2 için Nisan 2014 toplu güncelleştirme oluşturulmuştur bağlarken bilinen bir sorun yoktur. Lütfen tüm örneklerini Windows Server 2012 R2 ve Windows 8.1 uygulanan en iyi performans için bu düzeltme eki olduğundan emin olun (doğal olarak, her zaman Windows Update aracılığıyla tüm Windows düzeltme eklerinin alma önerilir). Daha fazla bilgi için lütfen ilgili KB makalesine göz atın [Windows 8.1 veya Server 2012 R2'den Azure dosyaları eriştiğinizde yavaş performans](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Özellikleri ve diğer hizmetleri ile birlikte çalışabilirlik
* <a id="cluster-witness"></a>**My Azure dosya paylaşımı olarak kullanmak bir *dosya paylaşımı tanığı* my Windows Server Yük devretme kümesi için mi?**  
    Bu şu anda desteklenmeyen bir Azure dosya paylaşımı için. Bu Azure Blob Depolama için ayarlama hakkında daha fazla bilgi için bkz: [bir yük devretme kümesi için bir bulut tanığı dağıtmak](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>**Azure kapsayıcı örneğinde bir Azure dosya paylaşımı takabilir miyim?** ?  
    Evet, Azure dosya paylaşımları bir kapsayıcı örnek ömür ötesinde bilgilerinin sürdürülmesi için harika seçeneği değildir. Daha fazla bilgi için lütfen bkz [Azure kapsayıcı örnekleri ile Azure dosya paylaşımının takma](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>**REST API içinde bir yeniden adlandırma işlemi var mı?**  
    Şu anda değil.

* <a id="nested-shares"></a>**Paylaşımlar, diğer bir deyişle, bir paylaşım kapsamında içe?**  
    Hayır. Dosya paylaşımı bağlayabileceğiniz sanal bir sunucudur. Bu nedenle, iç içe paylaşımlar desteklenmez.

* <a id="ibm-mq"></a>**Azure dosyaları IBM MQ ile kullanma**  
    IBM, IBM MQ müşterileri Azure dosyaları ile kendi hizmet yapılandırırken yardımcı olacak bir belge yayımladı. Daha fazla bilgi için bkz. [Microsoft Azure Dosya Hizmeti ile IBM MQ Çok örnekli kuyruk yöneticisini kurma](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).

## <a name="see-also"></a>Ayrıca bkz.
* [Windows Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
* [Linux Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
* [Azure dosya eşitleme (Önizleme) sorunlarını giderme](storage-sync-files-troubleshoot.md)