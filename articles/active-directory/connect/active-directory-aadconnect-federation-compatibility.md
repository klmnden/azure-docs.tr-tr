---
title: Azure AD federasyonu uyumluluk listesi
description: "Bu sayfayı çoklu oturum açmayı uygulamak için kullanılan Microsoft olmayan kimlik sağlayıcıları vardır."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: bce5867017647764546d872d97943d5d4f01f2d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD federasyonu uyumluluk listesi
Azure Active Directory çoklu oturum sağlar ve herhangi bir Microsoft dışı çözümü gerek kalmadan Office 365 ve diğer Microsoft Online Hizmetleri için karma ve yalnızca bulut uygulamaları için uygulama erişimi güvenliğini Gelişmiş. Office 365, Microsoft Online services, çoğu gibi dizin hizmetleri, kimlik doğrulama ve yetkilendirme için Azure Active Directory ile tümleşiktir. Şirket içi web uygulamaları ve Azure Active Directory de binlerce SaaS uygulamasına çoklu oturum açma sağlar. Lütfen desteklenen SaaS uygulamaları için Azure Active Directory Uygulama Galerisi bakın.

Microsoft dışı Federasyon çözümleri yatırım yapmış kullanıcılar kuruluşlar için bu konu, Windows Server Active Directory kullanıcılar için çoklu oturum açmayı Microsoft Online services ile Microsoft dışı kimlik sağlayıcılarını kullanarak yapılandırmaya yönelik yönergeler içerir. "Azure Active Directory Federasyon uyumluluğu listeden" altında. 

![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford bilgisayar grubu](http://oxfordcomputergroup.com/), bir üçüncü taraf, Microsoft adına, bir dizi ortak kullanım durumları Microsoft olmayan kimlik sağlayıcıları Azure Active Directory ile kullanarak bu tek oturum açma deneyimlerini test.

Burada listelenen üçüncü taraf kimlik sağlayıcınızı nasıl erişebileceğini hakkında daha fazla bilgi için Oxford bilgisayar grubu başvurun [ idp@oxfordcomputergroup.com ](mailto:idp@oxfordcomputergroup.com).

> [!IMPORTANT]
> Oxford bilgisayar grubu yalnızca bu tek oturum açma senaryoları Federasyon işlevlerini test. Oxford bilgisayar grubunu tüm eşitleme, iki öğeli kimlik doğrulama, bu tek oturum açma senaryoları vb. bileşenlerinin sınama gerçekleştirmedi.
> 
> Oturum açma tarafından UPN alternatif kimlik kullanımı da bu programda test edilmemiştir.
> 
> 

* [Azure Active Directory](#azure-active-directory)
* [AuthAnvil çoklu oturum açma 4.5](#authanvil-single-sign-on-45)
* [BIG-IP Erişim İlkesi Yöneticisi BIG-IP sürüm ile x – 11.6 11,3 inç](#big-ip-with-access-policy-manager-big-ip-ver-113x--116x) 
* [BitGlass](#bitglass)
* [CA güvenli bulut](#ca-secure-cloud) 
* [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
* [Centrify](#centrify) 
* [Dell bir kimlik bulut Erişim Yöneticisi v7.1](#dell-one-identity-cloud-access-manager-v71) 
* [DigitalPersona bileşik kimlik doğrulaması](#digitalpersona-composite-authentication)
* [IBM Tivoli Identity Manager 6.2.2 Federasyon](#ibm-tivoli-federated-identity-manager-622) 
* [IceWall Federasyon sürüm 3.0](#icewall-federation-version-30) 
* [Memority](#memority)
* [NetIQ Erişim Yöneticisi 4.x](#netiq-access-manager-4x) 
* [Okta](#okta) 
* [OneLogin](#onelogin) 
* [En iyi IDM sanal kimlik Server Federasyon Hizmetleri](#optimal-idm-virtual-identity-server-federation-services) 
* [PingFederate 6.11, 7.2, 8.x](#pingfederate-611-72-8x)
* [RadiantOne CFS 3.0](#radiantone-cfs-30) 
* [Sailpoint IdentityNow](#sailpoint-identitynow)
* [SecureAuth IDP 7.2.0](#secureauth-idp-720) 
* [Oturum & 5.3 gidin](#signgo-53) 
* [SoftBank teknoloji çevrimiçi hizmet kapısı](#softbank)
* [Bir VMware çalışma](#vmware-workspace-one)



> [!IMPORTANT]
> Bu üçüncü taraf ürünleri olduğundan, Microsoft destek için dağıtım, yapılandırma, sorun giderme, en iyi yöntemler, vb. sorunlar ve bu kimlik sağlayıcıları sorularınız sağlamaz. Destek ve bu kimlik sağlayıcıları ile ilgili sorular için desteklenen Üçüncü taraflardan doğrudan başvurun.
> 
> Bu üçüncü taraf kimlik sağlayıcıları, WS-Federasyon ve yalnızca WS-Trust protokolleri kullanarak Microsoft bulut hizmetleriyle birlikte çalışabilirlik için test edilmiş. Sınama SAML protokolü kullanılarak içermiyordu.
> 


## <a name="azure-active-directory"></a>Azure Active Directory

Bu oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |
| Office 2016 gibi ADAL kullanan Modern uygulamalar |Destekleniyor |None |

Azure Active Directory AD FS ile kullanma hakkında daha fazla bilgi için bkz: [Active Directory Federasyon Hizmetleri (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

Azure Active Directory Parola Eşitleme ile kullanma hakkında daha fazla bilgi için bkz: [Azure AD Connect](active-directory-aadconnect.md).

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil çoklu oturum açma 4.5

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Daha fazla bilgi için bkz: [AuthAnvil çoklu oturum açma.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-).


## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP Erişim İlkesi Yöneticisi BIG-IP sürüm ile. x – 11.6 11,3 inç

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Desteklenmiyor |Desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

BIG-IP Erişim İlkesi Yöneticisi hakkında daha fazla bilgi için bkz: [BIG-IP Erişim İlkesi Yöneticisi.](https://f5.com/products/modules/access-policy-manager) 

Active Directory kullanıcılarınıza, çoklu oturum açma deneyimini sağlamak üzere STS BIG-IP Erişim İlkesi Yöneticisi yönergeleri Bu yapılandırma için PDF'yi indirmek [BIG-IP](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf).

## <a name="bitglass"></a>BitGlass

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

BitGlass hakkında daha fazla bilgi için bkz: [BitGlass](http://www.bitglass.com).

## <a name="ca-secure-cloud"></a>CA güvenli bulut

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

CA güvenli bulut hakkında daha fazla bilgi için bkz: [CA güvenli bulut](http://www.ca.com/us/products/security-as-a-service.aspx).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 toplu sürüm 4

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

CA SiteMinder hakkında daha fazla bilgi için bkz: [CA SiteMinder Federasyon](http://www.ca.com/us/products/ca-single-sign-on.html). 

## <a name="centrify"></a>Centrify

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |İstemci erişim denetimi desteklenmiyor |

Centrify hakkında daha fazla bilgi için bkz: [Centrify](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp).

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell bir kimlik bulut Erişim Yöneticisi v7.1

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Dell bir kimlik bulut Erişim Yöneticisi hakkında daha fazla bilgi için bkz: [Dell bir kimlik bulut Erişim Yöneticisi](http://software.dell.com/products/cloud-access-manager).

 Office 365 kullanıcılarınız için çoklu oturum açma deneyimini sağlamak yönergeler için bu STS yapılandırmak, bkz: [yapılandırma Office 365 kullanıcıları](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365). 

## <a name="digitalpersona-composite-authentication"></a>DigitalPersona bileşik kimlik doğrulaması  

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor|
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor|
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Daha fazla bilgi için bkz: [DigitalPersona bileşik kimlik doğrulaması](http://www.crossmatch.com/uploadedFiles/Support/Reference_Material/DigitalPersona-Office-365-Deployment-Guide.pdf).


## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli Identity Manager 6.2.2 Federasyon

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

IBM Tivoli federe kimlik yöneticisi hakkında daha fazla bilgi için bkz: [IBM güvenlik erişim Yöneticisi for Microsoft Applications](http://www-01.ibm.com/support/docview.wss?uid=swg24029517).

## <a name="icewall-federation-version-30"></a>IceWall Federasyon sürüm 3.0

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

IceWall Federasyon hakkında daha fazla bilgi için bkz: [IceWall Federasyon sürüm 3.0](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) ve [IceWall Federasyon Office 365 ile](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html).

## <a name="memority"></a>Memority

Bu oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Memority kullanma hakkında daha fazla bilgi için bkz: [Memority](http://www.memority.com).


## <a name="netiq-access-manager-4x"></a>NetIQ Erişim Yöneticisi 4.x

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None|
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None|
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Daha fazla bilgi için bkz: [NetIQ Erişim Yöneticisi](https://www.netiq.com/documentation/access-manager-43/admin/data/b65ogn0.html#b12iqp0m).

## <a name="okta"></a>Okta

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması ek web sunucusu ve Okta Uygulama Kurulumu gerektirir. |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Okta hakkında daha fazla bilgi için bkz: [Okta](https://www.okta.com/).

## <a name="onelogin"></a>OneLogin

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

OneLogin hakkında daha fazla bilgi için bkz: [OneLogin](https://www.onelogin.com/).

## <a name="optimal-idm-virtual-identity-server-federation-services"></a>En iyi IDM sanal kimlik Server Federasyon Hizmetleri

Senaryo destek matrisi bu tek oturum açma deneyimi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |

Bkz: istemci erişimi hakkında daha fazla bilgi ilkeleri için [Office 365 Hizmetleri temel istemcinin konumu için erişimi sınırlama](https://technet.microsoft.com/library/hh526961.aspx).





## <a name="pingfederate-611-72-8x"></a>PingFederate 6.11, 7.2, 8.x

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Active Directory kullanıcılarınız için çoklu oturum açma deneyimini sağlamak üzere PingFederate yönergeler için bu STS yapılandırmak, aşağıdakilerden birini bakın: 

- [PingFederate 6.11](http://go.microsoft.com/fwlink/?LinkID=266321)
- [PingFederate 7.2](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)
- [PingFederate 8.x](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

RadiantOne CFS hakkında daha fazla bilgi için bkz: [RadiantOne CFS](http://www.radiantlogic.com/products/radiantone-cfs/).

## <a name="sailpoint-identitynow"></a>Sailpoint IdentityNow

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Daha fazla bilgi için bkz: [Sailpoint IdentityNow](https://www.sailpoint.com/idaas-identity-as-a-service-identitynow/).

## <a name="secureauth-idp-720"></a>SecureAuth IDP 7.2.0

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir: 

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |None |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

SecureAuth hakkında daha fazla bilgi için bkz: [SecureAuth IDP](http://go.microsoft.com/?linkid=9845293).














## <a name="signgo-53"></a>Oturum & 5.3 gidin

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Kerberos desteklenen sözleşmeleri |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |None |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Oturum & Git 5.3, Kerberos sözleşme yapılandırması üzerinden Kerberos kimlik doğrulamasını destekler.  Bu yapılandırma ile daha fazla yardım için lütfen Ilex başvurun veya kurulum kılavuzunu görüntülemek [oturum & Git](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)

## <a name="softbank-technology-online-service-gate"></a>SoftBank teknoloji çevrimiçi hizmet kapısı

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

SoftBank teknoloji çevrimiçi hizmet kapısı hakkında daha fazla bilgi için bkz: [Softbank](https://www.softbanktech.jp/service/list/osg-pro-ent/)

## <a name="vmware-workspace-one"></a>Bir VMware çalışma

Bu tek oturum açma deneyimi için senaryo destek matrisi verilmiştir:

| İstemci | Destek | Özel durumlar |
| --- | --- | --- |
| Exchange Web erişimi ve SharePoint Online gibi Web tabanlı istemciler |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Lync, Office aboneliği, CRM gibi zengin istemci uygulamaları |Destekleniyor |Tümleşik Windows kimlik doğrulaması desteklenmiyor |
| Outlook ve ActiveSync gibi zengin e-posta istemcileri |Destekleniyor |None |

Bkz: hakkında daha fazla bilgi için [VMware çalışma alanında bir](http://www.vmware.com/pdf/vidm-office365-saml.pdf)

