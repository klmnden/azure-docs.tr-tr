---
title: Azure CDN dosya sıkıştırma sorunlarını giderme | Microsoft Docs
description: Azure CDN dosya sıkıştırma ile ilgili sorunları giderin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 14d50cb7cac77af75dd4b7293812154d1f24e47c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33765533"
---
# <a name="troubleshooting-cdn-file-compression"></a>CDN dosya sıkıştırma sorunlarını giderme
Bu makale ile ilgili sorunları gidermenize yardımcı [CDN dosya sıkıştırma](cdn-improve-performance.md).

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) tıklatıp **destek alın**.

## <a name="symptom"></a>Belirti
Uç noktanız için sıkıştırma etkin, ancak dosyalar sıkıştırılmamış döndürülen.

> [!TIP]
> Dosyaları sıkıştırılmış döndürülen olup olmadığını denetlemek için gibi bir araç kullanmanız gerekir [Fiddler](http://www.telerik.com/fiddler) veya tarayıcınızın [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Onay HTTP yanıt üstbilgilerini, önbelleğe alınan CDN içerik döndürdü.  Adlı bir üst bilgi olup olmadığını `Content-Encoding` değerini **gzip**, **bzıp2**, veya **deflate**, içeriğinizi sıkıştırılır.
> 
> ![İçerik Kodlama üstbilgisi](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Nedeni
Dahil birkaç olası nedenleri şunlardır:

* İstenen içerik sıkıştırma için uygun değil.
* Sıkıştırma istenen dosya türü için etkin değil.
* HTTP isteği, geçerli sıkıştırma türünü isteyen bir üst bilgisi içermiyordu.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
> [!TIP]
> Yeni uç nokta dağıtırken olduğu gibi ile CDN yapılandırma değişiklikleri ağ üzerinden yayılması biraz zaman ayırın.  Genellikle, değişiklikler 90 dakika içinde uygulanır.  CDN uç noktanız için sıkıştırmayı ayarlama ayarladınız ilk kez kullanıyorsanız, 1-2 saat ayarlarını Pop'lere yayılmadan sıkıştırma emin olmak için bekleyen düşünmelisiniz. 
> 
> 

### <a name="verify-the-request"></a>İsteği doğrulama
İlk olarak, biz istek üzerinde bir hızlı sağlamlık denetimi yapmanız gerekir.  Tarayıcınızın kullanabilirsiniz [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) yapılan istekleri görüntülemek için.

* Uç nokta URL'nizi isteği gönderiliyor doğrulayın `<endpointname>.azureedge.net`ve kaynağınıza değil.
* İstek içerdiğini doğrulayın bir **Accept-Encoding** üstbilgi ve o üstbilgisi değeri içeren **gzip**, **deflate**, veya **bzıp2**.

> [!NOTE]
> **Akamai'den Azure CDN** yalnızca destek profilleri **gzip** kodlama.
> 
> 

![CDN istek üstbilgileri](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profiles"></a>Sıkıştırma ayarları (standart CDN profilleri) doğrulayın
> [!NOTE]
> Bu adım yalnızca CDN profilinizi ise geçerlidir bir **Azure CDN standart Microsoft**, **verizon'dan Azure CDN standart**, veya **akamai'den Azure CDN standart** profili. 
> 
> 

Uç noktanız gidin [Azure portal](https://portal.azure.com) tıklatıp **yapılandırma** düğmesi.

* Sıkıştırma etkin doğrulayın.
* Sıkıştırılacak içerik için MIME türü sıkıştırılmış biçimleri listesinde yer doğrulayın.

![CDN sıkıştırma ayarları](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profiles"></a>Sıkıştırma ayarları (Premium CDN profilleri) doğrulayın
> [!NOTE]
> Bu adım yalnızca CDN profilinizi ise geçerlidir bir **verizon'dan Azure CDN Premium** profili.
> 
> 

Uç noktanız gidin [Azure portal](https://portal.azure.com) tıklatıp **Yönet** düğmesi.  Ek Portalı'nı açar.  Üzerine gelerek **HTTP büyük** sekmesini ve ardından üzerine gelerek **önbellek ayarları** çıkma.  Tıklatın **sıkıştırma**. 

* Sıkıştırma etkin doğrulayın.
* Doğrulama **dosya türlerini** listesini içeren bir virgülle ayrılmış listesi (boşluksuz) MIME türleri.
* Sıkıştırılacak içerik için MIME türü sıkıştırılmış biçimleri listesinde yer doğrulayın.

![CDN premium sıkıştırma ayarları](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached-verizon-cdn-profiles"></a>İçeriği önbelleğe alınmış (Verizon CDN profilleri) olduğunu doğrulayın
> [!NOTE]
> Bu adım yalnızca CDN profilinizi ise geçerlidir bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili.
> 
> 

Tarayıcınızın Geliştirici Araçları'nı kullanarak dosyanın nereye istenmektedir bölgede önbelleğe sağlamak için yanıt üstbilgilerini denetleyin.

* Denetleme **Server** yanıtı üstbilgisi.  Üst bilgisi biçiminde olması **Platform (POP/sunucu kimliği)**, aşağıdaki örnekte görüldüğü gibi.
* Denetleme **X önbellek** yanıtı üstbilgisi.  Üstbilgi okumalısınız **İSABET**.  

![CDN yanıt üstbilgileri](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements-verizon-cdn-profiles"></a>Dosya boyutu gereksinimleri (Verizon CDN profilleri) karşıladığını doğrulayın
> [!NOTE]
> Bu adım yalnızca CDN profilinizi ise geçerlidir bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili.
> 
> 

Sıkıştırma için uygun olması için bir dosya aşağıdaki boyut gereksinimlerini karşılaması gerekir:

* 128 bayt daha büyük.
* 1 MB'tan küçük.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>İstek için kaynak sunucuda denetleyin bir **aracılığıyla** üstbilgisi
**Aracılığıyla** HTTP üstbilgisi web sunucusuna istek bir proxy sunucusu tarafından geçirilen gösterir.  Varsayılan olarak Microsoft IIS web sunucuları sıkıştırma yanıtları istek içerdiğinde bir **aracılığıyla** üstbilgi.  Bu davranışı değiştirmek için aşağıdakileri gerçekleştirin:

* **IIS 6**: [HcNoCompressionForProxies ayarlama IIS metatabanı özelliklerinde = "FALSE"](https://msdn.microsoft.com/library/ms525390.aspx)
* **IIS 7 ve en fazla**: [ayarlanacağı **noCompressionForHttp10** ve **noCompressionForProxies** false olarak sunucu yapılandırması](http://www.iis.net/configreference/system.webserver/httpcompression)

