---
title: "SAP HANA Azure yedekleme depolama anlık görüntü tabanlı | Microsoft Docs"
description: "İki ana yedekleme olasılıklarını SAP HANA Azure sanal makinelerde vardır, bu makalede, SAP HANA yedekleme depolama anlık görüntü tabanlı yer almaktadır"
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
ms.openlocfilehash: f332b8ac091b75a23489ac27f15ad1fd10d24ec6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Depolama anlık görüntülerine dayalı SAP HANA yedeklemesi

## <a name="introduction"></a>Giriş

Bu üç bölümlük SAP HANA yedeklemede ilgili makaleleri bir parçasıdır. [SAP HANA için yedekleme Kılavuzu Azure sanal makineler üzerinde](sap-hana-backup-guide.md) Başlarken, genel bir bakış ve bilgi sağlar ve [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneği ele alınmaktadır.

Tek Örnekli hepsi bir arada demo sistem için bir VM yedekleme özelliğini kullanarak, bir işletim sistemi düzeyinde HANA yedeklemeleri yönetmek yerine VM Yedekleme yapmayı düşünmelisiniz. Bir sanal makineye bağlı, tek tek sanal disk kopyaları oluşturmak için Azure blob anlık görüntülerini almak ve HANA veri dosyaları korumak için kullanılan bir alternatiftir. Ancak sistem yukarı durumdayken VM yedekleme veya disk anlık görüntü oluşturma ve çalıştırma olduğunda uygulama tutarlılığı kritik noktasıdır. Bkz: _depolama anlık görüntüleri duruma getirirken SAP HANA veri tutarlılığını_ ilgili makalede [yedekleme Kılavuzu SAP HANA Azure sanal makineler üzerinde](sap-hana-backup-guide.md). SAP HANA bu tür bir depolama anlık görüntüleri destekleyen bir özelliği vardır.

## <a name="sap-hana-snapshots"></a>SAP HANA anlık görüntüler

SAP HANA içinde depolama anlık destekleyen bir özellik yok. Ancak, aralık 2016 itibariyle tek kapsayıcı sistemleri için bir kısıtlama yoktur. Çok müşterili kapsayıcı yapılandırmaları, bu tür bir veritabanı anlık görüntüsü desteklemez (bkz [depolama anlık görüntü (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)).

Şu şekilde çalışır:

- SAP HANA anlık görüntü başlatarak depolama anlık görüntü için hazırlama
- Depolama anlık görüntü (anlık görüntü, örneğin Azure blob) çalıştırın
- SAP HANA anlık görüntü onaylayın

![Bu ekran görüntüsü, bir SAP HANA veri anlık bir SQL deyimi oluşturduğunuz gösterir.](media/sap-hana-backup-storage-snapshots/image011.png)

Bu ekran, SAP HANA veri anlık görüntüsünü bir SQL deyimi oluşturduğunuz gösterir.

![Anlık görüntü sonra da SAP HANA Studio yedekleme kataloğunda görüntülenir](media/sap-hana-backup-storage-snapshots/image012.png)

Anlık görüntü ardından da SAP HANA Studio yedekleme kataloğunda görüntülenir.

![Disk üzerinde anlık görüntü SAP HANA veri dizininde görüntülenir](media/sap-hana-backup-storage-snapshots/image013.png)

Disk üzerinde anlık görüntü SAP HANA veri dizininde görüntülenir.

SAP HANA anlık görüntü hazırlama modunda çalışırken depolama anlık görüntüyü çalıştırmadan önce dosya sistemi tutarlılığı da sağlanır emin olmak sahip. Bkz: _depolama anlık görüntüleri duruma getirirken SAP HANA veri tutarlılığını_ ilgili makalede [yedekleme Kılavuzu SAP HANA Azure sanal makineler üzerinde](sap-hana-backup-guide.md).

Depolama anlık görüntü yaptıktan sonra SAP HANA anlık görüntü onaylamak için önemlidir. Çalıştırmak için karşılık gelen bir SQL deyimi yoktur: yedekleme verilerini Kapat anlık görüntü (bkz [yedekleme verileri Kapat anlık görüntü deyimi (yedekleme ve kurtarma)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> HANA anlık görüntü onaylayın. Nedeniyle &quot;kopyalama yazarken,&quot; SAP HANA gerektirebilecek ek disk alanı hazırlama anlık görüntü modu ve SAP HANA anlık görüntü onaylandıktan kadar yeni yedeklemeleri başlatmak mümkün değildir.

## <a name="hana-vm-backup-via-azure-backup-service"></a>Azure Backup hizmeti aracılığıyla HANA VM yedekleme

Aralık 2016 itibariyle, Azure Backup hizmeti yedekleme aracısını Linux VM'ler için kullanılabilir değil. Yapmak için Azure dosya/dizin düzeyinde yedekleme kullanımı, bir Windows VM SAP HANA yedekleme dosyalarını kopyalayın ve yedekleme Aracısı'nı kullanmanız. Aksi takdirde, yalnızca tam bir Linux VM yedekleme Azure Backup hizmeti ile mümkündür. Bkz: [Azure Yedekleme'de özelliklerine genel bakış](../../../backup/backup-introduction-to-azure-backup.md) daha fazla bilgi bulmak için.

Azure Backup hizmeti yedekleme ve geri yükleme bir VM için bir seçenek sunar. Bu hizmet ve nasıl çalıştığı hakkında daha fazla bilgi makalesinde bulunabilir [azure'da VM yedekleme altyapınızı planlama](../../../backup/backup-azure-vms-introduction.md).

Bu makale göre iki önemli noktalar şunlardır:

_&quot;Linux VSS'yi için eşdeğer bir platform olmadığından Linux sanal makineleri için yalnızca dosya tutarlı yedeklemeler olasılığı vardır&quot;_

_&quot;Uygulamaların gerekiyor uygulamak kendi &quot;düzeltme&quot; geri yüklenen veriler üzerinde mekanizması.&quot;_

Bu nedenle, yedekleme başladığında SAP HANA disk tutarlı bir durumda olduğundan emin olmak sahip. Bkz: _SAP HANA anlık görüntüleri_ belgenin önceki bölümlerinde açıklanan. Ancak bu anlık görüntü hazırlama modunda SAP HANA kaldığında oluşan olası bir sorunu olduğunu. Bkz: [depolama anlık görüntü (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) daha fazla bilgi için.

Bu makale durumları:

_&quot;Onaylayın veya oluşturulduktan sonra depolama anlık görüntü mümkün olan en kısa sürede abandon önemle tavsiye edilir. Depolama anlık görüntü yüklenirken hazırlanmış veya oluşturulan, anlık görüntü ilgili verilerin dondurulmuş. Anlık görüntü ilgili verilerin dondurulmuş kalırken değişiklikleri hala veritabanında yapılabilir. Değiştirilecek dondurulmuş anlık görüntü ilgili verilerin değişikliğe neden olmaz. Bunun yerine, değişiklikler, depolama anlık görüntüden ayrı konumlara veri alanına yazılır. Değişiklikler de günlüğüne yazılır. Ancak, uzun anlık görüntü ilgili verilerin veri hacmi fazla büyüyebilir dondurulmuş, tutulur.&quot;_

Azure yedekleme Azure VM uzantıları aracılığıyla dosya sistemi tutarlılığı ilgilenir. Bu uzantılar, mevcut tek başına değildir ve yalnızca Azure Backup hizmeti ile birlikte çalışır. Bununla birlikte, yine bir SAP HANA anlık uygulama tutarlılığı garanti yönetmek için gerekli değildir.

Azure yedekleme iki ana aşamadan oluşur:

- Anlık görüntü alın
- Kasa için veri aktarımı girişi

Bir anlık görüntü alma Azure Backup hizmeti aşaması tamamlandıktan sonra bu nedenle bir SAP HANA anlık görüntü onaylayabilir. Azure portalında görmek için birkaç dakika sürebilir.

![Bu şekil, Azure Backup hizmeti yedekleme işi listesi parçası gösterir.](media/sap-hana-backup-storage-snapshots/image014.png)

Bu şekilde geri HANA VM test etmek için kullanılan bir Azure Backup hizmeti yedekleme işi listesi parçası gösterilmektedir.

![İş ayrıntılarını göstermek için Azure portalında yedekleme işi'ı tıklatın.](media/sap-hana-backup-storage-snapshots/image015.png)

İş ayrıntılarını göstermek için Azure portalında yedekleme işini tıklatın. Burada, bir iki aşamaya görebilirsiniz. Anlık görüntü aşaması tamamlandı olarak görüntüleyene kadar birkaç dakika sürebilir. Çoğu zaman, veri aktarımı aşamasında harcanır.

## <a name="hana-vm-backup-automation-via-azure-backup-service"></a>Azure Backup hizmeti aracılığıyla HANA VM yedekleme Otomasyonu

Biri el ile SAP HANA anlık görüntü bir kez daha önce açıklandığı şekilde Azure yedek anlık görüntü aşaması tamamlanır, ancak bir yönetici Azure portalında yedekleme işi listesi izlemeyecek çünkü Otomasyon düşünmek faydalıdır onaylayabilir.

Burada, nasıl, Azure PowerShell cmdlet'leri gerçekleştirilmesi bir açıklaması verilmiştir.

![Bir Azure yedekleme hizmeti adı hana-yedekleme-Kasayla birlikte oluşturuldu](media/sap-hana-backup-storage-snapshots/image016.png)

Azure Backup hizmeti adı ile oluşturulan &quot;hana yedekleme kasası.&quot; PS komutu **Get-AzureRmRecoveryServicesVault-ad hana yedekleme kasası** karşılık gelen nesne alır. Bu nesne, ardından yedekleme bağlam sonraki şekil görülen ayarlamak için kullanılır.

![Bir yedekleme işi şu anda devam ediyor için kontrol edebilirsiniz](media/sap-hana-backup-storage-snapshots/image017.png)

Doğru bağlamı ayarladıktan sonra bir yedekleme işi şu anda devam ediyor için denetleyin ve iş ayrıntılarını bakın. Alt liste Azure yedekleme işi anlık görüntü aşaması zaten tamamladığını gösterir:

```
$ars = Get-AzureRmRecoveryServicesVault -Name hana-backup-vault
Set-AzureRmRecoveryServicesVaultContext -Vault $ars
$jid = Get-AzureRmRecoveryServicesBackupJob -Status InProgress | select -ExpandProperty jobid
Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
```

![Tamamlandı olarak renge kadar döngü değerinde yoklama](media/sap-hana-backup-storage-snapshots/image018.png)

İş ayrıntılarını bir değişkende depolandıktan sonra yalnızca ilk dizi girişi almak ve durum değeri almak için PS sözdizimi şeklindedir. Otomasyon betiğini tamamlanması renge kadar döngü değeri yoklamak &quot;tamamlandı.&quot;

```
$st = Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
$st[0] | select -ExpandProperty status
```

## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>HANA lisans anahtarı ve VM Azure Backup hizmeti geri yükleme

Azure Backup hizmeti, geri yükleme sırasında yeni bir VM oluşturmak üzere tasarlanmıştır. Şu anda yapmak için hiçbir plan yoktur bir &quot;yerinde&quot; mevcut bir Azure VM'i geri yükleme.

![Bu şekilde, Azure portalında Azure hizmeti geri yükleme seçeneği gösterilmektedir](media/sap-hana-backup-storage-snapshots/image019.png)

Bu şekilde, Azure portalında Azure hizmeti geri yükleme seçeneği gösterilmektedir. Bir geri yükleme sırasında bir VM oluşturma veya diskleri geri yükleme arasında seçim yapabilirsiniz. Diskleri geri yükledikten sonra bunun üzerine yeni bir VM oluşturmak hala gerekli değildir. Azure üzerinde yeni bir VM benzersiz oluşturulan her VM kimliği değişiklikler (bkz [erişme ve Azure VM benzersiz kimliği kullanarak](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![Bu şekilde Azure VM benzersiz kimliği, önce ve sonra Azure Backup hizmeti aracılığıyla geri yükleme gösterilmektedir.](media/sap-hana-backup-storage-snapshots/image020.png)

Bu şekilde Azure VM benzersiz kimliği önce ve sonra Azure Backup hizmeti aracılığıyla geri yükleme gösterilmektedir. Bu benzersiz VM kimliği için lisans SAP kullanılır, SAP donanım anahtarı kullanma Sonuç olarak, yeni bir SAP lisans VM geri yüklendikten sonra yüklenmesi gerekir.

Yeni bir Azure yedekleme özelliği, bu yedekleme kılavuzu oluşturma sırasında önizleme modunda sunulmadı. VM için yedekleme alındığı VM anlık görüntüdeki tabanlı bir dosya düzeyinde geri yükleme sağlar. Bu yeni bir VM'yi dağıtmak için gereken engeller ve bu nedenle VM kimliği aynı kalır ve hiçbir yeni SAP HANA lisans anahtarı gereklidir. Tamamen test edildikten sonra bu özellik hakkında daha fazla belge sağlanacaktır.

Azure yedekleme sonunda tek tek Azure sanal diskler, yanı sıra dosya ve dizinleri VM içindeki yedekleme izin verir. Azure Backup önemli bir avantajı, müşteri yapmak zorunda kalmaktan kaydetme tüm yedeklemeler kendi yönetimidir. Bir geri yükleme gerekli hale gelirse, Azure Backup kullanmak için doğru yedekleme seçer.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>SAP HANA VM yedekleme el ile disk anlık görüntü aracılığıyla

Azure Backup hizmeti kullanmak yerine, bir tek tek bir yedekleme çözümü PowerShell aracılığıyla el ile Azure VHD blob anlık görüntüleri oluşturarak yapılandırabilirsiniz. Bkz: [PowerShell ile kullanma blob anlık görüntüleri](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) adımları açıklaması.

Daha fazla esneklik sağlar ancak bu belgenin önceki bölümlerinde açıklanan sorunları gidermek değil:

- Bir hala SAP HANA tutarlı bir durumda olduğundan emin olmanız gerekir
- Bir kira var olduğunu bildiren bir hata nedeniyle VM serbest bile, işletim sistemi diski üzerine yazılamaz. Yalnızca yeni bir benzersiz VM kimliği ve yeni bir SAP lisans yüklemeye gerek sunulmasını VM sildikten sonra çalışır.

![Yalnızca bir Azure VM veri disklerinin geri yüklenebilir](media/sap-hana-backup-storage-snapshots/image021.png)

Bunu yalnızca yeni bir benzersiz VM kimliği alma sorunu önlemenin bir Azure VM veri disklerinin geri yüklemek mümkündür ve bu nedenle, SAP lisans geçersiz:

- Test için iki Azure veri diski bir VM'ye bağlı ve yazılım RAID bunların üstüne tanımlandı 
- Bunu SAP HANA tutarlı bir durumda olduğunu SAP HANA anlık görüntü özelliği tarafından onaylanıp
- Dosya sistemi donmasına (bkz _depolama anlık görüntüleri duruma getirirken SAP HANA veri tutarlılığını_ ilgili makalede [yedekleme Kılavuzu SAP HANA Azure sanal makineler üzerinde](sap-hana-backup-guide.md))
- BLOB anlık görüntüleri hem veri disklerinden gerçekleştirilen
- Dosya Sistemi Çöz
- SAP HANA anlık görüntü onayı
- Veri diskleri geri yüklemek için VM kapatıldı ve her iki diskin ayrılmış
- Disk ayırma sonra eski blob anlık görüntüleri ile üzerlerine
- Daha sonra geri yüklenen sanal diskler için VM yeniden eklenmedi
- VM başlayarak sonra yazılımı üzerinde her şeyi RAID ince çalışılan ve geri blob anlık görüntü saati ayarlandı
- HANA HANA anlık görüntüye geri ayarlandı

SAP HANA blob anlık görüntüleri önce kapatılabilir ise yordamı daha az karmaşık olabilir. Bu durumda, bir HANA anlık görüntü atlayın ve, hiçbir şey sistemde olacağına dosya sistemi dondurma de atlayabilirsiniz. Her şeyi çevrimiçi durumdayken anlık görüntüleri yapmak gerekli olduğunda eklenen karmaşıklık devreye. Bkz: _depolama anlık görüntüleri duruma getirirken SAP HANA veri tutarlılığını_ ilgili makalede [yedekleme Kılavuzu SAP HANA Azure sanal makineler üzerinde](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Sonraki adımlar
* [SAP HANA için yedekleme Kılavuzu Azure sanal makineler üzerinde](sap-hana-backup-guide.md) genel bir bakış ve çalışmaya başlama hakkında bilgi sağlar.
* [SAP HANA yedekleme tabanlı dosya düzeyinde](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneği ele alınmaktadır.
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP HANA planlama azure'da (büyük örnekler) oluşturmayı öğrenmek için bkz: [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
