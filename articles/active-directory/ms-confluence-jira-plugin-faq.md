---
title: Azure Active Directory SSO'ya eklenti SSS | Microsoft Docs
description: Çoklu oturum açma Azure Active Directory ile Jira/Confluence arasında yapılandırma hakkında sık sorulan soruların yanıtlarını alın.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: jeedes
ms.openlocfilehash: eb7576c48d3836eec8051d849cefe4c5ec6658c9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699215"
---
# <a name="faq-for-the-azure-active-directory-sso-plug-in"></a>Azure Active Directory SSO'ya eklenti hakkında SSS

Bu ilgili herhangi bir sorgu varsa lütfen SSS başvurun eklentisi.

## <a name="what-does-the-plug-in-do"></a>Eklenti do nedir?

Eklenti Atlassian (Jira çekirdek, Jira yazılım, Jira hizmet Masası dahil) Jira ve Confluence şirket içi yazılımlar için çoklu oturum açma (SSO) yeteneği sağlar. Bir kimlik sağlayıcıyı (IDP) olarak Azure Active Directory (Azure AD) ile eklenti çalışır.

## <a name="which-atlassian-products-does-the-plug-in-work-with"></a>Hangi Atlassian ürünleri eklenti çalışmak mu?

Şirket içi sürümlerini Jira ve Confluence eklenti çalışır.

## <a name="does-the-plug-in-work-on-cloud-versions"></a>Bulut sürümlerinde eklenti iş mu?

Hayır. Eklenti destekler yalnızca şirket içi Jira ve Confluence sürümleri.

## <a name="which-versions-of-jira-and-confluence-does-the-plug-in-support"></a>Hangi sürümlerinin Jira ve Confluence Eklenti desteği mu?

Eklenti bu sürümlerini destekler:

* Jira çekirdek ve yazılım: 6.0 için 7,8
* Jira hizmet masasına: 3.0 3.2 için
* Confluence: 5.0 5.10

## <a name="is-the-plug-in-free-or-paid"></a>Eklenti ücretsiz ya da Ücretli mi?

Bu ücretsiz bir eklentidir.

## <a name="do-i-need-to-restart-jira-or-confluence-after-i-deploy-the-plug-in"></a>Jira veya Confluence ı Eklenti dağıttıktan sonra yeniden başlatmanız gerekiyor mu?

Yeniden başlatma gerekli değildir. Eklenti hemen kullanmaya başlayabilirsiniz.

## <a name="how-do-i-get-support-for-the-plug-in"></a>Nasıl destek eklenti için sağlarım?

Sizin için ulaşmak [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) herhangi bir destek için gereken bu için eklenti. Takım 24-48 iş saatleri içinde yanıt verir.

Azure portal kanal üzerinden Microsoft ile bir destek bileti de yükseltebilirsiniz.

## <a name="would-the-plug-in-work-on-a-mac-or-ubuntu-installation-of-jira-and-confluence"></a>Eklenti iş Jira ve Confluence Mac veya Ubuntu yüklemesinde musunuz?

Eklentiyi yalnızca 64-bit Windows Server yüklemeleri Jira ve Confluence sınanmıştır.

## <a name="does-the-plug-in-work-with-idps-other-than-azure-ad"></a>Azure AD dışında IdPs eklenti çalışmak mu?

Hayır. Yalnızca Azure AD ile çalışır.

## <a name="what-version-of-saml-does-the-plug-in-work-with"></a>SAML hangi sürümünün eklenti çalışmak mu?

SAML 2.0 ile çalışır.

## <a name="does-the-plug-in-do-user-provisioning"></a>Eklenti kullanıcı sağlamayı işe yarar?

Hayır. Eklenti yalnızca SAML 2.0 tabanlı SSO sağlar. Kullanıcının SSO oturum açma önce uygulamayı sağlanması gerekir.

## <a name="does-the-plug-in-support-cluster-versions-of-jira-and-confluence"></a>Eklenti desteği küme sürümleri Jira ve Confluence mu?

Hayır. Şirket içi sürümlerini Jira ve Confluence eklenti çalışır.

## <a name="does-the-plug-in-work-with-http-versions-of-jira-and-confluence"></a>Eklenti iş sürümleriyle HTTP Jira ve Confluence mu?

Hayır. Yalnızca HTTPS etkin yükleme eklenti çalışır.