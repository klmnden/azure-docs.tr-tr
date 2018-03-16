---
title: "Microsoft Azure Active Directory tek oturum açma eklentisi ile ilgili SSS | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Microsoft Azure Active Directory tek oturum açma için JIRA arasında yapılandırmayı öğrenin."
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
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: 571fbd5078f66375f6e81cba2a790121366f9d60
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-faq"></a>Microsoft Azure Active Directory tek oturum açma eklentisi ile ilgili SSS 

## <a name="1-whats-the-microsoft-sso-add-on"></a>1. Microsoft SSO eklenti nedir?

Bu eklenti Atlassian'ın (JIRA çekirdek, JIRA yazılım, JIRA hizmet Masası dahil) JIRA ve Confluence şirket içi yazılımlar için çoklu oturum açma özelliği sağlar. Eklenti IDP olarak Azure AD ile çalışır.

## <a name="2-add-on-works-with-which-atlassian-products"></a>2. Eklenti hangi Atlassian ürünleriyle çalışır?

Şimdi itibariyle, şirket içi JIRA ve sürümleriyle Confluence eklenti çalışır.

## <a name="3-does-this-add-on-work-on-cloud-version"></a>3. Bu eklenti bulut sürümünde çalışıyor mu?

Hayır. Yalnızca şirket içi JIRA ve Confluence sürümleri desteklenir.

## <a name="4-which-versions-of-jira-and-confluence-are-supported"></a>4. Hangi sürümlerinin JIRA ve Confluence destekleniyor mu?

Desteklenen sürümlerin listesi aşağıdadır:

* JIRA çekirdek ve yazılım: 6.0 için 7.2.2 
* JIRA Service Desk: 3.0 to 3.2 
* Confluence: 5.0 5.10

## <a name="5-is-this-add-on-free-or-paid"></a>5. Bu eklenti ücretsiz veya Ödendi mi?

Bu ücretsiz bir eklentidir ve Atlassian Pazar yerden yüklenebilir.

## <a name="6-do-i-need-to-restart-jiraconfluence-once-i-deploy-the-add-on"></a>6. Eklenti dağıttığınızda JIRA/Confluence yeniden başlatmanız gerekiyor mu

Yeniden başlatma gerekli post eklenti dağıtım değil. Eklenti dağıtımdan hemen sonra kullanmaya başlayabilirsiniz.

## <a name="7-how-do-i-get-support-for-the-add-on"></a>7. Eklenti için nasıl destek alma?

Üzerinde bize ulaşın: <email> . Biz <> saat içinde yanıt verir. Azure portal kanal üzerinden Microsoft ile bir destek bileti de yükseltebilirsiniz. Ayrıca bize üzerinde çağırabilirsiniz: <Number> <> arasında 'M <> için hafta içi günlerde pm.

## <a name="8-would-this-add-on-work-on-mac-or-ubuntu-installation-of-jira-and-confluence"></a>8. Bu eklenti, Mac veya Ubuntu yüklemesinde JIRA ve Confluence çalışır?

Bu eklenti yalnızca 64 bit Windows server yüklemeleri JIRA ve confluence üzerinde Test ettiğimiz.

## <a name="9-does-this-add-on-work-with-other-idps-than-azure-ad"></a>9. Bu eklenti diğer IdPs daha Azure AD ile çalışır mı?

Hayır. Eklenti yalnızca Azure AD ile çalışır.

## <a name="10-what-version-of-saml-does-the-add-on-work-with"></a>10. SAML hangi sürümünün eklenti ile çalışır mı?

Eklenti SAML 2.0 ile çalışır.

## <a name="11-does-the-add-on-do-use-provisioning-as-well"></a>11. Eklentinin kullanılması da sağlama mu?

Hayır. Şimdi itibariyle eklenti yalnızca SAML 2.0 SSO sağlar. Kullanıcının uygulamada SSO oturum açma önce sağlanması gerekir.

## <a name="12-are-cluster-versions-of-jira-and-confluence-supported-by-add-on"></a>12. Küme sürümleri JIRA ve eklenti tarafından desteklenen confluence mu?

Hayır. Eklenti şirket içi sürümleri JIRA ve Confluence ile çalışır.

## <a name="13-would-this-plugin-work-with-http-version-of-jira-and-confluence"></a>13. Bu eklenti HTTP sürümü JIRA ve Confluence ile çalışır?

Hayır. HTTPS ile eklenti çalışır yalnızca yükleme etkin.

## <a name="14-do-i-need-to-buy-license-of-the-add-on"></a>14. Eklentinin lisansı satın gerekiyor mu?

Bu ücretsiz bir eklentidir.