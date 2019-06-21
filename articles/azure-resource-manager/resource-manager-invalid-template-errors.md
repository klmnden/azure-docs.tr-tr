---
title: Azure geçersiz şablon hataları | Microsoft Docs
description: Geçersiz şablon hataları gidermek nasıl açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: 0417a975ffbbe0acd94ceac6c0f8e1de27a27aa6
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206254"
---
# <a name="resolve-errors-for-invalid-template"></a>Geçersiz şablon hataları çözün

Bu makalede, geçersiz şablon hataları gidermek açıklar.

## <a name="symptom"></a>Belirti

Bir şablon dağıtılırken belirten bir hata alırsınız:

```
Code=InvalidTemplate
Message=<varies>
```

Hata iletisi, hatanın türüne bağlıdır.

## <a name="cause"></a>Nedeni

Bu hata birçok farklı tür hataları neden olabilir. Bunlar genellikle bir şablon söz dizimi veya yapısal hata içerir.

<a id="syntax-error" />

## <a name="solution-1---syntax-error"></a>Çözüm 1 - söz dizimi hatası

Şablon doğrulama belirten bir hata iletisi alırsanız, şablonunuzun söz dizimi sorunu olabilir.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Bu hata, şablon ifadeleri karmaşık olabileceğinden yapmak kolaydır. Örneğin, bir depolama hesabı için aşağıdaki ad atama bir kümesini parantez, üç işlev, üç ayarlar parantez, tek tırnak bir dizi ve bir özellik vardır:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Eşleme sözdizimi sağlamazsanız, şablon amacınız farklı bir değer üretir.

Bu tür bir hata iletisini aldığınızda ifadesi söz dizimi dikkatle gözden geçirin. Gibi bir JSON düzenleyicisini kullanmayı [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) veya [Visual Studio Code](resource-manager-vs-code.md), hangi uyar, sözdizimi hataları hakkında.

<a id="incorrect-segment-lengths" />

## <a name="solution-2---incorrect-segment-lengths"></a>Çözüm 2 - yanlış segment uzunlukları

Kaynak adı doğru biçimde değil. başka bir geçersiz şablon hata oluşur.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Kök düzeyi kaynak adından kaynak türü, daha az bir segmente sahip olmaldır. Her segmentinde bir eğik çizgi Ayrıştırılan. Aşağıdaki örnekte, iki Segment türünde ve bunun için bir segment adına sahip bir **geçerli ad**.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "myHostingPlanName",
  ...
}
```

Ancak bir sonraki örnekte **geçerli bir ad** türü aynı sayıda Segment içerdiğinden.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "appPlan/myHostingPlanName",
  ...
}
```

Alt kaynaklar için tür ve ad kesimleri ile aynı sayıda sahip. Bu Segment sayısı tam adını ve alt türü içerdiği için üst adını ve türünü mantıklıdır. Bu nedenle, tam adı, yine de tam türünden daha az bir segmente vardır.

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

Segmentleri doğru başlama kaynak sağlayıcıları uygulanan Resource Manager türleriyle zor olabilir. Örneğin, bir web sitesine bir kaynak kilidi uygulanması, dört segmentleri türüyle gerekir. Bu nedenle, üç Segment adı şöyledir:

```json
{
    "type": "Microsoft.Web/sites/providers/locks",
    "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
    ...
}
```

<a id="parameter-not-valid" />

## <a name="solution-3---parameter-is-not-valid"></a>3 - çözüm parametresi geçerli değil

İzin verilen değerlerden biri değil bir parametre değeri sağlarsanız, aşağıdakine benzer bir ileti alırsınız:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Çift şablondaki izin verilen değerleri kontrol edin ve dağıtım sırasında bir sağlayın. İzin verilen parametre değerleri hakkında daha fazla bilgi için bkz. [parametreleri bölümünde Azure Resource Manager şablonları](resource-group-authoring-templates.md#parameters).

<a id="too-many-resource-groups" />

## <a name="solution-4---too-many-target-resource-groups"></a>Çözüm 4 - çok fazla hedef kaynak grubu

Tek bir dağıtımda beşten fazla hedef kaynak grubu belirtirseniz, bu hatayı alırsınız. Kaynak grubu, dağıtımınızdaki birleştirme veya şablon dağıtımları ayrı olarak bazıları dağıtma göz önünde bulundurun. Daha fazla bilgi için [birden fazla abonelik veya kaynak grubu dağıtma Azure kaynaklarına](resource-manager-cross-resource-group-deployment.md).

<a id="circular-dependency" />

## <a name="solution-5---circular-dependency-detected"></a>Çözüm 5 - döngüsel bağımlılık algılandı

Birbirine bağımlı kaynakları dağıtım başlatılmasını engelleyen bir yolla olduğunda bu hatayı alırsınız. Bağımlılıkları birleşimi, aynı zamanda bekleyen diğer kaynakları beklemek iki veya daha fazla kaynak sağlar. Örneğin, üzerinde resource3 resource1 bağlıdır, resource2 resource1 üzerinde bağlıdır ve resource3 resource2 üzerinde bağlıdır. Genellikle, gereksiz bağımlılıkları ortadan kaldırarak bu sorunu çözebilirsiniz.

Döngüsel bağımlılık çözmek için:

1. Şablonunuzda, döngüsel bağımlılığa yol tanımlanan kaynağa bulun. 
2. Bu kaynak için inceleyin **dependsOn** özellik ve tüm kullanımları **başvuru** ölçüye bağımlı olan kaynakları görmek için işlev. 
3. Bağlı oldukları kaynakları görmek için bu kaynakları inceleyin. Bağımlılıklar, özgün kaynakta olduğu kaynak fark kadar izleyin.
5. Döngüsel bağımlılığa yol dahil olan kaynaklar için tüm kullanımları dikkatlice inceleyin **dependsOn** gerekmeyen herhangi bir bağımlılığın tanımlamak için özellik. Bu bağımlılıkları kaldırın. Bir bağımlılık gerekli olmadığından emin değilseniz, da kaldırmayı deneyin. 
6. Şablonu yeniden dağıtın.

Değerleri kaldırma **dependsOn** özelliği, şablonu dağıttığınızda hatalara neden olabilir. Bir hata alırsanız, bağımlılık şablonu yeniden içine ekleyin. 

Bu yaklaşım döngüsel bağımlılık çözmezse, alt kaynaklar (örneğin, uzantıları veya yapılandırma ayarları) dağıtım mantığınızın bir parçası taşınmasını göz önünde bulundurun. Döngüsel bağımlılığa yol kaynaklardan sonra dağıtmak için bu alt kaynaklarını yapılandırın. Örneğin, iki sanal makineler dağıtırken, ancak diğer başvuran her birinde özelliklerini ayarlamalısınız varsayalım. Bunları şu sırayla dağıtabilirsiniz:

1. vm1
2. vm2
3. Vm1 uzantısı vm1 ve vm2 bağlıdır. Uzantı vm1, vm2 ' alır değerlerini ayarlar.
4. Vm1 ve vm2 vm2 uzantısı bağlıdır. Uzantı vm1 ' alır vm2 değerlerini ayarlar.

Aynı yaklaşımı, App Service uygulamaları için çalışır. Uygulama kaynak içinde bir alt kaynak yapılandırma değerlerini taşımayı düşünün. Aşağıdaki sırayla iki web uygulaması dağıtabilirsiniz:

1. webapp1
2. webapp2
3. yapılandırma için webapp1 webapp1 ve webapp2 bağlıdır. Uygulama ayarları webapp2 değerleri ile var.
4. yapılandırma için webapp2 webapp1 ve webapp2 bağlıdır. Uygulama ayarları webapp1 değerleri ile var.
