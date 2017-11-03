---
title: "Azure dağıtım hataları anlama | Microsoft Docs"
description: "Azure dağıtım hataları hakkında bilgi edinmek açıklar."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Dağıtım hatası, azure dağıtım azure'a dağıtma"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: b67bb30fa259fa08e37e11afec724c8b8c3eb633
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understand-azure-deployment-errors"></a>Azure dağıtım hataları anlama
Bu konu, dağıtım hatalarını ve hata hakkında daha fazla bilgi nasıl bulabilir açıklar. Ortak dağıtım hataları için çözümler için bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>İki tür hataları
İki tür alabileceğiniz hataları vardır:

* Doğrulama hataları
* Dağıtım hataları

Aşağıdaki resimde bir abonelik için etkinlik günlüğü gösterilmektedir. Bu iki dağıtım temsil eder. Bir dağıtımda, şablon, doğrulama başarısız oldu (**doğrulama**) ve devam. Başka bir dağıtım şablonu, doğrulamayı geçen ancak kaynakları oluşturma başarısız oldu (**yazma dağıtımları**). 

![Hata Kodu Göster](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Dağıtım öncesinde belirlenebilir senaryolardan doğrulama hataları ortaya çıkar. Sözdizimi hataları şablonunuzu veya abonelik kotalarında aşacak kaynakları dağıtılmaya çalışılırken içerirler. Dağıtım işlemi sırasında ortaya koşullar uygulamadaki dağıtım hataları kaynaklanır. Paralel olarak dağıtılan bir kaynağa erişmeye içerirler.

Her iki tür hataları dağıtım sorunlarını giderme için kullandığınız bir hata kodunu döndürür. Her iki tür hataları görünür [etkinlik günlüğü](resource-group-audit.md). Ancak, dağıtım hiçbir zaman başlamadı çünkü doğrulama hataları dağıtım geçmişiniz görünmez.

## <a name="determine-error-code"></a>Hata kodu belirleme

Hata iletisi ve hata kodu bakarak bir hata hakkında bilgi edinebilirsiniz. [Ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md) makalede hata koduna göre çözümler listelenmektedir. Bu konuda hata kodu bulmak için Azure Portalı'nı kullanmayı gösterir.

### <a name="validation-errors"></a>Doğrulama hataları

Portalı aracılığıyla dağıtırken değerlerinizi gönderdikten sonra bir doğrulama hatası bakın.

![Portal doğrulama hatasını göster](./media/resource-manager-troubleshoot-tips/validation-error.png)

Daha fazla ayrıntı için iletiyi seçin. Aşağıdaki resimde gördüğünüz bir **InvalidTemplateDeployment** hata ve bir ilke belirten bir ileti dağıtım engellendi.

![Doğrulama ayrıntıları göster](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Dağıtım hataları

İşlemi doğrulama başarılı olur, ancak dağıtım sırasında başarısız olduğunda, bildirimler hataya bakın. Bildirim seçin.

![bildirim hatası](./media/resource-manager-troubleshoot-tips/notification.png)

Dağıtım hakkında daha fazla ayrıntı bakın. Hata hakkında daha fazla bilgi bulmak için bu seçeneği belirleyin.

![dağıtım başarısız oldu](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Hata iletisi ve hata kodları bakın. İki hata kodları dikkat edin. İlk hata kodu (**DeploymentFailed**) hatayı çözmek için gereken ayrıntıları sağlamaz genel bir hatadır. İkinci hata kodunu (**StorageAccountNotFound**), gereksinim ayrıntıları sağlar. 

![Hata ayrıntıları](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>Hata ayıklama günlük kaydını etkinleştir
Bazen istek ve yanıt nelerin yanlış gittiğini bulmak için hakkında daha fazla bilgi gerekiyor. PowerShell veya Azure CLI kullanarak bir dağıtım sırasında günlüğe ek bilgiler isteyebilir.

- PowerShell

   PowerShell'de ayarlamak **DeploymentDebugLogLevel** tüm parametresi, ResponseContent veya RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Aşağıdaki cmdlet ile içerik isteği inceleyin:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Veya, ile içerik yanıtı:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Bu bilgiler şablondaki değeri yanlış ayarlanmış olup olmadığını belirlemenize yardımcı olabilir.

- Azure CLI

   Aşağıdaki komutla dağıtım işlemlerini inceleyin:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- İç içe geçmiş şablonu

   İç içe geçmiş bir şablon için hata ayıklama bilgileri oturum açmak için kullandığınız **debugSetting** öğesi.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Sorun giderme şablonu oluşturma
Bazı durumlarda, en kolay yolu, şablonunuzu gidermek için bazı bölümleri test etmektir. Oluşturabileceğiniz basitleştirilmiş bir hata olduğunu düşündüğünüz parçası üzerinde odaklanmanıza olanak tanıyan şablon neden olan. Örneğin, bir kaynak başvururken bir hata alıyorsunuz varsayalım. Tüm şablon postalarla yerine, soruna parçası döndürür bir şablon oluşturun. Şablon işlevleri doğru kullanarak sağ parametrelerinde geçtiğiniz ve beklediğiniz kaynak alınıyor belirlemenize yardımcı olabilir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Ya da yanlış bağımlılıkları ayarlamak üzere ilgili düşünüyorsanız dağıtım hataları karşılaşıyoruz varsayalım. Basitleştirilmiş şablonlara bölerek şablonunuzu test edin. İlk olarak, yalnızca tek bir kaynak (bir SQL sunucu gibi) dağıtan bir şablon oluşturun. Bu kaynağı doğru tanımlı sahip olduğunuzdan emin olduğunuzda (SQL veritabanı gibi) bağımlı bir kaynak ekleyin. Doğru tanımlanmadı iki kaynaklarla varsa, bağımlı diğer kaynaklar (örneğin, denetim ilkeleri) ekleyin. Her sınama dağıtımı arasında yeterli bağımlılıkları sınama emin olmak için kaynak grubunu silin. 

## <a name="check-deployment-sequence"></a>Onay dağıtım sırası

Kaynakları içinde beklenmeyen bir sıra dağıtılan birçok dağıtım hata meydana gelir. Bağımlılıklar doğru ayarlanmadı, bu hatalar ortaya çıkar. Gerekli bir bağımlılığı eksik olduğunda, başka bir kaynak için bir değer kullanmak bir kaynak çalışır ancak diğer henüz yok. Bir kaynağın bulunamadığını bildiren bir hata alırsınız. Bu tür hatalara zaman zaman karşılaşabilirsiniz her kaynak için dağıtım süresini değişebileceğinden. Örneğin, gerekli bir kaynağa zamanında rastgele tamamladığı için kaynaklarınızı dağıtmak için ilk deneme başarılı. Ancak, gerekli kaynak sürede tamamlanmadığı girişiminiz başarısız olur. 

Ancak, gerekli olmayan bağımlılıklarını ayarlama önlemek istiyorsanız. Gereksiz bağımlılıkları varsa, paralel olarak dağıtılan birbirlerine bağımlı olmayan kaynakları engelleyerek dağıtım süresini uzatmak. Ayrıca, dağıtım engelleme döngüsel bağımlılıklar oluşturabilir. [Başvuru](resource-group-template-functions-resource.md#reference) işlevi bu kaynakla aynı şablonunda dağıtıldığında bu dolaylı bir bağımlılığı başvurulan kaynakta oluşturur. Bu nedenle, bağımlılıkları, belirtilenden daha fazla bağımlılıkları olabilir **dependsOn** özelliği. [ResourceId](resource-group-template-functions-resource.md#resourceid) işlevi oluşturmaz dolaylı bir bağımlılığı veya kaynak var olduğunu doğrulayın.

Bağımlılık sorunlarını karşılaştığınızda, kaynak dağıtım sırasını kavramanıza gerekir. Dağıtım işlemlerini sırasını görüntülemek için:

1. Dağıtım geçmişi kaynak grubunuz için seçin.

   ![Dağıtım geçmişi seçin](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Bir dağıtım geçmişinden ve seçin **olayları**.

   ![Dağıtım olaylarını seçin](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Her kaynak için olayların sırası inceleyin. Her işlemin durumunu dikkat edin. Örneğin, aşağıdaki resimde, paralel olarak dağıtılan üç depolama hesaplarını gösterir. Üç depolama hesapları aynı anda başlatılır dikkat edin.

   ![Paralel dağıtım](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   Sonraki resmi paralel olarak değil dağıtılan üç depolama hesaplarını gösterir. İlk Depolama hesabında ikinci depolama hesabı bağlıdır ve ikinci depolama hesabında üçüncü depolama hesabına bağlıdır. Bu nedenle, ilk depolama hesabı başlatıldı, kabul ve sonraki başlatılmadan önce tamamlandı.

   ![sıralı dağıtım](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

Daha karmaşık senaryolar için bile, dağıtım başlatıldığında ve her kaynak için tamamlanan bulmak için aynı tekniği kullanabilirsiniz. Sıra beklediğiniz farklı olup olmadığını görmek için dağıtım olayları arayın. Öyleyse, bu kaynak için bağımlılıkları yeniden.

Resource Manager şablonu doğrulama sırasında döngüsel bağımlılıklar tanımlar. Döngüsel bağımlılık özellikle bildiren bir hata iletisi yok değerini döndürür. Döngüsel bağımlılık çözmek için:

1. Şablonunuzda, döngüsel bağımlılık olarak tanımlanan kaynak bulunamıyor. 
2. Bu kaynak için inceleyin **dependsOn** özellik ve tüm kullanımlarını **başvuru** işlevi bağımlı hangi kaynaklara bakın. 
3. Bağlı oldukları kaynakları görmek için bu kaynakları inceleyin. Özgün kaynak bağlı kaynak fark kadar bağımlılıkları izleyin.
5. Döngüsel bağımlılık olarak ilgili kaynaklar için tüm kullanımını dikkatlice inceleyin **dependsOn** gerekli olmayan tüm bağımlılıkları belirlemek için özellik. Bu bağımlılıkları kaldırın. Bir bağımlılık gerekli olmadığından emin değilseniz, bu kaldırmayı deneyin. 
6. Şablonunu yeniden dağıtın.

Değerleri kaldırma **dependsOn** özelliği şablonu dağıttığınız zaman hataları neden olabilir. Bir hatayla karşılaşırsanız, bağımlılık şablon uygulamasına geri ekleyin. 

Bu yaklaşımı döngüsel bağımlılık çözmezse alt kaynakları (örneğin, uzantıları veya yapılandırma ayarları), dağıtım mantığının parçası taşınmasını göz önünde bulundurun. Döngüsel bağımlılık olarak dahil edilen kaynakları sonra dağıtmak için bu alt kaynaklarını yapılandırın. Örneğin, iki sanal makine dağıtıyorsanız, ancak diğer başvuran her birine özelliklerini ayarlamanız gerekir varsayalım. Bunları şu sırayla dağıtabilirsiniz:

1. VM1
2. vm2
3. Vm1 uzantısı vm1 ve vm2 bağlıdır. Uzantı vm2 alır vm1 değerlerini ayarlar.
4. Vm2 uzantısı vm1 ve vm2 bağlıdır. Uzantı vm1 alır vm2 değerlerini ayarlar.

Aynı yaklaşımı uygulama hizmeti uygulamalar için çalışır. Uygulama kaynağı bir alt kaynak yapılandırma değerlerini taşımayı düşünün. İki web uygulamaları aşağıdaki sırayla dağıtabilirsiniz:

1. webapp1
2. webapp2
3. config webapp1 için webapp1 ve webapp2 bağlıdır. Uygulama ayarları webapp2 değerlerle içerir.
4. config webapp2 için webapp1 ve webapp2 bağlıdır. Uygulama ayarları webapp1 değerlerle içerir.


## <a name="next-steps"></a>Sonraki adımlar
* Ortak dağıtım hataları için çözümler için bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md).
* Eylemler denetleme hakkında bilgi edinmek için [denetim işlemleri Resource Manager ile](resource-group-audit.md).
* Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için [görüntülemek dağıtım işlemlerini](resource-manager-deployment-operations.md).
