---
title: Özel bir NuGet kullanma Azure Dev boşluk akış | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: article
description: Özel bir NuGet erişmek ve bir Azure Dev alanı NuGet paketlerini kullanmak üzere akışı kullanın.
keywords: Docker, Kubernetes, Azure, AKS, Azure kapsayıcı hizmeti, kapsayıcıları
manager: ghogen
ms.openlocfilehash: 3badd15bcfd09c97b43744a20c5df05f4ff57e84
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34199118"
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Bir Azure Dev alanı akış özel bir NuGet kullanma

Bir NuGet akışı paket kaynaklarını bir proje eklemek için kolay bir yol sağlar. Azure Dev alanları Docker kapsayıcısı düzgün bir şekilde yüklenmesi için bağımlılıkları sırada bu akışın erişebilmeleri gerekir.

## <a name="set-up-a-nuget-feed"></a>Bir NuGet akış ayarlayın

Bir NuGet akış ayarlamak için:
1. Ekleme bir [paketini başvuru](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files) içinde `*.csproj` altında dosya `PackageReference` düğümü.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Oluşturma bir [NuGet.Config](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file) proje klasöründeki dosya.
     * Kullanım `packageSources` konumu akış, NuGet başvurmak için bölüm. Önemli: NuGet akışı genel olarak erişilebilir olmalıdır.
     * Kullanım `packageSourceCredentials` bölümünde kullanıcı adı ve parola kimlik bilgilerini yapılandırın. 

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
    - Başvuru `NuGet.Config` içinde `.gitignore` kimlik bilgilerini kaynak deponuza yanlışlıkla yürütme yok şekilde dosya.
    - Açık `azds.yaml` dosya projenizde ve bulun `build` bölümünde ve emin olmak için aşağıdaki kod parçacığını ekleyin `NuGet.Config` dosya işlemleri senkronize edilir Azure'a böylece kapsayıcı görüntü oluşturma işlemi sırasında kullanılır. (Varsayılan olarak, Azure Dev alanları eşleşen dosyaları eşitlenmediğini `.gitignore` ve `.dockerignore` kuralları.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımları tamamladıktan sonra bir sonraki defa çalıştırılması `azds up` (veya isabet `F5` VSCode ya da Visual Studio), Azure Dev alanları eşitleyecek `NuGet.Config` sonra tarafından kullanılan Azure dosyasına `dotnet restore` paketini yüklemek için kapsayıcı bağımlılıkları.

