---
title: "Azure CDN dosyaları sıkıştırarak performansı | Microsoft Docs"
description: "Dosya aktarımı hızını artırmak ve Azure CDN dosyalarınızda sıkıştırarak sayfa yükleme performansı artırmak öğrenin."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2018
ms.author: mazha
ms.openlocfilehash: 743d1db803cdb58ae8fa37430ccffa10ca003f93
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Azure CDN dosyaları sıkıştırarak performansı
Dosya sıkıştırma, dosya aktarım hızını artırmak ve sunucudan gönderilmeden önce bir dosyanın boyutunu azaltarak sayfa yükleme performansı artırmak için basit ve etkili bir yöntemdir. Dosya sıkıştırma, bant genişliği giderlerini azaltmak ve kullanıcılarınız için daha esnek bir deneyim sağlar.

Dosya sıkıştırma etkinleştirmenin iki yolu vardır:

- Kaynak sunucunuzda sıkıştırmayı etkinleştirin. Bu durumda, CDN sıkıştırılmış dosyaların geçer ve bunları isteyen istemcilere sunar.
- Doğrudan CDN uç sunucularda sıkıştırmayı etkinleştirin. Bu durumda, CDN dosyaları sıkıştırır ve kaynak sunucu tarafından sıkıştırılmaz bile, bunları son kullanıcılara sunar.

> [!IMPORTANT]
> CDN yapılandırma değişiklikleri ağ üzerinden yayılması biraz zaman alabilir. İçin **akamai'den Azure CDN** profilleri yayma genellikle bir dakika içinde tamamlanır.  İçin **verizon'dan Azure CDN** profilleri yayma işlemi genellikle 90 dakika içinde tamamlanır. Sıkıştırma için CDN uç noktanız ilk kez ayarlama, 1-2 sıkıştırma ayarları Pop'lere yayılmadan olmak için sorun giderme önce saat bekleyen düşünün.
> 
> 

## <a name="enabling-compression"></a>Sıkıştırmayı etkinleştirme
Standart ve premium CDN katmanları için sıkıştırma işlevsellik sağlasa da, kullanıcı arabirimi farklıdır. Standart ve premium CDN katmanları arasındaki farklar hakkında daha fazla bilgi için bkz: [Azure CDN'ye genel bakış](cdn-overview.md).

### <a name="standard-tier"></a>Standart katman
> [!NOTE]
> Bu bölümde uygulandığı **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri.
> 
> 

1. CDN profili sayfasında yönetmek istediğiniz CDN uç noktası seçin.
   
    ![CDN profili uç noktaları](./media/cdn-file-compression/cdn-endpoints.png)
   
    CDN uç noktası sayfası açılır.
2. Seçin **sıkıştırma**.

    ![CDN sıkıştırma seçimi](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    Sıkıştırma sayfası açılır.
3. Seçin **üzerinde** sıkıştırmasını etkinleştirmek için.
   
    ![CDN dosya sıkıştırma seçenekleri](./media/cdn-file-compression/cdn-compress-standard.png)
4. Varsayılan MIME türleri kullanın veya ekleyerek veya kaldırarak MIME türleri listesini değiştirin.
   
   > [!TIP]
   > Mümkün olsa da, sıkıştırılmış biçimlerine sıkıştırma uygulamak için önerilmez. Örneğin, ZIP, MP3, MP4 veya JPG.
   > 
 
5. Yaptığınız değişiklikleri yaptıktan sonra seçin **kaydetmek**.

### <a name="premium-tier"></a>Premium katman
> [!NOTE]
> Bu bölüm, yalnızca geçerlidir **verizon'dan Azure CDN Premium** profilleri.
> 

1. CDN profili sayfasından seçin **Yönet**.
   
    ![CDN seçimi yönetme](./media/cdn-file-compression/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **HTTP büyük** sekmesini ve ardından üzerine gelerek **önbellek ayarları** çıkma. Seçin **sıkıştırma**.

    ![CDN sıkıştırma seçimi](./media/cdn-file-compression/cdn-compress-select.png)
   
    Sıkıştırma seçeneklerini görüntülenir.
   
    ![CDN dosya sıkıştırma seçenekleri](./media/cdn-file-compression/cdn-compress-files.png)
3. Sıkıştırma seçerek etkinleştirin **sıkıştırma etkin**. İstediğiniz virgülle ayrılmış liste olarak (boşluksuz) sıkıştırmak için MIME türlerini girin **dosya türlerini** kutusu.
   
   > [!TIP]
   > Mümkün olsa da, sıkıştırılmış biçimlerine sıkıştırma uygulamak için önerilmez. Örneğin, ZIP, MP3, MP4 veya JPG.
   > 
    
4. Yaptığınız değişiklikleri yaptıktan sonra seçin **güncelleştirme**.

## <a name="compression-rules"></a>Sıkıştırma kuralları

### <a name="azure-cdn-from-verizon-profiles-both-standard-and-premium-tiers"></a>Azure CDN Verizon profillerinden (standart ve premium Katmanlar)

İçin **verizon'dan Azure CDN** profilleri, yalnızca uygun dosyalar sıkıştırılır. Sıkıştırma için uygun olması için bir dosya gerekir:
- 128 bayttan büyük olmalıdır
- 1 MB'tan küçük olmalıdır
 
Bu profiller aşağıdaki sıkıştırma kodlamaları destekler:
- gzip (GNU zip)
- SÖNDÜR
- bzip2
- brotli 
 
İstek birden fazla sıkıştırma türünü destekliyorsa, bu sıkıştırma türleri brotli sıkıştırmasının önceliklidir.

Bir varlık için bir istek brotli sıkıştırma belirttiğinde (`Accept-Encoding: br` HTTP üstbilgisi) ve Azure CDN önbellek isabetsizliği isteği sonuçlarında brotli sıkıştırma varlığın kaynak sunucuda gerçekleştirir. Daha sonra sıkıştırılmış dosyayı doğrudan önbellekten sunulur.

### <a name="azure-cdn-from-akamai-profiles"></a>Azure CDN Akamai profillerinden

İçin **akamai'den Azure CDN** profilleri, tüm dosyaları sıkıştırma için uygun. Ancak, bir dosya yapılmış bir MIME türü olmalıdır [sıkıştırma için yapılandırılmış](#enabling-compression).

Bu profiller, yalnızca sıkıştırma gzip kodlama destekler. Bir profil uç noktası gzip kodlanmış bir dosyayı istediğinde, istemci istek bağımsız olarak kaynaktan her zaman istenmektedir. 

## <a name="compression-behavior-tables"></a>Sıkıştırma davranışı tabloları
Aşağıdaki tabloda, her senaryo için Azure CDN sıkıştırma davranışı açıklanmaktadır:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Sıkıştırma devre dışı bırakılmış veya dosya sıkıştırma için uygun değil
| İstemci tarafından istenen biçimi (aracılığıyla Accept-Encoding üstbilgisi) | Önbelleğe alınmış dosya biçimi | İstemci CDN yanıtı | Notlar&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- | --- | --- |
| Sıkıştırılmış |Sıkıştırılmış |Sıkıştırılmış | |
| Sıkıştırılmış |Sıkıştırılmamış |Sıkıştırılmamış | |
| Sıkıştırılmış |Önbelleğe alınmamış |Sıkıştırılmış veya sıkıştırılmamış |Kaynak yanıtı CDN bir sıkıştırır olup olmadığını belirler. |
| Sıkıştırılmamış |Sıkıştırılmış |Sıkıştırılmamış | |
| Sıkıştırılmamış |Sıkıştırılmamış |Sıkıştırılmamış | |
| Sıkıştırılmamış |Önbelleğe alınmamış |Sıkıştırılmamış | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Sıkıştırması etkinleştirilmişse ve dosya sıkıştırma için uygundur
| İstemci tarafından istenen biçimi (aracılığıyla Accept-Encoding üstbilgisi) | Önbelleğe alınmış dosya biçimi | İstemciye CDN yanıtı | Notlar |
| --- | --- | --- | --- |
| Sıkıştırılmış |Sıkıştırılmış |Sıkıştırılmış |CDN transcodes desteklenen biçimler arasında. |
| Sıkıştırılmış |Sıkıştırılmamış |Sıkıştırılmış |CDN sıkıştırır. |
| Sıkıştırılmış |Önbelleğe alınmamış |Sıkıştırılmış |Kaynak sıkıştırılmamış dosyasını döndürürse CDN bir sıkıştırır. <br/>**Verizon'dan Azure CDN** sıkıştırılmamış dosyayı ilk istek üzerine geçirir ve ardından sıkıştırır ve sonraki istekleri için dosyayı önbelleğe alır. <br/>Cache-Control dosyalarıyla: no cache üstbilgisi asla sıkıştırılmaz. |
| Sıkıştırılmamış |Sıkıştırılmış |Sıkıştırılmamış |CDN bir açma işlemi gerçekleştirir. |
| Sıkıştırılmamış |Sıkıştırılmamış |Sıkıştırılmamış | |
| Sıkıştırılmamış |Önbelleğe alınmamış |Sıkıştırılmamış | |

## <a name="media-services-cdn-compression"></a>CDN sıkıştırma medya Hizmetleri
Medya Hizmetleri CDN akış için etkin uç noktaları için sıkıştırma aşağıdaki MIME türleri için varsayılan olarak etkindir: 
- application/vnd.ms-sstr+xml 
- Uygulama/dash + xml
- application/vnd.apple.mpegurl
- Uygulama/f4m + xml 

## <a name="see-also"></a>Ayrıca bkz.
* [CDN dosya sıkıştırma sorunlarını giderme](cdn-troubleshoot-compression.md)    

