---
title: "Dosyaları bir Windows sunucusuna Azure yedeklemeden kurtarmak | Microsoft Docs"
description: "Bu öğretici, bir Windows sunucusuna Azure kurtarma öğeleri ayrıntıları verilmektedir."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: "Windows server yedekleme; dosyaları windows server geri; Yedekleme ve olağanüstü durum kurtarma"
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: saurabhsensharma;markgal;
ms.custom: 
ms.openlocfilehash: 28e0bc1414b0fea614f217dc3adf1484c1374018
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Dosyaları bir Windows Server'a Azure'dan kurtarma

Azure yedekleme, Windows Server Yedekleme tek tek öğelerin kurtarma sağlar. Bağımsız dosyaları kurtarmada, hızlı bir şekilde yanlışlıkla silinen dosyaları geri yüklemeniz gerekiyorsa yardımcı olur. Bu öğretici, Azure'da zaten gerçekleştirmiş yedeklerden öğeleri kurtarmak için Microsoft Azure kurtarma Hizmetleri Aracısı (MARS) Aracısı'nı nasıl kullanabileceğiniz kapsar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Tek tek öğelerin kurtarma 
> * Bir kurtarma noktası seçin 
> * Öğeleri bir kurtarma noktasından geri yükleme

Bu öğretici, zaten gerçekleştirdikten adımlarına varsayar [Azure için Windows Server Yedekleme](backup-configure-vault.md) ve Windows Server dosyalarınızda Azure en az bir yedeğine sahip.

## <a name="initiate-recovery-of-individual-items"></a>Tek tek öğelerin kurtarma

Microsoft Azure yedekleme adlı yararlı Kullanıcı Arabirimi Sihirbazı ile Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı yüklenir. Azure'da depolanan kurtarma noktalarından yedekleme verileri almak için Microsoft Azure kurtarma Hizmetleri (MARS) aracısı ile Microsoft Azure Yedekleme Sihirbazı çalışır. Dosyaları veya Windows Server geri yüklemek istediğiniz klasörleri belirlemek için Microsoft Azure Yedekleme Sihirbazı'nı kullanın. 

1. Açık **Microsoft Azure yedekleme** ek bileşenini. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. Sihirbazı'nda tıklatın **verileri kurtarabilirsiniz** içinde **Eylemler bölmesi** başlatmak için aracı konsolunun **verileri kurtarabilirsiniz** Sihirbazı.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. Üzerinde **Başlarken** sayfasında, **bu sunucu (sunucu adı)** tıklatıp **sonraki**.

4. Üzerinde **seçin kurtarma moduna** sayfasında **dosyalara ve klasörlere** ve ardından **sonraki** kurtarma noktası seçimi işlemine başlamak için.
 
5. Üzerinde **birim seçin ve tarih** sayfasında, dosya veya klasörleri tıklatın ve geri yüklemek istediğiniz içeren birimi seçin **bağlama**. Bir tarih seçin ve bir kurtarma noktası karşılık gelen açılan menüsünde bir saat seçin. İçinde tarihleri **kalın** o gün en az bir kurtarma noktası kullanılabilirliğini gösterir.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)
 
    Tıkladığınızda **bağlama**, Azure yedekleme kurtarma noktası bir disk olarak kullanılabilir yapar. Göz atın ve dosyaları diskten kurtarma.

## <a name="restore-items-from-a-recovery-point"></a>Öğeleri bir kurtarma noktasından geri yükleme

1. Kurtarma birimi bağlandıktan sonra tıklatın **Gözat** Windows Gezgini'ni açın ve kurtarmak istediğiniz klasörleri ve dosyaları bulmak için. 

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    Doğrudan kurtarma birimden dosyaları açmak ve dosyaları doğrulayabilirsiniz.

2. Windows Gezgini'nde, dosyaları ve/veya geri yükleme ve sunucu üzerindeki herhangi bir istenen konuma yapıştırmak için istediğiniz klasörleri kopyalayın.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Dosyaları ve/veya klasörleri üzerinde geri yüklemeyi tamamladıktan sonra **göz atın ve kurtarma dosyaları** sayfasında **verileri kurtarabilirsiniz** Sihirbazı'nı tıklatın **çıkarma**. 

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4.  Tıklatın **Evet** birim kaldırmak istediğinizi onaylamak için.

    Anlık görüntü kaldırılan, olduğunda **işi tamamlandı** görünür **işleri** Aracısı Konsol bölmesinde.

## <a name="next-steps"></a>Sonraki adımlar

Bu, yedekleme ve Windows Server verileri için Azure geri öğreticileri tamamlar. Azure yedekleme hakkında daha fazla bilgi için şifrelenmiş sanal makineleri yedekleme için PowerShell örnek bakın.

> [!div class="nextstepaction"]
> [Şifrelenmiş VM'yi yedeklemek](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
