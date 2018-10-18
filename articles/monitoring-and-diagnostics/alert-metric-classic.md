---
title: Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Klasik ölçüm uyarılarını yönetme
description: Azure portal, CLI veya Powershell oluşturun, görüntüleyin ve klasik ölçüm uyarı kuralları yönetmek için kullanmayı öğrenin.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: ffac9511ab93367cba3f006249bd92658a520e09
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49386952"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Klasik ölçüm uyarılarını yönetme

Azure İzleyici'de klasik ölçüm uyarılarını bildirim almak için bir yol sağlayan bir eşiği ölçümlerinizi birini çapraz zaman. Klasik ölçüm uyarıları yalnızca boyutsuz ölçümler üzerinde uyarı veren bir eski işlevi olur. Klasik ölçüm uyarıları gelişmiş işlevselliği olan ölçüm uyarıları adlı bir var olan yeni işlevi yoktur. Yeni ölçüm uyarıları işlevleri hakkında daha fazla bilgi [ölçüm uyarıları genel bakış](alert-metric-overview.md). Bu makalede, biz oluşturun, görüntüleyin ve klasik ölçüm uyarı kuralları Azure Portalı aracılığıyla Azure CLI ve Powershell yönetme konusunda açıklanmıştır.

## <a name="with-azure-portal"></a>Azure portalı ile

1. İçinde [portalı](https://portal.azure.com/), izlemek istediğiniz olan kaynağı bulun ve seçin.

2. İçinde **izleme** bölümünden **uyarılar (Klasik)**. Simge ve metin, farklı kaynaklar için biraz farklı olabilir. Bulamazsanız, **uyarılar (Klasik)** içinde burada bulabileceğiniz **uyarılar** veya **uyarı kuralları**.

    ![İzleme](./media/alerts-metric-classic/AlertRulesButton.png)

3. Seçin **ölçüm uyarısı Ekle (Klasik)** komutunu ve ardından alanları doldurun.

    ![Uyarı Ekle](./media/alerts-metric-classic/AddAlertOnlyParamsPage.png)

4. **Adı** , uyarı kuralı. Ardından bir **açıklama**, bildirim e-postalarda da görünen.

5. Seçin **ölçüm** izlemek istediğiniz. Ardından bir **koşul** ve **eşiği** ölçüm için değer. Ayrıca **süresi** uyarı tetiklenmeden önce ölçüm kuralının karşılanması gereken süre. Örneğin, CPU'nun % 80 5 dakika boyunca sürekli olarak yukarıdaki olduğunda "son 5 dakika boyunca" dönem kullanın ve % 80 üzerinde bir CPU için Uyarınız görünürse, bir uyarı tetikler. Sonra ilk tetikleyici gerçekleşir, CPU, 5 dakika boyunca % 80 aşağısına kaldığında oluşan tekrar tetikler. CPU ölçüm ölçüsü dakika başı gerçekleşir.

6. Seçin **e-posta sahipleri...**  yöneticileri ve ortak yöneticilerin uyarı tetiklendiğinde e-posta bildirimleri almak istiyorsanız.

7. Uyarı tetiklendiğinde ek e-posta adreslerine bildirim gönder istiyorsanız, bunları eklemek **ek yönetici email(s)** alan. Birden çok e-postalar aşağıdaki biçimde bir noktalı virgül ile ayırın:  *email@contoso.com;email2@contoso.com*

8. Geçerli bir URI koymak **Web kancası** uyarı tetiklendiğinde çağrılacak istiyorsanız alan.

9. Azure Otomasyonu kullanıyorsanız, uyarı tetiklendiğinde çalıştırılacak bir runbook'u seçebilirsiniz.

10. Seçin **Tamam** uyarı oluşturmak için.

Birkaç dakika içinde uyarı etkin ve daha önce açıklandığı gibi tetikler.

Bir uyarı oluşturduktan sonra seçin ve aşağıdaki görevlerden birini gerçekleştirebilirsiniz:

* Ölçüm eşiği ve gerçek değerler önceki gün gösteren bir grafiği görüntüleyin.
* Düzenleyin veya silin.
* **Devre dışı** veya **etkinleştirme** , geçici olarak durdurmak veya bu uyarı için bildirimleri almaya devam etmek istiyorsanız.

## <a name="with-azure-cli"></a>Azure CLI ile

Önceki bölümlerde oluşturun, görüntüleyin ve Azure portalını kullanarak ölçüm uyarı kuralları yönetmek nasıl kaydedileceği açıklanır. Bu bölümde, platformlar arası kullanarak aynı şeyi nasıl anlatılacaktır [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). Hızlı şekilde kullanmaya başlamak Azure CLI aracılığıyladır [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).

### <a name="get-all-classic-metric-alert-rules-in-a-resource-group"></a>Bir kaynak grubundaki tüm Klasik ölçüm uyarı kuralları alma

```azurecli
az monitor alert list --resource-group <group name>
```

### <a name="see-details-of-a-particular-classic-metric-alert-rule"></a>Bir belirli Klasik ölçüm uyarısı kuralının ayrıntılarına bakın

```azurecli
az monitor alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-classic-metric-alert-rule"></a>Klasik bir ölçüm uyarı kuralı oluşturma

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-a-classic-metric-alert-rule"></a>Klasik bir ölçüm uyarı kuralını Sil

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="with-powershell"></a>PowerShell ile

Bu bölüm, komutları oluşturun, görüntüleyin ve klasik ölçüm uyarılarını yönetme PowerShell'in nasıl kullanılacağı gösterilmektedir. Makaledeki örnekler, Azure İzleyici cmdlet'leri için Klasik ölçüm uyarıları nasıl kullanabileceğinizi gösterir.

1. Henüz yapmadıysanız, bilgisayarınızda çalıştırmak için PowerShell'i ayarlayın. Daha fazla bilgi için [yükleme ve yapılandırma PowerShell](/powershell/azure/overview). Ayrıca Azure İzleyici PowerShell cmdlet'leri listesini gözden geçirebilirsiniz [Azure İzleyici (Öngörüler) cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.insights).

2. İlk olarak, Azure aboneliğinizde oturum açın.

    ```PowerShell
    Connect-AzureRmAccount
    ```

3. Oturum açma ekranı görürsünüz. Hesabınızdaki Tenantıd, bir kez oturum açmak ve varsayılan abonelik kimliği görüntülenir. Tüm Azure cmdlet'lerini varsayılan aboneliğinizi bağlamında çalışır. Erişiminiz Aboneliklerin listesini görüntülemek için aşağıdaki komutu kullanın:

    ```PowerShell
    Get-AzureRmSubscription
    ```

4. Çalışma Bağlamınızı farklı bir aboneliğe değiştirmek için aşağıdaki komutu kullanın:

    ```PowerShell
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

5. Tüm Klasik ölçüm uyarı kuralları bir kaynak grubu üzerinde alabilirsiniz:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest
    ```

6. Klasik bir ölçüm uyarısı kuralının ayrıntılarını görüntüleyebilirsiniz.

    ```PowerShell
    Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. Bir hedef kaynak için uyarı kurallarının tümünü alabilir. Örneğin, bir VM'de tüm uyarı kuralları ayarlayın.

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig

8. You can use the `Add-AlertRule` cmdlet to create, update, or disable an alert rule. You can create email and webhook properties using  `New-AzureRmAlertRuleEmail` and `New-AzureRmAlertRuleWebhook`, respectively. In the Alert rule cmdlet, assign these properties as actions to the **Actions** property of the Alert Rule. The following table describes the parameters and values used to create an alert using a metric.

    | parameter | value |
    | --- | --- |
    | Name |simpletestdiskwrite |
    | Location of this alert rule |East US |
    | ResourceGroup |montest |
    | TargetResourceId |/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
    | MetricName of the alert that is created |\PhysicalDisk(_Total)\Disk Writes/sec. See the `Get-MetricDefinitions` cmdlet about how to retrieve the exact metric names |
    | operator |GreaterThan |
    | Threshold value (count/sec in for this metric) |1 |
    | WindowSize (hh:mm:ss format) |00:05:00 |
    | aggregator (statistic of the metric, which uses Average count, in this case) |Average |
    | custom emails (string array) |'foo@example.com','bar@example.com' |
    | send email to owners, contributors and readers |-SendToServiceOwners |

9. Create an Email action

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    ```

10. Bir Web kancası Eylem oluştur

    ```PowerShell
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Resource Manager şablonu ile klasik ölçüm uyarısı oluşturma](monitoring-enable-alerts-using-template.md).
- [Web kancası kullanarak bir Azure sistem bilgisini Klasik ölçüm uyarısı sahip](insights-webhooks-alerts.md).

