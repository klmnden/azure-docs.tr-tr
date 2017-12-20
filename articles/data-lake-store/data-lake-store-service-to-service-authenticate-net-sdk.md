---
title: "Hizmetten hizmete kimlik doğrulaması: Azure Active Directory'yi kullanarak .NET SDK Data Lake Store ile | Microsoft Docs"
description: "Hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Azure Active Directory kullanarak Data Lake Store ile elde öğrenin"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: c336cda6f3af4e2a4647371458b2db3e97917105
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-net-sdk"></a>.NET SDK kullanarak Data Lake Store ile hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’yı kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Data Lake Store ile hizmet kimlik doğrulaması yapmak için .NET SDK'sını kullanma hakkında bilgi edinin. .NET SDK kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması için bkz: [.NET SDK kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-net-sdk.md).


## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2013, 2015 veya 2017**. Aşağıdaki yönergelerde Visual Studio 2017 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Web" uygulama oluşturma**. ' Ndaki adımları tamamlanmış gerekir [hizmeti için kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-service-to-service-authenticate-using-active-directory.md).

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

6. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

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

## <a name="service-to-service-authentication-with-client-secret"></a>İstemci parolası ile hizmet kimlik doğrulaması
Bu kod parçacığında .NET istemci uygulamanızda ekleyin. Yer tutucu değerlerini (bir önkoşul olarak listelenen) Azure AD web uygulamasından alınan değerlerle değiştirin.  Bu kod parçacığında, uygulamanız için kimlik doğrulaması sağlar **etkileşimsiz** web uygulaması için Azure AD İstemci gizli/anahtar kullanarak Data Lake Store ile. 

    private static void Main(string[] args)
    {    
        // Service principal / appplication authentication with client secret / key
        // Use the client ID of an existing AAD "Web App" application.
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
        var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
        var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
    }

Önceki kod parçacığını bir yardımcı işlevini kullanır `GetCreds_SPI_SecretKey`. Bu yardımcı işlevi için kod kullanılabilir [github'da burada](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey).

## <a name="service-to-service-authentication-with-certificate"></a>Hizmeti için kimlik doğrulama sertifikası ile

Bu kod parçacığında .NET istemci uygulamanızda ekleyin. Yer tutucu değerlerini (bir önkoşul olarak listelenen) Azure AD web uygulamasından alınan değerlerle değiştirin. Bu kod parçacığında, uygulamanız için kimlik doğrulaması sağlar **etkileşimsiz** web uygulaması için Azure AD sertifikayı kullanarak Data Lake Store ile. Azure AD uygulaması oluşturma hakkında yönergeler için bkz: [sertifikalarla hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

    
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

Önceki kod parçacığını bir yardımcı işlevini kullanır `GetCreds_SPI_Cert`. Bu yardımcı işlevi için kod kullanılabilir [github'da burada](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, .NET SDK kullanarak Azure Data Lake Store ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz. Şimdi, Azure Data Lake Store ile çalışmak için .NET SDK'sını kullanma hakkında konuşun aşağıdaki makaleleri da bakabilirsiniz.

* [.NET SDK kullanılarak gerçekleştirilen Data Lake Store'daki hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store .NET SDK kullanarak veri işlemleri](data-lake-store-data-operations-net-sdk.md)


