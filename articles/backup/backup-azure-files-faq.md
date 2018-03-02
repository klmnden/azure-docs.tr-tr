---
title: "Azure Dosyalarını yedekleme hakkında SSS"
description: "Bu makalede, Azure'da Azure Dosyalarınızın nasıl korunacağına ilişkin ayrıntılar sağlanır. Bu özet, arama sonucunda görüntülenir."
services: backup
keywords: "SEO uzmanınıza danışmadan anahtar sözcükler eklemeyin veya anahtar sözcükleri düzenlemeyin."
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: d37e119709bc9d4643fcaa9512b5209d4139515e
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Azure Dosyalarını yedekleme ile ilgili sorular
Bu makale, Azure Dosyalarını yedekleme hakkındaki yaygın sorulara yanıtlar sunar. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

Bu makaledeki bölümleri hızlı taramak için **bu makale altında** sağ taraftaki bağlantıları kullanın.

## <a name="configuring-the-backup-job-for-azure-files"></a>Azure Dosyaları için yedekleme işini yapılandırma

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-file-shares-br"></a>Korumak istediğim ve geçerli Dosya paylaşımları içeren Depolama Hesaplarımın bazılarını neden göremiyorum? <br/>
Azure Dosyaları Yedeklemesi şu anda Önizleme aşamasında olup yalnızca desteklenen Depolama Hesapları, Yedekleme için yapılandırılabilir. Aradığınız Depolama Hesabının, desteklenen bir depolama hesabı olduğundan emin olun.

### <a name="why-cant-i-see-some-of-my-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Yedeklemeyi yapılandırmaya çalışırken, Depolama Hesabında Dosya paylaşımlarımdan bazılarını neden göremiyorum? <br/>
Dosya paylaşımının, aynı Kurtarma Hizmetleri kasasında zaten korumalı olup olmadığını denetleyin. Korumak istediğiniz Dosya paylaşımının yakın zamanda silinmemiş olduğundan emin olun.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Azure Dosya Eşitleme’deki bir Eşitleme Grubuna bağlanan Dosya Paylaşımlarını neden koruyamıyorum? <br/>
Eşitleme Gruplarına bağlanan Azure Dosya Paylaşımlarının koruması, sınırlı önizleme aşamasındadır. Lütfen istenen erişime yönelik Abonelik Kimliğiniz ile birlikte [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) adresine yazın. 

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>Azure Dosya paylaşımlarını hangi bölgelerde yedekleyebilirim? <br/>
Azure Dosya paylaşımları için yedekleme şu anda Önizleme aşamasındadır ve yalnızca aşağıdaki bölgelerde kullanılabilir. 
-   Kanada Orta (CNC)
-   Kanada Doğu (CE) 
-   Orta ABD (CUS)
-   Doğu Asya (EA)
-   Doğu Avustralya (AE) 
-   Hindistan Orta (INC) 
-   Orta Kuzey ABD (NCUS) 
-   UK Güney (UKS) 
-   UK Batı (UKW) 
-   Batı Orta ABD (WCUS)
-   Batı ABD 2 (WUS 2)

Azure Dosya paylaşımları için yedekleme, *23 Şubat* tarihinden itibaren aşağıdaki bölgelerde kullanılabilir olacaktır.
-   Avustralya Güneydoğu (ASE) 
-   Brezilya Güney (BRS) 
-   Doğu ABD (EUS) 
-   Doğu ABD 2 (EUS2) 
-   Kuzey Avrupa (NE) 
-   Orta Güney ABD (SCUS) 
-   Güneydoğu Asya (SEA)
-   Batı Avrupa (WE) 
-   Batı ABD (WUS)  

Yukarıda belirtilmeyen bir bölgede kullanmanız gerekiyorsa lütfen [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) adresine yazın.

### <a name="how-many-file-shares-can-i-protect-in-a-vaultbr"></a>Kasada kaç tane dosya paylaşımını koruyabilirim?<br/>
Önizleme sırasında, Kasa başına en fazla 25 Depolama Hesabından Dosya paylaşımlarını koruyabilirsiniz. Tek bir kasada en fazla 200 Dosya paylaşımını da koruyabilirsiniz. 

## <a name="backup"></a>Backup

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Dosya paylaşımı başına kaç tane İsteğe Bağlı yedekleme alabilirim? <br/>
Herhangi bir anda, ilkeniz tarafından tanımlandığı şekilde Azure Backup tarafından alınanlar da dahil olmak üzere, bir dosya paylaşımı için en fazla 200 Anlık Görüntünüz olabilir. Bu sınıra ulaşılması nedeniyle yedeklemeleriniz başarısız olmaya başlarsa lütfen İsteğe Bağlı geri yükleme noktalarını uygun şekilde silin.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Depolama Hesabımda Sanal Ağlar etkinleştirildikten sonra, hesaptaki dosya paylaşımlarının Yedeklemesi başarısız olmaya başladı. Neden?
Şu anda Azure Dosyalarının yedeklemesi, Sanal Ağların etkinleştirilmiş olduğu Depolama Hesapları için desteklenmez. Yedeklemek istediğiniz Depolama Hesaplarında Sanal Ağları devre dışı bırakın. 

## <a name="restore"></a>Geri Yükleme

### <a name="can-i-recover-from-a-deleted-file-share-br"></a>Silinen bir dosya paylaşımından kurtarma gerçekleştirebilir miyim? <br/>
Bir dosya paylaşımını silmeye çalıştığınızda yedeklemelerin listesi gösterilir ve devam ederseniz bu yedeklemeler silinip bir onay aranır. Silinen bir dosya paylaşımından geri yükleme yapamazsınız.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-a-file-share-br"></a>Dosya paylaşımındaki korumayı durdurursam yedeklemelerden geri yükleme yapabilir miyim? <br/>
Evet. Korumayı durdurduğunuzda **Yedekleme Verilerini Koru** seçeneğini belirlediyseniz tüm mevcut geri yükleme noktalarından geri yükleme yapabilirsiniz.

## <a name="manage-backup"></a>Yedeklemeyi Yönetme

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Azure Backup tarafından alınan anlık görüntülere erişebilir ve bu görüntüleri bağlayabilir miyim? <br/>
Azure Backup tarafından alınan tüm Anlık Görüntülere, portaldaki, PowerShell veya CLI’daki Anlık Görüntüler Görüntülenerek erişilebilir. Buradaki yordamı kullanarak bunları bağlayabilirsiniz.

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Yedeklemeler için yapılandırabileceğim en yüksek bekletme süresi nedir? <br/>
Azure Dosya paylaşımları için yedekleme, günlük yedeklemelerinizi en fazla 120 gün bekletme özelliği sunar.

### <a name="what-happens-when-i-change-the-backup-policy-for-a-file-share-br"></a>Bir dosya paylaşımı için Yedekleme ilkesini değiştirdiğimde ne olur? <br/>
Dosya paylaşımlarında yeni bir ilke uygulandığında yeni ilkenin zamanlama ve bekletmesi geçerli olur. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.


## <a name="see-also"></a>Ayrıca bkz.
Bu bilgiler yalnızca Azure Dosyalarının yedeklenmesiyle ilgilidir. Azure Backup’ın diğer yönleri hakkında daha fazla bilgi edinmek için Yedekleme ile ilgili diğer sık kullanılan sorulara bakın:
-  [Kurtarma Hizmetleri kasası hakkında SSS](backup-azure-backup-faq.md)
-  [Azure VM yedeklemesi hakkında SSS](backup-azure-vm-backup-faq.md)
-  [Azure Backup aracısı hakkında SSS](backup-azure-file-folder-backup-faq.md)
