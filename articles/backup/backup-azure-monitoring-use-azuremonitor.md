---
title: "Azure yedekleme: Azure İzleyici ile Azure Backup'ı izleme"
description: Azure Backup iş yüklerini izleyebilir ve Azure İzleyicisi'ni kullanarak özel uyarılar oluşturun
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Azure yedekleme; Uyarıları; Tanılama ayarları; Eylem grupları
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: 1e85b633024b5a3e85874707ae9a1f068e7a328d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808515"
---
# <a name="monitoring-at-scale-using-azure-monitor"></a>Uygun ölçekte Azure İzleyicisi'ni kullanarak izleme

[Yerleşik izleme ve uyarı makale](backup-azure-monitoring-built-in-monitor.md) izleme ve uyarı özellikleri tek bir kurtarma Hizmetleri kasası ve herhangi ek bir yönetim altyapısı, satışa listelenir. Ancak yerleşik hizmet aşağıdaki senaryolarda sınırlıdır.

- Abonelikler arasında birden çok RS kasalarını izleme verileri
- E-posta, tercih edilen bildirim kanalını değil ise
- Kullanıcılar daha fazla senaryo için uyarı istiyorsanız
- İçinde gösterilmez, azure'da şirket içi bileşeni System Center DPM (DPM SC) gibi bilgileri görüntülemek istiyorsanız, [yedekleme işleri](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) veya [yedekleme uyarıları](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) portalında.

## <a name="using-log-analytics-workspace"></a>Log Analytics çalışma alanı kullanma

> [!NOTE]
> Log Analytics çalışma alanına tanılama ayarları aracılığıyla Azure VM yedeklemeleri, MAB Aracısı, System Center DPM (SC-DPM), Azure sanal makinelerinde SQL yedekleme verileri ekleniyor. Azure dosya paylaşımı yedeklemelerini, Microsoft Azure Backup sunucusu (MABS) için destek yakında sunulacaktır.

Şu iki Azure Hizmetleri - yeteneklerini yararlanarak **tanılama ayarları** (verileri başka bir kaynağa birden çok Azure Resource Manager kaynakları göndermek için) ve **Log Analytics** (on - oluşturmak için Özel uyarılar) burada tanımlayabilirsiniz Eylem grupları kullanarak diğer bildirim kanallarına uygun ölçekte izleme. Uygun ölçekte Azure Backup izleme LA kullanma hakkında aşağıdaki bölümlerde ayrıntıları.

### <a name="configuring-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma

Azure kurtarma Hizmetleri kasası gibi bir Azure Resource Manager kaynağı zamanlanmış işlemleri ve tetikleyen kullanıcı işlemleri hakkında tüm olası bilgisini tanılama verisi olarak kaydeder. İzleme bölümündeki 'tanılama ayarlarını' tıklayın ve RS kasasının tanılama veri hedefini belirtin.

![Hedef olarak RS kasası tanılama ayarı on ile](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Hedef olarak başka bir Abonelikteki bir LA çalışma alanını seçebilirsiniz. *Birden çok RS kasaları için aynı LA çalışma alanını seçerek, tek bir yerde Aboneliklerdeki kasaları izleyebilirsiniz.* Tüm kanal için günlük 'AzureBackupReport' seçin Azure Backup ilgili bilgileri LA çalışma alanına.

> [!IMPORTANT]
> Yapılandırmasını tamamladıktan sonra ilk veri gönderimi tamamlanması için 24 saat beklemeniz gerekir. Bundan sonra tüm olayları belirtildiği gibi itilir [sıklığı bölüm](#diagnostic-data-update-frequency).

### <a name="deploying-solution-to-log-analytics-workspace"></a>Log Analytics çalışma alanına çözümü dağıtma

Veriler LA çalışma içinde olduğunda [GitHub şablon dağıtma](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) verileri görselleştirmek için on oturum. Aynı kaynak grubunu, çalışma alanı adı ve çalışma alanı konumu düzgün çalışma belirlemek ve bu şablonu üzerinde yüklemeyi size emin olun.

### <a name="view-azure-backup-data-using-log-analytics-la"></a>Log Analytics (on) kullanarak Azure Backup verileri görüntüleme

Şablon dağıtıldığında, Azure Backup izleme çözümü çalışma alanı Özet bölgede gösterilir. Aracılığıyla erişebilen

- Azure İzleyici 'Öngörüleri' bölümünde "Daha fazla" -> ve ilgili çalışma alanını seçin veya
- Log Analytics çalışma alanları seçin -> ilgili çalışma alanını -> 'Çalışma alanı Özet' genel bölümü altında.

![AzureBackupLAMonitoringTile](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

Kutucuğa tıklayarak üzerinde bir dizi temel izleme verileri gibi Azure Backup hakkında grafik Tasarımcı şablonu açar

#### <a name="all-backup-jobs"></a>Tüm yedekleme işleri

![LABackupJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

#### <a name="restore-jobs"></a>Geri yükleme işlerini

![LARestoreJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

#### <a name="inbuilt-azure-backup-alerts-for-azure-resources"></a>Azure kaynakları için yerleşik Azure yedekleme uyarıları

![LAInbuiltAzureBackupAlertsForAzureResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

#### <a name="inbuilt-azure-backup-alerts-for-on-prem-resources"></a>Şirket içi kaynaklar için yerleşik Azure yedekleme uyarıları

![LAInbuiltAzureBackupAlertsForOnPremResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

#### <a name="active-datasources"></a>Etkin veri kaynakları

![LAActiveBackedUpEntities](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

#### <a name="rs-vault-cloud-storage"></a>RS kasa bulut depolama

![LARSVaultCloudStorage](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

Yukarıdaki grafikleri şablonla sağlanır ve müşterinin düzenleme/daha fazla grafik eklemek ücretsizdir.

> [!IMPORTANT]
> Şablonu dağıtırken, aslında bir salt okunur kilidi oluşturur ve, şablonu düzenlemek ve kaydetmek için kaldırmanız gerekir. Log Analytics çalışma alanının 'Ayarlar' bölümünde 'Kilitleri' bölmesinde kilitlerini kaldırma için bakın.

### <a name="create-alerts-using-log-analytics"></a>Log Analytics kullanarak uyarıları oluşturma

Azure İzleyici tanır gerçekleştirebileceğiniz LA çalışma alanından kendi uyarılar oluşturmak için kullanıcıları *, tercih edilen bildirim mekanizması seçmek için Azure eylem gruplarından yararlanan*. Herhangi bir bölümünü 'Günlükleri' LA çalışma alanı açmak için yukarıdaki grafikleri tıklayarak ***nerede sorguları düzenleyebilir ve bunlara uyarıları oluşturma***.

![LACreateAlerts](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

"Yeni uyarı yukarıda da gösterildiği gibi kuralı" tıklayarak Azure İzleyici uyarı oluşturma ekranı açılır.

Aşağıda olduğunu fark edebilirsiniz gibi kaynak zaten LA çalışma alanı olarak işaretlenmiş ve eylem grubu tümleştirme sağlanmıştır.

![LAAzureBackupCreateAlert](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

> [!IMPORTANT]
> Bu sorgu oluşturma ilgili fiyatlandırma etkisi sağlandığını unutmayın [burada](https://azure.microsoft.com/pricing/details/monitor/).

#### <a name="alert-condition"></a>Uyarı koşulu

Uyarı tetikleme koşulunu anahtar yönüdür. 'Koşulu' tıklayarak aşağıda gösterildiği gibi 'Günlükleri' ekranda Kusto sorgusu otomatik olarak yüklenir ve kendi senaryonuza uyacak şekilde düzenleyebilirsiniz. Bazı örnek Kusto sorgular sağlanan [bölümüne](#sample-kusto-queries).

![LAAzureBackupAlertCondition](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Kusto sorgusu, gerekiyorsa düzenleyin, (Bu uyarıyı harekete ne zaman karar verir) doğru eşiği seçin, sağ süresi (zaman penceresi için sorguyu çalıştırın) ve sıklığı. Örneğin: Eşik 0'dan büyükse, süre 5 dakikadır ve sıklığı 5 dakikadır sonra kural olarak "Son 5 dakika boyunca 5 dakikada bir sorgu çalıştırın ve sonuç sayısı 0'dan büyükse, seçili eylem grubu aracılığıyla bildir" çevrilmiştir.

#### <a name="action-group-integration"></a>Eylem grubu tümleştirme

Eylem grupları, kullanılabilir bildirim kanallarını belirtin. "Yeni Oluştur üzerinde" "Altında Eylem grupları" tıklayarak bölüm bildirim mekanizmaları kullanılabilir listesini gösterir.

![LAAzureBackupNewActionGroup](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Daha fazla bilgi edinin [LA çalışma alanından uyarıları](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) ve yaklaşık [Eylem grupları](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) Azure İzleyici belgelerinde.

Bu nedenle tüm uyarı ve izleme tek başına la'dan gereksinimleri karşılaması veya yerleşik bildirim mekanizmaları için tamamlayıcı bir yöntem olarak kullanabilirsiniz.

### <a name="sample-kusto-queries"></a>Kusto sorgu örneği

Varsayılan grafikleri temel senaryolar için uyarılar oluşturun Kusto sorgu verirsiniz. Uyarı istediğiniz verileri almak için bunları değiştirebilirsiniz. Burada yapabilecekleriniz bazı örnek Kusto sorgular 'Günlükleri' penceresine yapıştırın ve ardından sorguya dayalı bir uyarı Oluştur sunuyoruz.

#### <a name="all-successful-backup-jobs"></a>Tüm başarılı yedekleme işleri

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Completed"
````

#### <a name="all-failed-backup-jobs"></a>Tüm yedekleme işleri başarısız oldu

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Failed"
````

#### <a name="all-successful-azure-vm-backup-jobs"></a>Tüm başarılı Azure VM yedekleme işleri

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-sql-log-backup-jobs"></a>Tüm başarılı SQL günlük yedekleme işleri

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-mab-agent-backup-jobs"></a>Tüm başarılı MAB aracısının yedekleme işleri

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

### <a name="diagnostic-data-update-frequency"></a>Tanılama verilerini güncelleştirme sıklığı

Tanılama verileri kasadan LA çalışma alanına bazı gecikme ile ekleniyor. Her olay LA çalışma alanına ulaşan ***RS kasadan gönderildiğinde sonra 20-30 dakika bir gecikme ile.***

- Yedekleme hizmeti yerleşik uyarılar (genelinde tüm çözümler), oluşturulduktan hemen sonra gönderilir. Genellikle LA çalışma alanında bir gecikme 20-30 dakika sonra göründükleri anlamına gelir.
- Geçici yedekleme işleri ve geri yükleme işleri (genelinde tüm çözümler) itilir kısa sürede onlar ***tamamlanır***.
- Zamanlanan yedekleme işlerinin (SQL yedekleme) hariç tüm çözümlerinden kısa sürede onlar itilir ***tamamlanır***.
- SQL yedekleme için günlük yedeklemeler için günlükleri de dahil olmak üzere tüm tamamlanan zamanlanan yedekleme işlerinin, her 15 dakikada olabileceği için bilgiler toplu ve 6 saatte bir gönderildi.
- Yedekleme öğesi, ilke, Kurtarma noktaları, depolama vb. tüm çözümleri arasında gibi diğer tüm bilgiler itilir **en az günde bir kez.**
- İlkeyi düzenleme İlkesi vb. değiştirme gibi yedekleme yapılandırmasında bir değişiklik yedekleme ilgili tüm bilgileri bir anında iletme tetikler.

## <a name="using-rs-vaults-activity-logs"></a>Etkinlik günlüklerini RS kasanın kullanma

Etkinlik günlükleri, yedekleme başarısının gibi olayları bildirimi almak için de kullanabilirsiniz.

> [!CAUTION]
> **Bu yalnızca Azure VM yedeklemeleri için geçerli olduğuna dikkat edin.** Azure, Azure dosyaları, vb. içinde SQL yedekleme Azure Yedekleme aracısı gibi diğer çözümler için kullanamazsınız.

### <a name="sign-in-into-azure-portal"></a>Azure portalında oturum açın

Azure portalında oturum açın, ilgili Azure kurtarma Hizmetleri Kasası'na devam ve Özellikler "Etkinlik günlüğü" bölümünde'ı tıklatın.

### <a name="identify-appropriate-log-and-create-alert"></a>Uygun günlük belirlemek ve uyarı oluşturma

Başarılı yedeklemeler için etkinlik günlüklerini alma doğrulamak için aşağıdaki resimde gösterilen filtre uygulayın. Timespan kayıtları görüntülemek için uygun şekilde değiştirin.

![Azure VM yedeklemeleri için etkinlik günlükleri](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

' A tıklayın işlem adına göre işlemi ve ilgili ayrıntılar görüntülenir.

![Yeni uyarı kuralı](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Tıklayın **yeni uyarı kuralı** açmak için **oluşturma kuralı** ekran, burada oluşturabilirsiniz açıklanan adımları kullanarak uyarı [makale](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

Kurtarma Hizmetleri kasası burada kaynaktır ve bu nedenle etkinlik günlükleri aracılığıyla bildirim istediğiniz tüm kasaları için aynı eylemi yineleyin gerekir. Bu olay-tabanlı uyarı olduğundan koşul sıklığı, süresi, eşiği olmaz. İlgili etkinlik günlüğü oluşturulur hemen sonra uyarı tetiklenir.

## <a name="recommendation"></a>Öneri

***Tüm uyarılar, etkinlik günlüklerinden oluşturulan ve LA çalışma alanları, Azure İzleyicisi'nde sol 'Uyarılar' bölmesinde görüntülenebilir.***

Etkinlik günlükleri aracılığıyla bildirim kullanılabilse ***Azure Backup hizmeti yüksek oranda önerir LA ölçek ve değil etkinlik günlüklerine aşağıdaki nedenlerden dolayı izleme için kullanılacak***.

- **Sınırlı sayıda senaryoda:** Yalnızca Azure VM yedeklemeleri için geçerlidir ve her RS kasası için tekrarlanmalıdır.
- **Uygun tanımı:** Zamanlanmış yedekleme etkinliğin etkinlik günlükleri ile en son tanımlarla sığmayan ve ile uyumludur [tanılama günlükleri](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-you-can-do-with-diagnostic-logs). Bu müşteri adayı için etkinlik günlüğü kanalındaki Pompalama verileri aşağıda olarak değiştirildiğinde beklenmeyen etkisi.
- **Etkinlik günlüğü kanal ile ilgili sorunlar:** Azure Backup kurtarma Hizmetleri kasaları üzerinde etkinlik günlüklerinden Pompalama, yeni bir Modeli'ne taşıdık. Ne yazık ki, taşıma, etkinlik günlüklerini Azure bağımsız bulutlarda nesil etkiledi. Azure bağımsız bulut kullanıcıları oluşturulan/Azure İzleyici aracılığıyla etkinlik günlüklerinden herhangi bir uyarı yapılandırdıysanız, bunlar tetiklenmesi değil. Bir kullanıcının kurtarma Hizmetleri etkinlik günlükleri bir günlük analizi çalışma alanına belirtildiği gibi topladığını, ayrıca, tüm Azure bölgelerinde genel, [burada](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), bu günlükleri değil de görünür.

Bu nedenle, günlük analizi çalışma alanı izleme için kullanılacak önemle tavsiye edilir ve tüm Azure Backup için uygun ölçekte uyarı korumalı iş yükü.

## <a name="next-steps"></a>Sonraki adımlar

- Başvurmak [Log analytics veri modeli](backup-azure-log-analytics-data-model.md) özel sorgular oluşturabilirsiniz.
