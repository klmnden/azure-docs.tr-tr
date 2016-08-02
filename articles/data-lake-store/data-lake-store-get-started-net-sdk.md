<properties
   pageTitle="Uygulama geliştirmek için Data Lake Store .NET SDK'yı kullanma | Azure"
   description="Uygulama geliştirmek için Azure Data Lake Store .NET SDK'yı kullanma"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="04/27/2016"
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

Azure Data Lake hesabı oluşturmak ve klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure Data Lake Store .NET SDK'nın nasıl kullanılacağını öğrenin. Data Lake hakkında daha fazla bilgi için bkz. [Azure Data Lake Store](data-lake-store-overview.md).

## Önkoşullar

* Visual Studio 2013 veya 2015. Aşağıdaki yönergelerde Visual Studio 2015 kullanılmıştır.
* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* Data Lake Store genel önizlemesi için **Azure aboneliğinizi etkinleştirme**. Bkz. [yönergeler](data-lake-store-get-started-portal.md#signup).
* **Azure Active Directory Uygulaması oluşturma**. Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmenin iki yolu vardır: **etkileşimli** ve **etkileşimli olmayan**. Kimlik doğrulamasını nasıl gerçekleştirmek istediğinize bağlı olarak farklı önkoşullar mevcuttur.
    * **Etkileşimli kimlik doğrulaması için** (bu makalede kullanılan) - Azure Active Directory'de bir **Yerel İstemci uygulaması** oluşturmanız gerekir. Uygulamayı oluşturduktan sonra uygulamayla ilgili aşağıdaki değerleri alın.
        - Uygulama için **istemci kimliği** ve **yeniden yönlendirme URI'si** bilgilerini alın
        - Yetki verilmiş izinleri ayarlayın

    * **Etkileşimli olmayan kimlik doğrulaması için** - Azure Active Directory'de bir **Web uygulaması** oluşturmanız gerekir. Uygulamayı oluşturduktan sonra uygulamayla ilgili aşağıdaki değerleri alın.
        - Uygulama için **istemci kimliği**, **gizli anahtar** ve **yeniden yönlendirme URI'si** bilgilerini alın
        - Yetki verilmiş izinleri ayarlayın
        - Azure Active Directory uygulamasını bir role atayın. Rol, Azure Active Directory uygulamasına izin vermek istediğiniz kapsam düzeyinde olabilir. Örneğin, uygulamayı abonelik düzeyinde veya kaynak grubu düzeyinde atayabilirsiniz. 

    Bu değerleri almaya, izinleri ayarlamaya ve rolleri atamaya yönelik yönergeler için bkz. [Portalı kullanarak Active Directory uygulaması ve hizmet sorumlusu oluşturma](../resource-group-create-service-principal-portal.md).

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
    3. Aşağıdaki Data Lake Store paketlerini arayıp yükleyin:

        * `Microsoft.Azure.Management.DataLake.Store`
        * `Microsoft.Azure.Management.DataLake.StoreUploader`

        ![Nuget kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Create a new Azure Data Lake account")

    4. Ayrıca, Azure Active Directory kimlik doğrulaması için `Microsoft.IdentityModel.Clients.ActiveDirectory` paketini yükleyin. Bu paketin kararlı bir sürümünü yüklemek için **Ön sürümü dahil et** onay kutusunu *temizlediğinizden* emin olun.

        ![Nuget kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/adl.install.azure.auth.png "Create a new Azure Data Lake account")


    5. **Nuget Paket Yöneticisi**'ni kapatın.

7. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

        using System;
        using System.IO;
        using System.Security;
        using System.Text;
        using System.Collections.Generic;
        using System.Linq;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;

8. Aşağıda gösterildiği gibi değişkenleri tanımlayın ve Data Lake Store adına ve kaynak grubu adına yönelik değerleri sağlayın. Sağladığınız Data Lake Store adı uygulama tarafından oluşturulur. Burada sağladığınız kaynak grubu zaten var olmalıdır. Ayrıca, burada sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun. Aşağıdaki kod parçacığını ad alanı bildirimlerinden sonra ekleyin.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value. This resource group should already exist.
                    _location = "East US 2";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kullanıcıların kimliğini doğrulamak, Data Lake Store hesabı oluşturmak, karşıya dosya yüklemek gibi işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz. Data Lake Store ile çalışma konusunda eksiksiz bir örnek arıyorsanız bu makalenin en altındaki [Ek](#appendix-sample-code)'e göz atın.

## Kullanıcının kimliğini doğrulama

Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmenin iki yolu mevcuttur:

* **Etkileşimli**; bu yol kullanıldığında, kullanıcı uygulamayı kullanarak oturum açar. Bu, aşağıdaki kod parçacığında bulunan `AuthenticateUser` yöntemine uygulanmıştır.

* **Etkileşimli olmayan**; bu yol kullanıldığında, uygulama kendi kimlik bilgilerini sağlar. Bu, aşağıdaki kod parçacığında bulunan `AuthenticateAppliaction` yöntemine uygulanmıştır.

### Etkileşimli kimlik doğrulaması

Aşağıdaki kod parçacığında, etkileşimli oturum açma deneyimi için kullanabileceğiniz bir `AuthenticateUser` yöntemi gösterilmiştir.

    // Authenticate the user with AAD through an interactive popup.
    // You need to have an application registered with AAD in order to authenticate.
    //   For more information and instructions on how to register your application with AAD, see:
    //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
    public static TokenCredentials AuthenticateUser(string tenantId, string resource, string appClientId, Uri appRedirectUri, string userId = "")
    {
        var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
    
        var tokenAuthResult = authContext.AcquireToken(resource, appClientId, appRedirectUri,
            PromptBehavior.Auto, UserIdentifier.AnyUser);
    
        return new TokenCredentials(tokenAuthResult.AccessToken);
    }

### Etkileşimli olmayan kimlik doğrulaması

Aşağıdaki kod parçacığında, etkileşimli olmayan oturum açma deneyimi için kullanabileceğiniz bir `AuthenticateApplication` yöntemi gösterilmiştir.

    // Authenticate the application with AAD through the application's secret key.
    // You need to have an application registered with AAD in order to authenticate.
    //   For more information and instructions on how to register your application with AAD, see:
    //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
    public static TokenCredentials AuthenticateApplication(string tenantId, string resource, string appClientId, Uri appRedirectUri, SecureString clientSecret)
    {
        var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
        var credential = new ClientCredential(appClientId, clientSecret);
    
        var tokenAuthResult = authContext.AcquireToken(resource, credential);
    
        return new TokenCredentials(tokenAuthResult.AccessToken);
    }
    
## Data Lake Store hesabı oluşturma

Aşağıdaki kod parçacığında, bir Data Lake Store hesabı oluşturmak için kullanabileceğiniz bir `CreateAccount` yöntemi gösterilmiştir.

    // Create Data Lake Store account
    public static void CreateAccount()
    {
        var adlsParameters = new DataLakeStoreAccount(location: _location);
        _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
    } 

## Bir abonelik içindeki tüm Data Lake Store hesaplarını listeleme

Aşağıdaki kod parçacığında, belirli bir Azure aboneliği içindeki tüm Data Lake Store hesaplarını listelemek için kullanabileceğiniz `ListAdlStoreAccounts` yöntemi gösterilmiştir.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
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

## Data Lake Store'a dosya yükleme

Aşağıdaki kod parçacığında, bir Data Lake Store hesabına dosya yüklemek için kullanabileceğiniz bir `UploadFile` yöntemi gösterilmiştir.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

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

## Data Lake Store hesabını silme

Aşağıdaki kod parçacığında, bir Data Lake Store hesabını silmek için kullanabileceğiniz bir `DeleteAccount` yöntemi gösterilmiştir.

    // Delete account
    public static void DeleteAccount()
    {
        _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
    }

## Ek: Örnek kod

Aşağıdaki kod parçacığı, Data Lake Store üzerinde uçtan uca işlemi görmek üzere uygulamanıza kopyalayıp yapıştırabileceğiniz kapsamlı bir kod örneğidir. Kod parçacığını çalıştırmadan önce, Data Lake Store adı, kaynak grubu adı gibi gerekli değerleri sağladığınızdan emin olun. Ayrıca, **\<APPLICATION-CLIENT-ID>** , **\<APPLICATION-REPLY-URI>** ve **\<SUBSCRIPTION-ID>** gibi, Azure Active Directory kimlik doğrulaması için gerekli olan değerleri de sağlamanız gerekir.

Aşağıdaki kod parçacığı, etkileşimli ve etkileşimli olmayan seçeneklerinden oluşan her iki yaklaşım için yöntem sağlasa da etkileşimli olmayan kod kapsam dışı bırakılmıştır. Etkileşimli yöntem, AAD uygulama istemci kimliğini ve yeniden yönlendirme URI'sini sağlamanızı gerektirir. Önkoşul içindeki bağlantıda, bunların nasıl edinileceğine yönelik yönergeler sağlanmıştır.

>[AZURE.NOTE] Kod parçacığını değiştirmek ve bunun yerine etkileşimli olmayan (`AuthenticateApplication`) yöntem kullanmak istiyorsanız yönteme giriş olarak istemci kimliği ve istemci yanıt URI'sinin yanı sıra istemci kimlik doğrulaması anahtarını da sağlamanız gerekir. [Portalı kullanarak Active Directory uygulaması ve hizmet sorumlusu oluşturma](../resource-group-create-service-principal-portal.md) makalesinde de istemci kimlik doğrulaması anahtarının nasıl oluşturulacağı ve alınacağı konusunda bilgi sağlanmaktadır.
    
Son olarak, burada sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun. Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.



    using System;
    using System.IO;
    using System.Security;
    using System.Text;
    using System.Collections.Generic;
    using System.Linq;

    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;
    using Microsoft.Azure.Management.DataLake.StoreUploader;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;

    namespace SdkSample
    {
        class Program
        {
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

            private static string _adlsAccountName;
            private static string _resourceGroupName;
            private static string _location;

            private static void Main(string[] args)
            {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
                _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value. This resource group should already exist.
                _location = "East US 2";

                string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                string remoteFolderPath = "/data_lake_path/";
                string remoteFilePath = remoteFolderPath + "file.txt";

                // Authenticate the user
                var tokenCreds = AuthenticateUser("common", "https://management.core.windows.net/",
                    "<APPLICATION-CLIENT-ID>", new Uri("<APPLICATION-REPLY-URI>")); // TODO: Replace bracketed values.

                SetupClients(tokenCreds, "<SUBSCRIPTION-ID>"); // TODO: Replace bracketed value.

                // Run sample scenarios
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Creating a directory.");

                // Create a directory in the Data Lake Store
                CreateDirectory(remoteFolderPath);
                WaitForNewline("Directory created.", "Showing directory info.");

                // Get info about the directory in the Data Lake Store
                var itemInfo = GetItemInfo(remoteFolderPath);
                Console.WriteLine("Type: " + itemInfo.Type);
                Console.WriteLine("Last modified (UTC): " +
                                  new DateTime(1970, 1, 1, 0, 0, 0, 0, DateTimeKind.Utc).AddMilliseconds(
                                      itemInfo.ModificationTime.Value));
                WaitForNewline("Directory info shown.", "Uploading a file.");

                // Upload a file to the Data Lake Store
                UploadFile(localFilePath, remoteFilePath);
                WaitForNewline("File uploaded.", "Listing files and directories.");

                // List the files in the Data Lake Store
                var itemList = ListItems(remoteFolderPath);
                var fileMenuItems = itemList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix));
                Console.WriteLine(String.Join("\r\n", fileMenuItems));
                WaitForNewline("Files and directories listed.", "Appending content to a file.");

                // Append to a file in the Data Lake Store
                AppendToFile(remoteFilePath, "123");
                WaitForNewline("Content appended.", "Downloading a file.");

                // Download a file from the Data Lake Store
                DownloadFile(remoteFilePath, localFilePath);
                WaitForNewline("File downloaded.", "Deleting account.");

                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted. You can now exit.");
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
                if (!String.IsNullOrWhiteSpace(nextAction))
                {
                    Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                    Console.ReadLine();
                    Console.WriteLine(nextAction);
                }
                else
                {
                    Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                    Console.ReadLine();
                }
            }

            // Authenticate the user with AAD through an interactive popup.
            // You need to have an application registered with AAD in order to authenticate.
            //   For more information and instructions on how to register your application with AAD, see:
            //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
            public static TokenCredentials AuthenticateUser(string tenantId, string resource, string appClientId, Uri appRedirectUri, string userId = "")
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);

                var tokenAuthResult = authContext.AcquireToken(resource, appClientId, appRedirectUri,
                    PromptBehavior.Auto, UserIdentifier.AnyUser);

                return new TokenCredentials(tokenAuthResult.AccessToken);
            }
            
            /*
            // Authenticate the application with AAD through the application's secret key.
            // You need to have an application registered with AAD in order to authenticate.
            //   For more information and instructions on how to register your application with AAD, see:
            //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
            public static TokenCredentials AuthenticateApplication(string tenantId, string resource, string appClientId, Uri appRedirectUri, SecureString clientSecret)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
                var credential = new ClientCredential(appClientId, clientSecret);

                var tokenAuthResult = authContext.AcquireToken(resource, credential);

                return new TokenCredentials(tokenAuthResult.AccessToken);
            }
            */

            //Set up clients
            public static void SetupClients(TokenCredentials tokenCreds, string subscriptionId)
            {
                _adlsClient = new DataLakeStoreAccountManagementClient(tokenCreds);
                _adlsClient.SubscriptionId = subscriptionId;

                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }

            // Create account
            public static void CreateAccount()
            {
                // Create ADLS account
                var adlsParameters = new DataLakeStoreAccount(location: _location);
                _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
            }

            // Delete account
            public static void DeleteAccount()
            {
                _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
            }

            // List all ADLS accounts within the subscription
            public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
            {
                var response = _adlsClient.Account.List(_adlsAccountName);
                var accounts = new List<DataLakeStoreAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlsClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }

            // Upload a file
            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
                var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
                var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
                var uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.Execute();
            }

            // Concatenate files
            public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
            {
                _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
            }

            // Get file or directory info
            public static FileStatusProperties GetItemInfo(string path)
            {
                return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
            }

            // List files and directories
            public static List<FileStatusProperties> ListItems(string directoryPath)
            {
                return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
            }

            // Download file
            public static void DownloadFile(string srcPath, string destPath)
            {
                var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
                var fileStream = new FileStream(destPath, FileMode.Create);

                stream.CopyTo(fileStream);
                fileStream.Close();
                stream.Close();
            }

            // Append to file
            public static void AppendToFile(string path, string content)
            {
                var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));

                _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
            }

            // Create a directory
            public static void CreateDirectory(string path)
            {
                _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
            }
        }
    }


## Sonraki adımlar

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)



<!---HONumber=Jun16_HO2-->


