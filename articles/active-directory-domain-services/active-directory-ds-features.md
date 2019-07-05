---
title: 'Azure Active Directory etki alanı Hizmetleri: Özellikler | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri özellikleri
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: e49a37ec95a8cf26a2c63bd90759da35fc537e41
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67474237"
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Özellikler
Aşağıdaki özellikler, Azure AD Domain Services yönetilen etki alanlarında kullanılabilir.

* **Basit Dağıtım deneyimi:** Yalnızca birkaç tıklamayla kullanarak Azure AD dizininiz için Azure AD Domain Services etkinleştirebilirsiniz. Yönetilen etki alanınıza yalnızca bulutta yer alan kullanıcı hesapları ve bir şirket içi dizininizden eşitlenmiş kullanıcı hesapları içerir.
* **Etki alanına katılma için destek:** Yönetilen etki alanınıza kullanılabilir Azure sanal ağındaki etki alanına katılım bilgisayarları kolayca kullanabilirsiniz. Windows istemci ve sunucu işletim sistemlerinde çalışan sorunsuz bir şekilde Azure AD Domain Services tarafından hizmet verilen etki alanlarına karşı etki alanına katılım deneyimi. Otomatik etki alanına katılma gibi etki alanlarına karşı araçları da kullanabilirsiniz.
* **Azure AD dizini başına bir etki alanı örneği:** Her Azure AD dizini için tek bir Active Directory etki alanı oluşturabilirsiniz.
* **Etki alanları, özel adlarla oluşturun:** Azure AD Domain Services'ı kullanarak etki alanları (örneğin, ' contoso100.com') özel adlarla oluşturabilirsiniz. Her iki doğrulanmış veya doğrulanmamış etki alanı adlarını kullanabilirsiniz. İsteğe bağlı olarak, bir etki alanı ile yerleşik etki alanı soneki oluşturabilirsiniz (diğer bir deyişle, ' *. onmicrosoft.com'), Azure AD directory tarafından sunulan.
* **Azure AD ile tümleştirilmiş:** Azure AD Domain Services için çoğaltmayı yönetme veya yapılandırma gerekmez. Azure AD Etki Alanı Hizmetleri'nde otomatik olarak kullanıcı hesapları ve grup üyelikleri kullanıcı kimlik bilgilerini (parola) Azure AD dizininizi kullanılabilir. Yeni kullanıcıların, grupların veya öznitelikleri değişiklikler Azure AD kiracınızda veya şirket içi dizininizi Azure AD Domain Services için otomatik olarak eşitlenir.
* **NTLM ve Kerberos kimlik doğrulaması:** NTLM ve Kerberos kimlik doğrulaması için destek ile Windows-Integrated kimlik doğrulamasını kullanan uygulamalar dağıtabilirsiniz.
* **Kurumsal kimlik bilgileri/parolalarınızı kullanın:** Azure AD kiracınızdaki kullanıcıların parolalarını Azure AD Domain Services ile çalışır. Kullanıcılar, etki alanına katılım makinelere şirket kimlik bilgilerini kullanın, etkileşimli olarak veya Uzak Masaüstü üzerinden oturum açın ve yönetilen etki alanına göre kimlik doğrulaması.
* **LDAP bağı & LDAP destek okuyun:** Azure AD Domain Services tarafından hizmet verilen etki alanlarındaki kullanıcılar kimlik doğrulaması için LDAP bağlamalar kullanan uygulamaları kullanabilir. Ayrıca, LDAP okuma işlemleri için sorgu kullanıcı/bilgisayar öznitelikleri dizinden kullanan uygulamalar Azure AD Domain Services karşı çalışabilir.
* **Güvenli LDAP (LDAPS):** Güvenli LDAP (LDAPS üzerinden) dizinine erişim etkinleştirebilirsiniz. Güvenli LDAP erişimini, varsayılan olarak sanal ağ içinde kullanılabilir. Bununla birlikte, internet üzerinden güvenli LDAP erişimini Ayrıca isteğe bağlı olarak etkinleştirebilirsiniz.
* **Grup İlkesi:** Kullanıcılar ve bilgisayarlar için tek bir yerleşik GPO'yu her kullanabileceğiniz kapsayıcılar ile uyumluluğu zorlamak için kullanıcı hesapları ve etki alanına katılmış bilgisayarlar için güvenlik ilkelerini gerekli. Ayrıca kendi özel GPO'ları oluşturmak ve özel kuruluş birimlerine atamanız [grup ilkesini yönetmenize](manage-group-policy.md).
* **DNS Yönetimi:** 'AAD DC Administrators' grubunun üyeleri, DNS Yönetim MMC ek bileşeninde gibi tanıdık DNS yönetim araçlarını kullanarak yönetilen etki alanınız için DNS yönetebilirsiniz.
* **Özel kuruluş birimine (OU) oluşturun:** 'AAD DC Administrators' grubunun üyeleri özel OU'ları yönetilen etki alanında oluşturabilirsiniz. Bunlar Ekle/Kaldır böylece hizmet hesaplarını, bilgisayarlar, gruplar vb. Bu özel OU içinde bu kullanıcılar özel OU'ları üzerinde tam yönetimsel ayrıcalıklara verilir.
* **Birçok Azure küresel bölgelerde kullanılabilir:** Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.
* **Yüksek Kullanılabilirlik:** Azure AD etki alanı Hizmetleri etki alanınız için yüksek kullanılabilirlik sunar. Bu özellik, daha yüksek hizmet çalışma süresi ve hatalarına dayanıklılık garantisi sunar. Yerleşik sistem durumu izleme teklifler arızalarına karşı düzeltme başarısız örnekleri değiştirmek ve etki alanınız için sürekli hizmeti sağlamak amacıyla yeni örnekleri hale getirerek otomatik.
* **AD hesap kilitleme korumasına:** Beş geçersiz parolaların 2 dakika içinde kullanılan kullanıcı hesaplarını 30 dakika boyunca kilitlidir. 30 dakika sonra kilidi otomatik olarak hesaplarıdır.
* **Tanıdık yönetim araçlarını kullanın:** Yönetilen etki alanlarını yönetmek için Active Directory Yönetim Merkezi'ni veya Active Directory PowerShell gibi tanıdık Windows Server Active Directory Yönetim Araçları'nı kullanabilirsiniz.
