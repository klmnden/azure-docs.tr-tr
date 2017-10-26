---
title: ".NET SDK: Azure Data Lake Store'daki dosya sistemi işlemleri | Microsoft Docs"
description: "Data Lake Store'da klasör oluşturma gibi dosya sistemi işlemlerini gerçekleştirmek için Azure Data Lake Store .NET SDK'sını kullanın."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: 7f6319dcf1ae66a686dd1c2ea2810b3041183098
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-net-sdk"></a>Azure Data Lake Store'da .NET SDK'sı kullanılarak gerçekleştirilen dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

Bu makalede .NET SDK kullanarak Data Lake Store'daki dosya sistemi işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz. Dosya sistemi işlemleri Data Lake Store hesabında klasör oluşturma, dosya yükleme, dosya indirme gibi işlemleri kapsar.

Data Lake Store'da hesap yönetim işlemlerini .NET SDK kullanarak gerçekleştirme talimatları için bkz. [Data Lake Store'da .NET SDK kullanılarak gerçekleştirilen hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2013, 2015 veya 2017**. Aşağıdaki yönergelerde Visual Studio 2017 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store hesabı**. Hesap oluşturmaya ilişkin yönergeler için bkz. [Azure Data Lake Store kullanmaya başlama](data-lake-store-get-started-portal.md)

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

7. Aşağıda gösterilen değişkenleri bildirin ve yer tutucu değerlerini sağlayın. Ayrıca, burada sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static async void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kimlik doğrulama, dosyayı karşıya yükleme vb. işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması

* Uygulamanızın son kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da son kullanıcı kimlik doğrulaması gerçekleştirme](data-lake-store-end-user-authenticate-net-sdk.md).
* Uygulamanızın servisler arası kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da servisler arası kimlik doğrulaması gerçekleştirme](data-lake-store-service-to-service-authenticate-net-sdk.md).


## <a name="create-client-objects"></a>İstemci nesneleri oluşturma
Aşağıdaki kod parçacığı Data Lake Store hesabını ve hizmete verme isteği göndermek için kullanılan dosya sistemi istemci nesnelerini oluşturur.

    // Create client objects
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

## <a name="create-a-directory"></a>Dizin oluşturma
Sınıfınıza aşağıdaki yöntemi ekleyin. Kod parçacığında, bir Data Lake Store hesabında dizin oluşturmak için kullanabileceğiniz bir `CreateDirectory()` yöntemi gösterilmiştir.

    // Create a directory
    public static void CreateDirectory(string path)
    {
            _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

Aşağıdaki kod parçacığını `Main()` yönteminize ekleyerek `CreateDirectory()` yöntemini çağırın.

    CreateDirectory(remoteFolderPath);
    Console.WriteLine("Created a directory in the Data Lake Store account. Press any key to continue ...");
    Console.ReadLine();

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme
Sınıfınıza aşağıdaki yöntemi ekleyin. Kod parçacığında, bir Data Lake Store hesabına dosya yüklemek için kullanabileceğiniz bir `UploadFile()` yöntemi gösterilmiştir. SDK bir yerel dosya ile Data Lake Store dosya yolu arasında yinelemeli karşıya yükleme ve indirmeyi destekler.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

Aşağıdaki kod parçacığını `Main()` yönteminize ekleyerek `UploadFile()` yöntemini çağırın.

    UploadFile(localFilePath, remoteFilePath, true);
    Console.WriteLine("Uploaded file in the directory. Press any key to continue...");
    Console.ReadLine();
    

## <a name="get-file-or-directory-info"></a>Dosya veya dizin bilgilerini alma
Aşağıdaki kod parçacığında, Data Lake Store'da kullanılabilir olan bir dosya veya dizin ile ilgili bilgileri almak için kullanabileceğiniz bir `GetItemInfo()` yöntemi gösterilmiştir.

    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

Aşağıdaki kod parçacığını `Main()` yönteminize ekleyerek `GetItemInfo()` yöntemini çağırın.

    
    var fileProperties = GetItemInfo(remoteFilePath);
    Console.WriteLine("The owner of the file at the path is:", fileProperties.Owner);
    Console.WriteLine("Press any key to continue...");
    Console.ReadLine();

## <a name="list-file-or-directories"></a>Dosyayı veya dizinleri listeleme
Aşağıdaki kod parçacığında, bir Data Lake Store hesabındaki dosyaları ve dizinleri listelemek için kullanabileceğiniz bir `ListItems()` yöntemi gösterilmiştir.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

Aşağıdaki kod parçacığını `Main()` yönteminize ekleyerek `ListItems()` yöntemini çağırın.

    var itemList = ListItems(remoteFolderPath);
    var fileMenuItems = itemList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix));
    Console.WriteLine(String.Join("\r\n", fileMenuItems));
    Console.WriteLine("Files and directories listed. Press any key to continue ...");
    Console.ReadLine();

## <a name="concatenate-files"></a>Dosyaları birleştirme
Aşağıdaki kod parçacığında, dosyaları birleştirmek için kullanabileceğiniz bir `ConcatenateFiles()` yöntemi gösterilmiştir.

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

Aşağıdaki kod parçacığını `Main()` yönteminize ekleyerek `ConcatenateFiles()` yöntemini çağırın. Bu kod parçacığında Data Lake Store hesabınıza başka bir dosya yüklediğiniz ve dosyanın yolunun *anotherRemoteFilePath* dizesinde ilettiğiniz kabul edilmektedir.

    string[] stringOfFiles = new String[] {remoteFilePath, anotherRemoteFilePath};
    string destFilePath = Path.Combine(remoteFolderPath, "Concatfile.txt");
    ConcatenateFiles(stringOfFiles, destFilePath);
    Console.WriteLine("Files concatinated. Press any key to continue ...");
    Console.ReadLine();

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme
Aşağıdaki kod parçacığında, bir Data Lake Store hesabında zaten depolanmış olan bir dosyanın sonuna veri eklemek için kullanabileceğiniz bir `AppendToFile()` yöntemi gösterilmiştir.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

Aşağıdaki kod parçacığını `Main()` yönteminize ekleyerek `AppendToFile()` yöntemini çağırın.

    AppendToFile(remoteFilePath, "123");
    Console.WriteLine("Content appended. Press any key to continue ...");
    Console.ReadLine();

## <a name="download-a-file"></a>Dosya indirme
Aşağıdaki kod parçacığında, bir Data Lake Store hesabındaki bir dosyayı indirmek için kullanabileceğiniz bir `DownloadFile()` yöntemi gösterilmiştir.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:true);
    }

Aşağıdaki kod parçacığını `Main()` yönteminize ekleyerek `DownloadFile()` yöntemini çağırın.

    DownloadFile(destFilePath, localFilePath);
    Console.WriteLine("File downloaded. Press any key to continue ...");
    Console.ReadLine();

## <a name="see-also"></a>Ayrıca bkz.
* [.NET SDK kullanılarak gerçekleştirilen Data Lake Store'daki hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store .NET SDK Başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
