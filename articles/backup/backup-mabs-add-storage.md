---
title: Modern yedekleme depolama alanı ile Azure Backup sunucusu kullanma
description: Azure Backup sunucusu yeni özellikler hakkında bilgi edinin. Bu makalede, Backup sunucusu yüklemesi yükseltileceği açıklanır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adigan
ms.openlocfilehash: 621d071f98701ff3a949f4172fef1d13819d7192
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60813119"
---
# <a name="add-storage-to-azure-backup-server"></a>Azure Backup Sunucusu’na depolama alanı ekleme

Azure Backup sunucusu V2 ve sonraki destekler, yüzde 50, yedekleme depolama tasarrufu sağlayan Modern yedekleme depolama üç kat daha hızlı ve daha verimli depolama hizmetidir. Ayrıca, iş yükü algılayan depolamayı da sunar.

> [!NOTE]
> Modern yedekleme depolama alanı kullanmak için Windows Server 2016 veya Windows Server 2019 tarihinde V3 yedekleme sunucusu V2 veya V3 çalıştırmalısınız.
> Windows Server'ın önceki bir sürümünde yedekleme sunucusu V2 çalıştırırsanız, Azure Backup sunucusu Modern yedekleme depolama alanı yararlanamaz. Bunun yerine, yedekleme sunucusu V1 ile yaptığı gibi iş yüklerini korur. Daha fazla bilgi için yedekleme sunucusu sürümü görmek [koruma matrisi](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server"></a>Backup sunucusu birimleri

Yedekleme sunucusu V2 veya daha sonra depolama birimleri kabul eder. Bir birim eklediğinizde, Backup sunucusu dayanıklı dosya Modern yedekleme depolama alanı gerektiren sistemi (ReFS) birimine biçimlendirir. Bir birim eklemek ve gerekirse daha sonra genişletmek için bu iş akışı kullanmanızı öneririz:

1.  Bir VM'deki yedekleme sunucusu olarak ayarlayın.
2.  Bir depolama havuzunda sanal diskte bir birim oluşturun:
    1.  Bir depolama havuzuna bir disk ekleyin ve basit düzene sahip bir sanal disk oluşturun.
    2.  Herhangi bir ek disk ekleyin ve sanal diski genişletin.
    3.  Sanal disk üzerinde birimler oluşturun.
3.  Birimleri, yedekleme sunucusuna ekleyin.
4.  İş yükü algılayan depolamayı yapılandırın.

## <a name="create-a-volume-for-modern-backup-storage"></a>Modern yedekleme depolama alanı için bir birim oluşturun

Yedekleme sunucusu V2 kullanarak veya birimleri ile sonraki gibi disk depolama, depolama denetimi sağlamanıza yardımcı olabilir. Bir birim, tek bir disk olabilir. Ancak, depolama gelecekte genişletmek istiyorsanız, depolama alanları kullanılarak oluşturulan bir disk dışında bir birim oluşturun. Yedekleme depolama birimi genişletmek isterseniz bu yardımcı olabilir. Bu bölümde, bu kurulum ile bir birim oluşturmak için en iyi yöntemler sunar.

1. Sunucu Yöneticisi'nde **dosya ve depolama hizmetleri** > **birimleri** > **depolama havuzları**. Altında **FİZİKSEL DİSKLER**seçin **yeni depolama havuzu**.

    ![Yeni bir depolama havuzu oluşturma](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. İçinde **görevleri** açılan kutusunda **Yeni Sanal Disk**.

    ![Sanal disk ekleme](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Depolama havuzunu seçin ve ardından **Fiziksel Disk Ekle**.

    ![Fiziksel disk ekleme](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Fiziksel diski seçin ve ardından **sanal diski Genişlet**.

    ![Sanal diski genişletin](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Sanal diski seçin ve ardından **yeni birim**.

    ![Yeni bir birim oluşturun](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. İçinde **sunucu ve disk seçin** iletişim, sunucusu ve yeni diski seçin. Sonra **İleri**’yi seçin.

    ![Sunucu ve disk seçin](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Backup sunucusu disk depolamasına birim Ekle

Backup sunucusu için bir birim eklemek için **Yönetim** bölmesinde depolamayı yeniden tarayın ve ardından **Ekle**. Bir yedekleme sunucusu depolama alanı için eklenebilecek tüm birimlerin listesi görüntülenir. Kullanılabilir birimler seçili birimler listesine eklendikten sonra bunları yönetmenize yardımcı olmak için bir kolay ad verebilirsiniz. ReFS için bu birimler Modern yedekleme depolama alanı avantajlarından Backup sunucusu kullanabilmeniz biçimlendirmek için işaretleyin **Tamam**.

![Kullanılabilir birim Ekle](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>İş yükü algılayan depolamayı ayarlama

İş yükü algılayan depolama sayesinde, birimlerin tercihen belirli türlerdeki iş yüklerini depolama seçebilirsiniz. Örneğin, sık sık, yüksek hacimli yedeklemeler gerektiren iş yükleri depolamak için çok sayıda giriş/çıkış işlemi (IOPS) saniyede destekleyen pahalı birimler ayarlayabilirsiniz. SQL Server ile işlem günlüklerini örneğidir. VM'ler gibi daha düşük bir sıklıkla yedeklenen diğer iş yükleri, düşük maliyetli birimlere yedeklenebilir.

### <a name="update-dpmdiskstorage"></a>Update = DPMDiskStorage

İş yükü algılayan depolamayı ayarlama, güncelleştirme bir Azure Backup sunucusu üzerinde depolama havuzundaki bir birimin özelliklerini güncelleştiren DPMDiskStorage, PowerShell cmdlet'ini kullanarak ayarlayabilirsiniz. 

Sözdizimi:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
Aşağıdaki ekran görüntüsünde cmdlet Update = DPMDiskStorage PowerShell penceresinde gösterir.

![PowerShell penceresinde Update = DPMDiskStorage komutu](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

PowerShell kullanarak yaptığınız değişiklikler, yedekleme Sunucu Yöneticisi Konsolu'ndaki yansıtılır.

![Diskleri ve birimleri Yönetici Konsolu](./media/backup-mabs-add-storage/mabs-add-storage-9.png)


## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Eski depolamayı Modern yedekleme depolama alanına geçirme
Yedekleme sunucusu V2 veya işletim sistemini Windows Server 2016'ya yükseltme sürümüne yükselttikten sonra koruma gruplarınızı Modern yedekleme depolama kullanacak şekilde güncelleştirin. Varsayılan olarak, koruma grupları değiştirilmez. Bunların başlangıçta ayarlanan gibi çalışmaya devam eder.

Modern yedekleme depolama alanı kullanmak için koruma gruplarını güncelleştirmek isteğe bağlıdır. Koruma grubunu güncelleştirmek için verileri tut seçeneğini kullanarak tüm veri kaynaklarının korumasını durdurun. Ardından, veri kaynaklarını yeni bir koruma grubuna ekleyin.

1. Yönetici Konsolu'nda seçin **koruma** özelliği. İçinde **koruma grubu üyesi** listesinde üyeye sağ tıklayın ve ardından **üyenin korumasını Durdur**.

   ![Üyenin korumasını Durdur](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. İçinde **grubundan** iletişim kutusunda, kullanılan disk alanı ve depolama havuzu için kullanılabilir boş alanı gözden geçirin. Diskte kurtarma noktalarını bırakmak ve kendi ilişkilendirildikleri bekletme ilkesine göre sürelerinin dolmasına izin vermek için varsayılandır. **Tamam** düğmesine tıklayın.

   Kullanılan disk alanını hemen boş depolama havuzuna döndürülecek istiyorsanız belirleyin **diskteki çoğaltmayı Sil** yedekleme verileri (ve kurtarma noktalarını) silmek için onay kutusunu, bu üye ile ilişkili.

   ![Grup iletişim kutusundan kaldırma](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Modern yedekleme depolama alanı kullanan bir koruma grubu oluşturun. Korumasız veri kaynaklarını içerir.

## <a name="add-disks-to-increase-legacy-storage"></a>Eski depolama alanını artırmak için disk ekleme

Backup sunucusu ile eski depolama kullanmak istiyorsanız, eski depolama alanını artırmak için disk eklemek gerekebilir.

Disk depolama eklemek için:

1. Yönetici Konsolu'nda seçin **Yönetim** > **Disk Depolama** > **Ekle**.

    ![Disk depolama alanı iletişim kutusu Ekle](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. İçinde **Disk Depolama Ekle** iletişim kutusunda **disk Ekle**.

5. Kullanılabilir diskler listesi, seçin, eklemek istediğiniz diskleri seçin **Ekle**ve ardından **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar
Backup sunucusu yükledikten sonra sunucunuzu hazırlama veya bir iş yükü korumaya başlamak öğrenin.

- [Backup sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
- [Bir VMware sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-backup-server-vmware.md)
- [SQL sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-sql-mabs.md)
