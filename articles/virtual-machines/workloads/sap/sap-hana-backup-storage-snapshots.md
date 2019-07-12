---
title: SAP HANA Azure yedekleme, depolama anlık görüntülerine dayalı | Microsoft Docs
description: İki ana yedekleme olasılık SAP HANA için Azure sanal makinelerinde vardır, bu makalede, depolama anlık görüntülerine dayalı SAP HANA yedeklemesi yer almaktadır
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: 875060a59cf70d295534c3a4f56136010a560e74
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709927"
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Depolama anlık görüntülerine dayalı SAP HANA yedeklemesi

## <a name="introduction"></a>Giriş

Bu, SAP HANA yedeklemesi ilgili makalelerde, üç bölümden oluşan bir parçasıdır. [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md) Başlarken üzerinde bir genel bakış ve bilgiler sağlar ve [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneği ele alınmaktadır.

Tek Örnekli hepsi bir arada tanıtım sistemi için bir VM yedekleme özelliğini kullanarak, bir işletim sistemi düzeyinde HANA yedeklemeleri yönetmek yerine bir VM Yedekleme yapmayı düşünmelisiniz. Bir sanal makineye bağlı olan, tek tek sanal disk kopyaları oluşturmak için Azure blob anlık görüntülerini alabilir ve HANA veri dosyaları korumak için kullanılan bir alternatiftir. Ancak, bir kritik sistem ayarlama modundayken bir VM yedekleme / disk anlık görüntüsü oluşturma ve çalıştırma olduğunda uygulama tutarlılığı noktasıdır. Bkz: _depolama anlık görüntülerini çekerken SAP HANA veri tutarlılığı_ ilgili makaledeki [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md). SAP HANA depolama anlık görüntüleri bu tür destekleyen bir özelliğe sahiptir.

## <a name="sap-hana-snapshots-as-central-part-of-application-consistent-backups"></a>Uygulama tutarlı yedekler merkezi bir parçası olarak SAP HANA anlık görüntüleri

Depolama anlık görüntüsünü destekleyen SAP HANA'da bir işlevi yoktur. Tek kapsayıcı sistemleri için bir kısıtlama yoktur. Senaryoları SING birden fazla Kiracı ile SAP HANA MCS, bu tür bir SAP HANA veritabanı anlık görüntüsü desteklemez (bkz [depolama anlık görüntü (SAP HANA Studio) oluşturma](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)).

Şu şekilde çalışır:

- SAP HANA anlık görüntü başlatarak için depolama anlık görüntüsü hazırlama
- Depolama anlık görüntüleri (Azure blob anlık görüntüsü, örneğin) çalıştırın
- SAP HANA anlık görüntü onaylayın

![Bu ekran görüntüsünde, bir SAP HANA veri anlık görüntü bir SQL deyimi oluşturulabilir gösterilmektedir.](media/sap-hana-backup-storage-snapshots/image011.png)

Bu ekran görüntüsünde, bir SAP HANA veri anlık görüntü bir SQL deyimi oluşturulabilir gösterilmektedir.

![Anlık görüntü daha sonra da SAP HANA Studio yedekleme kataloğunda görüntülenir](media/sap-hana-backup-storage-snapshots/image012.png)

Anlık görüntü ardından ayrıca SAP HANA Studio yedekleme kataloğunda görüntülenir.

![Disk üzerinde anlık görüntü SAP HANA veri dizinde için gösterilir.](media/sap-hana-backup-storage-snapshots/image013.png)

SAP HANA veri dizininde anlık görüntü diskte gösterilir.

SAP HANA anlık görüntü hazırlama modundayken, depolama anlık görüntüyü çalıştırmadan önce dosya sistemi tutarlılığı de sağlanır emin olmak varsa. Bkz: _depolama anlık görüntülerini çekerken SAP HANA veri tutarlılığı_ ilgili makaledeki [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md).

Depolama anlık görüntü tamamlandıktan sonra SAP HANA anlık görüntü onaylamak için önemlidir. Çalıştırılacak karşılık gelen bir SQL deyimi vardır: Yedekleme verileri Kapat anlık görüntü (bkz [yedekleme veri anlık görüntü tablosunu Kapat (yedekleme ve kurtarma)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> HANA anlık görüntü onaylayın. Şu nedenle &quot;kopyalama yazarken,&quot; SAP HANA gerektirebilecek ek disk alanı hazırlama anlık görüntü modu ve SAP HANA anlık görüntü onaylanana kadar yeni yedeklemeleri başlatmak mümkün değildir.

## <a name="hana-vm-backup-via-azure-backup-service"></a>Azure Backup hizmeti aracılığıyla HANA VM yedeklemesi

Linux Vm'leri için Yedekleme aracısı Azure Backup hizmeti kullanılamıyor. Windows, VSS ile içerdiğinden ayrıca Linux benzer işlevselliği yok  Yapmak için dosya/dizin düzeyinde Azure backup kullanan, SAP HANA için yedekleme dosyalarının bir Windows VM'ye kopyalama ve yedekleme Aracısı'nı kullanmanız. 

Aksi takdirde, yalnızca tam bir Linux VM yedekleme Azure Backup hizmeti ile mümkündür. Bkz: [Azure Backup özelliklerine genel bakış](../../../backup/backup-introduction-to-azure-backup.md) daha fazla bilgi edinmek için.

Azure Backup hizmeti, yedekleme ve geri yükleme bir VM için bir seçenek sunar. Bu hizmet ve nasıl çalıştığı hakkında daha fazla bilgi makalesinde bulunabilir [azure'da VM yedekleme altyapınızı planlama](../../../backup/backup-azure-vms-introduction.md).

Bu makalede göre iki önemli noktalar vardır:

_&quot;Linux VSS'yi eşdeğer bir platforma sahip olmadığından Linux sanal makineleri için yalnızca dosyayla tutarlı yedekleme mümkündür&quot;_

_&quot;Uygulamaların gerekiyor uygulamak kendi &quot;düzeltme&quot; geri yüklenen veriler üzerinde mekanizması.&quot;_

Bu nedenle, bir yedekleme başladığında, SAP HANA disk tutarlı bir durumda olduğundan emin olmak vardır. Bkz: _SAP HANA anlık görüntüleri_ belgenin önceki bölümlerinde açıklanan. Ancak, bu anlık görüntü hazırlama modunda SAP HANA kaldığında oluşan olası bir sorun olduğunu. Bkz: [depolama anlık görüntü (SAP HANA Studio) oluşturma](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) daha fazla bilgi için.

Bu makalede durumları:

_&quot;Onaylayın veya oluşturulduktan sonra depolama anlık görüntüleri olabildiğince çabuk abandon önemle tavsiye edilir. Depolama anlık görüntü yüklenirken hazır ya da oluşturulan, anlık görüntü ilgili verileri dondurulmuş. Anlık görüntü ilgili verileri dondurulmuş kaldığı sürece, veritabanında hala değişiklik yapılamaz. Bu tür değişiklikleri değiştirilecek dondurulmuş anlık görüntü ilgili verileri neden olmaz. Bunun yerine, değişiklikler, depolama anlık görüntüden farklı konumlara veri alanına yazılır. Değişiklikleri de günlüğüne yazılır. Ancak, uzun ilgili anlık görüntü verileri veri hacmi çok fazla büyüyebilir dondurulmuş, tutulur.&quot;_

Azure Backup, Azure VM uzantıları aracılığıyla dosya sistemi tutarlılığı üstlenir. Bu uzantılar, mevcut tek başına değildir ve yalnızca Azure Backup hizmeti ile birlikte çalışır. Bununla birlikte, yine de oluşturmak ve uygulama tutarlılığı güvence altına almak için bir SAP HANA anlık görüntüsünü silmek için betikler sağlamak için zorunludur.

Azure yedekleme, dört temel aşamadan oluşur:

- Yürütme komut dosyasını hazırlama - betik gereken bir SAP HANA anlık görüntüsünü oluşturmak
- Anlık görüntüsünü alın
- Anlık görüntü sonrası betik yürütme - betik hazırlama betiği tarafından oluşturulan SAP HANA'ni silmesi gerekir
- Verileri kasaya Aktar

Bu komut dosyalarını kopyalamak nereye hakkında ayrıntılı bilgi ve tam olarak Azure Backup birlikte nasıl çalıştığı hakkında bilgi için aşağıdaki makalelere bakın:

- [Azure’da VM yedekleme altyapınızı planlama](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)
- [Azure Linux sanal makinelerinin uygulamayla tutarlı yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)



Bu anda, Microsoft olmayan yayımladıktan komut dosyaları ve anlık görüntü sonrası betikler SAP HANA için hazırlayın. Müşteri ya da sistem entegratörü gibi bu betikleri oluşturmak ve yukarıda anılan belgeleri yordamı yapılandırmak gerekir.


## <a name="restore-from-application-consistent-backup-against-a-vm"></a>Bir VM'ye karşı uygulamayla tutarlı yedekleme geri yükleme
Azure backup tarafından alınan bir uygulamayla tutarlı yedekleme, geri yükleme işlemi makalesinde belgelenen [dosyaları Azure sanal makine yedekten kurtarma](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm). 

> [!IMPORTANT]
> Bu makalede [dosyaları Azure sanal makine yedekten kurtarma](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm) özel durumların bir listesi ve disk eşlikli kullanırken, listelenen adımları. Şeritli diskleri, normal bir VM yapılandırması için SAP HANA büyük olasılıkla içindir. Bu nedenle, makaleyi okuyun ve geri yükleme işlemi makalede listelenen gibi böyle durumlar için test etmek için gereklidir. 



## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>Azure Backup hizmeti ile HANA lisans anahtarı ve VM geri yükleme

Azure Backup hizmeti, geri yükleme sırasında yeni bir VM oluşturmak için tasarlanmıştır. Şu anda yapmak için hiçbir plan yoktur bir &quot;yerinde&quot; mevcut bir Azure VM'yi geri yükleme.

![Bu şekilde Azure hizmeti geri yükleme seçeneği Azure Portalı'nda gösterilmektedir.](media/sap-hana-backup-storage-snapshots/image019.png)

Bu şekilde, Azure portalında Azure hizmeti geri yükleme seçeneği gösterilmektedir. Bir geri yükleme sırasında bir VM oluşturma veya diskleri geri yükleme arasında seçim yapabilirsiniz. Diskleri geri yükledikten sonra bunun üstünde yeni bir VM oluşturmak yine de gereklidir. Yeni oluşturulan bir VM'yi Azure'da benzersiz her VM kimliği değiştirir (bkz [erişme ve Azure VM benzersiz kimliği kullanarak](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![Bu şekilde Azure VM benzersiz kimliği, önce ve sonra Azure Backup hizmeti aracılığıyla geri yükleme gösterilmektedir.](media/sap-hana-backup-storage-snapshots/image020.png)

Bu şekil, önce ve sonra Azure Backup hizmeti aracılığıyla geri yükleme Azure VM benzersiz Kimliğini gösterir. SAP lisansı için kullanılan SAP donanım anahtarı bu benzersiz sanal makine kimliği kullanıyor Sonuç olarak, yeni bir SAP lisansı bir sanal makine geri yüklendikten sonra yüklü olması gerekir.

Yeni bir Azure yedekleme özelliği önizleme modunda bu yedekleme kılavuzu oluşturma sırasında sunuldu. VM için yedekleme alındığı VM anlık görüntüsü üzerinde tabanlı bir dosya düzeyinde geri yükleme sağlar. Bu yeni bir VM dağıtma gereksinimini ortadan kaldırır ve bu nedenle benzersiz VM kimliği aynı kalır ve yeni SAP HANA lisans anahtarı gereklidir. Bu özellik hakkında daha fazla belge tamamen test edildikten sonra sağlanacaktır.

Azure yedekleme, sonunda tek tek Azure sanal diskler, yanı sıra dosya ve dizinleri VM içindeki yedekleme sağlar. Azure Backup'ın önemli bir avantajı, müşteri yapmanıza gerek kalmadan kaydetme tüm yedeklemeler, yönetimidir. Bir geri yükleme gerekli hale gelirse, Azure Backup kullanmak için doğru yedekleme seçer.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>SAP HANA VM yedeklemesi aracılığıyla el ile disk anlık görüntüsü

Azure Backup hizmeti kullanmak yerine, bir tek bir yedekleme çözümü Azure VHD'leri PowerShell aracılığıyla el ile blob anlık görüntüleri oluşturarak yapılandırabilirsiniz. Bkz: [PowerShell kullanarak blob anlık görüntüleri](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) adımları açıklaması.

Daha fazla esneklik sağlar, ancak bu belgenin önceki bölümlerinde açıklanan sorunları çözümlenmiyor:

- Hala bir SAP HANA anlık görüntü oluşturarak SAP HANA tutarlı bir durumda olduğundan emin olmanız gerekir
- Bir kira var olduğunu bildiren bir hata nedeniyle VM serbest bırakılmış olsa bile işletim sistemi diskini üzerine yazılamaz. Yalnızca yeni bir SAP lisansı yüklemek için gereken yeni ve benzersiz bir VM kimliği ile sonuçlanabilecek VM sildikten sonra çalışır.

![Yalnızca bir Azure VM'nin veri disklerini geri yüklemek mümkündür](media/sap-hana-backup-storage-snapshots/image021.png)

Bunu yalnızca yeni ve benzersiz bir VM kimliği alma sorunu önlemenin bir Azure VM'nin veri disklerini geri yüklemek mümkündür ve bu nedenle, SAP lisansı geçersiz:

- Test için iki Azure veri diski bir sanal Makineye bağlı ve yazılım RAID bunların üstüne tanımlandı 
- Bu anlık görüntü özelliği SAP HANA SAP HANA tutarlı bir durumda olduğunu Onaylanmadı
- Dosya sistemi dondurma (bkz _depolama anlık görüntülerini çekerken SAP HANA veri tutarlılığı_ ilgili makaledeki [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md))
- BLOB anlık görüntüleri hem veri disklerinden alınmıştır
- Dosya Sistemi Çöz
- SAP HANA anlık görüntü onayı
- Veri disklerini geri yüklemek için sanal makine kapatıldı ve her iki diskin kullanımdan çıkarıldı
- Disk ayırma sonra bunlar eski blob anlık görüntülerle üzerine yazıldı
- Daha sonra geri yüklenen sanal diskler VM yeniden eklenmedi
- VM'yi başlatma sonra yazılım üzerinde her şeyi RAID sorunsuz çalıştığı ve blob anlık görüntüsü durumuna geri ayarlandı
- HANA HANA anlık görüntüye geri ayarlandı

Blob anlık görüntüleri önce SAP HANA kapatılabilir, yordam daha az karmaşık olur. Bu durumda, bir HANA anlık görüntü atlayın ve, başka bir şey sistemde, olup biteni dosya sistemi dondurma da atlayın. Anlık görüntüleri olan her şeyi çevrimiçi durumdayken yapmak gerekli olduğunda eklenen karmaşıklık devreye. Bkz: _depolama anlık görüntülerini çekerken SAP HANA veri tutarlılığı_ ilgili makaledeki [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Sonraki adımlar
* [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md) genel bir bakış ve çalışmaya başlama hakkında bilgi sağlar.
* [SAP HANA yedeklemesi tabanlı dosya düzeyinde](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneği ele alınmaktadır.
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
