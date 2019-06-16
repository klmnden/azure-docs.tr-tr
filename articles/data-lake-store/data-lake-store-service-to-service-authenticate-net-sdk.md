---
title: 'Hizmetten hizmete kimlik doğrulaması: .NET SDK kullanarak Azure Active Directory ile Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Azure Active Directory kullanarak elde öğrenin
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
ms.openlocfilehash: 96c496ef67e26a3079577bf52e9d019d963467b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65915848"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>Hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Azure Data Lake depolama Gen1 ile
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’yı kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
>
>

Bu makalede, Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması yapmak için .NET SDK'sı kullanma hakkında bilgi edinin. .NET SDK kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1 bkz [son kullanıcı kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-end-user-authenticate-net-sdk.md).

## <a name="prerequisites"></a>Önkoşullar
* **Visual Studio 2013 veya üzeri**. Visual Studio 2019 aşağıdaki yönergeleri kullanın.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Active Directory "Web" uygulaması oluşturma**. Adımları tamamlamış olmanız gerekir [hizmetten hizmete kimlik doğrulaması Azure Active Directory kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>.NET uygulaması oluşturma
1. Visual Studio'da **dosya** menüsünde **yeni**, ardından **proje**.
2. Seçin **konsol uygulaması (.NET Framework)** ve ardından **sonraki**.
3. İçinde **proje adı**, girin `CreateADLApplication`ve ardından **Oluştur**.

4. NuGet paketlerini projenize ekleyin.

   1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
   2. **NuGet Paket Yöneticisi** sekmesinde, **Paket kaynağının** **nuget.org** olarak ayarlandığından ve **Ön sürümü dahil et** onay kutusunun işaretli olduğundan emin olun.
   3. Aşağıdaki NuGet paketlerini arayıp yükleyin:

      * `Microsoft.Azure.Management.DataLake.Store` - Bu öğreticide v2.1.3-preview kullanılır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - Bu öğreticide v2.2.12 kullanılır.

        ![NuGet kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Yeni bir Azure Data Lake hesabı oluşturma")
   4. **NuGet Paket Yöneticisi**'ni kapatın.

5. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

```csharp
using System;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading;
using System.Collections.Generic;
using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

## <a name="service-to-service-authentication-with-client-secret"></a>Gizli anahtarla hizmetten hizmete kimlik doğrulaması
Bu kod parçacığı, .NET istemci uygulamanıza ekleyin. Yer tutucu değerlerini (bir önkoşul olarak listelenen) bir Azure AD web uygulamasından alınan değerlerle değiştirin. Bu kod parçacığı uygulamanızın kimlik doğrulaması sağlar **etkileşimsiz** Data Lake depolama Gen1 ile Azure AD web uygulaması için istemci parolası/anahtarı kullanarak.

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
    var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
    var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
}
```

Yukarıdaki kod parçacığında yardımcı işlevini kullanan `GetCreds_SPI_SecretKey`. Bu yardımcı işlev kodunu kullanılabilir [github'da burada](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey).

## <a name="service-to-service-authentication-with-certificate"></a>Sertifika ile hizmetten hizmete kimlik doğrulaması

Bu kod parçacığı, .NET istemci uygulamanıza ekleyin. Yer tutucu değerlerini (bir önkoşul olarak listelenen) bir Azure AD web uygulamasından alınan değerlerle değiştirin. Bu kod parçacığı uygulamanızın kimlik doğrulaması sağlar **etkileşimsiz** Data Lake depolama Gen1 ile bir Azure AD web uygulaması için sertifika kullanma. Azure AD uygulaması oluşturma hakkında yönergeler için bkz: [sertifikalar ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    var cert = new X509Certificate2(@"d:\cert.pfx", "<certpassword>");
    var armCreds = GetCreds_SPI_Cert(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, cert);
    var adlCreds = GetCreds_SPI_Cert(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, cert);
}
```

Yukarıdaki kod parçacığında yardımcı işlevini kullanan `GetCreds_SPI_Cert`. Bu yardımcı işlev kodunu kullanılabilir [github'da burada](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Data Lake depolama Gen1 ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz .NET SDK kullanarak. Data Lake depolama Gen1 ile çalışması için .NET SDK'sı kullanma hakkında konuşmak Aşağıdaki makaleler artık göz atabilirsiniz.

* [Data Lake depolama Gen1 hesap yönetim işlemlerini .NET SDK'sını kullanma](data-lake-store-get-started-net-sdk.md)
* [Veri işlemleri .NET SDK kullanarak Data Lake depolama Gen1](data-lake-store-data-operations-net-sdk.md)
