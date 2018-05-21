---
title: Azure içeri/dışarı aktarma için bir içeri aktarma işi oluşturma | Microsoft Docs
description: Microsoft Azure içeri/dışarı aktarma hizmeti için bir alma oluşturmayı öğrenin.
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: ''
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: a80d2169f346238f997c727f0e9d82666897b608
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="creating-an-import-job-for-the-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmeti için bir alma işi oluşturma

REST API kullanarak Microsoft Azure içeri/dışarı aktarma hizmeti için bir alma işi oluşturma, aşağıdaki adımları içerir:

-   Azure içeri/dışarı aktarma aracı olan sürücüleri hazırlanıyor.

-   Sürücü dağıtmayı konum alma.

-   İçe aktarma işi oluşturuluyor.

-   Desteklenen taşıyıcı hizmeti üzerinden Microsoft'a sürücüleri aktarma.

-   İçe aktarma işi ile sevkiyat ayrıntılarını güncelleştiriliyor.

 Bkz: [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanılarak](storage-import-export-service.md) genel bir bakış içeri/dışarı aktarma hizmeti ve nasıl kullanılacağını gösteren bir öğretici için [Azure portal](https://portal.azure.com/) oluşturmak ve içeri aktarma yönetmek ve işleri vermek için.

## <a name="preparing-drives-with-the-azure-importexport-tool"></a>Azure içeri/dışarı aktarma aracı olan sürücüleri hazırlama

Sürücüleri içeri aktarma işi için hazırlamak üzere adımları portalı jobvia oluşturmak veya REST API aracılığıyla aynıdır.

Sürücü hazırlama kısa bir genel bakış aşağıdadır. Başvurmak [Azure alma ExportTool başvurusu](storage-import-export-tool-how-to-v1.md) tam yönergeler için. Azure içeri/dışarı aktarma aracı indirebilirsiniz [burada](http://go.microsoft.com/fwlink/?LinkID=301900).

Sürücünüz hazırlanıyor içerir:

-   İçeri aktarılacak veri tanımlama.

-   Windows Azure depolama alanındaki hedef BLOB'ları tanımlama.

-   Bir veya daha fazla sabit sürücüler, verileri kopyalamak üzere Azure içeri/dışarı aktarma aracını kullanarak.

 Hazırlandığını gibi Azure içeri/dışarı aktarma aracı ayrıca her sürücü için bir bildirim dosyası oluşturur. Bildirim dosyası içerir:

-   Karşıya yükleme ve bu dosyaların eşlemeleri BLOB'lar için yönelik tüm dosyaların listesi.

-   Her bir dosyanın parçalarını sağlama.

-   Her bir blob ile ilişkilendirmek için özellikler ve meta verileri hakkında bilgiler.

-   Karşıya yüklenen bir blob aynı adı taşıyan bir blob kapsayıcısında varsa yapılacak eylem listesi. Olası seçenekler: a) blob ile bu dosyanın üzerine, b) mevcut blob ve dosyayı karşıya yüklemeyi atlayın tutmak, c) bir sonek adına başka dosyalarla çakışmayacak şekilde ekleme.

## <a name="obtaining-your-shipping-location"></a>Sevkiyat Konumunuz alma

Sevkiyat konumu ad ve adres çağırarak elde etmeniz alma işi oluşturmadan önce [listesi konumları](/rest/api/storageimportexport/listlocations) işlemi. `List Locations` konumlar ve posta adresleri listesi döndürür. Döndürülen listeden bir konum seçin ve bu adresi, sabit sürücüler sevk. Aynı zamanda `Get Location` işlemi belirli bir konuma için teslimat adresi doğrudan elde edilir.

 Sevkiyat konum elde etmek için aşağıdaki adımları izleyin:

-   Konumun depolama hesabınızın adını belirleyin. Bu değer altında bulunabilir **konumu** depolama hesabının alanını **Pano** Azure portal ya da hizmet yönetimi API işlemi kullanarak için sorgulanan [depolama hesabı özellikleri Al](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Bu depolama hesabını çağırarak işlemek için konum almak `Get Location` işlemi.

-   Varsa `AlternateLocations` özelliği konumun Konum içerir ve ardından bu konumu kullanmak uygundur. Aksi halde çağrı `Get Location` alternatif konumlar biriyle yeniden işlemi. Özgün konuma bakım için geçici olarak kapalı.

## <a name="creating-the-import-job"></a>İçe aktarma işi oluşturma
İçe aktarma işi oluşturmak için arama [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) işlemi. Aşağıdaki bilgileri sağlamanız gerekir:

-   İş için bir ad.

-   Depolama hesabı adı.

-   Önceki adımda elde edilen sevkiyat konum adı.

-   İş türü (içe aktarma).

-   Dönüş adresi alma işi tamamlandıktan sonra sürücüleri burada gönderilmelidir.

-   İş sürücülerin listesi. Her bir sürücü için sürücü hazırlık adımında edinilen aşağıdaki bilgileri içermelidir:

    -   Sürücü Kimliği

    -   BitLocker anahtar

    -   Sabit sürücüde bildirim dosyası göreli yolu

    -   Bildirim dosyası MD5 karma Base16 kodlanmış

## <a name="shipping-your-drives"></a>Sürücülerinizin aktarma
Önceki adımda elde ettiğiniz adrese sürücülerinizin hazırlamalısınız ve paket izleme numarasıyla içeri/dışarı aktarma hizmeti sağlamalısınız.

> [!NOTE]
>  Sürücülerinizin paketiniz için bir izleme numarası sağlayacak bir desteklenen taşıyıcı hizmeti aracılığıyla hazırlamalısınız.

## <a name="updating-the-import-job-with-your-shipping-information"></a>İçe aktarma işi ile sevkiyat bilgilerinizi güncelleştirme
İzleme numaranızın aldıktan sonra arama [güncelleştirme işi özellikleri](/api/storageimportexport/jobs#Jobs_Update) Sevkiyat taşıyıcı adı, iş için izleme numarası ve dönüş Sevkiyat taşıyıcı hesap numarası güncelleştirmek için güncelleştirme işlemi. İsteğe bağlı olarak, sürücüler ve sevkiyat tarihi de sayısını belirtebilirsiniz.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'si kullanma](storage-import-export-using-the-rest-api.md)
