---
title: Azure CDN içeriğini ülkeye göre kısıtla | Microsoft Docs
description: Erişim coğrafi filtreleme özelliğini kullanarak, Azure CDN içeriğine erişimi kısıtlamak öğrenin.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim
ms.openlocfilehash: 93321c4c8a7f8d79835d702ca07132eed94f6493
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260761"
---
# <a name="restrict-azure-cdn-content-by-country"></a>Azure CDN içeriğini ülkeye göre kısıtla

## <a name="overview"></a>Genel Bakış
Kullanıcı varsayılan olarak, içerik istediğinde, içerik kullanıcı bu istekten burada yapılan bağımsız olarak sunulur. Bazı durumlarda, içeriğinizi ülkeye göre erişimi sınırlamak isteyebilirsiniz. Bu makalede nasıl kullanılacağı açıklanmaktadır *coğrafi filtreleme* izin vermek veya ülkeye göre erişimi engellemek için bu hizmeti yapılandırmak için özellik.

> [!IMPORTANT]
> Azure CDN ürünü tüm aynı coğrafi filtreleme işlevselliği sağlar ancak destekledikleri arı ülke kodlarına küçük bir fark vardır. Daha fazla bilgi için bkz: [Azure CDN ülke kodlarına](https://msdn.microsoft.com/library/mt761717.aspx).


Bu tür bir kısıtlama yapılandırma için geçerli konuları hakkında daha fazla bilgi için bkz: [konuları](cdn-restrict-access-by-country.md#considerations).  

![Ülke filtreleme](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a>1. adım: dizin yolu tanımlama
> [!IMPORTANT]
> **Azure CDN standart Microsoft** profilleri yol tabanlı coğrafi filtreleme desteklemez.
>


Uç noktanız portalındaki seçin ve bu özelliği bulmak için sol gezinti coğrafi filtreleme sekmesini bulun.

Bir ülke filtresi yapılandırırken, kullanıcılar izin verilen ya da erişim reddedildi konumunun göreli yolunu belirtmeniz gerekir. Coğrafi filtreleme, eğik çizgi (/) veya seçili klasörler ile tüm dosyalar için dizin yolu belirterek uygulayabileceğiniz */pictures/*. Ayrıca coğrafi filtreleme tek bir dosyaya dosyasını belirtmek ve eğik bırakarak uygulayabilirsiniz */pictures/city.png*.

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
Coğrafi filtreleme özelliği ülke kodlarına içinden bir istek izin verilen ya da güvenli bir dizin için engellenen ülkelerin tanımlamak için kullanır. Tüm Azure CDN ürünü aynı coğrafi filtreleme işlevsellik sunmalarına karşın, destekledikleri ülke kodlarına küçük bir fark yoktur. Daha fazla bilgi için bkz: [Azure CDN ülke kodlarına](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Ülke filtreleme yapılandırmanızı değişiklikler hemen etkili olmaz:
   * **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
   * **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
   * İçin **verizon'dan Azure CDN standart** ve **verizon'dan Azure CDN Premium** profilleri, yayma işlemi genellikle 10 dakika içinde tamamlanır. 
 
* Bu özellik joker karakterleri desteklemez (örneğin, ' *').

* Göreli yol ile ilişkili coğrafi filtreleme yapılandırması yol uygulanan yinelemeli olacaktır.

* Yalnızca bir kural aynı göreli yoluna uygulanabilir. Diğer bir deyişle, aynı göreli yolu noktası birden fazla ülke filtre oluşturulamıyor. Ancak, bir klasör ülke filtreleri özyinelemeli doğası nedeniyle birden fazla ülke filtreye sahip olabilir. Diğer bir deyişle, önceden yapılandırılmış bir klasörün bir alt farklı ülke filtre atanabilir.

