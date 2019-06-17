---
title: Azure portalını kullanarak Azure kaynaklarını yönetmek | Microsoft Docs
description: Azure portalı ve Azure Resource Manager kaynaklarınızı yönetmek için kullanın.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 20bf38b87ce29f8506a5611ecd25cf38f6d4ed61
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60550754"
---
# <a name="manage-azure-resources-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure kaynaklarını yönetme

Nasıl kullanacağınızı öğrenin [Azure portalında](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı yönetmek için. Kaynak grupları yönetmek için bkz: [Azure portalını kullanarak yönetme Azure kaynak grupları](./manage-resource-groups-portal.md).

Kaynakları yönetme hakkında diğer makaleler:

- [Azure CLI kullanarak Azure kaynaklarını yönetme](./manage-resources-cli.md)
- [Azure PowerShell kullanarak Azure kaynaklarını yönetme](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="deploy-resources-to-a-resource-group"></a>Kaynakları bir kaynak grubuna dağıtma

Resource Manager şablonu oluşturduktan sonra Azure portalını kullanarak Azure kaynaklarınızı dağıtmak için kullanabilirsiniz. Bir şablon oluşturmak için bkz: [hızlı başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](./resource-manager-quickstart-create-templates-use-the-portal.md). Portalı kullanarak bir şablonu dağıtmak için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).

## <a name="open-resources"></a>Açık kaynak

Azure kaynakları, kaynak grupları ve Azure Hizmetleri tarafından düzenlenir. Aşağıdaki yordamlar adlı bir depolama hesabı açmak nasıl gösterir **mystorage0207**. Sanal makinenin adında bir kaynak grubunda yer **mystorage0207rg**.

Kaynak türü hizmet tarafından açmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol bölmede, Azure hizmeti seçin. Bu durumda, **depolama hesapları**.  Listelenen hizmet görmüyorsanız seçin **tüm hizmetleri**ve ardından hizmet türünü seçin.

    ![Azure kaynak portalda açın](./media/manage-resources-portal/manage-azure-resources-portal-open-service.png)

3. Açmak istediğiniz kaynağı seçin.

    ![Azure kaynak portalda açın](./media/manage-resources-portal/manage-azure-resources-portal-open-resource.png)

    Bir depolama hesabı şu şekilde görünür:

    ![Azure kaynak portalda açın](./media/manage-resources-portal/manage-azure-resources-portal-open-resource-storage.png)

Bir kaynak tarafından kaynak grubunun açmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol bölmede seçin **kaynak grupları** kaynak grup içindeki listelemek için.
3. Açmak istediğiniz kaynağı seçin. 

## <a name="manage-resources"></a>Kaynakları yönetme

Bir kaynak portalda görüntülerken, bu kaynağa yönetmek için seçeneklere bakın.

![Azure kaynaklarını yönetme](./media/manage-resources-portal/manage-azure-resources-portal-manage-resource.png)

Ekran görüntüsü, bir Azure sanal makinesi için yönetim seçenekleri gösterir. Başlatma, yeniden başlatma ve sanal makinesi durduruluyor gibi işlemler gerçekleştirebilirsiniz.

## <a name="delete-resources"></a>Kaynakları silme

1. Kaynak portalda açın. Adımlar için bkz: [açık kaynak](#open-resources).
2. **Sil**’i seçin. Aşağıdaki ekran görüntüsünde, bir sanal makine yönetim seçenekleri gösterir.

    ![Azure kaynağı silme](./media/manage-resources-portal/manage-azure-resources-portal-delete-resource.png)
3. Silme işlemini onaylayın ve ardından kaynağın adını yazın **Sil**.

Azure Resource Manager kaynakların silinmesini nasıl siparişler hakkında daha fazla bilgi için bkz. [Azure Resource Manager kaynak grubu silme işlemi](./resource-group-delete.md).

## <a name="move-resources"></a>Kaynakları taşıma

1. Kaynak portalda açın. Adımlar için bkz: [açık kaynak](#open-resources).
2. Seçin **taşıma**. Aşağıdaki ekran görüntüsünde, bir depolama hesabı için yönetim seçenekleri gösterir.

    ![Azure kaynak taşıma](./media/manage-resources-portal/manage-azure-resources-portal-move-resource.png)
3. Seçin **başka bir kaynak grubuna taşımak** veya **başka bir aboneliğe Moeve** ihtiyaçlarınıza bağlı olarak.

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

## <a name="lock-resources"></a>Kaynakları kilitleme

Kilitleme yanlışlıkla Azure aboneliği, kaynak grubu ya da kaynağa gibi kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcılar engeller. 

1. Kaynak portalda açın. Adımlar için bkz: [açık kaynak](#open-resources).
2. Seçin **kilitler**. Aşağıdaki ekran görüntüsünde, bir depolama hesabı için yönetim seçenekleri gösterir.

    ![Lock azure kaynağı](./media/manage-resources-portal/manage-azure-resources-portal-lock-resource.png)
3. Seçin **Ekle**ve ardından kilit özelliklerini belirtin.

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Kaynakları etiketleme

Kaynak grubu ve kaynakları mantıksal olarak düzenlemek etiketleme yardımcı olur. 

1. Kaynak portalda açın. Adımlar için bkz: [açık kaynak](#open-resources).
2. **Etiketler**'i seçin. Aşağıdaki ekran görüntüsünde, bir depolama hesabı için yönetim seçenekleri gösterir.

    ![Etiket azure kaynağı](./media/manage-resources-portal/manage-azure-resources-portal-tag-resource.png)
3. Etiket özellikleri belirtin ve ardından **Kaydet**.

Bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](./resource-group-using-tags.md#portal).

## <a name="monitor-resources"></a>Kaynakları izleme

Kaynak açtığınızda, portalın varsayılan grafikleri ve tabloları bu kaynak türü izleme sunar. Aşağıdaki ekran görüntüsünde, bir sanal makine için grafikler gösterir:

![azure İzleyici kaynak](./media/manage-resources-portal/manage-azure-resources-portal-monitor-resource.png)

Grafikleri panoya grafik sabitleme sağ üst köşesinde bulunan Raptiye simgesini de seçebilirsiniz. Panolar ile çalışma hakkında bilgi edinmek için [oluşturma ve Azure Portalı'ndaki pano paylaşma](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-access-to-resources"></a>Kaynaklara erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).