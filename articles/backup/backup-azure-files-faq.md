---
title: Azure Dosyalarını yedekleme hakkında SSS
description: Bu makalede, Azure dosya paylaşımlarınızın nasıl korunacağına ilişkin ayrıntılar sağlanır.
services: backup
keywords: SEO uzmanınıza danışmadan anahtar sözcük eklemeyin veya anahtar sözcükleri düzenlemeyin.
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: 850d4d1e2ef6a13fcd8a072e6da210d558c7769b
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Azure Dosyalarını yedekleme ile ilgili sorular
Bu makale, Azure Dosyalarını yedekleme hakkındaki yaygın sorulara yanıtlar sunar. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

Bu makaledeki bölümleri hızlı taramak için **bu makale altında** sağ taraftaki bağlantıları kullanın.

## <a name="configuring-the-backup-job-for-azure-files"></a>Azure Dosyaları için yedekleme işini yapılandırma

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares-br"></a>Korumak istediğim ve geçerli Azure dosya paylaşımları içeren Depolama Hesaplarımın bazılarını neden göremiyorum? <br/>
Azure Dosya Paylaşımları için Yedekleme, önizleme sırasında her Depolama Hesabı türünü desteklemez. Desteklenen Depolama Hesaplarının listesini görüntülemek için [buraya](troubleshoot-azure-files.md#preview-boundaries) başvurun.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Yedeklemeyi yapılandırmaya çalışırken, Depolama Hesabında Azure dosya paylaşımlarımdan bazılarını neden göremiyorum? <br/>
Azure dosya paylaşımının, aynı Kurtarma Hizmetleri kasasında zaten korumalı olup olmadığını veya yakın zamanda silinip silinmediğini denetleyin.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-file-sync-br"></a>Azure Dosya Eşitleme’deki bir Eşitleme Grubuna bağlanan Dosya Paylaşımlarını neden koruyamıyorum? <br/>
Eşitleme gruplarına bağlanan Azure dosya paylaşımlarının koruması, sınırlı önizleme aşamasındadır. Lütfen istenen erişime yönelik Abonelik Kimliğiniz ile birlikte [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) adresine yazın. 

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>Azure Dosya paylaşımlarını hangi bölgelerde yedekleyebilirim? <br/>
Azure Dosya paylaşımları için yedekleme şu anda Önizleme aşamasındadır ve yalnızca aşağıdaki bölgelerde kullanılabilir: 
-   Avustralya Güneydoğu (ASE) 
- Brezilya Güney (BRS)
- Kanada Orta (CNC)
-   Kanada Doğu (CE)
-   Orta ABD (CUS)
-   Doğu Asya (EA)
-   Doğu Avustralya (AE) 
-   Doğu ABD (EUS)
-   Doğu ABD 2 (EUS2)
-   Hindistan Orta (INC) 
-   Orta Kuzey ABD (NCUS) 
-   Kuzey Avrupa (NE) 
-   Orta Güney ABD (SCUS) 
-   Güneydoğu Asya (SEA)
-   UK Güney (UKS) 
-   UK Batı (UKW) 
-   Batı Avrupa (WE) 
-   Batı ABD (WUS)
-   Batı Orta ABD (WCUS)
-   Batı ABD 2 (WUS 2)

Yukarıda belirtilmeyen bir bölgede kullanmanız gerekiyorsa lütfen [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) adresine yazın.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vaultbr"></a>Kasada kaç tane Azure dosya paylaşımını koruyabilirim?<br/>
Önizleme sırasında, Kasa başına en fazla 25 Depolama Hesabından Azure dosya paylaşımını koruyabilirsiniz. Tek bir kasada en fazla 200 Azure dosya paylaşımını koruyabilirsiniz. 

## <a name="backup"></a>Backup

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Dosya paylaşımı başına kaç tane İsteğe Bağlı yedekleme alabilirim? <br/>
Zamanın herhangi bir noktasında dosya paylaşımı için en fazla 200 Anlık görüntüye sahip olabilirsiniz. Sınır, ilkenizde tanımlandığı şekilde, Azure Backup tarafından alınan anlık görüntüleri içerir. Sınıra ulaştıktan sonra yedekleriniz başarısız olmaya başlarsa, daha sonraki yedeklemelerin başarılı olması için İsteğe Bağlı geri yükleme noktalarını silin.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Depolama Hesabımda Sanal Ağlar etkinleştirildikten sonra, hesaptaki dosya paylaşımlarının Yedeklemesi başarısız olmaya başladı. Neden?
Azure dosya paylaşımları için Yedekleme, Sanal Ağların etkin olduğu Depolama Hesaplarını desteklemez. Başarılı bir yedekleme işlemleri için Depolama Hesaplarında Sanal Ağları devre dışı bırakın. 

## <a name="restore"></a>Geri Yükleme

### <a name="can-i-recover-from-a-deleted-azure-file-share-br"></a>Silinen bir Azure dosya paylaşımından kurtarma gerçekleştirebilir miyim? <br/>
Bir Azure dosya paylaşımı silindiğinde, size yedeklemelerin listesi gösterilir ve onay beklenir. Silinen Azure dosya paylaşımı geri yüklenemez.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share-br"></a>Azure dosya paylaşımındaki korumayı durdurursam yedeklemelerden geri yükleme yapabilir miyim? <br/>
Evet. Korumayı durdurduğunuzda **Yedekleme Verilerini Koru** seçeneğini belirlediyseniz tüm mevcut geri yükleme noktalarından geri yükleme yapabilirsiniz.

## <a name="manage-backup"></a>Yedeklemeyi Yönetme

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Azure Backup tarafından alınan anlık görüntülere erişebilir ve bu görüntüleri bağlayabilir miyim? <br/>
Azure Backup tarafından alınan tüm Anlık Görüntülere, portaldaki, PowerShell veya CLI’daki Anlık Görüntüler Görüntülenerek erişilebilir. Azure Dosyaları paylaşım anlık görüntüleri hakkında daha fazla bilgi edinmek için bkz. [Azure Dosyaları için paylaşım anlık görüntülerine genel bakış (önizleme)](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Yedeklemeler için yapılandırabileceğim en yüksek bekletme süresi nedir? <br/>
Azure dosya paylaşımları için yedekleme, günlük yedeklemelerinizi en fazla 120 gün bekletme özelliği sunar.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share-br"></a>Bir Azure dosya paylaşımı için Yedekleme ilkesini değiştirdiğimde ne olur? <br/>
Dosya paylaşımlarında yeni bir ilke uygulandığında yeni ilkenin zamanlama ve bekletmesi geçerli olur. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

## <a name="see-also"></a>Ayrıca bkz.
Bu bilgiler yalnızca Azure Dosyalarının yedeklenmesiyle ilgilidir. Azure Backup’ın diğer yönleri hakkında daha fazla bilgi edinmek için Yedekleme ile ilgili diğer sık kullanılan sorulara bakın:
-  [Kurtarma Hizmetleri kasası hakkında SSS](backup-azure-backup-faq.md)
-  [Azure VM yedeklemesi hakkında SSS](backup-azure-vm-backup-faq.md)
-  [Azure Backup aracısı hakkında SSS](backup-azure-file-folder-backup-faq.md)