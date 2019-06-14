---
title: DirSync ve Azure AD eşitleme'den yükseltme | Microsoft Docs
description: DirSync ve Azure AD Sync'ten Azure AD Connect'e yükseltme işlemini açıklamaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 803fcc0161f2a092006e60db5a98f5bf18dce1c1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60381187"
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Windows Azure Active Directory Sync ve Azure Active Directory Sync yükseltme
Azure AD Connect, size Azure AD ve Office 365 ile şirket içi dizininize bağlanmak için en iyi yolu sunmaktadır. Bu araçlar artık kullanım dışı ve 13 Nisan 2017'den itibaren artık desteklenmemektedir Windows Azure Active Directory Sync (DirSync) veya Azure AD eşitleme'den Azure AD Connect'e yükseltmenin tam zamanı budur.

Kullanım dışı bırakılmıştır iki kimlik eşitleme Araçları tek orman müşteriler (DirSync) için sunulan ve birden çok orman ile diğer ileri düzey müşterilerin (Azure AD Sync). Bu eski araçların tüm senaryolar için kullanılabilir olan tek bir çözüm ile değiştirilmiştir: Azure AD Connect. Yeni işlevsellik ve özellik geliştirmeleri yeni senaryolar için destek sunar. Şirket içi kimlik verilerinizi Azure AD'ye eşitlemeye devam edebilmek için ve Office 365 önemle tavsiye ederiz Azure AD Connect'e yükseltme. Microsoft, 31 Aralık 2017'den sonra çalışması için bu eski sürümleri garanti etmez.

DirSync son sürümü, Temmuz 2014'te yayımlanmıştır ve Azure AD eşitleme'nin son yayın Mayıs 2015'te yayımlanmıştır.

## <a name="what-is-azure-ad-connect"></a>Azure AD Connect nedir?
Azure AD Connect, DirSync ve Azure AD eşitleme geçmiştir. Bu, bu iki desteklenen tüm senaryoları birleştirir. Daha fazla bilgi [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).

## <a name="deprecation-schedule"></a>Kullanımdan kaldırma zamanlamasına
| Tarih | Yorum |
| --- | --- |
| 13 Nisan 2016 |Windows Azure Active Directory Sync ("DirSync") ve Microsoft Azure Active Directory Sync ("Azure AD eşitleme") kullanım dışı olarak bildirilir. |
| 13 Nisan 2017 |Destek sona erer. Müşteriler artık Azure AD Connect'e yükseltme yapmadan önce bir destek olayı açmaya mümkün olacaktır. |
|31 Aralık 2017|Azure AD, artık Windows Azure Active Directory Sync ("DirSync") ve Microsoft Azure Active Directory Sync ("Azure AD eşitleme") gelen iletişimleri kabul edebilir.

## <a name="how-to-transition-to-azure-ad-connect"></a>Azure AD Connect'e geçiş yapma
DirSync çalıştırıyorsanız, Yükseltme yapabileceğiniz iki yolu vardır: Yerinde yükseltme ve paralel dağıtım. Sistem ve 50. 000'den az nesneniz işletim çoğu müşteri için bir yerinde yükseltme önerilir ve yeni bir sahip. Diğer durumlarda, DirSync yapılandırma yeni Azure AD Connect uygulamasını çalıştıran bir sunucuya taşındığı bir paralel dağıtım yapılması önerilir.

| Çözüm | Senaryo |
| --- | --- |
| [DirSync’ten yükseltme](how-to-dirsync-upgrade-get-started.md) |<li>Zaten çalışmakta olan bir DirSync sunucunuz varsa.</li> |
| [Azure AD eşitleme'den yükseltme](how-to-upgrade-previous-version.md) |<li>Azure AD eşitleme'den taşıyorsanız.</li> |

Dirsync'ten Azure AD Connect yerinde yükseltme nasıl görmek istiyorsanız, ardından bu kanal 9 videosu bakın:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>SSS
**S: Azure ekibi ve/veya Office 365 ileti Merkezi'nde bir ileti bir e-posta bildirimi aldım, ancak Connect kullanarak.**  
Bildirim, ayrıca bir yapı numarası 1.0 ile Azure AD Connect kullanarak müşterilere gönderildi. \*.0 (bir pre-1.1 sürümü kullanarak). Microsoft, müşterilerin Azure AD Connect sürümleri ile güncel kalın önerir. [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) 1.1 içinde sunulan özelliğini her zaman Azure AD Connect'i yeni bir sürümüne sahip yapmayı kolaylaştırır.

**S: DirSync/Azure AD eşitleme 13 Nisan 2017'de çalışma durdurur mu?**  
DirSync/Azure AD eşitleme 13 Nisan 2017'de çalışmaya devam eder.  Ancak, Azure AD artık DirSync/Azure AD eşitleme'den iletişimleri 31 Aralık 2017'den sonra kabul etmesi olabilir.

**S: Öğesinden hangi DirSync sürümlerinin yükseltebilir miyim?**  
Şu anda kullanılan DirSync sürümünden yükseltmek için desteklenir. 

**S: Azure AD Bağlayıcısı için FIM/MIM hakkında neler diyeceksiniz?**  
Azure AD Bağlayıcısı için FIM/MIM sahip **değil** olan kullanım dışı olarak duyurdu. Bunu altındadır **özellik dondurma**; hiçbir yeni işlevsellik eklenir ve hiçbir hata düzeltmeleri alır. Microsoft Azure AD Connect'e buradan taşımak plana kullanan müşteriler önerir. Bunu kullanarak tüm yeni dağıtımların başlangıç için önemle tavsiye edilir. Bu bağlayıcı, gelecekte duyurulacaktır.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
