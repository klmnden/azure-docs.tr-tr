---
title: "İzleyici Resource Manager tarafından dağıtılan sanal makine yedeklerini | Microsoft Docs"
description: "Olayları ve Resource Manager tarafından dağıtılan sanal makine yedeklerini uyarıları izleyin. Uyarılar temelinde e-posta gönderin."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: fed32015-2db2-44f8-b204-d89f6fd1bea2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: markgal;trinadhk;giridham;
ms.openlocfilehash: 1e9f6d44965e8a6cd9529ef860f0fb57fd8e572d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Azure sanal makine yedekleme uyarılarını izleme
Uyarıları olay eşiği karşıladığında veya aşılan hizmetinden yanıtları değildir. Bilerek zaman sorunları başlangıç iş maliyetleri tutarak için kritik olabilir. Uyarıları genellikle bir zamanlamaya göre gerçekleşmez ve bu nedenle uyarılar ortaya sonra mümkün olan en kısa sürede bilmeniz yararlı olur. Örneğin, bir yedekleme veya geri yükleme işi başarısız olduğunda bir uyarı hata beş dakika içinde gerçekleşir. Kasa panosunda, yedekleme uyarıları kutucuğu kritik ve uyarı düzeyi olayları görüntüler. Yedekleme uyarıları ayarlarında tüm olayları görüntüleyebilirsiniz. Ancak ayrı bir sorunu çalışırken bir uyarı ortaya çıkarsa ne yapacaksınız? Uyarı gerçekleştiğinde bunu bilmiyorsanız, küçük bir sorundan dolayı olabilir veya veri tehlikeye atabilecek. Doğru kişilerin oluştuğunda uyarının - farkında olduğundan emin olmak için e-posta üzerinden uyarı bildirimleri göndermek üzere hizmetini yapılandırın. E-posta bildirimlerini ayarlama hakkında daha fazla bilgi için bkz: [bildirimleri yapılandırmak](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Uyarılar hakkında bilgileri nasıl bulabilirim?
Bir uyarı oluşturan olay hakkında bilgi görüntülemek için yedekleme uyarıları dikey penceresi açmanız gerekir. Yedekleme uyarıları dikey penceresini açmak için iki yolu vardır: yedekleme uyarılardan ya da kasa panosunda veya uyarı ve olayları dikey penceresinden bölmesi.

Yedekleme uyarıları kutucuğunda yedekleme uyarıları dikey penceresini açmak için:

* Üzerinde **yedekleme uyarıları** döşeme kasa Panosu üzerinde tıklatın **kritik** veya **uyarı** bu önem düzeyi için çalışma olaylarını görüntülemek için.

    ![Yedekleme uyarıları kutucuğu](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

Uyarı ve olayları dikey penceresinden yedekleme uyarıları dikey penceresini açmak için:

1. Kasa panodan tıklatın **tüm ayarları**. ![Tüm ayarlar düğmesi](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Üzerinde **ayarları** dikey penceresinde tıklatın **uyarı ve olayları**. ![Uyarı ve olayları düğmesi](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Üzerinde **uyarı ve olayları** dikey penceresinde tıklatın **yedekleme uyarıları**. ![Yedekleme uyarıları düğmesi](./media/backup-azure-monitor-vms/backup-alerts.png)

    **Yedekleme uyarıları** dikey penceresi açılır ve filtrelenmiş uyarıları görüntüler.

    ![Yedekleme uyarıları kutucuğu](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. Olaylar, listesinden belirli bir uyarı hakkında ayrıntılı bilgi görüntülemek için açmak için uyarıyı tıklatın, **ayrıntıları** dikey.

    ![Olay Ayrıntıları](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Liste görünümünde görüntülenen nitelikleri özelleştirmek için bkz: [ek olay öznitelikleri görüntüleyin](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Bildirimleri yapılandırma
 Oluştu veya son bir saat içinde belirli türlerdeki olayların ortaya çıktığında uyarılar için e-posta bildirimleri göndermek üzere hizmetini yapılandırabilirsiniz.

Uyarılar için e-posta bildirimleri ayarlamak için

1. Yedekleme uyarıları menüsünde **bildirimleri yapılandırma**

    ![Yedekleme uyarıları menüsü](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Yapılandırma bildirimleri dikey pencere açılır.

    ![Bildirimleri dikey yapılandırın](./media/backup-azure-monitor-vms/configure-notifications.png)
2. Yapılandırma bildirimleri dikey penceresinde, e-posta bildirimlerinin tıklatın **üzerinde**.

    Alıcıları ve önem derecesi iletişim kutuları, bu bilgi gereklidir çünkü bunları yanında bir yıldız sahip. En az bir e-posta adresi sağlayın ve en az bir önem derecesini seçin.
3. İçinde **alıcılar (e-posta)** iletişim kutusunda, kimin bildirimleri almak için e-posta adreslerini yazın. Biçimi kullanın: username@domainname.com. Birden çok e-posta adresini noktalı virgül (;) ayırın.
4. İçinde **bildirim** alanı seçin **başına uyarı** belirtilen uyarı oluştuğunda, bildirim göndermek için veya **saatlik Özet** son bir saat için bir Özet göndermek için.
5. İçinde **önem** iletişim kutusunda, e-posta bildirimlerini tetiklemesini istediğiniz bir veya daha fazla düzeyleri seçin.
6. **Kaydet**’e tıklayın.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Hangi uyarı türleri için Azure Iaas sanal yedekleme var mı?
   | Uyarı düzeyi | Gönderilen uyarıları |
   | --- | --- |
   | Kritik | Kurtarma hatası yedekleme hatası için |
   | Uyarı | uyarılarla başarılı bir şekilde yedekleme işleri için (örn: bazı yazıcıları başarısız bir anlık görüntü oluşturulurken) |
   | Bilgilendirici | şu anda Azure VM yedekleme için hiçbir bilgilendirici uyarılar bulunmaktadır |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Bildirimler yapılandırılmış olsa bile e-postanın gönderilmediği durumlar var mı?
Bildirimlerin düzgün bir şekilde yapılandırmış olmanıza rağmen bir uyarı gönderilmez, durumlar vardır. Aşağıdaki durumlarda e-postayla aralığını belirterek uyarı sesini önlemek için bildirim gönderilmez:

* Bildirimler için saatlik Özet yapılandırılır ve bir uyarı oluşturulur ve aynı saat içinde çözümlenen istiyorsanız.
* İşleri iptal edilir.
* Bir yedekleme işi tetiklenir ve sonra başarısız olur ve başka bir yedekleme işi sürüyor.
* Resource Manager etkin bir VM için zamanlanmış bir yedekleme işini başlatır, ancak VM artık yok.

## <a name="customize-your-view-of-events"></a>Olayları görünümünü özelleştirme
**Denetim günlüklerini** ayarı filtreleri ve işletimsel olay bilgilerini gösteren sütunları önceden tanımlanmış bir dizi birlikte gelir. Bu nedenle görünümünü özelleştirebilirsiniz olduğunda **olayları** dikey penceresi açıldığında, istediğiniz bilgileri gösterir.

1. İçinde [kasa Panosu](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard)bulun ve tıklatın **denetim günlüklerini** açmak için **olayları** dikey.

    ![Denetim Günlükleri](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    **Olayları** yalnızca geçerli kasa için filtre işletimsel olayları dikey pencere açılır.

    ![Denetim günlükleri filtresi](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    Dikey kritik, hata, uyarı ve geçen hafta içinde oluştu bilgilendirme olaylarını listesini gösterir. Zaman aralığı kümesinde varsayılan değerdir **filtre**. **Olayları** dikey penceresinde de olaylar meydana geldiğinde izleme bir çubuk grafik gösterir. Çubuk grafik görmek istemiyorsanız **olayları** menüsünde tıklatın **Gizle grafik** grafik geçiş yapmak için. Varsayılan görünüm olayların işlemi, düzey, durum, kaynak ve saat bilgilerini gösterir. Ek olay öznitelikleri gösterme hakkında daha fazla bilgi için bkz [olay bilgilerini genişletme](backup-azure-monitor-vms.md#view-additional-event-attributes).
2. Bir operasyonel olay hakkında daha fazla bilgi için **işlemi** sütun, kendi dikey penceresini açmak için bir operasyonel olay'ı tıklatın. Dikey olaylarıyla ilgili ayrıntılı bilgiler içerir. Olaylar, kendi bağıntı kimliği ve bir süre içinde gerçekleşen olaylar listesi göre gruplandırılır.

    ![İşlem ayrıntıları](./media/backup-azure-monitor-vms/audit-logs-details-window.png)
3. Olaylar, listesinden belirli bir olay hakkında ayrıntılı bilgileri görüntülemek için açmak için olay'ı tıklatın, **ayrıntıları** dikey.

    ![Olay Ayrıntıları](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    Bilgi alır kadar ayrıntılı olay düzeyi bilgilerdir. Bu kadar her olay bilgilerini görme tercih ve bu kadar ayrıntı eklemek istediğiniz **olayları** dikey penceresinde bölümüne bakın [olay bilgilerini genişletme](backup-azure-monitor-vms.md#view-additional-event-attributes).

## <a name="customize-the-event-filter"></a>Olay filtresi özelleştirme
Kullanım **filtre** ayarlayın veya belirli bir dikey pencerede görüntülenen bilgileri seçin. Olay bilgileri filtrelemek için:

1. İçinde [kasa Panosu](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard)bulun ve tıklatın **denetim günlüklerini** açmak için **olayları** dikey.

    ![Denetim Günlükleri](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    **Olayları** yalnızca geçerli kasa için filtre işletimsel olayları dikey pencere açılır.

    ![Denetim günlükleri filtresi](./media/backup-azure-monitor-vms/audit-logs-filter.png)
2. Üzerinde **olayları** menüsünde tıklatın **filtre** bu dikey penceresini açın.

    ![Filtre dikey penceresini açın](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)
3. Üzerinde **filtre** dikey penceresinde ayarlamak **düzeyi**, **süresi**, ve **arayan** filtreler. Diğer filtreleri kurtarma Hizmetleri kasası geçerli bilgilerini sağlamak için ayarlanan kullanılabilir değildir.

    ![Denetim günlükleri Sorgu Ayrıntıları](./media/backup-azure-monitor-vms/filter-blade.png)

    Belirleyebileceğiniz **düzeyi** olayın: kritik, hata, uyarı veya bilgilendirici. Olay düzeyleri herhangi bir birleşimini seçebilirsiniz, ancak en az bir düzey seçili olması gerekir. Düzeyi açmak veya kapatmak Değiştir. **Süresi** filtre olayları yakalamak için süreyi belirtmenize olanak verir. Özel bir zaman aralığı kullanırsanız, başlangıç ve bitiş zamanlarını ayarlayabilirsiniz.
4. Filtre kullanarak işlem günlüklerini sorgulamak hazır olduğunuzda **güncelleştirme**. Sonuçları görüntülemek **olayları** dikey.

    ![İşlem ayrıntıları](./media/backup-azure-monitor-vms/edited-list-of-events.png)

### <a name="view-additional-event-attributes"></a>Ek olay öznitelikleri görüntüleyin
Kullanarak **sütunları** düğmesi üzerinde listesinde görünmesi ek olay öznitelikleri etkinleştirebilir **olayları** dikey. Varsayılan olaylarının bilgi işlem, düzey, durumu, kaynak ve saat için görüntüler. Ek öznitelikler etkinleştirmek için:

1. Üzerinde **olayları** dikey penceresinde tıklatın **sütunları**.

    ![Açık sütunları](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    **Sütunları seçin** dikey pencere açılır.

    ![Sütun dikey penceresi](./media/backup-azure-monitor-vms/columns-blade.png)
2. Öznitelik seçmek için onay kutusuna tıklayın. Öznitelik onay kutusunu açar ve kapatır.
3. Tıklatın **sıfırlama** içinde öznitelik listesi sıfırlamak için **olayları** dikey. Ekleme veya listeden öznitelikleri kaldırma sonrasında kullanın **sıfırlama** olay öznitelikleri yeni listesini görüntülemek için.
4. Tıklatın **güncelleştirme** olay verileri güncelleştirmek için öznitelikler. Aşağıdaki tabloda her özniteliği hakkında bilgi sağlar.

| Sütun adı | Açıklama |
| --- | --- |
| İşlem |İşlem adı |
| Düzey |İşlem düzeyi, değerler olabilir: bilgi, uyarı, hata veya kritik |
| Durum |Açıklayıcı işlemi durumu |
| Kaynak |Kaynağı tanımlayan URL; Kaynak Kimliği olarak da bilinir |
| Zaman |Zaman, olayın gerçekleştiği geçerli zamandan ölçülür |
| Çağıran |Kim veya ne adlı veya olayı tetikleyen; Sistem ya da bir kullanıcı olabilir |
| Zaman damgası |Olay zaman tetiklendi zamanı |
| Kaynak Grubu |İlişkili kaynak grubu |
| Kaynak Türü |Resource Manager tarafından kullanılan iç kaynak türü |
| Abonelik Kimliği |İlişkili abonelik kimliği |
| Kategori |Olay kategorisi |
| Bağıntı Kimliği |İlgili olayları ortak kimliği |

## <a name="use-powershell-to-customize-alerts"></a>Uyarıları özelleştirmek için PowerShell kullanın
Portalda işleri için özel uyarı bildirimleri alabilirsiniz. Bu işleri almak için işlem günlüklerini olaylarına PowerShell tabanlı uyarı kuralları tanımlayın. Kullanım *PowerShell sürüm 1.3.0 veya daha sonra*.

Yedekleme hataları için uyarı için özel bir bildirim tanımlamak için aşağıdaki komut dosyası gibi bir komutu kullanın:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.RecoveryServices/recoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/Microsoft.RecoveryServices/vaults/trinadhVault -Actions $actionEmail
```

**ResourceId** : denetim günlüklerinden ResourceId elde edebilirsiniz. ResourceId işlem günlükleri Kaynak sütununda sağlanan bir URL'dir.

**OperationName** : OperationName biçiminde olan "Microsoft.RecoveryServices/recoveryServicesVault/*EventName*" nerede *EventName* olabilir:<br/>

* Kaydolma <br/>
* Kaydı Kaldır <br/>
* ConfigureProtection <br/>
* Backup <br/>
* Geri Yükleme <br/>
* StopProtection <br/>
* DeleteBackupData <br/>
* CreateProtectionPolicy <br/>
* DeleteProtectionPolicy <br/>
* UpdateProtectionPolicy <br/>

**Durum** : desteklenen değerler başlatıldı, başarılı veya başarısız oldu.

**Kaynak grubu** : kaynak ait olduğu kaynak grubu budur. Kaynak grubu sütununun oluşturulan günlüklerin ekleyebilirsiniz. Kaynak grubu kullanılabilir türler olay bilgilerinin biridir.

**Ad** : uyarı kuralının adı.

**CustomEmail** : bir uyarı bildirimi göndermek istediğiniz özel e-posta adresi belirtin

**SendToServiceOwners** : Bu seçenek tüm yöneticileri ve ortak yöneticileri aboneliğin uyarı bildirimleri gönderir. İçinde kullanılabilir **yeni AzureRmAlertRuleEmail** cmdlet'i

### <a name="limitations-on-alerts"></a>Uyarıları sınırlamalar
Olay tabanlı uyarılara tabi aşağıdaki sınırlamalar vardır:

1. Kurtarma Hizmetleri Kasası'nda tüm sanal makinelerde uyarıları tetiklenir. Sanal makineler bir kurtarma Hizmetleri kasasına bir kısmı için uyarı özelleştiremezsiniz.
2. Bu özelliğin önizlemede değil. [Daha fazla bilgi](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Uyarılar gönderilen "alerts-noreply@mail.windowsazure.com". Şu anda e-posta gönderen değiştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Olay günlükleri proje harika Sonrası-Değerlendirme etkinleştirme ve yedekleme işlemleri için destek denetleyebilirsiniz. Aşağıdaki işlemleri günlüğe kaydedilir:

* Kaydolma
* Kaydı Kaldır
* Koruma yapılandırma
* Yedekleme (her ikisi de aynı zamanda talep üzerine yedekleme zamanlanmış)
* Geri Yükleme
* Korumayı Durdur
* Yedekleme verilerini sil
* İlke ekleme
* İlkeyi sil
* İlkeyi güncelleştir
* İşi iptal et

Olayları geniş kapsamlı bir açıklama için operations ve Azure Hizmetleri genelinde denetim günlüklerini makalesine bakın [olayları görüntülemek ve Denetim günlükleri](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Bir sanal makine bir kurtarma noktasından yeniden oluşturma hakkında daha fazla bilgi için kullanıma [geri Azure Vm'leri](backup-azure-arm-restore-vms.md). Sanal makinelerinizi koruma bilgi gerekirse bkz [ilk bakış: Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-vms-first-look-arm.md). Makalede VM yedeklemeler için yönetim görevleri hakkında bilgi edinin [yönetmek Azure sanal makine yedeklerini](backup-azure-manage-vms.md).
