---
title: Azure portalı kullanarak Azure kaynaklarını dağıtın | Microsoft Docs
description: Azure portalı ve Azure Resource Manager kaynaklarınızı dağıtmak için kullanın.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: 7b28129a3afe9f78d0ef749fa0c7759082c5f758
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652459"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma

Bu makalede nasıl kullanılacağını gösterir [Azure portalında](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı dağıtmak için. Kaynaklarınızı yönetme hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak Azure kaynaklarınızı yönetme](manage-resources-portal.md).

## <a name="create-resource-group"></a>Kaynak grubu oluştur

1. Boş bir kaynak grubu oluşturmak için Seç **kaynak grupları**.

   ![Kaynak grubu seçin](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. Kaynak grubu altında seçin **Ekle**.

   ![Kaynak grubu ekle](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. Bir ad ve konum verin ve gerekirse, bir abonelik seçin. Kaynak grubu, kaynaklarla ilgili meta verileri depoladığı için kaynak grubu için bir konum sağlamanız gerekir. Uyumluluk nedenleriyle, meta verilerin nerede depolanacağını belirtmek isteyebilirsiniz. Genel olarak, kaynaklarınızın en bulunacağı bir konum belirtmenizi öneririz. Konumun aynısını kullanarak şablonunuzu basitleştirebilir.

   ![Grup değerlerini ayarlama](./media/resource-group-template-deploy-portal/set-group-properties.png)

   Özellikleri ayarlamayı bitirdiğinizde, seçin **Oluştur**.

1. Yeni kaynak grubunuz görmek için seçin **Yenile**.

   ![Kaynak gruplarını Yenile](./media/resource-group-template-deploy-portal/refresh-resource-groups.png)

## <a name="deploy-resources-from-marketplace"></a>Market kaynakları dağıtma

Bir kaynak grubu oluşturduktan sonra Market'ten için kaynakları dağıtabilirsiniz. Market, yaygın senaryoları için önceden tanımlanmış çözümleri sağlar.

1. Bir dağıtım başlatmak için **kaynak Oluştur**.

   ![Yeni kaynak](./media/resource-group-template-deploy-portal/new-resources.png)

1. Dağıtmak istediğiniz kaynak türünü bulun.

   ![Kaynak türü seçin](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. Dağıtmak istediğiniz belirli çözüm görmüyorsanız, Market arama yapabilirsiniz. Örneğin, bir Wordpress çözüm bulmak için yazmaya başlayın **Wordpress** ve istediğiniz seçeneği belirleyin.

   ![Market'de Ara](./media/resource-group-template-deploy-portal/search-resource.png)

1. Seçilen kaynak türüne bağlı olarak, dağıtım öncesinde ayarlanacak ilgili özellikler koleksiyonu vardır. Tüm türleri için bir hedef kaynak grubu seçmeniz gerekir. Aşağıdaki resimde, bir web uygulaması oluşturma ve oluşturduğunuz kaynak grubuna dağıtma işlemi gösterilmektedir.

   ![Kaynak grubu oluştur](./media/resource-group-template-deploy-portal/select-existing-group.png)

   Alternatif olarak, kaynaklarınızı dağıtırken bir kaynak grubu oluşturmak karar verebilirsiniz. Seçin **Yeni Oluştur** ve kaynak grubuna bir ad verin.

   ![Yeni kaynak grubu oluştur](./media/resource-group-template-deploy-portal/select-new-group.png)

1. Dağıtımınız başlar. Dağıtım birkaç dakika sürebilir. Dağıtım tamamlandığında, bir bildirim görür.

   ![Görünüm bildirimi](./media/resource-group-template-deploy-portal/view-notification.png)

1. Kaynaklarınızı dağıttıktan sonra daha fazla kaynak kaynak grubuna seçerek ekleyebileceğiniz **Ekle**.

   ![Kaynak ekle](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Özel bir şablondan kaynakları dağıtma

Bir dağıtım yürütülürler ancak şablonlardan herhangi birini Market'te kullanmamak istiyorsanız, çözümünüz için altyapıyı tanımlayan özel bir şablon oluşturabilirsiniz. Şablonları oluşturma hakkında bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).

> [!NOTE]
> Portal arabirimi başvuran desteklemeyen bir [Key vault'tan gizli](resource-manager-keyvault-parameter.md). Bunun yerine, [PowerShell](resource-group-template-deploy.md) veya [Azure CLI](resource-group-template-deploy-cli.md) yerel olarak veya dış bir uri'den şablonunuzu dağıtmak için.

1. Portal aracılığıyla özelleştirilmiş bir şablonu dağıtmak için seçebileceğiniz **kaynak Oluştur**, araması **şablon dağıtımı** kadar seçenekler arasından seçim yapabilirsiniz.

   ![Arama şablon dağıtımı](./media/resource-group-template-deploy-portal/search-template.png)

1. **Oluştur**’u seçin.

   ![Oluştur’u seçin](./media/resource-group-template-deploy-portal/show-template-option.png)

1. Bir şablon oluşturmak için çeşitli seçenekler görürsünüz. **Düzenleyicide kendi şablonunuzu oluşturun**'u seçin.

   ![Görüntüleme seçenekleri](./media/resource-group-template-deploy-portal/see-options.png)

1. Özelleştirmek için kullanılabilir olan bir boş şablon var.

   ![Şablon oluşturma](./media/resource-group-template-deploy-portal/blank-template.png)

1. JSON söz dizimi el ile düzenlemeniz veya önceden oluşturulmuş bir şablondan seçim [hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/resources/templates/). Ancak bu makalede kullanmanız **kaynak ekleme** seçeneği.

   ![Şablonu düzenle](./media/resource-group-template-deploy-portal/select-add-resource.png)

1. Seçin **depolama hesabı** ve bir ad sağlayın. Değerleri girmeyi bitirdiğinizde seçin **Tamam**.

   ![Depolama hesabı seçin](./media/resource-group-template-deploy-portal/add-storage-account.png)

1. Düzenleyici JSON kaynak türü için otomatik olarak ekler. Depolama hesabı türünü tanımlamak için bir parametre içerdiğine dikkat edin. **Kaydet**’i seçin.

   ![Şablonu göster](./media/resource-group-template-deploy-portal/show-json.png)

1. Artık, şablonda tanımlanan kaynakları dağıtma seçeneği var. Dağıtın, hüküm ve koşulları kabul ediyorum ve **satın alma**.

   ![Şablon dağıtma](./media/resource-group-template-deploy-portal/provide-custom-template-values.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Hesabınız için kaydedilen bir şablondan kaynakları dağıtma

Portal, Azure hesabınızda bir şablonu kaydedin ve daha sonra yeniden sağlar. Şablonlar hakkında daha fazla bilgi için bkz. [oluşturun ve İlk Azure Resource Manager şablonunuzu dağıtmak](resource-manager-create-first-template.md).

1. Kaydedilen şablonlarınızı bulmak için seçin **diğer hizmetler**.

   ![Diğer hizmetler](./media/resource-group-template-deploy-portal/more-services.png)

1. Arama **şablonları** ve bu seçeneği belirleyin.

   ![Şablon ara](./media/resource-group-template-deploy-portal/find-templates.png)

1. Hesabınıza kaydedilebilir şablonları listesinden üzerinde çalışmak istediğiniz birini seçin.

   ![Kaydedilen şablonları](./media/resource-group-template-deploy-portal/saved-templates.png)

1. Seçin **Dağıt** kaydedilmiş bu şablonu yeniden dağıtmak için.

   ![Kaydedilen bir şablonu dağıtma](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Sonraki adımlar

- Denetim günlüklerini görüntülemek için bkz: [Resource Manager denetim işlemleri](./resource-group-audit.md).
- Dağıtım hatalarını giderme hakkında bilgi için bkz: [dağıtım işlemlerini görüntüleme](./resource-manager-deployment-operations.md).
- Bir dağıtım veya kaynak grubu şablonu dışarı aktarmak için bkz: [dışarı Azure Resource Manager şablonları](./manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Hizmetinizi birden çok bölge arasında güvenli bir şekilde piyasaya çıkma için görmeniz [Azure Deployment Manager](./deployment-manager-overview.md).
