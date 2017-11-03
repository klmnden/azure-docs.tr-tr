---
title: "Azure geçersiz şablonu hataları | Microsoft Docs"
description: "Geçersiz şablon hatalarını çözümlemeyi açıklar."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: e58c10cfb4cdd4ba49945e6c19845cbc957d6326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-invalid-template"></a>Geçersiz şablon için hataları çözümleyin

Bu makalede, geçersiz şablon hatalarını çözümlemeyi açıklar.

## <a name="symptom"></a>Belirti

Bir şablon dağıtılırken belirten bir hata alırsınız:

```
Code=InvalidTemplate
Message=<varies>
```

Hata iletisi hata türüne bağlıdır.

## <a name="cause"></a>Nedeni

Bu hata, birkaç farklı türlerinden hataları neden olabilir. Bunlar genellikle bir söz dizimi veya yapısal hatası şablondaki içerir.

## <a name="solution"></a>Çözüm

### <a name="solution-1---syntax-error"></a>Çözüm 1 - sözdizimi hatası

Şablon başarısız doğrulama bildiren bir hata iletisi alırsanız, şablonunuzun söz dizimi sorunu olabilir.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Bu hata, şablon ifadeleri karmaşık olabileceğinden yapmak kolaydır. Örneğin, aşağıdaki ad ataması bir depolama hesabı için bir dizi köşeli ayraçlar, üç işlevi, parantez üç kümesi, tek tırnak bir dizi ve bir özellik içerir:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Eşleştirme sözdizimi belirtmezseniz, şablon amacınız farklı bir değer oluşturur.

Bu tür hatalara aldığınızda, ifade sözdizimini dikkatle gözden geçirin. Bir JSON Düzenleyicisi gibi kullanmayı [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) veya [Visual Studio Code](resource-manager-vs-code.md), hangi uyar, söz dizimi hataları hakkında.

### <a name="solution-2---incorrect-segment-lengths"></a>Çözüm 2 - yanlış segment uzunluklarına

Kaynak adı doğru biçimde değil başka bir geçersiz şablon hata oluşur.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Kök düzeyinde kaynak daha az bir kesim adından kaynak türünde olması gerekir. Her segmentinde bir eğik çizgiyle Ayrıştırılan. Aşağıdaki örnekte, iki bölümü türü ve dolayısıyla bir kesim adına sahip bir **geçerli ad**.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "myHostingPlanName",
  ...
}
```

Ancak sonraki örnek **geçerli bir ad değil** türü aynı sayıda Segment içerdiğinden.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "appPlan/myHostingPlanName",
  ...
}
```

Alt kaynakları için kesimleri ile aynı sayıda türü ve ada sahip. Tam adı ve alt türü içerdiği için üst adını ve türünü Segment sayısı mantıklıdır. Bu nedenle, tam adı, tam türünü daha küçük bir kesim hala vardır.

```json
"resources": [
    {
        "type": "Microsoft.KeyVault/vaults",
        "name": "contosokeyvault",
        ...
        "resources": [
            {
                "type": "secrets",
                "name": "appPassword",
                ...
            }
        ]
    }
]
```

Segment sağ alma kaynak sağlayıcıları uygulanan Resource Manager türleriyle zor olabilir. Örneğin, bir web sitesi için bir kaynak kilidi uygulama dört kesimine sahip bir türü gerektirir. Bu nedenle, üç kesimleri adıdır:

```json
{
    "type": "Microsoft.Web/sites/providers/locks",
    "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
    ...
}
```

### <a name="solution-3---parameter-is-not-valid"></a>Çözüm 3 - parametresi geçerli değil

İzin verilen değerler bir parametre için kullanılacak şablonu belirtir ve bu değerlerden biri olmayan bir değer sağlayın, aşağıdakine benzer bir ileti alırsınız:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Double şablondaki izin verilen değerler denetleyin ve dağıtım sırasında bir sağlayın.

### <a name="solution-4---circular-dependency-detected"></a>Çözüm 4 - döngüsel bağımlılık algılandı

Birbirine bağımlı kaynakları dağıtım başlatılmasını engelleyen bir şekilde, bu hatayı alırsınız. Ayrıca bekleyen için diğer kaynaklar bekleyin iki veya daha fazla kaynak bağımlılıklarını bileşimini yapar. Örneğin, üzerinde resource3 resource1 bağlıdır, üzerinde resource1 switch2 bağlıdır ve üzerinde switch2 resource3 bağlıdır. Genellikle, gereksiz bağımlılıkları kaldırarak bu sorunu çözebilirsiniz.
