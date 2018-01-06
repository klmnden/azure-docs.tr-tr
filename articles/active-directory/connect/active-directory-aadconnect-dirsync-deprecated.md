---
title: "DirSync ve Azure AD eşitleme'den yükseltme | Microsoft Docs"
description: "DirSync ve Azure AD eşitleme için Azure AD Connect yükseltilecek açıklar."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e7b0aa1fc555f0fe4773b6bd67db87a55d85bcf
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Windows Azure Active Directory eşitleme ve Azure Active Directory Eşitleme'yi Yükselt
Azure AD Connect, size Azure AD ve Office 365 ile şirket içi dizininize bağlanmak için en iyi yolu sunmaktadır. Bu araçlar artık kullanım dışı bırakılmış ve 13 Nisan 2017'den itibaren artık desteklenmeyen gibi Windows Azure Active Directory eşitleme (DirSync) veya Azure AD eşitleme için Azure AD Connect yükseltmek için harika bir zamandır.

Kullanım dışı bırakılmıştır iki kimlik eşitleme Araçları (DirSync) tek bir orman müşteriler için sunulan ve Çoklu orman ve diğer müşteriler (Azure AD eşitleme) Gelişmiş. Bu eski araçları tüm senaryoları için kullanılabilir olan tek bir çözüm değiştirilmiştir: Azure AD Connect. Yeni işlev, özellik geliştirmeleri ve yeni senaryolar için destek sunar. Şirket içi kimlik verilerinizi Azure AD'ye eşitlemeye devam edebilmek için ve Office 365 öneririz Azure AD Connect'e yükseltme. Microsoft, 31 Aralık 2017 sonra çalışması için bu eski sürümleri garanti etmez.

DirSync son sürümü Temmuz 2014'te yayımlanmıştır ve Azure AD eşitleme'nın son sürümünde Mayıs 2015'te yayımlanmıştır.

## <a name="what-is-azure-ad-connect"></a>Azure AD Connect nedir?
Azure AD Connect DirSync ve Azure AD eşitleme devamıdır. Bu, desteklenen bu iki tüm senaryoları birleştirir. Daha fazla bilgiyi içinde hakkında [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Kullanımdan kaldırma zamanlaması
| Tarih | Yorum |
| --- | --- |
| 13 Nisan 2016 |Windows Azure Active Directory eşitleme ("DirSync") ve Microsoft Azure Active Directory eşitleme ("Azure AD eşitleme") kullanım dışı bırakıldı olarak bildirilir. |
| 13 Nisan 2017 |Desteği sona eriyor. Müşterilerin, artık Azure AD Connect'e yükseltme yapmadan önce bir destek servis talebi açma mümkün olacaktır. |
|31 Aralık 2017|Azure AD artık Windows Azure Active Directory eşitleme ("DirSync") ve Microsoft Azure Active Directory eşitleme ("Azure AD eşitleme") gelen iletişimleri kabul edebilir.

## <a name="how-to-transition-to-azure-ad-connect"></a>Azure AD Connect geçiş yapma
DirSync çalıştırıyorsanız, Yükseltme yapabileceğiniz iki yolu vardır: yerinde yükseltme ve paralel dağıtım. Yerinde yükseltme çoğu müşteri için önerilen ve en son varsa işletim sistemi ve 50. 000'den az nesneniz. Diğer durumlarda, DirSync yapılandırmanızı Azure AD Connect çalıştıran yeni bir sunucuya burada taşınır paralel dağıtım yapmak için önerilir.

>[!NOTE]
>Yerinde yükseltme form Azure AD Connect DirSync 31 Aralık 2017 sonra artık desteklenmez ve yükseltme paralel dağıtım yapmak gerekebilir.

Azure AD eşitleme kullanırsanız, bir yerinde yükseltme önerilir. İstiyorsanız, paralel olarak yeni bir Azure AD Connect sunucusu yüklemek ve Azure AD Connect, Azure AD eşitleme sunucudan bir esnek geçiş yapmak mümkündür.

| Çözüm | Senaryo |
| --- | --- |
| [DirSync’ten yükseltme](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Zaten çalışmakta olan bir DirSync sunucunuz varsa.</li> |
| [Azure AD eşitleme'den yükseltme](active-directory-aadconnect-upgrade-previous-version.md) |<li>Azure AD eşitleme'den taşıyorsanız.</li> |

Dirsync'ten Azure AD Connect'e yerinde yükseltme nasıl görmek istiyorsanız, bu Channel 9 video bakın:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>SSS
**S: bir e-posta bildirimi Azure ekibi ve/veya Office 365 ileti Merkezi'nden bir ileti aldım ancak bağlanırken kullanılacak.**  
Bildirim yapı numarası 1.0 ile Azure AD Connect kullanan müşteriler için de gönderildi. \*.0 (öncesi 1.1 sürüm kullanarak). Microsoft Azure AD Connect sürümleriyle güncel müşterilere önerir. [Otomatik yükseltme](active-directory-aadconnect-feature-automatic-upgrade.md) de 1.1 sunulan özelliğinin her zaman Azure AD Connect'i yeni bir sürümüne sahip yapmayı kolaylaştırır.

**S: olacak DirSync/Azure AD eşitleme çalışmamaya 13 Nisan 2017?**  
DirSync/Azure AD eşitleme 13 Nisan 2017 üzerinde çalışmaya devam eder.  Ancak, Azure AD DirSync/Azure AD eşitleme iletişimlerinden 31 Aralık 2017 sonra kabul artık.

**S: gelen hangi DirSync sürümleri yükseltebilir miyim?**  
Şu anda kullanılan tüm DirSync sürümünden yükseltme desteklenmiyor. Bu yerinde Not 31 Aralık 2017 sonra Azure AD Connect, dirsync'ten yükseltme desteklenmiyor. Bu tarihten sonra DirSync kullanıyorsanız ve Azure AD Connect'e taşımak istediğiniz müşteriler, Azure AD Connect yeni bir yüklemesidir yerine yapmanız gerekebilir.

**S: ne hakkında FIM/MIM için Azure AD Bağlayıcısı?**  
FIM/MIM için Azure AD Bağlayıcısı **değil** edilmiş kullanım dışı duyurdu. Bu **özelliği dondurma**; hiçbir yeni işlevsellik eklenir ve hiçbir hata düzeltmeleri alır. Microsoft Azure AD Connect'e buradan taşımak plana kullanan müşteriler önerir. Bunu kullanarak tüm yeni dağıtımlar başlatılmaz için önerilir. Bu bağlayıcı, gelecekte kullanım dışı duyurulacaktır.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
