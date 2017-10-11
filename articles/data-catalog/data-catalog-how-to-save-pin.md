---
title: "Aramaları kaydetme ve Azure veri Kataloğu'nda veri varlıklarının Sabitle | Microsoft Docs"
description: "Nasıl yapılır makalesi veri kaynakları ve daha sonra kullanmak için veri varlıklarını kaydetme için Azure veri Kataloğu özellikleri vurgulama."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 8c319d0dcbe8b95af11b8be2368a9348b260446c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Azure veri Kataloğu'nda arama ve PIN veri varlıklarını kaydetme
## <a name="introduction"></a>Giriş
Azure veri Kataloğu, veri kaynağı bulma için yetenekleri sağlar. Hızlı bir şekilde arayın ve veri kaynaklarını bulmak ve bunların amacı anlamak için katalog daha kolay elinizdeki iş için doğru verileri bulmak filtre.

Ancak düzenli olarak aynı verilerle çalışmak ne gerekir? Ve ne sizin ve diğer kullanıcıların düzenli olarak bilginiz kataloğunda aynı veri kaynaklarına katkıda? Bu durumlarda, art arda aynı aramaları vermek verimsiz olabilir. Kayıtlı arama ve sabitlenmiş veri varlıkları olduğu yardımcı olabilir budur.

## <a name="saved-searches"></a>Kaydedilen aramalar
Veri Kataloğu'nda kaydedilmiş bir aramayı bir yeniden kullanılabilir, kullanıcı başına arama tanımıdır. Arama terimleri, etiketler ve diğer filtreleri gibi bir arama tanımlamak ve kaydedin. Daha sonra kendi arama ölçütleriyle eşleşen hiçbir veri varlıklarını döndürmek için kayıtlı arama tanımı yeniden çalıştırabilirsiniz.

### <a name="create-a-saved-search"></a>Kaydedilmiş bir aramayı oluşturma
Kaydedilmiş bir aramayı oluşturmak için aşağıdakileri yapın:
1. Azure veri Kataloğu portalında içinde **geçerli aramayı** penceresinde tıklatın **kaydetmek**. 

    ![Geçerli arama ayarları Kaydet bağlantısı](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Yeniden kullanmak ve ardından istediğiniz arama ölçütlerini girin **kaydetmek**.

    ![Kaydedilen arama adı geçerli arama ayarları](./media/data-catalog-how-to-save-pin/02-name.png)

3. İstendiğinde, kayıtlı arama için bir ad girin. Anlamlı bir ad seçin ve araması tarafından döndürülen veri varlıklarını açıklar.

### <a name="manage-saved-searches"></a>Kaydedilmiş aramaları Yönet
Bir veya daha fazla aramalar, kaydettiğiniz sonra bir **kayıtlı aramaları** seçeneği altında görüntülenir **geçerli aramayı** kutusu. Liste genişletildiğinde tüm kaydedilmiş aramaları görüntülenir.

 ![Kaydedilmiş aramaları listesi](./media/data-catalog-how-to-save-pin/03-list.png)

Aşağıdakilerden birini yapın:

* Bir arama yürütmek için kaydedilmiş bir aramayı listeden seçin.

* Kayıtlı bir aramaya Yönetimi seçeneklerinin bir listesini görüntülemek için arama adının yanındaki aşağı oka tıklayın.

    ![Kaydedilmiş aramaları yönetmek için Seçenekler](./media/data-catalog-how-to-save-pin/04-managing.png)

* Kayıtlı arama için yeni bir ad girmeyi seçin **yeniden adlandırma**. Arama tanımı değiştirilmez.

* Kayıtlı arama listenizden kaldırmayı seçin **silmek**ve silme işlemini onaylayın.

* Kayıtlı arama varsayılan aramanızı olarak işaretlemek için seçin **varsayılan olarak Kaydet**. Azure veri Kataloğu giriş sayfasından bir "boş" araması gerçekleştirirseniz, varsayılan aramanızı yürütülür. Varsayılan arama olarak işaretlenmiş arama en üstünde ek olarak, görüntülenen **kayıtlı aramaları** listesi.

### <a name="organizational-saved-searches"></a>Kuruluş Kaydedilmiş aramaları
Kuruluşunuzdaki tüm kullanıcı kendi kullanmak için aramaları kaydedebilirsiniz. Veri Kataloğu yöneticileri kuruluş içindeki tüm kullanıcılar için aramaları da kaydedebilirsiniz. Yöneticiler bir aramayı kaydettiğinizde, bunlar ile sunulan bir **paylaşımı şirket içinde** seçeneği. Bu seçeneğin belirlenmesi, kuruluşunuzdaki tüm kullanıcılar için kayıtlı arama paylaşır.

 ![Kuruluş Kaydedilmiş aramaları](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Sabitlenmiş veri varlıklarını
Kaydedilmiş aramaları ile kaydedebilir ve arama tanımları yeniden kullanabilirsiniz. Aramaları tarafından döndürülen veri varlıklarını katalog değişiklik içeriğini olarak zaman içinde değişebilir. Veri varlıklarını PIN zaman bir arama kullanmaya gerek kalmadan daha kolay erişim sağlamak için belirli veri varlıklarını açıkça tanımlayabilirsiniz.

Bir veri varlığına sabitleme basittir. Veri varlığına sabitlenmiş listenize eklemek için tıklayın **PIN** simgesi. Simge varlık döşemenin döşeme görünümünü ve Azure veri Kataloğu portalını liste görünümünde en sol sütununda köşesinde görüntülenir.

![Veri varlığına PIN simgesi](./media/data-catalog-how-to-save-pin/05-pinning.png)

Bir veri varlığına kaldırıldığında eşit olarak açıktır. Tıklamanız yeterlidir **sabitleme** simgesi seçili varlık ayarını değiştirin.

![Veri varlığına sabitleme simgesi](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a>My varlıklar bölümü
Veri Kataloğu portalı giriş sayfasına içeren bir **My varlıklar** ilgi varlıklar geçerli kullanıcı için görüntüler bölümü. Bu bölümde her iki sabitlenmiş varlıkları içerir ve kayıtlı aramalar.

![Giriş sayfasında My varlıklar bölümü](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Özet
Azure veri Kataloğu sizin ve diğer kuruluş üyelerinin verileri ve daha fazla süre ile çalışmak için arayan daha az zaman harcayabilir için gereksinim duyduğunuz, veri kaynaklarını bulmasına kolaylaştırmak yetenekleri sağlar. Kayıtlı aramalar ve kullanıcılar ile art arda çalıştıkları veri kaynaklarını kolayca tanıyacak şekilde varlıklar bu çekirdek özellikler yapı veri sabitlenir.
