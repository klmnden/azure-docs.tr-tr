---
title: "Otomatik ayarlama e-posta bildirimlerini nasıl yapılır Kılavuzu - Azure SQL veritabanı | Microsoft Docs"
description: "Azure SQL veritabanını SQL sorgusunun analiz eder ve otomatik olarak kullanıcı iş yüküne uyum sağlar."
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
ms.reviewer: carlrab
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/31/2018
ms.author: v-daljep
ms.openlocfilehash: 3598c61634bf039e5c9500cc9a4f6b08e4830ce5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="email-notifications-for-automatic-tuning"></a>E-posta bildirimleri otomatik ayarlama

SQL veritabanı ayarlama önerileri Azure SQL veritabanı tarafından üretilen [otomatik ayarlama](sql-database-automatic-tuning.md). Bu çözüm, sürekli olarak izler ve dizin oluşturma, dizin silinmesi ve sorgu yürütme planları en iyi duruma getirilmesi için ilgili tek tek her veritabanı için öneriler ayarlama özelleştirilmiş SQL veritabanlarını sağlama iş yükleri analiz eder.

SQL veritabanı önerileri ayarlama otomatik görüntülenebilir içinde [Azure portal](sql-database-advisor-portal.md), ile alınan [REST API](https://docs.microsoft.com/en-us/rest/api/sql/databaserecommendedactions/listbydatabaseadvisor) kullanarak veya çağırır [T-SQL](https://azure.microsoft.com/en-us/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) ve [ PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabaserecommendedaction) komutları. Bu makalede, otomatik ayarlama önerileri almak için bir PowerShell Betiği kullanılarak temel alır.

## <a name="automate-email-notifications-for-automatic-tuning-recommendations"></a>E-posta bildirimleri otomatik ayarlama önerileri için otomatik hale getirme

Aşağıdaki çözüm otomatik ayarlama önerileri içeren e-posta bildirimleri gönderme otomatikleştirir. Açıklanan çözümünü kullanarak ayarlama önerileri almak için bir PowerShell Betiği yürütülmesini otomatikleştirme oluşur [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro), ve zamanlama Otomasyon e-posta teslim işlemini kullanarak [Microsoft Flow ](https://flow.microsoft.com).

## <a name="create-azure-automation-account"></a>Azure Automation hesabı oluşturma

Azure Otomasyonu, ilk adım bir automation hesabı oluşturma ve PowerShell komut dosyası yürütme için kullanılacak Azure kaynakları ile yapılandırmak için kullanmaktır. Azure Automation ve kendi özellikleri hakkında daha fazla bilgi için bkz: [Azure automation ile çalışmaya başlama](https://docs.microsoft.com/en-us/azure/automation/automation-offering-get-started).

Seçme ve Otomasyon uygulama marketten yapılandırma yöntemiyle Azure Automation hesabını oluşturmak için aşağıdaki adımları izleyin:

- Azure portalında oturum açın
- Tıklayın "**+ kaynak oluşturma**" sol üst köşedeki
- Arama "**Otomasyon**" (enter tuşuna basın)
- Arama sonuçlarında Otomasyon uygulama tıklayın

![Azure Otomasyonu ekleme](./media/sql-database-automatic-tuning-email-notifications/howto-email-01.png)

- "Bir Otomasyon hesabı oluştur" bölmesi içinde bir kez tıklayın "**oluşturma**"
- Gerekli bilgileri doldurun: Bu Otomasyon hesabı için bir ad girin, PowerShell Betiği yürütme için kullanılacak Azure abonelik kimliği ve Azure kaynaklarınızı seçin
- İçin "**oluşturma Azure farklı çalıştır hesabı**" seçeneği için **Evet** altında hangi PowerShell komut dosyasını çalıştırır Azure Otomasyonu yardımıyla hesap türünü yapılandırmak için. Hesap türleri hakkında daha fazla bilgi için bkz: [farklı çalıştır hesabı](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account)
- Otomasyon hesabı oluşturma tıklayarak sonuçlandırmak **oluştur**

> [!TIP]
> Azure Otomasyonu hesabı adı, abonelik kimliği ve kaynakları (örneğin, bir not defteri kopyala-yapıştır) Otomasyon uygulama oluşturulurken girerken tam olarak kaydedin. Daha sonra bu bilgileri gerekir.
>

Aynı Otomasyon oluşturmak istediğiniz birden fazla Azure aboneliğiniz varsa, diğer abonelikler için bu işlemi yinelemeniz gerekir.

## <a name="update-azure-automation-modules"></a>Azure Automation modülleri güncelleştir

Otomatik öneri ayarlama almak için PowerShell betiğini kullanır [Get-AzureRmResource](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Resources/Get-AzureRmResource) ve [Get-AzureRmSqlDatabaseRecommendedAction](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Sql/Get-AzureRmSqlDatabaseRecommendedAction) için Azure modüllerin hangi güncelleştirme komutları sürüm 4 ve üzeri gereklidir.

Azure PowerShell modülleri güncelleştirmek için aşağıdaki adımları izleyin:

- Otomasyon uygulama bölmesinde erişin ve seçin "**modülleri**" taraftaki menüsünde (Bu menü öğesini paylaşılan kaynaklar altında olduğu gibi ilerleyin).
- Modüller bölmesindeki tıklayın "**güncelleştirme Azure modülleri**" üstünde ve "Azure modülleri güncelleştirildi" iletisi görüntülenir kadar bekleyin. Bu işlemin tamamlanması birkaç dakika sürebilir.

![Azure automation modülleri güncelleştir](./media/sql-database-automatic-tuning-email-notifications/howto-email-02.png)

Gerekli sürümü sürüm 4 olması AzureRM.Resources ve AzureRM.Sql modülleri gerekiyor ve üstünde.

## <a name="create-azure-automation-runbook"></a>Azure Otomasyonu Runbook'u oluşturma

Sonraki adım, bir Runbook önerileri ayarlama, alma için PowerShell komut dosyası içinde bulunduğu Azure Otomasyonu'nda oluşturmaktır.

Yeni bir Azure Otomasyonu runbook oluşturmak için aşağıdaki adımları izleyin:

- Önceki adımda oluşturduğunuz Azure Automation hesabını erişim
- Otomasyon hesabı bölmesinde, bir kez tıklayın "**Runbook'lar**" PowerShell Betiği ile yeni bir Azure Otomasyonu runbook oluşturmak için sol taraftaki menü öğesi. Otomasyon runbook'larınızı oluşturma hakkında daha fazla bilgi için bkz: [yeni bir runbook oluşturma](../automation/automation-creating-importing-runbook.md).
- Yeni bir runbook eklemek için tıklayın "**+ runbook Ekle**" menü seçeneğini ve ardından üzerinde "**hızlı oluştur – yeni bir runbook oluşturmak**".
- Runbook bölmesinde runbook'unuz adını yazın (Bu örnekte, amacıyla "**AutomaticTuningEmailAutomation**" kullanılır), runbook türünü seçin **PowerShell** ve bir açıklamasını yazın amacını tanımlamak için bu runbook.
- Tıklayın **oluşturma** yeni bir runbook oluşturmayı tamamlamak için düğmesi

![Azure Otomasyonu runbook Ekle](./media/sql-database-automatic-tuning-email-notifications/howto-email-03.png)

Bir PowerShell Betiği oluşturulan runbook içine yüklemek için aşağıdaki adımları izleyin:

- İçindeki "**PowerShell Runbook'u düzenlemek**"bölmesinde,"**RUNBOOK'lar**" menüsünde ağacı ve görünümü, runbook adı görene kadar genişletin (Bu örnekte " **AutomaticTuningEmailAutomation**"). Bu runbook'u seçin.
- "PowerShell (numarası 1'den başlayarak) Runbook'unu Düzenle" ilk satırında kopyala-yapıştır aşağıdaki PowerShell komut dosyası kodu. Bu PowerShell Betiği olarak sağlanan-başlamanıza yardımcı olmaktır. Kodun paketine gereksinimlerinizi değiştirin.

Verilen PowerShell Betiği üstbilgisinde değiştirmeniz gerekiyor. `<SUBSCRIPTION_ID_WITH_DATABASES>` Azure abonelik kimliğinizi içeren Azure abonelik Kimliğinizi alma konusunda bilgi almak için bkz: [Azure abonelik GUID alma](https://blogs.msdn.microsoft.com/mschray/2016/03/18/getting-your-azure-subscription-guid-new-portal/).

Birkaç abonelikleri durumunda bunları virgülle ayrılmış komut dosyasının üstbilgisindeki "$subscriptions" özelliğine ekleyebilirsiniz.

```PowerShell
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
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Define the resource types
$resourceTypes = ("Microsoft.Sql/servers/databases")
$advisors = ("CreateIndex", "DropIndex");
$results = @()

# Loop through all subscriptions
foreach($subscriptionId in $subscriptions) {    
    Select-AzureRmSubscription -SubscriptionId $subscriptionId    
    $rgs = Get-AzureRmResourceGroup

    # Loop through all resource groups
    foreach($rg in $rgs) {
        $rgname = $rg.ResourceGroupName;

        # Loop through all resource types
        foreach($resourceType in $resourceTypes) {
            $resources = Get-AzureRmResource -ResourceGroupName $rgname -ResourceType $resourceType    

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
                    $recs = Get-AzureRmSqlDatabaseRecommendedAction -ResourceGroupName $ResourceGroupName -ServerName $ServerName  -DatabaseName $DatabaseName -AdvisorName $advisor
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

Tıklayın "**kaydetmek**" komut dosyasını kaydetmek için sağ üst köşesinde düğmesini. Komut dosyasıyla memnun kaldığınızda, tıklayın "**Yayımla**" Bu runbook'u yayımlamak için düğmesi. 

Ana runbook bölmesinde tıklayın seçebilirsiniz "**Başlat**" düğmesine **test** komut dosyası. Tıklayın "**çıkış**" yürütülen betik sonuçlarını görüntülemek için. Bu çıktı, e-posta içeriğini olacak. Komut dosyası örnek çıkışı aşağıdaki ekran görüntüsünde görülebilir.

![Görünümü Azure Automation ile ilgili öneriler ayarlama otomatik çalıştırma](./media/sql-database-automatic-tuning-email-notifications/howto-email-04.png)

PowerShell Betiği gereksinimlerinize özelleştirerek içeriği ayarlamak için emin olun.

Yukarıdaki adımları ile Azure Otomasyonu'nda otomatik ayarlama önerileri almak için PowerShell betiğini yüklenir. Otomatikleştirmek ve e-posta teslim işini zamanlamak için sonraki adım olacaktır.

## <a name="automate-the-email-jobs-with-microsoft-flow"></a>Microsoft Flow e-posta işleriyle otomatikleştirme

Çözüm son adım olarak tamamlamak için Microsoft Flow üç eylemden (işler) oluşan bir Otomasyon akışı oluşturun: 

1. "**- Azure Otomasyonu işi oluşturma**" – Azure Otomasyon runbook'u içinde önerileri ayarlama otomatik almak için PowerShell betiğini çalıştırmak için kullanılır
2. "**Azure Otomasyonu - Get iş çıktısı**" – yürütülen PowerShell komut dosyasından çıkış almak için kullanılır
3. "**Office 365 Outlook – bir e-posta Gönder**" – e-posta göndermek için kullanılır. E-posta akışını oluşturma tek Office 365 hesabı kullanılarak kullanıma gönderilir.

Microsoft Flow özellikleri hakkında daha fazla bilgi edinmek için [Microsoft Flow Başlarken](https://docs.microsoft.com/en-us/flow/getting-started).

Bu adım için önkoşuldur kaydolmak için [Microsoft Flow](https://flow.microsoft.com) hesabı ile oturum açın. Bir kez çözüm içinde ayarlamak için bu adımları bir **yeni akış**:

- Erişim "**My akışları**" menü öğesi
- My akışları içinde seçin "**+ Oluştur boş**" sayfanın üst kısmındaki bağlantı
- Bağlantıyı "**bağlayıcılar ve Tetikleyicileri yüzlerce Ara**" sayfanın sonundaki
- Arama alanı türü "**yineleme**" seçin "**çizelgesi - yinelenme**" e-posta teslim işin çalışmasını zamanlamak için Arama sonuçlarından.
- Sıklık alan yinelenme bölmesinde yürütmek, gönderme otomatik gibi e-posta her dakika, saat, gün, hafta, vb. Bu akış için zamanlama sıklığını seçin.

Sonraki adım, yeni oluşturulan yinelenen akışına (get çıktı ve Gönder'e-posta oluşturma,) üç iş eklemektir. Gerekli iş akışına ekleme gerçekleştirmek için şu adımları izleyin:

1. Ayarlama önerileri almak için PowerShell betiğini çalıştırmak için eylem oluşturun
- Seçin "**+ yeni adım**", ardından"**Eylem Ekle**" yineleme akışı bölmesinde içinde
- Arama alanı türü "**Otomasyon**"ve seçin"**Azure Otomasyonu – oluşturma işi**" Arama sonuçlarından
- Oluşturma işi bölmesinde iş özelliklerini yapılandırın. Bu yapılandırma, Azure abonelik kimliği, kaynak grubu ve Automation hesabı ayrıntılarını gerekir **daha önce kaydedilen** adresindeki **Otomasyon hesabı bölmesi**. Bu bölümdeki kullanılabilir seçenekler hakkında daha fazla bilgi edinmek için [Azure Otomasyonu - işi oluştur](https://docs.microsoft.com/connectors/azureautomation/#Create_job).
- Bu eylem tıklayarak oluşturmayı tamamlamak "**Kaydet akışının**"

2. Yürütülen PowerShell komut dosyasından çıkış almak için eylem oluşturun
- Seçin "**+ yeni adım**", ardından"**Eylem Ekle**" yineleme akışı bölmesinde içinde
- Aramada türü Dosyalanan "**Otomasyon**"ve seçin"**Azure Otomasyonu – Get iş çıktısı**" Arama sonuçlarından. Bu bölümdeki kullanılabilir seçenekler hakkında daha fazla bilgi edinmek için [Azure Otomasyonu – Get iş çıktısı](https://docs.microsoft.com/connectors/azureautomation/#Get_job_output).
- Doldur (önceki işi oluşturmak için benzer) gerekli alanları - Azure abonelik kimliği, kaynak grubu ve Automation hesabı (Automation hesabı Bölmesi'nde girilen gibi) doldurun
- Alanının içini tıklatın "**iş kimliği**" için "**dinamik içerik**" menü görünmesini sağlar. Bu menü içinde seçeneğini seçin "**iş kimliği**".
- Bu eylem tıklayarak oluşturmayı tamamlamak "**Kaydet akışının**"

3. Office 365 tümleştirmesi kullanarak e-posta göndermek için eylem oluşturun
- Seçin "**+ yeni adım**", ardından"**Eylem Ekle**" yineleme akışı bölmesinde içinde
- Aramada türü Dosyalanan "**bir e-posta Gönder**"ve seçin"**Office 365 Outlook – bir e-posta Gönder**" Arama sonuçlarından
- İçindeki "**için**" alanı için size gereken bildirim e-posta göndermek e-posta adresini yazın
- İçindeki "**konu**" alanı bu konu, e-posta, örneğin "otomatik ayarlama önerileri e-posta bildirimi" türü
- Alanının içini tıklatın "**gövde**" için "**dinamik içerik**" menü görünmesini sağlar. Öğesinden bu menü içinde altında "**iş çıktısı alma**", seçin"**içerik**" 
- Bu eylem tıklayarak oluşturmayı tamamlamak "**Kaydet akışının**"

> [!TIP]
> Farklı alıcıya otomatik e-postalar göndermek için ayrı akışları oluşturun. Bu ek akışlarında "İçin" alanındaki alıcı e-posta adresi ve "Subject" alanında e-posta konu satırı değiştirin. PowerShell komut dosyalarını özelleştirilmiş Azure Automation ile yeni runbook'lar oluşturma (gibi Azure abonelik kimliği değişikliği ile) daha fazla özelleştirme sağlar Otomatik senaryoları, gibi örneğin otomatik ayarlama üzerinde ayrı alıcıların e-postayla gönderme ayrı abonelikler için öneriler sunar.
>

Yukarıdaki e-posta teslim iş iş akışını yapılandırmak için gerekli adımları sonlanır. Aşağıdaki görüntüde yerleşik üç eylemden oluşan tüm akışı gösterilmektedir.

![E-posta bildirimleri akışı ayarlama otomatik görünümü](./media/sql-database-automatic-tuning-email-notifications/howto-email-05.png)

Akış test etmek için tıklayın "**Şimdi Çalıştır**" akışı bölmesinde içinde sağ üst köşesindeki.

Çalışan çıkışı, gönderilen e-posta bildirimleri başarısını gösteren otomatik işi istatistikleri akış analizi bölmesinden görülebilir.

![Otomatik ayarlama e-posta bildirimleri için akışı çalıştırma](./media/sql-database-automatic-tuning-email-notifications/howto-email-06.png)

Akış analizi işi yürütmeleri, başarı izlemek için yardımcı olur ve sorun giderme için gerekiyorsa.  Sorun giderme söz konusu olduğunda, ayrıca PowerShell komut dosyası yürütme günlüğü Azure Otomasyonu uygulama erişilebilir incelemek istediğiniz.

Otomatik e-posta son çıkışı aşağıdaki e-posta oluşturmak ve bu çözümü çalıştırmak sonra alınan benzer:

![Örnek e-posta otomatik ayarlama e-posta bildirimleri çıktısı](./media/sql-database-automatic-tuning-email-notifications/howto-email-07.png)

PowerShell Betiği ayarlayarak, çıkış ve otomatik e-posta gereksinimlerinize biçimlendirmesini ayarlayabilirsiniz.

Daha fazla temelinde belirli bir ayar olay ve birden çok abonelikleri veya özel senaryolarınızı bağlı olarak veritabanları için birden çok alıcıya e-posta bildirimleri yapı çözümü özelleştirme. 

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla üzerinde otomatik ayarlama veritabanı performansını geliştirmek için bkz: yardımcı olabileceğini öğrenin [Azure SQL veritabanı'nda otomatik ayarlama](sql-database-automatic-tuning.md).
- Otomatik İş yükünüzün yönetmek için Azure SQL veritabanında ayarlamayı etkinleştirmek için bkz: [otomatik ayarlamayı etkinleştirmek](sql-database-automatic-tuning-enable.md).
- El ile gözden geçirin ve öneriler ayarlama otomatik uygulamak için bkz: [bulmak ve performans önerileri uygulamak](sql-database-advisor-portal.md).
