---
title: Azure CDN içeriğini ülkeye göre kısıtla | Microsoft Docs
description: Erişim coğrafi filtreleme özelliğini kullanarak, Azure CDN içeriğine erişimi kısıtlamak öğrenin.
services: cdn
documentationcenter: ''
author: lichard
manager: akucer
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 30160088d9c770400f342e67527e1cf1cabc4f6b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="restrict-azure-cdn-content-by-country"></a>Azure CDN içeriğini ülkeye göre kısıtla

## <a name="overview"></a>Genel Bakış
Kullanıcı varsayılan olarak, içerik istediğinde, içerik kullanıcı bu istekten burada yapılan bağımsız olarak sunulur. Bazı durumlarda, içeriğinizi ülkeye göre erişimi sınırlamak isteyebilirsiniz. Bu konuda nasıl kullanılacağı açıklanmaktadır **coğrafi filtreleme** izin vermek veya ülkeye göre erişimi engellemek için bu hizmeti yapılandırmak için özellik.

> [!IMPORTANT]
> Verizon ve Akamai ürünleri aynı coğrafi filtreleme işlevselliği sağlar ancak destekledikleri arı ülke kodlarına küçük bir fark vardır. Adım 3 farklar bağlantısına bakın.


Bu tür bir kısıtlama yapılandırma için geçerli konuları hakkında daha fazla bilgi için bkz: [konuları](cdn-restrict-access-by-country.md#considerations) konunun sonunda bölüm.  

![Ülke filtreleme](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a>1. adım: dizin yolu tanımlama
Uç noktanız portalındaki seçin ve bu özelliği bulmak için sol gezinti coğrafi filtreleme sekmesini bulun.

Bir ülke filtresi yapılandırırken, kullanıcılar izin verilen ya da erişim reddedildi konumunun göreli yolunu belirtmeniz gerekir. Tüm dosyaları ile coğrafi filtreleme uygulayabilirsiniz "/" veya seçili klasörler dizin yolu "/ resimler /" belirterek. Ayrıca coğrafi filtreleme tek bir dosyaya dosyasını belirtmek ve eğik bırakarak uygulayabilirsiniz "/ resimler/şehir.PNG silebilir".

Örnek dizin yolu Filtresi:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a>2. adım: eylem tanımlama: Engelle veya izin ver
**Engelle:** belirtilen ülkelerin kullanıcılardan izni verilmez erişim o özyinelemeli yolundan istenen varlıklar için. Bu konumda hiçbir diğer ülke filtreleme seçenekleri yapılandırıldıysa diğer tüm kullanıcıların erişim verilmez.

**İzin ver:** yalnızca belirtilen ülkelerin kullanıcılardan, özyinelemeli yolundan istenen varlıklarına erişimine izin verilir.

## <a name="step-3-define-the-countries"></a>3. adım: ülkelerin tanımlama
Engellemek veya yolu için izin vermek istediğiniz ülkelerin seçin. 

Örneğin, /Photos/Strasbourg/engelleme kuralı dosyaları dahil süzer:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Ülke kodları
**Coğrafi filtreleme** özelliği, bir istek izin verilen ya da güvenli bir dizin için engellenen ülkelerin tanımlamak için ülke kodlarına kullanır. Ülke kodu bulacaksınız [Azure CDN ülke kodlarına](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a id="considerations"></a>Dikkat edilecek noktalar
* Verizon veya Akamai, birkaç dakika yapılandırmasının devreye girmesi için filtreleme ülkeniz değişiklikleri 90 dakikaya kadar sürebilir.
* Bu özellik joker karakterleri desteklemez (örneğin, ' *').
* Göreli yol ile ilişkili coğrafi filtreleme yapılandırması yol uygulanan yinelemeli olacaktır.
* Yalnızca bir kural (noktası aynı göreli yolu için birden fazla ülke filtre oluşturulamıyor. aynı göreli yol uygulanabilir Bununla birlikte, bir klasör birden fazla ülke filtre olabilir. Ülke filtreleri özyinelemeli doğası nedeniyle budur. Diğer bir deyişle, önceden yapılandırılmış bir klasörün bir alt farklı ülke filtre atanabilir.

