---
title: Azure Service Fabric Mesh uygulama parolalarını yönetme | Microsoft Docs
description: Uygulama gizli dizilerini güvenli bir şekilde oluşturabilir ve bir Service Fabric Mesh uygulaması dağıtma şekilde yönetin.
services: service-fabric-mesh
keywords: Gizli dizileri
author: aljo-microsoft
ms.author: aljo
ms.date: 4/2/2019
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chackdan
ms.openlocfilehash: c2548ea3cf892ebe1a56cbb0909bfa5d5e805acf
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503303"
---
# <a name="manage-service-fabric-mesh-application-secrets"></a>Service Fabric Mesh uygulama parolalarını yönetme
Service Fabric Mesh gizli Azure kaynaklarını destekler. Service Fabric Mesh gizli dizi herhangi bir depolama bağlantı dizeleri, parolalar veya güvenli şekilde iletilmesini ve depolanan gereken diğer değerleri gibi hassas metin bilgi olabilir. Bu makalede, Service Fabric güvenli Store hizmeti dağıtma ve gizli dizileri korumak için nasıl kullanılacağını gösterir.

Mesh uygulaması gizli oluşur:
* A **gizli dizileri** kaynağı metin gizli dizileri depolayan bir kapsayıcıdır. İçindeki gizli dizileri **gizli dizileri** kaynak depolanır ve güvenli bir şekilde iletilen.
* Bir veya daha fazla **gizli anahtarları/değerleri** depolanan kaynakları **gizli dizileri** kaynak kapsayıcısı. Her **gizli anahtarları/değerleri** bir sürüm numarası tarafından kaynak ayırt edici. Bir sürümünü değiştiremezsiniz bir **gizli anahtarları/değerleri** kaynağı, yalnızca yeni bir sürüm ekleyin.

Gizli anahtarları yönetme, aşağıdaki adımlardan oluşur:
1. Kafes bildirmek **gizli dizileri** inlinedValue tür ve SecretsStoreRef contentType tanımlarını kullanarak bir Azure kaynak modeli YAML veya JSON dosyasında kaynak.
2. Kafes bildirmek **gizli anahtarları/değerleri** kaynaklar içinde depolanan bir Azure kaynak modeli YAML veya JSON dosyasında **gizli dizileri** kaynaktan (1. adım).
3. Mesh uygulaması kafes gizli değerleri başvurmak için değiştirin.
4. Dağıtın veya sıralı yükseltme Mesh uygulamasının gizli değerlerini kullanır.
5. Güvenli Store hizmeti yaşam döngüsü yönetimi için Azure'daki "az" CLI komutları.

## <a name="declare-a-mesh-secrets-resource"></a>Mesh gizli dizileri kaynak bildirme
Mesh gizli dizileri kaynak, bir Azure kaynak modeli JSON veya YAML dosyası inlinedValue tür tanımı kullanılarak bildirilir. Mesh gizli dizileri kaynak güvenli Store kaynaklanan hizmeti gizli dizileri destekler. 
>
Bir JSON dosyası Mesh gizli kaynaklara bildirmek nasıl bir örnek verilmiştir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "WestUS",
      "metadata": {
        "description": "Location of the resources (e.g. westus, eastus, westeurope)."
      }
    }
  },
  "sfbpHttpsCertificate": {
      "type": "string",
      "metadata": {
        "description": "Plain Text Secret Value that your container ingest"
      }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]", 
      "dependsOn": [],
      "properties": {
        "kind": "inlinedValue",
        "description": "SFBP Application Secret",
        "contentType": "text/plain",
      }
    }
  ]
}
```
Bir YAML dosyası Mesh gizli kaynaklara bildirmek nasıl bir örnek verilmiştir:
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="declare-mesh-secretsvalues-resources"></a>Mesh gizli anahtarları/değerleri kaynakları bildirme
Kafes gizli anahtarları/değerleri kaynaklar, önceki adımda tanımlanan Mesh gizli dizileri kaynaklar üzerinde bir bağımlılık sahiptir.

"Kaynaklar" bölümü arasındaki ilişki ile ilgili "değeri:" ve "adı:" alanları: ikinci bölümü "adı:" iki nokta ile ayrılmış iki nokta üst üste sahip olduğu için Kafes gizli değer ile eşleşmesi gerekiyor önce bir gizli anahtarı ve adı için kullanılan sürüm numarasını dizedir bir bağımlılık. Örneğin, öğe için ```name: mysecret:1.0```, 1.0 ve sürüm numarasını adıdır ```mysecret``` önceden tanımlanmış eşleşmelidir ```"value": "mysecret"```.

>
Bir JSON dosyası Mesh gizli anahtarları/değerleri kaynaklarında bildirmek nasıl bir örnek verilmiştir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "WestUS",
      "metadata": {
        "description": "Location of the resources (e.g. westus, eastus, westeurope)."
      }
    }
  },
  "sfbpHttpsCertificate": {
      "type": "string",
      "metadata": {
        "description": "Plain Text Secret Value that your container ingest"
      }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]", 
      "dependsOn": [],
      "properties": {
        "kind": "inlinedValue",
        "description": "SFBP Application Secret",
        "contentType": "text/plain",
      }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx/2019.02.28",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/secrets/sfbpHttpsCertificate.pfx"
      ],
      "properties": {
        "value": "[parameters('sfbpHttpsCertificate')]"
      }
    }
  ],
}
```
Bir YAML dosyası Mesh gizli anahtarları/değerleri kaynaklarında bildirmek nasıl bir örnek verilmiştir:
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          Secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
            - name: mysecret:1.0
            description: My Mesh Application Secret Value
            secret_type: value
            content_type: text/plain
            value: "P@ssw0rd#1234"
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="modify-mesh-application-to-reference-mesh-secret-values"></a>Mesh uygulaması gizli Mesh değerleri başvurmak için değiştirme
Service Fabric Örgü uygulamalar Store hizmet parolasını güvenli değerleri kullanmak için aşağıdaki iki dizenin dikkat etmeniz gerekir:
1. Microsoft.ServiceFabricMesh/Secrets.name dosya adını içerir ve düz metin parolaları değeri içerir.
2. Windows veya Linux ortam değişkeni "Fabric_SettingPath" nerede Store hizmet gizli dizileri güvenli değerleri içeren dosyalar erişilebilir olacaktır için dizin yolunu içerir. Bunun için "C:\Settings" olan Windows barındırılan ve "/ var/ayarları" kafes Linux barındırılan uygulamalar için sırasıyla.

## <a name="deploy-or-use-a-rolling-upgrade-for-mesh-application-to-consume-secret-values"></a>Dağıtmanıza veya gizli değerler kullanılacağı Mesh uygulaması için sıralı yükseltme
Gizli dizileri ve/veya tutulan gizli anahtarları/değerleri oluşturmak için kaynak modeli dağıtımlarını bildirilen sınırlıdır. Bu kaynakları oluşturmak için tek bir kaynak modeli JSON veya YAML dosyası kullanılarak geçirerek yoludur **az kafes dağıtım** komutuyla şu şekilde:

```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```

## <a name="azure-cli-commands-for-secure-store-service-lifecycle-management"></a>Güvenli Store hizmeti yaşam döngüsü yönetimi için Azure CLI komutları

### <a name="create-a-new-secrets-resource"></a>Yeni gizli dizileri kaynak oluştur
```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```
Ya da geçirmeniz **şablon dosyası** veya **URI şablonu** (ancak her ikisini birden değil).

Örneğin:
- az kafes dağıtım--c:\MyMeshTemplates\SecretTemplate1.txt oluştur
- az kafes dağıtım oluşturma--https:\//www.fabrikam.com/MyMeshTemplates/SecretTemplate1.txt

### <a name="show-a-secret"></a>Gizli dizi Göster
Gizli dizi (ancak değer değil) açıklamasını döndürür.
```azurecli-interactive
az mesh secret show --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="delete-a-secret"></a>Gizli anahtarı silme

- Mesh uygulama tarafından başvuruluyor ancak bir gizli dizi silinemiyor.
- Gizli dizileri kaynağın silinmesi, tüm gizli dizileri/kaynakları sürümlerinin siler.
  ```azurecli-interactive
  az mesh secret delete --Resource-group <myResourceGroup> --secret-name <mySecret>
  ```

### <a name="list-secrets-in-subscription"></a>Abonelik gizli anahtarları listeleme
```azurecli-interactive
az mesh secret list
```
### <a name="list-secrets-in-resource-group"></a>Kaynak grubundaki gizli anahtarları listeleme
```azurecli-interactive
az mesh secret list -g <myResourceGroup>
```
### <a name="list-all-versions-of-a-secret"></a>Gizli dizi tüm sürümlerini listeleme
```azurecli-interactive
az mesh secretvalue list --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="show-secret-version-value"></a>Gizli dizi sürümü değeri göster
```azurecli-interactive
az mesh secretvalue show --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

### <a name="delete-secret-version-value"></a>Gizli dizi sürümü değerini sil
```azurecli-interactive
az mesh secretvalue delete --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh hakkında daha fazla bilgi için genel bakış okuyun:
- [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md)
