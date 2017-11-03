---
title: "Modern yedekleme depolama ile Azure yedekleme sunucusu v2 kullanın | Microsoft Docs"
description: "Azure yedekleme sunucusu v2'deki yeni özellikler hakkında bilgi edinin. Bu makalede, yedekleme sunucusu yüklemenizi yükseltmek açıklar."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: 751b9b495fd368dff1f72429707f5f33a0ccb569
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-storage-to-azure-backup-server-v2"></a>Azure yedekleme sunucusu v2 depolama ekleme

Azure yedekleme sunucusu v2 System Center 2016 veri koruma Yöneticisi Modern yedekleme depolama ile birlikte gelir. Modern yedekleme depolama alanı yüzde 50, üç kez daha hızlı ve daha verimli depolama yedeklemeler depolama alanından tasarruf etmenizi sağlar. Ayrıca, iş yükü algılayan depolama sunar. 

> [!NOTE]
> Modern yedekleme depolama kullanmak için Windows Server 2016 yedekleme sunucusu v2 çalıştırmanız gerekir. Windows Server'ın önceki bir sürümünü yedekleme sunucusu v2 çalıştırırsanız, Azure yedekleme sunucusu Modern yedekleme depolama yararlanamaz. Bunun yerine, yedekleme sunucusu v1 ile yaptığı gibi iş yüklerini korur. Daha fazla bilgi için bkz: yedekleme sunucu sürümü [koruma matris](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server-v2"></a>Yedek sunucu v2 birimleri

Yedek sunucu v2 depolama birimleri kabul eder. Bir birim eklediğinizde, yedekleme sunucusu dayanıklı dosya Modern yedekleme depolama gerektiren sistemi (ReFS) birimine biçimlendirir. Bir birim eklemek ve gerekirse daha sonra genişletmek için bu iş akışı kullanmanızı öneririz:

1.  Bir VM üzerinde yedekleme sunucusu v2 ayarlayın.
2.  Bir depolama havuzunda bir sanal disk üzerinde bir birim oluşturun:
    1.  Bir depolama havuzuna bir disk ekleyin ve Basit Düzen sanal diski oluşturun.
    2.  Tüm ek disk ekleyin ve sanal diski genişletme.
    3.  Birim üzerindeki sanal diski oluşturun.
3.  Birimleri yedekleme sunucusuna ekleyin.
4.  İş yükü algılayan depolama yapılandırın.

## <a name="create-a-volume-for-modern-backup-storage"></a>Modern yedekleme depolama bir birim oluşturun

Yedekleme sunucusu v2 birimlerle kullanarak disk depolaması, depolama üzerinde denetim tutmanıza yardımcı olabilir. Bir birim, tek bir disk olabilir. Ancak, depolama gelecekte genişletmek istiyorsanız, depolama alanları kullanılarak oluşturulan bir disk dışında bir birim oluşturun. Yedekleme depolama birimi genişletmek istiyorsanız, bu yardımcı olabilir. Bu bölümde, bu kurulum ile bir birim oluşturmak için en iyi yöntemler sunar.

1. Sunucu Yöneticisi'nde seçin **dosya ve depolama hizmetleri** > **birimleri** > **depolama havuzları**. Altında **FİZİKSEL DİSKLER**seçin **yeni depolama havuzu**. 

    ![Yeni bir depolama havuzu oluşturma](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. İçinde **görevleri** açılan kutusunda **Yeni Sanal Disk**.

    ![Sanal disk ekleme](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Depolama havuzunu seçin ve ardından **Fiziksel Disk Ekle**.

    ![Fiziksel disk ekleme](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Fiziksel diski seçin ve ardından **sanal diski Genişlet**.

    ![Sanal diski Genişlet](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Sanal diski seçin ve ardından **yeni birim**.

    ![Yeni bir birim oluşturun](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. İçinde **sunucu ve disk seçin** iletişim, sunucusu ve yeni diski seçin. Ardından, seçin **sonraki**.

    ![Sunucu ve disk seçin](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Birimleri yedekleme sunucusu disk depolama alanına ekleme

Bir birim yedekleme sunucusuna eklemek için **Yönetim** bölmesinde, depolama birimini yeniden tarayın ve ardından **Ekle**. Yedekleme sunucusu depolama alanına eklemek kullanılabilir tüm birimleri listesi görüntülenir. Kullanılabilir birimleri, seçilen birimleri listesine eklendikten sonra bunları yönetmenize yardımcı olmak için bir kolay ad verebilirsiniz. Yedekleme sunucusu Modern yedekleme depolama yararları kullanabilmek için bu birimleri ReFS için biçimlendirmek için **Tamam**.

![Kullanılabilir birimleri ekleyin](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>İş yükü algılayan depolama alanı ayarlama

İş yükü uyumlu depolama ile tercihen belirli türdeki iş yüklerini depolayan birimleri seçebilirsiniz. Örneğin, sık kullanılan, yüksek hacimli yedeklemeler gerektiren iş yüklerini depolamak için çok sayıda giriş/çıkış işlemi (IOPS) saniyede destekleyen pahalı birimler ayarlayabilirsiniz. SQL Server ile işlem günlükleri örneğidir. Sanal makineleri gibi daha az sıklıkta yedeklenen diğer iş yükleri için düşük maliyetli birimler yedeklenebilir.

### <a name="update-dpmdiskstorage"></a>Güncelleştirme DPMDiskStorage

Data Protection Manager sunucusundaki depolama havuzunda birim özelliklerini güncelleştiren güncelleştirme-DPMDiskStorage PowerShell cmdlet'ini kullanarak, iş yükü algılayan depolama alanı ayarlayabilirsiniz.

Sözdizimi:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
Aşağıdaki ekran görüntüsünde PowerShell penceresinde güncelleştirme DPMDiskStorage cmdlet'i gösterilmektedir.

![PowerShell penceresinde güncelleştirme DPMDiskStorage komutu](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

PowerShell kullanarak yaptığınız değişiklikler yedekleme sunucu Yönetici konsolunda yansıtılır.

![Diskleri ve birimleri Yönetici konsolunda](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a>Sonraki adımlar
Yedekleme sunucusu yükledikten sonra sunucunuzu hazırlama ya da bir iş yükü korumaya başlamak öğrenin.

- [Yedekleme sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
- [VMware sunucusunu yedeklemek için yedekleme sunucusu kullanın](backup-azure-backup-server-vmware.md)
- [SQL Server için yedekleme sunucusu kullanın](backup-azure-sql-mabs.md)

