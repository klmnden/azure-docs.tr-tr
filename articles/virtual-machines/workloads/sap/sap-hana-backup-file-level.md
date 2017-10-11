---
title: "SAP HANA Azure Backup dosya düzeyinde | Microsoft Docs"
description: "İki ana yedekleme olasılıklarını SAP HANA Azure sanal makinelerde vardır, bu makalede, SAP HANA Azure yedekleme dosya düzeyinde yer almaktadır"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: 5db0ceb1648b5afa278e1cbe1c42fce8033bfdc1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="sap-hana-azure-backup-on-file-level"></a>Dosya düzeyinde bir SAP HANA Azure yedekleme

## <a name="introduction"></a>Giriş

Bu üç bölümlük SAP HANA yedeklemede ilgili makaleleri bir parçasıdır. [SAP HANA için yedekleme Kılavuzu Azure sanal makineler üzerinde](./sap-hana-backup-guide.md) Başlarken, genel bir bakış ve bilgi sağlar ve [tabanlı depolama anlık görüntü SAP HANA yedeklemeyi](./sap-hana-backup-storage-snapshots.md) depolama anlık görüntü tabanlı yedekleme seçeneği ele alınmaktadır.

Azure VM boyutlarda baktığınızda, biri bir GS5 64 eklenen veri disklerini izin verdiğini görebilirsiniz. Büyük SAP HANA sistemler için çok sayıda disk zaten veri ve günlük dosyaları için büyük olasılıkla en iyi disk g/ç işleme için RAID yazılım ile birlikte duruma getirilebilir. Soru daha sonra eklenen veri disklerini zamanla doldurabilir SAP HANA yedekleme dosyalarının depolanacağı mi? Bkz: [azure'daki Linux sanal makineler için Boyutlar](../../linux/sizes.md) Azure VM boyutu tablolar için.

Şu anda Azure Backup hizmeti ile kullanılabilecek hiçbir SAP HANA yedekleme tümleştirme yoktur. Dosya düzeyinde yedekleme/geri yükleme yönetmek için standart dosya tabanlı Yedekleme SAP HANA Studio aracılığıyla veya SAP HANA SQL deyimlerini aracılığıyla ile yoludur. Bkz: [SAP HANA SQL ve sistem görünümleri başvurusu](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) daha fazla bilgi için.

![Bu şekilde yedekleme menü öğesi iletişim SAP HANA Studio'da gösterilmektedir.](media/sap-hana-backup-file-level/image022.png)

Bu şekilde yedekleme menü öğesi iletişim SAP HANA Studio'da gösterilmektedir. Türü seçerken &quot;dosyası&quot; varsa dosya sistemindeki yolunu belirtmek yedekleme dosyalarının nerede SAP HANA yazar. Geri yükleme aynı şekilde çalışır.

Bu seçenek basit ve düz iletme sesleri olsa da, bazı noktalar vardır. Önceden belirtildiği gibi bir Azure VM eklenebilecek veri disklerinin sayısının bir kısıtlaması vardır. SAP HANA yazılım gerektirebilir veritabanı ve disk işleme gereksinimleri boyutuna bağlı olarak VM dosya sistemi yedek dosyalarını depolamak için kapasite olmayabilir arasında birden çok veri diskleri bölümlemesine kullanarak RAID. Bu yedekleme dosyaları ve yönetme dosya boyutu sınırlamaları ve performans verileri terabayt işlerken taşıma için çeşitli seçenekler bu makalenin sonraki bölümlerinde sağlanır.

Toplam Kapasite ilgili daha fazla özgürlük sunar, başka bir seçenek Azure blob depolama olur. Şu anda tek bir blob de 1 TB'ye kadar kısıtlı olsa da, bir tek blob kapsayıcı toplam kapasitesini 500 TB'tır. Buna ek olarak, müşteriler içermeyeceğini seçebilirsiniz verir &quot;cool&quot; blob depolama maliyeti avantajına sahiptir. Bkz: [Azure Blob Storage: sık erişimli ve seyrek erişimli depolama katmanları](../../../storage/blobs/storage-blob-storage-tiers.md) cool blob depolama hakkında ayrıntılı bilgi için.

Ek güvenlik için SAP HANA yedeklemelerini depolamak için bir coğrafi olarak çoğaltılmış depolama hesabı kullanın. Bkz: [Azure Storage çoğaltma](../../../storage/common/storage-redundancy.md) depolama hesabı çoğaltma hakkında ayrıntılı bilgi için.

Bir SAP HANA yedeklemeler için ayrılmış VHD'leri coğrafi olarak çoğaltılmış bir özel yedekleme depolama hesabında yerleştir. Aksi takdirde bir SAP HANA yedeklemeler coğrafi olarak çoğaltılmış depolama hesabına veya farklı bir bölgede olan bir depolama hesabını tutmak VHD'ler kopyalamak.

## <a name="azure-backup-agent"></a>Azure Yedekleme aracısı

Azure yedekleme yalnızca tam VM'ler, ancak ayrıca dosyaları ve dizinleri konuk işletim sistemi yüklü olmalıdır yedekleme aracısını aracılığıyla yedekleme seçeneği sunar. Ancak aralık 2016 itibariyle, bu aracı yalnızca Windows üzerinde desteklenir (bkz [bir Windows Server veya Azure Resource Manager dağıtım modelini kullanarak istemciye yedekleme](../../../backup/backup-configure-vault.md)).

Geçici bir çözüm, ilk SAP HANA yedekleme dosyalarını Azure üzerinde bir Windows VM (örneğin, SAMBA paylaşım) kopyalayın ve ardından Azure Yedekleme aracısı buradan kullanmaktır. Teknik olarak mümkün olsa da, karmaşıklık eklemek ve yedekleme yavaş veya bir bit Linux ve Windows VM arasında kopyalama nedeniyle geri yükleme işlemi. Bu yaklaşım izlemek için önerilmez.

## <a name="azure-blobxfer-utility-details"></a>Azure blobxfer yardımcı programı ayrıntıları

Azure depolama dosyaları ve dizinleri depolamak için bir CLI veya PowerShell kullanın veya birini kullanarak bir aracı geliştirmeyi [Azure SDK'ları](https://azure.microsoft.com/downloads/). Ayrıca bir kullanıma hazır yardımcı AzCopy, Azure depolama alanına veri kopyalamak için ancak Windows ise yalnızca (bkz [AzCopy komut satırı yardımcı programı ile veri aktarma](../../../storage/common/storage-use-azcopy.md)).

Bu nedenle blobxfer SAP HANA yedekleme dosyalarını kopyalamak için kullanıldı. Açık kaynak, üretim ortamlarında birçok müşteri tarafından kullanılan ve kullanılabilir olan [GitHub](https://github.com/Azure/blobxfer). Bu araç doğrudan Azure blob depolama ya da Azure dosya paylaşımı için veri kopyalamak için sağlar. Ayrıca, çeşitli md5 karma değeri veya bir dizin ile birden çok dosya kopyalarken otomatik paralellik gibi kullanışlı özellikler sunar.

## <a name="sap-hana-backup-performance"></a>SAP HANA yedekleme performansı

![SAP HANA Studio SAP HANA yedekleme konsolunda bu ekran değil](media/sap-hana-backup-file-level/image023.png)

SAP HANA Studio'da SAP HANA yedekleme konsolunun bu ekran görüntüsüdür. XFS dosya sistemi kullanılarak HANA VM'ye bağlı tek bir Azure standart depolama diskte 230 GB yedekleme yapmak için yaklaşık 42 dakika sürdü.

![Bu ekran YaST SAP HANA üzerinde VM sınamasıdır](media/sap-hana-backup-file-level/image024.png)

Bu ekran YaST SAP HANA test VM ' dir. Önceden belirtildiği gibi bir SAP HANA yedekleme için 1 TB'lik tek disk görebilirsiniz. Yedekleme 230 GB yaklaşık 42 dakika sürdü. Ayrıca, beş 200 GB disk eklenmedi ve bu beş Azure veri diskleri üstünde şeritleme ile yazılım RAID md0 oluşturulmuş.

![Yazılım üzerinde aynı yedekleme yinelenen RAID 5 arasında şeritleme ile Azure standart depolama veri diskleri bağlı](media/sap-hana-backup-file-level/image025.png)

Yazılım üzerinde aynı yedekleme yinelenen RAID 5 arasında şeritleme ile Azure standart depolama veri diskleri yedekleme süresi 10 dakika kadar 42 dakika duruma bağlı. Diskler VM önbelleğe alma olmadan eklenmedi. Dolayısıyla belirgin ne kadar önemli disk yazma üretimi yedekleme zamanı taşır. Bir sonra daha fazla en iyi performans için işlemi hızlandırmak için Azure premium Depolama'ya geçiş. Genel olarak, Azure premium storage üretim sistemleri için kullanılmalıdır.

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>SAP HANA yedekleme dosyalarını Azure blob depolama alanına kopyalayın

Aralık 2016 itibariyle hızla SAP HANA yedek dosyalarını depolamak için en iyi Azure blob depolama seçenektir. Tek tek blob kapsayıcısı yeterli GS5 VM ile yeterli SAP HANA yedekleri korumak için Azure üzerinde çalışan çoğu SAP HANA sistemler için 500 TB'lık bir sınırı vardır. Müşterilerin sahip arasında seçim &quot;etkin&quot; ve &quot;soğuk&quot; blob depolamada (bkz [Azure Blob Storage: sık erişimli ve seyrek erişimli depolama katmanları](../../../storage/blobs/storage-blob-storage-tiers.md)).

Blobxfer aracı ile SAP HANA yedekleme dosyalarını doğrudan Azure blob depolama alanına kopyalama kolaydır.

![Bir tam SAP HANA dosya yedekleme dosyaları burada görebilirsiniz](media/sap-hana-backup-file-level/image026.png)

Burada bir tam SAP HANA dosya yedekleme dosyaları görebilirsiniz. Dört dosyası vardır ve en büyük kaygılardan biri kabaca 230 GB sahiptir.

![Bir Azure standart depolama hesabı blob kapsayıcısına 230 GB kopyalamak için kabaca 3000 saniye sürdü](media/sap-hana-backup-file-level/image027.png)

MD5 karma değeri ilk testinde kullanmak değil, bir Azure standart depolama hesabı blob kapsayıcısına 230 GB kopyalamak için kabaca 3000 saniye sürdü.

![Bu ekran görüntüsünde, bir Azure Portal'da nasıl göründüğünü görebilirsiniz](media/sap-hana-backup-file-level/image028.png)

Bu ekran görüntüsünde, bir Azure Portal'da nasıl göründüğünü görebilirsiniz. Adlı bir blob kapsayıcı &quot;sap hana yedeklemeleri&quot; oluşturuldu ve SAP HANA yedekleme dosyalarını temsil eden dört BLOB içerir. Bunlardan birini kabaca 230 GB bir boyuta sahiptir.

HANA Studio yedekleme konsol HANA yedek dosyaları en fazla dosya boyutunu sınırlamak bir izin verir. Örnek ortamında, birden çok daha küçük yedek dosya yerine bir büyük 230 GB dosyasına sahip olacak şekilde sağlayarak, performans geliştirilmiş.

![T artırmak yedekleme saati HANA yan belirlemeden #39; üzerinde yedekleme dosya boyutu sınırını ayarlama](media/sap-hana-backup-file-level/image029.png)

HANA yan belirlemeden #39; üzerinde yedekleme dosya boyutu sınırını ayarlama t artırmak yedekleme saati dosyaları bu şekilde gösterildiği gibi sıralı olarak yazıldığından. Yedekleme 230 GB tek dosya yerine dört büyük veri dosyaları oluşmasını dosya boyutu sınırını 60 GB ayarlandı.

![Paralellik blobxfer aracının test etmek için HANA yedeklemeler için maksimum dosya boyutu 15 GB sonra ayarlandı](media/sap-hana-backup-file-level/image030.png)

Paralellik blobxfer aracının test etmek için HANA yedeklemeler için maksimum dosya boyutu sonra 19 yedekleme dosyalarında sonuçlandı 15 GB ayarlandı. Bu yapılandırma 230 GB 875 saniye kadar 3000 saniye Azure blob depolama alanına kopyalama blobxfer süredir duruma getirdi.

Bir Azure blob yazmak için 60 MB/sn sınırı nedeniyle bu sonucudur. Birden çok BLOB'ları aracılığıyla paralellik sorununu çözer, ancak bir dezavantajı yok: tüm bu HANA yedekleme dosyaları Azure blob depolama alanına kopyalamak için blobxfer aracı performansını artırma koyar yük HANA VM hem ağ. İşlem HANA sisteminin etkilenmiş haline gelir.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>Yedekleme yazılım RAID ayrılmış Azure veri diski BLOB kopyalama

El ile VM veri disk yedekleme, bu yaklaşım bir değil HANA veriler dahil tüm SAP yükleme kaydetmek için bir VM üzerindeki tüm veri diskleri yedekleme HANA dosyaları ve yapılandırma dosyaları günlüğe kaydetmez. Bunun yerine, bir fikir yazılım RAID şeritleme ile birden çok Azure veri VHD'ler arasında tam bir SAP HANA dosya yedekleme depolamak için ayrılmış olmaktır. Bir SAP HANA yedeğiniz yalnızca bu diskler, kopyalar. Bunlar kolayca bir adanmış HANA yedekleme depolama hesabında tutulan veya ayrılmış bir bağlı &quot;yönetim VM yedekleme&quot; başka bir işleme için.

![Tüm VHD'leri söz konusu kullanarak kopyalanan ** start-azurestorageblobcopy ** PowerShell komutu](media/sap-hana-backup-file-level/image031.png)

Yerel yazılım RAID Yedekleme tamamlandıktan sonra tüm VHD'leri söz konusu kullanarak kopyalanan **başlangıç azurestorageblobcopy** PowerShell komutunu (bkz [başlangıç AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy)). Yalnızca yedekleme dosyalarını tutmak için ayrılmış dosya sistemi etkilediği gibi SAP HANA veri veya günlük dosyası tutarlılık disk üzerinde hiçbir endişeniz vardır. Bu komutun VM çevrimiçi kalırken çalıştığını avantajdır. Hiçbir işlem yedekleme kümesi Yazar emin olmak için önce blob kopyalama çıkarın ve daha sonra yeniden bağlamak emin olun. Veya uygun bir şekilde kullanmayı &quot;Dondur&quot; dosya sistemi. Örneğin, xfs aracılığıyla\_XFS dosya sistemini Dondur.

![Bu ekran görüntüsü, Azure portalındaki VHD'ler kapsayıcısında BLOB listesi gösterir.](media/sap-hana-backup-file-level/image032.png)

Bu ekran BLOB'ları listesini gösterir &quot;VHD'ler&quot; Azure portalındaki kapsayıcı. Ekran görüntüsü, SAP HANA sunucunun SAP HANA yedek dosyaları saklamak için RAID yazılım olarak hizmet vermek için VM eklendiği beş VHD'leri gösterir. Ayrıca, aracılığıyla blob kopyalama komut gerçekleştirilen beş kopyaları gösterir.

![Test amacıyla, SAP HANA yedekleme yazılımı RAID disklerini kopyalarını VM uygulama sunucusunun eklenmedi](media/sap-hana-backup-file-level/image033.png)

Test amacıyla, SAP HANA yedekleme yazılımı RAID disklerini kopyalarını VM uygulama sunucusunun eklenmedi.

![VM diski eklemek için kapatıldı uygulama sunucusuna kopyalar.](media/sap-hana-backup-file-level/image034.png)

VM uygulama sunucusunun diski eklemek için kopyalar kapatıldı. VM başlattıktan sonra diskler ve RAID doğru bulunan (UUID bağlanan). Bağlama noktası eksik yalnızca YaST bölümleyici oluşturulduğu. Daha sonra SAP HANA yedek dosya kopyalarını işletim sistemi düzeyinde görünür hale geldi.

## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>SAP HANA yedekleme dosyalarını NFS paylaşımına kopyalayın

Bir performans veya disk alanı perspektif SAP HANA sistemde olası etkisini azaltmak için bir NFS paylaşımını SAP HANA yedekleme dosyalarını depolamak düşünebilirsiniz. Teknik olarak çalışır, ancak ikinci bir Azure VM bir NFS paylaşımına konağı olarak kullanılması anlamına gelir. VM ağ bant genişliği nedeniyle küçük bir VM boyutu olmamalıdır. Ardından bu kapatmaya anlamlı olacaktır &quot;yedekleme VM&quot; ve yalnızca SAP HANA yedekleme yürütme kaydınızı duruma getirin. Bir NFS yazma paylaşım ağda yükü getirir ve yalnızca yedekleme dosyalarının daha sonra açmayı yönetme ancak SAP HANA sistem etkiler &quot;yedekleme VM&quot; SAP HANA sistem hiç etkilemek değil.

![Başka bir Azure VM NFS paylaşımından SAP HANA server VM'ye takılı](media/sap-hana-backup-file-level/image035.png)

NFS kullanım doğrulamak için başka bir Azure VM NFS paylaşımından SAP HANA sunucunun VM takıldı. Hiçbir özel vardı uygulanan NFS ayarlama.

![1 saat ve 46 yedekleme doğrudan yapmak için dakika sürdü](media/sap-hana-backup-file-level/image036.png)

NFS paylaşım hızlı kümesi, SAP HANA sunucuda gibi oluştu. Bununla birlikte, 10 dakika yerine yerel bir Şerit yazma ayarlandığında NFS paylaşım doğrudan üzerinde yedekleme yapmak için 1 saat ve 46 dakika sürdü.

![Seçenek 1 saat 43 dakikada çok daha hızlı değildi](media/sap-hana-backup-file-level/image037.png)

Yerel kümesi için Yedekleme yapmayı ve işletim sistemi düzeyinde NFS paylaşımına kopyalanması alternatif (basit bir **cp - avr** komutu) çok daha hızlı değildi. 1 saat 43 dakika sürdü.

Bu nedenle çalışır ancak performans 230 GB yedekleme test için iyi değildi. Görüneceğini çoklu terabayt için daha büyük.

## <a name="copy-sap-hana-backup-files-to-azure-file-service"></a>Azure dosya hizmeti SAP HANA yedekleme dosyalarını kopyalayın

Bir Azure Linux VM içinde bir Azure dosya paylaşımının mümkündür. Makaleyi [Azure File storage'ı Linux ile kullanma konusunda](../../../storage/files/storage-how-to-use-files-linux.md) yapmak ayrıntılar verilmektedir. Olduğundan şu anda bir Azure dosya paylaşımı 5 TB kota sınırı ve 1 TB dosya başına bir dosya boyutu sınırını aklınızda bulundurun. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../../storage/common/storage-scalability-targets.md) depolama sınırları hakkında bilgi için.

Testler, ancak, SAP HANA yedekleme olmayan & #39 göstermiştir; t şu anda doğrudan CIFS bağlama bu tür ile çalışır. İçinde de belirtildiği [SAP Not 1820529](https://launchpad.support.sap.com/#/notes/1820529) , CIFS önerilmez.

![Bu şekil, SAP HANA Studio'da yedekleme iletişiminde bir hata gösterir.](media/sap-hana-backup-file-level/image038.png)

Bu şekilde bir hata SAP HANA Studio'da yedekleme iletişim kutusunda doğrudan bir CIFS monte Azure dosya paylaşımına yedeklemek çalışırken gösterilmektedir. Bu nedenle standart SAP HANA yedek bir VM dosya sistemine ilk yapabilir ve ardından Yedekleme dosyaları buradan Azure dosya hizmeti kopyalayın sahip.

![Bu şekilde işlem 19 SAP HANA yedekleme dosyalarını kopyalamak için yaklaşık 929 saniye sürdü gösterilmektedir](media/sap-hana-backup-file-level/image039.png)

Bu şekilde, Azure dosya paylaşımına kabaca 230 GB toplam boyutunu 19 SAP HANA yedek dosyaları kopyalamak için yaklaşık 929 saniye sürdü gösterilmektedir.

![SAP HANA VM kaynak dizin yapısını Azure dosya paylaşımına kopyalandı](media/sap-hana-backup-file-level/image040.png)

Bu ekran görüntüsünde, bir kaynak dizin yapısını SAP HANA VM Azure dosya paylaşımına kopyalanmıştır görebilirsiniz: bir dizin (hana\_yedekleme\_fsl\_15 gb) ve 19 tek tek yedekleme dosyaları.

Gelecekte SAP HANA dosyası yedeklerini doğrudan desteklendiğinde Azure dosyalarda SAP HANA yedekleme dosyalarını depolamak ilginç bir seçenek olabilir. Veya ne zaman bağlama Azure dosyaları NFS yoluyla olanaklı hale gelir ve en yüksek kota sınırına 5 TB'den önemli ölçüde daha yüksek.

## <a name="next-steps"></a>Sonraki adımlar
* [SAP HANA için yedekleme Kılavuzu Azure sanal makineler üzerinde](sap-hana-backup-guide.md) genel bir bakış ve çalışmaya başlama hakkında bilgi sağlar.
* [SAP HANA yedekleme depolama anlık görüntü tabanlı](sap-hana-backup-storage-snapshots.md) depolama anlık görüntü tabanlı yedekleme seçeneği açıklar.
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP HANA planlama azure'da (büyük örnekler) oluşturmayı öğrenmek için bkz: [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
