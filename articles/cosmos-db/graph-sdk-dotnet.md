---
title: "Azure CosmosDB grafik API'si .NET SDK'sını & kaynakları | Microsoft Docs"
description: "Yayın tarih, sona erme tarihlerini ve her bir sürümü arasında yapılan değişiklikler dahil olmak üzere Azure CosmosDB grafik API'si hakkında bilgi alın."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/17/2017
ms.author: mimig
ms.openlocfilehash: 7d6ba5794e4a3e431abd72a780b60b9e59e9f4db
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="azure-cosmos-db-graph-net-api-download-and-release-notes"></a>Azure Cosmos DB grafik .NET API: İndirme ve sürüm notları

|   |   |
|---|---|
|**SDK yükleme**|[NuGet](https://aka.ms/acdbgraphnuget)|
|**API belgeleri**|[.NET API başvuru belgeleri](https://aka.ms/acdbgraphapiref)|
|**Hızlı Başlangıç**|[Azure Cosmos DB: .NET ve grafik API'sini kullanarak bir grafik uygulaması oluşturma](create-graph-dotnet.md)|
|**Öğretici**|[Azure CosmosDB: grafik API'si ile bir kapsayıcı oluşturma](tutorial-develop-graph-dotnet.md)|
|**Geçerli desteklenen çerçevelerden**| [Microsoft .NET Framework 4.6.1](https://www.microsoft.com/en-us/download/details.aspx?id=49981)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |


## <a name="release-notes"></a>Sürüm notları

### <a name="a-name031-preview031-preview"></a><a name="0.3.1-preview"/>0.3.1-Preview

#### <a name="bug-fixes"></a>Hata düzeltmeleri
* İsteğe bağlı olarak yüklemek düzeltme `appsettings.json` (`netstandard1.6`)

#### <a name="whats-new"></a>Yenilikler
* Microsoft.Azure.Graphs hedef platformu AnyCPU geçin.
* Mono bütünleştirilmiş koddan kaldırabilir `net461` paket bildirimi.

### <a name="a-name030-preview030-preview"></a><a name="0.3.0-preview"/>0.3.0-Preview

#### <a name="whats-new"></a>Yenilikler
* Desteği eklendi`.netstandard 1.6`
  * Gerektirir`Microsoft.Azure.DocumentDB.Core >= 1.5.1`
* Yeni eklenen `gremlin-groovy` varolan ayrıştırıcı değiştirmek için ayrıştırıcı. Bir alt kümesini Tinkerpop'ın bu ayrıştırıcı destekler `gremlin-groovy` sözdizimi ve içerir:
  * 2 x ayrıştırma performansı geliştirildi.
  * Dizeleri, yanlış işlenmesi değişmez değerler ve diğer eski ayrıştırıcı sıradışı kaçış karakteri sorunlarını bir dizi ilgili çözümlendi.
* Çapraz geçişlerine kenar koşulları ile iyileştirmelerini eklendi.
  *  Filtreler çapraz geçiş durağı görmeniz gerekir bu geliştirme, örneğin: `g.V('1').outE().has('name', 'marko').inV()`.
* Eklenen çapraz geçişlerine ile iyileştirmelerini `limit()` adım.

#### <a name="breaking-changes"></a>Yeni değişiklikler
* .NET Framework 4.5.1 için kaldırılan destek

* İle yeni ayrıştırıcı hizalar `gremlin-groovy` dilbilgisi. Sonuç olarak, daha önce çalışan bazı ifadeler için yeni ayrıştırıcı belirsiz. Bir durum Not:
  * `in`ve `as` ayrılmış sözcükler `gremlin-groovy`, şu adımları ile nitelenmelidir `.in()` veya `.as()` sözdizimi hataları önlemek için. Örneğin: `g.V().repeat(in()).times(2)`  ->  _bir sözdizimi hatası oluşturur_  
 `g.V().repeat(__.in()).times(2)` -> _başarılı_

### <a name="a-name024-preview024-preview"></a><a name="0.2.4-preview"/>0.2.4-Preview

### <a name="a-name022-preview022-preview"></a><a name="0.2.2-preview"/>0.2.2-Preview

### <a name="a-name021-preview021-preview"></a><a name="0.2.1-preview"/>0.2.1-Preview

### <a name="a-name020-preview020-preview"></a><a name="0.2.0-preview"/>0.2.0-Preview

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview
* İlk önizleme sürümü.

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihleri
Microsoft sağlayacaktır bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

Yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sı yalnızca eklenir, bu nedenle, her zaman en son SDK sürüme erken mümkün olduğunca yükseltmeniz önerilir. 

Hizmet tarafından devre dışı bırakılan bir SDK kullanarak Azure Cosmos DB'de herhangi bir istek reddedilir.

<br/>

| Sürüm | Sürüm tarihi | Sona erme tarihi |
| --- | --- | --- |
| [0.3.1-Preview](#0.3.1-preview) |17 Ekim 2017 |--- |
| [0.3.0-Preview](#0.3.0-preview) |2 Ekim 2017 |--- |
| [0.2.4-Preview](#0.2.4-preview) |4 Ağustos 2017 |--- |
| [0.2.2-Preview](#0.2.2-preview) |23 Haziran 2017 |--- |
| [0.2.1-Preview](#0.2.2-preview) |8 Haziran 2017 |--- |
| [0.2.0-Preview](#0.2.2-preview) |10 Mayıs 2017 |--- |
| [0.1.0-Preview](#0.1.0-preview) |8 Mayıs 2017 |--- |

## <a name="see-also"></a>Ayrıca bkz.
Azure Cosmos DB grafik API'si hakkında daha fazla bilgi için bkz: [Azure Cosmos DB giriş: grafik API'si](graph-introduction.md). 
