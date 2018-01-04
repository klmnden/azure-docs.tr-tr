---
title: "Öznitelikleri eşitlenmiş Azure AD Connect tarafından | Microsoft Docs"
description: "Azure Active Directory'ye eşitlenen öznitelikler listelenmiştir."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: c2bb36e0-5205-454c-b9b6-f4990bcedf51
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 1fb5772f58511b33d6927c3d0ff155980ed756ad
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect eşitleme: öznitelikleri eşitlenmiş Azure Active Directory'ye
Bu konuda, Azure AD Connect eşitleme tarafından eşitlenen öznitelikler listelenir.  
Öznitelikleri ilgili Azure tarafından gruplandırılır AD uygulaması.

## <a name="attributes-to-synchronize"></a>Eşitlenecek öznitelikleri
Ortak bir soru *eşitlemek için en düşük öznitelikler listesi nedir*. Varsayılan ve önerilen yaklaşım, tam GAL (genel adres listesi) bulutta oluşturulabilir şekilde varsayılan öznitelikler tutmak için ve Office 365 iş yüklerini tüm özellikleri alınamıyor. Bazı durumlarda, bu öznitelikler içeren bu yana, kuruluşunuz bulutta eşitlenmiş hassas istemediği bazı öznitelikler vardır veya PII (kişisel bilgi) verileri, bu örnekte, ister:  
![hatalı öznitelikleri](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

Bu durumda, bu konu içinde öznitelik listesi ile başlayın ve duyarlı veya PII veri içerir ve eşitlenemiyor özniteliklerle tanımlayın. Ardından yükleme kullanarak sırasında özniteliklerle seçimini [Azure AD uygulaması ve öznitelik filtrelemesi](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

> [!WARNING]
> Öznitelikleri seçimini zaman dikkatli olun ve yalnızca özniteliklerle eşitlemek kesinlikle yapılamaz seçimini kaldırın. Diğer öznitelikleri unselecting özellikleri olumsuz bir etkisi olabilir.
>
>

## <a name="office-365-proplus"></a>Office 365 ProPlus
| Öznitelik Adı | Kullanıcı | Yorum |
| --- |:---:| --- |
| accountEnabled |X |Bir hesap etkinleştirilirse tanımlar. |
| CN = |X | |
| Görünen adı |X | |
| objectSID |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| pwdLastSet |X |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola Eşitleme ve Federasyon tarafından kullanılır. |
| sourceAnchor |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| usageLocation |X |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userPrincipalName |X |UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |

## <a name="exchange-online"></a>Exchange Online
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinleştirilirse tanımlar. |
| Yardımcısı |X |X | | |
| altRecipient |X | | |Azure AD Connect yapı 1.1.552.0 gerektirir veya sonra. |
| authOrig |X |X |X | |
| C |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| CountryCode |X |X | | |
| Bölüm |X |X | | |
| açıklama |X |X |X | |
| Görünen adı |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| HomePhone |X |X | | |
| bilgileri |X |X |X |Bu özellik şu anda grupları için kullanılmaz. |
| Baş harfleri |X |X | | |
| m |X |X | | |
| LegacyExchangeDN |X |X |X | |
| mailNickname |X |X |X | |
| Şirketiniz tarafından | | |X | |
| Yöneticisi |X |X | | |
| Üye | | |X | |
| Mobil |X |X | | |
| msDS-HABSeniorityIndex |X |X |X | |
| msDS-PhoneticDisplayName |X |X |X | |
| msExchArchiveGUID |X | | | |
| msExchArchiveName |X | | | |
| değerlerinin msExchAssistantName |X |X | | |
| msExchAuditAdmin |X | | | |
| msExchAuditDelegate |X | | | |
| msExchAuditDelegateAdmin |X | | | |
| msExchAuditOwner |X | | | |
| msExchBlockedSendersHash |X |X | | |
| msExchBypassAudit |X | | | |
| msExchBypassModerationLink | | |X |Azure AD CONNECT'te sürüm 1.1.524.0 kullanılabilir |
| msExchCoManagedByLink | | |X | |
| msExchDelegateListLink |X | | | |
| msExchELCExpirySuspensionEnd |X | | | |
| msExchELCExpirySuspensionStart |X | | | |
| msExchELCMailboxFlags |X | | | |
| msExchEnableModeration |X | |X | |
| msExchExtensionCustomAttribute1 |X |X |X |Bu özellik şu anda Exchange Online tarafından mı tüketiliyor değil. |
| msExchExtensionCustomAttribute2 |X |X |X |Bu özellik şu anda Exchange Online tarafından mı tüketiliyor değil. |
| msExchExtensionCustomAttribute3 |X |X |X |Bu özellik şu anda Exchange Online tarafından mı tüketiliyor değil. |
| msExchExtensionCustomAttribute4 |X |X |X |Bu özellik şu anda Exchange Online tarafından mı tüketiliyor değil. |
| msExchExtensionCustomAttribute5 |X |X |X |Bu özellik şu anda Exchange Online tarafından mı tüketiliyor değil. |
| msExchHideFromAddressLists |X |X |X | |
| msExchImmutableID |X | | | |
| msExchLitigationHoldDate |X |X |X | |
| msExchLitigationHoldOwner |X |X |X | |
| msExchMailboxAuditEnable |X | | | |
| msExchMailboxAuditLogAgeLimit |X | | | |
| msExchMailboxGUID |X | | | |
| msExchModeratedByLink |X |X |X | |
| msExchModerationFlags |X |X |X | |
| msExchRecipientDisplayType |X |X |X | |
| msExchRecipientTypeDetails |X |X |X | |
| msExchRemoteRecipientType |X | | | |
| msExchRequireAuthToSendTo |X |X |X | |
| msExchResourceCapacity |X | | | |
| msExchResourceDisplay |X | | | |
| msExchResourceMetaData |X | | | |
| msExchResourceSearchProperties |X | | | |
| msExchRetentionComment |X |X |X | |
| msExchRetentionURL |X |X |X | |
| msExchSafeRecipientsHash |X |X | | |
| msExchSafeSendersHash |X |X | | |
| msExchSenderHintTranslations |X |X |X | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| msExchUserHoldPolicies |X | | | |
| msOrg IsOrganizational | | |X | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherTelephone |X |X | | |
| Çağrı cihazı |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| proxyAddresses |X |X |X | |
| publicDelegates |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola Eşitleme ve Federasyon tarafından kullanılır. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityEnabled | | |X |GroupType türetilmiş |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Başlık |X |X | | |
| unauthOrig |X |X |X | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userCertificate |X |X | | |
| userPrincipalName |X | | |UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |
| userSMIMECertificates |X |X | | |
| wWWHomePage |X |X | | |

## <a name="sharepoint-online"></a>SharePoint Online
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinleştirilirse tanımlar. |
| authOrig |X |X |X | |
| C |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| CountryCode |X |X | | |
| Bölüm |X |X | | |
| açıklama |X |X |X | |
| Görünen adı |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| hideDLMembership | | |X | |
| homephone |X |X | | |
| bilgileri |X |X |X | |
| baş harfleri |X |X | | |
| ipPhone |X |X | | |
| m |X |X | | |
| Posta |X |X |X | |
| mailnickname |X |X |X | |
| Şirketiniz tarafından | | |X | |
| Yöneticisi |X |X | | |
| Üye | | |X | |
| middleName |X |X | | |
| Mobil |X |X | | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointLinkedBy |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherIpPhone |X |X | | |
| OtherMobile |X |X | | |
| otherPager |X |X | | |
| otherTelephone |X |X | | |
| Çağrı cihazı |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| postOfficeBox |X |X | |Bu özellik şu anda SharePoint Online tarafından mı tüketiliyor değil. |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola Eşitleme ve Federasyon tarafından kullanılır. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityEnabled | | |X |GroupType türetilmiş |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Başlık |X |X | | |
| unauthOrig |X |X |X | |
| url |X |X | | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |
| wWWHomePage |X |X | | |

## <a name="lync-online-subsequently-known-as-skype-for-business"></a>Lync Online (daha sonra Skype Kurumsal olarak bilinir)
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinleştirilirse tanımlar. |
| C |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| Bölüm |X |X | | |
| açıklama |X |X |X | |
| Görünen adı |X |X |X | |
| facsimiletelephonenumber |X |X |X | |
| givenName |X |X | | |
| homephone |X |X | | |
| ipPhone |X |X | | |
| m |X |X | | |
| Posta |X |X |X | |
| mailNickname |X |X |X | |
| Şirketiniz tarafından | | |X | |
| Yöneticisi |X |X | | |
| Üye | | |X | |
| Mobil |X |X | | |
| msExchHideFromAddressLists |X |X |X | |
| Msrtcsıp-ApplicationOptions |X | | | |
| Msrtcsıp-DeploymentLocator |X |X | | |
| Msrtcsıp-Line |X |X | | |
| Msrtcsıp-OptionFlags |X |X | | |
| Msrtcsıp-OwnerUrn |X | | | |
| Msrtcsıp-Primaryuseraddress'teki |X |X | | |
| Msrtcsıp-UserEnabled |X |X | | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| otherTelephone |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola Eşitleme ve Federasyon tarafından kullanılır. |
| securityEnabled | | |X |GroupType türetilmiş |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| St |X |X | | |
| StreetAddress |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Başlık |X |X | | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |
| wWWHomePage |X |X | | |

## <a name="azure-rms"></a>Azure RMS
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinleştirilirse tanımlar. |
| CN = |X | |X |Ortak adı veya diğer ad. En sık [posta] değeri önek. |
| Görünen adı |X |X |X |Genellikle kolay ad (ad Soyadı) olarak gösterilen adını temsil eden bir dize. |
| Posta |X |X |X |tam e-posta adresi. |
| Üye | | |X | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| proxyAddresses |X |X |X |mekanik özelliği. Azure AD tarafından kullanılır. Kullanıcı için tüm ikincil e-posta adreslerini içerir. |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. |
| securityEnabled | | |X |GroupType türetilmiş. |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userPrincipalName |X | | |Bu UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |

## <a name="intune"></a>Intune
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinleştirilirse tanımlar. |
| C |X |X | | |
| CN = |X | |X | |
| açıklama |X |X |X | |
| Görünen adı |X |X |X | |
| Posta |X |X |X | |
| mailnickname |X |X |X | |
| Üye | | |X | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola Eşitleme ve Federasyon tarafından kullanılır. |
| securityEnabled | | |X |GroupType türetilmiş |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |

## <a name="dynamics-crm"></a>Dynamics CRM
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinleştirilirse tanımlar. |
| C |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| CountryCode |X |X | | |
| açıklama |X |X |X | |
| Görünen adı |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| m |X |X | | |
| Şirketiniz tarafından | | |X | |
| Yöneticisi |X |X | | |
| Üye | | |X | |
| Mobil |X |X | | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| preferredLanguage |X | | | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola Eşitleme ve Federasyon tarafından kullanılır. |
| securityEnabled | | |X |GroupType türetilmiş |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| St |X |X | | |
| StreetAddress |X |X | | |
| telephoneNumber |X |X | | |
| Başlık |X |X | | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |

## <a name="3rd-party-applications"></a>3 taraf uygulamalar
Bu grup, genel iş yükü veya uygulama için gerekli en az öznitelikleri olarak kullanılan öznitelikler kümesidir. Microsoft olmayan uygulama veya başka bir bölümünde listelenmeyen bir iş yükü için kullanılabilir. Açıkça aşağıdakiler için kullanılır:

* (Yalnızca kullanıcı tüketilen) yammer
* [SharePoint gibi kaynaklar tarafından sunulan karma işletmeden işletmeye (B2B) org arası işbirliği senaryoları](http://go.microsoft.com/fwlink/?LinkId=747036)

Bu grup, Office 365, Dynamics veya Intune desteklemek için Azure AD dizini kullanılmazsa, kullanılabilen öznitelikleri kümesidir. Küçük bir çekirdek öznitelikler kümesi vardır.

| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinleştirilirse tanımlar. |
| CN = |X | |X | |
| Görünen adı |X |X |X | |
| givenName |X |X | | |
| Posta |X | |X | |
| Şirketiniz tarafından | | |X | |
| mailNickName |X |X |X | |
| Üye | | |X | |
| objectSID |X | | |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçler geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola Eşitleme ve Federasyon tarafından kullanılır. |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişkiyi korumak için tanımlayıcısı değişmez. |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans atama için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcı için oturum açma kimliğidir. En sık [posta] aynı değeri. |

## <a name="windows-10"></a>Windows 10
Etki alanına katılmış Windows 10 computer(device) bazı öznitelikler Azure ad ile eşitler. Senaryoları hakkında daha fazla bilgi için bkz: [Windows 10 deneyimleri için etki alanına katılmış cihazlar için Azure AD Connect](../active-directory-azureadjoin-devices-group-policy.md). Bu öznitelikler her zaman eşitleyin ve Windows 10 işaretini kaldırabilirsiniz bir uygulama görünmez. Windows 10 etki alanına katılmış bir bilgisayar doldurulmuş özniteliği userCertificate sahip olarak tanımlanır.

| Öznitelik Adı | Cihaz | Yorum |
| --- |:---:| --- |
| accountEnabled |X | |
| deviceTrustType |X |Etki alanına katılmış bilgisayarlar için sabit kodlu değeri. |
| Görünen adı |X | |
| MS DS CreatorSID |X |RegisteredOwnerReference olarak da bilinir. |
| objectGUID |X |Cihaz kimliği olarak da bilinir. |
| objectSID |X |OnPremisesSecurityIdentifier olarak da bilinir. |
| operatingSystem |X |DeviceOSType olarak da bilinir. |
| İşletimsistemisürümü |X |DeviceOSVersion olarak da bilinir. |
| userCertificate |X | |

Bu öznitelikler için **kullanıcı** seçtiğiniz diğer uygulamalar yanı sıra şunlardır.  

| Öznitelik Adı | Kullanıcı | Yorum |
| --- |:---:| --- |
| domainFQDN |X |DNSEtkiAlanıAdı olarak da bilinir. Örneğin, contoso.com. |
| domainNetBios |X |NetBiosName olarak da bilinir. Örneğin, CONTOSO. |

## <a name="exchange-hybrid-writeback"></a>Exchange karma geri yazma
Etkinleştirmeyi seçtiğinizde bu öznitelikler geri Azure AD'den şirket içi Active Directory'ye yazılır **Exchange karma**. Exchange sürümünüzün bağlı olarak, daha az sayıda öznitelik eşitlenmiş olabilir.

| Öznitelik Adı | Kullanıcı | İletişim | Grup | Yorum |
| --- |:---:|:---:|:---:| --- |
| msDS-ExternalDirectoryObjectID |X | | |Azure AD'de cloudAnchor türetilmiş. Bu öznitelik, Exchange 2016 ve Windows Server 2016 AD yenidir. |
| msExchArchiveStatus |X | | |Çevrimiçi Arşiv: posta arşivlemek müşterilerin sağlar. |
| msExchBlockedSendersHash |X | | |Filtreleme: geri filtreleme şirket içi ve çevrimiçi güvenli ve Engellenen gönderen veri istemcilerden yazar. |
| msExchSafeRecipientsHash |X | | |Filtreleme: geri filtreleme şirket içi ve çevrimiçi güvenli ve Engellenen gönderen veri istemcilerden yazar. |
| msExchSafeSendersHash |X | | |Filtreleme: geri filtreleme şirket içi ve çevrimiçi güvenli ve Engellenen gönderen veri istemcilerden yazar. |
| msExchUCVoiceMailSettings |X | | |Birleşik Mesajlaşma (UM) - çevrimiçi sesli posta etkinleştirin: Microsoft Lync Server tarafından kullanılan tümleştirme Lync Server belirtmek için kullanıcının çevrimiçi hizmetlerinde sesli posta olduğunu şirket. |
| msExchUserHoldPolicies |X | | |Mahkeme tutun: hangi kullanıcıların belirlemek için etkinleştirir bulut Hizmetleri, mahkeme tutun altında oluşturulur. |
| proxyAddresses |X |X |X |Yalnızca Exchange Online adresinden eklenir x500. |
| publicDelegates |X | | |Şirket içi Exchange posta kutusu kullanıcılarla SendOnBehalfTo hakkı verilecek bir Exchange Online posta kutusu sağlar. Azure AD Connect yapı 1.1.552.0 gerektirir veya sonra. |

## <a name="exchange-mail-public-folder"></a>Exchange posta ortak klasörü
Bu öznitelikler seçtiğinizde etkinleştirmek için Azure AD ile şirket içi Active Directory'den eşitlenen **Exchange posta ortak klasör**.

| Öznitelik Adı | PublicFolder | Yorum |
| --- | :---:| --- |
| Görünen adı | X |  |
| Posta | X |  |
| msExchRecipientTypeDetails | X |  |
| objectGUID | X |  |
| proxyAddresses | X |  |
| targetAddress | X |  |

## <a name="device-writeback"></a>Cihaz geri yazma
Cihaz nesneleri, Active Directory içinde oluşturulur. Bu nesneler için Azure AD alanına katılmış aygıtlar olabilir veya etki alanına katılmış Windows 10 bilgisayarlar.

| Öznitelik Adı | Cihaz | Yorum |
| --- |:---:| --- |
| altSecurityIdentities |X | |
| Görünen adı |X | |
| DN |X | |
| msDS-CloudAnchor |X | |
| msDS-DeviceID |X | |
| msDS-DeviceObjectVersion |X | |
| msDS-DeviceOSType |X | |
| msDS-DeviceOSVersion |X | |
| msDS-DevicePhysicalIDs |X | |
| msDS-KeyCredentialLink |X |Yalnızca Windows Server 2016 AD şemasıyla |
| msDS-IsCompliant |X | |
| msDS-IsEnabled |X | |
| msDS-Ismanaged |X | |
| msDS-RegisteredOwner |X | |

## <a name="notes"></a>Notlar
* Alternatif kimlik kullanırken, şirket içi özniteliği userPrincipalName Azure AD özniteliği onPremisesUserPrincipalName ile eşitlenir. Alternatif kimlik öznitelik örnek posta için Azure AD özniteliği userPrincipalName ile eşitlenir.
* Yukarıdaki nesne türü listelerinde **kullanıcı** nesne türü için de geçerlidir **iNetOrgPerson**.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
