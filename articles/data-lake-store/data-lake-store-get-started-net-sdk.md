---
title: "Azure Data Lake Store’da uygulama geliştirmek için .NET SDK&quot;sını kullanma | Microsoft Docs"
description: "Bir Data Lake Store hesabı oluşturmak ve Data Lake Store&quot;da temel işlemleri gerçekleştirmek için Azure Data Lake Store .NET SDK’sını kullanma"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 74ea95349faa7ee3376050c22b4bb2375837b5c0
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>.NET SDK'yı kullanarak Azure Data Lake Store ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme gibi temel işlemler gerçekleştirmek üzere [Azure Data Lake Store .NET SDK’sını](https://msdn.microsoft.com/library/mt581387.aspx) kullanma hakkında bilgi edinin. Data Lake hakkında daha fazla bilgi için bkz. [Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2013, 2015 veya 2017**. Aşağıdaki yönergelerde Visual Studio 2015 Güncelleştirme 2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store hesabı**. Hesap oluşturmaya ilişkin yönergeler için bkz. [Azure Data Lake Store kullanmaya başlama](data-lake-store-get-started-portal.md)

* **Azure Active Directory Uygulaması oluşturma**. Data Lake Store uygulamasında Azure AD ile kimlik doğrulaması yapmak için Azure AD uygulamasını kullanın. Azure AD kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** gibi farklı yaklaşımlar bulunmaktadır. Kimlik doğrulaması hakkında yönergeler ve daha fazla bilgi için bkz. [Azure Active Directory kullanarak Data Lake Store kimlik doğrulaması yapma](data-lake-store-authenticate-using-active-directory.md).

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
5. Nuget paketlerini projenize ekleyin.

   1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
   2. **Nuget Paket Yöneticisi** sekmesinde, **Paket kaynağının** **nuget.org** olarak ayarlandığından ve **Ön sürümü dahil et** onay kutusunun işaretli olduğundan emin olun.
   3. Aşağıdaki NuGet paketlerini arayıp yükleyin:

      * `Microsoft.Azure.Management.DataLake.Store` - Bu öğreticide v2.1.3-preview kullanılır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - Bu öğreticide v2.2.12 kullanılır.

        ![Nuget kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Yeni bir Azure Data Lake hesabı oluşturma")
   4. **Nuget Paket Yöneticisi**'ni kapatın.
6. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. Aşağıda gösterildiği gibi değişkenleri tanımlayın ve Data Lake Store adına ve zaten var olan kaynak grubu adına yönelik değerleri sağlayın. Ayrıca, burada sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun. Aşağıdaki kod parçacığını ad alanı bildirimlerinden sonra ekleyin.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kimlik doğrulama, dosyayı karşıya yükleme vb. işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Son kullanıcı kimlik doğrulaması kullanıyorsanız (bu öğretici için önerilir)

Uygulamanızın kimliğini **etkileşimli olarak** doğrulamak üzere bunu var olan bir Azure AD yerle uygulamasıyla kullanın, bunun anlamı Azure kimlik bilgilerinizi girmeniz isteneceğidir.

Kullanım kolaylığı için, aşağıdaki kod parçacığında, herhangi bir Azure aboneliğiyle çalışacak yönlendirme URI’si ve istemci kimliği için varsayılan değerler kullanılır. Bu öğreticiyi daha hızlı tamamlamanıza yardımcı olması için bu yaklaşımı kullanmanız önerilir. Aşağıdaki kod parçacığında, yalnızca kiracı kimliğiniz için değeri sağlayın. [Active Directory Uygulaması oluşturma](data-lake-store-end-user-authenticate-using-active-directory.md) kısmında verilen yönergeleri kullanarak bunu alabilirsiniz.

    // User login via interactive popup
    // Use the client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with the user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

Yukarıdaki bu kod parçacığı hakkında bilmeniz gereken birkaç şey:

* Öğreticiyi daha hızlı tamamlamanıza yardımcı olmak üzere bu kod parçacığı tüm Azure abonelikleri için varsayılan olarak kullanılabilen bir Azure AD etki alanı ve istemci kimliği kullanır. Böylece **bu kod parçacığını uygulamanızda olduğu gibi kullanabilirsiniz**.
* Ancak, kendi Azure AD etki alanınızı ve uygulama istemci kimliğinizi kullanmak istemiyorsanız, bir Azure AD yerel uygulaması oluşturmanız ve ardından oluşturduğunuz uygulamaya ait Azure AD kiracı kimliği, istemci kimliği ve yeniden yönlendirme URI’sini kullanmanız gerekir. Yönergeler için, bkz: [Data Lake Store ile son kullanıcı kimlik doğrulaması için Active Directory Uygulama oluşturma](data-lake-store-end-user-authenticate-using-active-directory.md).

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Gizli anahtar ile hizmetten hizmete kimlik doğrulaması kullanıyorsanız
Gizli anahtar / uygulama anahtarı / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın **etkileşimli olmayan** kimlik doğrulaması için kullanılabilir. Bunu var olan Azure AD "Web App" uygulaması ile birlikte kullanın. Azure AD web uygulamasının nasıl oluşturulacağını ve aşağıdaki kod parçacığında gereken istemci kimliği ile istemci parolasının nasıl alınacağıyla ilgili yönergeler için, bkz: [Data Lake Store ile servis-servis kimlik doğrulaması için Active Directory Uygulaması oluşturma](data-lake-store-authenticate-using-active-directory.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Sertifika ile hizmetten hizmete kimlik doğrulaması kullanıyorsanız

Üçüncü bir seçenek olarak, bir Azure Active Directory uygulama sertifikası / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın **etkileşimli olmayan** kimlik doğrulaması için kullanılabilir. Bunu var olan, [Sertifikalı Azure AD Uygulaması](../azure-resource-manager/resource-group-authenticate-service-principal.md) ile birlikte kullanın.

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a>İstemci nesneleri oluşturma
Aşağıdaki kod parçacığı Data Lake Store hesabını ve hizmete verme isteği göndermek için kullanılan dosya sistemi istemci nesnelerini oluşturur.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Bir abonelik içindeki tüm Data Lake Store hesaplarını listeleme
Aşağıdaki kod parçacığı belirli bir Azure aboneliği içindeki tüm Data Lake Store hesaplarını listeler.

    // List all ADLS accounts within the subscription
    public static async Task<List<DataLakeStoreAccount>> ListAdlStoreAccounts()
    {
        var response = await _adlsClient.Account.ListAsync();
        var accounts = new List<DataLakeStoreAccount>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="create-a-directory"></a>Dizin oluşturma
Aşağıdaki kod parçacığında, bir Data Lake Store hesabında dizin oluşturmak için kullanabileceğiniz bir `CreateDirectory` yöntemi gösterilmiştir.

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme
Aşağıdaki kod parçacığında, bir Data Lake Store hesabına dosya yüklemek için kullanabileceğiniz bir `UploadFile` yöntemi gösterilmiştir.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

SDK bir yerel dosya ile Data Lake Store dosya yolu arasında yinelemeli karşıya yükleme ve indirmeyi destekler.    

## <a name="get-file-or-directory-info"></a>Dosya veya dizin bilgilerini alma
Aşağıdaki kod parçacığında, Data Lake Store'da kullanılabilir olan bir dosya veya dizin ile ilgili bilgileri almak için kullanabileceğiniz bir `GetItemInfo` yöntemi gösterilmiştir.

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Dosyayı veya dizinleri listeleme
Aşağıdaki kod parçacığında, bir Data Lake Store hesabındaki dosyaları ve dizinleri listelemek için kullanabileceğiniz bir `ListItem` yöntemi gösterilmiştir.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Dosyaları birleştirme
Aşağıdaki kod parçacığında, dosyaları birleştirmek için kullanabileceğiniz bir `ConcatenateFiles` yöntemi gösterilmiştir.

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme
Aşağıdaki kod parçacığında, bir Data Lake Store hesabında zaten depolanmış olan bir dosyanın sonuna veri eklemek için kullanabileceğiniz bir `AppendToFile` yöntemi gösterilmiştir.

    // Append to file
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a>Dosya indirme
Aşağıdaki kod parçacığında, bir Data Lake Store hesabındaki bir dosyayı indirmek için kullanabileceğiniz bir `DownloadFile` yöntemi gösterilmiştir.

    // Download file
       public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Data Lake Store .NET SDK Başvurusu](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [Data Lake Store REST Başvurusu](https://msdn.microsoft.com/library/mt693424.aspx)

