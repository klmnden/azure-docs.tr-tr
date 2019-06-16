---
title: Azure içeri/dışarı aktarma için bir içeri aktarma işi oluşturma | Microsoft Docs
description: Microsoft Azure içeri/dışarı aktarma hizmeti alma oluşturmayı öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: fa76f4fb5d4da5fd00bb9fa4ed862c6977a47e90
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61483086"
---
# <a name="creating-an-import-job-for-the-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmeti için bir içeri aktarma işi oluşturma

Microsoft Azure içeri/dışarı aktarma hizmeti REST API kullanarak içeri aktarma işi oluşturma, aşağıdaki adımları içerir:

- Azure içeri/dışarı aktarma aracı ile sürücüleri hazırlanıyor.

- Sürücü göndermeye konum edinme.

- İçeri aktarma işi oluşturma.

- İçin Microsoft sürücüleri desteklenen taşıyıcı hizmeti aracılığıyla aktarma.

- İçeri aktarma işi ile Sevkiyat ayrıntıları güncelleştiriliyor.

  Bkz: [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmetini kullanarak](storage-import-export-service.md) içeri/dışarı aktarma hizmeti ile nasıl kullanılacağını gösteren bir öğreticiye genel bakış [Azure portalında](https://portal.azure.com/) oluşturmak için içeri aktarma yönetme ve dışarı aktarma işleri.

## <a name="preparing-drives-with-the-azure-importexport-tool"></a>Azure içeri/dışarı aktarma aracı ile sürücüleri hazırlama

Proje portalı veya REST API aracılığıyla oluşturmak isteyip sürücüleri için içeri aktarma işine hazırlamak için adımları aynıdır.

Sürücü hazırlık kısa bir genel bakış aşağıda verilmiştir. Başvurmak [Azure içeri aktarma ExportTool başvurusu](storage-import-export-tool-how-to-v1.md) eksiksiz yönergeler için. Azure içeri/dışarı aktarma aracı indirebileceğiniz [burada](https://go.microsoft.com/fwlink/?LinkID=301900).

Sürücünüz hazırlanıyor içerir:

- İçeri aktarılacak veri tanımlama.

- Windows Azure depolama alanındaki hedef BLOB'ları tanımlama.

- Bir veya daha fazla sabit sürücüler için verilerinizi kopyalamak için Azure içeri/dışarı aktarma Aracı'nı kullanarak.

  Hazırlandığı sırada Azure içeri/dışarı aktarma aracı ayrıca her sürücü için bir bildirim dosyası oluşturur. Bir bildirim dosyası içerir:

- Karşıya yükleme ve bu dosyaların eşlemeleri bloblarına yönelik tüm dosyaları numaralandırması.

- Her dosyanın parçalarını sağlama.

- Her blob ile ilişkilendirmek için özellikleri ve meta verileri hakkında bilgiler.

- Karşıya yüklenen bir blob kapsayıcısında mevcut bir bloba aynı ada sahipse gerçekleştirilecek eylemi bir listesi. Olası seçenekler: a) blob dosyanızın üzerine, (b) dosyayı karşıya yüklemeyi atlayın ve mevcut blob tutmak, c) bir sonek adına diğer dosyaları ile çakışmadığından emin Ekle.

## <a name="obtaining-your-shipping-location"></a>Sevkiyat konumunuz edinme

Bir dağıtımı konum adı ve adresi çağırarak elde etmeniz içeri aktarma işine oluşturmadan önce [List Locations](https://docs.microsoft.com/rest/api/storageimportexport/locations/list) işlemi. `List Locations` konumlar ve posta adresleri listesi döndürür. Döndürülen listeden bir konum seçin ve sabit sürücülerinizi bu adrese gönderin. Ayrıca `Get Location` doğrudan belirli bir konumun teslimat adresini edinme işlemi.

 Sevkiyat konum elde etmek için aşağıdaki adımları izleyin:

-   Konumun depolama hesabınızın adını belirleyin. Bu değeri altında bulunabilir **konumu** depolama hesabının ile sekmesindeki **Pano** Azure portal ya da hizmet yönetimi API işlemi'ni kullanarak için sorgulanan [depolama hesabı edinin Özellikleri](/rest/api/storagerp/storageaccounts).

-   Bu depolama hesabı çağırarak işlemek kullanılabilir olan konumu almak `Get Location` işlemi.

-   Varsa `AlternateLocations` özelliği konumun Konum içerir ve bu konumu kullanmak uygundur. Aksi takdirde, çağrı `Get Location` alternatif konumlar biriyle yeniden işlemi. Özgün konuma geçici olarak bakım için kapalı olabilir.

## <a name="creating-the-import-job"></a>İçeri aktarma işi oluşturma
İçeri aktarma işi oluşturmak için arama [Put işlemini](/rest/api/storageimportexport/jobs) işlemi. Aşağıdaki bilgileri sağlamanız gerekir:

-   İş için bir ad.

-   Depolama hesabı adı.

-   Önceki adımda elde edilen sevkiyat konum adı.

-   Bir iş türü (alma).

-   Sürücüleri içeri aktarma işi tamamlandıktan sonra burada gönderilmesi gereken dönüş adresi.

-   İş sürücülerin listesi. Her bir sürücü için sürücü hazırlık adımı sırasında edinilen aşağıdaki bilgileri içermelidir:

    -   Sürücü Kimliği

    -   BitLocker anahtarı

    -   Sabit sürücünün bildirim dosyasına göreli yol

    -   Kodlamalı Base16 bildirim dosyası MD5 karma

## <a name="shipping-your-drives"></a>Sürücülerinizi aktarma
Önceki adımda elde ettiğiniz adrese sürücülerinizin hazırlamalısınız ve içeri/dışarı aktarma hizmeti izleme paketi sayısı ile sağlamanız gerekir.

> [!NOTE]
>  Sürücülerinizi paketiniz için bir izleme numarası sağlayacak bir desteklenen taşıyıcı hizmeti aracılığıyla göndermeniz gerekir.

## <a name="updating-the-import-job-with-your-shipping-information"></a>Sevkiyat bilgilerinizi içeri aktarma işi güncelleştiriliyor
İzleme numaranızı sonra çağrı [güncelleştirme işi özellikleri](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Update) Sevkiyat taşıyıcısı adı, iş için takip numarasını ve taşıyıcı hesap numarası iade gönderimi için güncelleştirme işlemi. İsteğe bağlı olarak, sürücüler ve gönderim tarihi de belirtebilirsiniz.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
