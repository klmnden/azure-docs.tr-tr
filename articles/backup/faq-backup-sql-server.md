---
title: Azure Backup ile Azure vm'lerde SQL Server veritabanlarını yedekleme hakkında sık sorulan sorular
description: Azure Backup ile Azure vm'lerde SQL Server veritabanlarını yedekleme hakkında sık sorulan sorulara yanıtlar bulun.
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: sachdevaswati
ms.openlocfilehash: 8d6323c73e5313a29b7b0df09ebdd24a190879f5
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58876437"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Bir Azure VM yedeklemesi üzerinde çalışan SQL Server veritabanları hakkında SSS

Bu makalede, Azure sanal makinelerinde (VM) çalıştıran ve kullanan SQL Server veritabanlarını yedekleme hakkında sık sorulan sorular yanıtlanmaktadır [Azure Backup](backup-overview.md) hizmeti.

## <a name="does-the-solution-retry-or-auto-heal-the-backups"></a>Çözümü yeniden deneyin veya yedekleme otomatik olarak düzeltmek?

Bazı durumlarda, Azure Backup hizmeti, düzeltici yedeklemeler tetikler. Otomatik olarak düzeltmek için aşağıda belirtilen altı koşullardan herhangi biri olabilir:

  - Günlük veya değişiklik yedeği LSN doğrulama hatası nedeniyle başarısız olursa, sonraki günlük veya değişiklik yedeği yerine bir tam yedekleme dönüştürülür.
  - Bunun yerine, günlük veya değişiklik yedeği tam yedekleme, günlük veya değişiklik yedeği önce gerçekleştiyse, tam yedekleme dönüştürülür.
  - En son tam Yedekleme'nin-belirli bir noktaya 15 günden daha eski ise, sonraki günlük veya değişiklik yedeği yerine bir tam yedekleme dönüştürülür.
  - Bir uzantı yükseltmesi nedeniyle iptal edilmesi tüm yedekleme işleri yeniden yükseltme tamamlandıktan sonra uzantı çalışmaya tetiklenir.
  - Geri yükleme sırasında veritabanının üzerine yazmak tercih ederseniz sonraki günlük/fark yedekleme başarısız olur ve tam bir yedekleme yerine tetiklenir.
  - Tam yedekleme veritabanı kurtarma modelinde değiştirmek için son günlük zincirleri sıfırlamak için gerekli olduğu durumlarda, tam bir sonraki zamanlamaya göre otomatik olarak tetiklenen.

Otomatik-bir özelliği varsayılan olarak tüm kullanıcı için etkin olarak onarımı; Ancak durumunda bunu ayrılma seçin, ardından gerçekleştirmek aşağıda:

  * SQL Server örneğinde, *C:\Program Files\Azure iş yükü Backup\bin* klasör oluşturma veya düzenleme **ExtensionSettingsOverrides.json** dosya.
  * İçinde **ExtensionSettingsOverrides.json**ayarlayın *{"EnableAutoHealer": false}*.
  * Yaptığınız değişiklikleri kaydedin ve dosyayı kapatın.
  * SQL Server örneğinde açın **görevi yönetmek** ve yeniden **AzureWLBackupCoordinatorSvc** hizmeti.  

## <a name="can-i-control-as-to-how-many-concurrent-backups-run-on-the-sql-server"></a>SQL server üzerinde kaç tane eş zamanlı yedeklemeleri çalıştırma için farklı denetleyebilir miyim?

Evet. Yedekleme İlkesi bir SQL Server örneği üzerindeki etkiyi en aza indirmek için çalıştığı oranı kısıtlayabilirsiniz. Bu ayarı değiştirmek için:
1. SQL Server örneğinde, *C:\Program Files\Azure iş yükü Backup\bin* klasör oluşturma *ExtensionSettingsOverrides.json* dosya.
2. İçinde *ExtensionSettingsOverrides.json* dosya, değişiklik **DefaultBackupTasksThreshold** ayarını daha düşük bir değere (örneğin, 5). <br>
  `{"DefaultBackupTasksThreshold": 5}`

3. Yaptığınız değişiklikleri kaydedin ve dosyayı kapatın.
4. SQL Server örneğinde açın **Görev Yöneticisi'ni**. Yeniden **AzureWLBackupCoordinatorSvc** hizmeti.

> [!NOTE]
> Yine de devam edin ve herhangi bir zamanda kadar yedeklemelerin UX ile ancak göründükleri varsayalım, 5, yukarıdaki örnekte, bir kayan pencereye işlenebilir.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Bir ikincil çoğaltma tam yedekleme çalıştırabilir miyim?
SQL sınırlamaları göre ikincil kopyada kopyalama yalnızca tam yedekleme çalıştırabilirsiniz; Ancak, tam yedeklemeye izin verilmez.

## <a name="can-i-protect-availability-groups-on-premises"></a>Kullanılabilirlik grupları şirket içi koruyabilirim?
Hayır. Azure yedekleme, Azure'da çalışan SQL Server veritabanlarını korur. Bir kullanılabilirlik grubu (ağ), Azure ve şirket içi makineler arasında yayılır, yalnızca birincil çoğaltma Azure'da çalışıyorsa AG korunabilir. Ayrıca, Azure Backup kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde çalışan düğümleri korur.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kullanılabilirlik grupları bölgeler arasında koruyabilir miyim?
Azure Backup kurtarma Hizmetleri kasası algılayabilir ve kasa ile aynı bölgede bulunan tüm düğümleri koruyun. Birincil düğüm olan bölge yedekleme, SQL Server Always On kullanılabilirlik grubu birden çok Azure bölgesine yayılmış durumdaysa ayarlayın. Azure Backup, algılamak ve yedekleme tercihinize göre kullanılabilirlik grubundaki tüm veritabanlarını korumak. Yedekleme tercihinizi uyulmazsa, yedeklemeleri başarısız olur ve hata uyarısı alın.

## <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri uyarılar oluşturulur?
Hayır. Başarılı yedekleme işleri uyarıları oluşturma. Uyarılar, başarısız olan yedekleme işleri için gönderilir. Portal uyarılar için ayrıntılı davranışı belgelenen [burada](backup-azure-monitoring-built-in-monitor.md). Ancak, ilgilendiğiniz durumunda uyarılar sahip daha başarılı işler için kullanabileceğiniz [Azure İzleyicisi'ni kullanarak izleme](backup-azure-monitoring-use-azuremonitor.md).

## <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Zamanlanan yedekleme işlerinin yedekleme işi menüsünde görebilir miyim?
**Yedekleme işi** menü geçici yalnızca yedekleme işleri gösterilir. Zamanlanmış iş için kullanmak [Azure İzleyicisi'ni kullanarak izleme](backup-azure-monitoring-use-azuremonitor.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Gelecekteki veritabanları için Yedekleme otomatik olarak eklenir?
Evet, bu özellik ile elde edebileceğiniz [otomatik korumayı](backup-sql-server-database-azure-vms.md#enable-auto-protection).  

## <a name="if-i-delete-a-database-from-an-autoprotected-instance-what-will-happen-to-the-backups"></a>Bir veritabanı bir autoprotected örneğinden silerseniz, yedekleri gerçekleştirilecek?
Bir veritabanı bir autoprotected örneğinden kesilirse, veritabanı yedeklemeleri hala denenir. Bu, silinen veritabanını altında sağlıksız görünmesini başlar gelir **yedekleme öğeleri** ve yine de korunur.

Bu veritabanını korumayı durdurmak için doğru şekilde yapmaktır **yedeklemeyi Durdur** ile **verilerini Sil** bu veritabanı.  

## <a name="if-i-do-stop-backup-operation-of-an-autoprotected-database-what-will-be-its-behavior"></a>Yedekleme işlemi autoprotected veritabanı durdurursanız ne davranışını olacaktır?
Bunu yaparsanız **verileri içeren yedeklemeyi durdurma korumak**, bundan sonraki yedeklemeler gerçekleşir ve mevcut kurtarma noktalarını değişmeden kalır. Veritabanı hala korumalı olarak değerlendirilir ve altında gösterilen **yedekleme öğeleri**.

Bunu yaparsanız **verileri içeren yedeklemeyi durdurma**, bundan sonraki yedeklemeler gerçekleşir ve mevcut kurtarma noktalarını da silinecek. Veritabanı olarak kabul edilecek beklemediğiniz korumalı ve yedeklemeyi Yapılandır örneğinde altında gösterilir. Ancak, el ile seçilebilir veya autoprotected elde edebilirsiniz diğer yukarı korumalı veritabanı, aksine bu veritabanı gri renkte görünür ve seçilemez. Bu veritabanını yeniden korumak için tek bir örneğinde otomatik korumayı devre dışı bırakmak için yoludur. Artık bu veritabanını seçin ve da koruma yapılandırın veya yeniden örneğinde otomatik korumayı yeniden etkinleştirin.

## <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-be-the-behavior"></a>Koruma uygulandıktan sonra veritabanının adı değiştirebilir, ne davranışı olacaktır?
Yeniden adlandırılmış bir veritabanını yeni bir veritabanı olarak kabul edilir. Bu nedenle, hizmet veritabanı bulunamadı ve yedeklemelerin başarısız gibi bu durum değerlendirir.

Şimdi yeniden adlandırıldı ve koruma üzerinde yapılandırma veritabanını seçebilirsiniz. Örnek üzerinde otomatik koruma etkin durumda değiştirilen veritabanını otomatik olarak algılanan korumalı ve başlatılır.

##  <a name="why-cant-i-see-an-added-database-for-an-autoprotected-instance"></a>Eklenen bir veritabanı autoprotected örneği için neden göremiyorum?
Bir veritabanı [autoprotected örneğine ekleme](backup-sql-server-database-azure-vms.md#enable-auto-protection) hemen altındaki korumalı öğelerin görünmeyebilir. Bulma, genellikle her 8 saatte bir çalışır olmasıdır. Ancak, bulmak ve seçerek el ile bir bulma çalıştırırsanız yeni veritabanlarını hemen korumak **veritabanlarını kurtarmak**, aşağıdaki görüntüde gösterildiği gibi.

  ![El ile yeni eklenen bir veritabanı keşfedin](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [SQL Server veritabanı yedekleme](backup-azure-sql-database.md) bir Azure sanal makinesinde çalışan.
