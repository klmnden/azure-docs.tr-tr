---
title: "Azure Active Directory koşullu erişim ile ilgili SSS | Microsoft Docs"
description: "Azure Active Directory'de koşullu erişim hakkında sık sorulan soruların yanıtlarını alın."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: e89550763209bc2d56dbc5dd374239ff404ccb11
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory koşullu erişim ile ilgili SSS

## <a name="which-applications-work-with-conditional-access-policies"></a>Hangi uygulamaların koşullu erişim ilkeleriyle birlikte çalışır?

Koşullu erişim ilkeleriyle birlikte çalışan uygulamalar hakkında daha fazla bilgi için bkz: [uygulamalar ve Azure Active Directory'de koşullu erişim kuralları kullanma tarayıcılar](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Koşullu erişim ilkeleri, B2B işbirliğinin ve Konuk kullanıcılar için uygulanır?

İlkeleri işletmeden işletmeye (B2B) işbirliği kullanıcılar için uygulanır. Ancak, bazı durumlarda, bir kullanıcı ilkesi gereksinimlerini karşılamak mümkün olmayabilir. Örneğin, bir Konuk kullanıcı kuruluşunun çok faktörlü kimlik doğrulamasını desteklemiyor olabilir. 



## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>SharePoint Online İlkesi ayrıca onedrive iş uygulanacak?

Evet. SharePoint Online İlkesi OneDrive iş için de geçerlidir.


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Neden t istemci uygulamaları, Word veya Outlook gibi bir ilke ayarlanamaz?

Bir koşullu erişim ilkesi, bir hizmete erişim gereksinimleri ayarlar. Bu hizmet için kimlik doğrulaması oluştuğunda zorlanır. İlkeyi doğrudan bir istemci uygulaması ayarlanmadı. Bunun yerine, bir istemci bir hizmet çağırdığında uygulanır. Örneğin, SharePoint üzerinde ayarlanmış bir ilkeyle SharePoint çağırma istemciler için geçerlidir. Bir ilke Exchange'de kümesi Outlook'a uygulanır.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Bir koşullu erişim ilkesi hizmet hesaplarına uygulanacak?

Koşullu erişim ilkeleri, tüm kullanıcı hesaplarına uygulanır. Bu, hizmet hesapları olarak kullanılan kullanıcı hesaplarını içerir. Genellikle, katılımsız çalışan bir hizmet hesabı bir koşullu erişim ilkesinin gereksinimlerini gerçekleştiremiyor. Örneğin, çok faktörlü kimlik doğrulaması gerekli olabilir. Hizmet hesapları, koşullu erişim ilkesi yönetimi ayarları kullanarak bir ilkeden tutulabilir. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Graph API koşullu erişim ilkelerini yapılandırma için kullanılabilir mi?

Şu anda yok. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Desteklenmeyen cihaz platformları için varsayılan dışlama İlkesi nedir?

Şu anda, koşullu erişim ilkeleri seçmeli olarak iOS ve Android cihazlarının kullanıcıları uygulanır. Uygulamalar diğer cihaz platformları üzerinde varsayılan olarak, iOS ve Android cihazlar için koşullu erişim ilkesi tarafından etkilenmez. Bir kiracı Yöneticisi, desteklenmeyen platformlarda kullanıcılara erişimi engellemek için genel ilke geçersiz kılmayı seçebilirsiniz.


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Koşullu erişim ilkeleri Microsoft Teams nasıl çalışır?

Microsoft Teams yoğun olarak Exchange Online ve SharePoint Online toplantılar, takvimler ve dosya paylaşımı gibi temel üretkenlik senaryolar için dayanır. Bu bulut uygulamaları için ayarlanan koşullu erişim ilkeleri, doğrudan Microsoft Teams halinde Microsoft Teams kullanıcı işaretlerini olduğunda geçerlidir.

Microsoft Teams de Azure Active Directory koşullu erişim ilkeleri, bir bulut uygulamasında olarak ayrı olarak desteklenir. Bir kullanıcı oturum açtığında bir bulut uygulaması için ayarlanan koşullu erişim ilkeleri Microsoft Teams uygulayın. Ancak, Exchange Online ve SharePoint Online gibi diğer uygulamalar üzerinde doğru ilkeleri olmadan kullanıcılar hala doğrudan bu kaynaklara erişmek mümkün olabilir.

Windows ve Mac için Microsoft Teams Masaüstü istemcileri modern kimlik doğrulamasını destekler. Modern kimlik doğrulaması oturum üzerinde Azure Active Directory Authentication Library (ADAL) Microsoft Office istemci uygulamaları için platformlar arası dayalı açma getirir.