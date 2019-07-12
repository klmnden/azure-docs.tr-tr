---
title: Azure geliştirme alanlarında özel NuGet kullanma akışı
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: conceptual
description: Akış erişmek ve bir Azure geliştirme alanında NuGet paketlerini kullanmak için özel bir NuGet kullanın.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
manager: gwallace
ms.openlocfilehash: ca1fee76dfe280322a39fad56b9f85ebe1a92d3b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704032"
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Bir Azure geliştirme alanında özel bir NuGet akışı kullanma

Bir NuGet akışı paket kaynaklarını bir projeye dahil etmek için kullanışlı bir yol sağlar. Azure geliştirme alanları Docker kapsayıcısında düzgün yüklenmemiş bağımlılıklar için sırada bu akışa erişebilmesi gerekir.

## <a name="set-up-a-nuget-feed"></a>Akış nuget'i ayarlama

Bir NuGet akışı ayarlamak için:
1. Ekleme bir [paket başvurusu](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) içinde `*.csproj` altında dosya `PackageReference` düğümü.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Oluşturma bir [NuGet.Config](https://docs.microsoft.com/nuget/reference/nuget-config-file) proje klasöründeki dosya.
     * Kullanım `packageSources` Özet akışı konumu NuGet başvurmak için bölüm. Önemli: NuGet akışı genel olarak erişilebilir olması gerekir.
     * Kullanım `packageSourceCredentials` bölümü, kullanıcı adı ve parola kimlik bilgilerini yapılandırın. 

   ```xml
   <packageSources>
       <add key="Contoso" value="https://contoso.com/packages/" />
   </packageSources>

   <packageSourceCredentials>
       <Contoso>
           <add key="Username" value="user@contoso.com" />
           <add key="ClearTextPassword" value="33f!!lloppa" />
       </Contoso>
   </packageSourceCredentials>
   ```

3. Kaynak kodu denetimi kullanıyorsanız:
    - Başvuru `NuGet.Config` içinde `.gitignore` kimlik yanlışlıkla kaynak deponuza işleme olmayan şekilde dosya.
    - Açık `azds.yaml` dosya projenizde ve bulun `build` bölümünde ve emin olmak için aşağıdaki kod parçacığını eklemek `NuGet.Config` dosya işlemleri senkronize edilir Azure'a böylece kapsayıcı görüntüsü oluşturma işlemi sırasında kullanılır. (Varsayılan olarak, Azure geliştirme alanları eşleşen dosyaları eşitlemez `.gitignore` ve `.dockerignore` kuralları.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımları tamamladıktan sonra sonraki açışınızda çalıştırmanız `azds up` (veya isabet `F5` VSCode ya da Visual Studio), Azure geliştirme alanları eşitleneceğini `NuGet.Config` ardından tarafından kullanılan Azure dosya `dotnet restore` paketini yüklemek için kapsayıcı bağımlılıkları.

