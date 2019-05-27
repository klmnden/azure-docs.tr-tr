---
title: Otomatik ayarlama e-posta bildirimleri ile ilgili nasıl yapılır Kılavuzu - Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı sorgu otomatik ayarlama e-posta bildirimlerini etkinleştirin.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 2af8ef7d29d1ac506ddca654544bc938758aa0d8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66149796"
---
# <a name="email-notifications-for-automatic-tuning"></a>E-posta bildirimlerini otomatik ayarlama

SQL veritabanı ayarlama önerileri Azure SQL veritabanı tarafından oluşturulan [otomatik ayarlama](sql-database-automatic-tuning.md). Bu çözümü sürekli olarak izler ve SQL veritabanları sağlama iş yükleri önerilerinde dizin oluşturma, dizin silinmesi ve sorgu yürütme planlarını iyileştirilmesi için ilgili tek tek her veritabanı için özelleştirilmiş çözümler.

SQL veritabanı otomatik ayarlama önerileri içinde görüntülenebilir [Azure portalında](sql-database-advisor-portal.md), birlikte alınan [REST API](https://docs.microsoft.com/rest/api/sql/databaserecommendedactions/listbydatabaseadvisor) kullanarak veya çağıran [T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) ve [ PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction) komutları. Bu makalede, otomatik ayarlama önerileri almak için bir PowerShell betiğini kullanarak temel alır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

## <a name="automate-email-notifications-for-automatic-tuning-recommendations"></a>E-posta bildirimlerini otomatik ayarlama önerileri için otomatik hale getirin

Aşağıdaki çözüm otomatik ayarlama önerileri içeren e-posta bildirimleri gönderme otomatikleştirir. Açıklanan çözümünü kullanarak ayar önerileri almak için bir PowerShell betiğini yürütme otomatikleştirme oluşur [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro), Otomasyon zamanlama e-posta teslim işlemini kullanarak ve [Microsoft Flow ](https://flow.microsoft.com).

## <a name="create-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

Azure Otomasyonu, ilk adım bir Otomasyon hesabı oluşturmak ve PowerShell betiğinin yürütülmesi için kullanılacak Azure kaynaklarıyla yapılandırmak için kullanmaktır. Azure Otomasyonu ve özellikleri hakkında daha fazla bilgi için bkz: [Azure Otomasyonu ile çalışmaya başlama](https://docs.microsoft.com/azure/automation/automation-offering-get-started).

Seçme ve Market'ten Otomasyon uygulamasını yapılandırma yöntemi aracılığıyla Azure Otomasyonu hesabını oluşturmak için şu adımları izleyin:

- Azure portalında oturum açın
- Tıklayın "**+ kaynak Oluştur**" sol üst köşedeki
- Arama "**Otomasyon**" (enter tuşuna basın)
- Arama sonuçlarında Otomasyonu uygulamaya tıklayarak

![Azure Otomasyonu ekleme](./media/sql-database-automatic-tuning-email-notifications/howto-email-01.png)

- "Bir Otomasyon hesabı oluştur" bölmesinde içinde bir kez tıklayın "**Oluştur**"
- Gerekli bilgileri doldurun: Bu Otomasyon hesabı için bir ad girin, PowerShell Betiği yürütme için kullanılacak Azure abonelik kimliği ve Azure kaynaklarınızı seçin
- İçin "**oluşturma Azure farklı çalıştır hesabı**" seçeneği için **Evet** altında hangi PowerShell Betiği, Azure Otomasyonu yardımıyla çalıştıran hesap türünü yapılandırmak için. Hesap türleri hakkında daha fazla bilgi için bkz: [farklı çalıştır hesabı](https://docs.microsoft.com/azure/automation/automation-create-runas-account)
- Otomasyon hesabı oluşturma tıklayarak sonlandırma **oluştur**

> [!TIP]
> Otomasyon uygulamasını oluştururken girildiği gibi tam olarak, Azure Otomasyonu hesabı adı, abonelik kimliği ve kaynakları (örneğin, bir not defteri kopyala-yapıştır) kaydedin. Bu bilgiler daha sonra ihtiyacınız var.
>

Aynı Otomasyon oluşturmak istediğiniz birden fazla Azure aboneliğiniz varsa, diğer abonelikler için bu işlemi tekrarlamanız gerekir.

## <a name="update-azure-automation-modules"></a>Azure Automation modülleri güncelleştirme

Otomatik ayarlama öneri almak için PowerShell betiğini kullanır [Get-AzResource](https://docs.microsoft.com/powershell/module/az.Resources/Get-azResource) ve [Get-AzSqlDatabaseRecommendedAction](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlDatabaseRecommendedAction) hangi Azure modüllerini güncelleştirme 4 sürüm komutları ve sonraki sürümleri gereklidir.

Azure PowerShell modüllerini güncelleştirme için şu adımları izleyin:

- Otomasyon uygulama bölmesi erişin ve seçin "**modülleri**" sol taraftaki menüsünden (paylaşılan kaynaklar altında bu menü öğesi olduğu gibi kaydırın).
- Modüller bölmesinde, tıklayarak "**güncelleştirme Azure modülleri**" üstünde ve "Azure modülleri güncelleştirildi" iletisi görüntülenene kadar bekleyin. Bu işlemin tamamlanması birkaç dakika sürebilir.

![Azure automation modülleri güncelleştirme](./media/sql-database-automatic-tuning-email-notifications/howto-email-02.png)

## <a name="create-azure-automation-runbook"></a>Azure Otomasyonu Runbook'u oluşturma

Sonraki adımda PowerShell betiğine alma önerilerinde, içinde bulunduğu Azure Otomasyonu'nda Runbook oluşturma sağlamaktır.

Yeni bir Azure Otomasyonu runbook oluşturmak için aşağıdaki adımları izleyin:

- Önceki adımda oluşturduğunuz Azure Otomasyonu hesabı erişim
- Otomasyon hesabı bölmesinde bir kez tıklayın "**runbook'ları**" yeni bir Azure Otomasyonu runbook ile PowerShell betiği oluşturmak için sol taraftaki menü öğesi. Otomasyon runbook'larınızı oluşturma hakkında daha fazla bilgi için bkz: [yeni bir runbook oluşturma](../automation/manage-runbooks.md#create-a-runbook).
- Yeni bir runbook eklemek için tıklayın "**+ runbook Ekle**" seçeneğine ve ardından üzerinde "**hızlı oluştur – yeni bir runbook oluşturmak**".
- Runbook, runbook adını yazın (Bu örneğin amacı doğrultusunda "**AutomaticTuningEmailAutomation**" kullanılır), runbook türünü seçin **PowerShell** ve açıklamasını yazma amacı açıklamak için bu runbook.
- Tıklayarak **Oluştur** yeni bir runbook oluşturma işlemini düğmesi

![Azure Otomasyonu runbook'u ekleme](./media/sql-database-automatic-tuning-email-notifications/howto-email-03.png)

Oluşturulan runbook içindeki bir PowerShell Betiği yüklemek için aşağıdaki adımları izleyin:

- İçinde "**PowerShell Runbook'u Düzenle**"bölmesinde"**runbook'ları**" menüsünde ağaç ve görünümü, runbook adı görene kadar genişletin (Bu örnekte " **AutomaticTuningEmailAutomation**"). Bu runbook'u seçin.
- "PowerShell (numarası 1 ile başlayan) Runbook'unu Düzenle" ilk satırında kopyala-yapıştır aşağıdaki PowerShell komut dosyası kodu. Bu PowerShell Betiği olarak sağlanan-başlamanıza yardımcı olmaktır. Komut dosyasını Suite gereksinimlerinizi değiştirin.

PowerShell betiğini üst bilgisinde değiştirmeniz gerekiyor. `<SUBSCRIPTION_ID_WITH_DATABASES>` Azure abonelik kimliğinizi Azure abonelik Kimliğinizi almak nasıl öğrenmek için bkz. [başlama, Azure abonelik GUİD'i](https://blogs.msdn.microsoft.com/mschray/20../../getting-your-azure-subscription-guid-new-portal/).

Birkaç abonelikleri olması durumunda bunları virgülle ayrılmış komut dosyasının üst bilgisindeki "$subscriptions" özelliğine ekleyebilirsiniz.

```powershell
# PowerShell script to retrieve Azure SQL Database Automatic tuning recommendations.
#
# Provided “as-is” with no implied warranties or support.
# The script is released to the public domain.
#
# Replace <SUBSCRIPTION_ID_WITH_DATABASES> in the header with your Azure subscription ID.
#
# Microsoft Azure SQL Database team, 2018-01-22.

# Set subscriptions : IMPORTANT – REPLACE <SUBSCRIPTION_ID_WITH_DATABASES> WITH YOUR SUBSCRIPTION ID
$subscriptions = ("<SUBSCRIPTION_ID_WITH_DATABASES>", "<SECOND_SUBSCRIPTION_ID_WITH_DATABASES>", "<THIRD_SUBSCRIPTION_ID_WITH_DATABASES>")

# Get credentials
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Define the resource types
$resourceTypes = ("Microsoft.Sql/servers/databases")
$advisors = ("CreateIndex", "DropIndex");
$results = @()

# Loop through all subscriptions
foreach($subscriptionId in $subscriptions) {
    Select-AzSubscription -SubscriptionId $subscriptionId
    $rgs = Get-AzResourceGroup

    # Loop through all resource groups
    foreach($rg in $rgs) {
        $rgname = $rg.ResourceGroupName;

        # Loop through all resource types
        foreach($resourceType in $resourceTypes) {
            $resources = Get-AzResource -ResourceGroupName $rgname -ResourceType $resourceType

            # Loop through all databases
            # Extract resource groups, servers and databases
            foreach ($resource in $resources) {
                $resourceId = $resource.ResourceId
                if ($resourceId -match ".*RESOURCEGROUPS/(?<content>.*)/PROVIDERS.*") {
                    $ResourceGroupName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*SERVERS/(?<content>.*)/DATABASES.*") {
                    $ServerName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*/DATABASES/(?<content>.*)") {
                    $DatabaseName = $matches['content']
                } else {
                    continue
                }

                # Skip if master
                if ($DatabaseName -eq "master") {
                    continue
                }

                # Loop through all Automatic tuning recommendation types
                foreach ($advisor in $advisors) {
                    $recs = Get-AzSqlDatabaseRecommendedAction -ResourceGroupName $ResourceGroupName -ServerName $ServerName  -DatabaseName $DatabaseName -AdvisorName $advisor
                    foreach ($r in $recs) {
                        if ($r.State.CurrentValue -eq "Active") {
                            $object = New-Object -TypeName PSObject
                            $object | Add-Member -Name 'SubscriptionId' -MemberType Noteproperty -Value $subscriptionId
                            $object | Add-Member -Name 'ResourceGroupName' -MemberType Noteproperty -Value $r.ResourceGroupName
                            $object | Add-Member -Name 'ServerName' -MemberType Noteproperty -Value $r.ServerName
                            $object | Add-Member -Name 'DatabaseName' -MemberType Noteproperty -Value $r.DatabaseName
                            $object | Add-Member -Name 'Script' -MemberType Noteproperty -Value $r.ImplementationDetails.Script
                            $results += $object
                        }
                    }
                }
            }
        }
    }
}

# Format and output results for the email
$table = $results | Format-List
Write-Output $table
```

Tıklayın "**Kaydet**" betiği kaydetmek için sağ üst köşedeki düğmesi. Betikle memnun kaldığınızda, tıklayın "**Yayımla**" düğmesi, bu runbook'u yayımlamak için.

Ana runbook bölmesinde, tıklayarak seçebilirsiniz "**Başlat**" düğmesi **test** betiği. Tıklayın "**çıkış**" yürütülen betik sonuçlarını görüntülemek için. Bu çıkış, e-posta içeriğini zordur. Örnek betiğin çıkışı aşağıdaki ekran görüntüsünde görülebilir.

![Görünüm otomatik ayarlama önerileri Azure Otomasyonu ile Çalıştır](./media/sql-database-automatic-tuning-email-notifications/howto-email-04.png)

PowerShell betiğini ihtiyaçlarınıza özelleştirerek içeriği ayarlamanıza emin olun.

Yukarıdaki adımlarla otomatik ayarlama önerileri almak için PowerShell Betiği, Azure Automation'da yüklenir. Sonraki adım, otomatik hale getirmek ve e-posta teslim işi zamanlama oluşturmaktır.

## <a name="automate-the-email-jobs-with-microsoft-flow"></a>Microsoft Flow ile e-posta işleri otomatikleştirin

Son adım olarak çözüm tamamlamak için Microsoft Flow üç eylem (işler) oluşan bir Otomasyon akışı oluşturun:

1. "**- Azure Otomasyonu işi oluşturma**" – otomatik ayarlama önerileri Azure Otomasyonu runbook'u içine almak için PowerShell betiğini çalıştırmak için kullanılır
2. "**Azure Otomasyonu - Get iş çıktısı**" – yürütülen PowerShell komut dosyasından çıkış almak için kullanılır
3. "**Office 365 Outlook-e-posta Gönder**" – e-posta göndermek için kullanılır. Akış oluşturma kişinin Office 365 hesabınızı kullanarak e-posta gönderilir.

Microsoft Flow özellikleri hakkında daha fazla bilgi edinmek için [Microsoft Flow ile çalışmaya başlama](https://docs.microsoft.com/flow/getting-started).

Bu adım için önkoşuldur kaydolmak için [Microsoft Flow](https://flow.microsoft.com) hesabı ile oturum açın. Bir kez çözüm içinde ayarlamak için bu adımları bir **yeni akış**:

- Erişim "**Akışlarım**" menü öğesi
- Akışlarım içinde seçin "**+ boş akış Oluştur**" sayfasının üstündeki bağlantısı
- Bağlantıyı tıklatın "**yüzlerce bağlayıcı ve tetikleyicide arama**" sayfanın alt kısmındaki
- Arama alanı türü "**yinelenme**" seçin "**zamanlama - yinelenme**" e-posta teslim işi çalıştırmak için zamanlama ve arama sonuçlarında.
- Yinelenme sıklığı alanındaki bölmesinde bu akışı çalıştırmak için otomatik gönderme gibi e-posta her dakika, saat, gün, haftalık, vb. için zamanlama sıklığını seçin.

Sonraki adım, yeni oluşturulan yinelenen akış için üç işleri (get çıkış ve Gönder'e-posta oluşturma,) eklemektir. Gerekli iş akışına ekleme yapmak için aşağıdaki adımları izleyin:

1. Ayar önerileri almak için PowerShell Betiği yürütmek için eylem oluşturma

   - Seçin "**+ yeni adım**", ardından"**Eylem Ekle**" içindeki yinelenme akış bölmesi
   - Arama alanı türü "**Otomasyon**"ve"**Azure Otomasyonu – oluşturma işi**" Arama sonuçlarından
   - Oluşturma işi bölmesinde iş özelliklerini yapılandırın. Bu yapılandırma için Azure abonelik kimliği, kaynak grubu ve Otomasyon hesabı ayrıntılarını gerekir **daha önce kaydedilen** adresindeki **Otomasyon hesabı bölmesinde**. Bu bölümdeki seçenekleri hakkında daha fazla bilgi edinmek için [Azure Otomasyonu - işi oluştur](https://docs.microsoft.com/connectors/azureautomation/#create-job).
   - Bu eylem tıklayarak oluşturmayı tamamlayamadı "**akışı Kaydet**"

2. Yürütülen PowerShell komut dosyasından çıkış almak için eylem oluşturma

   - Seçin "**+ yeni adım**", ardından"**Eylem Ekle**" içindeki yinelenme akış bölmesi
   - Arama türü Dosyalanan "**Otomasyon**"ve"**Azure Otomasyonu – Get iş çıktısı**" Arama sonuçlarından. Bu bölümdeki seçenekleri hakkında daha fazla bilgi edinmek için [Azure Otomasyonu – Get iş çıktısı](https://docs.microsoft.com/connectors/azureautomation/#get-job-output).
   - Doldur (önceki işi oluşturmaya benzer) gerekli alanları - Otomasyon hesabı ve Azure abonelik kimliği, kaynak grubu, (Otomasyon hesabı bölmesinde girildiği gibi) Doldur
   - ' A tıklayın alanın içine "**iş kimliği**" için "**dinamik içerik**" menüsünde gösterilecek. Bu menü seçeneğini seçin "**iş kimliği**".
   - Bu eylem tıklayarak oluşturmayı tamamlayamadı "**akışı Kaydet**"

3. E-posta kullanarak Office 365 tümleştirmesi göndermek için eylem oluşturma

   - Seçin "**+ yeni adım**", ardından"**Eylem Ekle**" içindeki yinelenme akış bölmesi
   - Arama türü Dosyalanan "**bir e-posta**"ve"**Office 365 Outlook-e-posta Gönder**" Arama sonuçlarından
   - İçinde "**için**" alanında bildirim e-posta göndermek ihtiyacınız olan e-posta adresini yazın
   - İçinde "**konu**" alan e-postanızın konusu örneğin "otomatik ayarlama önerilerinin e-posta bildirimi" türü
   - ' A tıklayın alanın içine "**gövdesi**" için "**dinamik içerik**" menüsünde gösterilecek. Gelen içinde bu menü altında "**alın, iş çıktısı**"seçeneğini"**içerik**"
   - Bu eylem tıklayarak oluşturmayı tamamlayamadı "**akışı Kaydet**"

> [!TIP]
> Farklı alıcılara otomatik e-postalar göndermek için ayrı akışlar oluşturun. Bu ek akışlarında alıcı e-posta adresi "Kime" alanına ve "Konu" alanında e-posta konu satırını değiştirin. Özelleştirilmiş PowerShell betikleri ile Azure Otomasyonu'nda yeni runbook'lar oluşturma (gibi değişikliği Azure abonelik kimliği ile) daha fazla özelleştirme sağlayan otomatik senaryoları gibi örneğin otomatik ayarlama üzerinde ayrı alıcılara e-postayla gönderme ayrı ayrı abonelikler için önerileri.
>

Yukarıdaki'de, e-posta teslim işi iş akışını yapılandırmak için gereken adımlar burada sona eriyor. Aşağıdaki görüntüde üç eylemleri yerleşik oluşan tüm akışı gösterilmektedir.

![Otomatik ayarlama e-posta bildirimleri akış görünümü](./media/sql-database-automatic-tuning-email-notifications/howto-email-05.png)

Akışı test etmek için tıklayın "**Şimdi Çalıştır**" İç akış bölmesinde sağ üst köşedeki.

Akış analizi bölmesinden, e-posta bildirimi başarısını gösterecek şekilde otomatik işleri çalıştırmanın istatistikleri görülebilir.

![Otomatik ayarlama e-posta bildirimleri için akış çalıştırma](./media/sql-database-automatic-tuning-email-notifications/howto-email-06.png)

Akış analizi işi yürütme başarılı izlemek için yararlıdır ve sorun giderme için gerekirse.  Sorun giderme söz konusu olduğunda, ayrıca Azure Otomasyonu uygulama üzerinden erişilebilir PowerShell komut dosyası yürütme günlüğü incelemek isteyebilirsiniz.

Otomatik e-posta son çıkışı, aşağıdaki e-posta oluşturma ve bu çözümü çalıştırma sonra alınan benzer:

![Örnek e-posta otomatik ayarlama e-posta bildirimleri çıktısı](./media/sql-database-automatic-tuning-email-notifications/howto-email-07.png)

PowerShell Betiği ayarlayarak, çıktı ve ihtiyaçlarınıza otomatik e-posta biçimini ayarlayabilirsiniz.

Daha fazla derleme belirli bir ayar eylemi ve birden çok abonelik veya özel senaryolarınıza bağlı olarak, veritabanları için birden fazla alıcıya e-posta bildirimleri için çözümü özelleştirmek.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla üzerinde nasıl otomatik ayarlama, veritabanı performansını artırmak için bkz: yardımcı olabileceğini öğrenin [otomatik ayarlama Azure SQL veritabanı'nda](sql-database-automatic-tuning.md).
- İş yükünüz yönetmek için Azure SQL veritabanında otomatik ayarlama etkinleştirmek için bkz: [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).
- El ile incelemeniz ve otomatik ayarlama önerileri uygulamak için bkz: [bulun ve performans önerilerini uygulama](sql-database-advisor-portal.md).
