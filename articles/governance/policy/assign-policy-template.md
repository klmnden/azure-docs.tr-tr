---
title: Resource Manager şablonu ile bir ilke ataması oluşturma
description: Bu makalede, uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturmak için Resource Manager şablonu kullanmak için adımları gösterilmektedir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/13/2019
ms.topic: quickstart
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: f31d6197c22be4d66e0610ad7914f541a45ed995
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979566"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-by-using-a-resource-manager-template"></a>Hızlı Başlangıç: Resource Manager şablonu kullanarak uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturma

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile *uyumsuzdur*.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturma ve adlı yerleşik ilke tanımı atama *denetim yönetilen diskleri kullanmayan Vm'leri*. Kullanılabilir yerleşik ilkeler kısmi bir listesi için bkz. [Azure ilkesi örnekleri](./samples/index.md).

İlke ataması oluşturmak için çeşitli yöntemler vardır. Bu hızlı başlangıçta, kullandığınız bir [Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/).
Şablonun bir kopyasını şu şekildedir:

[!code-json[policy-assignment](~/quickstart-templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/azuredeploy.json)]

> [!NOTE]
> Azure İlkesi Hizmeti ücretsiz olarak kullanılabilir.  Daha fazla bilgi için [Azure İlkesi Genel Bakış](./overview.md).

1. Aşağıdaki görüntüde Azure portalında oturum açın ve bir şablonu açmak için seçin:

   [![İlke şablonu Azure'a dağıtma](./media/assign-policy-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurepolicy-assign-builtinpolicy-resourcegroup%2Fazuredeploy.json)

1. Seçin veya aşağıdaki değerleri girin:

   | Ad | Değer |
   |------|-------|
   | Abonelik | Azure aboneliğinizi seçin. |
   | Kaynak grubu | Seçin **Yeni Oluştur**bir ad belirtin ve ardından **Tamam**. Kaynak grubu adı ekran görüntüsünde olduğu *mypolicyquickstart\<MMDD tarih > rg*. |
   | Location | Bölge seçin. Örneğin, **Orta ABD**. |
   | İlke ataması adı | Bir ilke ataması adı belirtin. İsterseniz, ilke tanımı görünen kullanabilirsiniz. Örneğin, **denetim yönetilen diskleri kullanmayan Vm'leri**. |
   | Rg adı | İlkeyi atamak istediğiniz yerin bir kaynak grubu adı belirtin. Bu hızlı başlangıçta, varsayılan değeri kullanın **[resourceGroup () .name]**. **[resourceGroup()](../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)**  kaynak grubunu alır. bir şablon işlevi. |
   | İlke tanım kimliği | Belirtin **/providers/Microsoft.Authorization/policyDefinitions/0a914e76-4921-4c19-b460-a2d36003525a**. |
   | Yukarıda belirtilen hüküm ve koşulları kabul ediyorum | (Seç) |

1. **Satın al**'ı seçin.

Ek kaynaklar şunladır:

- Daha fazla örnek şablonu bulmak için bkz: [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular).
- Şablon başvurusu görmek için Git [Azure şablon başvurusu](/azure/templates/microsoft.authorization/allversions).
- Resource Manager şablonları geliştirme hakkında bilgi edinmek için [Azure Resource Manager belgelerini](/azure/azure-resource-manager/).
- Abonelik düzeyinde dağıtım bilgi edinmek için [oluşturma kaynak grubu ve kaynak abonelik düzeyinde](../../azure-resource-manager/deploy-to-subscription.md).

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Seçin **Uyumluluk** sayfanın sol tarafındaki. Ardından bulun **denetim yönetilen diskleri kullanmayan Vm'leri** oluşturduğunuz ilke ataması.

![İlke uyumluluk genel bakış sayfası](./media/assign-policy-template/policy-compliance.png)

Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa, altında görünür **uyumlu olmayan kaynaklar**.

Daha fazla bilgi için [uyumluluk nasıl çalıştığını](./how-to/get-compliance-data.md#how-compliance-works).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz atamayı kaldırmak için aşağıdaki adımları izleyin:

1. Azure İlkesi sayfasının sol tarafından **Uyumluluk**’u (veya **Atamalar**’ı) seçin ve oluşturduğunuz **Yönetilen disk kullanmayan VM'leri denetle** ilke atamasını bulun.

1. Sağ **denetim yönetilen diskleri kullanmayan Vm'leri** ilke ataması ve select **atamayı Sil**.

   ![Uyumluluk genel bakış sayfasından bir atamayı Sil](./media/assign-policy-template/delete-assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir kapsama yerleşik ilke tanımı atadınız ve kendi uyumluluk raporu değerlendirilir. İlke tanımı, kapsamdaki tüm kaynakların uyumlu olan ve olmayanları tanımlayan doğrular.

Yeni kaynakların uyumlu olduğunu doğrulamak için ilkeleri atama hakkında daha fazla bilgi için öğreticisiyle devam edin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)