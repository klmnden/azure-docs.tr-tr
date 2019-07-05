---
title: Azure Active Directory koşullu erişim Hizmet bağımlılıklarını nelerdir? | Microsoft Docs
description: Koşul bir ilkeyi tetiklemek için Azure Active Directory koşullu erişim nasıl kullanıldığı hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9aca2e4ea5e107358ff72e83562057830ece2cc
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509358"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim Hizmet bağımlılıklarını nelerdir? 

Koşullu erişim ilkeleriyle birlikte, Web siteleri ve Hizmetleri için erişim gereksinimleri belirtebilirsiniz. Örneğin, erişim gereksinimlerinizi multi factor authentication (MFA) gerektirme içerebilir veya [yönetilen cihazlar](require-managed-devices.md). 

Bir site veya hizmet doğrudan erişmek, ilgili ilke etkisini değerlendirmek genellikle kolaydır. Örneğin, MFA için SharePoint yapılandırılmış çevrimiçi gerektiren bir ilke varsa, her oturum açma için SharePoint web portalı MFA zorlanır. Ancak, her zaman diğer bulut uygulamaları için bağımlılıklar ile bulut uygulamaları olduğundan bir ilke etkisini değerlendirmek için rahatça değildir. Örneğin, Microsoft Teams, SharePoint Online kaynaklarına erişim sağlayabilir. Bu nedenle, Microsoft Teams geçerli Senaryomuzda eriştiğinizde, ayrıca SharePoint MFA ilkesini tabi değildir.   

## <a name="policy-enforcement"></a>İlke zorlama 

Yapılandırılmış bir hizmet bağımlılığı varsa, erken bağlanan ya da geç bağlanan zorlama kullanarak ilke uygulanabilir. 

- **Erken bağlanmış ilke zorlaması** kullanıcı gerekir karşılamak bağımlı hizmet ilkesi çağıran uygulama erişmeden önce anlamına gelir. Örneğin, bir kullanıcı MS Teams'e imzalamadan önce SharePoint ilkeyi karşılaması gerekir. 
- **Geç bağlama ilkesi zorlama** çağıran uygulamada oturum kullanıcı işaretinden sonra gerçekleşir. Uygulama, uygulama isteklerini, aşağı akış hizmeti için bir belirteç çağırırken ertelenir. MS Teams Planner ve SharePoint erişme Office.com erişim verilebilir. 

Aşağıdaki diyagramda, MS Teams Hizmet bağımlılıkları gösterir. Geç bağlama zorlama Planner gösterir düz bir ok erken bağlanan zorlama kesikli oku gösterir. 

![MS Teams Hizmet bağımlılıkları](./media/service-dependencies/01.png)

En iyi uygulama, ilgili uygulama ve hizmetlerde mümkün olduğunda genel ilkeler ayarlamanız gerekir. Tutarlı bir güvenlik duruşu sahip en iyi kullanıcı deneyimi sağlar. Örneğin, ortak bir ilke Exchange Online, SharePoint Online, Microsoft Teams ve Skype işletmeniz için önemli ölçüde ayarı için aşağı akış hizmetleriyle geçerli farklı ilkelerinin kaynaklanabilecek beklenmeyen komut istemlerini azaltır. 

Aşağıdaki tabloda gereken istemci uygulamaları burada karşılamak, ek Hizmet bağımlılıkları listeler.  

| İstemci uygulamaları         | Aşağı Akış hizmeti                          | Zorlama |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure Yönetim (portal ve API) | Erken bağlama |
| Microsoft sınıf | Exchange                                    | Erken bağlama |
|                     | SharePoint                                  | Erken bağlama  |
| Microsoft Teams     | Exchange                                    | Erken bağlama |
|                     | MS Planner                                  | Geç bağlama  |
|                     | SharePoint                                  | Erken bağlama |
|                     | Skype Kurumsal Çevrimiçi Sürüm                   | Erken bağlama |
| Office portalı       | Exchange                                    | Geç bağlama  |
|                     | SharePoint                                  | Geç bağlama  |
| Outlook grupları      | Exchange                                    | Erken bağlama |
|                     | SharePoint                                  | Erken bağlama |
| PowerApps           | Microsoft Azure Yönetim (portal ve API) | Erken bağlama |
|                     | Windows Azure Active Directory              | Erken bağlama |
| Proje             | Dynamics CRM                                | Erken bağlama |
| Skype Kurumsal  | Exchange                                    | Erken bağlama |
| Visual Studio       | Microsoft Azure Yönetim (portal ve API) | Erken bağlama |

## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim, ortamınızda uygulama hakkında bilgi edinmek için bkz: [Azure Active Directory'de koşullu erişim dağıtımınızı planlama](plan-conditional-access.md).
