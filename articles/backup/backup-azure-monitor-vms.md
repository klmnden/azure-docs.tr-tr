---
title: Azure sanal makineler için yedekleme uyarıları izleme
description: Olayları ve Azure sanal makine yedekleme işleri uyarıları izleyin. Uyarılar temelinde e-posta gönderin.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: markgal
ms.openlocfilehash: 3783014738ec4e8f185531773b1259dc63e7f49f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606316"
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Azure sanal makine yedekleme uyarılarını izleme
Uyarıları olay eşiği karşıladığında veya aşılan hizmetinden yanıtları değildir. Bilerek zaman sorunları başlangıç iş maliyetleri tutarak için kritik olabilir. Uyarıları genellikle bir zamanlamaya göre gerçekleşmez ve bu nedenle uyarılar ortaya sonra mümkün olan en kısa sürede bilmeniz yararlı olur. Örneğin, bir yedekleme veya geri yükleme işi başarısız olduğunda bir uyarı hata beş dakika içinde gerçekleşir. Kasa panosunda, yedekleme uyarıları kutucuğu kritik ve uyarı düzeyi olayları görüntüler. Yedekleme uyarıları ayarlarında tüm olayları görüntüleyebilirsiniz. Ancak ayrı bir sorunu çalışırken bir uyarı ortaya çıkarsa ne yapacaksınız? Uyarı gerçekleştiğinde bunu bilmiyorsanız, küçük bir sorundan dolayı olabilir veya veri tehlikeye atabilecek. Doğru kişilerin oluştuğunda uyarının - farkında olduğundan emin olmak için e-posta üzerinden uyarı bildirimleri göndermek üzere hizmetini yapılandırın. E-posta bildirimlerini ayarlama hakkında daha fazla bilgi için bkz: [bildirimleri yapılandırmak](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Uyarılar hakkında bilgileri nasıl bulabilirim?
Bir uyarı oluşturan olay hakkında bilgi görüntülemek için yedekleme uyarıları bölümünde açmanız gerekir. Yedekleme uyarıları bölümünde açmak için iki yolu vardır: yedekleme uyarılardan ya da kasa panosunda veya uyarı ve olayları bölümünden bölmesi.

Yedekleme uyarıları kutucuğunda yedekleme uyarıları dikey penceresini açmak için:

* Üzerinde **yedekleme uyarıları** döşeme kasa Panosu üzerinde tıklatın **kritik** veya **uyarı** bu önem düzeyi için çalışma olaylarını görüntülemek için.

    ![Yedekleme uyarıları kutucuğu](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

Uyarı ve olayları bölümünden yedekleme uyarıları dikey penceresini açmak için:

1. Kasa panodan tıklatın **tüm ayarları**. ![Tüm ayarlar düğmesi](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Üzerinde **ayarları** dikey penceresinde tıklatın **uyarı ve olayları**. ![Uyarı ve olayları düğmesi](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Üzerinde **uyarı ve olayları** dikey penceresinde tıklatın **yedekleme uyarıları**. ![Yedekleme uyarıları düğmesi](./media/backup-azure-monitor-vms/backup-alerts.png)

    **Yedekleme uyarıları** bölümü açılır ve filtrelenmiş uyarıları görüntüler.

    ![Yedekleme uyarıları kutucuğu](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. Olaylar, listesinden belirli bir uyarı hakkında ayrıntılı bilgi görüntülemek için açmak için uyarıyı tıklatın, **ayrıntıları** bölümü.

    ![Olay Ayrıntıları](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Liste görünümünde görüntülenen nitelikleri özelleştirmek için bkz: [ek olay öznitelikleri görüntüleyin](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Bildirimleri yapılandırma
 Oluştu veya son bir saat içinde belirli türlerdeki olayların ortaya çıktığında uyarılar için e-posta bildirimleri göndermek üzere hizmetini yapılandırabilirsiniz.

Uyarılar için e-posta bildirimleri ayarlamak için

1. Yedekleme uyarıları menüsünde **bildirimleri yapılandırma**

    ![Yedekleme uyarıları menüsü](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Yapılandırma bildirimler bölümü açılır.

    ![Bildirimleri dikey yapılandırın](./media/backup-azure-monitor-vms/configure-notifications.png)
2. E-posta bildirimleri için yapılandırma bildirimleri bölümünde tıklatın **üzerinde**.

    Alıcıları ve önem derecesi iletişim kutuları, bu bilgi gereklidir çünkü bunları yanında bir yıldız sahip. En az bir e-posta adresi sağlayın ve en az bir önem derecesini seçin.
3. İçinde **alıcılar (e-posta)** iletişim kutusunda, kimin bildirimleri almak için e-posta adreslerini yazın. Biçimi kullanın: username@domainname.com. Birden çok e-posta adresini noktalı virgül (;) ayırın.
4. İçinde **bildirim** alanı seçin **başına uyarı** belirtilen uyarı oluştuğunda, bildirim göndermek için veya **saatlik Özet** son bir saat için bir Özet göndermek için.
5. İçinde **önem** iletişim kutusunda, e-posta bildirimlerini tetiklemesini istediğiniz bir veya daha fazla düzeyleri seçin.
6. **Kaydet**’e tıklayın.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Hangi uyarı türleri için Azure Iaas sanal yedekleme var mı?
   | Uyarı düzeyi | Gönderilen uyarıları |
   | --- | --- |
   | Kritik | Kurtarma hatası yedekleme hatası için |
   | Uyarı | uyarılarla başarılı bir şekilde yedekleme işleri için (örneğin: bazı yazıcıları başarısız bir anlık görüntü oluşturulurken) |
   | Bilgilendirici | şu anda Azure VM yedekleme için hiçbir bilgilendirici uyarılar bulunmaktadır |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Bildirimler yapılandırılmış olsa bile e-postanın gönderilmediği durumlar var mı?
Bildirimlerin düzgün bir şekilde yapılandırmış olmanıza rağmen bir uyarı gönderilmez, durumlar vardır. Aşağıdaki durumlarda e-postayla aralığını belirterek uyarı sesini önlemek için bildirim gönderilmez:

* Bildirimler için saatlik Özet yapılandırılır ve bir uyarı oluşturulur ve aynı saat içinde çözümlenen istiyorsanız.
* İşleri iptal edilir.
* Bir yedekleme işi tetiklenir ve sonra başarısız olur ve başka bir yedekleme işi sürüyor.
* Resource Manager etkin bir VM için zamanlanmış bir yedekleme işini başlatır, ancak VM artık yok.

## <a name="using-activity-logs-to-get-notifications-for-successful-backups"></a>Başarılı yedeklemeler için bildirimleri almak için etkinlik günlüklerini kullanma

Yedeklemeler başarılı olduktan sonra bildirim almak istiyorsanız, üzerinde oluşturulan uyarılar kullanabilirsiniz [etkinlik günlükleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) kasasının.

### <a name="login-into-azure-portal"></a>Azure portalında oturum aç
Azure portalında oturum aç ve ilgili Azure kurtarma Hizmetleri Kasası'na devam etmek ve Özellikler "Etkinlik günlüğü" bölümünde'i tıklatın.

### <a name="identify-appropriate-log"></a>Uygun günlük tanımlayın

Başarılı yedeklemeler için etkinlik günlüklerini alma doğrulamak için aşağıdaki resimde gösterilen filtre uygulayın. Timespan kayıtları görüntülemek için uygun şekilde değiştirin.

![Etkinlik Günlükleri](./media/backup-azure-monitor-vms/activity-logs-identify.png)

Daha fazla bilgi almak ve kopyalama, bir metin düzenleyicisi yapıştıran göre görüntülemek için "JSON" segment tıklatabilirsiniz. Kasa ayrıntıları ve etkinlik günlüğü başka bir deyişle, yedekleme öğesi tetiklemiş öğesi görüntülemelidir.

"Etkinlik günlüğü uyarı Ekle" ı tüm günlükler için uyarıları oluşturmak için.

### <a name="add-activity-log-alert"></a>Etkinlik günlüğü uyarısı ekleme

"Etkinlik günlüğü uyarı Ekle" a tıklayarak, bir ekran aşağıda gösterildiği gibi gösterir

![Etkinlik günlüğü uyarısı](./media/backup-azure-monitor-vms/activity-logs-alerts-successful.png)
    
Abonelik ve kaynak grubu, uyarıyı depolamak için kullanılır. Ölçüt önceden doldurulur. Tüm değerleri gereksinim için uygun olduğundan emin olun.

Başarılı yedeklemeler için 'Düzeyi' "Bilgi" ve durumu "Başarılı" olarak işaretlenir.

"Kaynak" Yukarıdaki seçerseniz, bu kaynak veya kasa için etkinlik günlükleri kaydedildiğinde uyarı oluşturulur. Kuralın tüm kasa için geçerli olmasını istiyorsanız, "boş olmasını kaynak" bırakın.

### <a name="define-action-on-alert-firing"></a>Üzerinde uyarı tetikleme eylemi tanımlayın

"Eylem grubu" kullanan bir uyarı oluşturmadan bağlı eylemi tanımlamak için. "Eylem"tür tıklayabilirsiniz kullanılabilir eylemler hakkında daha fazla ile ITSM vb. gibi e-posta/SMS/tümleştirme bilmek.

![Etkinlik günlüğü eylem grubu](./media/backup-azure-monitor-vms/activity-logs-alerts-action-group.png)


Tamam'a tıkladıktan sonra bir etkinlik günlüğü uyarı oluşturulur ve sonraki etkinlik günlükleri başarılı yedeklemeler için kaydedilen eylem eylem grubunda tanımlanan ateşlenir.

### <a name="limitations-on-alerts"></a>Uyarıları sınırlamalar
Olay tabanlı uyarılara tabi aşağıdaki sınırlamalar vardır:

1. Kurtarma Hizmetleri Kasası'nda tüm sanal makinelerde uyarıları tetiklenir. Sanal makineler bir kurtarma Hizmetleri kasasına bir kısmı için uyarı özelleştiremezsiniz.
2. Uyarılar gönderilen "alerts-noreply@mail.windowsazure.com". Şu anda e-posta gönderen değiştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bir sanal makine bir kurtarma noktasından yeniden oluşturma hakkında daha fazla bilgi için kullanıma [geri Azure Vm'leri](backup-azure-arm-restore-vms.md).

Sanal makinelerinizi koruma bilgi gerekirse bkz [ilk bakış: Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-vms-first-look-arm.md). 

Makalede VM yedeklemeler için yönetim görevleri hakkında daha fazla bilgi [yönetmek Azure sanal makine yedeklerini](backup-azure-manage-vms.md).
