---
title: Oluşturun, görüntüleyin ve Azure İzleyicisi'nde etkinlik günlüğü Uyarıları yönetme
description: Etkinlik günlüğü uyarıları, Azure portalı, Azure Resource Manager şablonu ve Azure PowerShell kullanarak oluşturma.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: vinagara
ms.openlocfilehash: 71e61228fcdbd52abedbc1f1205baa60b1aea923
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705288"
---
# <a name="create-view-and-manage-activity-log-alerts-by-using-azure-monitor"></a>Oluşturun, görüntüleyin ve etkinlik günlüğü uyarıları, Azure İzleyicisi'ni kullanarak yönetme  

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşulları karşılayan yeni bir etkinlik günlüğü olay meydana geldiğinde etkin uyarılar.

Bu uyarılar için Azure kaynaklarıdır ve bir Azure Resource Manager şablonu kullanılarak oluşturulabilir. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Genellikle, etkinlik günlüğü uyarıları, Azure aboneliğinizdeki kaynaklara belirli değişiklik gerçekleştiğinde bildirim almak oluşturma. Uyarılar, genellikle belirli kaynak grupları ve kaynaklara belirlenir. Örneğin, tüm aştığında size bildirilmesini isteyebilirsiniz örnek kaynak grubundaki sanal makine **myProductionResourceGroup** silinir. Veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rolü atanmışsa bildirim almak isteyebilirsiniz.

> [!IMPORTANT]
> Hizmet durumu bildirimi ile ilgili uyarılar, etkinlik günlüğü uyarısı oluşturma arabirimi üzerinden oluşturulamıyor. Hizmet durumu bildirimlerine oluşturulacağı ve kullanılacağı hakkında daha fazla bilgi için bkz: [hizmet durumu bildirimlerini etkinlik günlük uyarılarını alırsınız](alerts-activity-log-service-notifications.md).

Uyarı kuralları oluşturduğunuzda aşağıdakilerden emin olun:

- Abonelik kapsamda uyarının oluşturulduğu abonelikten farklı değildir.
- Ölçüt düzeyi, durumu, çağıranın, kaynak grubu, kaynak kimliği veya kaynak türü olay kategorisi uyarının yapılandırıldığı olmalıdır.
- "Herhangi" koşulu veya uyarı yapılandırmasında JSON iç içe geçmiş koşullar yoktur. Temel olarak, tek bir "tümü" koşul, hiçbir ek "tümü" veya "herhangi" koşullarla izin verilir.
- Kategori "Yönetici" olduğunda, uyarıyı yukarıdaki ölçütlerden en az bir belirtmeniz gerekir. Etkinlik günlüğünde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturabilirsiniz.


## <a name="azure-portal"></a>Azure portal

Azure portalı, oluşturmak ve etkinlik günlüğü uyarı kuralları değiştirmek için kullanabilirsiniz. Deneyimi, ilgilendiğiniz belirli olayları sorunsuz çok uyarı oluşturma emin olmak için bir Azure etkinlik günlüğü ile tümleşiktir.

### <a name="create-with-the-azure-portal"></a>Azure portal ile oluşturma

Aşağıdaki yordamı kullanın.

1. Azure portalında **İzleyici** > **uyarılar**.
2. Seçin **yeni uyarı kuralı** sol alt köşesindeki **uyarılar** penceresi.

     ![Yeni uyarı kuralı](media/alerts-activity-log/AlertsPreviewOption.png)

     **Oluşturma kuralı** penceresi görüntülenir.

      ![Yeni uyarı kuralı seçenekleri](media/alerts-activity-log/create-new-alert-rule-options.png)

3. Altında **uyarı koşulunu tanımlama**, aşağıdaki bilgileri sağlayın ve seçin **Bitti**:

   - **Uyarı hedefi:** Yeni uyarı için bir hedef seçin ve görüntülemek için kullanın **aboneliğe göre filtrele** / **kaynak türüne göre filtre**. Kaynak veya kaynak grubu görüntülenen listeden seçin.

     > [!NOTE]
     > 
     > Bir kaynak, kaynak grubuna ya da aboneliğin tümü için bir etkinlik günlüğü sinyali seçebilirsiniz.

     **Uyarı hedef örnek görünümü**

     ![Hedef seçin](media/alerts-activity-log/select-target.png)

   - Altında **hedef ölçütleri**seçin **Ölçüt Ekle**. Çeşitli kategorileri olanlardan içeren hedef için tüm kullanılabilir sinyaller görüntülenen **etkinlik günlüğü**. Kategori adı eklenir **İzleyicisi hizmeti** adı.

   - Sinyal çeşitli işlemlerin olası türü için görüntülenen listeden seçin **etkinlik günlüğü**.

     Günlük geçmişi zaman çizelgesi ve karşılık gelen bir uyarı mantık bu hedef sinyal seçebilirsiniz:

     **Ölçüt ekranı ekleme**

     ![Ölçüt Ekle](media/alerts-activity-log/add-criteria.png)

     - **Geçmiş zaman**: Seçilen işlem için kullanılabilir olayları, son 6, 12 veya 24 saat içindeki veya geçen hafta boyunca gösterilebilir.

     - **Uyarı mantığı**:

       - **Olay düzeyi**: Olay önem derecesi: _Ayrıntılı_, _bilgilendirici_, _uyarı_, _hata_, veya _kritik_.
       - **Durum**: Olay durumu: _Başlatılan_, _başarısız_, veya _başarılı_.
       - **Olayı başlatan tarafından**: Arayan olarak da bilinir. E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanımlayıcısı.

       Bu örnek sinyal grafiği uygulanan uyarı mantığı vardır:

       ![seçilen ölçütlere](media/alerts-activity-log/criteria-selected.png)

4. Altında **uyarı ayrıntılarını tanımlama**, şu bilgileri sağlayın:

    - **Uyarı kuralı adı**: Yeni uyarı kuralı adı.
    - **Açıklama**: Yeni uyarı kuralı açıklaması.
    - **Uyarıyı kaynak grubuna kaydetme**: Bu yeni kuralını kaydetmek için istediğiniz kaynak grubunu seçin.

5. Altında **eylem grubu**, açılan menüsünde bu yeni bir uyarı kuralının atamak istediğiniz eylem grubu belirtin. Veya, [yeni bir eylem grubu oluşturma](../../azure-monitor/platform/action-groups.md) ve yeni kural için atayın. Yeni bir grup oluşturmak için Seç **+ yeni grup**.

6. Kuralları oluşturduktan sonra etkinleştirmek için seçin **Evet** için **oluşturulduktan sonra kuralı etkinleştir** seçeneği.
7. Seçin **uyarı kuralı oluştur**.

    Etkinlik günlüğü için yeni uyarı kuralı oluşturulur ve pencerenin sağ üst köşesinde bulunan bir onay iletisi görüntülenir.

    Etkinleştirme, devre dışı bırakmak, düzenlemek veya silmek bir kural. Etkinlik günlüğü kuralları yönetme hakkında daha fazla bilgi edinin.


Bir basit benzerleme, uyarı kuralları oluşturulabilir bir etkinlik günlüğü'nde anlama koşulları için keşfetmek veya aracılığıyla olayları filtreleyin [Azure portalında etkinlik günlüğü](activity-log-view.md#azure-portal). İçinde **Azure İzleyici - etkinlik günlüğü** ekran, filtreleyebilir veya gerekli olan olayı bulun ve ardından kullanarak bir uyarı oluşturma **etkinlik günlüğü uyarısı Ekle** düğmesi. Gösterilen adımlar 4 ila 7 olarak daha önce daha sonra izleyin.
    
 ![Etkinlik günlüğü uyarısı Ekle](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-the-azure-portal"></a>Görüntüleme ve Azure portalında yönetme

1. Azure portalında **İzleyici** > **uyarılar**. Seçin **uyarı kurallarını yönet** penceresinin sol üst köşesindeki.

    ![Uyarı kurallarını yönet](media/alerts-activity-log/manage-alert-rules.png)

    Kullanılabilir kurallar listesi görüntülenir.

2. Değiştirmek etkinlik günlüğü kuralını arayın.

    ![Etkinlik günlüğü uyarı kurallarını Ara](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    Kullanılabilir filtreleri kullanabilirsiniz _abonelik_, _kaynak grubu_, _kaynak_, _sinyal türü_, veya _durumu_ , düzenlemek istediğiniz etkinliği kural bulunamadı.

   > [!NOTE]
   > 
   > Yalnızca düzenlemek **açıklama**, **hedef ölçütleri**, ve **Eylem grupları**.

3. Kuralı seçin ve kuralı seçeneklerini düzenlemek için çift tıklayın. Gerekli değişiklikleri yapın ve ardından **Kaydet**.

   ![Uyarı kurallarını yönet](media/alerts-activity-log/activity-log-rule-edit-page.png)

4. Etkinleştirme, devre dışı bırakın veya bir kural silme. Kural 2. adımda açıklandığı gibi seçtikten sonra pencerenin üst kısmındaki uygun seçeneği seçin.


## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Bir Azure Resource Manager şablonu kullanarak bir etkinlik günlüğü uyarısı oluşturmak için kaynak türü oluştur `microsoft.insights/activityLogAlerts`. Daha sonra tüm ilgili özellikleri doldurun. Bir etkinlik günlüğü uyarısı oluşturan bir şablonu aşağıda verilmiştir:

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
Önceki örnek JSON olarak, örneğin, bu kılavuzun amacı doğrultusunda sampleActivityLogAlert.json kaydedilebilir ve kullanılarak dağıtılan [Azure portalında Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy-portal.md).

> [!NOTE]
> Bu yeni etkinlik günlük uyarı kuralının etkin 5 dakika kadar sürebilir.

## <a name="rest-api"></a>REST API 
[Azure İzleyici etkinlik günlüğü uyarıları API](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) bir REST API. Azure Resource Manager REST API ile tamamen uyumludur. Resource Manager cmdlet'ini veya Azure CLI kullanarak PowerShell üzerinden kullanılabilir.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-the-resource-manager-template-with-powershell"></a>PowerShell ile Resource Manager şablonu dağıtma
Önceki gösterilen örnek Resource Manager şablonu dağıtmak için PowerShell kullanmayı [Azure Resource Manager şablonu](#azure-resource-manager-template) bölümünde, aşağıdaki komutu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

Burada sampleActivityLogAlert.parameters.json uyarı kuralı oluşturmak için gereken parametreleri için sağlanan değerler içerir.

### <a name="use-activity-log-powershell-cmdlets"></a>Etkinlik günlüğünü PowerShell cmdlet'lerini kullanma

Etkinlik günlüğü uyarıları adanmış PowerShell cmdlet'leri kullanılabilir:

- [Set-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Set-AzActivityLogAlert): Yeni bir etkinlik günlüğü uyarısı oluşturur veya mevcut bir etkinlik günlüğü uyarısını güncelleştirir.
- [Get-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Get-AzActivityLogAlert): Bir veya daha fazla etkinlik günlük uyarı kaynakları alır.
- [Etkinleştir-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Enable-AzActivityLogAlert): Mevcut bir etkinlik günlüğü uyarısını sağlar ve kendi etiketleri ayarlar.
- [Disable-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Disable-AzActivityLogAlert): Mevcut bir etkinlik günlüğü uyarısını devre dışı bırakır ve kendi etiketleri ayarlar.
- [Remove-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Remove-AzActivityLogAlert): Bir etkinlik günlüğü uyarısı kaldırır.

## <a name="azure-cli"></a>Azure CLI

Azure CLI komutları kümesi altında ayrılmış [az İzleyici etkinlik günlüğü Uyarısı](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert) etkinlik günlüğü uyarı kuralları yönetmek için kullanılabilir.

Yeni bir etkinlik günlüğü uyarı kuralı oluşturmak için bu sırayla aşağıdaki komutları kullanın:

1. [az İzleyici etkinlik günlüğü uyarısı oluşturma](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): Yeni bir etkinlik günlük uyarı kuralı kaynağı oluşturun.
1. [az İzleyici etkinlik günlüğü uyarısı kapsam](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/scope): Oluşturulan etkinlik günlük uyarı kuralı için kapsam ekleyin.
1. [az İzleyici etkinlik günlüğü uyarısı eylem grubu](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/action-group): Etkinlik günlüğü uyarı kuralı için bir eylem grubu ekleyin.

Bir etkinlik günlük uyarı kuralı kaynağı almak için Azure CLI komutunu kullanın. [az İzleyici etkinlik günlüğü Uyarısı Göster](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
). Bir kaynak grubundaki tüm etkinlik günlük uyarı kuralı kaynakları görüntülemek için kullanın [az İzleyici etkinlik günlüğü uyarı listesi](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list).
Azure CLI komutunu kullanarak etkinlik günlük uyarı kuralı kaynakları kaldırılabilir [az İzleyici etkinlik günlüğü uyarısını silme](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [etkinlik günlükleri için Web kancası şeması](../../azure-monitor/platform/activity-log-alerts-webhook.md).
- Okuma bir [etkinlik günlüklerine genel bakış](../../azure-monitor/platform/activity-log-alerts.md).
- Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md).  
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../../azure-monitor/platform/service-notifications.md).
