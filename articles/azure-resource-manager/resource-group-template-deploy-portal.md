---
title: Azure portalı kullanarak Azure kaynaklarını dağıtın | Microsoft Docs
description: Azure portalı ve Azure Resource Manager kaynaklarınızı dağıtmak için kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: tomfitz
ms.openlocfilehash: a171d9b4f054c942eebb08e7e11dd1164545f408
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67460455"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma

Nasıl kullanacağınızı öğrenin [Azure portalında](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı dağıtmak için. Kaynaklarınızı yönetme hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak Azure kaynaklarınızı yönetme](manage-resources-portal.md).

Genellikle Azure portalını kullanarak Azure kaynaklarını dağıtma iki adımdan oluşur:

- Bir kaynak grubu oluşturun.
- Kaynaklar kaynak grubuna dağıtın.

Ayrıca, Azure kaynaklarını oluşturmak için bir Azure Resource Manager şablonu dağıtabilirsiniz.

Bu makalede her iki yöntem de gösterilir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. Yeni bir kaynak grubu oluşturmak için Seç **kaynak grupları** gelen [Azure portalında](https://portal.azure.com).

   ![Kaynak grubu seçin](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. Kaynak grubu altında seçin **Ekle**.

   ![Kaynak Grubu Ekle](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. Seçin veya aşağıdaki özellik değerlerini girin:

    - **Abonelik**: Bir Azure aboneliği seçin.
    - **Kaynak grubu**: Kaynak grubu, bir ad verin.
    - **Bölge**: Bir Azure konumu belirtin. Kaynak grubu, kaynaklarla ilgili meta verileri depoladığı budur. Uyumluluk nedenleriyle, meta verilerin nerede depolanacağını belirtmek isteyebilirsiniz. Genel olarak, kaynaklarınızın en bulunacağı bir konum belirtmenizi öneririz. Konumun aynısını kullanarak şablonunuzu basitleştirebilir.

   ![Grup değerlerini ayarlama](./media/resource-group-template-deploy-portal/set-group-properties.png)

1. **İncele ve oluştur**’u seçin.
1. değerleri inceleyin ve ardından **Oluştur**.
1. Seçin **Yenile** yeni kaynak grubu listesinde görebilmek için önce.

## <a name="deploy-resources-to-a-resource-group"></a>Kaynakları bir kaynak grubuna dağıtma

Bir kaynak grubu oluşturduktan sonra kaynak grubuna marketten dağıtabilirsiniz. Market, yaygın senaryoları için önceden tanımlanmış çözümleri sağlar.

1. Bir dağıtım başlatmak için **kaynak Oluştur** gelen [Azure portalında](https://portal.azure.com).

   ![Yeni kaynak](./media/resource-group-template-deploy-portal/new-resources.png)

1. Dağıtmak istediğiniz kaynak türünü bulun. Kaynakları kategoriler halinde düzenlenmiştir. Dağıtmak istediğiniz belirli çözüm görmüyorsanız, Market arama yapabilirsiniz. Aşağıdaki ekran görüntüsünde, Ubuntu Server'ın seçildiğini gösterir.

   ![Kaynak türü seçin](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. Seçilen kaynak türüne bağlı olarak, dağıtım öncesinde ayarlanacak ilgili özellikler koleksiyonu vardır. Tüm türleri için bir hedef kaynak grubu seçmeniz gerekir. Aşağıdaki resimde, bir Linux sanal makine oluşturmak ve oluşturduğunuz kaynak grubunu dağıtma işlemi gösterilmektedir.

   ![Kaynak grubu oluşturma](./media/resource-group-template-deploy-portal/select-existing-group.png)

   Alternatif olarak, kaynaklarınızı dağıtırken bir kaynak grubu oluşturmak karar verebilirsiniz. Seçin **Yeni Oluştur** ve kaynak grubuna bir ad verin.

1. Dağıtımınız başlar. Dağıtım birkaç dakika sürebilir. Bazı kaynaklar diğer kaynaklara daha uzun sürer. Dağıtım tamamlandığında, bir bildirim görür. Seçin **kaynağa Git** açmak için

   ![Görünüm bildirimi](./media/resource-group-template-deploy-portal/view-notification.png)

1. Kaynaklarınızı dağıttıktan sonra daha fazla kaynak kaynak grubuna seçerek ekleyebileceğiniz **Ekle**.

   ![Kaynak ekle](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Özel bir şablondan kaynakları dağıtma

Bir dağıtım yürütülürler ancak şablonlardan herhangi birini Market'te kullanmamak istiyorsanız, çözümünüz için altyapıyı tanımlayan özel bir şablon oluşturabilirsiniz. Şablonları oluşturma hakkında bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).

> [!NOTE]
> Portal arabirimi başvuran desteklemeyen bir [Key vault'tan gizli](resource-manager-keyvault-parameter.md). Bunun yerine, [PowerShell](resource-group-template-deploy.md) veya [Azure CLI](resource-group-template-deploy-cli.md) yerel olarak veya dış bir uri'den şablonunuzu dağıtmak için.

1. Portal aracılığıyla özelleştirilmiş bir şablonu dağıtmak için seçebileceğiniz **kaynak Oluştur**, arama **şablon**. ve ardından **şablon dağıtımı**.

   ![Arama şablon dağıtımı](./media/resource-group-template-deploy-portal/search-template.png)

1. **Oluştur**’u seçin.
1. Bir şablon oluşturmak için birkaç seçenek görürsünüz:

    - **Düzenleyicide kendi şablonunuzu oluşturun**: Düzenleyicisi portal şablonu kullanarak bir şablon oluşturun.  Düzenleyici kaynak şablon şeması ekleme yeteneğine sahiptir.
    - **Ortak şablonlar**: Linux sanal makinesi, Windows sanal makine, bir web uygulaması ve Azure SQL veritabanı oluşturmak için dört ortak templatess vardır.
    - **GitHub Hızlı Başlangıç şablonu Yükle**: mevcut bir kullanın [hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/).

   ![Görüntüleme seçenekleri](./media/resource-group-template-deploy-portal/see-options.png)

    Bu öğretici Hızlı Başlangıç şablonu yüklemeye yönelik yönerge sağlar.

1. Altında **GitHub Hızlı Başlangıç şablonu Yükle**yazın veya seçin **101-storage-hesap-oluşturma**.

    İki seçeneğiniz vardır:

    - **Şablon Seç**: şablonu dağıtmak.
    - **Şablonu Düzen**: dağıtmadan önce Hızlı Başlangıç şablonu düzen.

1. Seçin **şablonu Düzen** portal şablonu Düzenleyicisi'ni keşfedin. Şablonu Düzenleyicisi'nde yüklenir. İki parametre olduğuna dikkat edin: **storageAccountType** ve **konumu**.

   ![Şablon oluşturma](./media/resource-group-template-deploy-portal/show-json.png)

1. Küçük bir değişiklik yapın. Örneğin, güncelleştirme **storageAccountName** değişkenini:

    ```json
    "storageAccountName": "[concat('azstore', uniquestring(resourceGroup().id))]"
    ```

1. **Kaydet**’i seçin. Portal şablon dağıtımı arabirimi göreceksiniz. Şablonda tanımladığınız iki parametre dikkat edin.
1. Özellik değerleri seçin veya girin:

    - **Abonelik**: Bir Azure aboneliği seçin.
    - **Kaynak grubu**: Seçin **Yeni Oluştur** ve bir ad verin.
    - **Konum**: Bir Azure konumu seçin.
    - **Depolama hesabı türü**: Varsayılan değeri kullanın.
    - **Konum**: Varsayılan değeri kullanın.
    - **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: (seçin)

1. **Satın al**'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Denetim günlüklerini görüntülemek için bkz: [Resource Manager denetim işlemleri](./resource-group-audit.md).
- Dağıtım hatalarını giderme hakkında bilgi için bkz: [dağıtım işlemlerini görüntüleme](./resource-manager-deployment-operations.md).
- Bir dağıtım veya kaynak grubu şablonu dışarı aktarmak için bkz: [dışarı Azure Resource Manager şablonları](./manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Güvenli bir şekilde, bir hizmetin ölçeğini birden çok bölgede toplamak için bkz: [Azure Deployment Manager](./deployment-manager-overview.md).
