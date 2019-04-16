---
title: 'Azure AD uygulama ara sunucusu: Sürüm yayımlama geçmişi | Microsoft Docs'
description: Bu makalede, Azure AD uygulama proxy'si tüm sürümleri listeler ve yeni özellikler ve düzeltilen sorunlar açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: msmimart
manager: celested
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.subservice: manage-apps
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf9ee43c6c6b332c05286da8e330812d7e0db6c2
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578598"
---
# <a name="azure-ad-application-proxy-version-release-history"></a>Azure AD uygulama ara sunucusu: Sürüm yayınlama geçmişi
Bu makalede sürümleri ve yayımlanan Azure Active Directory (Azure AD) uygulama proxy'si özellikleri listeler. Azure AD ekibi, yeni özellikler ve işlevler ile uygulama proxy'si düzenli olarak güncelleştirmektedir. Yeni bir sürümü yayımlandığında uygulama ara sunucusu bağlayıcılarını otomatik olarak güncelleştirilir.

İlgili kaynaklar listesi aşağıda verilmiştir:

Kaynak |  Ayrıntılar
--------- | --------- |
Uygulama proxy'sini etkinleştirme | Uygulama proxy'sini etkinleştirme, yükleme ve bir bağlayıcıyı kaydetme için ön koşullar Bu açıklanmıştır [öğretici](application-proxy-add-on-premises-application.md).
Azure AD uygulama ara sunucusu bağlayıcıları anlama | Hakkında daha fazla bilgi edinin [Bağlayıcısı Yönetim](application-proxy-connectors.md) ve nasıl Bağlayıcılar [otomatik yükseltme](application-proxy-connectors.md#automatic-updates).
Azure AD uygulama ara sunucusu bağlayıcısını indir |  [En son bağlayıcı indirmek](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download).

## <a name="156120"></a>1.5.612.0

### <a name="release-status"></a>Yayın durumu

20 Eylül 2018'den: İndirme için yayımlanan

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

- QlikSense uygulamanın WebSocket desteği eklendi. QlikSense uygulaması Ara sunucusu ile tümleştirme hakkında daha fazla bilgi için bkz [izlenecek](application-proxy-qlik.md). 
- Yükleme Sihirbazı, bir giden proxy yapılandırma kolaylaştıracak şekilde geliştirildi. 
- TLS 1.2 bağlayıcıları için varsayılan protokol olarak ayarlayın. 
- Yeni bir son kullanıcı lisans sözleşmesi (EULA) eklendi.  

### <a name="fixed-issues"></a>Düzeltilen sorunlar

- Bazı bellek sızıntılarını bağlayıcısında neden olan hata düzeltildi.
- Bağlayıcı zaman aşımı sorunlarıyla ilgili bir hata düzeltmesi içeren Azure Service Bus sürüm güncelleştirildi.

## <a name="154020"></a>1.5.402.0

### <a name="release-status"></a>Yayın durumu

19 Ocak 2018: İndirme için yayımlanan

### <a name="fixed-issues"></a>Düzeltilen sorunlar

- Tanımlama bilgisinin etki alanı çeviri gereken özel etki alanları için destek eklendi.

## <a name="151320"></a>1.5.132.0

### <a name="release-status"></a>Yayın durumu 

25 Mayıs 2017: İndirme için yayımlanan 

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler 

Bağlayıcılar giden bağlantı sınırları üzerinde Gelişmiş denetim. 

## <a name="15360"></a>1.5.36.0

### <a name="release-status"></a>Yayın durumu

15 Nisan 2017: İndirme için yayımlanan

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

- Basitleştirilmiş ekleme ve daha az gerekli bağlantı noktaları ile yönetimi. Uygulama proxy'si, artık yalnızca iki standart giden bağlantı noktaları açılması gerekir: 443 ve 80. Uygulama proxy'si, bir çevre ağında herhangi bir bileşeni hala zorunda kalmazsınız yalnızca giden bağlantılar'ı kullanmaya devam eder. Ayrıntılar için bkz. bizim [yapılandırma belgelerine](application-proxy-add-on-premises-application.md).  
- Dış Ara sunucunuzda veya güvenlik duvarı tarafından destekleniyorsa, ağınız tarafından DNS yerine IP aralığı artık açabilirsiniz. Uygulama Ara sunucusu hizmetlerine bağlantılar gerektiren *. msappproxy.net ve *. servicebus.windows.net yalnızca.


## <a name="earlier-versions"></a>Önceki sürümleri

1.5.36.0'den önceki bir uygulama Proxy Bağlayıcısı sürümünü kullandığınızdan, en son olduğundan emin olmak için en son sürüme güncelleştirin. Özellikleri'tam olarak desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [uzaktan erişim için şirket içi uygulamaları Azure AD uygulama proxy'si aracılığıyla](application-proxy.md).
- Uygulama proxy'sini kullanmaya başlamak için bkz: [Öğreticisi: Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md).
