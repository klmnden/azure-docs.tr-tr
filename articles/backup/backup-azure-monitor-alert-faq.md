---
title: Azure Backup izleme uyarı ile ilgili SSS
description: 'Hakkında sık sorulan sorulara yanıtlar: Azure Backup izleme Uyarısı'
services: backup
author: srinathvasireddy
manager: sivan
ms.service: backup
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: srinathv
ms.openlocfilehash: bb684f65539b4429862b2dce0e378d8f659d2975
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705039"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Azure Backup izleme uyarısı - SSS
Bu makalede, Azure izleme uyarı hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>Raporlama verileri bir depolama hesabına akar başlatılmış olup olmadığını nasıl denetlerim?
Yapılandırdığınız depolama hesabına gidin ve kapsayıcılar'ı seçin. Kapsayıcı öngörüleri günlükleri azurebackupreport için bir girdi varsa, veri raporlama akan başlatıldığını gösterir.

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>Bir depolama hesabı ve Azure Backup içerik Paketi'ne Power bı'daki veri gönderme sıklığı nedir?
  0\. gün kullanıcılar için bunu bir depolama hesabına veri göndermeye ilişkin yaklaşık 24 saat sürer. Bu ilk gönderme tamamlandıktan sonra verileri aşağıdaki şekilde gösterilen sıklıkta yenilenir.

  * İlgili verileri **işleri**, **uyarılar**, **yedekleme öğeleri**, **kasaları**, **korumalı sunucuların**ve  **İlkeleri** gibi ve günlüğe bir müşterinin depolama hesabına gönderilir.

  * İlgili verileri **depolama** 24 saatte bir müşterinin depolama hesabına gönderilir.

       ![Azure Backup raporları veri gönderme sıklığı](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  * Power bı'da bir [zamanlanmış yenileme günde bir kez](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Power BI'da İçerik Paketi için verileri el ile yenileme gerçekleştirebilirsiniz.

### <a name="how-long-can-i-retain-reports"></a>Raporlar ne kadar süreyle tutabilir miyim?
Bir depolama hesabı yapılandırmak, depolama hesabında bir rapor verileri saklama süresini seçebilirsiniz. İzleme adımı 6 [raporlar için depolama hesabı yapılandırma](backup-azure-configure-reports.md#configure-storage-account-for-reports) bölümü. Ayrıca [raporları Excel'de Çözümle](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) ve bunları ihtiyaçlarınıza göre daha uzun bir bekletme dönemi için kaydedin.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>Raporlardaki tüm Verilerimin, depolama hesabı yapılandırabilirim sonra görür müyüm?
 Bir depolama hesabı yapılandırdıktan sonra oluşturulan tüm veriler depolama hesabına gönderilir ve raporlarında kullanılabilir. Raporlama için devam eden işleri gönderilmez. İşin tamamlanana veya başarısız olduktan sonra Bu raporlar için gönderilir.

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>Depolama hesabı, raporları görüntülemek için yapılandırılmış, başka bir depolama hesabı kullanmak üzere yapılandırma değiştirebilirim?
Evet, farklı bir depolama hesabına işaret edecek şekilde yapılandırmasını değiştirebilirsiniz. Azure Backup içerik Paketi'ne bağlanmak yeni yapılandırılan depolama hesabı kullanın. Ayrıca, farklı bir depolama hesabı yapılandırdıktan sonra yeni veriler bu depolama hesabına akar. (Yapılandırmayı değiştirmemeden önce) daha eski verilere hala eski depolama hesabında kalır.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Raporları kasaları ve Aboneliklerde görüntüleyebilir miyim?
Evet, kasa arası raporları görüntülemek için çeşitli kasaları aynı depolama hesabı yapılandırabilirsiniz. Ayrıca, abonelikler arasında kasaları için aynı depolama hesabı yapılandırabilirsiniz. Power bı'da raporları görüntülemek için Azure Backup içerik Paketi'ne bağlanırken, bu depolama hesabı kullanabilirsiniz. Seçilen depolama hesabına kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Azure Yedekleme aracısı iş durumu Portalı'nda yansıtacak şekilde ne kadar sürüyor?
Azure portalı, Azure Yedekleme aracısı iş durumunu yansıtacak şekilde 15 dakika kadar sürebilir.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Bir yedekleme işi başarısız olduğunda ne kadar uyarı sürer?
Azure yedekleme hata 20 dakika içinde bir uyarı oluşturulur.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Bildirimler yapılandırıldıysa bir e-posta burada gönderilmez bir servis talebi var mı?
Evet. Aşağıdaki durumlarda bildirimleri gönderilmez.

* Bildirimleri saatlik yapılandırıldıysa ve bir uyarı oluşturulur ve aynı saat içinde çözülen
* Ne zaman bir işi iptal edildi
* İlk yedekleme işi devam ettiğinden, ikinci bir yedekleme işi başarısız olursa

## <a name="recovery-services-vault"></a>Kurtarma Hizmetleri kasası

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Azure Yedekleme aracısı iş durumu Portalı'nda yansıtacak şekilde ne kadar sürüyor?
Azure portalı, Azure Yedekleme aracısı iş durumunu yansıtacak şekilde 15 dakika kadar sürebilir.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Bir yedekleme işi başarısız olduğunda ne kadar uyarı sürer?
Azure yedekleme hata 20 dakika içinde bir uyarı oluşturulur.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Bildirimler yapılandırıldıysa bir e-posta burada gönderilmez bir servis talebi var mı?
Evet. Aşağıdaki durumlarda bildirimleri gönderilmez.

* Bildirimleri saatlik yapılandırıldıysa ve bir uyarı oluşturulur ve aynı saat içinde çözülen
* Ne zaman bir işi iptal edildi
* İlk yedekleme işi devam ettiğinden, ikinci bir yedekleme işi başarısız olursa

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS'leri okuyun:

- [Sık sorulan sorular](backup-azure-vm-backup-faq.md) Azure VM yedeklemeleri hakkında.
- [Sık sorulan sorular](backup-azure-file-folder-backup-faq.md) Azure Backup Aracısı hakkında
