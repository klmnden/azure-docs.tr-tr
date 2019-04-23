---
title: Azure DevTest Labs'de bir laboratuvar için bir Duyurusu gönderin | Microsoft Docs
description: Duyuru Azure DevTest labs'deki bir laboratuvara ekleme hakkında bilgi edinin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 2fe31271fa84bc4170bd431a4aadbcafc0df9086
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60148977"
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de bir laboratuvar için bir duyuru gönderin

Laboratuvar Yöneticisi olarak, son değişiklikleri veya laboratuvara ekleme konusunda kullanıcıları bilgilendirmek üzere var olan bir laboratuvar içindeki özel bir duyuru gönderebilir. Örneğin, hakkında kullanıcılara bildirmek isteyebilirsiniz:

- Kullanılabilir olan yeni sanal makine boyutları
- Şu anda kullanılamaz durumda görüntüleri
- Laboratuvar ilkeleri güncelleştirmeleri

Sonra gönderilen, duyuru Laboratuvar genel bakış sayfasında görüntülenir ve kullanıcı bunu daha fazla ayrıntı için seçebilirsiniz.

Duyuru özellik geçici bildirimler için kullanılmak üzere tasarlanmıştır.  Artık gerekli olmadığında bir duyuru kolayca devre dışı bırakabilirsiniz.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Mevcut bir laboratuvarda duyuru göndermek için adımları

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. (Laboratuvarınızı zaten altında Panoda görüntülenebilir **tüm kaynakları**).
1. Duyuru gönderin istediğiniz Laboratuvar labs listesinden seçin.
1. Laboratuvar'ın **genel bakış** alanında **yapılandırması ve ilkelerini**.

    ![Yapılandırması ve ilkelerini düğmesi](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. Soldaki altında **ayarları**seçin **Laboratuvar duyuru**.

    ![Laboratuvar duyuru düğmesi](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Bu laboratuvarda kullanıcılar için bir ileti oluşturmak için **etkin** için **Evet**.

1. Girebileceğiniz bir **sona erme tarihi** bir tarih ve saat sonra bildiri artık gösterilen kullanıcıları belirtmek için. Sona erme tarihi girmezseniz, bunu devre dışı kadar duyuruyu kalır.

   > [!NOTE]
   > Duyurunun sona erdikten sonra artık kullanıcılara gösterilir, ancak hala var. **Laboratuvar duyuru** bölmesi. Düzenlemeler yapmak ve yeniden etkinleştirmek için yeniden etkinleştirin.
   >
   >

1. Girin bir **Duyurunun başlığını** ve **duyuru metin**.

   Başlık en fazla 100 karakter olabilir ve Laboratuvar genel bakış sayfasında kullanıcıya gösterilir. Kullanıcı başlığı seçerse, duyuru metni görüntülenir.

   Duyuru metin markdown kabul eder. Duyuru metin girerken, ekranın alt kısmındaki Önizleme alanında ileti görüntüleyebilirsiniz.

    ![İleti oluşturmak üzere Laboratuvar duyuru ekranı.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Seçin **Kaydet** , duyuru gönderilmeye hazır olduğunda.

Artık Laboratuvar kullanıcılara bu duyuru göstermek istediğiniz zaman dönün **Laboratuvar duyuru** sayfasında ve ayarlayın **etkin** için **Hayır**. Sona erme tarihi belirtilen duyuruyu otomatik olarak en, tarih ve saat devre dışı bırakılır.

## <a name="steps-for-users-to-view-an-announcement"></a>Bir duyurunun görüntülemek kullanıcılar için adımları

1. Gelen [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040), Laboratuvar seçin.

1. Laboratuvar için gönderilen bir duyuru varsa, bir bilgi uyarısı Laboratuvar genel bakış sayfasının en üstünde gösterilir. Bu bilgi uyarısı duyuruyu oluştururken belirttiğiniz Duyurunun başlığını ' dir.

    ![Genel bakış sayfasında Laboratuvar Duyurusu](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. Kullanıcının, iletinin tamamı duyuruyu görüntüleyin seçebilirsiniz.

    ![Daha fazla bilgi için laboratuvar Duyurusu](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Aşağıdaki örnekte gösterildiği gibi bir Azure Resource Manager şablonunun bir parçası bir duyuru belirtebilirsiniz:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "string",
            "defaultValue": "devtestlabfromarm"
        },
        "regionId": {
            "type": "string",
            "defaultValue": "eastus"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-04-26-preview",
            "name": "[parameters('name')]",
            "type": "Microsoft.DevTestLab/labs",
            "location": "[parameters('regionId')]",
            "tags": {},
            "properties": {
                "labStorageType": "Premium",
                "announcement":
                {
                    "title": "Important! Three images are currently inaccessible. Click for more information.",
                    "markdown":"# Images are inaccessible\n\nThe following 3 images are currently not available for use: \n\n- image1\n- image2\n- image3\n\nI am working to fix the problem ASAP.",
                    "enabled": "Enabled",
                    "expirationDate":"2018-12-31T06:00:00+00:00",
                    "expired": "false"
                },
                "support": {
                    "markdown": "",
                    "enabled": "Enabled"
                }
            },
            "resources": [
                {
                    "apiVersion": "2017-04-26-preview",
                    "name": "LabVmsShutdown",
                    "location": "[parameters('regionId')]",
                    "type": "schedules",
                    "dependsOn": [
                        "[resourceId('Microsoft.DevTestLab/labs', parameters('name'))]"
                    ],
                    "properties": {
                        "status": "Enabled",
                        "timeZoneId": "Eastern Standard Time",
                        "dailyRecurrence": {
                            "time": "1900"
                        },
                        "taskType": "LabVmsShutdownTask",
                        "notificationSettings": {
                            "status": "Disabled",
                            "timeInMinutes": 30
                        }
                    }
                },
                {
                    "apiVersion": "2017-04-26-preview",
                    "name": "[concat('Dtl', parameters('name'))]",
                    "type": "virtualNetworks",
                    "location": "[parameters('regionId')]",
                    "dependsOn": [
                        "[resourceId('Microsoft.DevTestLab/labs', parameters('name'))]"
                    ]
                }
            ]
        }
    ]
}
```

Aşağıdaki yöntemlerden birini kullanarak bir Azure Resource Manager şablonu dağıtabilirsiniz:

- [Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
- [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
- [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
- [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

## <a name="next-steps"></a>Sonraki adımlar
* Laboratuvar ilkesini ayarlamak veya değiştirirseniz, kullanıcılara bildiren bir duyuru gönderin isteyebilirsiniz. [İlke ve zamanlamalar ayarlama](devtest-lab-set-lab-policy.md) özelleştirilmiş ilkeler kullanılarak aboneliğiniz kısıtlamaları ve kurallarını uygulama hakkında bilgi sağlar.
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates).
