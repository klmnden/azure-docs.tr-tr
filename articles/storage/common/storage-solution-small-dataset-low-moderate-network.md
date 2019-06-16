---
title: Orta ağ bant genişliği için Azure veri aktarımı düşük ile küçük veri kümeleri için Seçenekler | Microsoft Docs
description: Ortamınızda Orta ağ bant genişliği düşük olması ve küçük veri kümeleri aktarmayı planlıyorsanız, veri aktarımı için bir Azure çözümü belirleyin öğrenin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 12/05/2018
ms.author: alkohli
ms.openlocfilehash: 3e6f4f3eb312f0d4d96a008c0944a9608d0bf4a7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60397286"
---
# <a name="data-transfer-for-small-datasets-with-low-to-moderate-network-bandwidth"></a>Orta ağ bant genişliği düşük ile küçük veri kümeleri için veri aktarımı
 
Bu makalede, ortamınızda Orta ağ bant genişliği düşük olduğunda ve küçük veri kümeleri aktarmak planlama çözümleri veri aktarımı için genel bir bakış sağlar. Makalede de önerilen veri aktarımı seçenekleri ve bu senaryo için ilgili önemli özellik matrisi açıklanır.

Kullanılabilir veri aktarımı seçeneklerini tümü bir genel bakış anlamak için Git [bir Azure veri aktarım çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="scenario-description"></a>Senaryo açıklaması

Küçük veri kümeleri için birkaç TB'a GB sırasına göre veri boyutları bakın. Orta ağ bant genişliği düşük 45 MB/sn (veri merkezinde T3 bağlantı) ile 1 GB/sn anlamına gelir.

- Yalnızca birkaç dosyalarının aktardığınız ve veri aktarımı otomatik hale getirmek gerekmez, grafik arabirimi araçları göz önünde bulundurun.
- Sistem Yönetimi ile rahatça kullanabiliyorsanız, komut satırı veya program/komut dosyası oluşturma araçları göz önünde bulundurun.

## <a name="recommended-options"></a>Önerilen seçenekleri

Bu senaryoda, önerilen seçenek vardır:

- **Grafik arabirimi Araçları** Azure Depolama Gezgini ve Azure portalında Azure depolama gibi. Bunlar, verilerinizi görüntüleyin ve hızlı bir şekilde birkaç dosya aktarmak için kolay bir yol sağlar.

    - **Azure Depolama Gezgini** -bu platformlar arası aracı, Azure depolama hesaplarınızın içeriğini yönetmenize olanak tanır. Yükleme, indirme ve bloblar, dosyalar, kuyruklar, tablolar ve Azure Cosmos DB varlıklarını yönetmenize olanak tanır. Blob Depolama ile blobları ve klasörleri yönetmek yanı sıra karşıya yüklemek ve blobları depolama hesapları arasında veya yerel dosya sistemi ve Blob Depolama arasında indirmek için kullanın.
    - **Azure portalında** - Azure portalında Azure depolama dosyaları keşfetmek için web tabanlı bir arabirim sağlar ve karşıya yeni dosya teker teker. Komut dosyalarınızı hızla Keşfetmenin veya yenilerini birkaç yüklenecek ya da herhangi bir aracı yüklemek istemiyorsanız bu iyi bir seçenektir.

- **Betik/programlı Araçları** AzCopy/PowerShell/Azure CLI ve Azure depolama REST API'leri gibi.

    - **AzCopy** - kolayca ve dosyaları, Azure bloblarından veri kopyalamak için bu komut satırı aracını kullanın ve tablo depolama ile en iyi performans. AzCopy, eşzamanlılık ve paralellik ve kopyalama işlemleri kesintiye olduğunda sürdürebilme destekler.
    - **Azure PowerShell** - sistem yönetimi ile deneyimli kullanıcılar için Azure Storage modülünde Azure PowerShell'de veri aktarımı.
    - **Azure CLI** -Azure hizmetlerini yönetmek ve Azure Depolama'ya veri yüklemek için bu platformlar arası aracı kullanın.
    - **Azure depolama REST API'ler/SDK'lar** – bir uygulama oluştururken, Azure depolama REST API'ler/SDK'lar karşı uygulama geliştirmek ve birden çok dilde sunulan Azure istemci kitaplıkları kullanın.


## <a name="comparison-of-key-capabilities"></a>Önemli özellikleri karşılaştırma

Aşağıdaki tabloda temel işlevleri farklılıkları özetlemektedir.

| Özellik | Azure Depolama Gezgini | Azure portal | AzCopy<br>Azure PowerShell<br>Azure CLI | Azure depolama REST API veya SDK'ları |
|---------|------------------------|--------------|-----------------------------------------|---------------------------------|
| Kullanılabilirlik | İndirme ve yükleme <br>Tek başına aracı | Azure portalında Web tabanlı araştırma araçları | Komut satırı aracı |.NET, Java, Python, JavaScript, C++, Go, Ruby ve PHP'ye programlanabilir arabirimleri |
| Grafik arabirimi | Evet | Evet | Hayır | Hayır |
| Desteklenen platformlar | Windows, Mac, Linux | Web tabanlı |Windows, Mac, Linux |Tüm platformlar |
| BLOB Depolama işlemlerine izin<br>BLOB'ları ve klasörleri | Karşıya Yükle<br>İndirme<br>Yönetme | Karşıya Yükle<br>İndirme<br>Yönetme |Karşıya Yükle<br>İndirme<br>Yönetme | Evet, özelleştirilebilir |
| İzin verilen Data Lake Gen1 depolama<br>dosyalar ve klasörler için işlemler | Karşıya Yükle<br>İndirme<br>Yönetme | Hayır |Karşıya Yükle<br>İndirme<br>Yönetme                   | Hayır |
| Dosya depolama işlemlerine izin<br>dosyalar ve dizinler için | Karşıya Yükle<br>İndirme<br>Yönetme | Karşıya Yükle<br>İndirme<br>Yönetme   |Karşıya Yükle<br>İndirme<br>Yönetme | Evet, özelleştirilebilir |
| Tablo depolama işlemlerine izin<br>tablolar için |Yönetme | Hayır |AzCopy v7 tablo desteği |Evet, özelleştirilebilir|
| İzin verilen kuyruk depolama | Yönetme | Hayır  |Hayır | Evet, özelleştirilebilir.|


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Azure Depolama Gezgini ile veri aktarma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-azure-storage-explorer).
- [AzCopy ile verileri aktarma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

