---
title: Kullanım Azure Data Box, Azure veri kutusu yoğun veri göndermek için sık erişimli, soğuk, arşiv blob katmanı | Veri Microsoft Docs
description: Azure Data Box'ı veya Azure veri kutusu ağır bir uygun blok blob depolama katmanını sık erişimli, soğuk gibi veri göndermek ya da arşivlemek için nasıl kullanılacağını açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 05/24/2019
ms.author: alkohli
ms.openlocfilehash: ea208c395e2ef69ce8f28052351643e963cceb05
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427883"
---
# <a name="use-azure-data-box-or-azure-data-box-heavy-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Uygun Azure depolama blob katmanı veri göndermek için Azure Data Box'ı veya Azure veri kutusu ağır kullanın

Azure Data Box, yüksek miktarda verilerden oluşan özel depolama cihazı sevk tarafından Azure'a taşır. Veri cihazla'kurmak doldurun ve döndürün. Data Box verileri, depolama hesabıyla ilişkili bir varsayılan katman yüklenir. Ardından, verileri başka bir depolama katmanı taşıyabilirsiniz.

Bu makale, Data Box tarafından karşıya yüklenen verileri sık erişimli, soğuk veya arşiv blob katmanı için nasıl taşınabilir açıklar. Bu makale, Azure Data Box hem Azure veri kutusu ağır için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Verileriniz için doğru depolama katmanı seçme

Azure depolama, en uygun maliyetli bir şekilde - sık erişimli, verileri depolamak üç farklı katmanları soğuk ya da arşiv sağlar. Sık erişimli depolama katmanı, sık erişilen verileri depolamak için optimize edilmiştir. Sık erişimli depolama, seyrek erişimli ve depolama, ancak en düşük erişim maliyetini arşiv daha yüksek depolama maliyetleri vardır.

En az 30 gün için depolanması gereken erişilen verileri seyrek erişimli depolama katmanı için sık var. Depolama maliyet soğuk katmanı, sık erişimli depolama katmanı düşük olduğu, ancak veri erişimi ücretleri sık erişimli katmana kıyasla yüksek.

Azure arşiv katmanını çevrimdışı olduğundan ve en düşük depolama maliyetleri, ancak ayrıca en yüksek erişim maliyetleri sunar. Bu katman, en az 180 günlük arşiv depolama alanında kalan veriler için tasarlanmıştır. Ayrıntılar için her birinin bu katmanlar ve fiyatlandırma modeli, Git [depolama katmanlarının karşılaştırması](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

Data Box verilerden veya veri kutusu ağır depolama hesabıyla ilişkili depolama katmanı yüklenmektedir. Bir depolama hesabı oluşturduğunuzda erişim katmanını sıcak veya soğuk olarak belirtebilirsiniz. İş yükü ve maliyeti erişim desenini bağlı olarak, bu verileri başka bir depolama katmanı varsayılan katmanından taşıyabilirsiniz.

Blob Depolama veya genel amaçlı v2 nesne depolama verilerinizi yalnızca Katmanı (GPv2) hesapları. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Verileriniz için doğru depolama katmanı seçme için ayrıntılı konuları gözden geçirin. [Azure Blob Depolama: Premium, sık erişimli, seyrek erişimli ve Arşiv depolama katmanları](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

## <a name="set-a-default-blob-tier"></a>Varsayılan blob katmanı ayarlama

Azure portalında depolama hesabı oluştururken varsayılan blob katmanı belirtilir. GPv2 veya Blob Depolama alanı olarak depolama türü seçildikten sonra erişim katmanı özniteliği belirtilebilir. Varsayılan olarak, sık erişimli katmanı seçilir.

Katmanları olamaz bir veri kutusu veya veri kutusu ağır sıralanırken yeni bir hesap oluşturmaya çalışıyorsanız belirtildi. Hesap oluşturulduktan sonra hesap portalında varsayılan erişim katmanını ayarlamaya değiştirebilirsiniz.

Alternatif olarak, bir depolama hesabı ilk belirtilen erişim katmanı özniteliği ile oluşturun. Data Box veya yoğun veri kutusu siparişi oluştururken, mevcut depolama hesabını seçin. Depolama hesabı oluşturma sırasında varsayılan blob katmanı ayarlama konusunda daha fazla bilgi için Git [Azure portalında depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal).

## <a name="move-data-to-a-non-default-tier"></a>Varsayılan olmayan katmanına veri taşıma

Data Box cihaz verileri için varsayılan katman karşıya yüklendikten sonra verileri varsayılan olmayan bir katmana taşımak isteyebilirsiniz. Bu verileri varsayılandan farklı bir katmana taşımak için iki yolu vardır.

- **Azure Blob Depolama yaşam döngüsü yönetimi** -otomatik olarak veri katmanı veya yaşam döngüsü sonunda sona ilke tabanlı bir yaklaşım kullanın. Daha fazla bilgi için Git [Azure Blob Depolama yaşam döngüsü yönetimi](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts).
- **Komut dosyası** -Azure PowerShell aracılığıyla simgeleştirilmiş bir yaklaşım, blob düzeyinde katman ayarlama özelliği etkinleştirmek için kullanabilirsiniz. Çağırabilirsiniz `SetBlobTier` üzerinde blob katmanı ayarlama işlemi.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Blob katmanı ayarlama için Azure PowerShell'i kullanma

Aşağıdaki adımlarda, arşiv için Azure PowerShell Betiği kullanarak blob katmanı nasıl ayarlanacağı açıklanmaktadır.

1. Yükseltilmiş bir Windows PowerShell oturumu açın. Emin olun, çalışan bir PowerShell 5.0 veya üzeri. Şunu yazın:

   `$PSVersionTable.PSVersion`     

2. Azure PowerShell oturum açın. 

   `Login-AzAccount`  

3. Depolama hesabı, erişim anahtarı, kapsayıcı ve depolama bağlam değişkenleri tanımlayın.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Tüm blobları kapsayıcıda alın.

    `$blobs = Get-AzStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. Arşiv kapsayıcıdaki tüm blobların katmanını ayarlayın.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    Örnek çıktı aşağıda gösterilmiştir:

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    PS C:\WINDOWS\system32> $PSVersionTable.PSVersion

    Major  Minor  Build  Revision
    -----  -----  -----  --------
    5      1      17763  134
    PS C:\WINDOWS\system32> Login-AzAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
   > [!TIP]
   > Veri çubuğunda arşivlemek için isterseniz, içe alma, varsayılan hesap katmanını sık erişimliden ayarlayın. Varsayılan katmanını seyrek erişimli ise, ardından olup olmadığını 30 günlük erken silme cezası veriler hemen arşive taşınır.

## <a name="next-steps"></a>Sonraki adımlar

-  Bilgi nasıl adresine [yaşam döngüsü ilkesi kuralları ile ortak veri katmanlama senaryoları](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples)

