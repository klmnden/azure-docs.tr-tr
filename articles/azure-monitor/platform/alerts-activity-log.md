---
title: Oluşturma, görüntüleme ve Azure İzleyicisi'nde etkinlik günlüğü Uyarıları yönetme
description: Azure Portal, kaynak şablonu ve PowerShell etkinlik günlüğü uyarıları oluşturma
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.openlocfilehash: 2b90457ed939999b5163078750650c92a3516cca
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55816586"
---
# <a name="create-view-and-manage-activity-log-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Etkinlik günlüğü Uyarıları yönetme  

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşulları karşılayan yeni bir etkinlik günlüğü olay meydana geldiğinde etkin uyarılar.

Bu uyarılar Azure kaynakları için bir Azure Resource Manager şablonu kullanılarak oluşturulabilir. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Genellikle, etkinlik günlüğü uyarıları, genellikle belirli bir kaynak grubu veya kaynak için kapsamlı, Azure aboneliğinizdeki kaynaklar üzerinde belirli bir değişiklik gerçekleştiğinde bildirim almak oluşturma. Örneğin, tüm aştığında size bildirilmesini isteyebilirsiniz (örnek kaynak grubunda) sanal makine **myProductionResourceGroup** silindi, veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rolü atanmışsa bildirim almak isteyebilirsiniz.

> [!IMPORTANT]
> Hizmet durumu bildirimi ile ilgili uyarılar, etkinlik günlüğü uyarısı oluşturma arabirimi üzerinden oluşturulamaz. Daha fazla hakkında oluşturma ve hizmet durumu bildirimlerini kullanarak bilgi edinmek için [hizmet durumu bildirimlerini etkinlik günlük uyarılarını alırsınız](alerts-activity-log-service-notifications.md).

## <a name="azure-portal"></a>Azure portal

> [!NOTE]

>  Uyarı kuralları oluştururken aşağıdakilerden emin olun:

> - Uyarının oluşturulduğu özelliği abonelik kapsamdaki abonelikten farklı değil.
- Ölçüt düzeyi/status olmalıdır/çağıran / kaynak grubu / kaynak kimliği / kaynak türü / olay kategorisi uyarı yapılandırılır.
- "Herhangi" koşulu veya uyarı yapılandırmasında JSON iç içe geçmiş koşullar yoktur (temel olarak, yalnızca bir tümü, başka hiçbir tümü/herhangi kullanılabilir).
- Kategori "Yönetici" olduğunda olmadığı. Yukarıdaki ölçütlerden en az bir uyarıyı belirtmeniz gerekir. Etkinlik günlüğünde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturabilirsiniz.

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

    > bir kaynak, kaynak grubuna ya da aboneliğin tümü için etkinlik günlüğü sinyali seçebilirsiniz.

    **Uyarı hedef örnek görünümü** ![hedef seçin](media/alerts-activity-log/select-target.png)

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


Alternatif olarak, bir basit benzerleme, uyarı kuralları oluşturulabilir, etkinlik günlüğünde anlama koşullar için olan keşfedin veya aracılığıyla olayları filtrelemek için [Azure portalında etkinlik günlüğü](../../azure-monitor/platform/activity-logs-overview.md#query-the-activity-log-in-the-azure-portal). Azure İzleyici - etkinlik günlüğü, bir filtre veya gerekli olay bulabilir ve ardından kullanarak bir uyarı oluşturma **etkinlik günlüğü uyarısı Ekle** düğmesini; ardından adımları 4 ve üzeri öğreticide yukarıda belirtildiği gibi izleyin.
    
 ![ Etkinlik günlüğü uyarısı Ekle](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-azure-portal"></a>Görüntüleyin ve Azure Portal'da yönetin

1. Azure portalından tıklayın **İzleyici** > **uyarılar** tıklatıp **kuralları yönetmek** , pencerenin sol üst köşesindeki.

    ![ Uyarı kurallarını yönet](media/alerts-activity-log/manage-alert-rules.png)

    Kullanılabilir kurallar listesi görüntülenir.

2. Etkinlik günlüğü kuralı değiştirmek için arama yapın.

    ![ Etkinlik günlüğü uyarı kurallarını Ara](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    Kullanılabilir filtreleri - kullanabileceğiniz _abonelik_, _kaynak grubu_, _kaynak_, _sinyal türü_, veya _durumu_  düzenlemek istediğiniz etkinliği kural bulunamadı.

    > [!NOTE]

    > Yalnızca düzenlemek **açıklama** , **hedef ölçütleri** ve **Eylem grupları**.

3.  Kuralı seçin ve kuralı seçeneklerini düzenlemek için çift tıklayın. Gerekli değişiklikleri yapın ve ardından **Kaydet**.

    ![ Uyarı kurallarını yönet](media/alerts-activity-log/activity-log-rule-edit-page.png)

4.  Devre dışı bırakma, etkinleştirme veya kural silme. Kural 2. adımda açıklandığı seçtikten sonra pencerenin üst kısmındaki uygun seçeneği belirleyin.


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
Aşağıda örnek daha önce gösterilen kaynak şablonu (sampleActivityLogAlert.json) için Azure Resource Manager PowerShell cmdlet'i aracılığıyla kullanım kaynak şablonu bölümünde gösterilmektedir:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```
Burada görüntülerle sampleActivityLogAlert.parameters.json uyarı kuralı oluşturmak için gereken parametreleri için sağlanan değerlere sahiptir.

## <a name="cli"></a>CLI
Aşağıda Azure Resource Manager kaynak şablonu (sampleActivityLogAlert.json) daha önce gösterilen örnek için Azure CLI komutu aracılığıyla kullanım kaynak şablonu bölümünde gösterilmektedir:

```azurecli
az group deployment create --resource-group myRG --template-file sampleActivityLogAlert.json --parameters @sampleActivityLogAlert.parameters.json
```
*SampleActivityLogAlert.parameters.json* dosyası uyarı kuralı oluşturmak için gereken parametreleri için sağlanan değerlere sahiptir.


## <a name="next-steps"></a>Sonraki adımlar

- [Etkinlik günlükleri için Web kancası şeması](../../azure-monitor/platform/activity-log-alerts-webhook.md)
- [Etkinlik günlüklerine genel bakış](../../azure-monitor/platform/activity-log-alerts.md) 
- Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md).  
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../../azure-monitor/platform/service-notifications.md).
