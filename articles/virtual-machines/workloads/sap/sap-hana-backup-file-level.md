---
title: SAP HANA dosya düzeyi Azure yedekleme | Microsoft Docs
description: İki ana yedekleme olasılık SAP HANA için Azure sanal makinelerinde vardır, bu makale SAP HANA Azure yedekleme dosya düzeyinde kapsar
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: fc35077e00bc6322a815a52ca6ab3571a4e06d3d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60937805"
---
# <a name="sap-hana-azure-backup-on-file-level"></a>SAP HANA dosya düzeyi Azure yedekleme

## <a name="introduction"></a>Giriş

Bu, SAP HANA yedeklemesi ilgili makalelerde, üç bölümden oluşan bir parçasıdır. [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](./sap-hana-backup-guide.md) Başlarken üzerinde bir genel bakış ve bilgiler sağlar ve [tabanlı depolama anlık görüntüleri üzerinde SAP HANA yedeklemeyi](./sap-hana-backup-storage-snapshots.md) depolama anlık görüntü tabanlı yedekleme seçeneği ele alınmaktadır.

Azure'da farklı VM türleri farklı bir ekli VHD'ler sayısını sağlar. Hakkında tam Ayrıntılar bölümünde belgelendirilen [azure'da Linux sanal makine boyutları](../../linux/sizes.md). Bu belgede başvurulan testler için GS5 Azure VM 64 bağlı veri diskleri sağlayan kullandık. Daha büyük SAP HANA sistemleri için çok sayıda disk zaten yazılım bölümleme en iyi disk g/ç aktarım türüyle birlikte, büyük olasılıkla veri ve günlük dosyalarının duruma getirilebilir. Azure vm'lerde SAP HANA dağıtımlar için önerilen disk yapılandırmaları hakkında daha fazla ayrıntı için makaleyi okuyun [Azure işlemler kılavuzu üzerinde SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations). Disk alanı önerileri de yerel yedeklemeler için yapılan öneriler dahil.

Şu anda Azure Backup hizmeti ile kullanılabilir hiçbir SAP HANA yedekleme tümleştirme yoktur. Dosya düzeyinde yedekleme/geri yükleme yönetmek için standart dosya tabanlı SAP HANA SQL deyimleri veya SAP HANA Studio aracılığıyla bir yedekleme ile yoludur. Bkz: [SAP HANA SQL'in ve sistem görünümleri başvuru](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) daha fazla bilgi için.

![Bu şekilde yedekleme menü öğesinin iletişim SAP HANA Studio gösterilmektedir.](media/sap-hana-backup-file-level/image022.png)

Bu şekilde, SAP HANA Studio yedekleme menü öğesinin iletişim kutusu gösterilmektedir. Türü seçerken &quot;dosyası&quot; varsa dosya sistemindeki bir yol belirtmek SAP HANA yedekleme dosyalarının nerede yazar. Geri yükleme, aynı şekilde çalışır.

Bu seçenek, basit ve anlaşılır görünse de, bazı noktalar vardır. Daha önce belirtildiği gibi Azure VM'deki eklenebilecek veri diski sayısı ile ilgili bir sınırlama vardır. VM, birden çok veri diskte şeritleme yazılım gerektirebilir veritabanı ve disk aktarım hızı gereksinimleri boyutuna bağlı olarak dosya sisteminde SAP HANA yedekleme dosyalarını depolamak için kapasite olmayabilir. Bu makalenin sonraki bölümlerinde bu yedek dosyaları ve yönetme dosya boyutu sınırlamaları ve performans verileri terabaytlarca işlerken taşımak için çeşitli seçenekleri sağlanır.

Toplam kapasite ile ilgili daha fazla özgürlük sunar, başka bir seçenek, Azure blob depolama alanıdır. Tek bir blob da 1 TB ile kısıtlı olsa da, toplam kapasite bir tek bir blob kapsayıcısı şu anda 500 TB'tır. Ayrıca, müşteriler sözde seçin seçeneği sunar &quot;seyrek erişimli&quot; blob depolama alanı maliyeti avantajına sahiptir. Bkz: [Azure Blob Depolama: Sık erişimli ve seyrek erişimli depolama katmanları](../../../storage/blobs/storage-blob-storage-tiers.md) seyrek erişimli blob depolama hakkında ayrıntılı bilgi için.

Ek güvenlik için SAP HANA yedeklemeleri depolamak için bir coğrafi çoğaltmalı depolama hesabı kullanın. Bkz: [Azure depolama çoğaltma](../../../storage/common/storage-redundancy.md) depolama hesabı çoğaltma hakkındaki ayrıntılar için.

Coğrafi olarak çoğaltılmış bir ayrılmış bir yedekleme depolama hesabında bir SAP HANA yedeklemeler için adanmış VHD'ler yerleştirebilirsiniz. Aksi takdirde bir SAP HANA yedeklemelerin coğrafi olarak çoğaltılmış bir depolama hesabı veya farklı bir bölgede bir depolama hesabı için tutmak VHD'ler konumuna yüklenemedi.

## <a name="azure-backup-agent"></a>Azure Yedekleme aracısı

Azure backup yalnızca tam VM'ler, ancak ayrıca dosyaları ve dizinleri konuk işletim sistemi yüklü olması gereken yedekleme aracısını aracılığıyla yedekleme seçeneği sunar. Ancak bu aracı yalnızca Windows üzerinde desteklenir (bkz [bir Windows sunucusundaki veya istemcisindeki Resource Manager dağıtım modelini kullanarak azure'a yedekleme](../../../backup/backup-configure-vault.md)).

Öncelikle SAP HANA için yedekleme dosyalarının azure'da Windows VM (örneğin, SAMBA paylaşım) kopyalamanız ve ardından Azure Yedekleme aracısı buradan bir çözüm olabilir. Teknik olarak mümkün olsa da, karmaşıklık ekleyin ve yedeklemenin yavaşlamasına veya işlemi, Linux ve Windows sanal makine arasında kopyalama nedeniyle bir bit geri. Bu yaklaşım izlemek için önerilmez.

## <a name="azure-blobxfer-utility-details"></a>Azure blobxfer yardımcı programı ayrıntıları

Dizinleri ve dosyaları Azure depolamada depolamak için bir CLI veya PowerShell kullanın veya bir aracı kullanarak geliştirmeyi [Azure SDK'ları](https://azure.microsoft.com/downloads/). Ayrıca kullanıma hazır yardımcı programı, AzCopy, Azure depolama alanına veri kopyalamak için olmakla birlikte, yalnızca Windows olur (bkz [AzCopy komut satırı yardımcı programı ile veri aktarma](../../../storage/common/storage-use-azcopy.md)).

Bu nedenle, blobxfer SAP HANA yedekleme dosyalarını kopyalamak için kullanıldı. Açık kaynaklı, üretim ortamlarında birçok müşteri tarafından kullanılan ve kullanılabilir olduğu [GitHub](https://github.com/Azure/blobxfer). Bu aracı, verileri doğrudan Azure blob depolama veya Azure dosya paylaşımı kopyalamak için sağlar. Ayrıca, çeşitli md5 karma değeri veya bir dizin ile birden çok dosya kopyalarken otomatik paralellik gibi kullanışlı özellikler sunar.

## <a name="sap-hana-backup-performance"></a>SAP HANA yedekleme performansı

![SAP HANA Studio SAP HANA yedekleme konsolunda bu ekran değil](media/sap-hana-backup-file-level/image023.png)

Bu ekran, SAP HANA Studio SAP HANA yedekleme konsolunun şeklindedir. HANA XFS dosya sistemini kullanarak VM'ye bir tek bir Azure standart depolama diskte 230 GB yedekleme yapmak için yaklaşık 42 dakika sürdü.

![Bu ekran YaST SAP HANA test sanal makinesi olduğundan.](media/sap-hana-backup-file-level/image024.png)

Bu ekran, SAP HANA test sanal makinesi YaST şeklindedir. Daha önce belirtildiği gibi bir SAP HANA yedeklemesi için 1 TB tek disk görebilirsiniz. Yedekleme 230 GB-yaklaşık 42 dakika sürdüğünü. Ayrıca, beş 200 GB disk eklenmiş ve bölümleme türüyle bu beş Azure veri diskleri üzerinde ile yazılım RAID md0 oluşturdunuz.

![Yinelenen aynı yedekleme yazılımı ile beş arasında şeritleme RAID bağlı veri diskleri Azure standart depolama](media/sap-hana-backup-file-level/image025.png)

Aynı yedekleme yazılımı yinelenen bağlı 10 dakika kadar 42 dakika yedekleme zamanını duruma standart Azure depolama veri diskleri ile beş arasında şeritleme RAID. Diskler VM önbelleğe almadan eklenmedi. Ne kadar önemli belirgin disk yazma üretimi için yedekleme zamanı olduğundan, bu nedenle. Bir sonra daha iyi performans için sürecini hızlandırmak için Azure Premium Depolama'ya geçiş. Genel olarak, Azure Premium depolama, üretim sistemleri için kullanılmalıdır.

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>SAP HANA yedekleme dosyalarını Azure blob depolamaya kopyalama

SAP HANA için yedekleme dosyalarının hızlı bir şekilde depolamak için başka bir Azure blob depolama seçeneğidir. Tek tek bir blob kapsayıcısı 500 yeterli SAP HANA yedeklemeleri korumak için Azure, M32ts, M32ls M64ls ve GS5 VM türleri kullanarak TB, bazı küçük bir SAP HANA sistemleri için yeterli bir sınırı vardır. İmajlarını arasında seçim &quot;sık erişimli&quot; ve &quot;soğuk&quot; blob depolama (bkz [Azure Blob Depolama: Sık erişimli ve seyrek erişimli depolama katmanları](../../../storage/blobs/storage-blob-storage-tiers.md)).

Blobxfer aracı SAP HANA yedekleme dosyalarını doğrudan Azure blob depolama alanına kopyalamak kolaydır.

![Bir tam bir SAP HANA dosya yedekleme dosyaları burada görebilirsiniz](media/sap-hana-backup-file-level/image026.png)

Burada bir tam bir SAP HANA dosya yedekleme dosyalarını görebilirsiniz. Kabaca 230 GB en büyük zorluklardan biri olan ve dört dosyası vardır.

![Bir Azure standart depolama hesabı blob kapsayıcısına 230 GB kopyalamak için yaklaşık 3000 saniye sürdü](media/sap-hana-backup-file-level/image027.png)

MD5 karma değeri ilk testinde kullanmak değil, bir Azure standart depolama hesabı blob kapsayıcısına 230 GB kopyalamak için yaklaşık 3000 saniye sürdü.

![Bu ekran görüntüsünde, bir Azure portalında nasıl göründüğünü görebilirsiniz](media/sap-hana-backup-file-level/image028.png)

Bu ekran görüntüsünde, bir Azure portalında nasıl göründüğünü görebilirsiniz. Adlı bir blob kapsayıcısı &quot;sap hana yedeklemeleri&quot; oluşturuldu ve SAP HANA için yedekleme dosyalarının temsil eden dört blobları içerir. Bunlardan biri, bir boyut kabaca 230 GB sahiptir.

HANA Studio yedekleme konsolu bir HANA için yedekleme dosyalarının en büyük dosya boyutunu sınırlamak sağlar. Örnek ortamda bu büyük 230 GB dosya yerine birden çok daha küçük yedekleme dosyaları olası hale getirerek performansı geliştirildi.

![HANA yan eklenmemişse'üzerinde yedek dosya boyutu sınırını ayarlama&#39;t yedekleme zamanını geliştirin](media/sap-hana-backup-file-level/image029.png)

HANA yan eklenmemişse'üzerinde yedek dosya boyutu sınırını ayarlama&#39;t yedekleme zamanını dosyaları bu şekilde gösterildiği gibi sıralı olarak yazıldığı geliştirmek. Yedekleme 230 GB tek dosya yerine dört büyük veri dosyalarını oluşmasını 60 GB dosya boyutu sınırını ayarlandı. Birden çok yedekleme dosyalarını kullanarak M64s, M64ms M128s ve M128ms gibi daha büyük Azure sanal makinelerin bellek yararlanan HANA veritabanlarını yedeklemek için bir zorunluluktur.

![Paralellik blobxfer aracı test etmek için HANA yedeklemeler için maksimum dosya boyutu için 15 GB sonra ayarlandı](media/sap-hana-backup-file-level/image030.png)

Paralellik blobxfer aracı test etmek için HANA yedeklemeler için maksimum dosya boyutu ardından 19 yedekleme dosyalarında sonuçlandı 15 GB ayarlanmıştır. Bu yapılandırma, süre blobxfer 875 saniye kadar 3000 saniye veritabanından Azure blob depolama alanına 230 GB kopyalamak için duruma getirdi.

Bir Azure blob yazmak için 60 MB/sn sınırı nedeniyle bu sonucudur. Birden çok BLOB'ları aracılığıyla paralellik performans sorunu çözer, ancak bir dezavantajı yoktur: tüm bu HANA yedekleme dosyalarını Azure blob depolama alanına kopyalamak blobxfer aracı performansı artırma koyar yük HANA VM hem ağ. HANA sistemi işleyişini etkilenmiş olur.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>Özel bir Azure veri diskleri yedekleme yazılım RAID BLOB kopyası

El ile VM veri diski yedekleme, bu yaklaşım bir değil tüm SAP HANA verileri dahil olmak üzere yükleme, kaydetmek için bir VM üzerindeki tüm veri disklerini yedekleme HANA dosyaları ve yapılandırma dosyaları günlüğe kaydetmez. Bunun yerine, fikir yazılım RAID ile bölümleme türüyle birden fazla Azure veri VHD'leri arasında tam bir SAP HANA dosya yedekleme depolamak için ayrılmış olmaktır. Bir SAP HANA yedeğiniz yalnızca bu diskleri kopyalar. Bunlar kolayca adanmış bir HANA yedekleme depolama hesabı içinde tutulan veya ayrılmış bir bağlı &quot;yönetim VM yedekleme&quot; daha fazla işleme için.

![Dahil olan tüm VHD'lerin kullanarak kopyalanan ** Başlat-azurestorageblobcopy ** PowerShell komutu](media/sap-hana-backup-file-level/image031.png)

Yerel yazılım RAID Yedekleme tamamlandıktan sonra ilgili tüm VHD'leri kullanma kopyalanan **başlangıç azurestorageblobcopy** PowerShell komutunu (bkz [başlangıç AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy)). Yalnızca yedekleme dosyalarını tutmak için ayrılmış dosya sistemi etkiler gibi hiçbir disk üzerinde SAP HANA veri veya günlük dosyası tutarlılık DB'dir vardır. Bu komutun bir avantajı, sanal makine çevrimiçi kalırken çalıştığını içindir. Hiçbir işlem yedekleme kümesi Yazar emin olmak için önce blob kopyası çıkarın ve daha sonra tekrar bağlayın emin olun. Veya bir uygun bir şekilde kullanabilirler &quot;dondurma&quot; dosya sistemi. Örneğin, xfs aracılığıyla\_XFS dosya sistemini dondurma.

![Bu ekran, Azure portalında VHD'ler kapsayıcıdaki BLOB listesini gösterir.](media/sap-hana-backup-file-level/image032.png)

Bu ekran BLOB listesini gösteren &quot;VHD'ler&quot; Azure portalında kapsayıcı. SAP HANA sunucusunun yazılım SAP HANA için yedekleme dosyalarının tutmak için RAID görev yapacak bir VM eklendiği beş VHD'lerin ekran görüntüsünde gösterilmektedir. Ayrıca, blob kopyalama komutuyla alınan beş kopyalar gösterir.

![Test amacıyla, SAP HANA yedekleme yazılımı RAID disk kopyaları uygulama sunucusu VM'sine eklenmedi](media/sap-hana-backup-file-level/image033.png)

SAP HANA yedekleme yazılımı RAID disk kopyaları, test amacıyla, uygulama sunucusu VM'sine eklenmedi.

![VM diski eklemek için kapatıldı uygulama sunucusuna kopyalar.](media/sap-hana-backup-file-level/image034.png)

Uygulama sunucusu VM diski kopyalar kapatıldı. VM başlatıldıktan sonra disk ve RAID doğru bulunan (UUID bağlı). Bağlama noktası eksik yalnızca YaST bölümleyici oluşturulduğu. Daha sonra SAP HANA yedekleme dosya kopyalarını işletim sistemi düzeyinde görünür hale geldi.

## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>SAP HANA için yedekleme dosyalarının NFS paylaşımına kopyalayın.

SAP HANA sistem performans veya disk alanı açısından olası etkisini azaltmak için bir NFS paylaşımına SAP HANA yedekleme dosyalarını depolamak düşünebilirsiniz. Teknik olarak çalışır, ancak ikinci bir Azure sanal makine kullanarak NFS paylaşım ana bilgisayarı olarak anlamına gelir. VM ağ bant genişliği nedeniyle küçük bir VM boyutu olmamalıdır. Ardından bu kapatmak için anlamlı olacaktır &quot;VM yedekleme&quot; ve SAP HANA yedeklemesi yürütme için yalnızca getirin. Yazma bir NFS paylaşım ağda yük koyar ve yalnızca yedekleme dosyaları daha sonra açmayı yönetme ancak SAP HANA sistem etkiler &quot;VM yedekleme&quot; SAP HANA sistem hiç etkilemek değil.

![SAP HANA sunucusu VM'sine bir NFS paylaşımına başka bir Azure VM'den takıldı](media/sap-hana-backup-file-level/image035.png)

NFS kullanım örneği doğrulamak için bir NFS paylaşımına başka bir Azure VM'den SAP HANA sunucusu VM'sine takıldı. Hiçbir özel oluştu uygulanan NFS ayarlama.

![1 saat ve 46 doğrudan yedekleme yapmak için dakika sürdü](media/sap-hana-backup-file-level/image036.png)

NFS paylaşım hızlı kümesi, SAP HANA sunucusunun birinde gibi oluştu. Bununla birlikte, 10 dakika yerine yerel bir Şerit yazma ayarlandığında NFS paylaşım doğrudan üzerinde yedekleme yapmak için 1 saat 46 dakika sürdü.

![Seçenek 1 saat 43 dakikalarda çok daha hızlı değildi](media/sap-hana-backup-file-level/image037.png)

Yerel bir kümesi için bir yedekleme yapmayı ve işletim sistemi düzeyinde NFS paylaşımına kopyalama alternatif (basit **cp - avr** komut) daha hızlı değildi. Bu, 1 saat ve 43 dakika sürdü.

Bu nedenle çalışır, ancak performans 230 GB yedekleme test için iyi olmadığını. Görüneceğini çoklu terabayt için daha büyük.

## <a name="copy-sap-hana-backup-files-to-azure-files"></a>Azure dosyaları için SAP HANA yedekleme dosyalarını kopyalama

Bir Azure Linux VM içinde bir Azure dosya paylaşımını bağlayabilmeniz mümkündür. Makaleyi [Azure dosya depolamayı Linux ile kullanma konusunda](../../../storage/files/storage-how-to-use-files-linux.md) nasıl yapılacağı hakkında ayrıntılar sağlar. Olduğunu şu anda bir Azure dosya paylaşımını 5 TB kota sınırı ve dosya başına 1 TB'lık dosya boyutu sınırı göz önünde bulundurun. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../../storage/common/storage-scalability-targets.md) depolama sınırları hakkında bilgi için.

Testleri göstermiştir, Bununla birlikte, SAP HANA yedekleme engellemeyen&#39;t şu anda doğrudan CIFS bağlama bu tür ile çalışma. İçinde de belirtildiği [SAP notu 1820529](https://launchpad.support.sap.com/#/notes/1820529) , CIFS önerilmez.

![Bu şekil, SAP HANA Studio yedekleme iletişim bir hata gösterir.](media/sap-hana-backup-file-level/image038.png)

Bu şekilde bir hata SAP HANA Studio yedekleme iletişim doğrudan bir CIFS bağlanan Azure dosya paylaşımınızı yedeklemek çalışırken gösterilmektedir. Standart bir SAP HANA yedeklemesi öncelikle bir VM dosya sistemine yapmak ve sonra yedekleme dosyaları Azure dosya Hizmeti'ne buradan kopyalayın. Bu nedenle bir tane vardır.

![Bu şekilde, 19 SAP HANA yedekleme dosyalarını kopyalamak için yaklaşık 929 saniye sürdü gösterilmektedir.](media/sap-hana-backup-file-level/image039.png)

Bu şekilde, Azure dosya paylaşımı için toplam boyut kabaca 230 GB ile 19 SAP HANA yedekleme dosyalarını kopyalamak için yaklaşık 929 saniye sürdü gösterilmektedir.

![SAP HANA VM kaynak dizin yapısına Azure dosya paylaşımına kopyalanmıştır.](media/sap-hana-backup-file-level/image040.png)

Bu ekran görüntüsünde, bir kaynak dizin yapısına SAP HANA VM Azure dosya paylaşımına kopyalandığını görebilirsiniz: bir dizin (hana\_yedekleme\_fsl\_15 gb) ve 19 tek tek yedekleme dosyaları.

Azure dosyalarını yedekleme dosyaları SAP HANA depolama gelecekte SAP HANA dosyası yedeklerini doğrudan desteklendiğinde ilginç bir seçenek olabilir. Veya ne zaman aracılığıyla NFS bağlama Azure dosyaları mümkün hale gelir ve en yüksek kota sınırına 5 TB'den oldukça yüksektir.

## <a name="next-steps"></a>Sonraki adımlar
* [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md) genel bir bakış ve çalışmaya başlama hakkında bilgi sağlar.
* [SAP HANA yedeklemesi depolama anlık görüntülerine dayalı](sap-hana-backup-storage-snapshots.md) depolama anlık görüntü tabanlı yedekleme seçeneği açıklar.
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
