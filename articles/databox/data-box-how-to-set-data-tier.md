---
title: -Sık erişimli, soğuk, arşiv - blok blob katmanı için veri göndermek için Azure Data Box'ı kullanma | Veri Microsoft Docs
description: Uygun blok blob depolama katmanı sık erişimli, soğuk gibi veri göndermek ya da arşivlemek için Azure Data Box'ı kullanmayı açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: bb1d6c5bd51fcfe35127c2f6d8dd6a80b727c45f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517156"
---
# <a name="use-azure-data-box-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Uygun Azure depolama blob katmanı veri göndermek için Azure Data Box'ı kullanma

Azure Data Box, yüksek miktarda verilerden oluşan özel depolama cihazı sevk tarafından Azure'a taşır. Veri cihazla'kurmak doldurun ve döndürün. Data Box verileri, depolama hesabıyla ilişkili bir varsayılan katman yüklenir. Ardından, verileri başka bir depolama katmanı taşıyabilirsiniz.

Bu makale, Data Box tarafından karşıya yüklenen verileri sık erişimli, soğuk veya arşiv blob katmanı için nasıl taşınabilir açıklar.  

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Verileriniz için doğru depolama katmanı seçme

Azure depolama, en uygun maliyetli bir şekilde - sık erişimli, verileri depolamak üç farklı katmanları soğuk ya da arşiv sağlar. Sık erişimli depolama katmanı, sık erişilen verileri depolamak için optimize edilmiştir. Sık erişimli depolama, seyrek erişimli ve depolama, ancak en düşük erişim maliyetini arşiv daha yüksek depolama maliyetleri vardır.

En az 30 gün için depolanması gereken erişilen verileri seyrek erişimli depolama katmanı için sık var. Depolama maliyet soğuk katmanı, sık erişimli depolama katmanı düşük olduğu, ancak veri erişimi ücretleri sık erişimli katmana kıyasla yüksek.

Azure arşiv katmanını çevrimdışı olduğundan ve en düşük depolama maliyetleri, ancak ayrıca en yüksek erişim maliyetleri sunar. Bu katman, en az 180 günlük arşiv depolama alanında kalan veriler için tasarlanmıştır. Ayrıntılar için her birinin bu katmanlar ve fiyatlandırma modeli, Git [depolama katmanlarının karşılaştırması](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

Data Box verileri, depolama hesabıyla ilişkili depolama katmanı için yüklenir. Bir depolama hesabı oluşturduğunuzda erişim katmanını sıcak veya soğuk olarak belirtebilirsiniz. İş yükü ve maliyeti erişim desenini bağlı olarak, bu verileri başka bir depolama katmanı varsayılan katmanından taşıyabilirsiniz.

Blob Depolama veya genel amaçlı v2 nesne depolama verilerinizi yalnızca Katmanı (GPv2) hesapları. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Verileriniz için doğru depolama katmanı seçme için ayrıntılı konuları gözden geçirin. [Azure Blob Depolama: Premium, sık erişimli, seyrek erişimli ve Arşiv depolama katmanları](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

## <a name="set-a-default-blob-tier"></a>Varsayılan blob katmanı ayarlama

Azure portalında depolama hesabı oluştururken varsayılan blob katmanı belirtilir. GPv2 veya Blob Depolama alanı olarak depolama türü seçildikten sonra erişim katmanı özniteliği belirtilebilir. Varsayılan olarak, sık erişimli katmanı seçilir.

Katmanları olamaz Data Box sipariş zaman yeni bir hesap oluşturmaya çalışıyorsanız belirtildi. Hesap oluşturulduktan sonra hesap portalında varsayılan erişim katmanını ayarlamaya değiştirebilirsiniz.

Alternatif olarak, bir depolama hesabı ilk belirtilen erişim katmanı özniteliği ile oluşturun. Data Box Siparişiniz oluştururken, mevcut depolama hesabını seçin. Depolama hesabı oluşturma sırasında varsayılan blob katmanı ayarlama konusunda daha fazla bilgi için Git [Azure portalında depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal).

## <a name="move-data-to-a-non-default-tier"></a>Varsayılan olmayan katmanına veri taşıma

Data Box verileri için varsayılan katman karşıya yüklendikten sonra verileri varsayılan olmayan bir katmana taşımak isteyebilirsiniz. Bu verileri varsayılandan farklı bir katmana taşımak için iki yolu vardır.

- **Azure Blob Depolama yaşam döngüsü yönetimi** -otomatik olarak veri katmanı veya yaşam döngüsü sonunda sona ilke tabanlı bir yaklaşım kullanın. Daha fazla bilgi için Git [Azure Blob Depolama yaşam döngüsü yönetimi](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts).
- **Komut dosyası** -Azure PowerShell aracılığıyla simgeleştirilmiş bir yaklaşım, blob düzeyinde katman ayarlama özelliği etkinleştirmek için kullanabilirsiniz. Çağırabilirsiniz `SetBlobTier` üzerinde blob katmanı ayarlama işlemi.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Blob katmanı ayarlama için Azure PowerShell'i kullanma

Aşağıdaki adımlarda, arşiv için Azure PowerShell Betiği kullanarak blob katmanı nasıl ayarlanacağı açıklanmaktadır.

1. Yükseltilmiş bir Windows PowerShell oturumu açın. Emin olun, çalışan bir PowerShell 5.0 veya üzeri. Şunu yazın:

   `$PSVersionTable.PSVersion`     

2. Azure PowerShell oturum açın. 

   `Login-AzureRmAccount`  

3. Depolama hesabı, erişim anahtarı, kapsayıcı ve depolama bağlam değişkenleri tanımlayın.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Tüm blobları kapsayıcıda alın.

    `$blobs = Get-AzureStorageBlob -Container "<enter container name>" -Context $ctx`
 
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
    PS C:\WINDOWS\system32> Login-AzureRmAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzureStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
   > [!TIP]
   > Veri çubuğunda arşivlemek için isterseniz, içe alma, varsayılan hesap katmanını sık erişimliden ayarlayın. Varsayılan katmanını seyrek erişimli ise, ardından olup olmadığını 30 günlük erken silme cezası veriler hemen arşive taşınır.

## <a name="next-steps"></a>Sonraki adımlar

-  Bilgi nasıl adresine [yaşam döngüsü ilkesi kuralları ile ortak veri katmanlama senaryoları](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples)

