---
title: Azure yığın dosyaları ve uygulamaları yedekleme | Microsoft Docs
description: Azure Backup, yedekleme ve Azure yığın dosyaları ve uygulamaları Azure yığın ortamınıza kurtarmak için kullanın.
services: backup
documentationcenter: ''
author: adiganmsft
manager: shivamg
editor: ''
keyword: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: adigan,markgal
ms.openlocfilehash: 905f6b13928d11243202059af0ad255971102da8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Dosyalar ve uygulamalar Azure yığında yedekleme
Koruma (veya yedeklemek için) Azure Yedekleme'yi kullanabilirsiniz dosyalar ve uygulamalar Azure yığında. Dosya ve uygulamaların yedeklemek için Azure yığın üzerinde çalışan bir sanal makine olarak Microsoft Azure yedekleme sunucusu yükleyin. Azure yedekleme sunucusu kez yüklediğiniz, kısa vadeli yedekleme verileri için kullanılabilir yerel depolama artırmak için Azure disk ekleyin. Azure yedekleme sunucusu uzun vadeli bekletme için Azure storage kullanır.

## <a name="azure-backup-server-protection-matrix"></a>Azure Backup Sunucusu koruma matrisi
Azure yedekleme sunucusu aşağıdaki Azure yığın sanal makine iş yüklerini korur.

| Korunan veri kaynağı | Koruma ve kurtarma |
| --------------------- | ----------------------- |
| Windows Server yarı yıllık kanalı - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2016 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 R2 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 - Entprise/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2008 R2 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2013 | Grup, veritabanı, ön uç, web sunucusu |
| SharePoint 2010 | Grup, veritabanı, ön uç, web sunucusu |
| Sistem Durumu | Sistem Durumu |
| Tam kurtarma (BMR) | BMR, sistem durumu, dosyalar, klasörler | 

## <a name="install-azure-backup-server"></a>Azure yedekleme Sunucusu'nu Yükle
Bir Azure yığın sanal makineye Azure yedekleme sunucusu yüklemek için bkz [Azure yedekleme sunucusu kullanarak iş yüklerini Yedeklemeye hazırlanma](backup-azure-microsoft-azure-backup.md). Yükleyip Azure yedekleme sunucusu yapılandırmadan önce aşağıdakilere dikkat edin:

### <a name="determining-size-of-virtual-machine"></a>Sanal makine boyutunu belirleme
Azure yedekleme sunucusu bir Azure yığın sanal makine üzerinde çalıştırılacak boyutunu A2 kullanın veya daha büyük. Bir sanal makine boyutu seçerken Yardım almak için indirin [Azure yığın VM boyutu hesaplayıcı](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Azure yığın sanal makinelerde sanal ağlar
Bir Azure yığın iş yükü kullanılan tüm sanal makineler aynı Azure sanal ağı ve Azure aboneliğine ait olmalıdır. 

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Yerel disk ve Azure yedekleme veri depolama
Azure Backup sunucusu işletimsel kurtarma için sanal makineye bağlı Azure disklerde yedek verileri depolar. Diskler ve depolama alanı sanal makineye bağlı sonra Azure yedekleme sunucusu depolama yönetir. Yedek veri depolama alanı miktarı sayısını ve her birine bağlı disklerin boyutunu bağlıdır [Azure yığın sanal makine](../azure-stack/user/azure-stack-storage-overview.md). Her Azure yığın VM boyutu en fazla sayıda sanal makineye bağlı diskler vardır. Örneğin, A2 dört diskler ' dir. A3 sekiz diskler ' dir. A4 16 disk ' dir. Yeniden disk sayısı ve boyutu, toplam yedekleme depolama havuzunu belirler.

> [!IMPORTANT]
> Yapmanız gerekenler **değil** beş günden fazla bir süre için Azure yedekleme sunucusuna bağlı disklerde işletimsel Kurtarma (Yedekleme) verileri korur.
>

Yedek veri depolama, yedekleme altyapısı Azure yığında azaltır. Veri beş günden daha eski ise, Azure'da depolanması gerekir.

Azure'da yedek verileri depolamak için oluşturun veya bir kurtarma Hizmetleri kasası kullanın. Azure yedekleme sunucusu iş yükü çalışma yedeklemek hazırlanırken şunları yapacaksınız [kurtarma Hizmetleri kasası yapılandırma](backup-azure-microsoft-azure-backup.md#recovery-services-vault). Bir kere yapılandırıldığında, yedekleme işi her çalıştığında, kasaya bir kurtarma noktası oluşturulur. Her kurtarma Hizmetleri kasası kadar 9999 kurtarma noktalarını tutar. Oluşturulan kurtarma noktaları ve ne kadar süreyle saklanacağını sayısına bağlı olarak, birçok yıldır yedekleme verileri koruyabilirsiniz. Örneğin, aylık kurtarma noktaları oluşturmak ve bunları beş yıl boyunca bekletmek.
 
### <a name="using-sql-server"></a>SQL Server kullanma
Azure yedekleme sunucusu veritabanı için uzak bir SQL Server kullanmak isterseniz, yalnızca bir Azure yığın SQL Server çalıştıran VM seçin.

### <a name="azure-backup-server-vm-performance"></a>Azure yedekleme sunucusu VM performans
Diğer sanal makinelerle paylaşılan, depolama hesabı boyut ve IOPS sınırları Azure yedekleme sunucusu sanal makine performansını etkileyebilir. Bu nedenle, Azure yedekleme sunucusu sanal makine için ayrı bir depolama hesabı kullanmanız gerekir. Azure yedekleme sunucusu üzerinde çalışan Azure Yedekleme aracısı için geçici depolama gerekir:
    - kendi kullanımı (bir önbellek konumu)
    - (yerel hazırlama alanı) buluttan geri yüklenen veriler
  
### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Backup geçici disk depolamayı yapılandırma
Birim D: kullanıcıya kullanılabilir geçici disk depolama her Azure yığın sanal makine birlikte`\`. Azure Yedekleme'nin ihtiyaç duyduğu yerel hazırlama alanı içinde D: bulunmasını yapılandırılabilir`\`, ve önbellek konumu C'de yerleştirilebilir`\`. Bu şekilde, depolama Azure yedekleme sunucusu sanal makinesine bağlı veri disklerinden çıktığınızda yontulmuş gerekiyor.

### <a name="scaling-deployment"></a>Dağıtım ölçeklendirme
Dağıtımınız ölçeklendirmek istiyorsanız aşağıdaki seçenekleriniz vardır:
  - Ölçeği artırma - D serisinin serisinden Azure yedekleme sunucusu sanal makineye boyutunu artırır ve yerel depolama alanını büyütmek [Azure yığın sanal makine yönergeleri başına](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Veri Boşalt - eski verileri Azure yedekleme sunucusuna göndermek ve yalnızca en yeni verileri Azure yedekleme sunucusuna bağlı depolama korur.
  - -Daha fazla Azure yedekleme iş yüklerini korumak için sunucu eklemek ölçeğini.


## <a name="bare-metal-recovery-for-azure-stack-vm"></a>Azure yığın VM için tam kurtarma

Tam kurtarma (BMR) yedekleme işletim sistemi dosyalarını ve kullanıcı verileri hariç tüm kritik birim verilerini korur. Bir BMR yedeklemesi, sistem durumu yedeklemesi içerir. Aşağıdaki yordamlar, BMR verileri geri yüklemek açıklanmaktadır. 

### <a name="run-recovery-on-the-azure-backup-server"></a>Kurtarma Azure yedekleme sunucusunda çalıştırın 

Azure Yedekleme Sunucusu konsolunu açın.

1. Konsolunda, **kurtarma**, Kurtar öğesini tıklatıp istediğiniz makineyi Bul **tam kurtarma**.
2. Kullanılabilir kurtarma noktalarını görünür Takvim üzerinde kalın. Tarih ve saat için kullanmak istediğiniz kurtarma noktasını seçin.
3. İçinde **kurtarma türünü seçin**seçin **bir ağ klasörüne Kopyala**.
4. İçinde **hedef belirtin**veri kopyalamak istediğiniz yerde seçin. Seçilen hedefin kurtarma noktası için yeterli alana sahip olması gerektiğini unutmayın. Yeni bir klasör oluşturmanız önerilir.
5. İçinde **kurtarma seçeneklerini belirtin**, uygulamak ve daha hızlı kurtarma için SAN tabanlı donanım anlık görüntülerini kullanılıp kullanılmayacağını seçmesine için güvenlik ayarlarını seçin.     Yalnızca bir SAN etkin bu işlevselliği ve oluşturma ve kopyayı yazılabilir yapmak için bölme yeteneği varsa SAN tabanlı donanım anlık görüntüleri bir seçenektir. Ayrıca çalışmak SAN tabanlı donanım anlık görüntüleri için Azure yedekleme sunucusu ve korunan makinenin aynı ağa bağlanmalıdır.
6. Bildirim seçeneklerini ayarlama ve tıklayın **kurtarmak** üzerinde **Özet** sayfası.

### <a name="set-up-the-share-location"></a>Paylaşım konum kurma
Azure yedekleme sunucusu konsolunda:
1. Geri yükleme konumunda, yedeği içeren klasöre gidin.
2. Böylece WindowsImageBackup klasörü köküdür paylaşılan klasörün kökünün paylaşır. Paylaşılan WindowsImageBackup klasörü değil, geri yükleme işlemi yedeği bulmaz. WinRE kullanarak bağlanmak için WinRE erişilebilir bir paylaşımı ve doğru IP adresi ve kimlik bilgileri gerekir.

### <a name="restore-the-machine"></a>Makineyi geri yükleme

1. BMR geri yüklemek istediğiniz sanal makinedeki bir yükseltilmiş komut istemi açın ve aşağıdaki komutları yazın. **/bootore** Windows RE sistem başlangıç otomatik olarak başlatıldığında başlar belirtir.
```
Reagent /boottore
shutdown /r /t 0
```

2. Açılan iletişim kutusunda, dil ve yerel ayarları seçin. Üzerinde **yükleme** ekran, select **Bilgisayarınızı onarın**.
3. Üzerinde **Sistem Kurtarma Seçenekleri** sayfasında, **önceden oluşturduğunuz sistem görüntüsünü kullanarak bilgisayarınızı geri**.
4. Üzerinde **sistem yansıması yedeği Seç** sayfasında, **Sistem Görüntüsü Seç** > **Gelişmiş** > **bir sistem görüntüsü Ara ağ üzerinde**. Bir uyarı görünürse seçin **Evet**. Görüntüyü seçmek için ağ paylaşımı için kimlik bilgilerini girin ve kurtarma noktası seçin gidin. Bu, Kurtarma noktasında kullanılabilir belirli yedeklemelerinize tarar. Kurtarma noktası seçin.
5. İçinde **yedeklemeyi geri yükleme şeklini seçin**seçin **diskleri Biçimlendir ve yeniden bölümle**. Sonraki ekranda ayarları doğrulayın ve **son** geri yükleme işini başlatın. Yeniden başlatma gerekli.

## <a name="see-also"></a>Ayrıca bkz.
Diğer iş yüklerini korumak için Azure Backup'ı kullanma hakkında daha fazla bilgi için aşağıdaki makalelere birine bakın:
- [SharePoint grubunun kurulumu yedekleyin](backup-azure-backup-sharepoint-mabs.md)
- [SQL server'ı Yedekle](backup-azure-sql-mabs.md)
