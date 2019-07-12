---
title: Azure işlevleri için Azure özel sağlayıcılar Kurulumu
description: Bu öğreticide bir Azure işlevi oluşturma ve Azure özel sağlayıcıları ile çalışacak şekilde ayarlamak üzerinden geçer
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: d7e4de43659db88bfd9aad40cc3b9f1753189bba
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799997"
---
# <a name="setup-azure-functions-for-azure-custom-providers"></a>Azure işlevleri için Azure özel sağlayıcılar Kurulumu

Özel sağlayıcılar azure'da iş akışlarını özelleştirmenizi sağlar. Özel bir sağlayıcı Azure arasında bir sözleşmedir ve `endpoint`. Bu öğreticide, özel bir sağlayıcı çalışmak için bir Azure işlevini ayarlama işleminde geçer `endpoint`.

Bu öğreticide aşağıdaki adımlar ayrılır:

- Azure işlevi oluşturma
- Azure tablo bağlamaları yükleyin
- Güncelleştirme RESTful HTTP yöntemleri
- Azure Resource Manager NuGet paketleri Ekle

Bu öğreticide, aşağıdaki öğreticiler oluşturacaksınız:

- [Azure portalı üzerinden ilk Azure işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md)

## <a name="creating-the-azure-function"></a>Azure işlevi oluşturma

> [!NOTE]
> Bu öğreticide, biz bir Azure işlevi kullanarak basit bir hizmet uç noktası oluşturacağınız, ancak özel bir sağlayıcı erişilebilen tüm genel kullanabilirsiniz `endpoint`. Azure Logic Apps, Azure API Management ve Azure Web Apps bazı harika alternatiflerdir var.

Bu öğreticide başlamak için öğretici izlemelidir [Azure portalda ilk Azure işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md). Öğretici, Azure portalında değiştirilebilir bir .NET core Web kancası işlevi oluşturur.

## <a name="install-azure-table-bindings"></a>Azure tablo bağlamaları yükleyin

Bu bölümde, Azure tablo depolama bağlamaları yüklemek için hızlı adımlar geçer.

1. Gidin `Integrate` HttpTrigger için sekmesinde.
2. Tıklayarak `+ New Input`.
3. `Azure Table Storage` öğesini seçin.
4. Yükleme `Microsoft.Azure.WebJobs.Extensions.Storage` zaten yüklü değilse.
5. Güncelleştirme `Table parameter name` "tableStorage" için ve `Table name` "myCustomResources" için.
6. Güncelleştirilmiş giriş parametresi kaydedin.

![Özel sağlayıcı genel bakış](./media/create-custom-providers/azure-functions-table-bindings.png)

## <a name="update-restful-http-methods"></a>Güncelleştirme RESTful HTTP yöntemleri

Bu bölüm, özel sağlayıcı RESTful istek yöntemleri içerecek şekilde Azure işlevini ayarlamak için hızlı adımlar geçer.

1. Gidin `Integrate` HttpTrigger için sekmesinde.
2. Güncelleştirme `Selected HTTP methods` için: GET, POST, DELETE ve PUT.

![Özel sağlayıcı genel bakış](./media/create-custom-providers/azure-functions-http-methods.png)

## <a name="modifying-the-csproj"></a>Csproj değiştirme

> [!NOTE]
> Csproj dizinden eksikse, el ile eklenebilir veya bir kez görünür `Microsoft.Azure.WebJobs.Extensions.Storage` uzantısı işlev üzerinde yüklü.

Ardından, özel sağlayıcılardan gelen istekleri Ayrıştır daha kolay hale getirecek yararlı NuGet kitaplıklarını eklemek csproj dosyasının güncelleştireceğiz. Bölümündeki adımları [uzantıları ekleyin portalından](../azure-functions/install-update-binding-extensions-manual.md) ve csproj aşağıdaki paket başvuruları içerecek şekilde güncelleştirin:

```xml
<PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
<PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
<PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
```

Örnek csproj dosyası:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <WarningsAsErrors />
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
  </ItemGroup>
</Project>
```

## <a name="next-steps"></a>Sonraki adımlar

Biz bu makalede, Azure özel sağlayıcısı olarak iş için bir Azure işlevi kurulumu `endpoint`. Özel bir RESTful sağlayıcısı hakkında bilgi edinmek için sonraki makaleye gidin `endpoint`.

- [Öğretici: Bir RESTful özel sağlayıcı uç noktası yazma](./tutorial-custom-providers-function-authoring.md)
