---
title: Azure AD Connect ile eşitlenen öznitelikler | Microsoft Docs
description: Azure Active Directory ile eşitlenen öznitelikler listelenir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c2bb36e0-5205-454c-b9b6-f4990bcedf51
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/24/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d32564808151c4895d2b3802fb48d2bd2d8f753
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64729538"
---
# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect eşitleme: Azure Active Directory ile eşitlenen öznitelikler
Bu konuda, Azure AD Connect eşitlemesi ile eşitlenen öznitelikler listelenir.  
İlgili Azure tarafından gruplandırılmış öznitelikleri AD uygulaması.

## <a name="attributes-to-synchronize"></a>Eşitlenecek öznitelikleri
Sık karşılaşılan bir soru *eşitlemek için en düşük öznitelikler listesinde nedir*. Varsayılan ve önerilen bir yaklaşım olan tam bir GAL (genel adres listesi) bulutta oluşturulması için varsayılan öznitelikler tutmak ve Office 365 iş yüklerini tüm özellikleri almak için. Bazı durumlarda, PII (kişisel bilgiler) verileri, bu örnekte gibi veya bu öznitelikler içeren bu yana, kuruluşunuz bulutta eşitlenmiş hassas istemediği bazı öznitelikleri vardır:  
![hatalı öznitelikleri](./media/reference-connect-sync-attributes-synchronized/badextensionattribute.png)

Bu durumda, bu konudaki özniteliklerin listesi ile başlayın ve duyarlı veya PII verileri içerir ve eşitlenemiyor özniteliklerle belirleyin. Ardından bu öznitelikleri kullanarak yükleme sırasında seçimini [Azure AD uygulaması ve öznitelik filtreleme](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering).

> [!WARNING]
> Öznitelikleri seçimini, dikkatli ve özniteliklerle eşitlemek olası kesinlikle değil yalnızca seçimini gerekir. Diğer öznitelikleri seçimini özellikleri olumsuz bir etkiye sahip.
>
>

## <a name="office-365-proplus"></a>Office 365 ProPlus
| Öznitelik Adı | Kullanıcı | Açıklama |
| --- |:---:| --- |
| accountEnabled |X |Bir hesap etkinse tanımlar. |
| CN = |X | |
| displayName |X | |
| objectSID |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| pwdLastSet |X |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon tarafından kullanılır. |
|samAccountName|X| |
| sourceAnchor |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| usageLocation |X |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userPrincipalName |X |UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |

## <a name="exchange-online"></a>Exchange Online
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinse tanımlar. |
| Yardımcısı |X |X | | |
| altRecipient |X | | |Azure AD Connect derleme 1.1.552.0 gerektirir ya da sonra. |
| authOrig |X |X |X | |
| c |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| CountryCode |X |X | | |
| Bölüm |X |X | | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
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
| homePhone |X |X | | |
| bilgi |X |X |X |Bu öznitelik şu anda gruplar için kullanılmaz. |
| Baş harfleri |X |X | | |
| m |X |X | | |
| legacyExchangeDN |X |X |X | |
| mailNickname |X |X |X | |
| managedBy | | |X | |
| yönetici |X |X | | |
| üye | | |X | |
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
| msExchBypassModerationLink | | |X |Azure AD Connect sürüm 1.1.524.0 kullanılabilir |
| msExchCoManagedByLink | | |X | |
| msExchDelegateListLink |X | | | |
| msExchELCExpirySuspensionEnd |X | | | |
| msExchELCExpirySuspensionStart |X | | | |
| msExchELCMailboxFlags |X | | | |
| msExchEnableModeration |X | |X | |
| msExchExtensionCustomAttribute1 |X |X |X |Bu öznitelik şu anda Exchange Online tarafından kullanılan değil. |
| msExchExtensionCustomAttribute2 |X |X |X |Bu öznitelik şu anda Exchange Online tarafından kullanılan değil. |
| msExchExtensionCustomAttribute3 |X |X |X |Bu öznitelik şu anda Exchange Online tarafından kullanılan değil. |
| msExchExtensionCustomAttribute4 |X |X |X |Bu öznitelik şu anda Exchange Online tarafından kullanılan değil. |
| msExchExtensionCustomAttribute5 |X |X |X |Bu öznitelik şu anda Exchange Online tarafından kullanılan değil. |
| msExchHideFromAddressLists |X |X |X | |
| msExchImmutableID |X | | | |
| msExchLitigationHoldDate |X |X |X | |
| msExchLitigationHoldOwner |X |X |X | |
| msExchMailboxAuditEnable |X | | | |
| msExchMailboxAuditLogAgeLimit |X | | | |
| msExchMailboxGuid |X | | | |
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
| Çağrı |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| proxyAddresses |X |X |X | |
| publicDelegates |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. Hem parola eşitleme hem de Federasyon tarafından kullanılır. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| St |X |X | | |
| streetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| başlık |X |X | | |
| unauthOrig |X |X |X | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userCertificate |X |X | | |
| userPrincipalName |X | | |UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |
| userSMIMECertificates |X |X | | |
| wWWHomePage |X |X | | |

## <a name="sharepoint-online"></a>SharePoint Online
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinse tanımlar. |
| authOrig |X |X |X | |
| c |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| CountryCode |X |X | | |
| Bölüm |X |X | | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
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
| bilgi |X |X |X | |
| Baş harfleri |X |X | | |
| ipPhone |X |X | | |
| m |X |X | | |
| posta |X |X |X | |
| mailnickname |X |X |X | |
| managedBy | | |X | |
| yönetici |X |X | | |
| üye | | |X | |
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
| otherMobile |X |X | | |
| otherPager |X |X | | |
| otherTelephone |X |X | | |
| Çağrı |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| postOfficeBox |X |X | |Bu öznitelik şu anda SharePoint Online tarafından kullanılan değil. |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon tarafından kullanılır. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| St |X |X | | |
| streetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| başlık |X |X | | |
| unauthOrig |X |X |X | |
| url |X |X | | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |
| wWWHomePage |X |X | | |

## <a name="lync-online-subsequently-known-as-skype-for-business"></a>Lync Online (daha sonra Skype Kurumsal'a olarak bilinir)
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinse tanımlar. |
| c |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| Bölüm |X |X | | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
| facsimiletelephonenumber |X |X |X | |
| givenName |X |X | | |
| homephone |X |X | | |
| ipPhone |X |X | | |
| m |X |X | | |
| posta |X |X |X | |
| mailNickname |X |X |X | |
| managedBy | | |X | |
| yönetici |X |X | | |
| üye | | |X | |
| Mobil |X |X | | |
| msExchHideFromAddressLists |X |X |X | |
| msRTCSIP-ApplicationOptions |X | | | |
| Msrtcsıp-DeploymentLocator |X |X | | |
| Msrtcsıp-çizgi |X |X | | |
| Msrtcsıp-OptionFlags |X |X | | |
| msRTCSIP-OwnerUrn |X | | | |
| msRTCSIP-PrimaryUserAddress |X |X | | |
| msRTCSIP-UserEnabled |X |X | | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| otherTelephone |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon tarafından kullanılır. |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| St |X |X | | |
| streetAddress |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| başlık |X |X | | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |
| wWWHomePage |X |X | | |

## <a name="azure-rms"></a>Azure RMS
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinse tanımlar. |
| CN = |X | |X |Ortak adı veya diğer adı. En sık [adres] değeri önek. |
| displayName |X |X |X |Genellikle kolay ad (adı soyadı) olarak gösterilen adını temsil eden bir dize. |
| posta |X |X |X |tam e-posta adresi. |
| üye | | |X | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| proxyAddresses |X |X |X |mekanik özelliği. Azure AD tarafından kullanılır. Kullanıcı için tüm ikincil e-posta adreslerini içerir. |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |Bu UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |

## <a name="intune"></a>Intune
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinse tanımlar. |
| c |X |X | | |
| CN = |X | |X | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
| posta |X |X |X | |
| mailnickname |X |X |X | |
| üye | | |X | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon tarafından kullanılır. |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |

## <a name="dynamics-crm"></a>Dynamics CRM
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinse tanımlar. |
| c |X |X | | |
| CN = |X | |X | |
| Ortak |X |X | | |
| Şirket |X |X | | |
| CountryCode |X |X | | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| m |X |X | | |
| managedBy | | |X | |
| yönetici |X |X | | |
| üye | | |X | |
| Mobil |X |X | | |
| objectSID |X | |X |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| physicalDeliveryOfficeName |X |X | | |
| posta kodu |X |X | | |
| preferredLanguage |X | | | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon tarafından kullanılır. |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| St |X |X | | |
| streetAddress |X |X | | |
| telephoneNumber |X |X | | |
| başlık |X |X | | |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |

## <a name="3rd-party-applications"></a>3. taraf uygulamalar
Bu grup, genel iş yükü veya uygulama için gerekli en az bir öznitelik olarak kullanılan öznitelikler kümesidir. Microsoft dışı bir uygulama veya başka bir bölümde listelenmeyen bir iş yükü için kullanılabilir. Açıkça aşağıdakiler için kullanılır:

* Yammer (yalnızca kullanıcı kullanılır)
* [SharePoint gibi kaynaklar tarafından sunulan hibrit işletmeden işletmeye (B2B) kuruluş arası işbirliği senaryoları](https://go.microsoft.com/fwlink/?LinkId=747036)

Bu grup, Office 365, Dynamics veya Intune desteği sağlamak üzere Azure AD dizini kullanılmıyorsa, kullanılabilen öznitelikleri kümesidir. Bu, küçük bir temel öznitelik vardır.

| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Bir hesap etkinse tanımlar. |
| CN = |X | |X | |
| displayName |X |X |X | |
| givenName |X |X | | |
| posta |X | |X | |
| managedBy | | |X | |
| mailNickName |X |X |X | |
| üye | | |X | |
| objectSID |X | | |mekanik özelliği. AD Kullanıcı tanımlayıcısı Azure arasında eşitleme korumak için kullanılan AD ve AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özelliği. Zaten verilen belirteçleri geçersiz kılmak ne zaman öğrenmek için kullanılır. Parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon tarafından kullanılır. |
| sn |X |X | | |
| sourceAnchor |X |X |X |mekanik özelliği. EKLER ve Azure AD arasındaki ilişki korumak için sabit tanımlayıcısı. |
| usageLocation |X | | |mekanik özelliği. Kullanıcının ülke. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN kullanıcının oturum açma kimliğidir. En sık [adres] olarak aynı değeri. |

## <a name="windows-10"></a>Windows 10
Windows 10 etki alanına katılmış bir computer(device) bazı öznitelikler Azure AD'ye eşitler. Senaryoları hakkında daha fazla bilgi için bkz. [deneyimleri Windows 10 için etki alanına katılan cihazları Azure AD'ye bağlanma](../active-directory-azureadjoin-devices-group-policy.md). Bu öznitelikleri her zaman eşitleme ve Windows 10 işaretini kaldırabilirsiniz bir uygulama görünmez. Windows 10 etki alanına katılmış bir bilgisayar doldurulmuş özniteliği userCertificate sağlayarak tanımlanır.

| Öznitelik Adı | Cihaz | Açıklama |
| --- |:---:| --- |
| accountEnabled |X | |
| deviceTrustType |X |Etki alanına katılmış bilgisayarlar için sabit kodlanmış değeri. |
| displayName |X | |
| ms-DS-CreatorSID |X |RegisteredOwnerReference olarak da adlandırılır. |
| objectGUID |X |Cihaz kimliği olarak da adlandırılır. |
| objectSID |X |OnPremisesSecurityIdentifier olarak da adlandırılır. |
| operatingSystem |X |DeviceOSType olarak da adlandırılır. |
| operatingSystemVersion |X |DeviceOSVersion olarak da adlandırılır. |
| userCertificate |X | |

Bu öznitelikler için **kullanıcı** seçtiğiniz olan diğer uygulamaların yanı sıra şunlardır.  

| Öznitelik Adı | Kullanıcı | Açıklama |
| --- |:---:| --- |
| domainFQDN |X |DNSEtkiAlanıAdı olarak da adlandırılır. Örneğin, contoso.com. |
| domainNetBios |X |NetBiosName olarak da adlandırılır. Örneğin, CONTOSO. |
| msDS-KeyCredentialLink |X |Kullanıcı, Windows iş için Hello kaydedildikten sonra. | 

## <a name="exchange-hybrid-writeback"></a>Exchange karma geri yazma
Etkinleştirmeyi seçtiğinizde bu öznitelikler geri Azure AD'den şirket içi Active Directory'ye yazılır **Exchange karma**. Exchange sürümünüzün bağlı olarak daha az sayıda öznitelik eşitleniyor olabilir.

| Öznitelik adı (kullanıcı Arabirimi Connect) |Öznitelik adı (şirket içi AD) | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |---|
| msDS-ExternalDirectoryObjectID| ms-DS-External-Directory-Object-Id |X | | |Azure AD'de cloudAnchor türetilmiş. Bu öznitelik, Exchange 2016 ve Windows Server 2016 AD yeni bir özelliktir. |
| msExchArchiveStatus| ms-Exch-ArchiveStatus |X | | |Çevrimiçi Arşiv: Müşterilerin posta arşiv sağlar. |
| msExchBlockedSendersHash| ms-Exch-BlockedSendersHash |X | | |Filtreleme: Geri istemcilerden şirket içinde filtreleme ve çevrimiçi güvenli ve engellenen sender verileri yazar. |
| msExchSafeRecipientsHash| ms-Exch-SafeRecipientsHash  |X | | |Filtreleme: Geri istemcilerden şirket içinde filtreleme ve çevrimiçi güvenli ve engellenen sender verileri yazar. |
| msExchSafeSendersHash| ms-Exch-SafeSendersHash  |X | | |Filtreleme: Geri istemcilerden şirket içinde filtreleme ve çevrimiçi güvenli ve engellenen sender verileri yazar. |
| msExchUCVoiceMailSettings| ms-Exch-UCVoiceMailSettings |X | | |Birleştirilmiş ileti um (:) çevrimiçi sesli posta etkinleştir: Microsoft Lync Server tarafından kullanılan tümleştirme Lync Server belirtmek için kullanıcının sesli posta çevrimiçi hizmetlere sahip şirket içi. |
| msExchUserHoldPolicies| MS hariç tutulan hUserHoldPolicies |X | | |Dava tutun: Bulut kullanıcıları dava tutun altında olduğunu belirlemek için hizmetleri sağlar. |
| proxyAddresses| proxyAddresses |X |X |X |Yalnızca Exchange Online adresinden eklenir x500. |
| publicDelegates| ms-Exch-Public-temsilciler  |X | | |Kullanıcılara şirket içi Exchange posta kutusu ile SendOnBehalfTo hakkı verilmesi bir Exchange Online posta kutusu sağlar. Azure AD Connect derleme 1.1.552.0 gerektirir ya da sonra. |

## <a name="exchange-mail-public-folder"></a>Exchange posta ortak klasör
Bu öznitelikler seçtiğinizde etkinleştirmek için Azure AD'ye eşitlenmiş şirket içi Active Directory'den **Exchange posta ortak klasör**.

| Öznitelik Adı | PublicFolder | Açıklama |
| --- | :---:| --- |
| displayName | X |  |
| posta | X |  |
| msExchRecipientTypeDetails | X |  |
| objectGUID | X |  |
| proxyAddresses | X |  |
| targetAddress | X |  |

## <a name="device-writeback"></a>Cihaz geri yazma
Cihaz nesneleri, Active Directory'de oluşturulur. Bu nesneler, Azure AD'ye katılmış cihazlar olabilir veya etki alanına katılmış Windows 10 bilgisayarlar.

| Öznitelik Adı | Cihaz | Açıklama |
| --- |:---:| --- |
| altSecurityIdentities |X | |
| displayName |X | |
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
* Alternatif kimlik kullanırken, şirket içi userPrincipalName özniteliği ile Azure AD özniteliği onPremisesUserPrincipalName eşitlenir. Örnek postası, alternatif kimlik öznitelik ile Azure AD özniteliği userPrincipalName eşitlenir.
* Yukarıdaki nesne türünü listelerindeki **kullanıcı** nesne türü için de geçerlidir **iNetOrgPerson**.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
