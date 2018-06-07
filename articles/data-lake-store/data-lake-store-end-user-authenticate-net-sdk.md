---
title: "Son kullanıcı kimlik doğrulaması: Azure Active Directory'yi kullanarak .NET SDK Data Lake Store ile | Microsoft Docs"
description: Azure Active Directory ile .NET SDK kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması elde öğrenin
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: cbb0f703f61b6c15b3a827dc75821286b7914c21
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623970"
---
# <a name="end-user-authentication-with-data-lake-store-using-net-sdk"></a>.NET SDK kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’yı kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Data Lake Store ile son kullanıcı kimlik doğrulaması yapmak için .NET SDK'sını kullanma hakkında bilgi edinin. .NET SDK kullanarak Data Lake Store ile hizmet kimlik doğrulaması için bkz: [hizmeti için kimlik doğrulaması .NET SDK kullanarak Data Lake Store ile](data-lake-store-service-to-service-authenticate-net-sdk.md).

## <a name="prerequisites"></a>Önkoşullar
* **Visual Studio 2013, 2015 veya 2017**. Aşağıdaki yönergelerde Visual Studio 2017 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. ' Ndaki adımları tamamlanmış gerekir [son kullanıcı kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-end-user-authenticate-using-active-directory.md).

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
7. Using Değiştir deyimleri aşağıdaki satırlar:

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
Bu kod parçacığında .NET istemci uygulamanızda ekleyin. Yer tutucu değerlerini (önkoşul olarak listelenen) bir Azure AD yerel uygulamadan alınan değerlerle değiştirin. Bu kod parçacığında, uygulamanız için kimlik doğrulaması sağlar **etkileşimli olarak** Data Lake Store ile başka bir deyişle, Azure kimlik bilgilerinizi girmeniz istenir.

Kullanım kolaylığı için aşağıdaki kod parçacığında, istemci kimliği ve yeniden yönlendirme hiçbir Azure aboneliği için geçerli bir URI için varsayılan değerleri kullanır. Aşağıdaki kod parçacığında, yalnızca Kiracı kimliğiniz için değer sağlamanız gerekir Verilen yönergeleri kullanarak Kiracı kimliği alabilir [Kiracı Kimliğinizi alma](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
    
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

Yukarıdaki kod parçacığında hakkında bilmeniz gerekenler birkaç:

* Önceki kod parçacığını yardımcı işlevlerini kullanan `GetTokenCache` ve `GetCreds_User_Popup`. Bu yardımcı işlevleri için kod kullanılabilir [github'da burada](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#gettokencache).
* Öğreticinin daha hızlı tamamlamanıza yardımcı olması için kod parçacığını tüm Azure abonelikleri için varsayılan olarak kullanılabilir bir yerel uygulama istemci Kimliğini kullanır. Böylece **bu kod parçacığını uygulamanızda olduğu gibi kullanabilirsiniz**.
* Ancak, kendi Azure AD etki alanınızı ve uygulama istemci kimliğinizi kullanmak istemiyorsanız, bir Azure AD yerel uygulaması oluşturmanız ve ardından oluşturduğunuz uygulamaya ait Azure AD kiracı kimliği, istemci kimliği ve yeniden yönlendirme URI’sini kullanmanız gerekir. Yönergeler için, bkz: [Data Lake Store ile son kullanıcı kimlik doğrulaması için Active Directory Uygulama oluşturma](data-lake-store-end-user-authenticate-using-active-directory.md).

  
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, .NET SDK kullanarak Azure Data Lake Store ile kimlik doğrulaması için son kullanıcı kimlik doğrulaması kullanmayı öğrendiniz. Şimdi, Azure Data Lake Store ile çalışmak için .NET SDK'sını kullanma hakkında konuşun aşağıdaki makaleleri da bakabilirsiniz.

* [.NET SDK kullanılarak gerçekleştirilen Data Lake Store'daki hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store .NET SDK kullanarak veri işlemleri](data-lake-store-data-operations-net-sdk.md)

