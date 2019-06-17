---
title: Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Klasik ölçüm uyarılarını yönetme
description: Azure portal, CLI veya Powershell oluşturun, görüntüleyin ve klasik ölçüm uyarı kuralları yönetmek için kullanmayı öğrenin.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.openlocfilehash: 4a225dbc8e84d65a6ea25f63627599e5bb7d2ced
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129767"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Klasik ölçüm uyarılarını yönetme

Azure İzleyici'de klasik ölçüm uyarılarını bildirim almak için bir yol sağlayan bir eşiği ölçümlerinizi birini çapraz zaman. Klasik ölçüm uyarıları yalnızca boyutsuz ölçümler üzerinde uyarı veren bir eski işlevi olur. Klasik ölçüm uyarıları gelişmiş işlevselliği olan ölçüm uyarıları adlı bir var olan yeni işlevi yoktur. Yeni ölçüm uyarıları işlevleri hakkında daha fazla bilgi [ölçüm uyarıları genel bakış](../../azure-monitor/platform/alerts-metric-overview.md). Bu makalede, biz oluşturun, görüntüleyin ve klasik ölçüm uyarı kuralları Azure Portalı aracılığıyla Azure CLI ve Powershell yönetme konusunda açıklanmıştır.

## <a name="with-azure-portal"></a>Azure portalı ile

1. İçinde [portalı](https://portal.azure.com/), izlemek istediğiniz olan kaynağı bulun ve seçin.

2. İçinde **izleme** bölümünden **uyarılar (Klasik)** . Simge ve metin, farklı kaynaklar için biraz farklı olabilir. Bulamazsanız, **uyarılar (Klasik)** içinde burada bulabileceğiniz **uyarılar** veya **uyarı kuralları**.

    ![İzleme](media/alerts-classic-portal/AlertRulesButton.png)

3. Seçin **ölçüm uyarısı Ekle (Klasik)** komutunu ve ardından alanları doldurun.

    ![Uyarı Ekle](media/alerts-classic-portal/AddAlertOnlyParamsPage.png)

4. **Adı** , uyarı kuralı. Ardından bir **açıklama**, bildirim e-postalarda da görünen.

5. Seçin **ölçüm** izlemek istediğiniz. Ardından bir **koşul** ve **eşiği** ölçüm için değer. Ayrıca **süresi** uyarı tetiklenmeden önce ölçüm kuralının karşılanması gereken süre. Örneğin, CPU'nun % 80 5 dakika boyunca sürekli olarak yukarıdaki olduğunda "son 5 dakika boyunca" dönem kullanın ve % 80 üzerinde bir CPU için Uyarınız görünürse, bir uyarı tetikler. Sonra ilk tetikleyici gerçekleşir, CPU, 5 dakika boyunca % 80 aşağısına kaldığında oluşan tekrar tetikler. CPU ölçüm ölçüsü dakika başı gerçekleşir.

6. Seçin **e-posta sahipleri...**  yöneticileri ve ortak yöneticilerin uyarı tetiklendiğinde e-posta bildirimleri almak istiyorsanız.

7. Uyarı tetiklendiğinde ek e-posta adreslerine bildirim gönder istiyorsanız, bunları eklemek **ek yönetici email(s)** alan. Birden çok e-postalar aşağıdaki biçimde bir noktalı virgül ile ayırın: *e-posta\@contoso.com;email2\@contoso.com*

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

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu bölüm, komutları oluşturun, görüntüleyin ve klasik ölçüm uyarılarını yönetme PowerShell'in nasıl kullanılacağı gösterilmektedir. Makaledeki örnekler, Azure İzleyici cmdlet'leri için Klasik ölçüm uyarıları nasıl kullanabileceğinizi gösterir.

1. Henüz yapmadıysanız, bilgisayarınızda çalıştırmak için PowerShell'i ayarlayın. Daha fazla bilgi için [yükleme ve yapılandırma PowerShell](/powershell/azure/overview). Ayrıca Azure İzleyici PowerShell cmdlet'leri listesini gözden geçirebilirsiniz [Azure İzleyici (Öngörüler) cmdlet'leri](https://docs.microsoft.com/powershell/module/az.applicationinsights).

2. İlk olarak, Azure aboneliğinizde oturum açın.

    ```powershell
    Connect-AzAccount
    ```

3. Oturum açma ekranı görürsünüz. Hesabınızdaki Tenantıd, bir kez oturum açmak ve varsayılan abonelik kimliği görüntülenir. Tüm Azure cmdlet'lerini varsayılan aboneliğinizi bağlamında çalışır. Erişiminiz Aboneliklerin listesini görüntülemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzSubscription
    ```

4. Çalışma Bağlamınızı farklı bir aboneliğe değiştirmek için aşağıdaki komutu kullanın:

    ```powershell
    Set-AzContext -SubscriptionId <subscriptionid>
    ```

5. Tüm Klasik ölçüm uyarı kuralları bir kaynak grubu üzerinde alabilirsiniz:

    ```powershell
    Get-AzAlertRule -ResourceGroup montest
    ```

6. Klasik bir ölçüm uyarısı kuralının ayrıntılarını görüntüleyebilirsiniz.

    ```powershell
    Get-AzAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. Bir hedef kaynak için uyarı kurallarının tümünü alabilir. Örneğin, bir VM'de tüm uyarı kuralları ayarlayın.

    ```powershell
    Get-AzAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
    ```

8. Klasik bir uyarı kuralları artık PowerShell oluşturulabilir. Yeni kullanması gereken bir uyarı kuralı oluşturmak için ['AzMetricAlertRule Ekle'](/powershell/module/az.monitor/add-azmetricalertrule) komutu.

## <a name="next-steps"></a>Sonraki adımlar

- [Resource Manager şablonu ile klasik ölçüm uyarısı oluşturma](../../azure-monitor/platform/alerts-enable-template.md).
- [Web kancası kullanarak bir Azure sistem bilgisini Klasik ölçüm uyarısı sahip](../../azure-monitor/platform/alerts-webhooks.md).
