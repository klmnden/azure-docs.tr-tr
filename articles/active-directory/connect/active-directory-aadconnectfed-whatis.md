---
title: Azure AD Connect ve Federasyon | Microsoft Docs
description: "Bu sayfa, Azure AD Connect kullanan AD FS işlemleri ile ilgili tüm belgeleri için merkezi konumdur."
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: mtillman
editor: 
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: anandy
ms.openlocfilehash: 04516e38e72405ca797a0d748d9ed825ae452966
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect ve federasyon
Federasyon ile yapılandırmanıza azure Active Directory (Azure AD) Bağlan olanak şirket içi Active Directory Federasyon Hizmetleri (AD FS) ve Azure AD. Federasyon ile oturum açma, kullanıcıların parolalarını yeniden girmek zorunda kalmadan şirket ağ üzerindeyken, Azure AD tabanlı hizmetlere ile şirket içi parolalarını--ve oturum açmak etkinleştirebilirsiniz. AD FS ile Federasyon seçeneğini kullanarak, yeni bir AD FS yüklemesini dağıtabilir veya bir Windows Server 2012 R2 grubunda varolan bir yüklemesini belirtebilirsiniz.

Bu konu, Azure AD Connect Federasyon ile ilgili işlevler hakkında bilgi için giriş ilgilidir. Tüm ilgili konulara bağlantılar listeler. Azure AD Connect bağlantıları için bkz: [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: Federasyon konuları
| Konu | Ne kapsar ve okumak ne zaman |
|:--- |:--- |
| **Azure AD Connect kullanıcı oturum açma seçenekleri** | |
| [Kullanıcı oturum açma seçeneklerini anlama](active-directory-aadconnect-user-signin.md) |Çeşitli kullanıcı oturum açma seçenekleri ve Azure oturum açma kullanıcı deneyimini nasıl etkilediklerini hakkında bilgi edinin. |
| **Azure AD Connect kullanarak AD FS yükleme** | |
| [Önkoşullar](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Azure AD Connect yoluyla başarılı bir AD FS yükleme önkoşulları bakın. |
| [Bir AD FS grubunu yapılandırma](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Yeni bir AD FS grubunu, Azure AD Connect kullanarak yükleyin. |
| [Alternatif oturum açma Kimliğini kullanarak Azure AD ile birleştirmek](active-directory-aadconnect-federation-management.md#alternateid) | Alternatif oturum açma Kimliğini kullanarak Federasyon yapılandırma  |
| **AD FS yapılandırmasını Değiştir** | |
| [Güven onarın](active-directory-aadconnect-federation-management.md#repairthetrust) |Onarım geçerli güven arasında AD FS ve Office 365/Azure şirket içi. |
| [Yeni bir AD FS Sunucusu Ekle](active-directory-aadconnect-federation-management.md#addadfsserver) |İlk yüklemeden sonra ek bir AD FS sunucu içeren bir AD FS grubunu genişletin. |
| [Yeni bir AD FS WAP Sunucusu Ekle](active-directory-aadconnect-federation-management.md#addwapserver) |İlk yüklemeden sonra ek bir Web uygulaması Ara sunucusu (WAP) sunucu içeren bir AD FS grubunu genişletin. |
| [Yeni bir Federasyon etki alanına ekleme](active-directory-aadconnect-federation-management.md#addfeddomain) |Azure AD ile birleştirilecek başka bir etki alanına ekleyin. |
| [SSL sertifikasını güncelleştir](active-directory-aadconnectfed-ssl-update.md)| SSL sertifikası için bir AD FS grubunu güncelleştirin. |
| [Office 365 ve Azure AD için Federasyon sertifikalarını yenileme](active-directory-aadconnect-o365-certs.md)|Azure AD ile O365 sertifikanızı yenileyin.|
| **Başka bir Federasyon yapılandırma** | |
| [Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme](active-directory-aadconnectfed-single-adfs-multitenant-federation.md) | Birden çok Azure AD ile tek bir AD FS grubunu birleştirmek| 
| [Bir özel şirket logosu/çizim Ekle](active-directory-aadconnect-federation-management.md#customlogo) |Oturum açma deneyimi, AD FS oturum açma sayfasında gösterilen özel logo belirterek değiştirin. |
| [Bir oturum açma açıklama ekleme](active-directory-aadconnect-federation-management.md#addsignindescription) |AD FS oturum açma sayfasında oturum açma açıklamayı değiştirin. |
| [AD FS talep kuralları değiştirme](active-directory-aadconnect-federation-management.md#modclaims) |Değiştirebilir veya Azure AD Connect eşitleme yapılandırması için karşılık gelen AD FS'de talep kuralı ekleyin. |


## <a name="additional-resources"></a>Ek kaynaklar
* [Federasyonunu iki Azure AD tek AD FS ile](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)
* [Azure AD FS dağıtımı](active-directory-aadconnect-azure-adfs.md)
* [Azure Traffic Manager ile azure'da yüksek kullanılabilirlik çapraz coğrafi AD FS dağıtımı](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
