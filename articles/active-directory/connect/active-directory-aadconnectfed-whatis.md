---
title: Azure AD Connect ve Federasyon | Microsoft Docs
description: Bu sayfa, Azure AD Connect kullanan AD FS işlemleri ile ilgili tüm belgeler için merkezi konumdur.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ce6b889d23959e9d04b95ec2bc5847340b596448
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915845"
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect ve federasyon
Azure Active Directory (Azure AD) Connect sağlar ile Federasyonu yapılandırma şirket içi Active Directory Federasyon Hizmetleri (AD FS) ve Azure AD. Federasyon oturum açma ile kullanıcılar parolalarını yeniden girmek zorunda kalmadan şirket ağ üzerindeyken, Azure AD tabanlı hizmetler ile şirket içi parolalarını--ve için oturum açabilir etkinleştirebilirsiniz. AD FS ile Federasyon seçeneğini kullanarak yeni bir AD FS yüklemesini dağıtabilir veya bir Windows Server 2012 R2 grubunda var olan bir yüklemesini belirtebilirsiniz.

Bu konu Azure AD Connect'i Federasyon ile ilgili işlevler hakkında bilgi için platformdur. Tüm ilgili konulara bağlantılar listeler. Azure AD Connect bağlantıları için bkz [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: Federasyon konuları
| Konu | Neleri kapsar ve okumak ne zaman |
|:--- |:--- |
| **Azure AD Connect kullanıcı oturum açma seçenekleri** | |
| [Kullanıcı oturum açma seçeneklerini anlama](active-directory-aadconnect-user-signin.md) |Azure oturum açma kullanıcı deneyimini nasıl etkilediklerini ve çeşitli kullanıcı oturum açma seçenekleri hakkında bilgi edinin. |
| **Azure AD Connect kullanarak AD FS'yi yükleyin** | |
| [Önkoşullar](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Azure AD Connect aracılığıyla başarılı bir AD FS yükleme önkoşullarını bakın. |
| [AD FS grubu Yapılandır](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Yeni bir AD FS grubu, Azure AD Connect kullanarak yükleyin. |
| [Alternatif bir oturum açma Kimliğini kullanarak Azure AD ile federasyona ](active-directory-aadconnect-federation-management.md#alternateid) | Alternatif oturum açma kimliği kullanarak Federasyonu yapılandırma  |
| **AD FS yapılandırmasını değiştirme** | |
| [Güveni onarın](active-directory-aadconnect-federation-management.md#repairthetrust) |Onarım geçerli güven arasında AD FS ve Office 365/Azure şirket içi. |
| [Yeni bir AD FS Sunucusu Ekle](active-directory-aadconnect-federation-management.md#addadfsserver) |İlk yüklemeden sonra ek AD FS sunucusu ile bir AD FS grubunu genişletin. |
| [Yeni bir AD FS WAP sunucusu ekleme](active-directory-aadconnect-federation-management.md#addwapserver) |İlk yüklemeden sonra ek bir Web uygulaması Ara sunucusu (WAP) sunucusu ile bir AD FS grubunu genişletin. |
| [Yeni bir Federasyon etki alanına ekleme](active-directory-aadconnect-federation-management.md#addfeddomain) |Azure AD ile federasyona eklenmesi için başka bir etki alanına ekleyin. |
| [SSL sertifikasını güncelleştirme](active-directory-aadconnectfed-ssl-update.md)| Bir AD FS grubu için SSL sertifikasını güncelleştirin. |
| [Office 365 ve Azure AD için Federasyon sertifikalarını yenileme](active-directory-aadconnect-o365-certs.md)|Azure AD ile O365 sertifikanızı yenileyin.|
| **Başka bir Federasyon yapılandırma** | |
| [Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme](active-directory-aadconnectfed-single-adfs-multitenant-federation.md) | Birden çok Azure AD'yi tek bir AD FS grubunu ile federasyona eklemek| 
| [Bir özel şirket logosu/gösterim Ekle](active-directory-aadconnect-federation-management.md#customlogo) |Oturum açma deneyimi, AD FS oturum açma sayfasında gösterilen özel logo belirterek değiştirin. |
| [Oturum açma bir açıklama ekleyin](active-directory-aadconnect-federation-management.md#addsignindescription) |AD FS oturum açma sayfasında oturum açma açıklamayı değiştirin. |
| [AD FS talep kurallarını değiştirme](active-directory-aadconnect-federation-management.md#modclaims) |Değiştirin veya talep kuralları karşılık gelen AD FS için Azure AD Connect eşitleme yapılandırmasını ekleyin. |


## <a name="additional-resources"></a>Ek kaynaklar
* [Federasyon iki Azure AD'yi tek AD FS ile](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)
* [Azure'da AD FS dağıtımı](active-directory-aadconnect-azure-adfs.md)
* [Azure Traffic Manager ile azure'da yüksek kullanılabilirlik çapraz coğrafi AD FS dağıtımı](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
