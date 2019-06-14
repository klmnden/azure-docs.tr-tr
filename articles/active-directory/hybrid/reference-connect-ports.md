---
title: Karma kimlik gerekli bağlantı noktaları ve protokoller - Azure | Microsoft Docs
description: Bu sayfa bir Azure AD Connect için açık olması için gerekli bağlantı noktaları için teknik başvuru sayfasıdır
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 08/02/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48d2ef0de9ae59e63cd9957200c46c788e2d785f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60387319"
---
# <a name="hybrid-identity-required-ports-and-protocols"></a>Karma Kimlik için Gereken Bağlantı Noktaları ve Protokoller
Gerekli bağlantı noktaları ve protokolleri bir karma kimlik çözümü uygulamak için teknik başvuru belgesidir. Aşağıdaki çizim kullanın ve karşılık gelen tabloya bakın.

![Azure AD Connect nedir?](./media/reference-connect-ports/required3.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tablo 1 - Azure AD Connect ve şirket içi AD
Bu tablo, Azure AD Connect sunucusu arasındaki iletişim için gereken protokolleri ve bağlantı noktalarını açıklar ve şirket içi AD.

| Protocol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |Hedef orman DNS araması. |
| Kerberos |88 (TCP/UDP) |AD ormanı için Kerberos kimlik doğrulaması. |
| MS-RPC |135 (TCP/UDP) |Azure AD Connect Sihirbazı tarafından AD ormanına bağladığında ilk yapılandırma sırasında ve ayrıca parola eşitleme sırasında kullanılır. |
| LDAP |389 (TCP/UDP) |AD alanından veri içeri aktarma için kullanılır. Veriler, Kerberos oturum & korumalı ile şifrelenir. |
| SMB | 445 (TCP/UDP) |Sorunsuz çoklu oturum açma tarafından AD ormanında bilgisayar hesabını oluşturmak için kullanılır. |
| LDAP/SSL |636 (TCP/UDP) |AD alanından veri içeri aktarma için kullanılır. Veri aktarımı imzalanır ve şifrelenir. Yalnızca SSL kullanıyorsanız kullanılır. |
| RPC |49152 - 65535 (rastgele yüksek RPC Port)(TCP/UDP) |Azure AD Connect'in AD ormanına için bağladığında ilk yapılandırma sırasında ve parola eşitleme sırasında kullanılır. Bkz: [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017), ve [KB224196](https://support.microsoft.com/kb/224196) daha fazla bilgi için. |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tablo 2 - Azure AD Connect ve Azure AD
Bu tablo, Azure AD Connect sunucusuyla Azure AD arasındaki iletişim için gereken protokolleri ve bağlantı noktalarını açıklar.

| Protocol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |CRL'leri (sertifika iptal SSL sertifikalarını doğrulamak için listeleri) indirmek için kullanılır. |
| HTTPS |443(TCP/UDP) |Azure AD ile eşitlemek için kullanılır. |

URL'leri ve IP listesi için gereken Güvenlik Duvarı'nda açmak adreslerini görmek [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>Tablo 3 - Azure AD Connect ve AD FS federasyon sunucuları/WAP
Bu tablo, Azure AD Connect sunucusu ve AD FS federasyon/WAP sunucuları arasındaki iletişim için gereken protokolleri ve bağlantı noktalarını açıklar.  

| Protocol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |CRL'leri (sertifika iptal SSL sertifikalarını doğrulamak için listeleri) indirmek için kullanılır. |
| HTTPS |443(TCP/UDP) |Azure AD ile eşitlemek için kullanılır. |
| WinRM |5985 |WinRM dinleyicisi |

## <a name="table-4---wap-and-federation-servers"></a>Tablo 4 - WAP ve Federasyon sunucuları
Bu tablo, WAP sunucularını ve Federasyon sunucuları arasında iletişim için gereken protokolleri ve bağlantı noktalarını açıklar.

| Protocol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Kimlik doğrulaması için kullanılır. |

## <a name="table-5---wap-and-users"></a>Tablo 5 - WAP ve kullanıcılar
Bu tablo, kullanıcılar ve WAP sunucuları arasındaki iletişim için gereken protokolleri ve bağlantı noktalarını açıklar.

| Protocol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Cihaz kimlik doğrulaması için kullanılır. |
| TCP |49443 (TCP) |Sertifika kimlik doğrulaması için kullanılır. |

## <a name="table-6a--6b---pass-through-authentication-with-single-sign-on-sso-and-password-hash-sync-with-single-sign-on-sso"></a>Tablo 6a & 6b - geçişli kimlik doğrulaması ile çoklu oturum açma (SSO) ve çoklu oturum açma (SSO) ile parola karma eşitlemesi
Azure AD Connect ve Azure AD arasındaki iletişim için gereken protokolleri ve bağlantı noktalarını aşağıdaki tablolarda açıklanmıştır.

### <a name="table-6a---pass-through-authentication-with-sso"></a>Tablo 6a - geçişli kimlik doğrulaması ile SSO
|Protocol|Bağlantı Noktası Numarası|Açıklama
| --- | --- | ---
|HTTP|80|SSL gibi güvenlik doğrulaması için giden HTTP trafiğini etkinleştirin. Bağlayıcı için düzgün çalışması otomatik güncelleştirme özelliğini de gerekiyor.
|HTTPS|443| Giden HTTPS trafiğini etkinleştirme ve özelliği devre dışı bırakma, bağlayıcıları kaydetmeye, bağlayıcı güncelleştirmeler karşıdan yükleniyor ve tüm kullanıcı oturum açma isteklerini işleme gibi işlemler için etkinleştirin.

Ayrıca, Azure AD Connect için doğrudan IP bağlantı yapabilmesi gerekir [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="table-6b---password-hash-sync-with-sso"></a>Tablo 6b - parola karma eşitlemesi ile SSO

|Protocol|Bağlantı Noktası Numarası|Açıklama
| --- | --- | ---
|HTTPS|443| SSO kaydı (yalnızca SSO kayıt işlemi için gereklidir) etkinleştirin.

Ayrıca, Azure AD Connect için doğrudan IP bağlantı yapabilmesi gerekir [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Yeniden, bu sadece, SSO kayıt işlemi için gereklidir.

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablo 7a & 7b - Azure AD Connect Health aracısı için (AD FS/eşitleme) ve Azure AD
Aşağıdaki tablo uç noktaları, bağlantı noktaları ve Azure AD ile Azure AD Connect Health aracıları arasındaki iletişim için gereken protokolleri açıklar.

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablo 7a - bağlantı noktaları ve protokoller için (AD FS/eşitleme) Azure AD Connect Health aracısı ve Azure AD
Bu tablo, Azure AD ve Azure AD Connect Health aracıları arasındaki iletişim için gereken protokolleri ve şu giden bağlantı noktaları açıklar.  

| Protocol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Giden |
| Azure Service Bus |5671 (TCP/UDP) |Giden |

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>7B - için (AD FS/eşitleme) Azure AD Connect Health Aracısı uç noktaları ve Azure AD
Uç noktalar listesi için bkz. [Azure AD Connect Health aracısı için gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements).

