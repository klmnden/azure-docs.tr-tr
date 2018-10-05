---
title: Veri varlıkları | Microsoft Docs
description: Veri varlıklarına genel bakış.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: c7b321ab04df405c56cab0952942b0d6e142da6d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811476"
---
# <a name="data-entities"></a>Veri varlıkları

Bu makalede, tanımlar ve veri varlıkları için genel bir bakış sağlar. Veri varlıkları yeteneklerini, destekledikleri senaryolar, bunları ve bunları oluşturmak için yöntemleri için kullanılan kategorileri hakkında bilgiler içerir.

## <a name="overview"></a>Genel Bakış

Bir veri varlığı veritabanı tablolarının fiziksel uygulamasından bir soyutlamadır. Örneğin, normalleştirilmiş tablolar, bir müşteri tablodaki her müşteriye ait verilerin çoğunu depolanabilir ve sonra geri kalan küçük bir ilişkili tablolar arasında yayılıyor olabilir. Bu durumda, veri varlığı için müşteri kavramı her satır müşteri tablosu ve onun ilişkili tablolar tüm verileri içeren normalleştirilmişlikten çıkarılmış bir görünüm olarak görünür. Bir veri varlığı, geliştirme ve tümleştirme daha kolay anlaşılır bir biçime iş kavramını kapsüller. Bir veri varlığı bulunabilen yapısını, uygulama geliştirme ve özelleştirme basitleştirebilir. Daha sonra Özet ayrıca fiziksel tabloların sürümleri arasında kaçınılmaz karmaşıklığı uygulama kodundan korunmasını sağlar.

Özetlersek: veri varlığı kavramsal soyutlama ve anahtar veri kavramlarını ve İşlevler temsil etmek için temel alınan tablo şemalarını Kapsüllemesi (normalleştirilmişlikten çıkarılmış görünümü) sağlar.

## <a name="capabilities"></a>Özellikler

Bir veri varlığı aşağıdaki özellikleri içerir:

- AXD, verileri içeri/dışarı aktarma Framework (DIXF) varlıklar, ayrılan ve parçalanmış kavramlarını değiştirir ve tek kavramına toplama sorgular.
- Bu, iş mantığı yakalamak ve içeri/dışarı aktarma, tümleştirme ve programlama gibi senaryoları etkinleştirmek için tek bir yığın sağlar.
- Dışarı aktarma ve uygulama yaşam döngüsü yönetimi (ALM) ve tanıtım veri senaryoları için veri paketleri içeri aktarma için birincil mekanizma olur.
- OData hizmet olarak kullanıma sunulan ve olması ardından tablo stili zaman uyumlu tümleştirme senaryolar ve Microsoft Office içinde kullanılan.

Bkz: [veri varlıkları](https://docs.microsoft.com/dynamics365/operations/dev-itpro/data-entities/data-entities) daha fazla bilgi için.
