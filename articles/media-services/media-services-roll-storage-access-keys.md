---
title: "Depolama erişim tuşlarını çalışırken sonra Media Services güncelleştirme | Microsoft Docs"
description: "Bu makaleler depolama erişim tuşlarını çalışırken sonra Media Services güncelleştirme hakkında rehberlik sağlar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 304e72e0d2d4a7e95df513e6d5481def9eae3f68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Depolama erişim tuşlarını çalışırken sonra güncelleştirme medya Hizmetleri

Yeni bir Azure Media Services (AMS) hesabı oluşturduğunuzda, medya içeriğinizi depolamak için kullanılan bir Azure depolama hesabı seçmeniz istenir. Media Services hesabınıza birden fazla depolama hesapları ekleyebilirsiniz. Bu konuda depolama anahtarlarını döndürmek nasıl gösterilmektedir. Ayrıca, depolama hesapları medya eklemek nasıl gösterir. 

Bu konuda açıklanan eylemleri gerçekleştirmek için kullandığınız [ARM API'leri](https://docs.microsoft.com/rest/api/media/mediaservice) ve [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).  Daha fazla bilgi için bkz: [PowerShell ve Resource Manager ile Azure kaynaklarınızı yönetmek nasıl](../azure-resource-manager/powershell-azure-resource-manager.md).

## <a name="overview"></a>Genel Bakış

Yeni bir depolama hesabı oluşturulduğunda, Azure depolama hesabınıza erişim için kimlik doğrulama için kullanılan iki 512 bit depolama erişim tuşu oluşturur. Depolama bağlantılarınızı daha güvenli tutmak için düzenli aralıklarla yeniden oluşturmak ve depolama erişim anahtarınızı döndürmek için önerilir. İki erişim tuşu (birincil ve ikincil), bir erişim anahtarı yeniden bir erişim tuşunu kullanarak depolama hesabına bağlantıları sağlamanıza olanak tanıyan için sağlanır. Bu yordamı, "çalışırken erişim tuşları" olarak da adlandırılır.

Media Services, sağlanan bir depolama anahtarı bağlıdır. Özellikle, belirtilen depolama erişim tuşu akışla aktarmak veya varlıklarınızı indirmek için kullanılan bulucular bağlıdır. AMS hesabı oluşturduğunuzda, bir bağımlılık birincil depolama erişim anahtarı varsayılan olarak alır ancak bir kullanıcı olarak AMS sahip depolama anahtarı güncelleştirebilirsiniz. Bu konuda açıklanan adımları izleyerek kullanmak için hangi anahtarını bilmesi Media Services olanak emin olmanız gerekir.  

>[!NOTE]
> Birden çok depolama hesabınız yoksa, bu yordamı her bir depolama hesabıyla gerçekleştirmelisiniz. Depolama anahtarları döndürme sipariş sabit değildir. İkincil anahtar ilk ve birincil anahtar veya bunun tersini de döndürebilirsiniz.
>
> Bir üretim hesabında bu konuda açıklanan adımları yürütmeden önce bir üretim öncesi hesabında test emin olun.
>

## <a name="steps-to-rotate-storage-keys"></a>Depolama anahtarlarını döndürmek için adımları 
 
 1. Powershell cmdlet'i aracılığıyla depolama hesabının birincil anahtarını değiştirin veya [Azure](https://portal.azure.com/) portal.
 2. Depolama hesabı anahtarlarını almak için medya hesabı zorlamak için uygun parametreleri eşitleme AzureRmMediaServiceStorageKeys cmdlet'ini çağırın
 
    Aşağıdaki örnek, depolama hesaplarına anahtarları eşitleme gösterilmektedir.
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Bir saat ya da bunu bekleyin. Akış senaryoları çalıştığınız doğrulayın.
 4. Powershell cmdlet veya Azure portal üzerinden depolama hesabının ikincil anahtarını değiştirin.
 5. Eşitleme AzureRmMediaServiceStorageKeys powershell yeni depolama hesabı anahtarlarını almak için medya hesabı zorlamak için uygun parametreleri ile çağırın. 
 6. Bir saat ya da bunu bekleyin. Akış senaryoları çalıştığınız doğrulayın.
 
### <a name="a-powershell-cmdlet-example"></a>Powershell cmdlet örneği 

Aşağıdaki örnek, depolama hesabı almak ve AMS hesabının ile eşitlemek gösterilmiştir.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>AMS hesabınızı depolama hesapları ekleme adımları

Aşağıdaki konu AMS hesabınızı depolama hesapları ekleme gösterilmektedir: [bir Media Services hesabına birden çok depolama hesabı ekleme](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>İlgili kaynaklar
Bu belge oluşturma doğrultusunda katkıda bulunan aşağıdaki kişilerin onaylamak isteriz: Cenk Dingiloglu, Milan Gada Seva Titov.
