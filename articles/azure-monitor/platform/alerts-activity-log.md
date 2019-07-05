---
title: Oluşturma, görüntüleme ve yönetme etkinliği Azure İzleyici'de günlük uyarıları
description: Azure portalı, Azure Resource Manager şablonu ve Azure PowerShell kullanarak Etkinlik günlüğü uyarıları oluşturma nasıl.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: vinagara
ms.openlocfilehash: 8183c7070b5d42e1c7a96fc0d64974658b2ec7d0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448918"
---
# <a name="create-view-and-manage-activity-log-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Etkinlik günlüğü Uyarıları yönetme  

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşulları karşılayan yeni bir etkinlik günlüğü olay meydana geldiğinde etkin uyarılar.

Bu uyarılar Azure kaynakları için bir Azure Resource Manager şablonu kullanılarak oluşturulabilir. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Genellikle, etkinlik günlüğü uyarıları, genellikle belirli bir kaynak grubu veya kaynak için kapsamlı, Azure aboneliğinizdeki kaynaklar üzerinde belirli bir değişiklik gerçekleştiğinde bildirim almak oluşturma. Örneğin, tüm aştığında size bildirilmesini isteyebilirsiniz (örnek kaynak grubunda) sanal makine **myProductionResourceGroup** silindi, veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rolü atanmışsa bildirim almak isteyebilirsiniz.

> [!IMPORTANT]
> Hizmet durumu bildirimi ile ilgili uyarılar, etkinlik günlüğü uyarısı oluşturma arabirimi üzerinden oluşturulamaz. Daha fazla hakkında oluşturma ve hizmet durumu bildirimlerini kullanarak bilgi edinmek için [hizmet durumu bildirimlerini etkinlik günlük uyarılarını alırsınız](alerts-activity-log-service-notifications.md).

Uyarı kuralları oluştururken aşağıdakilerden emin olun:

- Uyarının oluşturulduğu özelliği abonelik kapsamdaki abonelikten farklı değil.
- Ölçüt düzeyi/status olmalıdır/çağıran / kaynak grubu / kaynak kimliği / kaynak türü / olay kategorisi uyarı yapılandırılır.
- "Herhangi" koşulu veya uyarı yapılandırmasında JSON iç içe geçmiş koşullar yoktur (temel olarak, yalnızca bir tümü, başka hiçbir tümü/herhangi kullanılabilir).
- Kategori "Yönetici" olduğunda olmadığı. Yukarıdaki ölçütlerden en az bir uyarıyı belirtmeniz gerekir. Etkinlik günlüğünde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturabilirsiniz.


## <a name="azure-portal"></a>Azure portal

Azure portalını kullanarak, kullanıcı oluşturma & etkinlik günlük uyarı kuralı değiştirin. Ve belirli olaylar için sorunsuz uyarı oluşturma ilgi emin olmak için Azure etkinlik günlüğü - tümleşik bir deneyim.

### <a name="create-with-azure-portal"></a>Azure portalı ile oluşturma

Aşağıdaki yordamı kullanın:

1. Azure portalından seçin **İzleyici** > **uyarıları**
2. Tıklayın **yeni uyarı kuralı** en üstündeki **uyarılar** penceresi.

     ![Yeni uyarı kuralı](media/alerts-activity-log/AlertsPreviewOption.png)

     **Oluşturma kuralı** penceresi görüntülenir.

      ![Yeni uyarı kuralı seçenekleri](media/alerts-activity-log/create-new-alert-rule-options.png)

3. **Tanımlama Uyarı koşulu altında** aşağıdaki bilgileri belirtin ve tıklayın **Bitti**.

   - **Uyarı hedefi:** Yeni uyarı için bir hedef seçin ve görüntülemek için kullanın **aboneliğe göre filtrele** / **kaynak türüne göre filtre** ve görüntülenen listeden kaynak veya kaynak grubu seçin.

     > [!NOTE]
     > 
     > bir kaynak, kaynak grubuna ya da aboneliğin tümü için etkinlik günlüğü sinyali seçebilirsiniz.

     **Uyarı hedef örnek görünümü**
     ![hedef seçin](media/alerts-activity-log/select-target.png)

   - Altında **hedef ölçütleri**, tıklayın **Ölçüt Ekle** ve çeşitli kategorileri ait olanlar da dahil olmak üzere tüm kullanılabilir sinyaller hedefi için görüntülenen **etkinlik günlüğü**; Kategori adı eklenmiş olarak **İzleyicisi hizmeti** adı.

   - Sinyal çeşitli işlemlerin olası türü için görüntülenen listeden seçin **etkinlik günlüğü**.

     Günlük geçmişi zaman çizelgesi ve karşılık gelen bir uyarı mantık bu hedef sinyal seçebilirsiniz:

     **Ölçüt ekranı ekleme**

     ![Ölçüt Ekle](media/alerts-activity-log/add-criteria.png)

     **Geçmiş zaman**: Seçilen işlem için kullanılabilir olayları, saatler (veya) geçen hafta boyunca son 6/12/24 gösterilebilir.

     **Uyarı mantığı**:

     - **Olay düzeyi**-olay önem derecesi. _Ayrıntılı_, _bilgilendirici_, _uyarı_, _hata_, veya _kritik_.
     - **Durum**: Olay durumu. _Başlatılan_, _başarısız_, veya _başarılı_.
     - **Olayı başlatan tarafından**: Arayan olarak da bilinir; E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanımlayıcısı.

       Uyarı mantığı uygulanan örnek sinyal grafiği:

       ![ seçilen ölçütlere](media/alerts-activity-log/criteria-selected.png)

4. Altında **uyarı kuralları ayrıntılarını tanımlayın**, şu bilgileri sağlayın:

    - **Uyarı kuralı adı** – yeni bir uyarı kuralı adı
    - **Açıklama** – yeni uyarı kuralı için açıklama
    - **Uyarıyı kaynak grubuna kaydetme** – bu yeni kuralını kaydetmek istediğiniz kaynak grubunu seçin.

5. Altında **eylem grubu**, açılan menüsünde bu yeni bir uyarı kuralının atamak istediğiniz eylem grubu belirtin. Alternatif olarak, [yeni bir eylem grubu oluşturma](../../azure-monitor/platform/action-groups.md) ve yeni kural için atayın. Yeni bir grup oluşturmak için tıklayın **+ yeni grup**.

6. Kuralları oluşturduktan sonra etkinleştirmek için tıklayın **Evet** için **oluşturulduktan sonra kuralı etkinleştir** seçeneği.
7. Tıklayın **uyarı kuralı oluştur**.

    Etkinlik günlüğü için yeni uyarı kuralı oluşturulur ve bir onay iletisi üst kısmında görünür sağ.

    Etkinleştirme, devre dışı bırakmak, düzenlemek veya silmek bir kural. Etkinlik günlüğü kurallarını yönetme hakkında daha fazla bilgi edinin.


Alternatif olarak, bir basit benzerleme, uyarı kuralları oluşturulabilir, etkinlik günlüğünde anlama koşullar için olan keşfedin veya aracılığıyla olayları filtrelemek için [Azure portalında etkinlik günlüğü](activity-log-view.md#azure-portal). Azure İzleyici - etkinlik günlüğü, bir filtre veya gerekli olay bulabilir ve ardından kullanarak bir uyarı oluşturma **etkinlik günlüğü uyarısı Ekle** düğmesini; ardından adımları 4 ve üzeri öğreticide yukarıda belirtildiği gibi izleyin.
    
 ![ Etkinlik günlüğü uyarısı Ekle](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-azure-portal"></a>Görüntüleyin ve Azure Portal'da yönetin

1. Azure portalından tıklayın **İzleyici** > **uyarılar** tıklatıp **kuralları yönetmek** , pencerenin sol üst köşesindeki.

    ![ Uyarı kurallarını yönet](media/alerts-activity-log/manage-alert-rules.png)

    Kullanılabilir kurallar listesi görüntülenir.

2. Etkinlik günlüğü kuralı değiştirmek için arama yapın.

    ![ Etkinlik günlüğü uyarı kurallarını Ara](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    Kullanılabilir filtreleri - kullanabileceğiniz _abonelik_, _kaynak grubu_, _kaynak_, _sinyal türü_, veya _durumu_  düzenlemek istediğiniz etkinliği kural bulunamadı.

   > [!NOTE]
   > 
   > Yalnızca düzenlemek **açıklama** , **hedef ölçütleri** ve **Eylem grupları**.

3. Kuralı seçin ve kuralı seçeneklerini düzenlemek için çift tıklayın. Gerekli değişiklikleri yapın ve ardından **Kaydet**.

   ![ Uyarı kurallarını yönet](media/alerts-activity-log/activity-log-rule-edit-page.png)

4. Devre dışı bırakma, etkinleştirme veya kural silme. Kural 2. adımda açıklandığı seçtikten sonra pencerenin üst kısmındaki uygun seçeneği belirleyin.


## <a name="azure-resource-template"></a>Azure kaynak şablonu
Resource Manager şablonu kullanarak bir etkinlik günlüğü uyarısı oluşturmak için kaynak türü oluştur `microsoft.insights/activityLogAlerts`. Daha sonra tüm ilgili özellikleri doldurun. Bir etkinlik günlüğü uyarısı oluşturan bir şablonu aşağıda verilmiştir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
Yukarıdaki örnek json (örneğin) sampleActivityLogAlert.json amacıyla bu kılavuzda olarak kaydedilebilir ve kullanılarak dağıtılabilir [Azure portalında Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy-portal.md).

> [!NOTE]
> Bu 5 dakika kadar sürebilir etkin hale gelmek için yeni bir etkinlik günlük uyarı kuralı

## <a name="rest-api"></a>REST API 
[Azure İzleyici - etkinlik günlüğü uyarıları API](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) REST API ve Azure Resource Manager REST API'si ile tamamen uyumlu. Bu nedenle Azure CLI yanı sıra, Resource Manager cmdlet'ini kullanarak Powershell kullanılabilir.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-resource-manager-template-with-powershell"></a>PowerShell ile Resource Manager şablonu dağıtma
Örnek kaynak önceki bir [kaynak şablonu bölüm] içinde gösterilen şablonu dağıtmak için PowerShell kullanma (#resource-manager-şablonu, aşağıdaki komutu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

Burada sampleActivityLogAlert.parameters.json uyarı kuralı oluşturmak için gereken parametreleri için sağlanan değerler içerir.

### <a name="use-activity-log-powershell-cmdlets"></a>Etkinlik günlüğünü PowerShell cmdlet'lerini kullanma

Etkinlik günlüğü uyarıları adanmış PowerShell cmdlet'leri kullanılabilir:

- [Set-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Set-AzActivityLogAlert) : Yeni bir oluşturur veya mevcut bir etkinlik günlüğü uyarısını güncelleştirin.
- [Get-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Get-AzActivityLogAlert) : Bir veya daha fazla etkinlik günlük uyarı kaynakları alır.
- [Etkinleştir-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Enable-AzActivityLogAlert) : Mevcut bir etkinlik günlüğü uyarısını sağlar ve kendi etiketleri ayarlar.
- [Disable-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Disable-AzActivityLogAlert) : Mevcut bir etkinlik günlüğü uyarısını devre dışı bırakır ve kendi etiketleri ayarlar.
- [Remove-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Remove-AzActivityLogAlert) : Bir etkinlik günlüğü uyarısı kaldırır.

## <a name="cli"></a>CLI

Azure CLI komutları kümesi altında ayrılmış [az İzleyici etkinlik günlüğü Uyarısı](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert) etkinlik günlüğü uyarı kuralları yönetmek için kullanılabilir.

Yeni bir etkinlik günlüğü uyarı kuralı oluşturmak için bu sırayla kullanın:

1. [az İzleyici etkinlik günlüğü uyarısı oluşturma](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): Yeni etkinlik günlük uyarı kuralı kaynağı oluşturma
1. [az İzleyici etkinlik günlüğü uyarısı kapsam](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/scope): Kapsam için oluşturulan etkinlik günlük uyarı kuralı Ekle
1. [az İzleyici etkinlik günlüğü uyarısı eylem grubu](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/action-group): Eylem grubu için etkinlik günlüğü uyarı kuralı Ekle

Bir etkinlik günlük uyarı kuralı kaynağı, Azure CLI komutunu alınacak [az İzleyici etkinlik günlüğü Uyarısı Göster](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
) kullanılabilir. Ve bir kaynak grubundaki tüm etkinlik günlük uyarı kuralı kaynağı görüntülemek için [az İzleyici etkinlik günlüğü uyarı listesi](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list).
Azure CLI komutunu kullanarak etkinlik günlük uyarı kuralı kaynakları kaldırılabilir [az İzleyici etkinlik günlüğü uyarısını silme](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete).

## <a name="next-steps"></a>Sonraki adımlar

- [Etkinlik günlükleri için Web kancası şeması](../../azure-monitor/platform/activity-log-alerts-webhook.md)
- [Etkinlik günlüklerine genel bakış](../../azure-monitor/platform/activity-log-alerts.md) 
- Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md).  
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../../azure-monitor/platform/service-notifications.md).
