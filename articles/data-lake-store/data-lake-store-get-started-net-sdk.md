<properties
   pageTitle="Uygulama geliştirmek için Data Lake Store .NET SDK'yı kullanma | Microsoft Azure"
   description="Uygulama geliştirmek için Azure Data Lake Store .NET SDK'yı kullanma"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/26/2016"
   ms.author="nitinme"/>


# .NET SDK'yı kullanarak Azure Data Lake Store ile çalışmaya başlama

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme gibi temel işlemler gerçekleştirmek üzere [Azure Data Lake Store .NET SDK’sını](https://msdn.microsoft.com/library/mt581387.aspx) kullanma hakkında bilgi edinin. Data Lake hakkında daha fazla bilgi için bkz. [Azure Data Lake Store](data-lake-store-overview.md).

## Önkoşullar

* **Visual Studio 2013 veya 2015**. Aşağıdaki yönergelerde Visual Studio 2015 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store hesabı**. Hesap oluşturmaya ilişkin yönergeler için bkz. [Azure Data Lake Store kullanmaya başlama](data-lake-store-get-started-portal.md)

* **Azure Active Directory Uygulaması oluşturma**. Data Lake Store uygulamasında Azure AD ile kimlik doğrulaması yapmak için Azure AD uygulamasını kullanın. Azure AD kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** gibi farklı yaklaşımlar bulunmaktadır. Kimlik doğrulaması hakkında yönergeler ve daha fazla bilgi için bkz. [Azure Active Directory kullanarak Data Lake Store kimlik doğrulaması yapma](data-lake-store-authenticate-using-active-directory.md).

## .NET uygulaması oluşturma

1. Visual Studio'yu açın ve bir konsol uygulaması oluşturun.

2. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın.

3. **Yeni Proje** bölümünden, aşağıdaki değerleri yazın veya seçin:

  	| Özellik | Değer                       |
  	|----------|-----------------------------|
  	| Kategori | Şablonlar/Visual C#/Windows |
  	| Şablon | Konsol Uygulaması         |
  	| Ad     | CreateADLApplication        |

4. Projeyi oluşturmak için **Tamam**'a tıklayın.

5. Nuget paketlerini projenize ekleyin.

    1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
    2. **Nuget Paket Yöneticisi** sekmesinde, **Paket kaynağının** **nuget.org** olarak ayarlandığından ve **Ön sürümü dahil et** onay kutusunun işaretli olduğundan emin olun.
    3. Aşağıdaki NuGet paketlerini arayıp yükleyin:

        * `Microsoft.Azure.Management.DataLake.Store` - Bu öğreticide v0.12.5-preview kullanılır.
        * `Microsoft.Azure.Management.DataLake.StoreUploader` - Bu öğreticide v0.10.6-preview kullanılır.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - Bu öğreticide v2.2.8-preview kullanılır.

        ![Nuget kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Create a new Azure Data Lake account")

    4. **Nuget Paket Yöneticisi**'ni kapatın.

6. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

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
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kimlik doğrulama, dosyayı karşıya yükleme vb. işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz.

## Kimlik Doğrulaması

### Son kullanıcı kimlik doğrulaması kullanıyorsanız

Bunu mevcut Azure AD "Yerel İstemci" Uygulamanız ile birlikte kullanın; aşağıda bir tanesi verilmiştir.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Yukarıdaki parçacıkta varsayılan olarak tüm Azure abonelikleri için kullanılabilir olan bir Azure AD etki alanı ve istemci kimliği kullanılmaktadır. Kendi Azure AD etki alanınızı ve uygulama istemci kimliğinizi kullanmak istiyorsanız bir Azure AD yerel uygulaması oluşturmanız gerekir. Yönergeler için bkz. [Active Directory Uygulaması oluşturma](../resource-group-create-service-principal-portal.md#create-an-active-directory-application).

>[AZURE.NOTE] Yukarıdaki bağlantılarda verilen yönergeler bir Azure AD web uygulaması içindir. Bununla birlikte, bunun yerine yerel istemci uygulaması oluşturmayı seçtiğinizde bile adımlar tam olarak aynıdır. 

### Gizli anahtar ile hizmetten hizmete kimlik doğrulaması kullanıyorsanız 

Gizli anahtar / uygulama anahtarı / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın etkileşimli olmayan kimlik doğrulaması için kullanılabilir. Bunu var olan [Azure AD "Web App" Uygulaması](../resource-group-create-service-principal-portal.md) ile birlikte kullanın.

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### Sertifika ile hizmetten hizmete kimlik doğrulaması kullanıyorsanız

Üçüncü bir seçenek olarak, uygulama sertifikası / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın etkileşimli olmayan kimlik doğrulaması için kullanılabilir. Bunu var olan [Azure AD "Web App" Uygulaması](../resource-group-create-service-principal-portal.md) ile birlikte kullanın.

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## İstemci nesneleri oluşturma

Aşağıdaki kod parçacığı Data Lake Store hesabını ve hizmete verme isteği göndermek için kullanılan dosya sistemi istemci nesnelerini oluşturur.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## Bir abonelik içindeki tüm Data Lake Store hesaplarını listeleme

Aşağıdaki kod parçacığı belirli bir Azure aboneliği içindeki tüm Data Lake Store hesaplarını listeler.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## Dizin oluşturma

Aşağıdaki kod parçacığında, bir Data Lake Store hesabında dizin oluşturmak için kullanabileceğiniz bir `CreateDirectory` yöntemi gösterilmiştir.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## Dosyayı karşıya yükleme

Aşağıdaki kod parçacığında, bir Data Lake Store hesabına dosya yüklemek için kullanabileceğiniz bir `UploadFile` yöntemi gösterilmiştir.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader` bir yerel dosya ile Data Lake Store dosya yolu arasında yinelemeli karşıya yükleme ve indirmeyi destekler.    

## Dosya veya dizin bilgilerini alma

Aşağıdaki kod parçacığında, Data Lake Store'da kullanılabilir olan bir dosya veya dizin ile ilgili bilgileri almak için kullanabileceğiniz bir `GetItemInfo` yöntemi gösterilmiştir. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## Dosyayı veya dizinleri listeleme

Aşağıdaki kod parçacığında, bir Data Lake Store hesabındaki dosyaları ve dizinleri listelemek için kullanabileceğiniz bir `ListItem` yöntemi gösterilmiştir.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## Dosyaları birleştirme

Aşağıdaki kod parçacığında, dosyaları birleştirmek için kullanabileceğiniz bir `ConcatenateFiles` yöntemi gösterilmiştir. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## Dosyanın sonuna ekleme

Aşağıdaki kod parçacığında, bir Data Lake Store hesabında zaten depolanmış olan bir dosyanın sonuna veri eklemek için kullanabileceğiniz bir `AppendToFile` yöntemi gösterilmiştir.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## Dosya indirme

Aşağıdaki kod parçacığında, bir Data Lake Store hesabındaki bir dosyayı indirmek için kullanabileceğiniz bir `DownloadFile` yöntemi gösterilmiştir.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## Sonraki adımlar

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Data Lake Store .NET SDK Başvurusu](https://msdn.microsoft.com/library/mt581387.aspx)
- [Data Lake Store REST Başvurusu](https://msdn.microsoft.com/library/mt693424.aspx)



<!--HONumber=Sep16_HO4-->


