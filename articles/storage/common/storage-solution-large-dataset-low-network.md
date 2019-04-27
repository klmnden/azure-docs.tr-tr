---
title: Azure veri aktarımı düşük veya hiçbir ağ bant genişliği ile büyük veri kümeleri için Seçenekler | Microsoft Docs
description: Veri aktarımı için bir Azure çözümü seçin, ortamınızda hiçbir ağ bant genişliği sınırlı ve büyük veri kümeleri aktarmak planlama hakkında bilgi edinin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 4c4ac9489b9613b2eeaf26a3df9f4cbc664a1026
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730699"
---
# <a name="data-transfer-for-large-datasets-with-low-or-no-network-bandwidth"></a>Düşük veya hiçbir ağ bant genişliği ile büyük veri kümeleri için veri aktarımı
 
Bu makalede ortamınızda hiçbir ağ bant genişliği sınırlı ve büyük veri kümeleri aktarmak planlama yaparken çözümleri veri aktarımı için genel bir bakış sağlar. Makalede de önerilen veri aktarımı seçenekleri ve bu senaryo için ilgili önemli özellik matrisi açıklanır.

Kullanılabilir veri aktarımı seçeneklerini tümü bir genel bakış anlamak için Git [bir Azure veri aktarım çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="offline-transfer-or-network-transfer"></a>Çevrimdışı aktarımı veya ağ aktarımı

Büyük veri kümeleri, veri birkaç PBs birkaç TB'a sahip kapsıyor. Hiçbir ağ bant genişliği sınırlı, ağınıza yavaşsa veya güvenilir değil. Ayrıca:

- Ağ aktarım maliyetlerini tarafından Internet hizmet sağlayıcılarının (ISS'ler) sınırlıdır.
- Güvenlik veya kuruluş ilkeleri giden bağlantılar hassas verilerle ilgilenirken izin vermez.

Tüm Yukarıdaki örneklerde, tek seferde toplu veri aktarımı yapmak için fiziksel bir cihaz kullanın. Data Box Disk, Data Box, Microsoft veya kendi disklerinizi kullanarak içeri/dışarı aktarma tarafından sağlanan veri kutusu ağır cihazları seçin.

Fiziksel bir cihaz doğru seçenek olup olmadığını doğrulamak için aşağıdaki tabloyu kullanın. Ağ veri aktarımı için (% 90'ını kullanımı varsayılarak) çeşitli kullanılabilir bant genişlikleri için öngörülen saati gösterir. Çok yavaş ağ aktarımı öngörülen, fiziksel bir cihaz kullanmanız gerekir.  

![Ağ aktarım veya çevrimdışı aktarımı](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

## <a name="recommended-options"></a>Önerilen seçenekleri

Bu senaryoda kullanılabilir seçenekler, Azure Data Box çevrimdışı transfer ya da Azure içeri/dışarı aktarma cihazlara yöneliktir.

- **Azure Data Box ailesi çevrimdışı aktarımları** – zaman, ağ kullanılabilirliği veya maliyet sınırlama getirilir, büyük miktarda veriyi Azure'a taşımak için Microsoft tarafından sağlanan veri kutusu cihazlarından cihazları kullanın. Robocopy gibi araçları kullanarak şirket içi veri kopyalayın. Veri boyutu aktarım için hedeflenen bağlı olarak Data Box Disk, Data Box veya veri kutusu ağır seçebilirsiniz.
- **Azure içeri/dışarı aktarma** – büyük miktarda veriyi Azure Blob Depolama ve Azure dosyaları için güvenli bir şekilde içeri aktarmak için kendi disk sürücüleri sevkiyat tarafından kullanımı Azure içeri/dışarı aktarma hizmeti. Bu hizmet, verileri Azure Blob depolama alanından disk sürücülerine aktarmak ve şirket içi sitelerinize teslim etme için de kullanılabilir.

## <a name="comparison-of-key-capabilities"></a>Önemli özellikleri karşılaştırma

Aşağıdaki tabloda temel işlevleri farklılıkları özetlemektedir.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy              |    İçeri/Dışarı Aktarma                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    Veri boyutu                        |    35 TB'a kadar                 |    Cihaz başına en fazla 80 TB'a                       |    Cihaz başına en fazla 800 TB               |    Değişken                            |
|    Veri türü                        |    Azure BLOB'ları                  |    Azure BLOB'ları<br>Azure Dosyaları                    |    Azure BLOB'ları<br>Azure Dosyaları            |    Azure BLOB'ları<br>Azure Dosyaları          |
|    Form faktörü                      |    Sipariş başına 5 SSD             |    1 x 50-lb. Sipariş başına Masaüstü boyutlu cihaz    |    1 X ~ 500-lb. Sipariş başına büyük cihaz    |    Sipariş başına en fazla 10 HDD'ler/SSD        |
|    İlk kurulum süresi               |    Düşük <br>(15 dakika)            |    Orta için düşük <br> (< 30 dakika)               |    Orta<br>(1-2 saat)               |    Çok zor Orta<br>(değişken) |
|    Verileri Azure'a Gönder               |    Evet                          |    Evet                                           |    Evet                                   |    Evet                                 |
|    Azure'dan veri dışarı aktarma           |    Hayır                           |    Hayır                                            |    Hayır                                    |    Evet                                 |
|    Şifreleme                       |    AES 128 bit                  |    AES 256 bit                                   |    AES 256 bit                           |    AES 128 bit                         |
|    Donanım                         |     Microsoft tarafından sağlanan          |    Microsoft tarafından sağlanan                            |    Microsoft tarafından sağlanan                    |    Müşteri tarafından sağlanan                   |
|    Ağ arabirimi                |    USB 3.1/SATA                 |    RJ 45, SFP+                                   |    RJ45, QSFP+                           |    SATA II/SATA III                    |
|    İş ortağı tümleştirmesi              |    Belirli Kullanıcılar                         |    [Yüksek](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                          |    [Yüksek](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                  |    Belirli Kullanıcılar                                |
|    Sevkiyat                         |    Microsoft tarafından yönetilen            |    Microsoft tarafından yönetilen                             |    Microsoft tarafından yönetilen                     |    Müşteri tarafından yönetilen                    |
| Veri hareket ettiğinde kullanın         |Bir ticari sınır içinde|Bir ticari sınır içinde|Bir ticari sınır içinde|Coğrafi sınırlar arasında örneğin BİZE AB|
|    Fiyatlandırma                          |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


## <a name="next-steps"></a>Sonraki adımlar

- Anlamak için nasıl

    - [Data Box Disk ile veri aktarımı](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal).
    - [Data Box ile veri aktarımı](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal).
    - [İçeri/dışarı aktarma ile veri aktarımı](/azure/storage/common/storage-import-export-data-to-blobs).
