---
title: Azure Dosyalarını yedekleme hakkında SSS
description: Bu makalede, Azure dosya paylaşımlarınızın nasıl korunacağına ilişkin ayrıntılar sağlanır.
services: backup
author: dcurwin
ms.author: dacurwin
ms.date: 01/31/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: 0c9e68a14aa18726a751709e99953d494c62308e
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275541"
---
# <a name="questions-about-backing-up-azure-files"></a>Azure Dosyalarını yedekleme ile ilgili sorular
Bu makale, Azure Dosyalarını yedekleme hakkındaki yaygın sorulara yanıtlar sunar. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

Bu makaledeki bölümleri hızlı taramak için **bu makale altında** sağ taraftaki bağlantıları kullanın.

## <a name="configuring-the-backup-job-for-azure-files"></a>Azure Dosyaları için yedekleme işini yapılandırma

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares-br"></a>Korumak istediğim ve geçerli Azure dosya paylaşımları içeren Depolama Hesaplarımın bazılarını neden göremiyorum? <br/>
Azure Dosya Paylaşımları için Yedekleme, önizleme sırasında her Depolama Hesabı türünü desteklemez. Desteklenen Depolama Hesaplarının listesini görüntülemek için [buraya](troubleshoot-azure-files.md#limitations-for-azure-file-share-backup-during-preview) başvurun. Aradığınız Depolama Hesabının zaten korumalı veya başka bir Kasa ile kayıtlı olması da mümkündür. Depolama Hesabını koruma için diğer Kasalarda bulmak için kasadan [kaldırın](troubleshoot-azure-files.md#configuring-backup).

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Yedeklemeyi yapılandırmaya çalışırken, Depolama Hesabında Azure dosya paylaşımlarımdan bazılarını neden göremiyorum? <br/>
Azure dosya paylaşımının, aynı Kurtarma Hizmetleri kasasında zaten korumalı olup olmadığını veya yakın zamanda silinip silinmediğini denetleyin.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Azure Dosya Eşitleme’deki bir Eşitleme Grubuna bağlanan Dosya Paylaşımlarını koruyabilir miyim? <br/>
Evet. Eşitleme Gruplarına bağlanan Azure Dosya Paylaşımlarının koruması etkindir ve Genel önizlemenin parçasıdır.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-did-not-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>Dosya paylaşımlarını yedeklemeye çalışırken, dosya paylaşımlarını keşfetmek için bir Depolama Hesabına tıkladım. Ancak koruyamadım. Bu dosya paylaşımlarını başka bir Kasa ile nasıl koruyabilirim?
Yedeklemeye çalışırken, dosya paylaşımlarını keşfetmek için bir Depolama Hesabı seçildiğinde Depolama Hesabı, bu işlemin yapıldığı Kasaya kaydolur. Dosya paylaşımları farklı bir Kasa ile korumayı seçerseniz, bu Kasadan seçilen Depolama Hesabının [Kaydını Sil](troubleshoot-azure-files.md#configuring-backup)’in.

### <a name="can-i-change-the-vault-to-which-i-backup-my-file-shares"></a>Dosya paylaşımlarımı yedeklediğim Kasayı değiştirebilir miyim?
Evet. Ancak bağlanan Kasadan [Korumayı Durdurmanız](backup-azure-files.md#stop-protecting-an-azure-file-share), bu Depolama Hesabının [Kaydını Sil](troubleshoot-azure-files.md#configuring-backup)’meniz ve farklı bir Kasadan bunu korumanız gerekir.

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>Azure Dosya paylaşımlarını hangi bölgelerde yedekleyebilirim? <br/>
Azure Dosya paylaşımları için yedekleme şu anda Önizleme aşamasındadır ve yalnızca aşağıdaki bölgelerde kullanılabilir:
- Avustralya Doğu (AE)
- Avustralya Güneydoğu (ASE)
- Brezilya Güney (BRS)
- Kanada Orta (CNC)
- Kanada Doğu (CE)
- Orta ABD (CUS)
- Doğu Asya (EA)
- Doğu ABD (EUS)
- Doğu ABD 2 (EUS2)
- Japonya Doğu (JPE)
- Japonya Batı (JPW)
- Hindistan Orta (INC)
- Hindistan Güney (INS)
- Kore Orta (KRC)
- Kore Güney (KRS)
- Orta Kuzey ABD (NCUS)
- Kuzey Avrupa (NE)
- Orta Güney ABD (SCUS)
- Güneydoğu Asya (SEA)
- UK Güney (UKS)
- UK Batı (UKW)
- Batı Avrupa (WE)
- Batı ABD (WUS)
- Batı Orta ABD (WCUS)
- Batı ABD 2 (WUS 2)
- ABD Devleti Arizona (UGA)
- ABD Devleti Texas (UGT)
- ABD Devleti Virginia (UGV)

Yukarıda belirtilmeyen bir bölgede kullanmanız gerekiyorsa [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) adresine yazın.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vaultbr"></a>Kasada kaç tane Azure dosya paylaşımını koruyabilirim?<br/>
Önizleme sırasında, Kasa başına en fazla 50 Depolama Hesabından Azure dosya paylaşımını koruyabilirsiniz. Tek bir kasada en fazla 200 Azure dosya paylaşımını koruyabilirsiniz.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>Aynı Depolama Hesabından, farklı Kasalara iki farklı dosya paylaşımını koruyabilir miyim?
Hayır. Depolama Hesabındaki tüm dosya paylaşımları, aynı Kasa tarafından korunabilir.

## <a name="backup"></a>Backup

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Dosya paylaşımı başına kaç tane İsteğe Bağlı yedekleme alabilirim? <br/>
Zamanın herhangi bir noktasında dosya paylaşımı için en fazla 200 Anlık Görüntüye sahip olabilirsiniz. Sınır, ilkenizde tanımlandığı şekilde, Azure Backup tarafından alınan anlık görüntüleri içerir. Sınıra ulaştıktan sonra yedekleriniz başarısız olmaya başlarsa, daha sonraki yedeklemelerin başarılı olması için İsteğe Bağlı geri yükleme noktalarını silin.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Depolama Hesabımda Sanal Ağlar etkinleştirildikten sonra, hesaptaki dosya paylaşımlarının Yedeklemesi başarısız olmaya başladı. Neden?
Azure dosya paylaşımları için Yedekleme, Sanal Ağların etkin olduğu Depolama Hesaplarını desteklemez. Başarılı bir yedekleme işlemleri için Depolama Hesaplarında Sanal Ağları devre dışı bırakın.

## <a name="restore"></a>Geri Yükleme

### <a name="can-i-recover-from-a-deleted-azure-file-share-br"></a>Silinen bir Azure dosya paylaşımından kurtarma gerçekleştirebilir miyim? <br/>
Bir Azure dosya paylaşımı silindiğinde, size silinecek yedeklemelerin listesi gösterilir ve onay beklenir. Silinen Azure dosya paylaşımı geri yüklenemez.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share-br"></a>Azure dosya paylaşımındaki korumayı durdurursam yedeklemelerden geri yükleme yapabilir miyim? <br/>
Evet. Korumayı durdurduğunuzda **Yedekleme Verilerini Koru** seçeneğini belirlediyseniz tüm mevcut geri yükleme noktalarından geri yükleme yapabilirsiniz.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Ben bir devam eden geri yükleme işi iptal edersem ne olur?
Devam eden geri yükleme işi iptal edilirse, geri yükleme işlemi durdurur ve tüm dosyaları iptalden önce geri kalın herhangi düzeyine olmadan yapılandırılmış hedef (özgün veya alternatif konum). 


## <a name="manage-backup"></a>Yedeklemeyi Yönetme

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares-br"></a>PowerShell Azure dosya paylaşımlarının yapılandırma/yönetmek/geri yükleme yedeklemeler için kullanabilir miyim? <br/>
Evet. Lütfen ayrıntılı belgelere başvurun [burada](backup-azure-afs-automation.md)

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Azure Backup tarafından alınan anlık görüntülere erişebilir ve bu görüntüleri bağlayabilir miyim? <br/>
Azure Backup tarafından alınan tüm Anlık Görüntülere, portaldaki, PowerShell veya CLI’daki Anlık Görüntüler Görüntülenerek erişilebilir. Azure Dosyaları paylaşım anlık görüntüleri hakkında daha fazla bilgi edinmek için bkz. [Azure Dosyaları için paylaşım anlık görüntülerine genel bakış (önizleme)](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Yedeklemeler için yapılandırabileceğim en yüksek bekletme süresi nedir? <br/>
Azure dosya paylaşımları için yedekleme bekletme ilkelerini ayarlama 180 gün olarak yapılandırma olanağı sunar. Ancak, [PowerShell "talep üzerine yedekleme" seçeneğinde](backup-azure-afs-automation.md#trigger-an-on-demand-backup), hatta 10 yıl için bir kurtarma noktası tutabilirsiniz.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share-br"></a>Bir Azure dosya paylaşımı için Yedekleme ilkesini değiştirdiğimde ne olur? <br/>
Dosya paylaşımlarında yeni bir ilke uygulandığında yeni ilkenin zamanlama ve bekletmesi geçerli olur. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

## <a name="see-also"></a>Ayrıca bkz.
Bu bilgiler yalnızca Azure Dosyalarının yedeklenmesiyle ilgilidir. Azure Backup’ın diğer yönleri hakkında daha fazla bilgi edinmek için Yedekleme ile ilgili diğer sık kullanılan sorulara bakın:
-  [Kurtarma Hizmetleri kasası hakkında SSS](backup-azure-backup-faq.md)
-  [Azure VM yedeklemesi hakkında SSS](backup-azure-vm-backup-faq.md)
-  [Azure Backup aracısı hakkında SSS](backup-azure-file-folder-backup-faq.md)
