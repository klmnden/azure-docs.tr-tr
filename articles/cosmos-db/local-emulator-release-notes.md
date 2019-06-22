---
title: Azure Cosmos öykünücüsü'nü indirme ve sürüm notları
description: Azure Cosmos öykünücüsü'nü sürüm notlarını okuyun ve indirin.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 06/20/2019
ms.openlocfilehash: 5985d0d82341c76993ee91b8dff6927edd1ed8b4
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332123"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve test için Azure Cosmos öykünücüsünü kullanma

Bu makalede Azure Cosmos öykünücü sürüm notları her sürümde yapılan özellik güncelleştirmeleri listesini gösterir. Ayrıca, en son sürümü karşıdan yüklemek ve kullanmak için öykünücü listelenir.

## <a name="download"></a>İndirme

| | |
|---------|---------|
|**MSI yükleme**|[Microsoft İndirme Merkezi](https://aka.ms/cosmosdb-emulator)|
|**Kullanmaya başlama**|[Azure Cosmos öykünücü ile yerel olarak geliştirme](local-emulator.md)|

## <a name="release-notes"></a>Sürüm notları

### <a name="243"></a>2.4.3

- Devre dışı varsayılan olarak MongoDB hizmeti başlatılıyor. Yalnızca SQL uç noktası, varsayılan etkindir. Kullanıcı öykünücü'nın el ile kullanarak uç nokta başlamalıdır "/ EnableMongoDbEndpoint" komut satırı seçeneği. Şimdi, tüm diğer hizmet uç noktaları, Gremlin, Cassandra ve tablo gibi gibidir.
- Öykünücüde, ile başlatılırken bir hata düzeltildi "/ AllowNetworkAccess" nerede Gremlin, Cassandra ve tablo uç noktaları olmayan düzgün bir şekilde işleme dış istemcilerden gelen istekleri.
- Doğrudan bağlantı noktaları için güvenlik duvarı kuralları ayarları ekleyin.

### <a name="240"></a>2.4.0

- Pulse istemcisi gibi ağ izleme uygulamaları ana bilgisayarda mevcut olduğunda başlatılamamasına öykünücü ile bir sorun düzeltildi.
