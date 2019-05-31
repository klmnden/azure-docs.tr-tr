---
title: Azure sanal makineler için yedekleme uyarıları izleme
description: Olayları ve Uyarıları Azure sanal makine yedekleme işleri izleyin. Uyarılara göre e-posta gönderin.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: 350e13a4b1c01329bef1ec270af5ba007cd788aa
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399743"
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Azure sanal makine yedekleme uyarılarını izleme

Bir olayı eşiği karşılanmalı veya aşılan hizmetinden alınan yanıtları uyarılardır. Bilerek zaman sorunları başlangıç iş maliyetleri düşük düzeyde tutarak için kritik olabilir. Uyarılar genellikle bir zamanlamaya göre gerçekleşmez ve bu nedenle uyarılar ortaya sonra hemen bilmek yararlı olur. Örneğin, bir yedekleme veya geri yükleme işi başarısız olduğunda bir uyarı hata beş dakika içinde gerçekleşir. Kasa panosunda, yedekleme uyarıları kutucuğuna kritik ve uyarı düzeyi olayları görüntüler. Yedekleme uyarıları Ayarları'nda, tüm olayları görüntüleyebilirsiniz. Ancak, ayrı bir sorunu üzerinde çalışırken, bir uyarı ortaya çıkarsa ne yapacaksınız? Uyarı gerçekleştiğinde bilmiyorsanız, küçük bir sorun nedeniyle olabilir veya verileri tehlikeye atabilecek. Oluştuğunda doğru kişilere bir uyarının - farkında olduğundan emin olmak için hizmetini e-posta üzerinden uyarı bildirimleri gönderecek şekilde yapılandırın. E-posta bildirimleri ayarlama hakkında daha fazla ayrıntı için bkz. [bildirimleri yapılandırma](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Uyarılarla ilgili bilgileri nasıl bulabilirim?

Bir uyarı oluşturdu olayla ilgili bilgileri görüntülemek için yedekleme Uyarılar bölümünde açmanız gerekir. Yedekleme Uyarılar bölümünde açmanın iki yolu vardır: yedekleme uyarılardan ya da kasa panosunda veya uyarıları ve olayları bölümünden bölmesi.

Yedekleme uyarıları kutucuğundan yedekleme uyarılar dikey penceresini açmak için:

* Üzerinde **yedekleme uyarıları** kasa panosunda'a tıklayın **kritik** veya **uyarı** işletimsel olayları bu önem düzeyi için görüntülenecek.

    ![Yedekleme uyarıları kutucuğu](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

Yedekleme uyarıları dikey penceresi uyarıları ve olayları bölümünden açmak için:

1. Kasa panosundan tıklayın **tüm ayarlar**. ![Tüm ayarları düğmesi](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Üzerinde **ayarları** dikey penceresinde tıklayın **uyarıları ve olayları**. ![Uyarıları ve olayları düğmesi](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Üzerinde **uyarıları ve olayları** dikey penceresinde tıklayın **yedekleme uyarıları**. ![Yedekleme uyarıları düğmesi](./media/backup-azure-monitor-vms/backup-alerts.png)

    **Yedekleme uyarıları** bölümü açılır ve filtrelenmiş uyarıları görüntüler.

    ![Yedekleme uyarıları kutucuğu](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. Olaylar listesinden belirli bir uyarı hakkında ayrıntılı bilgi görüntülemek için açmak için uyarıya tıklayın. kendi **ayrıntıları** bölümü.

    ![Olay Ayrıntıları](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Listede gösterilen öznitelikleri özelleştirmek için bkz [ek olay öznitelikleri görüntüleyin](backup-azure-monitor-vms.md)

## <a name="configure-notifications"></a>Bildirimleri yapılandırma

 Son bir saat boyunca veya belirli türde olaylar meydana geldiğinde gerçekleşen uyarılar için e-posta bildirimleri gönderecek şekilde hizmeti yapılandırabilirsiniz.

Uyarılar için e-posta bildirimlerini ayarlamak için

1. Yedekleme uyarıları menüsünde **bildirimleri yapılandırma**

    ![Yedekleme uyarıları menüsü](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Yapılandırma bildirimler bölümü açılır.

    ![Bildirimleri dikey penceresini Yapılandır](./media/backup-azure-monitor-vms/configure-notifications.png)
2. E-posta bildirimi yapılandırma bildirimler bölümüne tıklayarak **üzerinde**.

    Alıcılar ve önem derecesi iletişim kutuları, bu bilgi gereklidir çünkü yanında bir yıldız sahip. En az bir e-posta adresi sağlayın ve en az bir önem derecesini seçin.
3. İçinde **alıcılar (e-posta)** iletişim kutusunda, kimin bildirimleri almak için e-posta adreslerini yazın. Biçimini kullanın: username@domainname.com. Birden çok e-posta adreslerini noktalı virgül (;) ayırın.
4. İçinde **bildirim** alanında seçin **uyarı başına** belirtilen uyarı oluştuğunda, bildirim göndermek veya **saatlik Özet** son bir saat için bir Özet göndermek için.
5. İçinde **önem derecesi** iletişim e-posta bildirimi tetiklemek istediğiniz bir veya daha fazla düzeyleri seçin.
6. **Kaydet**’e tıklayın.

### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Azure Iaas VM yedeklemesi için hangi uyarı türleri kullanılabilir

   | Uyarı düzeyi | Gönderilen uyarılar |
   | --- | --- |
   | Kritik | Kurtarma hatası yedekleme hatası için |
   | Uyarı | şu anda hiçbir uyarı bildirimleri Azure VM yedeklemeleri için kullanılabilir (örneğin: bazı yazarları, bir anlık görüntü oluşturulurken başarısız oldu) |
   | Bilgilendirici | şu anda, Azure VM yedeklemesi için hiçbir bilgilendirici uyarılar kullanılabilir |

### <a name="situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Bildirimler yapılandırılmış olsa bile e-postanın gönderilmediği durumlar

Bildirimlerin düzgün yapılandırılmış olsa da burada bir uyarı gönderilmez, durumlar vardır. Aşağıdaki durumlarda e-postayla uyarı gürültüsünü önlemek için bildirimleri gönderilmez:

* Bildirimler, saatlik Özet için yapılandırılır ve bir uyarı oluşturulur ve aynı saat içinde çözülen durumunda.
* İş iptal edildi.
* Bir yedekleme işi tetiklenir ve sonra başarısız oluyor ve başka bir yedekleme işi devam ediyor.
* Resource Manager özellikli bir VM için zamanlanmış bir yedekleme işi başlatır, ancak VM artık yok.

## <a name="using-activity-logs-to-get-notifications-for-successful-backups"></a>Başarılı yedeklemeler için bildirim almak için etkinlik günlüklerini kullanma

> [!NOTE]
> Azure Backup kurtarma Hizmetleri kasaları üzerinde etkinlik günlüklerinden Pompalama, yeni bir Modeli'ne taşıdık. Ne yazık ki bu etkinlik günlüklerini Azure bağımsız bulutlarda nesil etkilenmiş. Azure bağımsız bulut kullanıcıları oluşturulan/herhangi bir uyarıyı aşağıda belirtildiği gibi Azure İzleyici aracılığıyla etkinlik günlüklerinden yapılandırdıysanız, bunlar tetiklenmesi değil. Bu durumda, biz böyle kullanıcıların tanılama ayarları ve LA çalışma kullanmasını öneriyoruz veya [Powerbı çözüm raporlama](backup-azure-configure-reports.md) ilgili bilgileri almak için. Bir kullanıcının kurtarma Hizmetleri etkinlik günlükleri bir günlük analizi çalışma alanına belirtildiği gibi topladığını, ayrıca, tüm Azure bölgelerinde genel, [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity), bu günlükleri değil de görünür.

Yedeklemeler başarılı olduktan sonra size bildirilmesini istiyorsanız, üzerinde oluşturulan uyarılar kullanabileceğiniz [etkinlik günlüklerini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) kasasının.

### <a name="login-into-azure-portal"></a>Azure portalında oturum açın

Azure portalında oturum açın ve ilgili Azure kurtarma Hizmetleri Kasası'na devam ve Özellikler "Etkinlik günlüğü" bölümünde'ı tıklatın.

### <a name="identify-appropriate-log"></a>Uygun günlük tanımlayın

Başarılı yedeklemeler için etkinlik günlüklerini alma doğrulamak için aşağıdaki resimde gösterilen filtre uygulayın. Timespan kayıtları görüntülemek için uygun şekilde değiştirin.

![Etkinlik Günlükleri](./media/backup-azure-monitor-vms/activity-logs-identify.png)

Daha fazla bilgi almak ve kopyalama, bir metin düzenleyicisi yapıştırma göre görüntülemek için "JSON" segment tıklayabilirsiniz. Kasa ayrıntıları ve etkinlik günlüğü başka bir deyişle, yedekleme öğesi tetikleyen öğesi görüntülemelidir.

"Etkinlik günlüğü uyarısı Ekle" ı tüm günlükler için uyarılar oluşturulacak.

### <a name="add-activity-log-alert"></a>Etkinlik günlüğü uyarısı Ekle

"Etkinlik günlüğü uyarısı Ekle" tıklandığında, bir ekran aşağıda gösterildiği gibi gösterilir

![Etkinlik günlüğü Uyarısı](./media/backup-azure-monitor-vms/activity-logs-alerts-successful.png) abonelik ve kaynak grubu, uyarı depolamak için kullanılır. Ölçüt önceden doldurulur. Tüm değerleri, gereksinime uygun olduğundan emin olun.

Başarılı yedeklemeler için 'Düzeyi' "Bilgilendirici" ve durumu "Başarılı" olarak işaretlenir.

Bir "kaynak" Yukarıdaki seçerseniz, ilgili kaynağın veya kasa için etkinlik günlüklerini kaydedildiğinde uyarı oluşturulur. Kuralın tüm kasaları için geçerli olmasını istiyorsanız, "boş olacak şekilde kaynak" bırakın.

### <a name="define-action-on-alert-firing"></a>Uyarı Açmadığınızda üzerinde eylem tanımlayın

"Eylem grubu" kullanan bir uyarı üreten bağlı eylemi tanımlamak için. "Eylem türü üzerinde" tıklayabilirsiniz kullanılabilir eylemler hakkında daha fazla ile ITSM vb. gibi e-posta/SMS/tümleştirme bilmek.

![Etkinlik günlüğü eylem grubu](./media/backup-azure-monitor-vms/activity-logs-alerts-action-group.png)

Tamam'a tıkladığınızda, bir etkinlik günlüğü uyarısı oluşturulur ve eylem grubunda tanımlanan eylem başarılı yedeklemeler için kaydedilen bir sonraki etkinlik günlüklerini ateşlenir.

### <a name="limitations-on-alerts"></a>Uyarılar ilgili sınırlamalar

Olay tabanlı uyarılara aşağıdaki sınırlamalara tabi şunlardır:

1. Kurtarma Hizmetleri kasasındaki tüm sanal makinelerde uyarılar tetiklenir. Bir alt kümesi sanal makinelerinin bir kurtarma Hizmetleri kasası için uyarı özelleştiremezsiniz.
2. Uyarılar gönderilen "alerts-noreply@mail.windowsazure.com". Şu anda e-posta gönderen değiştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir kurtarma noktasından bir sanal makine yeniden oluşturma hakkında daha fazla bilgi için kullanıma [Azure Vm'leri geri yükleme](backup-azure-arm-restore-vms.md).

Sanal makinelerinizi koruma hakkında daha fazla bilgi gerekirse bkz [ilk bakış: Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-vms-first-look-arm.md).

Makalede, VM yedeklemeleri için yönetim görevleri hakkında daha fazla bilgi [yönetme Azure sanal makine yedeklemelerini](backup-azure-manage-vms.md).
