---
title: "Son kullanıcı kimlik doğrulaması: Azure Active Directory'yi kullanarak .NET SDK'sı ile Azure Data Lake depolama Gen1 | Microsoft Docs"
description: .NET SDK'sı ile Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1 elde öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 78a290d8136f8804e853d36a9bc95571625ed89c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58876777"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>.NET SDK kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’yı kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Data Lake depolama Gen1 son kullanıcı kimlik doğrulaması yapmak için .NET SDK'sı kullanma hakkında bilgi edinin. .NET SDK kullanarak hizmetten hizmete kimlik doğrulaması ile Data Lake depolama Gen1 bkz [hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-net-sdk.md).

## <a name="prerequisites"></a>Önkoşullar
* **Visual Studio 2013, 2015 veya 2017**. Aşağıdaki yönergelerde Visual Studio 2017 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. Adımları tamamlamış olmanız gerekir [Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>.NET uygulaması oluşturma
1. Visual Studio'yu açın ve bir konsol uygulaması oluşturun.
2. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın.
3. **Yeni Proje** bölümünden, aşağıdaki değerleri yazın veya seçin:

   | Özellik | Değer |
   | --- | --- |
   | Kategori |Şablonlar/Visual C#/Windows |
   | Şablon |Konsol Uygulaması |
   | Ad |CreateADLApplication |

4. Projeyi oluşturmak için **Tamam**'a tıklayın.

5. NuGet paketlerini projenize ekleyin.

   1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
   2. **NuGet Paket Yöneticisi** sekmesinde, **Paket kaynağının** **nuget.org** olarak ayarlandığından ve **Ön sürümü dahil et** onay kutusunun işaretli olduğundan emin olun.
   3. Aşağıdaki NuGet paketlerini arayıp yükleyin:

      * `Microsoft.Azure.Management.DataLake.Store` - Bu öğreticide v2.1.3-preview kullanılır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - Bu öğreticide v2.2.12 kullanılır.

        ![NuGet kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Yeni bir Azure Data Lake hesabı oluşturma")
   4. **NuGet Paket Yöneticisi**'ni kapatın.

6. Açık **Program.cs**
7. Kullanarak yerine aşağıdaki satırlarla ifadeleri:

    ```csharp
    using System;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Collections.Generic;
            
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```     

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Bu kod parçacığı, .NET istemci uygulamanıza ekleyin. Yer tutucu değerlerini (önkoşul olarak listelenen) bir Azure AD yerel uygulaması alınan değerlerle değiştirin. Bu kod parçacığı uygulamanızın kimlik doğrulaması sağlar **etkileşimli olarak** ile Data Lake depolama Gen1, yani Azure kimlik bilgilerinizi girmeniz istenir.

Kullanım kolaylığı için aşağıdaki kod parçacığı yeniden yönlendirme URI'si herhangi bir Azure aboneliği için geçerli olan ve istemci kimliği için varsayılan değerleri kullanır. Aşağıdaki kod parçacığında, yalnızca Kiracı kimliğiniz için değer sağlamanız gerekir Verilen yönergeleri kullanarak Kiracı kimliği alabilir [Kiracı Kimliğinizi alma](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).
    
- Main() işlevi aşağıdaki kodla değiştirin:

    ```csharp
    private static void Main(string[] args)
    {
        //User login via interactive popup
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        string MY_DOCUMENTS = System.Environment.GetFolderPath(System.Environment.SpecialFolder.MyDocuments);
        string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");
        var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
        var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
        var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
    }
    ```

Bir önceki kod parçacığı hakkında bilmeniz gereken birkaç şey:

* Yukarıdaki kod parçacığında, bir yardımcı işlevleri kullanır `GetTokenCache` ve `GetCreds_User_Popup`. Bu yardımcı işlevleri için kod kullanılabilir [github'da burada](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#gettokencache).
* Öğreticiyi daha hızlı tamamlamanıza yardımcı olması için kod parçacığı tüm Azure abonelikleri için varsayılan olarak kullanılabilir bir yerel uygulama istemci Kimliğini kullanır. Böylece **bu kod parçacığını uygulamanızda olduğu gibi kullanabilirsiniz**.
* Ancak, kendi Azure AD etki alanınızı ve uygulama istemci kimliğinizi kullanmak istemiyorsanız, bir Azure AD yerel uygulaması oluşturmanız ve ardından oluşturduğunuz uygulamaya ait Azure AD kiracı kimliği, istemci kimliği ve yeniden yönlendirme URI’sini kullanmanız gerekir. Bkz: [Data Lake depolama Gen1 ile son kullanıcı kimlik doğrulaması için Active Directory uygulaması oluşturma](data-lake-store-end-user-authenticate-using-active-directory.md) yönergeler için.

  
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Data Lake depolama Gen1 ile kimlik doğrulaması için son kullanıcı kimlik doğrulaması kullanmayı öğrendiniz .NET SDK kullanarak. Şimdi, Azure Data Lake depolama Gen1 ile çalışması için .NET SDK'sı kullanma hakkında konuşmak aşağıdaki makaleleri da bakabilirsiniz.

* [Data Lake depolama Gen1 hesap yönetim işlemlerini .NET SDK'sını kullanma](data-lake-store-get-started-net-sdk.md)
* [Veri işlemleri .NET SDK kullanarak Data Lake depolama Gen1](data-lake-store-data-operations-net-sdk.md)

