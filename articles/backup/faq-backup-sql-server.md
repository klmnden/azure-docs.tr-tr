---
title: Azure Backup ile Azure vm'lerde SQL Server veritabanlarını yedekleme hakkında sık sorulan sorular
description: Azure Backup ile Azure vm'lerde SQL Server veritabanlarını yedekleme hakkında sık sorulan soruların yanıtlarını sağlar.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: sogup
ms.openlocfilehash: a14406733ff60d53d4bf7792ff0c9a015c57d9b3
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430783"
---
# <a name="faq-on-sql-server-running-on-azure-vm-backup"></a>Azure VM yedeklemesi üzerinde çalışan SQL Server hakkında SSS

Bu makalede, Azure Vm'leri üzerinde çalışan SQL Server veritabanlarını yedeklemek hakkında sık sorulan sorular yanıtlanmaktadır [Azure Backup](backup-overview.md) hizmeti.

> [!NOTE]
> Bu özellik şu anda genel Önizleme aşamasındadır.



## <a name="can-i-throttle-the-backup-speed"></a>Ben yedekleme hızı kısıtlayabilir miyim?

Evet. Yedekleme İlkesi bir SQL Server örneği üzerindeki etkiyi en aza indirmek için çalıştığı oranı kısıtlayabilirsiniz. Bu ayarı değiştirmek için:
1. SQL Server örneğinde, *C:\Program Files\Azure iş yükü Backup\bin klasör*, oluşturma **ExtensionSettingsOverrides.json** dosya.
2. İçinde **ExtensionSettingsOverrides.json** dosya, değişiklik **DefaultBackupTasksThreshold** ayarını daha düşük bir değere (örneğin, 5) <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Yaptığınız değişiklikleri kaydedin. Dosyayı kapatın.
4. SQL Server örneğinde açın **Görev Yöneticisi'ni**. Yeniden **AzureWLBackupCoordinatorSvc** hizmeti.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Bir ikincil çoğaltma tam yedekleme çalıştırabilir miyim?
Hayır. Bu özellik desteklenmez.

## <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri uyarılar oluşturulur?

Hayır. Başarılı yedekleme işleri uyarıları oluşturma. Uyarılar, başarısız olan yedekleme işleri için gönderilir.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>Zamanlanan yedekleme işlerinin işleri menüsünde görebilir miyim?

Hayır. **Yedekleme işleri** menüsü, isteğe bağlı iş ayrıntıları, ancak değil zamanlanan yedekleme işlerinin gösterir. Zamanlanmış tüm yedekleme işleri başarısız olursa, başarısız işi uyarıları ayrıntıları kullanılabilir. Tüm izlemek için zamanlanan ve geçici yedekleme işlerini kullanma [SQL Server Management Studio](manage-monitor-sql-database-backup.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Gelecekteki veritabanları için Yedekleme otomatik olarak eklenir?

Hayır. Sunucu düzeyi seçeneği seçerseniz bir SQL Server örneği için korumayı yapılandırdığınızda, tüm veritabanları eklenir. Bir SQL Server örneğine veritabanları eklerseniz, koruma yapılandırdıktan sonra onları korumak için yeni veritabanlarını elle eklemeniz gerekir. Veritabanlarını yapılandırılmış korumayı otomatik olarak dahil edilmez.

##  <a name="how-do-i-restart-protection-after-changing-recovery-type"></a>Kurtarma türünü değiştirdikten sonra koruma nasıl yeniden?

Tam yedekleme tetikleyin. Günlük yedeklemeler beklendiği gibi başlayın.

## <a name="can-i-protect-availability-groups-on-premises"></a>Kullanılabilirlik grupları şirket içi koruyabilirim?

Hayır. Azure Backup, Azure'da çalışan SQL sunucuları korur. Bir kullanılabilirlik grubu (ağ), Azure ve şirket içi makineler arasında yayılır, yalnızca birincil çoğaltma Azure'da çalışıyorsa AG korunabilir. Ayrıca, Azure Backup yalnızca kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde çalışan düğümleri korur.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kullanılabilirlik grupları bölgeler arasında koruyabilir miyim?

Azure Backup kurtarma Hizmetleri kasası, algılayın ve kurtarma Hizmetleri kasasıyla aynı bölgede olan tüm düğümleri koruyun. Birden fazla Azure bölgesini kapsayan bir SQL her zaman üzerinde kullanılabilirlik grubu varsa, birincil düğüm olan bölgeden yedeklemeyi yapılandırmak gerekir. Azure yedekleme, algılamak ve yedekleme tercihi göre kullanılabilirlik grubundaki tüm veritabanlarını korumak mümkün olacaktır. Yedekleme tercihi karşılanmazsa, yedeklemeler başarısız olur ve hata uyarısı alırsınız.

## <a name="can-i-exclude-databases-with-auto-protection-enabled"></a>Otomatik korumanın etkin olan veritabanları tutabilir miyim?

Hayır, [otomatik korumayı](backup-azure-sql-database.md#enable-auto-protection) tüm örneğine uygular. Seçime bağlı olarak, otomatik koruma kullanarak bir örnek veritabanlarını koruyamaz.

## <a name="can-i-have-different-policies-in-an-auto-protected-instance"></a>Bir otomatik korumalı örnekte farklı ilkeler olabilir mi?

Korumalı bazı veritabanları bir örneğine zaten varsa bunlar da açıldıktan sonra ilgili ilkelerini ile korunacak devam eder **ON** [otomatik korumayı](backup-azure-sql-database.md#enable-auto-protection) seçeneği. Ancak gelecekte eklersiniz olanları yanı sıra tüm korumasız veritabanlarını altında tanımladığınız yalnızca tek bir ilke olacaktır **yedeklemeyi Yapılandır** veritabanlarını seçtikten sonra. Aslında, korunan diğer veritabanlarının, ilke için bir veritabanı örneği otomatik korumalı altında bile değiştiremezsiniz.
Bunu yapmak istiyorsanız, tek şimdilik örneği otomatik korumasını devre dışı ve ardından bu veritabanı için ilkeyi değiştirmek için yoludur. Bu örnek için otomatik korumayı yeniden etkinleştirebilirsiniz.

## <a name="if-i-delete-a-database-from-auto-protection-will-backups-stop"></a>Bir veritabanı otomatik koruması silerseniz yedeklemeleri durdurur mu?

Hayır, bir veritabanı otomatik korumalı örneği kesilirse, veritabanı yedekleri hala denenir. Bu, silinen veritabanını altında sağlıksız görünmesini başlar gelir **yedekleme öğeleri** ve hala korumalı olarak kabul edilir.

Devre dışı bırakmak için bu veritabanını korumayı durdurmanın tek yolu olduğundan [otomatik korumayı](backup-azure-sql-database.md#enable-auto-protection) şimdilik örneğinde seçip **yedeklemeyi Durdur** altındaki **yedekleme öğeleri**bu veritabanı için. Bu örnek için otomatik korumayı yeniden etkinleştirebilirsiniz.

##  <a name="why-cant-i-see-an-added-database-for-an-auto-protected-instance"></a>Otomatik korumalı bir örnek için eklenen bir veritabanı neden göremiyorum?

Yeni eklenen bir veritabanına göremeyebilirsiniz bir [otomatik korumalı](backup-azure-sql-database.md#enable-auto-protection) hemen altındaki korumalı örnek, korumalı öğeler. Bulma, genellikle her 8 saatte bir çalışır olmasıdır. Ancak, kullanıcı kullanarak el ile keşif çalıştırabilirsiniz **veritabanlarını kurtarmak** seçeneği bulmak ve yeni korumak için veritabanları hemen gösterildiği gibi aşağıdaki görüntüde:

  ![Yeni eklenen veritabanınızı görüntüleyin](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi edinmek için nasıl](backup-azure-sql-database.md) bir Azure sanal makinesinde çalışan SQL Server veritabanını ayarlama.
