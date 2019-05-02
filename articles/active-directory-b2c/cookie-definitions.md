---
title: Tanımlama bilgisi tanımları - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de kullanılan tanımlama bilgileri için tanımları sağlar.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7864320b71416d1b06661b8ae96c6113962250dd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703972"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için tanımlama bilgileri tanımları

Aşağıdaki tablo, Azure Active Directory B2C'de kullanılan tanımlama bilgilerini listeler.

| Ad | Domain | Süre sonu | Amaç |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | Kiracılar genelinde kullanıcı üyelik verileri tutar. Kiracılar bir kullanıcının üyesi olduğu ve düzey üyelik (yönetici veya kullanıcı). |
| x-ms-cpım-dilim | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | İstekleri uygun üretim örneği kullanılır. |
| x-ms-cpım-işlem | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | İşlemler (Azure AD B2C'ye kimlik doğrulama isteklerinin sayısı) ve geçerli işlem izlemek için kullanılır. |
| x-ms-cpim-sso:{Id} | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | SSO oturum korumak için kullanılır. |
| x-ms-cpim-cache:{id}_n | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md), başarılı kimlik doğrulaması | İstek durumu korumak için kullanılır. |
| x-ms-cpım-csrf | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | Siteler arası istek sahteciliğini belirteci CRSF koruma için kullanılır. |
| x-ms-cpim-dc | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | Azure AD B2C'yi ağ yönlendirme için kullanılır. |
| x-ms-cpım-ctx | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | Bağlam |
| x-ms-cpım-rp | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | Kaynak sağlayıcısı Kiracı için üyelik verilerinin depolanması için kullanılır. |
| x-ms-cpım-rc | Login.microsoftonline.com, b2clogin.com markalı bir etki alanı | Bitiş [tarayıcı oturumu](active-directory-b2c-token-session-sso.md) | Geçiş tanımlama bilgisi depolamak için kullanılır. |

