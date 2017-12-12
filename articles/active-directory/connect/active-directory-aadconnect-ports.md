---
title: "Karma kimlik gerekli bağlantı noktalarını ve protokolleri - Azure | Microsoft Docs"
description: "Bu sayfa bir Azure AD Connect için açık olması için gerekli bağlantı noktaları için teknik başvuru sayfadır"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: 79c339c136e614be8a98461533c63d244dabc166
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="hybrid-identity-required-ports-and-protocols"></a>Karma Kimlik için Gereken Bağlantı Noktaları ve Protokoller
Gerekli bağlantı noktalarını ve protokolleri karma kimlik çözümü uygulamak için teknik başvuru belgedir. Aşağıdaki çizimde kullanın ve karşılık gelen tabloya bakın.

![Azure AD Connect nedir?](./media/active-directory-aadconnect-ports/required3.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tablo 1 - Azure AD Connect ve şirket içi AD
Bu tablo Azure AD Connect sunucusu arasındaki iletişim için gereken protokolleri ve bağlantı noktalarını açıklar ve şirket içi AD.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |Hedef orman DNS aramalar. |
| Kerberos |88 (TCP/UDP) |AD ormanı Kerberos kimlik doğrulaması. |
| MS-RPC |135 (TCP/UDP) |AD ormanına bağladığında Azure AD Connect Sihirbazı'nın ilk yapılandırma sırasında ve parola eşitleme sırasında kullanılır. |
| LDAP |389 (TCP/UDP) |AD alanından veri içeri aktarma için kullanılır. Veriler, Kerberos oturum & korumalı ile şifrelenir. |
| RPC | 445 (TCP/UDP) |AD ormanında bir bilgisayar hesabı oluşturmak için sorunsuz SSO tarafından kullanılır. |
| LDAP/SSL |636 (TCP/UDP) |AD alanından veri içeri aktarma için kullanılır. Veri aktarımı imzalanır ve şifrelenir. SSL kullanıyorsanız, yalnızca kullanılır. |
| RPC |49152 - 65535 (rastgele yüksek RPC Port)(TCP/UDP) |AD ormanına bağladığında Azure AD Connect ilk yapılandırma sırasında ve parola eşitleme sırasında kullanılır. Bkz: [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017), ve [KB224196](https://support.microsoft.com/kb/224196) daha fazla bilgi için. |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tablo 2 - Azure AD Connect ve Azure AD
Bu tablo, Azure AD Connect sunucusu ve Azure AD arasındaki iletişim için gereken protokolleri ve bağlantı noktaları açıklar.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |CRL'leri (sertifika iptal SSL sertifikalarını doğrulamak için listelerini) indirmek için kullanılır. |
| HTTPS |443(TCP/UDP) |Azure AD ile eşitlemek için kullanılır. |

URL'leri ve IP listesi için Güvenlik Duvarı'nda açmak için gereken adreslerini görmek [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>Tablo 3 - Azure AD Connect ve AD FS federasyon sunucuları/WAP
Bu tablo, Azure AD Connect sunucusu ve AD FS federasyon/WAP sunucuları arasındaki iletişim için gereken protokolleri ve bağlantı noktaları açıklar.  

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |CRL'leri (sertifika iptal SSL sertifikalarını doğrulamak için listelerini) indirmek için kullanılır. |
| HTTPS |443(TCP/UDP) |Azure AD ile eşitlemek için kullanılır. |
| WinRM |5985 |WinRM dinleyicisi |

## <a name="table-4---wap-and-federation-servers"></a>Tablo 4 - WAP ve Federasyon sunucuları
Bu tablo, Federasyon sunucuları ve WAP sunucuları arasındaki iletişim için gereken protokolleri ve bağlantı noktaları açıklar.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Kimlik doğrulaması için kullanılır. |

## <a name="table-5---wap-and-users"></a>Tablo 5 - WAP ve kullanıcılar
Bu tablo, kullanıcılar ve WAP sunucuları arasında iletişim için gereken protokolleri ve bağlantı noktaları açıklar.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Cihaz kimlik doğrulaması için kullanılır. |
| TCP |49443 (TCP) |Sertifika kimlik doğrulaması için kullanılır. |

## <a name="table-6a--6b---pass-through-authentication-with-single-sign-on-sso-and-password-hash-sync-with-single-sign-on-sso"></a>Tablo 6a & 6b - çoklu oturum açma (SSO) ile doğrudan kimlik doğrulaması ve çoklu oturum açma (SSO) ile parola karma eşitlemesi
Aşağıdaki tablolarda, Azure AD Connect ve Azure AD arasındaki iletişim için gereken protokolleri ve bağlantı noktaları açıklar.

### <a name="table-6a---pass-through-authentication-with-sso"></a>Tablo 6a - SSO ile doğrudan kimlik doğrulaması
|Protokol|Bağlantı Noktası Numarası|Açıklama
| --- | --- | ---
|HTTP|80|SSL gibi güvenlik doğrulaması için giden HTTP trafiğini etkinleştirin. Ayrıca düzgün çalışabilmesi bağlayıcı otomatik güncelleştirme yeteneği için gerekli.
|HTTPS|443| Giden HTTPS trafiğini etkinleştirme ve özelliğini devre dışı bırakma, bağlayıcılar kaydetme, bağlayıcı güncelleştirmeler karşıdan yükleniyor ve tüm kullanıcı oturum açma isteklerini işleme gibi işlemleri için etkinleştirin.

Buna ek olarak, Azure AD Connect doğrudan IP bağlantı kurabilmesi gerekir [Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

### <a name="table-6b---password-hash-sync-with-sso"></a>Tablo 6b - parola karması eşitlemesi SSO

|Protokol|Bağlantı Noktası Numarası|Açıklama
| --- | --- | ---
|HTTPS|443| (Yalnızca SSO kayıt işlemi için gereklidir) SSO kaydını etkinleştirebilirsiniz.

Buna ek olarak, Azure AD Connect doğrudan IP bağlantı kurabilmesi gerekir [Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=41653). Yeniden, bu Only'dir SSO kayıt işlemi için gereklidir.

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablo 7a & 7b - Azure AD Connect Health aracısı için (AD FS/eşitleme) ve Azure AD
Aşağıdaki tablolarda uç noktaları, bağlantı noktaları ve Azure AD Connect Health aracıları ve Azure AD arasındaki iletişim için gereken protokolleri açıklanmaktadır

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablo 7a - bağlantı noktaları ve protokoller için Azure AD Connect Health aracısı için (AD FS/eşitleme) ve Azure AD
Bu tabloda aşağıdaki giden bağlantı noktalarını ve Azure AD Connect Health aracıları ve Azure AD arasındaki iletişim için gereken protokolleri açıklanmaktadır.  

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Giden |
| Azure Service Bus |5671 (TCP/UDP) |Giden |

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>7B - Azure AD Connect Health aracısı için (AD FS/eşitleme) için uç noktaları ve Azure AD
Uç noktalar listesi için bkz: [Azure AD Connect Health Aracısı gereksinimleri bölümüne](../connect-health/active-directory-aadconnect-health-agent-install.md#requirements).

