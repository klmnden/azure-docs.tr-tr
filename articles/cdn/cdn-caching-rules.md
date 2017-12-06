---
title: "Azure içerik teslim ağı kuralları önbelleğe alma ile önbelleğe alma davranışı denetlemek | Microsoft Docs"
description: "Kuralları önbelleğe alma CDN ayarlamak veya hem genel hem de URL yolu ve dosya uzantılarını gibi koşullarla varsayılan önbellek süre sonu davranışını değiştirmek için kullanabilirsiniz."
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: v-deasim
ms.openlocfilehash: 8f89ef5a1763d5fc4ad09a9aeae89ccf683138c7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-caching-rules"></a>Denetim Azure içerik teslim ağı kuralları önbelleğe alma ile önbelleğe alma davranışı

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca kullanılabilir **Azure CDN Verizon standardı** ve **Azure CDN Akamai standardı**. İçin **Azure CDN Verizon Premium'a**, kullanabileceğiniz [Azure CDN kurallar altyapısı](cdn-rules-engine.md) içinde **Yönet** benzer işlevselliği için portal.
 
Azure içerik teslim ağı dosyalarınızı nasıl önbelleğe denetlemek için iki yol sunar: 

- Kuralları önbelleğe alma: Bu makalede ayarlamak veya hem genel hem de bir URL yolu ve dosya uzantısı gibi özel koşullarla varsayılan önbellek süre sonu davranışını değiştirmek için kuralları önbelleğe alma içerik teslim ağı (CDN) nasıl kullanabileceğinizi açıklar. Azure CDN iki tür kuralları önbelleğe almayı sağlar:
   - Genel kurallar önbelleğe alma: uç nokta için tüm istekleri etkiler, profilinizi her bitiş noktasıyla ilgili genel bir önbellek kuralı ayarlayabilirsiniz. Genel önbellek kuralı tüm HTTP önbellek yönergesi üstbilgileri kılar ayarlayın.
   - Özel önbelleğe alma kuralları: veya daha fazla özel önbelleği kurallarını profilinizde her bitiş noktasıyla ilgili bir ayarlayabilirsiniz. Eşleşme belirli yollar ve dosya uzantıları, sırayla işlenir ve genel önbellek kuralı geçersiz kıl kuralları, önbelleğe alma özel ayarlayın. 

- Sorgu dizesini önbelleğe alma: sorgu dizeleri içeren istekleri önbelleğe alma için Azure CDN nasıl işler ayarlayabilirsiniz. Bilgi için bkz: [denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle](cdn-query-string.md). Dosya alınabilir değilse ayarını önbelleği sorgu dizesi dayalı kurallar ve CDN varsayılan davranışlar önbelleğe alma üzerinde etkisi olmaz.

Varsayılan önbelleğe alma davranışını etkinleştirmek ve yönerge üstbilgileri önbelleğe alma hakkında daha fazla bilgi için bkz: [önbelleğe alma nasıl çalışır](cdn-how-caching-works.md).

## <a name="tutorial"></a>Öğretici

CDN önbelleğe alma nasıl ayarlanacağı kuralları:

1. Azure Portalı'nı açın, bir CDN profili seçin ve sonra bir uç nokta seçin.
2. Sol bölmede ayarları altında tıklatın **önbellek**.
3. Genel bir önbellek kuralı gibi oluşturun:
   1. Altında **genel kurallar önbelleğe alma**ayarlayın **sorgu dizesini önbelleğe alma davranışı** için **sorgu dizelerini yoksayabilir**.
   2. Ayarlama **önbelleğe alma davranışı** için **eksikse ayarlamak**.
   3. İçin **önbelleğe sona erme süresi**, 10 girin **gün** alan.

       Genel önbellek kuralını uç nokta için tüm istekleri etkiler. Varsa, bu kural kaynak önbellek yönergesi üstbilgileri geliştirir (`Cache-Control` veya `Expires`); Aksi halde, bunlar belirtilmezse, önbellek 10 gün olarak ayarlar. 

4. Özel bir önbellek kuralı gibi oluşturun:
    1. Altında **özel kurallar önbelleğe alma**ayarlayın **eşleşen koşulu** için **yolu** ve **eşleşen değeri** için `/images/*.jpg`.
    2. Ayarlama **önbelleğe alma davranışı** için **geçersiz kılma** ve 30 olarak girin **gün** alan.
       
       Bu özel önbellek kuralını herhangi bir önbellek süre olan 30 günden ayarlar `.jpg` görüntü dosyaları `/images` uç noktanızı klasörü. Tüm geçersiz kılmaları `Cache-Control` veya `Expires` kaynak sunucu tarafından gönderilen HTTP üstbilgileri.

  ![Önbelleğe alma kuralları iletişim kutusu](./media/cdn-caching-rules/cdn-caching-rules-dialog.png)

> [!NOTE] 
> Bir kural değişiklikten önce önbelleğe alınan dosyalar kendi kaynak önbellek süresi ayarı koruyun. Önbellek süreler sıfırlamak için şunları yapmalısınız [dosya temizleme](cdn-purge-endpoint.md). İçin **verizon'dan Azure CDN** uç noktaları, onu etkili olması için kuralları önbelleği 90 dakika sürebilir.

## <a name="reference"></a>Başvuru

### <a name="caching-behavior-settings"></a>Önbelleğe alma davranışını ayarları
Genel ve özel önbelleğe alma kuralları için aşağıdaki belirtebilirsiniz **önbelleğe alma davranışı** ayarları:

- **Atlama önbellek**: önbelleğe kaydedilmez ve kaynak tarafından sağlanan önbellek yönergesi üstbilgileri yoksay.
- **Geçersiz kılma**: sağlanan kaynak önbellek yönergesi üstbilgileri yoksay; sağlanan önbellek süresi kullanın.
- **Eksik ayarlamanız**: Uy sağlanan kaynak önbellek yönergesi üst bilgiler, varsa; Aksi takdirde kullanın sağlanan önbellek süresi.

### <a name="cache-expiration-duration"></a>Önbellek sona erme süresi
Genel ve özel önbelleğe alma kuralları için gün, saat, dakika ve saniyeleri önbellek sona erme süresi belirtebilirsiniz:

- İçin **geçersiz kılma** ve **eksikse ayarlamak** **önbelleğe alma davranışı** ayarları, geçerli önbellek süreleri aralık 0 saniye ile 366 gün arasında. 0 saniye değerini, CDN içeriği önbelleğe alır, ancak her istek kaynak sunucu ile düzeltin gerekir.
- İçin **atlama önbellek** ayarı önbellek süresi 0 saniye olarak otomatik olarak ayarlanır ve değiştirilemez.

### <a name="custom-caching-rules-match-conditions"></a>Eşleşme koşullar kuralları özel önbelleğe alma

Özel önbellek kuralları için iki eşleme koşulları kullanılabilir:
 
- **Yol**: Bu koşul hariç, etki alanı adı, URL yolunun eşleşmesi ve joker karakter sembolünü destekler (\*). Örneğin, `/myfile.html`, `/my/folder/*`, ve `/my/images/*.jpg`. Uzunluk 260 karakter olabilir.

- **Uzantı**: Bu durum istenen dosyanın dosya uzantısı ile eşleşir. Eşleştirilecek virgülle ayrılmış dosya uzantıları listesini sağlar. Örneğin, `.jpg`, `.mp3`, veya `.png`. Uzantıları en fazla 50'dir ve uzantı başına karakter sayısı 16'dır. 

### <a name="global-and-custom-rule-processing-order"></a>Genel ve özel kural işleme sırası
Genel ve özel önbelleğe alma kuralları aşağıdaki sırayla işlenir:

- Genel önbelleğe alma kurallar varsayılan CDN önbelleğe alma davranışını (HTTP önbellek yönergesi üstbilgi ayarları) önceliklidir. 

- Özel önbelleğe alma kuralları genel önbelleğe alma kuralları, bunlar burada geçerli önceliklidir. Özel önbelleğe alma kuralları üstten alta sırada işlenir. Bir istek koşulların her ikisi de eşleşirse, diğer bir deyişle, kurallar listesinin altındaki listenin başındaki kuralları önceliklidir. Bu nedenle, daha ayrıntılı kurallar alt listede yerleştirmeniz gerekir.

**Örnek**:
- Genel önbellek kuralını: 
   - Önbelleğe alma davranışı: **geçersiz kıl**
   - Önbellek yenileme süresi: 1 gün

- Özel kural #1 önbelleğe alma:
   - Eşleşen koşul: **yolu**
   - Eşleşen değer:`/home/*`
   - Önbelleğe alma davranışı: **geçersiz kıl**
   - Önbellek yenileme süresi: 2 gün

- Özel kural #2 önbelleğe alma:
   - Eşleşen koşul: **uzantısı**
   - Eşleşen değer:`.html`
   - Önbelleğe alma davranışı: **eksikse ayarlayın**
   - Önbellek yenileme süresi: 3 gün

Bu kurallar ayarlandığında, bir istek için `<endpoint>.azureedge.net/home/index.html` özel önbelleğe alma kural ayarlandığında #2 Tetikleyicileri: **eksikse ayarlamak** ve 3 gün. Bu nedenle, varsa `index.html` dosyasının `Cache-Control` veya `Expires` HTTP üstbilgileri ayardaki; bu üstbilgileri ayarlanmazsa, dosya için 3 gün Aksi halde, önbelleğe alınır.

