---
title: "Yönetme ve Azure sanal makine yedeklerini izleme | Microsoft Docs"
description: "Bir Azure sanal makine yedeklerini izlemek ve yönetmek hakkında bilgi edinin"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e3d3de79c7f2465791ec68f850df2fc6317880f9
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-the-classic-portal"></a>Ortak Azure yedekleme işleri ve tetikleyici uyarıları Klasik portalda yönetin
> [!div class="op_single_selector"]
> * [Azure VM yedeklemeleri yönetme](backup-azure-manage-vms.md)
> * [Klasik VM yedeklemeleri yönetme](backup-azure-manage-vms-classic.md)
>
>

Bu makale ortak yönetim ve izleme görevlerini Azure'da korunan Klasik modeli sanal makineler için hakkında bilgi sağlar.  

> [!NOTE]
> Azure'da kaynak oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli mevcuttur: [Resource Manager ve Klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bkz: [Azure sanal makineleri yedeklemek için ortamınızı hazırlama](backup-azure-vms-prepare.md) Klasik dağıtımı ile çalışma hakkında ayrıntılar için model VM'ler.
>
> [!IMPORTANT]
>Mart 2017’den itibaren Backup kasaları oluşturmak için klasik portalı kullanamayacaksınız.
>
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> 30 Kasım 2017 sonra yedekleme kasaları oluşturmak için PowerShell kullanmanız mümkün olmaz. **30 Kasım 2017 tarafından**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.

## <a name="manage-protected-virtual-machines"></a>Korumalı sanal makineleri yönetme
Korumalı sanal makineleri yönetmek için:

1. Bir sanal makine için Yedekleme ayarlarını görüntülemek ve yönetmek için tıklatın **korunan öğeler** sekmesi.
2. Görmek için korumalı bir öğe adını tıklatın **yedekleme ayrıntıları** sekmesinde son yedekleme hakkında bilgi gösterir.

    ![Sanal makine yedekleme](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. Görüntülemek ve yönetmek için bir sanal makine için bir yedekleme İlkesi ayarları'nı tıklatın. **ilkeleri** sekmesi.

    ![Sanal Makine ilkesi](./media/backup-azure-manage-vms/manage-policy-settings.png)

    **Yedekleme ilkeleri** sekmesi varolan ilkeyi gösterir. Gerektiği şekilde değiştirebilirsiniz. Yeni bir ilke oluşturmak ihtiyacınız varsa **oluşturma** üzerinde **ilkeleri** sayfası. Bir ilke kaldırmak istiyorsanız, kendisiyle ilişkilendirilmiş herhangi bir sanal makine olmamalıdır unutmayın.

    ![Sanal Makine ilkesi](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. Eylemler veya durumu hakkında daha fazla bilgi için sanal makine Al **işleri** sayfası. Daha fazla bilgi almak için listedeki bir işi tıklayın ya da işleri belirli bir sanal makine filtreleyebilirsiniz.

    ![İşler](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>Bir sanal makinenin talep üzerine yedekleme
Koruma için yapılandırıldıktan sonra isteğe bağlı bir sanal makine yedekleme devam edebilir. İlk yedekleme bekleme durumundaysa sanal makine için talep üzerine yedekleme Azure yedekleme kasasında sanal makinenin tam bir kopyasını oluşturur. İlk yedekleme tamamlandığında, Azure yedekleme önceki yedekten gönderme değişiklikler yani kasa yalnızca talep üzerine yedekleme her zaman artımlıdır.

> [!NOTE]
> Bir talep üzerine yedekleme bekletme aralığı günlük bekletme VM'ye karşılık gelen yedekleme İlkesi'nde belirtilen bekletme değerine ayarlanır.  
>
>

İsteğe bağlı bir sanal makineye yedek almak için:

1. Gidin **korunan öğeler** sayfasından seçim yapıp **Azure sanal makine** olarak **türü** (henüz seçili değilse) ve tıklayın **seçin** düğmesi.

    ![VM türü](./media/backup-azure-manage-vms/vm-type.png)
2. İsteğe bağlı yedekleme alıp tıklayın istediğiniz sanal makineyi seçin **şimdi yedek** altındaki sayfasının düğmesini.

    ![Şimdi Yedekle](./media/backup-azure-manage-vms/backup-now.png)

    Seçili sanal makinede bu yedekleme işi oluşturur. Bu iş oluşturulan kurtarma noktası bekletme aralığını sanal makineyle ilişkili İlkesi'nde belirtilen aynı olacaktır.

    ![Yedekleme işi oluşturma](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > Bir sanal makineyle ilişkili ilkesini görüntülemek için ayrıntıya aşağı sanal makinede **korunan öğeler** sayfasında ve yedekleme İlkesi sekmesine gidin.
   >
   >
3. İş oluşturulduğunda, tıklatabilirsiniz **işi görüntüle** işleri sayfasında karşılık gelen iş görmek için bildirim çubuğunda düğmesi.

    ![Yedekleme işi oluşturuldu](./media/backup-azure-manage-vms/created-job.png)
4. İş başarıyla tamamlandıktan sonra sanal makineyi geri yüklemek için kullanabileceğiniz bir kurtarma noktası oluşturulur. Bu aynı zamanda kurtarma noktası sütun değeri 1'tarafından artırır **korunan öğeler** sayfası.

## <a name="stop-protecting-virtual-machines"></a>Sanal makineleri koruma Durdur
Aşağıdaki seçeneklere sahip bir sanal makine, gelecekteki yedeklemeler durdurmayı seçebilirsiniz:

* Azure yedekleme kasasında sanal makineyle ilişkili yedekleme verilerini korur
* Sanal makine ile ilişkili yedekleme verilerini sil

Sanal makine ile ilişkili yedekleme verilerini korumak için seçtiyseniz, sanal makineyi geri yüklemek için yedekleme verilerini kullanabilirsiniz. Bu tür sanal makineler için ayrıntıları fiyatlandırma için tıklatın [burada](https://azure.microsoft.com/pricing/details/backup/).

Bir sanal makine için korumayı durdurmak için:

1. Gidin **korunan öğeler** sayfasından seçim yapıp **Azure sanal makinesi** 'ı tıklatın ve (henüz seçili değilse) filtre türü olarak **seçin** düğmesi.

    ![VM türü](./media/backup-azure-manage-vms/vm-type.png)
2. Sanal makineyi seçin ve tıklayın **korumayı Durdur** sayfanın sonundaki.

    ![Korumayı Durdur](./media/backup-azure-manage-vms/stop-protection.png)
3. Varsayılan olarak, Azure yedekleme sanal makineyle ilişkili yedekleme verileri silmez.

    ![Durdurma onaylayın](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    Yedekleme verilerini silmek istiyorsanız, onay kutusunu seçin.

    ![Onay kutusu](./media/backup-azure-manage-vms/checkbox.png)

    Lütfen yedeklemeyi durdurma nedeni seçin. Bu isteğe bağlı olmakla birlikte, bir nedeni sağlama geribildirim iş ve müşteri senaryoları önceliğini belirlemek için Azure Yedekleme'yi yardımcı olur.
4. Tıklayın **gönderme** gönderme düğmesi **korumayı** işi. Tıklayın **işi görüntüle** işinde karşılık gelen görmek için **işleri** sayfası.

    ![Korumayı Durdur](./media/backup-azure-manage-vms/stop-protect-success.png)

    Değil seçtiyseniz **Delete ilişkilendirilmiş yedekleme verileri** sırasında seçeneği **korumayı Durdur** sihirbazını sonra post iş tamamlandığında, koruma durumu değişiklikleri **koruması durdurulmuş**. Açıkça silinene kadar verileri Azure yedekleme ile kalır. Sanal makinede seçerek verileri her zaman silebilir **korunan öğeler** sayfa ve tıklayarak **silmek**.

    ![Durdurulan koruma](./media/backup-azure-manage-vms/protection-stopped-status.png)

    Seçtiyseniz **Delete ilişkilendirilmiş yedekleme verileri** seçeneği, sanal makinenin parçası olmayacak **korunan öğeler** sayfası.

## <a name="re-protect-virtual-machine"></a>Sanal makine yeniden koruma
Değil seçtiyseniz **ilişkilendirme yedekleme verileri silmek** seçeneğini **korumayı Durdur**, sanal makine kaydedilmiş sanal makineleri yedekleme için benzer adımları izleyerek yeniden koruyabilirsiniz. Korumalı, bu sanal makineyi durdurma önce korunur yedekleme verilere sahip olur ve sonra kurtarma noktaları oluşturulur sonra yeniden koruyun.

Yeniden koruma sonra sanal makinenin koruma durumu değiştirilecek **korumalı** kurtarma noktası öncesinde yoksa **korumayı Durdur**.

  ![Korunmuş VM](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Sanal makine yeniden korurken, başka bir ilke ile sanal makine başlangıçta korunan ilkesinden seçebilirsiniz.
>
>

## <a name="unregister-virtual-machines"></a>Sanal makineler kaydı
Sanal makine yedekleme Kasası'nı kaldırmak istiyorsanız:

1. Tıklayın **UNREGISTER** altındaki sayfasının düğmesini.

    ![Korumayı devre dışı bırakın](./media/backup-azure-manage-vms/unregister-button.png)

    Bir bildirim onaylamanızı isteyen ekranın alt kısmında görüntülenir. Tıklatın **Evet** devam etmek için.

    ![Korumayı devre dışı bırakın](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini sil
Ya da bir sanal makineyle ilişkili yedekleme verileri silebilirsiniz:

* Korumayı Durdur işini sırasında
* Bir sanal makinede durdurma sonra iş tamamlandı

İçinde olan bir sanal makine, yedekleme verilerini silmek için *koruması durdurulmuş* durumu başarılı şekilde tamamlandığını sonrası bir **durdurmak yedekleme** iş:

1. Gidin **korunan öğeler** sayfasından seçim yapıp **Azure sanal makine** olarak *türü* tıklatıp **seçin** düğmesi.

    ![VM türü](./media/backup-azure-manage-vms/vm-type.png)
2. Sanal makineyi seçin. Sanal makine olacaktır **koruması durdurulmuş** durumu.

    ![Koruma durduruldu](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Tıklatın **silmek** altındaki sayfasının düğmesini.

    ![Yedek Sil](./media/backup-azure-manage-vms/delete-backup.png)
4. İçinde **yedekleme verileri Sil** Sihirbazı'nı (önemle önerilir) yedekleme verilerini silmek için bir neden seçin ve tıklatın **gönderme**.

    ![Yedekleme verilerini sil](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Bu, seçilen sanal makine, yedekleme verilerini silmek için bir iş oluşturur. Tıklatın **işi görüntüle** işleri sayfasında karşılık gelen iş görmek için.

    ![Veri silme başarılı](./media/backup-azure-manage-vms/delete-data-success.png)

    İş tamamlandıktan sonra sanal makineye karşılık gelen girişi kaldırılır **korunan öğeler** sayfası.

## <a name="dashboard"></a>Pano
Üzerinde **Pano** sayfa Azure sanal makineler, depolama ve son 24 saat içindeki ilişkili işleri hakkındaki bilgileri gözden geçirebilirsiniz. Yedekleme durumu ve ilişkili yedekleme hataları görüntüleyebilirsiniz.

![Pano](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Pano değerleri her 24 saatte bir yenilenir.
>
>

## <a name="auditing-operations"></a>Denetim işlemleri
Azure backup "tam olarak hangi yönetim işlemlerini yedekleme kasası üzerinde gerçekleştirilen görmeyi kolaylaştırır müşteri tarafından tetiklenen işlem günlüklerinin" yedekleme işlemleri incelenmesi sağlar. İşlem günlükleri proje harika Sonrası-Değerlendirme etkinleştirme ve yedekleme işlemleri için destek denetleyebilirsiniz.

Aşağıdaki işlemleri işlem günlüklerine kaydedilir:

* Kaydolma
* Kaydı Kaldır
* Koruma yapılandırma
* Yedekleme (Bu her ikisi de talep üzerine yedekleme BackupNow aracılığıyla yanı sıra zamanlanmış)
* Geri Yükleme
* Korumayı Durdur
* Yedekleme verilerini sil
* İlke ekleme
* İlkeyi Sil
* Güncelleştirme ilkesi
* İşi iptal et

Bir yedekleme Kasası'na karşılık gelen işlem günlükleri görüntülemek için:

1. Gidin **Yönetim Hizmetleri** Azure portal ve ardından **işlem günlükleri** sekmesi.

    ![İşlem günlükleri](./media/backup-azure-manage-vms/ops-logs.png)
2. Filtreleri seçin **yedekleme** olarak *türü* ve yedekleme kasasının adını belirtin *hizmet adı* ve tıklayın **gönderme**.

    ![İşlem günlükleri filtresi](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. İşlem günlükleri, herhangi bir işlem seçin ve tıklatın **ayrıntıları** karşılık gelen bir işlem ayrıntıları görmek için.

    ![İşlem günlükleri getirme ayrıntıları](./media/backup-azure-manage-vms/ops-logs-details.png)

    **Ayrıntıları Sihirbazı** tetiklenen, iş kimliği, üzerinde bu işlemi tetiklenir ve işlem başlatışınızda kaynak işlemiyle ilgili bilgi içerir.

    ![İşlem ayrıntıları](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Uyarı bildirimleri
Portalda işleri için özel uyarı bildirimleri alabilirsiniz. Bu işlem günlüklerini olaylarına PowerShell'i dayalı olarak uyarı kuralları tanımlayarak sağlanır. Kullanmanızı öneririz *PowerShell sürüm 1.3.0 veya yukarıdaki*.

Yedekleme hataları için uyarı için özel bir bildirim tanımlamak için bir örnek komut gibi görünür:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: Bu işlem günlükleri açılır penceresinden bölümde açıklandığı gibi alabilirsiniz. Bir işlemin ayrıntıları açılan penceresinde ResourceUri için bu cmdlet sağlanacak ResourceId ' dir.

**OperationName**: Bu biçimi olacaktır "Microsoft.Backup/backupvault/<EventName>" EventName kayıt, Unregister, ConfigureProtection biri olduğunda, yedekleme, geri yükleme, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy

**Durum**: değerlerini olduğunuz-başlatıldı, desteklenen başarılı ve başarısız oldu.

**Kaynak grubu**: işlemi tetiklenir kaynak kaynak grubu. Bu ResourceId değeri elde edebilirsiniz. Alanlar arasında değer */resourceGroups/* ve */providers/* içinde ResourceId değeri ResourceGroup değeridir.

**Ad**: uyarı kuralının adı.

**CustomEmail**: uyarı bildirim göndermek istediğiniz özel e-posta adresi belirtin

**SendToServiceOwners**: Bu seçenek tüm yöneticileri ve ortak yöneticileri aboneliğin uyarı bildirim gönderir. İçinde kullanılabilir **yeni AzureRmAlertRuleEmail** cmdlet'i

### <a name="limitations-on-alerts"></a>Uyarıları sınırlamalar
Olay tabanlı uyarılara aşağıdaki sınırlamalara tabi:

1. Yedekleme Kasası'nda tüm sanal makinelerde uyarıları tetiklenir. Belirli bir yedekleme kasasına sanal makineler kümesi için uyarılar almak için özelleştiremezsiniz.
2. Bu özelliğin önizlemede değil. [Daha fazla bilgi](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Gelen uyarılar alırsınız "alerts-noreply@mail.windowsazure.com". Şu anda e-posta gönderen değiştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure sanal makineleri geri yükleme](backup-azure-restore-vms.md)
