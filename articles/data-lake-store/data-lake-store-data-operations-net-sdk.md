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
ms.date: 11/02/2017
ms.author: nitinme
ms.openlocfilehash: a5d446986f810993d65c7e73eb95eeb2283c39a3
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
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
[GitHub’da](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) bulunan kod örneği, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve depodaki bazı dosyaları silme işlemlerinde size yol gösterir. Makalenin bu bölümü, kodun ana bölümlerinde sizi yönlendirir.

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

      * `Microsoft.Azure.DataLake.Store` - Bu öğreticide v1.0.0 kullanılır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - Bu öğreticide v2.3.1 kullanılır.
    
    **NuGet Paket Yöneticisi**'ni kapatın.

6. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

        using System;
        using System.IO;using System.Threading;
        using System.Linq;
        using System.Text;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.DataLake.Store;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Aşağıda gösterilen değişkenleri bildirin ve yer tutucu değerlerini sağlayın. Ayrıca, burada sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun.

        namespace SdkSample
        {
            class Program
            {
                private static string _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; //Replace this value with the name of your existing Data Lake Store account.        
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kimlik doğrulama, dosyayı karşıya yükleme vb. işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması

* Uygulamanızın son kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da son kullanıcı kimlik doğrulaması gerçekleştirme](data-lake-store-end-user-authenticate-net-sdk.md).
* Uygulamanızın servisler arası kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da servisler arası kimlik doğrulaması gerçekleştirme](data-lake-store-service-to-service-authenticate-net-sdk.md).


## <a name="create-client-object"></a>İstemci nesnesi oluşturma
Aşağıdaki kod parçacığı Data Lake Store ve hizmete verme isteği göndermek için kullanılan dosya sistemi istemci nesnesini oluşturur.

    // Create client objects
    AdlsClient client = AdlsClient.CreateClient(_adlsAccountName, adlCreds);

## <a name="create-a-file-and-directory"></a>Dosya ve dizin oluşturma
Aşağıdaki kod parçacığını uygulamanıza ekleyin. Bu kod parçacığı, bir dosyanın yanı sıra mevcut olmayan herhangi bir üst dizin ekler.

    // Create a file - automatically creates any parent directories that don't exist
    
    string fileName = "/Test/testFilename1.txt";
    using (var streamWriter = new StreamWriter(client.CreateFile(fileName, IfExists.Overwrite)))
    {
        streamWriter.WriteLine("This is test data to write");
        streamWriter.WriteLine("This is line 2");
    }

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme
Aşağıdaki kod parçacığı Data Lake Store hesabında var olan bir dosyaya veri ekler.

    // Append to existing file
    using (var streamWriter = new StreamWriter(client.GetAppendStream(fileName)))
    {
        streamWriter.WriteLine("This is the added line");
    }

## <a name="read-a-file"></a>Dosya okuma
Aşağıdaki kod parçacığı Data Lake Store’daki bir dosyanın içeriğini okur.

    //Read file contents
    using (var readStream = new StreamReader(client.GetReadStream(fileName)))
    {
        string line;
        while ((line = readStream.ReadLine()) != null)
        {
            Console.WriteLine(line);
        }
    }

## <a name="get-file-properties"></a>Dosya özelliklerini alma
Aşağıdaki kod parçacığı bir dosya veya dizin ile ilişkili özellikleri döndürür.

    // Get file properties
    var directoryEntry = client.GetDirectoryEntry(fileName);
    PrintDirectoryEntry(directoryEntry);

`PrintDirectoryEntry` yönteminin tanımı, [Github'daki](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) örneğin bir parçası olarak kullanılabilir. 

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma
Aşağıdaki kod parçacığı Data Lake Store hesabında var olan bir dosyayı yeniden adlandırır.

    // Rename a file
    string destFilePath = "/Test/testRenameDest3.txt";
    client.Rename(fileName, destFilePath, true);

## <a name="enumerate-a-directory"></a>Dizin listeleme
Aşağıdaki kod parçacığı Data Lake Store hesabındaki dizinleri listeler

    // Enumerate directory
    foreach (var entry in client.EnumerateDirectory("/Test"))
    {
        PrintDirectoryEntry(entry);
    }

`PrintDirectoryEntry` yönteminin tanımı, [Github'daki](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) örneğin bir parçası olarak kullanılabilir.

## <a name="delete-directories-recursively"></a>Dizinleri yinelemeli olarak silme
Aşağıdaki kod parçacığı bir dizini ve tüm alt dizinlerini yinelemeli olarak siler.

    // Delete a directory and all it's subdirectories and files
    client.DeleteRecursive("/Test");

## <a name="samples"></a>Örnekler
Data Lake Store Filesystem SDK’sını kullanma örnekleri aşağıda verilmiştir.
* [Github'daki temel örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [Github'daki gelişmiş örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Ayrıca bkz.
* [.NET SDK kullanılarak gerçekleştirilen Data Lake Store'daki hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store .NET SDK Başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
