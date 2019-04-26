---
title: Veri varlıkları | Microsoft Docs
description: Veri varlıkları genel bakış.
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
ms.openlocfilehash: 579856ab87aaf8d051f2e3c161bb2d0e2f693ed5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60397303"
---
# <a name="data-entities"></a>Veri varlıkları

Bu makalede, tanımlar ve veri varlıkları için genel bir bakış sağlar. Veri varlıkları yeteneklerini, destekledikleri senaryolar, bunları ve bunları oluşturmak için yöntemleri için kullanılan kategorileri hakkında bilgiler içerir.

## <a name="overview"></a>Genel Bakış

Bir veri varlığı veritabanı tablolarının fiziksel uygulamasından bir soyutlamadır. Örneğin, normalleştirilmiş tablolar, bir müşteri tablodaki her müşteriye ait verilerin çoğunu depolanabilir ve sonra geri kalan küçük bir ilişkili tablolar arasında yayılıyor olabilir. Bu durumda, veri varlığı için müşteri kavramı her satır müşteri tablosu ve onun ilişkili tablolar tüm verileri içeren normalleştirilmişlikten çıkarılmış bir görünüm olarak görünür. Bir veri varlığı, geliştirme ve tümleştirme daha kolay anlaşılır bir biçime iş kavramını kapsüller. Bir veri varlığı bulunabilen yapısını, uygulama geliştirme ve özelleştirme basitleştirebilir. Daha sonra Özet ayrıca fiziksel tabloların sürümleri arasında kaçınılmaz karmaşıklığı uygulama kodundan korunmasını sağlar.

Özetlersek: Veri varlığı kavramsal soyutlama ve anahtar veri kavramlarını ve İşlevler temsil etmek için temel alınan tablo şemalarını Kapsüllemesi (normalleştirilmişlikten çıkarılmış görünümü) sağlar.

## <a name="capabilities"></a>Özellikler

Bir veri varlığı aşağıdaki özellikleri içerir:

- AXD, verileri içeri/dışarı aktarma Framework (DIXF) varlıklar, ayrılan ve parçalanmış kavramlarını değiştirir ve tek kavramına toplama sorgular.
- Bu, iş mantığı yakalamak ve içeri/dışarı aktarma, tümleştirme ve programlama gibi senaryoları etkinleştirmek için tek bir yığın sağlar.
- Dışarı aktarma ve uygulama yaşam döngüsü yönetimi (ALM) ve tanıtım veri senaryoları için veri paketleri içeri aktarma için birincil mekanizma olur.
- OData hizmet olarak kullanıma sunulan ve olması ardından tablo stili zaman uyumlu tümleştirme senaryolar ve Microsoft Office içinde kullanılan.

Bkz: [veri varlıkları](https://docs.microsoft.com/dynamics365/operations/dev-itpro/data-entities/data-entities) daha fazla bilgi için.
