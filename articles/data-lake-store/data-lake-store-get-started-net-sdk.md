---
title: '.NET SDK: Hesap yönetimi işlemleri Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Data Lake depolama Gen1 içinde hesap yönetim işlemlerini gerçekleştirmek için Azure Data Lake depolama Gen1 .NET SDK'sını kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 8ab051d49e7ed67e642ef656dfb382ed07763ed2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60877448"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-net-sdk"></a>Azure Data Lake depolama Gen1 hesap yönetim işlemlerini .NET SDK'sını kullanma
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Bu makalede, Azure Data Lake depolama Gen1 hesap yönetim işlemlerini .NET SDK kullanarak gerçekleştirmeyi öğrenin. Bir Data Lake depolama Gen1 hesabı silme hesapları, vb. bir Azure aboneliğindeki hesapları listeleme, oluşturma, hesap yönetim işlemlerini içerir.

Data Lake depolama Gen1 veri yönetim işlemlerini .NET SDK kullanarak gerçekleştirme talimatları için bkz. [.NET SDK'sı kullanılarak gerçekleştirilen dosya sistemi işlemleri Data Lake depolama Gen1](data-lake-store-data-operations-net-sdk.md).

## <a name="prerequisites"></a>Önkoşullar
* **Visual Studio 2013, 2015 veya 2017**. Aşağıdaki yönergelerde Visual Studio 2017 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

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

7. Değişkenleri bildirin ve yer tutucu değerlerini sağlayın. Ayrıca, sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORAGE-GEN1-NAME>.azuredatalakestore.net"; 
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; 
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";                    
                }
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kimlik doğrulama, dosyayı karşıya yükleme vb. işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması

* Uygulamanız için son kullanıcı kimlik doğrulaması için bkz. [son kullanıcı kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-end-user-authenticate-net-sdk.md).
* Uygulamanız için hizmetten hizmete kimlik doğrulaması için bkz [hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-net-sdk.md).

## <a name="create-client-object"></a>İstemci nesnesi oluşturma
Aşağıdaki kod parçacığı, hesap yönetim isteklerini hizmete iletmek, hesap silme gibi hesap oluşturmak için kullanılan Data Lake depolama Gen1 hesabı istemci nesnesini oluşturur.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(armCreds) { SubscriptionId = _subId };
    
## <a name="create-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabı oluşturun
Aşağıdaki kod parçacığı Data Lake depolama Gen1 hesabı istemci nesnesini oluştururken sağladığınız Azure aboneliğinde bir Data Lake depolama Gen1 hesabı oluşturur.

    // Create Data Lake Storage Gen1 account
    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

## <a name="list-all-data-lake-storage-gen1-accounts-within-a-subscription"></a>Bir abonelik içindeki tüm Data Lake depolama Gen1 hesaplarını listeleme
Sınıf tanımına aşağıdaki yöntemi ekleyin. Aşağıdaki kod parçacığında, belirli bir Azure aboneliği içindeki tüm Data Lake depolama Gen1 hesaplarını listeler.

    // List all Data Lake Storage Gen1 accounts within the subscription
    public static List<DataLakeStoreAccountBasic> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
        var accounts = new List<DataLakeStoreAccountBasic>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="delete-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabını Sil
Aşağıdaki kod parçacığı, daha önce oluşturduğunuz Data Lake depolama Gen1 hesabını siler.

    // Delete Data Lake Storage Gen1 account
    _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);

## <a name="see-also"></a>Ayrıca bkz.
* [.NET SDK'sı kullanılarak gerçekleştirilen dosya sistemi işlemleri Data Lake depolama Gen1](data-lake-store-data-operations-net-sdk.md)
* [Data Lake depolama Gen1 .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
