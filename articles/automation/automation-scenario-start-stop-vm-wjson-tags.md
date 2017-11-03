---
title: "Azure VM durumunu zamanlamak için JSON biçimli etiketler kullanın | Microsoft Docs"
description: "Bu makalede, JSON dizeler etiketlerini VM başlatma ve kapatma zamanlama otomatikleştirmek için nasıl kullanılacağı gösterilmektedir."
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: cae4020741003be54b133efa121b3c09b859a176
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-to-create-a-schedule-for-azure-vm-startup-and-shutdown"></a>Azure otomasyonu senaryosu: Azure VM başlatma ve kapatma için bir zamanlama oluşturmak için JSON biçimli etiketleri kullanma
Müşteriler genellikle başlatma ve kapatma abonelik maliyetlerini azaltmak veya işletme ve teknik gereksinimleri desteği yardımcı olmak için sanal makinelerin zamanlamak ister.

Aşağıdaki senaryoda, bir kaynak grubu düzeyinde veya azure'da sanal makine düzeyinde Schedule adlı bir etiket kullanarak otomatik başlatma ve kapatma, VM'lerin ayarlamak sağlar. Bu zamanlamayı Pazar Cumartesi için bir başlangıç saati ve kapatma saati ile yapılandırılabilir.

Biz, bazı Giden kutusu seçeneğiniz vardır. Bunlar:

* [Sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) etkinleştirmeniz veya ölçeklendirmek otomatik ölçeklendirme ayarları.
* [DevTest Labs](../devtest-lab/devtest-lab-overview.md) hizmetini başlatma ve kapatma işlemleri planlama yerleşik özelliğine sahiptir.

Ancak, bu yalnızca belirli senaryolar destek seçenekleri ve altyapı olarak-hizmet (Iaas) VM'ler için uygulanamaz.

Bir kaynak grubuna zamanlama etiket uygulandığında, bu kaynak grubu içindeki tüm sanal makinelere de uygulanır. Bir zamanlama da doğrudan bir VM'ye uyguladıysanız, son zamanlama önceliği aşağıdaki sırayla alır:

1. Bir kaynak grubuna uygulanan zamanlama
2. Bir kaynak grubu ve kaynak grubundaki sanal makine uygulanan zamanlama
3. Bir sanal makineye uygulanan zamanlama

Bu senaryo, aslında bir JSON dizesinde belirtilen biçiminde alır ve değeri olarak ekler Schedule adlı bir etiket için. Ardından bir runbook'un tüm kaynak grupları ve sanal makineleri listeler ve daha önce listelenen senaryolarını temel alarak her VM için zamanlamaları tanımlar. Sonraki zamanlamaları bağlı olan sanal makineleri döngüler ve ne yapılması değerlendirir. Örneğin, bu sanal makineleri durduruldu, kapatmak veya göz ardı gereken belirler.

Bu runbook'ları kullanarak kimlik doğrulaması [Azure farklı çalıştır hesabı](automation-sec-configure-azure-runas-account.md).

## <a name="download-the-runbooks-for-the-scenario"></a>Senaryo için runbook'ları indirme
Bu senaryo indirebileceğiniz dört PowerShell iş akışı runbook oluşur [TechNet Galerisi](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) veya [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) bu proje için depo.

| Runbook | Açıklama |
| --- | --- |
| Test-ResourceSchedule |Her sanal makine zamanlamayı denetler ve kapanması veya başlatılması zamanlamaya bağlı olarak gerçekleştirir. |
| Ekleme ResourceSchedule |Zamanlama etiketi bir VM veya kaynak grubuna ekler. |
| Güncelleştirme ResourceSchedule |Varolan bir zamanlamayı etiketi, yeni bir tane ile değiştirerek değiştirir. |
| Remove-ResourceSchedule |Zamanlama etiketi bir VM veya kaynak grubundan kaldırır. |

## <a name="install-and-configure-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma
### <a name="install-and-publish-the-runbooks"></a>Runbook yükleme ve yayımlama
Runbook'ları indirdikten sonra bunları yordamı kullanarak içeri aktarabilirsiniz [oluşturma veya bir Azure Otomasyonu runbook'u içeri aktarma](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).  Otomasyon hesabınızda başarıyla alındıktan sonra her runbook yayımlayın.

### <a name="add-a-schedule-to-the-test-resourceschedule-runbook"></a>Test-ResourceSchedule runbook'a bir zamanlama Ekle
Test-ResourceSchedule runbook zamanlamasını etkinleştirmek için aşağıdaki adımları izleyin. Hangi sanal makinelerin başlatıldı, kapatmak veya olarak sol doğrular runbook budur.

1. Azure portalından Automation hesabınızı açın ve ardından **Runbook'lar** döşeme.
2. Üzerinde **Test ResourceSchedule** dikey penceresinde tıklatın **zamanlamaları** döşeme.
3. Üzerinde **zamanlamaları** dikey penceresinde tıklatın **bir zamanlama Ekle**.
4. Üzerinde **zamanlamaları** dikey penceresinde, select **runbook'a bir zamanlama Bağla**. Ardından **yeni bir zamanlama oluşturmak**.
5. Üzerinde **yeni zamanlama** dikey penceresinde, bu zamanlamayı adını yazın örneğin: *HourlyExecution*.
6. Zamanlama için **Başlat**, başlangıç saati bir saat artışı ayarlayın.
7. Seçin **yineleme**ve ardından **her aralığı yinelenmesini**seçin **1 saat**.
8. Doğrulayın **sona erme süresini ayarlamanıza** ayarlanır **Hayır**ve ardından **oluşturma** yeni zamanlamanızı kaydetmek için.
9. Üzerinde **zamanlama Runbook** seçenekleri dikey penceresinde, select **parametreler ve çalıştırma ayarları**. Test ResourceSchedule içinde **parametreleri** dikey penceresinde, aboneliğinizde adını girin **varlığıyla SubscriptionName** alan.  Bu runbook için gerekli olan tek bir parametredir.  İşiniz bittiğinde tıklatın **Tamam**.

Tamamlandığında runbook zamanlaması aşağıdaki gibi görünmelidir:

![Yapılandırılmış Test ResourceSchedule runbook](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-the-json-string"></a>Biçim JSON dizesi
Bu çözüm, JSON ile belirtilen bir biçim dizesi ve bir etiket için değer olarak ekler alır zamanlama temel olarak çağrılır. Ardından bir runbook'un tüm kaynak grupları ve sanal makineleri listeler ve her bir sanal makine için zamanlamalar tanımlar.

Runbook bağlı zamanlamaları sahip sanal makineler üzerinde döngüler ve hangi eylemleri alınıp alınmayacağını denetler. Çözümler nasıl biçimlendirilmiş bir örnek verilmiştir:

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

Bu yapı hakkında ayrıntılı bazı bilgiler aşağıdadır:

1. Bu JSON yapısındaki biçimi Azure içindeki bir tek etiket değeri 256 karakterlik sınırlamaya geçici olarak çözmek için optimize edilmiştir.
2. *TZID* sanal makinenin saat dilimini temsil eder. Bu kodu bir PowerShell oturumu--içinde Timezoneınfo .NET sınıfını kullanarak elde edilebilir**[System.TimeZoneInfo]:: GetSystemTimeZones()**.

   ![PowerShell'de GetSystemTimeZones](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * Haftanın günü altı sıfır sayısal değeri ile temsil edilir. Sıfır değeri Pazar eşittir.
   * Başlangıç saati ile temsil edilen **S** özniteliği ve değerini 24 saat biçiminde değil.
   * Son veya kapatma süresi ile temsil edilen **E** özniteliği ve değerini 24 saat biçiminde değil.

     Varsa **S** ve **E** her özniteliklere sahip bir değeri sıfır (0), sanal makine değerlendirme aynı anda mevcut durumda kalır.
3. Haftanın belirli bir gün için değerlendirme atlamak istiyorsanız, bir bölümü, haftanın günü için eklemeyin. Aşağıdaki örnekte, yalnızca Pazartesi değerlendirilir ve haftanın günlerini göz ardı edilir:

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a>Etiket kaynak grupları veya VM'ler
Sanal makineleri kapatmaya VM'ler veya içinde bulunduğu oldukları kaynak grupları etiket gerekir. Bir zamanlama etiketi sahip olmayan sanal makineleri değerlendirilmez. Bu nedenle, bunlar çalışmaya veya kapat.

Bu çözümle etiketi kaynak grupları ya da sanal makineleri iki yolu vardır. Bu doğrudan portalından yapabilirsiniz. Veya Ekle ResourceSchedule, güncelleştirme ResourceSchedule ve Kaldır-ResourceSchedule runbook'ları kullanabilirsiniz.

### <a name="tag-through-the-portal"></a>Portalı aracılığıyla etiketi
Bir sanal makine ya da kaynak grubu portalda etiketlemek için şu adımları izleyin:

1. JSON dizesindeki düzleştirmek ve boşluk olmayan doğrulayın.  JSON dizenizi aşağıdaki gibi görünmelidir:

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. Seçin **etiketi** Bu zamanlamanın uygulamak bir VM veya kaynak grubu simgesi.

   ![VM etiketi seçeneği](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. Etiketler, bir anahtar/değer çifti aşağıdaki tanımlanır. Tür **zamanlama** içinde **anahtar** alan ve JSON dizeye yapıştırın **değeri** alan. **Kaydet** düğmesine tıklayın. Yeni etiket şimdi kaynağınız için etiketler listesinde gösterilmelidir.

   ![VM zamanlama etiketi](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a>PowerShell etiketinden
Tüm içeri aktarılan runbook'lar runbook'ları doğrudan Powershell'den yürütmek açıklar komut dosyasının başında Yardım bilgilerini içerir. Add-ScheduleResource ve güncelleştirme ScheduleResource runbook'lar Powershell'den çağırabilirsiniz. Bunun için bir VM veya kaynak grubunu portal dışında zamanlama etiketinde güncelle sağlayan gerekli parametreleri geçirerek.

Oluşturmak için Ekle ve PowerShell, ilk gerek aracılığıyla etiketleri silmek [için Azure PowerShell ortamınızı ayarlayın](/powershell/azure/overview). Kurulum tamamlandıktan sonra aşağıdaki adımlarla devam edebilirsiniz.

### <a name="create-a-schedule-tag-with-powershell"></a>PowerShell ile bir zamanlama etiket oluşturma
1. Bir PowerShell oturumu açın. Ardından, farklı çalıştır hesabıyla kimlik doğrulaması ve abonelik belirtmek için aşağıdaki örneği kullanın:

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Bir zamanlama karma tablo tanımlayın. Nasıl oluşturulmalıdır örneği şöyledir:

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. Runbook tarafından gerekli parametrelerini tanımlayın. Aşağıdaki örnekte, biz VM hedeflediğiniz:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    Bir kaynak grubu etiketleme kaldırmanız *VMName* $params karma parametresinden tablo şu şekilde:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. Add-ResourceSchedule runbook zamanlama etiket oluşturmak için aşağıdaki parametrelerle çalıştırın:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. Bir kaynak grubunu veya sanal makine etiketi güncelleştirmek için yürütme **güncelleştirme ResourceSchedule** runbook aşağıdaki parametrelerle:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a>PowerShell ile bir zamanlama Etiketi Kaldır
1. Bir PowerShell oturumu açın ve, farklı çalıştır hesabıyla kimlik doğrulaması seçin ve bir aboneliği belirtmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Runbook tarafından gerekli parametrelerini tanımlayın. Aşağıdaki örnekte, biz VM hedeflediğiniz:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    Bir kaynak grubundan bir etiket kaldırıyorsanız kaldırmak *VMName* $params karma parametresinden tablo şu şekilde:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. Zamanlama etiketi kaldırmak için Remove-ResourceSchedule runbook yürütün:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. Bir kaynak grubunu veya sanal makine etiketi güncelleştirmek için aşağıdaki parametreleri Kaldır ResourceSchedule runbook'la yürütün:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> Biz, önceden sanal makinelerinizi kapatıldığını doğrulamak için bu runbook'lar (ve sanal makine durumları) aşağı izlemeniz önerilir ve buna uygun olarak başlatıldı.
>

Azure Portalı'nda Test ResourceSchedule runbook işi ayrıntılarını görüntülemek için seçin **işleri** döşeme runbook'un. İş özetinde giriş parametreleri ve çıkış akışına ek olarak işe ilişkin genel bilgiler ve gerçekleşmişse özel durumlar gösterilir.

**İş Özeti** bölümünde çıkış, uyarı ve hata akışlarından iletiler bulunur. Runbook yürütmeyle ilgili ayrıntılı sonuçları görüntülemek için **Çıkış** kutucuğunu seçin.

![Test ResourceSchedule çıkış](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a>Sonraki adımlar
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md).
* Runbook türleri ve avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz: [Azure Automation runbook türleri](automation-runbook-types.md).
* PowerShell betik desteği özellikleri hakkında daha fazla bilgi için bkz: [Azure automation'da yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* Runbook günlüğü ve çıktı hakkında daha fazla bilgi için bkz: [Runbook çıkışı ve iletileri Azure Automation](automation-runbook-output-and-messages.md).
* Bir Azure farklı çalıştır hesabı ve bunu kullanarak runbook'larınızın kimliklerini nasıl hakkında daha fazla bilgi için bkz: [runbook'ları Azure farklı çalıştır hesabıyla kimlik doğrulaması](automation-sec-configure-azure-runas-account.md).
