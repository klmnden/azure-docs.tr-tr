---
title: Azure Backup ile Azure vm'lerde SQL Server veritabanlarını yedekleme hakkında sık sorulan sorular
description: Azure Backup ile Azure vm'lerde SQL Server veritabanlarını yedekleme hakkında sık sorulan sorulara yanıtlar bulun.
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: sachdevaswati
ms.openlocfilehash: 3d02c8b3d763e3bd767964c8056fa948bfbb13d1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57996053"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Bir Azure VM yedeklemesi üzerinde çalışan SQL Server veritabanları hakkında SSS

Bu makalede, Azure sanal makinelerinde (VM) çalıştıran ve kullanan SQL Server veritabanlarını yedekleme hakkında sık sorulan sorular yanıtlanmaktadır [Azure Backup](backup-overview.md) hizmeti.

> [!NOTE]
> Bu özellik şu anda genel Önizleme aşamasındadır.

## <a name="can-i-control-as-to-how-many-concurrent-backups-run-on-the-sql-server"></a>SQL server üzerinde kaç tane eş zamanlı yedeklemeleri çalıştırma için farklı denetleyebilir miyim?

Evet. Yedekleme İlkesi bir SQL Server örneği üzerindeki etkiyi en aza indirmek için çalıştığı oranı kısıtlayabilirsiniz. Bu ayarı değiştirmek için:
1. SQL Server örneğinde, *C:\Program Files\Azure iş yükü Backup\bin* klasör oluşturma *ExtensionSettingsOverrides.json* dosya.
2. İçinde *ExtensionSettingsOverrides.json* dosya, değişiklik **DefaultBackupTasksThreshold** ayarını daha düşük bir değere (örneğin, 5). <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Yaptığınız değişiklikleri kaydedin ve dosyayı kapatın.
4. SQL Server örneğinde açın **Görev Yöneticisi'ni**. Yeniden **AzureWLBackupCoordinatorSvc** hizmeti.

> [!NOTE]
> Yine de devam edin ve herhangi bir zamanda kadar yedeklemelerin UX ile ancak göründükleri varsayalım, 5, yukarıdaki örnekte, bir kayan pencereye işlenebilir.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Bir ikincil çoğaltma tam yedekleme çalıştırabilir miyim?
SQL sınırlamaları göre ikincil kopyada kopyalama yalnızca tam yedekleme çalıştırabilirsiniz; Ancak, tam yedeklemeye izin verilmez.

## <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri uyarılar oluşturulur?
Hayır. Başarılı yedekleme işleri uyarıları oluşturma. Uyarılar, başarısız olan yedekleme işleri için gönderilir.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>Zamanlanan yedekleme işlerinin işleri menüsünde görebilir miyim?
**Yedekleme işi** menü geçici yalnızca yedekleme işleri gösterilir. Zamanlanmış iş için kullanmak [Azure İzleyicisi'ni kullanarak izleme](backup-azure-monitoring-use-azuremonitor.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Gelecekteki veritabanları için Yedekleme otomatik olarak eklenir?
Evet, ile [otomatik koruma](backup-azure-sql-database.md#enable-auto-protection)

## <a name="can-i-protect-availability-groups-on-premises"></a>Kullanılabilirlik grupları şirket içi koruyabilirim?
Hayır. Azure yedekleme, Azure'da çalışan SQL Server veritabanlarını korur. Bir kullanılabilirlik grubu (ağ), Azure ve şirket içi makineler arasında yayılır, yalnızca birincil çoğaltma Azure'da çalışıyorsa AG korunabilir. Ayrıca, Azure Backup kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde çalışan düğümleri korur.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kullanılabilirlik grupları bölgeler arasında koruyabilir miyim?
Azure Backup kurtarma Hizmetleri kasası algılayabilir ve kasa ile aynı bölgede bulunan tüm düğümleri koruyun. Birincil düğüm olan bölge yedekleme, SQL Server Always On kullanılabilirlik grubu birden çok Azure bölgesine yayılmış durumdaysa ayarlayın. Azure Backup, algılamak ve yedekleme tercihinize göre kullanılabilirlik grubundaki tüm veritabanlarını korumak. Yedekleme tercihinizi uyulmazsa, yedeklemeleri başarısız olur ve hata uyarısı alın.

## <a name="can-i-exclude-databases-with-autoprotection-enabled"></a>Veritabanları ile etkin autoprotection tutabilir miyim?
Hayır. Autoprotection [tüm örneğine uygular](backup-azure-sql-database.md#enable-auto-protection). Autoprotection seçerek bir örneğindeki veritabanlarını korumak için kullanamazsınız.

## <a name="can-i-have-different-policies-in-an-autoprotected-instance"></a>Farklı ilkeler autoprotected örneği olabilir mi?
Örneğiniz korumalı bazı veritabanları içeriyorsa, bunlar altında ilgili ilkelerini sonra bile, korunacak devam edeceğiz [üzerinde autoprotection kapatma](backup-azure-sql-database.md#enable-auto-protection). Ancak, tüm korumasız veritabanları ve daha sonra eklediğiniz veritabanları yalnızca tek bir ilke gerekir. Bu ilke altında tanımladığınız **yedeklemeyi Yapılandır** veritabanlarını seçin sonra. Aslında, korunan diğer veritabanlarının, ilke autoprotected örneğinde bir veritabanı için bile değiştiremezsiniz.
Bu veritabanının ilkesini değiştirmek için tek yolu, geçici olarak autoprotection örneği için'devre dışı bırakmaktır. Ardından autoprotection örneği için yeniden etkinleştirin.

## <a name="if-i-delete-a-database-from-an-autoprotected-instance-will-backups-stop"></a>Bir veritabanı bir autoprotected örneğinden silerseniz, yedeklemeler durdurur mu?
Hayır. Bir veritabanı bir autoprotected örneğinden kesilirse, veritabanı yedeklemeleri hala denenir. Bu, silinen veritabanını altında sağlıksız görünmesini başlar gelir **yedekleme öğeleri** ve yine de korunur.

Geçici olarak bu veritabanını korumayı durdurmanın tek yolu olduğundan [autoprotection devre dışı](backup-azure-sql-database.md#enable-auto-protection) örneğinde. Ardından, altında **yedekleme öğeleri** veritabanını seçin **yedeklemeyi Durdur**. Bu örneğin autoprotection daha sonra yeniden etkinleştirin.

##  <a name="why-cant-i-see-an-added-database-for-an-autoprotected-instance"></a>Eklenen bir veritabanı autoprotected örneği için neden göremiyorum?
Bir veritabanı [autoprotected örneğine ekleme](backup-azure-sql-database.md#enable-auto-protection) hemen altındaki korumalı öğelerin görünmeyebilir. Bulma, genellikle her 8 saatte bir çalışır olmasıdır. Ancak, bulmak ve seçerek el ile bir bulma çalıştırırsanız yeni veritabanlarını hemen korumak **veritabanlarını kurtarmak**, aşağıdaki görüntüde gösterildiği gibi.

  ![El ile yeni eklenen bir veritabanı keşfedin](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [SQL Server veritabanı yedekleme](backup-azure-sql-database.md) bir Azure sanal makinesinde çalışan.
