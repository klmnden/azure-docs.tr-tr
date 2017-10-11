---
title: "Dışa aktarma işi için Azure içeri/dışarı aktarma oluşturun | Microsoft Docs"
description: "Microsoft Azure içeri/dışarı aktarma hizmeti için bir dışarı aktarma işinin oluşturmayı öğrenin."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: bdeac373aa8270bd9de8f135ec7166d744fd83ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-export-job-for-the-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmeti için bir dışa aktarma işi oluşturma
REST API kullanarak Microsoft Azure içeri/dışarı aktarma hizmeti için bir dışarı aktarma işinin oluşturma, aşağıdaki adımları içerir:

-   BLOB'ları dışarı aktarmak için seçme.

-   Bir sevkiyat konum alma.

-   Dışarı aktarma işinin oluşturuluyor.

-   Microsoft'a boş sürücülerinizin desteklenen taşıyıcı hizmeti üzerinden aktarma.

-   Dışarı aktarma işinin paket bilgilerle güncelleştiriliyor.

-   Sürücüleri Microsoft'tan geri alınıyor.

 Bkz: [Blob depolama alanına veri aktarmak için Windows Azure içeri/dışarı aktarma hizmetini kullanarak](storage-import-export-service.md) genel bir bakış içeri/dışarı aktarma hizmeti ve nasıl kullanılacağını gösteren bir öğretici için [Azure portal](https://portal.azure.com/) oluşturmak ve içeri aktarma yönetmek ve işleri vermek için.

## <a name="selecting-blobs-to-export"></a>BLOB'ları dışarı aktarmak için seçme
 Dışarı aktarma işini oluşturmak için depolama hesabınızdan dışarı aktarmak istediğiniz BLOB'ları listesini sağlamanız gerekir. BLOB'ları dışarı seçmek için birkaç yolu vardır:

-   Tek bir blob ve tüm alt anlık görüntü seçmek için göreli blob yolu kullanabilirsiniz.

-   Kendi anlık görüntüleri hariç olmak üzere tek bir blob seçmek için göreli blob yolu kullanabilirsiniz.

-   Tek bir anlık görüntü seçmek için göreli blob yolu ve bir anlık görüntü saati kullanabilirsiniz.

-   Bir blob öneki tüm BLOB'ları ve anlık görüntüleri verilen önekiyle seçmek için kullanabilirsiniz.

-   Tüm BLOB'ları ve anlık görüntü depolama hesabındaki dışarı aktarabilirsiniz.

 BLOB'ları dışarı aktarmak için belirtme hakkında daha fazla bilgi için bkz: [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) işlemi.

## <a name="obtaining-your-shipping-location"></a>Sevkiyat Konumunuz alma
Sevkiyat konumu ad ve adres çağırarak elde etmeniz bir dışarı aktarma işinin oluşturmadan önce [alma konumu](https://portal.azure.com) veya [listesi konumları](/rest/api/storageimportexport/listlocations) işlemi. `List Locations`konumlar ve posta adresleri listesi döndürür. Döndürülen listeden bir konum seçin ve bu adresi, sabit sürücüler sevk. Aynı zamanda `Get Location` işlemi belirli bir konuma için teslimat adresi doğrudan elde edilir.

Sevkiyat konum elde etmek için aşağıdaki adımları izleyin:

-   Konumun depolama hesabınızın adını belirleyin. Bu değer altında bulunabilir **konumu** depolama hesabının alanını **Pano** Klasik portal ya da hizmet yönetimi API işlemi kullanarak için sorgulanan içinde [depolama hesabı özellikleri Al](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Bu depolama hesabını çağırarak işlemek için kullanılabilecek konumu almak `Get Location` işlemi.

-   Varsa `AlternateLocations` özelliği konumun Konum içerir ve ardından bu konumu kullanmak uygundur. Aksi halde çağrı `Get Location` alternatif konumlar biriyle yeniden işlemi. Özgün konuma bakım için geçici olarak kapalı.

## <a name="creating-the-export-job"></a>Dışa aktarma işi oluşturma
 Dışarı aktarma işini oluşturmak için arama [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) işlemi. Aşağıdaki bilgileri sağlamanız gerekir:

-   İş için bir ad.

-   Depolama hesabı adı.

-   Önceki adımda elde sevkiyat konum adı.

-   İş türü (verme).

-   Dönüş adresi dışa aktarma işi tamamlandıktan sonra sürücüleri burada gönderilmelidir.

-   Dışa aktarılacak BLOB'lar (veya blob önekler) listesi.

## <a name="shipping-your-drives"></a>Sürücülerinizin aktarma
 Ardından, Azure içeri/dışarı aktarma aracı göndermek istediğiniz sürücü sayısını belirlemek için verilecek seçtiğiniz BLOB'ları ve disk boyutu göre kullanın. Bkz: [Azure içeri/dışarı aktarma aracı başvurusu](storage-import-export-tool-how-to-v1.md) Ayrıntılar için.

 Tek bir paket sürücüleri paketini ve bunları önceki adımda elde adrese gönderme. Paketinizin bir sonraki adım için izleme sayısını not edin.

> [!NOTE]
>  Sürücülerinizin paketiniz için bir izleme numarası sağlayacak bir desteklenen taşıyıcı hizmeti aracılığıyla hazırlamalısınız.

## <a name="updating-the-export-job-with-your-package-information"></a>Dışarı aktarma işinin paket bilgilerinizi ile güncelleştirme
 İzleme numaranızın aldıktan sonra arama [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) taşıyıcı adı ve iş numaralı izleme işlemi için güncelleştirildi. İsteğe bağlı olarak, sürücüler, dönüş adresi ve sevkiyat tarihi de sayısını belirtebilirsiniz.

## <a name="receiving-the-package"></a>Paket alma
 Sürücülerinizin dışarı aktarma işini işlendikten sonra size, şifrelenmiş verilerle döndürülür. Çağırarak her sürücüleri için BitLocker anahtarını alabilir [alma işi](/rest/api/storageimportexport/jobs#Jobs_Get) işlemi. Ardından anahtar kullanılarak sürücünün kilidini açabilirsiniz. Her sürücüde sürücü bildirim dosyası, her dosya için özgün blob adresi yanı sıra, sürücü dosyaları listesini içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'si kullanma](storage-import-export-using-the-rest-api.md)
