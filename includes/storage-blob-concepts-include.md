---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/18/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8d1f9ca5a964787f0ee68e6d99fe870cd7b5994b
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673558"
---
Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. BLOB Depolama, büyük miktarda yapılandırılmamış verileri depolamak için optimize edilmiştir. Yapılandırılmamış verileri belirli bir veri modeli veya tanım, metin veya ikili veri gibi kalmıyor verilerdir.

## <a name="about-blob-storage"></a>BLOB Depolama hakkında

BLOB Depolama için tasarlanmıştır:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Günlük dosyalarına yazma.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Kullanıcılar veya istemci uygulamaları HTTP/HTTPS üzerinden Blob depolamadaki nesnelere herhangi bir dünyada erişebilirsiniz. Blob depolamadaki nesnelere aracılığıyla erişilebilir [Azure Storage REST API'sini](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage), veya bir Azure depolama istemci kitaplığı. İstemci kitaplıkları vardır ve çeşitli gibi diller için kullanılabilir [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](https://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [gidin ](https://github.com/azure/azure-storage-blob-go/), [PHP](https://azure.github.io/azure-storage-php/), ve [Ruby](https://azure.github.io/azure-storage-ruby).

## <a name="about-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 hakkında

BLOB Depolama, Azure Data Lake depolama Gen2, bulut için Microsoft'un Kurumsal büyük veri analizi çözümü destekler. Azure Data Lake depolama Gen2 teklifler hiyerarşik bir sistem, hem de Blob Depolama, düşük maliyetli, katmanlı depolama da dahil olmak üzere avantajları dosya; yüksek kullanılabilirlik; güçlü tutarlılık; ve olağanüstü durum kurtarma özellikleri.

Data Lake depolama Gen2 hakkında daha fazla bilgi için bkz: [Azure Data Lake depolama Gen2'ye Giriş](../articles/storage/data-lake-storage/introduction.md).
