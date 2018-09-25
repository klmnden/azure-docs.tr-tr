---
title: Oluşturma, görüntüleme ve Azure İzleyicisi'nde etkinlik günlüğü Uyarıları yönetme
description: Azure Portal, kaynak şablonu ve PowerShell etkinlik günlüğü uyarıları oluşturma
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 526c50fa4d261a30738c3f24d537fe5e0d765f6d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951313"
---
# <a name="create-view-and-manage-activity-log-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Etkinlik günlüğü Uyarıları yönetme  

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşulları karşılayan yeni bir etkinlik günlüğü olay meydana geldiğinde etkin uyarılar.

Bu uyarılar Azure kaynakları için bir Azure Resource Manager şablonu kullanılarak oluşturulabilir. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Genellikle, etkinlik günlüğü uyarıları, genellikle belirli bir kaynak grubu veya kaynak için kapsamlı, Azure aboneliğinizdeki kaynaklar üzerinde belirli bir değişiklik gerçekleştiğinde bildirim almak oluşturma. Örneğin, tüm aştığında size bildirilmesini isteyebilirsiniz (örnek kaynak grubunda) sanal makine **myProductionResourceGroup** silindi, veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rolü atanmışsa bildirim almak isteyebilirsiniz.

> [!IMPORTANT]
> Hizmet durumu bildirimi ile ilgili uyarılar, etkinlik günlüğü uyarısı oluşturma arabirimi üzerinden oluşturulamaz. Daha fazla hakkında oluşturma ve hizmet durumu bildirimlerini kullanarak bilgi edinmek için [hizmet durumu bildirimlerini etkinlik günlük uyarılarını alırsınız](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="manage-alert-rules-for-activity-log-using-azure-portal"></a>Azure portalını kullanarak Etkinlik günlüğü uyarı kurallarını yönet

> [!NOTE]

>  Uyarı kuralları oluştururken aşağıdakilerden emin olun:

> - Uyarının oluşturulduğu özelliği abonelik kapsamdaki abonelikten farklı değil.
- Ölçüt düzeyi/status olmalıdır/çağıran / kaynak grubu / kaynak kimliği / kaynak türü / olay kategorisi uyarı yapılandırılır.
- "Herhangi" koşulu veya uyarı yapılandırmasında JSON iç içe geçmiş koşullar yoktur (temel olarak, yalnızca bir tümü, başka hiçbir tümü/herhangi kullanılabilir).
- Kategori "Yönetici" olduğunda olmadığı. Yukarıdaki ölçütlerden en az bir uyarıyı belirtmeniz gerekir. Etkinlik günlüğünde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturabilirsiniz.

### <a name="create-an-alert-rule-for-an-activity-log-using-azure-portal"></a>Azure portalını kullanarak bir etkinlik günlüğü uyarı kuralı oluşturma

Aşağıdaki yordamı kullanın:

1. Azure portalından seçin **İzleyici** > **uyarıları**
2. Tıklayın **yeni uyarı kuralı** en üstündeki **uyarılar** penceresi.

     ![Yeni uyarı kuralı](./media/monitor-alerts-unified/AlertsPreviewOption.png)

     **Oluşturma kuralı** penceresi görüntülenir.

      ![Yeni uyarı kuralı seçenekleri](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule-options.png)

3. **Tanımlama Uyarı koşulu altında** aşağıdaki bilgileri belirtin ve tıklayın **Bitti**.

    - **Uyarı hedefi:** görüntülemek ve yeni uyarı hedefi seçmek için kullanın **aboneliğe göre filtrele** / **kaynak türüne göre filtre** ve kaynak veya kaynak grubu seçin liste görüntülenir.

    > [!NOTE]

    > bir kaynak, kaynak grubuna ya da aboneliğin tümü için etkinlik günlüğü sinyali seçebilirsiniz.

    **Uyarı hedef örnek görünümü** ![hedef seçin](./media/monitoring-activity-log-alerts-new-experience/select-target.png)

    - Altında **hedef ölçütleri**, tıklayın **Ölçüt Ekle** ve çeşitli kategorileri ait olanlar da dahil olmak üzere tüm kullanılabilir sinyaller hedefi için görüntülenen **etkinlik günlüğü**; Kategori adı eklenmiş olarak **İzleyicisi hizmeti** adı.

    - Sinyal çeşitli işlemlerin olası türü için görüntülenen listeden seçin **etkinlik günlüğü**.

    Günlük geçmişi zaman çizelgesi ve karşılık gelen bir uyarı mantık bu hedef sinyal seçebilirsiniz:

    **Ölçüt ekranı ekleme**

    ![Ölçüt Ekle](./media/monitoring-activity-log-alerts-new-experience/add-criteria.png)

    **Geçmiş zaman**: Seçili işlem için kullanılabilir olayları çizilen son 6/12/24 saatler (veya) geçen hafta boyunca.

    **Uyarı mantığı**:

     - **Olay düzeyi**-olay önem derecesi. _Ayrıntılı_, _bilgilendirici_, _uyarı_, _hata_, veya _kritik_.
     - **Durum**: olay durumu. _Başlatılan_, _başarısız_, veya _başarılı_.
     - **Olayı başlatan tarafından**: çağıranların da bilinir E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanımlayıcısı.

        Uyarı mantığı uygulanan örnek sinyal grafiği:

        ![ seçilen ölçütlere](./media/monitoring-activity-log-alerts-new-experience/criteria-selected.png)

4. Altında **uyarı kuralları ayrıntılarını tanımlayın**, şu bilgileri sağlayın:

    - **Uyarı kuralı adı** – yeni bir uyarı kuralı adı
    - **Açıklama** – yeni uyarı kuralı için açıklama
    - **Uyarıyı kaynak grubuna kaydetme** – bu yeni kuralını kaydetmek istediğiniz kaynak grubunu seçin.

5. Altında **eylem grubu**, açılan menüsünde bu yeni bir uyarı kuralının atamak istediğiniz eylem grubu belirtin. Alternatif olarak, [yeni bir eylem grubu oluşturma](monitoring-action-groups.md) ve yeni kural için atayın. Yeni bir grup oluşturmak için tıklayın **+ yeni grup**.

6. Kuralları oluşturduktan sonra etkinleştirmek için tıklayın **Evet** için **oluşturulduktan sonra kuralı etkinleştir** seçeneği.
7. Tıklayın **uyarı kuralı oluştur**.

    Etkinlik günlüğü için yeni uyarı kuralı oluşturulur ve bir onay iletisi üst kısmında görünür sağ.

    Etkinleştirme, devre dışı bırakmak, düzenlemek veya silmek bir kural. [Daha fazla bilgi edinin](#view-and-manage-activity-log-alert-rules-in-azure-portal) etkinlik günlüğü kurallarını yönetme hakkında.


Alternatif olarak, bir basit benzerleme, uyarı kuralları oluşturulabilir, etkinlik günlüğünde anlama koşullar için olan keşfedin veya aracılığıyla olayları filtrelemek için [Azure portalında etkinlik günlüğü](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). Azure İzleyici - etkinlik günlüğü, bir filtre veya gerekli olay bulabilir ve ardından kullanarak bir uyarı oluşturma **etkinlik günlüğü uyarısı Ekle** düğmesini; ardından adımları 4 ve üzeri öğreticide yukarıda belirtildiği gibi izleyin.
    
 ![ Etkinlik günlüğü uyarısı Ekle](./media/monitoring-activity-log-alerts-new-experience/add-activity-log.png)
    

### <a name="view-and-manage-activity-log-alert-rules-in-azure-portal"></a>Görüntüleme ve etkinlik günlüğü uyarı kuralları, Azure portalında yönetme

1. Azure portalından tıklayın **İzleyici** > **uyarılar** tıklatıp **kuralları yönetmek** , pencerenin sol üst köşesindeki.

    ![ Uyarı kurallarını yönet](./media/monitoring-activity-log-alerts-new-experience/manage-alert-rules.png)

    Kullanılabilir kurallar listesi görüntülenir.

2. Etkinlik günlüğü kuralı değiştirmek için arama yapın.

    ![ Etkinlik günlüğü uyarı kurallarını Ara](./media/monitoring-activity-log-alerts-new-experience/searth-activity-log-rule-to-edit.png)

    Kullanılabilir filtreleri - kullanabileceğiniz _abonelik_, _kaynak grubu_, _kaynak_, _sinyal türü_, veya _durumu_  düzenlemek istediğiniz etkinliği kural bulunamadı.

    > [!NOTE]

    > Yalnızca düzenlemek **açıklama** , **hedef ölçütleri** ve **Eylem grupları**.

3.  Kuralı seçin ve kuralı seçeneklerini düzenlemek için çift tıklayın. Gerekli değişiklikleri yapın ve ardından **Kaydet**.

    ![ Uyarı kurallarını yönet](./media/monitoring-activity-log-alerts-new-experience/activity-log-rule-edit-page.png)

4.  Devre dışı bırakma, etkinleştirme veya kural silme. Kural 2. adımda açıklandığı seçtikten sonra pencerenin üst kısmındaki uygun seçeneği belirleyin.


## <a name="manage-alert-rules-for-activity-log-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak Etkinlik günlüğü uyarı kurallarını yönet
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
Yukarıdaki örnek json (örneğin) sampleActivityLogAlert.json amacıyla bu kılavuzda olarak kaydedilebilir ve kullanılarak dağıtılabilir [Azure portalında Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy-portal.md).

> [!NOTE]
> Bu 5 dakika kadar sürebilir etkin hale gelmek için yeni bir etkinlik günlük uyarı kuralı

## <a name="manage-alert-rules-for-activity-log-using-powershell-cli-or-api"></a>PowerShell, CLI veya API kullanarak Etkinlik günlüğü uyarı kurallarını yönet
[Azure İzleyici - etkinlik günlüğü uyarıları API](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) REST API ve Azure Resource Manager REST API'si ile tamamen uyumlu. Bu nedenle Azure CLI yanı sıra, Resource Manager cmdlet'ini kullanarak Powershell kullanılabilir.

Kaynak şablonu (sampleActivityLogAlert.json) daha önce gösterilen örnek için Azure Resource Manager PowerShell cmdlet'i aracılığıyla kullanımı aşağıda gösterilen [kaynak şablon bölümü](#manage-alert-rules-for-activity-log-using-azure-resource-template) :
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```
Burada görüntülerle sampleActivityLogAlert.parameters.json uyarı kuralı oluşturmak için gereken parametreleri için sağlanan değerlere sahiptir.

Azure Resource Manager kaynak şablonu (sampleActivityLogAlert.json) daha önce gösterilen örnek için Azure CLI komutu aracılığıyla kullanımı aşağıda gösterilen [kaynak şablon bölümü](#manage-alert-rules-for-activity-log-using-azure-resource-template) :

```azurecli
az group deployment create --resource-group myRG --template-file sampleActivityLogAlert.json --parameters @sampleActivityLogAlert.parameters.json
```
Burada görüntülerle sampleActivityLogAlert.parameters.json uyarı kuralı oluşturmak için gereken parametreleri için sağlanan değerlere sahiptir.


## <a name="next-steps"></a>Sonraki adımlar

- [Etkinlik günlükleri için Web kancası şeması](monitoring-activity-log-alerts-webhook.md)
- [Etkinlik günlüklerine genel bakış](monitoring-activity-log-alerts.md) 
- Daha fazla bilgi edinin [Eylem grupları](monitoring-action-groups.md).  
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](monitoring-service-notifications.md).
