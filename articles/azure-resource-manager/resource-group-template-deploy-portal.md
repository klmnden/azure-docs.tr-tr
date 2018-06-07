---
title: Azure kaynaklarınızı dağıtmak için Azure portalını kullanma | Microsoft Docs
description: Azure portalı ve Azure Resource Manager kaynaklarınızı dağıtmak için kullanın.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2017
ms.author: tomfitz
ms.openlocfilehash: 79bc42394513efc2ac03ea9d7170f035d71edb4f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34603739"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma

Bu konuda nasıl kullanılacağını gösterir [Azure portal](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı dağıtmak için. Kaynaklarınızın yönetilmesi hakkında bilgi edinmek için [yönetmek Azure kaynakları portal üzerinden](resource-group-portal.md).

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

1. Boş bir kaynak grubu oluşturmak için seçin **kaynak grupları**.

   ![Kaynak gruplarını seçin](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. Kaynak grubu altında seçin **Ekle**.

   ![Kaynak Grubu Ekle](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. Bir ad ve konum girin ve gerekiyorsa, bir abonelik seçin. Kaynak grubu kaynaklarla ilgili meta verileri depoladığından kaynak grubu için bir konum sağlamanız gerekir. Uyumluluk nedenleriyle meta verilerin nerede depolanacağını belirtmek isteyebilirsiniz. Genel olarak, çoğu kaynaklarınızın bulunacağı bir konum belirtin öneririz. Aynı konuma kullanarak şablonunuzu basitleştirebilirsiniz.

   ![kümesi Grup değerleri](./media/resource-group-template-deploy-portal/set-group-properties.png)

   Özellikleri ayarlama tamamladığınızda seçin **oluşturma**.

1. Yeni kaynak grubunuz görmek için seçin **yenileme**.

   ![Kaynak grupları Yenile](./media/resource-group-template-deploy-portal/refresh-resource-groups.png)

## <a name="deploy-resources-from-marketplace"></a>Market kaynaklardan dağıtma

Bir kaynak grubu oluşturduktan sonra kendisine marketten kaynakları dağıtabilirsiniz. Market yaygın senaryoları için önceden tanımlanmış çözümleri sağlar.

1. Bir dağıtımı başlatmak için **kaynak oluşturma**.

   ![Yeni kaynak](./media/resource-group-template-deploy-portal/new-resources.png)

1. Dağıtmak istediğiniz kaynak türünü bulun.

   ![Kaynak türü seçin](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. Dağıtmak istediğiniz belirli çözüm görmüyorsanız, Market arayabilirsiniz. Örneğin, Wordpress çözüm bulmak için yazmaya başlayın **Wordpress** ve istediğiniz seçeneği seçin.

   ![Arama Market](./media/resource-group-template-deploy-portal/search-resource.png)

1. Seçilen kaynak türüne bağlı olarak, dağıtım öncesinde ayarlamak için ilgili özellikleri koleksiyonu vardır. Tüm türleri için bir hedef kaynak grubu seçmelisiniz. Aşağıdaki resimde, bir web uygulaması oluşturmak ve oluşturduğunuz kaynak grubuna dağıtmak gösterilmiştir.

   ![Kaynak grubu oluşturma](./media/resource-group-template-deploy-portal/select-existing-group.png)

   Alternatif olarak, kaynaklarınızın dağıtırken bir kaynak grubu oluşturmak için karar verebilirsiniz. Seçin **Yeni Oluştur** ve kaynak grubu bir ad verin.

   ![Yeni kaynak grubu oluştur](./media/resource-group-template-deploy-portal/select-new-group.png)

1. Dağıtımınızı başlar. Dağıtımın birkaç dakika sürebilir. Dağıtım tamamlandığında, bir bildirim görür.

   ![Görünüm bildirim](./media/resource-group-template-deploy-portal/view-notification.png)

1. Kaynaklarınızı dağıttıktan sonra daha fazla kaynak kaynak grubuna seçerek ekleyebilirsiniz **Ekle**.

   ![Kaynak ekle](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Özel şablon kaynaklardan dağıtma

Bir dağıtım yürütme ancak şablonlardan herhangi birini markette kullanmamak istiyorsanız, çözümünüz için altyapıyı tanımlayan özelleştirilmiş bir şablon oluşturabilirsiniz. Şablonları oluşturma hakkında bilgi edinmek için [yapısı ve Azure Resource Manager şablonları sözdizimini anlamanız](resource-group-authoring-templates.md).

1. Özelleştirilmiş bir şablon portal üzerinden dağıtmak için seçin **kaynak oluşturma**, arayın ve **şablon dağıtımı** kadar seçenekler arasından seçim yapabilirsiniz.

   ![Arama şablon dağıtımı](./media/resource-group-template-deploy-portal/search-template.png)

1. **Oluştur**’u seçin.

   ![Oluştur’u seçin](./media/resource-group-template-deploy-portal/show-template-option.png)

1. Bir şablon oluşturmak için birkaç seçenek bakın. Seçin **kendi şablonunuzu Düzenleyicisi'nde yapı**.

   ![Görünüm Seçenekleri](./media/resource-group-template-deploy-portal/see-options.png)

1. Özelleştirme için kullanılabilir boş bir şablon var.

   ![Şablon oluşturma](./media/resource-group-template-deploy-portal/blank-template.png)

1. JSON söz dizimi manuel olarak düzenlemeniz veya önceden oluşturulmuş bir şablondan seçin [hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/resources/templates/). Ancak, bu makalede, kullandığınız **kaynak ekleyin** seçeneği.

   ![Şablonu düzenle](./media/resource-group-template-deploy-portal/select-add-resource.png)

1. Seçin **depolama hesabı** ve bir ad sağlayın. Değerleri sağlayarak bittiğinde seçin **Tamam**.

   ![Depolama hesabı seçme](./media/resource-group-template-deploy-portal/add-storage-account.png)

1. Düzenleyici JSON kaynak türü için otomatik olarak ekler. Depolama hesabı türünü tanımlamak için bir parametre içerdiğine dikkat edin. **Kaydet**’i seçin.

   ![Şablonu göster](./media/resource-group-template-deploy-portal/show-json.png)

1. Şimdi, şablonda tanımlanan kaynaklara dağıtmak için seçeneğiniz vardır. Dağıtmak, hüküm ve koşulları kabul ve seçmek için **satın alma**.

   ![Şablon dağıtma](./media/resource-group-template-deploy-portal/provide-custom-template-values.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Hesabınız için kaydedilen bir şablon kaynaklardan dağıtma

Portal, Azure hesabınızda bir şablonu kaydetmek ve daha sonra yeniden dağıtmak sağlar. Şablonlar hakkında daha fazla bilgi için bkz: [oluşturma ve İlk Azure Resource Manager şablonu dağıtma](resource-manager-create-first-template.md).

1. Kaydedilen şablonlarınızı bulmak için seçin **daha fazla hizmet**.

   ![Diğer hizmetler](./media/resource-group-template-deploy-portal/more-services.png)

1. Arama **şablonları** ve bu seçeneği belirleyin.

   ![Şablon ara](./media/resource-group-template-deploy-portal/find-templates.png)

1. Hesabınıza kaydedilebilir şablonları listesinden, üzerinde çalışmak istediğiniz birini seçin.

   ![kaydedilen şablonları](./media/resource-group-template-deploy-portal/saved-templates.png)

1. Seçin **dağıtma** kaydedilmiş bu şablonu yeniden dağıtılamadı.

   ![Kaydedilmiş şablonu dağıtma](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Sonraki adımlar
* Denetim günlüklerini görüntülemek için bkz: [denetim işlemleri Resource Manager ile](resource-group-audit.md).
* Dağıtım hataları gidermek için bkz: [görüntülemek dağıtım işlemlerini](resource-manager-deployment-operations.md).
* Bir dağıtım veya kaynak grubunu şablon almak için bkz: [Azure Resource Manager şablonunu dışarı mevcut kaynaklardan](resource-manager-export-template.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).
