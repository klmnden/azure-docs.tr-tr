---
title: Kurtarma Hizmetleri kasası farklı Azure abonelikleri veya başka bir kaynak grubuna taşıma
description: Kurtarma Hizmetleri taşımak için yönergeler, azure abonelik ve kaynak grupları arasında kasası.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 1/4/2019
ms.author: sogup
ms.openlocfilehash: e1df91a11a474faf3a10dbbb7c99ea058037d685
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108149"
---
# <a name="move-a-recovery-services-vault-across-azure-subscriptions-and-resource-groups-limited-public-preview"></a>Azure abonelik ve kaynak gruplarında (sınırlı genel Önizleme) bir kurtarma Hizmetleri kasası Taşı

Bu makalede, Azure abonelikleri genelinde veya başka bir kaynak grubuna aynı abonelikte bulunan Azure Backup için yapılandırıldığı bir kurtarma Hizmetleri kasasına taşımak açıklanmaktadır. Bir kurtarma Hizmetleri kasasına taşımak için Azure portalını veya PowerShell'i kullanabilirsiniz.

## <a name="prerequisites-for-moving-a-vault"></a>Bir kasa taşımak için Önkoşullar

- Kaynak grupları arasında taşırken, hem kaynak kaynak grubu hem de hedef kaynak grubu, işlem sırasında kilitlenir. Taşıma işlemi tamamlanana kadar yazma ve silme işlemleri kaynak gruplarında engellenir.
- Yalnızca Abonelik Yöneticisi bir kasa taşımak için izinlere sahiptir.
- Bir kasa abonelikler arasında taşırken, hedef abonelik etkin durumda bulunmalı ve kaynak abonelik olarak aynı kiracıda olması gerekir.
- Hedef kaynak grubu yazma işlemleri gerçekleştirmek için izniniz olmalıdır.
- Kurtarma Hizmetleri kasasının konumu değiştirilemiyor. Kasa taşımak, yalnızca kaynak grubu değiştirir. Yeni kaynak grubu farklı bir konumda olabilir ancak, kasanın konumu değiştirmez.
- Şu anda bir kurtarma Hizmetleri kasası, bölge başına aynı anda taşıyabilirsiniz.
- VM'yi abonelikler arasında ya da yeni bir kaynak grubu için kurtarma Hizmetleri kasası ile taşınmaz süreleri doluncaya kadar geçerli VM kurtarma noktaları kasaya değişmeden kalır.
- VM veya Kasayla birlikte taşınır olup olmadığını kasadaki tutulan yedekleme geçmişinden her zaman sanal Makinenin geri yükleyebilirsiniz.
-   Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı Azure bölgesindeki ve abonelikte bulunmasını gerektirir.
-   Yönetilen disklere sahip bir sanal makineyi taşımak için bkz [makale](https://azure.microsoft.com/blog/move-managed-disks-and-vms-now-available/).
-   Klasik modelle dağıtılmış kaynakları taşımak için seçenekler, bir Abonelikteki veya yeni bir aboneliğe kaynakları olup taşıyor bağlı olarak değişir. Daha fazla bilgi için bkz. Bu [makale](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources#classic-deployment-limitations).
-   Abonelikler arasında ya da yeni bir kaynak grubu için kasa taşındıktan sonra kasa için tanımlanan yedekleme ilkeleri korunur.
-   Şu anda Abonelikleriniz ve kaynak gruplarınız arasında Azure dosyaları, Azure dosya eşitleme veya SQL Iaas Vm'leri de içeren kasa taşıyamazsınız. Bu senaryolar için destek, gelecek sürümlerde eklenecektir.
-   Abonelikler arasında VM yedekleme verilerini içeren bir kasayı taşırsanız, Vm'lerinizin aynı aboneliğe taşıyın ve yedeklemeler devam etmek için aynı hedef kaynak grubu kullanın.<br>

> [!NOTE]
>
Kurtarma Hizmetleri kasaları ile kullanmak üzere yapılandırılmış **Azure Site Recovery** , henüz taşınamıyor. Herhangi bir VM yapılandırdıysanız (Azure Iaas, Hyper-V, VMware) veya fiziksel makineler için olağanüstü durum kurtarma'yı kullanarak **Azure Site Recovery**, taşıma işlemi engellenir. Site Recovery hizmeti için kaynak taşıma özelliğini henüz kullanılamıyor.
>
>

## <a name="register-the-subscription-to-move-your-recovery-services-vault"></a>Kurtarma Hizmetleri kasasına taşımak için aboneliği kaydedin

Aboneliği kaydetmek için **taşıma** kurtarma Hizmetleri kasasına, PowerShell terminalden aşağıdaki cmdlet'leri çalıştırın:

1. Azure hesabınızda oturum açma

  ```
  Connect-AzureRmAccount
  ```

2.  Kaydetmek istediğiniz aboneliği seçin

    ```
    Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```
3.  Bu aboneliği Kaydet

  ```
  Register-AzureRmProviderFeature -ProviderNamespace Microsoft.RecoveryServices -FeatureName RecoveryServicesResourceMove
  ```

Abonelik taşıma işlemi Azure portal veya PowerShell kullanarak başlamadan önce Güvenilenler listesine eklenmek 30 dakika bekleyin.

## <a name="use-azure-portal-to-move-a-recovery-services-vault-to-different-resource-group"></a>Kurtarma Hizmetleri kasası farklı kaynak grubuna taşımak için Azure portalını kullanma

Bir kurtarma Hizmetleri kasası ve ilişkili kaynakları farklı bir kaynak grubuna taşımak için

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Listesini açmak **kurtarma Hizmetleri kasaları** ve taşımak istediğiniz kasayı seçin. Kasa panosunda oturum açtığında, aşağıdaki görüntüde gösterildiği gibi görünür.

  ![Açık kurtarma Hizmetleri kasası](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

  Görmüyorsanız, **Essentials** , kasa için bilgileri açılan simgesine tıklayın. Artık kasanız için temel bilgileri görmeniz gerekir.

  ![Temel bilgi sekmesi](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Kasa genel bakış menüde **değiştirme** yanındaki **kaynak grubu**açmak için **kaynakları taşıma** dikey penceresi.

  ![Kaynak grubu Değiştir](./media/backup-azure-move-recovery-services/change-resource-group.png)

4. İçinde **kaynakları taşıma** dikey penceresinde, seçilen kasa için önerilir aşağıdaki görüntüde gösterildiği gibi onay kutusunu seçerek ilgili isteğe bağlı kaynakları Taşı.

  ![Abonelik taşıma](./media/backup-azure-move-recovery-services/move-resource.png)

5. Hedef kaynak grubu eklemek için **kaynak grubu** açılan listesini seçin mevcut bir kaynak grubu veya tıklayın **yeni bir grup oluşturmak** seçeneği.

  ![Kaynak Oluştur](./media/backup-azure-move-recovery-services/create-a-new-resource.png)

6. Kaynak grubuna ekledikten sonra onaylayın **bunları yeni kaynak kimliğini kullanacak şekilde güncelleştirilene kadar araçların ve komut dosyalarının taşınmış kaynaklarla ilişkili çalışmayacağını anlıyorum** seçeneğini ve ardından **Tamam** tamamlamak için Kasa taşıma.

  ![Onay iletisi](./media/backup-azure-move-recovery-services/confirmation-message.png)


## <a name="use-azure-portal-to-move-a-recovery-services-vault-to-a-different-subscription"></a>Kurtarma Hizmetleri kasası farklı bir aboneliğe taşımak için Azure portalını kullanma

Bir kurtarma Hizmetleri kasasını ve ilişkili kaynakları farklı bir aboneliğe geçebilirsiniz

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Kurtarma Hizmetleri kasalarının listesini açmak ve taşımak istediğiniz kasayı seçin. Kasa panosunda oturum açtığında, aşağıdaki resimde gösterildiği gibi görünür.

    ![Açık kurtarma Hizmetleri kasası](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

    Görmüyorsanız, **Essentials** , kasa için bilgileri açılan simgesine tıklayın. Artık kasanız için temel bilgileri görmeniz gerekir.

    ![Temel bilgi sekmesi](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Kasa genel bakış menüde **değiştirme** yanındaki **abonelik**açmak için **kaynakları taşıma** dikey penceresi.

  ![Aboneliği Değiştir](./media/backup-azure-move-recovery-services/change-resource-subscription.png)

4. Taşınacak, kaynakları seçin kullanmanızı öneririz burada **Tümünü Seç** seçeneği listelenen tüm isteğe bağlı kaynakları seçin.

  ![Kaynak taşıma](./media/backup-azure-move-recovery-services/move-resource-source-subscription.png)

5. Hedef abonelik seçin **abonelik** aşağı açılan listesinde, istediğiniz kasaya taşınacak.
6. Hedef kaynak grubu eklemek için **kaynak grubu** açılan listesini seçin mevcut bir kaynak grubu veya tıklayın **yeni bir grup oluşturmak** seçeneği.

  ![Abonelik Ekle](./media/backup-azure-move-recovery-services/add-subscription.png)

7. Tıklayın **bunları yeni kaynak kimliğini kullanacak şekilde güncelleştirilene kadar araçların ve komut dosyalarının taşınmış kaynaklarla ilişkili çalışmayacağını anlıyorum** onaylayın ve ardından seçeneği **Tamam**.

> [!NOTE]
> Çapraz abonelik yedekleme (RS kasa ve korumalı Vm'leri olan farklı Aboneliklerde) desteklenen bir senaryo değildir. Ayrıca, depolama yedekliliği yerel olarak yedekli depolama (LRS) genel olarak yedekli depolama (GRS) seçeneği ve tam tersi kasa taşıma işlemi sırasında değiştirilemez.
>
>

## <a name="use-powershell-to-move-a-vault"></a>Bir kasa taşımak için PowerShell kullanma

Kurtarma Hizmetleri kasası için başka bir kaynak grubuna taşımak için kullanın `Move-AzureRMResource` cmdlet'i. `Move-AzureRMResource` Kaynak adı ve kaynak türü gerektirir. Hem de alabilirsiniz `Get-AzureRmRecoveryServicesVault` cmdlet'i.

```
$destinationRG = "<destinationResourceGroupName>"
$vault = Get-AzureRmRecoveryServicesVault -Name <vaultname> -ResourceGroupName <vaultRGname>
Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Kaynakları farklı aboneliğe taşımak dahil `-DestinationSubscriptionId` parametresi.

```
Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Yukarıdaki cmdlet'lerinden yürütüldükten sonra belirtilen kaynakları taşımak istediğiniz onaylayın istenir. Tür **Y** onaylamak için. Doğrulama başarılı olduktan sonra kaynak taşır.

## <a name="use-cli-to-move-a-vault"></a>Bir kasa taşımak için CLI kullanma

Kurtarma Hizmetleri kasası için başka bir kaynak grubuna taşımak için aşağıdaki cmdlet'i kullanın:

```
az resource move --destination-group <destinationResourceGroupName> --ids <VaultResourceID>
```

Yeni bir aboneliğe taşımak için sağlamak `--destination-subscription-id` parametresi.

## <a name="post-migration"></a>Geçiş sonrası

1. Kaynak grupları için erişim denetimleri kümesini/doğrulamanız gerekir.  
2. Yedekleme raporlama ve izleme özelliği için kasa gönderme taşıma tamamlandıktan yeniden yapılandırılması gerekir. Önceki yapılandırmayı taşıma işlemi sırasında kaybolur.



## <a name="next-steps"></a>Sonraki adımlar

Birçok farklı türdeki kaynakların kaynak grupları ve abonelikler arasında taşıyabilirsiniz.

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).