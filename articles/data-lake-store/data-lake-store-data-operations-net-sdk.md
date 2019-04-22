---
title: '.NET SDK: Azure Data Lake depolama Gen1 gerçekleştirilen dosya sistemi işlemleri | Microsoft Docs'
description: Dosya sistemi işlemleri Data Lake depolama Gen1 gibi gerçekleştirmek için kullanım Azure Data Lake depolama Gen1 .NET SDK, klasörleri, vb. oluşturun.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 02091f1b650e3e9932f9924bf36a5841861d3b1e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58876964"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-net-sdk"></a>Azure Data Lake depolama Gen1 .NET SDK'sı kullanılarak gerçekleştirilen dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

Bu makalede .NET SDK'sı kullanılarak gerçekleştirilen dosya sistemi işlemleri Data Lake depolama Gen1 gerçekleştirmeyi öğrenin. Dosya indirme gibi dosyaları karşıya yükleme, bir Data Lake depolama Gen1 hesabında klasör oluşturma, dosya sistemi işlemlerini içerir.

Data Lake depolama Gen1 hesap yönetim işlemlerini .NET SDK kullanarak gerçekleştirme talimatları için bkz. [hesap yönetim işlemlerini .NET SDK kullanarak Data Lake depolama Gen1](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Önkoşullar
* **Visual Studio 2013, 2015 veya 2017**. Aşağıdaki yönergelerde Visual Studio 2017 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake depolama Gen1 hesabı**. Bir hesap oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)

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
                private static string _adlsg1AccountName = "<DATA-LAKE-STORAGE-GEN1-NAME>.azuredatalakestore.net";        
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kimlik doğrulama, dosyayı karşıya yükleme vb. işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz.

## <a name="authentication"></a>Authentication

* Uygulamanız için son kullanıcı kimlik doğrulaması için bkz. [son kullanıcı kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-end-user-authenticate-net-sdk.md).
* Uygulamanız için hizmetten hizmete kimlik doğrulaması için bkz [hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-net-sdk.md).


## <a name="create-client-object"></a>İstemci nesnesi oluşturma
Aşağıdaki kod parçacığı hizmete verme isteği göndermek için kullanılır Data Lake depolama Gen1 dosya sistemi istemci nesnesini oluşturur.

    // Create client objects
    AdlsClient client = AdlsClient.CreateClient(_adlsg1AccountName, adlCreds);

## <a name="create-a-file-and-directory"></a>Dosya ve dizin oluşturma
Aşağıdaki kod parçacığını uygulamanıza ekleyin. Bu kod parçacığı, bir dosyanın yanı sıra mevcut olmayan herhangi bir üst dizin ekler.

    // Create a file - automatically creates any parent directories that don't exist
    // The AdlsOutputStream preserves record boundaries - it does not break records while writing to the store
    using (var stream = client.CreateFile(fileName, IfExists.Overwrite))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is test data to write.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);

        textByteArray = Encoding.UTF8.GetBytes("This is the second line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme
Aşağıdaki kod parçacığı Data Lake depolama Gen1 hesabında var olan bir dosyaya veri ekler.

    // Append to existing file
    using (var stream = client.GetAppendStream(fileName))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is the added line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="read-a-file"></a>Dosya okuma
Aşağıdaki kod parçacığı Data Lake depolama Gen1 bir dosyanın içeriğini okur.

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

Tanımı `PrintDirectoryEntry` yöntemi kullanılabilir örnek bir parçası olarak [github'da](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted). 

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma
Aşağıdaki kod parçacığı Data Lake depolama Gen1 hesabında var olan bir dosyayı yeniden adlandırır.

    // Rename a file
    string destFilePath = "/Test/testRenameDest3.txt";
    client.Rename(fileName, destFilePath, true);

## <a name="enumerate-a-directory"></a>Dizin listeleme
Aşağıdaki kod parçacığı Data Lake depolama Gen1 hesabındaki dizinleri listeler

    // Enumerate directory
    foreach (var entry in client.EnumerateDirectory("/Test"))
    {
        PrintDirectoryEntry(entry);
    }

Tanımı `PrintDirectoryEntry` yöntemi kullanılabilir örnek bir parçası olarak [github'da](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted).

## <a name="delete-directories-recursively"></a>Dizinleri yinelemeli olarak silme
Aşağıdaki kod parçacığı bir dizini ve tüm alt dizinlerini yinelemeli olarak siler.

    // Delete a directory and all its subdirectories and files
    client.DeleteRecursive("/Test");

## <a name="samples"></a>Örnekler
Data Lake depolama Gen1 dosya sistemi SDK'sını kullanma örnekleri aşağıda verilmiştir.
* [Github'daki temel örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [Github'daki Gelişmiş örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake depolama Gen1 hesap yönetim işlemlerini .NET SDK'sını kullanma](data-lake-store-get-started-net-sdk.md)
* [Data Lake depolama Gen1 .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
