---
title: 'Azure Active Directory etki alanı Hizmetleri: Özellikleri | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri özellikleri
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: ergreenl
ms.openlocfilehash: cd4fdb1e59f5b85fdfd8eb7ca17b77d02dcdac05
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157046"
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Özellikler
Aşağıdaki özellikler, Azure AD Domain Services yönetilen etki alanlarında kullanılabilir.

* **Basit Dağıtım deneyimi:** kullanarak yalnızca birkaç tıklamayla Azure AD dizininiz için Azure AD Domain Services'ı etkinleştirebilirsiniz. Yönetilen etki alanınıza yalnızca bulutta yer alan kullanıcı hesapları ve bir şirket içi dizininizden eşitlenmiş kullanıcı hesapları içerir.
* **Etki alanına katılma için destek:** yönetilen etki alanınıza kullanılabilir Azure sanal ağındaki etki alanına katılım bilgisayarları kolayca yapabilirsiniz. Windows istemci ve sunucu işletim sistemlerinde çalışan sorunsuz bir şekilde Azure AD Domain Services tarafından hizmet verilen etki alanlarına karşı etki alanına katılım deneyimi. Otomatik etki alanına katılma gibi etki alanlarına karşı araçları da kullanabilirsiniz.
* **Azure AD dizini başına bir etki alanı örneği:** her Azure AD dizini için tek bir Active Directory etki alanı oluşturabilirsiniz.
* **Özel adlara sahip etki alanları oluşturun:** özel adları (örneğin, ' contoso100.com') ile Azure AD Domain Services'ı kullanarak etki oluşturabilirsiniz. Her iki doğrulanmış veya doğrulanmamış etki alanı adlarını kullanabilirsiniz. İsteğe bağlı olarak, bir etki alanı ile yerleşik etki alanı soneki oluşturabilirsiniz (diğer bir deyişle, ' *. onmicrosoft.com'), Azure AD directory tarafından sunulan.
* **Azure AD ile tümleştirilmiş:** yapılandırmak veya Azure AD Domain Services için çoğaltmayı yönetmek gerekmez. Azure AD Etki Alanı Hizmetleri'nde otomatik olarak kullanıcı hesapları ve grup üyelikleri kullanıcı kimlik bilgilerini (parola) Azure AD dizininizi kullanılabilir. Yeni kullanıcıların, grupların veya öznitelikleri değişiklikler Azure AD kiracınızda veya şirket içi dizininizi Azure AD Domain Services için otomatik olarak eşitlenir.
* **NTLM ve Kerberos kimlik doğrulaması:** NTLM ve Kerberos kimlik doğrulaması için destek ile Windows-Integrated kimlik doğrulamasını kullanan uygulamalar dağıtabilirsiniz.
* **Şirket kimlik bilgilerini/parolalar kullanın:** kullanıcıların Azure ad parolalarını Kiracı Azure AD Domain Services ile çalışma. Kullanıcılar, etki alanına katılım makinelere şirket kimlik bilgilerini kullanın, etkileşimli olarak veya Uzak Masaüstü üzerinden oturum açın ve yönetilen etki alanına göre kimlik doğrulaması.
* **LDAP bağı & LDAP okuma desteği:** Azure AD Domain Services tarafından hizmet verilen etki alanlarındaki kullanıcılar kimlik doğrulaması için LDAP bağlamalar kullanan uygulamaları kullanabilir. Ayrıca, LDAP okuma işlemleri için sorgu kullanıcı/bilgisayar öznitelikleri dizinden kullanan uygulamalar Azure AD Domain Services karşı çalışabilir.
* **Güvenli LDAP (LDAPS):** dizinine erişimi güvenli LDAP (LDAPS üzerinden) etkinleştirebilirsiniz. Güvenli LDAP erişimini, varsayılan olarak sanal ağ içinde kullanılabilir. Bununla birlikte, internet üzerinden güvenli LDAP erişimini Ayrıca isteğe bağlı olarak etkinleştirebilirsiniz.
* **Grup İlkesi:** kullanıcılar ve bilgisayarlar için tek bir yerleşik GPO'yu her kullanabileceğiniz kapsayıcılar ile uyumluluğu zorlamak için kullanıcı hesapları ve etki alanına katılmış bilgisayarlar için güvenlik ilkelerini gerekli. Ayrıca kendi özel GPO'ları oluşturmak ve özel kuruluş birimlerine atamanız [grup ilkesini yönetmenize](active-directory-ds-admin-guide-administer-group-policy.md).
* **DNS Yönetimi:** 'AAD DC Administrators' grubunun üyeleri, DNS Yönetim MMC ek bileşeninde gibi tanıdık DNS yönetim araçlarını kullanarak yönetilen etki alanınız için DNS yönetebilir.
* **Özel kuruluş birimine (OU) oluşturun:** 'AAD DC Administrators' grubunun üyeleri, yönetilen etki alanında özel OU'ları oluşturabilirsiniz. Bunlar Ekle/Kaldır böylece hizmet hesaplarını, bilgisayarlar, gruplar vb. Bu özel OU içinde bu kullanıcılar özel OU'ları üzerinde tam yönetimsel ayrıcalıklara verilir.
* **Birçok Azure küresel bölgelerde kullanılabilir:** bkz [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services/) Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için sayfa.
* **Yüksek Kullanılabilirlik:** Azure AD Domain Services etki alanınız için yüksek kullanılabilirlik sunar. Bu özellik, daha yüksek hizmet çalışma süresi ve hatalarına dayanıklılık garantisi sunar. Yerleşik sistem durumu izleme teklifler arızalarına karşı düzeltme başarısız örnekleri değiştirmek ve etki alanınız için sürekli hizmeti sağlamak amacıyla yeni örnekleri hale getirerek otomatik.
* **AD hesap kilitleme korumasına:** kullanıcı hesapları kilitli 30 dakika boyunca beş geçersiz parolaların 2 dakika içinde kullanılması durumunda. 30 dakika sonra kilidi otomatik olarak hesaplarıdır.
* **Tanıdık yönetim araçlarını kullanın:** yönetilen etki alanlarını yönetmek için Active Directory Yönetim Merkezi'ni veya Active Directory PowerShell gibi alışık olduğunuz Windows Server Active Directory Yönetim Araçları'nı kullanabilirsiniz.
