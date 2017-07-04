---
title: "Azure Active Directory cihaz kaydına genel bakış | Microsoft Belgeleri"
description: "Azure Active Directory cihaz kaydı, cihaz temelli koşullu erişim senaryoları için altyapıdır. Bir cihaz kaydedildiğinde Azure Active Directory cihaz kaydı, cihaza kullanıcı oturum açtığı zaman kimlik doğrulama için kullanılan kimliği sağlar."
services: active-directory
keywords: "cihaz kaydı, cihaz kaydını etkinleştirme, cihaz kaydı ve MDM"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 1e92c1a2-01b8-4225-950b-373cd601b035
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: d19956e4964f57251f51eb8ffe5041c6a49da1a7
ms.contentlocale: tr-tr
ms.lasthandoff: 03/10/2017


---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydına başlarken
Azure Active Directory cihaz kaydı, cihaz temelli koşullu erişim senaryoları için altyapıdır. Bir cihaz kaydedildiğinde Azure Active Directory cihaz kaydı, cihaza kullanıcı oturum açtığı zaman kimlik doğrulama için kullanılan kimliği sağlar. Kimliği doğrulanmış cihaz ve cihaz öznitelikleri böylece bulutta ve şirket içinde barındırılan uygulamalar için koşullu erişim ilkelerini zorlamak üzere kullanılabilir.

Microsoft Intune gibi bir mobil cihaz yönetimi (MDM) çözümü ile birleştirildiğinde Azure Active Directory'deki cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Bu durum, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar. Microsoft Intune’da cihazları kaydetme hakkında daha fazla bilgi için bkz. [Intune’da yönetim için cihazları kaydetme](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı tarafından etkinleştirilen senaryolar
Azure Active Directory cihaz kaydı iOS, Android ve Windows cihazları için destek içerir. Azure AD cihaz kaydını kullanan bireysel senaryolar daha fazla özel gereksinime ve platform desteğine sahip olabilir. Bu senaryolar aşağıdaki gibidir:

* **Şirket içinde barındırılan uygulamalara koşullu erişim**: Windows Server 2012 R2 ile AD FS'yi kullanmak için yapılandırılan uygulamalar için erişim ilkeleri içeren kayıtlı cihazları kullanabilirsiniz. Şirket içi uygulamalara koşullu erişimi ayarlama hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory cihaz kaydı hizmetini kullanarak Şirket İçi Uygulamalara Koşullu Erişim](active-directory-device-registration-on-premises-setup.md).
* **Microsoft Intune ile Office 365 uygulamaları için koşullu erişim** : BT yöneticileri kurumsal kaynakların güvenliğinin yanı sıra uyumlu cihazlar üzerindeki bilgi çalışanlarının hizmetlere erişimine olanak sağlamak için koşullu erişim cihaz ilkeleri sağlayabilirler. Daha fazla bilgi edinmek için bkz. [Office 365 hizmetleri için Koşullu Erişim Cihaz İlkeleri](active-directory-conditional-access-device-policies.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydını ayarlama
Mobil cihazların iyi bilinen DNS kayıtlarına bakarak hizmeti keşfedebilmesi için Azure portalında Azure AD cihaz kaydını etkinleştirmeniz gerekir. Windows 10, Windows 8.1, Windows 7, Android ve iOS cihazlarının hizmeti keşfedebilmesi ve kullanabilmesi için şirket DNS'nizi yapılandırmanız gerekir.
Azure Active Directory'de Yönetici Portalı'nı kullanarak kayıtlı cihazları görüntüleyebilir ve etkinleştirebilir/devre dışı bırakabilirsiniz.

> [!NOTE]
> Otomatik cihaz kaydını ayarlama hakkında en son yönergeler için bkz. [Windows etki alanına katılmış cihazların Azure Active Directory ile otomatik kaydını ayarlama](active-directory-conditional-access-automatic-device-registration-setup.md).
> 
> 

### <a name="enable-azure-active-directory-device-registration-service"></a>Azure Active Directory cihaz kaydı hizmetini etkinleştirme
1. Microsoft Azure portalında Yönetici olarak oturum açın.
2. Sol bölmede **Active Directory**'yi seçin.
3. **Directory (Dizin)** sekmesinde dizininizi seçin.
4. **Configure (Yapılandır)** sekmesini seçin.
5. **Devices (Cihazlar)** adlı bölüme kaydırın.
6. **USERS MAY WORKPLACE JOIN DEVICES (KULLANICILAR ÇALIŞMA ALANINA CİHAZ KATABİLİR)** için **ALL (TÜMÜ)** seçeneğini belirleyin.
7. Kullanıcı başına yetkilendirmek istediğiniz maksimum cihaz sayısını seçin.

> [!NOTE]
> Office 365 için Microsoft Intune veya Mobil Cihaz Yönetimi ile kayıt için Çalışma Alanına Katılım gereklidir. Bu hizmetlerden herhangi birini yapılandırdıysanız TÜMÜ seçilir ve HİÇBİRİ düğmesi devre dışı bırakılır.
> 
> 

Varsayılan olarak hizmet için iki öğeli kimlik doğrulama etkin değildir. Yine de bir cihaz kaydedilirken iki öğeli kimlik doğrulama önerilir.

* Bu hizmet için iki öğeli kimlik doğrulama gerekmeden önce Azure Active Directory'de iki öğeli kimlik doğrulama sağlayıcısını yapılandırmanız ve kullanıcı hesaplarınızı Multi-Factor Authentication için yapılandırmanız gerekir, bkz. [Azure Active Directory'ye Multi-Factor Authentication ekleme](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Windows Server 2012 R2 ile AD FS kullanıyorsanız AD FS'de iki öğeli kimlik doğrulama modülünü yapılandırmanız gerekir, bkz. [Active Directory Federasyon Hizmetleri ile Multi-Factor Authentication kullanma](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Azure Active Directory cihaz kaydı keşfini yapılandırma
Windows 7 ve Windows 8.1 cihazları, kullanıcı hesabı adı ile iyi bilinen bir cihaz kaydı sunucu adını birleştirerek cihaz kaydı hizmetini bulur.

Azure Active Directory cihaz kaydı hizmetiniz ile ilişkili A kaydına işaret eden DNS CNAME oluşturmanız gerekir. CNAME kaydı, kuruluşunuzdaki kullanıcı hesapları tarafından kullanılan UPN sonekinin izlediği iyi bilinen enterpriseregistration önekini kullanmalıdır. Kuruluşunuz birden fazla UPN soneki kullanırsa DNS'de birden çok CNAME kaydının oluşturulması gerekir.

Örneğin, kuruluşunuzda @contoso.com ve @region.contoso.com adlı iki UPN soneki kullanırsanız aşağıdaki DNS kayıtlarını oluşturursunuz.

| Girdi | Tür | Adres |
| --- | --- | --- |
| enterpriseregistration.contoso.com |CNAME |enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com |CNAME |enterpriseregistration.windows.net |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Azure Active Directory'de cihaz nesnelerini görüntüleme ve yönetme
1. Azure Yönetici portalından cihazları görüntüleyebilir, engelleyebilir ve engellerini kaldırabilirsiniz. Bu işlemden sonra, engellenen bir cihaz yalnızca kayıtlı cihazlara izin vermek için yapılandırılan uygulamalara erişim sağlayamaz.
2. Microsoft Azure Portal'da Yönetici olarak oturum açın.
3. Sol bölmede **Active Directory**'yi seçin.
4. Dizininizi seçin.
5. **Users (Kullanıcılar)** sekmesini seçin. Ardından cihazlarını görüntülemek için bir kullanıcı seçin.
6. **Devices (Cihazlar)** sekmesini seçin.
7. Açılan menüden **Registered Devices (Kayıtlı Cihazlar)** seçeneğini belirleyin.
8. Burada kullanıcının kayıtlı cihazlarını görüntüleyebilir, engelleyebilir veya engellerini kaldırabilirsiniz.

## <a name="additional-topics"></a>Ek konu başlıkları
Azure AD cihaz kaydı ile Windows 7 ve Windows 8.1 Etki Alanına Katılmış cihazlarınızı kaydedebilirsiniz. Aşağıdaki konu başlıklarında Windows 7 ve Windows 8.1 cihazlarında cihaz kaydını yapılandırmak için gereken önkoşullar ve adımlar hakkında daha fazla bilgi sağlanmaktadır.

* [Windows Etki Alanına Katılmış Cihazlar için Azure Active Directory ile Otomatik cihaz kaydı](active-directory-conditional-access-automatic-device-registration.md)
* [Windows 10 etki alanına katılmış cihazlar için Azure Active Directory ile otomatik cihaz kaydı](active-directory-azureadjoin-devices-group-policy.md)


