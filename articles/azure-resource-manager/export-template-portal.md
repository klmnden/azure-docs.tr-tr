---
title: Azure portalını kullanarak Azure Resource Manager şablonunu dışarı aktarma
description: Bir Azure Resource Manager şablonu, aboneliğinizdeki kaynaklar dışarı aktarmak için Azure portalını kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: tomfitz
ms.openlocfilehash: ea9499da3dac67635a48704f439f6592c6ed467e
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65515392"
---
# <a name="single-and-multi-resource-export-to-template-in-azure-portal"></a>Azure portalında şablon için tek ve birden çok kaynak dışarı aktarma

Azure Resource Manager şablonları oluşturmaya yardımcı olmak için var olan kaynaklardan bir şablonu dışarı aktarabilirsiniz. Dışarı aktarılan şablon JSON söz dizimi ve kaynaklarınızı dağıtma özelliklerini anlamanıza yardımcı olur. Gelecekteki dağıtımlar otomatikleştirmek için dışarı aktarılan şablonu ile başlayın ve senaryonuz için değiştirin.

Resource Manager için bir şablonu dışarı aktarma için bir veya daha fazla kaynak seçmenizi sağlar. Tam olarak şablonda ihtiyacınız olan kaynakları üzerinde odaklanabilirsiniz.

## <a name="choose-the-right-export-option"></a>Doğru dışa aktarma seçeneği seçin

Şablonu dışarı aktarmanın iki yolu vardır:

* **Kaynak grubu ya da kaynak dışarı aktarma**. Bu seçenek, mevcut kaynaklardan yeni bir şablon oluşturur. Dışarı aktarılan kaynak grubunun geçerli durumu "snapshot" şablonudur. Bir kaynak grubunun tamamını ya da kaynak grubu içindeki belirli kaynaklar dışarı aktarabilirsiniz.

* **Dağıtımdan önce veya geçmişinden dışarı aktarma**. Bu seçenek, dağıtım için kullanılan bir şablon tam bir kopyasını alır.

Belirlediğiniz seçeneğe bağlı olarak, dışarı aktarılan şablonları farklı niteliklere sahip.

| Kaynak grubuna ya da kaynağa | Dağıtımdan önce veya geçmişi |
| --------------------- | ----------------- |
| Şablon anlık görüntü kaynakların geçerli durumu. Bu, herhangi bir el ile dağıtımdan sonra yapılan değişiklikler içerir. | Şablon, dağıtım sırasında yalnızca kaynakların durumunu gösterir. Dağıtımı olmayan sonra dahil yaptığınız tüm el ile yapılan değişiklikler. |
| Bir kaynak grubundan dışarı aktarmak için hangi kaynakların seçebilirsiniz. | Belirli bir dağıtımı için tüm kaynakları dahil edilir. Bu kaynakların alt kümesini seçin veya farklı bir zamanda eklenen kaynakları ekleyin. |
| Şablon dağıtımı sırasında ayarladığınız normalde mıydı bazı özellikler dahil olmak üzere, kaynakların tüm özellikleri içerir. Kaldırın veya şablonu yeniden kullanmadan önce bu özelliklerini temizleme isteyebilirsiniz. | Şablon dağıtımı için gerekli özellikleri içerir. Kullanıma hazır şablonudur. |
| Şablon, büyük olasılıkla tüm için yeniden kullanılması gereken parametreler içermez. Çoğu özellik değerleri, şablonda sabit kodlanmış. Diğer ortamlarda şablonu yeniden dağıtmak için kaynakları yapılandırma yeteneğini artırmak parametrelerini eklemeniz gerekir. | Şablonu yeniden dağıtmak farklı ortamlarda kolaylaştırır parametreleri içerir. |

Bir kaynak grubu ya da kaynak şablonu dışarı aktarma olduğunda:

* Kaynakları özgün dağıtım sonrasında yapılan değişiklikleri yakalamak gerekir.
* Hangi kaynakların dışarı seçmek istiyorum.

Dağıtımdan önce veya geçmişinden şablonu dışarı aktarma olduğunda:

* Bir kolayca yeniden şablonu kullanmanız gerekir.
* Özgün dağıtım sonrasında yaptığınız değişiklikleri eklemeniz gerekmez.

## <a name="export-template-from-resource-group"></a>Şablonu kaynak grubundan dışarı aktarma

Bir veya daha fazla kaynak bir kaynak grubundan dışarı aktarmak için:

1. Dışarı aktarmak istediğiniz kaynakları içeren kaynak grubunu seçin.

1. Kaynak grubundaki tüm kaynakları dışarı aktarmak için tüm seçin ve ardından **şablonu dışarı aktarma**. **Şablonu dışarı aktarma** seçenek haline gelir etkinleştirilebilmesi için en az bir kaynak seçtikten sonra.

   ![Tüm kaynakları Dışarı Aktar](./media/export-template-portal/select-all-resources.png)

1. Dışarı aktarma için belirli kaynaklara seçmek için bu kaynakları yanındaki onay kutularını seçin. Ardından, **şablonu dışarı aktarma**.

   ![Dışarı aktarılacak kaynakları seçin](./media/export-template-portal/select-resources.png)

1. Dışarı aktarılan şablon, görüntülenir ve indirilebilir.

   ![Şablonu göster](./media/export-template-portal/show-template.png)

## <a name="export-template-from-resource"></a>Şablonu kaynak grubundan dışarı aktarma

Bir kaynak dışarı aktarmak için:

1. Dışarı aktarmak istediğiniz kaynak içeren kaynak grubunu seçin.

1. Dışarı aktarmak için kaynağı seçin.

   ![Kaynak seçin](./media/export-template-portal/select-link-resource.png)

1. Bu kaynak için seçin **şablonu dışarı aktarma** sol bölmesinde.

   ![Kaynak dışarı aktarma](./media/export-template-portal/export-single-resource.png)

1. Dışarı aktarılan şablon, görüntülenir ve indirilebilir. Bu şablon, yalnızca tek bir kaynak içerir.

## <a name="export-template-before-deployment"></a>Dağıtımdan önce şablonu dışarı aktarma

1. Dağıtmak istediğiniz Azure hizmeti seçin.

1. Yeni hizmet için değerleri girin.

1. Doğrulama geçirmeden sonra ancak dağıtıma başlamadan önce seçin **Otomasyon için bir şablonunu indirebilirsiniz**.

   ![Şablonu indir](./media/export-template-portal/download-before-deployment.png)

1. Şablon görüntülenir ve indirme için kullanılabilir.

   ![Şablonu göster](./media/export-template-portal/show-template-before-deployment.png)

## <a name="export-template-after-deployment"></a>Dağıtımdan sonra şablonu dışarı aktarma

Var olan kaynakları dağıtmak için kullanılan şablonu dışarı aktarabilirsiniz. Size tam dağıtım için kullanılan bir şablonudur.

1. Dışarı aktarmak istediğiniz kaynak grubunu seçin.

1. Altındaki bağlantıyı seçin **dağıtımları**.

   ![Dağıtım geçmişi seçin](./media/export-template-portal/select-deployment-history.png)

1. Dağıtımlardan biri dağıtım geçmişinden seçin.

   ![Dağıtımını seçin](./media/export-template-portal/select-details.png)

1. Seçin **şablon**. Bu dağıtım için kullanılan şablon görüntülenir ve indirme için kullanılabilir.

   ![Şablon seçin](./media/export-template-portal/show-template-from-history.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).