---
title: Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı denetlemek | Microsoft Docs
description: Kuralları önbelleğe alma CDN ayarlamak veya hem genel hem de URL yolu ve dosya uzantılarını gibi koşullarla varsayılan önbellek süre sonu davranışını değiştirmek için kullanabilirsiniz.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim
ms.openlocfilehash: 4095ed763de378a673908d033d87b2aa6d72f13c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260015"
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>Denetim Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca kullanılabilir **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri. İçin **verizon'dan Azure CDN Premium** profilleri kullanmalıdır [Azure CDN kurallar altyapısı](cdn-rules-engine.md) içinde **Yönet** benzer işlevselliği için portal.
 
Azure içerik teslim ağı (CDN) dosyalarınızı nasıl önbelleğe denetlemek için iki yol sunar: 

- Kuralları önbelleğe alma: Bu makalede ayarlamak veya hem genel hem de bir URL yolu ve dosya uzantısı gibi özel koşullarla varsayılan önbellek süre sonu davranışını değiştirmek için kuralları önbelleğe alma içerik teslim ağı (CDN) nasıl kullanabileceğinizi açıklar. Azure CDN iki tür önbelleğe alma kuralı sağlar:

   - Genel önbelleğe alma kuralları: Profilinizdeki her uç nokta için bir genel önbelleğe alma kuralı ayarlayabilirsiniz. Bu işlem uç noktaya yönelik tüm istekleri etkiler. Genel önbelleğe alma kuralı ayarlandığında tüm HTTP önbellek yönergesi üst bilgilerini geçersiz kılar.

   - Özel önbelleğe alma kuralları: Profilinizdeki her uç nokta için bir veya daha fazla özel önbelleğe alma kuralı ayarlayabilirsiniz. Özel önbelleğe alma kuralları ayarlandığında belirli yollar ve dosya uzantılarıyla eşleşir, sırasıyla işlenir ve genel önbelleğe alma kuralını geçersiz kılar. 

- Sorgu dizesini önbelleğe alma: sorgu dizeleri içeren istekleri önbelleğe alma için Azure CDN nasıl işler ayarlayabilirsiniz. Bilgi için bkz: [denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle](cdn-query-string.md). Dosya alınabilir değilse ayarını önbelleği sorgu dizesi dayalı kurallar ve CDN varsayılan davranışlar önbelleğe alma üzerinde etkisi olmaz.

Varsayılan önbelleğe alma davranışını etkinleştirmek ve yönerge üstbilgileri önbelleğe alma hakkında daha fazla bilgi için bkz: [önbelleğe alma nasıl çalışır](cdn-how-caching-works.md). 


## <a name="accessing-azure-cdn-caching-rules"></a>Azure CDN önbelleğe alma kurallarını erişme

1. Azure Portalı'nı açın, bir CDN profili seçin ve sonra bir uç nokta seçin.

2. Ayarların altındaki sol bölmede **Önbelleğe alma kuralları**’nı seçin.

   ![CDN Önbelleğe alma kuralları düğmesi](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   **Önbelleğe alma kuralları** sayfası görüntülenir.

   ![CDN Önbelleğe alma kuralları sayfası](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>Önbelleğe alma davranışını ayarları
Genel ve özel önbelleğe alma kuralları için aşağıdaki belirtebilirsiniz **önbelleğe alma davranışı** ayarları:

- **Atlama önbellek**: önbelleğe kaydedilmez ve kaynak tarafından sağlanan önbellek yönergesi üstbilgileri yoksay.

- **Geçersiz kılma**: sağlanan kaynak önbellek yönergesi üstbilgileri yoksay; sağlanan önbellek süresi kullanın.

- **Eksik ayarlamanız**: Uy sağlanan kaynak önbellek yönergesi üst bilgiler, varsa; Aksi takdirde kullanın sağlanan önbellek süresi.

![Genel önbelleğe alma kuralları](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![Özel önbelleğe alma kuralları](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>Önbellek sona erme süresi
Genel ve özel önbelleğe alma kuralları için gün, saat, dakika ve saniyeleri önbellek sona erme süresi belirtebilirsiniz:

- İçin **geçersiz kılma** ve **eksikse ayarlamak** **önbelleğe alma davranışı** ayarları, geçerli önbellek süreleri aralık 0 saniye ile 366 gün arasında. 0 saniye değerini, CDN içeriği önbelleğe alır, ancak her istek kaynak sunucu ile düzeltin gerekir.

- İçin **atlama önbellek** ayarı önbellek süresi 0 saniye olarak otomatik olarak ayarlanır ve değiştirilemez.

## <a name="custom-caching-rules-match-conditions"></a>Eşleşme koşullar kuralları özel önbelleğe alma

Özel önbellek kuralları için iki eşleme koşulları kullanılabilir:
 
- **Yol**: Bu koşul hariç, etki alanı adı, URL yolunun eşleşmesi ve joker karakter sembolünü destekler (\*). Örneğin, _/myfile.html_, _/my/klasör / *_, ve _/my/images/*.jpg_. Uzunluk 260 karakter olabilir.

- **Uzantı**: Bu durum istenen dosyanın dosya uzantısı ile eşleşir. Eşleştirilecek virgülle ayrılmış dosya uzantıları listesini sağlar. Örneğin, _.jpg_, _.mp3_, veya _.png_. Uzantıları en fazla 50'dir ve uzantı başına karakter sayısı 16'dır. 

## <a name="global-and-custom-rule-processing-order"></a>Genel ve özel kural işleme sırası
Genel ve özel önbelleğe alma kuralları aşağıdaki sırayla işlenir:

- Genel önbelleğe alma kurallar varsayılan CDN önbelleğe alma davranışını (HTTP önbellek yönergesi üstbilgi ayarları) önceliklidir. 

- Özel önbelleğe alma kuralları genel önbelleğe alma kuralları, bunlar burada geçerli önceliklidir. Özel önbelleğe alma kuralları üstten alta sırada işlenir. Bir istek koşulların her ikisi de eşleşirse, diğer bir deyişle, kurallar listesinin altındaki listenin başındaki kuralları önceliklidir. Bu nedenle, daha ayrıntılı kurallar alt listede yerleştirmeniz gerekir.

**Örnek**:
- Genel önbellek kuralını: 
   - Önbelleğe alma davranışı: **geçersiz kıl**
   - Önbellek yenileme süresi: 1 gün

- Özel kural #1 önbelleğe alma:
   - Eşleşen koşul: **yolu**
   - Eşleşen değer:   _/home / *_
   - Önbelleğe alma davranışı: **geçersiz kıl**
   - Önbellek yenileme süresi: 2 gün

- Özel kural #2 önbelleğe alma:
   - Eşleşen koşul: **uzantısı**
   - Eşleşen değer: _.html_
   - Önbelleğe alma davranışı: **eksikse ayarlayın**
   - Önbellek yenileme süresi: 3 gün

Bu kurallar ayarlandığında, bir istek için  _&lt;uç noktası ana bilgisayar adı&gt;_.azureedge.net/home/index.html Tetikleyicileri özel önbelleğe alma kural ayarlandığında #2: **eksikse ayarlamak** ve 3 gün sayısı. Bu nedenle, varsa *index.html* dosyasının `Cache-Control` veya `Expires` HTTP üstbilgileri ayardaki; bu üstbilgileri ayarlanmazsa, dosya için 3 gün Aksi halde, önbelleğe alınır.

> [!NOTE] 
> Bir kural değişiklikten önce önbelleğe alınan dosyalar kendi kaynak önbellek süresi ayarı koruyun. Önbellek süreler sıfırlamak için şunları yapmalısınız [dosya temizleme](cdn-purge-endpoint.md). 
>
> Azure CDN yapılandırma değişiklikleri ağ üzerinden yayılması biraz zaman alabilir: 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - İçin **verizon'dan Azure CDN standart** profilleri, yayma işlemi genellikle 10 dakika içinde tamamlanır.  
>

## <a name="see-also"></a>Ayrıca bkz.

- [Önbelleğe alma nasıl işler?](cdn-how-caching-works.md)
- [Öğretici: Azure CDN önbelleğe alma kurallarını ayarlama](cdn-caching-rules-tutorial.md)
