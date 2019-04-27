---
title: Azure’dan Windows Server’a dosya kurtarma
description: Bu öğretici, Azure’dan Windows Server’a öğelerin kurtarılmasını açıklar.
services: backup
author: saurabhsensharma
manager: shivamg
keywords: windows server yedekleme; windows server’a dosyaları geri yükleme; yedekleme ve olağanüstü durum kurtarma
ms.service: backup
ms.topic: tutorial
ms.date: 2/14/2018
ms.author: saurse
ms.custom: mvc
ms.openlocfilehash: b01811d9c933802263e975b23b5d40cd77303766
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60722958"
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Azure’dan Windows Server’a dosya kurtarma

Azure Backup, Windows Server’ınızın yedeklemelerinden tek tek öğelerin kurtarılmasını sağlar. Yanlışlıkla silinen dosyaları hızlı şekilde geri yüklemeniz gerekirse tek tek dosyaların kurtarılması yararlı olur. Bu öğretici, Azure'da önceden gerçekleştirdiğiniz yedeklemelerden öğeleri kurtarmak için Microsoft Azure Kurtarma Hizmetleri Aracısı (MARS) aracısını kullanabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Tek tek öğelerin kurtarmasını başlatma 
> * Bir kurtarma noktası seçme 
> * Bir kurtarma noktasından öğeleri geri yükleme

Bu öğreticide, önceden [Windows Server’ı Azure’da yedekleme](backup-configure-vault.md) adımlarını gerçekleştirdiğiniz ve Azure’da Windows Server dosyalarınızın en az bir yedeklemesine sahip olduğunuz varsayılır.

## <a name="initiate-recovery-of-individual-items"></a>Tek tek öğelerin kurtarmasını başlatma

Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı ile Microsoft Azure Backup adlı yardımcı bir kullanıcı arabirimi sihirbazı yüklenir. Microsoft Azure Backup sihirbazı, Azure’da depolanan kurtarma noktalarından yedekleme verilerini almak için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı ile birlikte çalışır. Windows Server’a geri yüklemek istediğiniz dosyaları veya klasörleri belirlemek için Microsoft Azure Backup sihirbazını kullanın. 

1. **Microsoft Azure Backup** ek bileşenini açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. Sihirbazda, **Veri Kurtarma** sihirbazını başlatmak için aracı konsolunun **Eylemler Bölmesi**’nde **Veri Kurtar**’a tıklayın.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. **Başlarken** sayfasında **Bu sunucu (sunucu adı)** seçeneğini belirleyin ve **İleri**’ye tıklayın.

4. **Kurtarma Modunu Seç** sayfasında **Tek tek dosyalar ve klasörler** seçeneğini belirleyin ve sonra **İleri**’ye tıklayarak kurtarma noktası seçim işlemini başlatın.
 
5. **Birim ve Tarih Seçin** sayfasında, geri yüklemek istediğiniz dosyaları veya klasörleri içeren birimi seçin ve **Bağla**’ya tıklayın. Bir tarih seçin ve bir kurtarma noktasına karşılık gelen açılır menüden bir saat seçin. **Kalın** yazılan tarihler, o günde en az bir kurtarma noktasının kullanılabilir olduğunu belirtir.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)
 
    **Bağla**’ya tıkladığınızda Azure Backup, kurtarma noktasını bir disk olarak kullanılabilir hale getirir. Göz atıp diskteki dosyaları kurtarın.

## <a name="restore-items-from-a-recovery-point"></a>Bir kurtarma noktasından öğeleri geri yükleme

1. Kurtarma birimi bağlandıktan sonra **Gözat**’a tıklayıp Windows Gezgini'ni açın ve kurtarmak istediğiniz dosyaları ve klasörleri bulun. 

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    Doğrudan kurtarma biriminden dosyaları açabilir ve dosyaları doğrulayabilirsiniz.

2. Windows Gezgini'nde, geri yüklemek istediğiniz dosyaları ve/veya klasörleri kopyalayıp sunucuda istediğiniz bir konuma yapıştırın.

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Dosyaları ve/veya klasörleri geri yükleme işlemini tamamladığınızda, **Veri Kurtarma** sihirbazının **Dosyalara Göz At ve Dosyaları Kurtar** sayfasında **Çıkar**’a tıklayın. 

    ![Yedekleme beklemede](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4.  Birimi çıkarmak istediğinizi doğrulamak için **Evet**’e tıklayın.

    Anlık görüntü çıkarıldıktan sonra, aracı konsolunun **İşler** bölmesinde **İş Tamamlandı** görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Böylece, Windows Server verilerinin Azure’a yedeklenmesi ve geri yüklenmesi ile ilgili öğreticiler tamamlanmış olur. Azure Backup hakkında daha fazla bilgi edinmek için, şifrelenmiş sanal makinelerin yedeklenmesine yönelik PowerShell örneğine bakın.

> [!div class="nextstepaction"]
> [Şifrelenmiş sanal makineyi yedekleme](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
