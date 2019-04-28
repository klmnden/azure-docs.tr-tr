---
title: Azure veri Kataloğu'nda arama ve PIN veri varlıklarını kaydetme
description: Nasıl yapılır makalesi veri kaynakları ve daha sonra kullanmak için veri varlıklarını kaydetme, Azure veri Kataloğu'teki vurgulama.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: df3220ddb80ebc329ed6b0024ca4eefd2bdfb321
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61002096"
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Azure veri Kataloğu'nda arama ve PIN veri varlıklarını kaydetme
## <a name="introduction"></a>Giriş
Azure veri Kataloğu, veri kaynağı bulma için özellikleri sağlar. Hızlı bir şekilde, aramak ve eldeki iş için doğru verileri bulmayı kolaylaştıran Kataloğu veri kaynakları bulmak ve kullanım amaçları anlamak için filtreleyin.

Ancak, düzenli olarak aynı verilerle çalışmak ihtiyacım olursa ne? Ve ne sizin ve diğer kullanıcıların düzenli olarak, aynı veri kaynaklarına kataloğunda katkıda? Bu durumda, aynı aramaları tekrar tekrar vermek zorunda kalmadan verimsiz olabilir. Bu, burada kayıtlı arama ve sabitlenmiş veri varlıklarını yardımcı olur.

## <a name="saved-searches"></a>Kayıtlı aramalar
Veri Kataloğu'nda kayıtlı bir aramayı bir yeniden kullanılabilir, kullanıcı başına arama tanımıdır. Arama terimleri, etiketler ve diğer filtreler dahil olmak üzere, bir arama tanımlayabilir ve kaydedin. Daha sonra kendi arama ölçütleriyle eşleşen herhangi bir veri varlıklarını döndürmek için kayıtlı arama tanımı yeniden çalıştırabilirsiniz.

### <a name="create-a-saved-search"></a>Kaydedilmiş arama oluştur
Kayıtlı bir aramayı oluşturmak için aşağıdakileri yapın:
1. Azure veri Kataloğu portalında içinde **geçerli aramayı** penceresinde tıklayın **Kaydet**. 

    ![Geçerli arama ayarları kaydetme bağlantısı](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Yeniden kullanmak ve ardından istediğiniz arama ölçütlerini girin **Kaydet**.

    ![Kayıtlı arama adı geçerli arama ayarları](./media/data-catalog-how-to-save-pin/02-name.png)

3. İstendiğinde, kayıtlı arama için bir ad girin. Anlamlı bir ad seçin ve bu arama sonucunda döndürülen veri varlıkları açıklar.

### <a name="manage-saved-searches"></a>Kaydedilmiş aramaları Yönet
Bir veya daha fazla arama kaydettikten sonra bir **kayıtlı aramalar** seçenek altında görüntülenen **geçerli aramayı** kutusu. Tüm kayıtlı aramalar, listenin genişletildiğinde görüntülenir.

 ![Kayıtlı aramalar listesi](./media/data-catalog-how-to-save-pin/03-list.png)

Aşağıdakilerden birini yapın:

* Bir arama yürütmek için kayıtlı bir aramayı listeden seçin.

* Yönetim seçenekleri için kayıtlı bir aramayı bir listesini görüntülemek için arama adının yanındaki aşağı oka tıklayın.

    ![Kayıtlı aramalar yönetimi seçenekleri](./media/data-catalog-how-to-save-pin/04-managing.png)

* Kayıtlı arama için yeni bir ad girmeyi seçin **Yeniden Adlandır**. Arama tanımı değiştirilmez.

* Kayıtlı arama listenizden kaldırmak için seçin **Sil**ve ardından silme işlemini onaylayın.

* Kayıtlı arama varsayılan arama olarak işaretlemek için seçin **varsayılan olarak Kaydet**. Azure veri Kataloğu giriş sayfasından bir "boş" araması yaptığınızda, varsayılan arama yürütülür. Ayrıca, varsayılan arama işaretlenmiş arama üst kısmında görüntülenir **kayıtlı aramalar** listesi.

### <a name="organizational-saved-searches"></a>Kuruluş kayıtlı aramalar
Kuruluşunuzdaki tüm kullanıcı aramaları, kendi kullanmak için kaydedebilirsiniz. Veri Kataloğu yöneticilerinin kuruluştaki tüm kullanıcılar için aramaları da kaydedebilirsiniz. Yöneticiler, bir arama kaydettiğinizde, bunlar eklemediğiniz bir **şirket içinde paylaşımı** seçeneği. Bu seçeneğin belirlenmesi, kuruluştaki tüm kullanıcılar için kayıtlı arama paylaşır.

 ![Kuruluş kayıtlı aramalar](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Sabitlenmiş veri varlıkları
Kayıtlı aramalar ile kaydedin ve yeniden arama tanımları. Arama tarafından döndürülen veri varlıklarını Kataloğu değişiklik içeriğini zaman içinde değişebilir. Veri varlıklarını sabitleme, bir arama kullanmak zorunda kalmadan erişim daha kolay yapmak için belirli veri varlıklarını açıkça ayırt edebilirsiniz.

Bir veri varlığına sabitleme oldukça basittir. Veri varlığı sabitlenmiş listenize eklemek için tıklamanız yeterlidir **PIN** simgesi. Simge varlık kutucuk, kutucuk görünümü ve Azure veri Kataloğu portalında liste görünümünde en soldaki sütunda köşesinde görüntülenir.

![Veri varlığı Raptiye simgesi](./media/data-catalog-how-to-save-pin/05-pinning.png)

Bir veri varlığına unpinning eşit oldukça basittir. Tıklamanız yeterlidir **sabitleme** simgesi seçili varlık için ayarı değiştirmek için.

![Veri varlığı Kaldır simgesi](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a>My varlıklar bölüm
Veri Kataloğu portalı giriş sayfasına içeren bir **My varlıklar** geçerli kullanıcıya ilgi varlıklarını görüntüler bölümü. Bu bölümde, her iki Sabitlenen varlıklar içerir ve aramalar.

![Giriş sayfasında My varlıklar bölüm](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Özet
Azure veri Kataloğu, siz ve diğer kuruluş üyeleri için verileri ve bununla çalışmaya daha fazla zaman aramaya zaman ayırmasına ihtiyacınız veri kaynaklarını bulmak daha kolay hale getirmek özellikleri sağlar. Kayıtlı aramalar ve kullanıcılar ile sürekli çalıştıkları veri kaynaklarını kolayca belirleyebilmeniz varlıklar bu temel özelliğe derleme verilerinin sabitlenebilir.
