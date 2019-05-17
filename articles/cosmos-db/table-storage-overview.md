---
title: Azure tablo depolamaya genel bakış
description: Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: overview
ms.date: 11/03/2017
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: 0c15a7bc3295a26b004a32a8a464b41ac8c0a4b4
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787238"
---
# <a name="azure-table-storage-overview"></a>Azure Tablo depolamaya genel bakış

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Tablo Depolama, yapılandırılmış NoSQL verilerini bulutta depolayan ve şemasız bir tasarım ile anahtar/öznitelik deposu sağlayan bir hizmettir. Table Storage şemasız olduğu için uygulamanızın ihtiyaçları geliştikçe verilerinizi kolayca uyarlayabilirsiniz. Tablo Depolama verilerine erişim, çoğu uygulama türü için hızlı ve ekonomik olmanın yanı sıra benzer veri hacimleri için geleneksel SQL’e kıyasla genellikle daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri veya hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini Tablo Depolama ile depolayabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.

* [.NET SDK kullanarak Azure Cosmos DB tablo API'si ve Azure tablo depolama ile çalışmaya başlama](table-storage-how-to-use-dotnet.md)

* Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Tablo hizmeti başvuru belgelerini görüntüleyin:

    * [.NET başvurusu için Depolama İstemci Kitaplığı](https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [REST API başvurusu](https://msdn.microsoft.com/library/azure/dd179355)
