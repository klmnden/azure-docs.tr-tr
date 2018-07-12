---
title: Bir dışarı aktarma için Azure içeri/dışarı aktarma işi oluşturma | Microsoft Docs
description: Microsoft Azure içeri/dışarı aktarma hizmeti için dışarı aktarma işi oluşturmayı öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3fb3f2af5e5cebcac21f4372bc9d9dc9ee837202
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232276"
---
# <a name="creating-an-export-job-for-the-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmeti için dışarı aktarma işi oluşturma
Microsoft Azure içeri/dışarı aktarma hizmeti REST API kullanarak dışarı aktarma işi oluşturma, aşağıdaki adımları içerir:

-   Dışarı aktarılacak bloblar seçme.

-   Sevkiyat konumu edinme.

-   Dışarı aktarma işi oluşturma.

-   Microsoft'a boş sürücülerinizin desteklenen taşıyıcı hizmeti aracılığıyla aktarma.

-   Dışarı aktarma işi paket bilgileriyle güncelleştiriliyor.

-   Sürücüleri, Microsoft'tan geri alınıyor.

 Bkz: [Blob depolama alanına veri aktarmak için Windows Azure içeri/dışarı aktarma hizmetini kullanarak](storage-import-export-service.md) içeri/dışarı aktarma hizmeti ile nasıl kullanılacağını gösteren bir öğreticiye genel bakış [Azure portalında](https://portal.azure.com/) oluşturmak için ve içeri aktarma yönetin ve dışarı aktarma işleri.

## <a name="selecting-blobs-to-export"></a>Dışarı aktarılacak bloblar seçme
 Dışarı aktarma işi oluşturmak için depolama hesabınızdan dışarı aktarmak istediğiniz BLOB listesini sağlamanız gerekir. Dışarı aktarılacak bloblar seçmek için birkaç yolu vardır:

-   Tek bir blob ve tüm anlık görüntüleri seçmek için göreli blob yolu kullanabilirsiniz.

-   Tek bir blob anlık görüntüleri hariç seçmek için göreli blob yolu kullanabilirsiniz.

-   Tek bir anlık görüntü seçmek için göreli blob yolu ve anlık görüntü zaman kullanabilirsiniz.

-   Tüm BLOB'ları ve belirtilen öneke sahip anlık görüntü seçmek için blob öneki kullanabilirsiniz.

-   Tüm BLOB'ları ve anlık görüntü depolama hesabındaki dışarı aktarabilirsiniz.

 Dışarı aktarılacak bloblar belirtme hakkında daha fazla bilgi için bkz. [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) işlemi.

## <a name="obtaining-your-shipping-location"></a>Sevkiyat konumunuz edinme
Bir dağıtımı konum adı ve adresi çağırarak elde etmeniz dışarı aktarma işi oluşturmadan önce [alma konumu](https://portal.azure.com) veya [List Locations](/rest/api/storageimportexport/listlocations) işlemi. `List Locations` konumlar ve posta adresleri listesi döndürür. Döndürülen listeden bir konum seçin ve sabit sürücülerinizi bu adrese gönderin. Ayrıca `Get Location` doğrudan belirli bir konumun teslimat adresini edinme işlemi.

Sevkiyat konum elde etmek için aşağıdaki adımları izleyin:

-   Konumun depolama hesabınızın adını belirleyin. Bu değeri altında bulunabilir **konumu** depolama hesabının ile sekmesindeki **Pano** Azure portal ya da hizmet yönetimi API işlemi'ni kullanarak için sorgulanan [depolama hesabı edinin Özellikleri](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Bu depolama hesabı çağırarak işlemek için uygun olan konumu almak `Get Location` işlemi.

-   Varsa `AlternateLocations` özelliği konumun Konum içerir ve bu konumu kullanmak uygundur. Aksi takdirde, çağrı `Get Location` alternatif konumlar biriyle yeniden işlemi. Özgün konuma geçici olarak bakım için kapalı olabilir.

## <a name="creating-the-export-job"></a>Dışarı aktarma işi oluşturma
 Dışarı aktarma işi oluşturmak için arama [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) işlemi. Aşağıdaki bilgileri sağlamanız gerekir:

-   İş için bir ad.

-   Depolama hesabı adı.

-   Önceki adımda elde edilen sevkiyat konum adı.

-   Bir iş türü (dışarı aktarma).

-   Sürücüleri, dışarı aktarma işi tamamlandıktan sonra nereye gönderileceğini dönüş adresi.

-   Dışa aktarılacak bloblar (veya blob ön ekleri) listesi.

## <a name="shipping-your-drives"></a>Sürücülerinizi aktarma
 Ardından, Azure içeri/dışarı aktarma aracı göndermek istediğiniz sürücü sayısını belirlemek için aktarılabilmesi için seçtiğiniz BLOB'ları ve sürücünün boyutuna bağlı olarak kullanın. Bkz: [Azure içeri/dışarı aktarma aracı başvurusu](storage-import-export-tool-how-to-v1.md) Ayrıntılar için.

 Tek bir pakette sürücü paketi ve bunları önceki adımda elde edilen adresine gönderin. Paketinizin bir sonraki adım için takip numarasını not edin.

> [!NOTE]
>  Sürücülerinizi paketiniz için bir izleme numarası sağlayacak bir desteklenen taşıyıcı hizmeti aracılığıyla göndermeniz gerekir.

## <a name="updating-the-export-job-with-your-package-information"></a>Paket bilgileriyle dışarı aktarma işi güncelleştiriliyor
 İzleme numaranızı sonra çağrı [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) taşıyıcı adı ve numarası iş için izleme işlemi için güncelleştirildi. İsteğe bağlı olarak, sürücüleri, dönüş adresi ve gönderim tarihi de sayısını belirtebilirsiniz.

## <a name="receiving-the-package"></a>Paketi alma
 Sürücüler, dışarı aktarma işi işlendikten sonra şifrelenmiş verilerinizle döndürülür. Çağırarak her sürücüleri için BitLocker anahtarı alabilirsiniz [alma işi](/rest/api/storageimportexport/jobs#Jobs_Get) işlemi. Ardından anahtar kullanılarak sürücünün kilidini açabilirsiniz. Her sürücüde sürücü bildirim dosyası, her dosya için özgün blob adresini yanı sıra, sürücü dosyaları listesini içerir.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
