---
title: "Azure Active Directory etki alanı Hizmetleri: Özellikleri | Microsoft Docs"
description: "Azure Active Directory etki alanı Hizmetleri özellikleri"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 8005be7ded6ea005af086aeaf594963a5f2d4ac2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Özellikler
Aşağıdaki özellikler Azure AD etki alanı Hizmetleri yönetilen etki alanlarında kullanılabilir.

* **Basit Dağıtım deneyimi:** yalnızca birkaç tıklama kullanarak Azure AD kiracınız için Azure AD Etki Alanı Hizmetleri'ni etkinleştirebilirsiniz. Azure AD kiracınıza bulut Kiracı olup şirket içi dizininize ile eşitlenen bağımsız olarak, yönetilen etki alanınızı hızlı bir şekilde sağlanabilir.
* **Etki alanına katılma için destek:** yönetilen etki alanınızı sağlanmıştır Azure sanal ağındaki etki alanına katılma bilgisayarları kolayca. Windows istemci ve sunucu işletim sistemleri works sorunsuz bir şekilde Azure AD etki alanı Hizmetleri tarafından hizmet verilen etki alanları karşı etki alanına katılım deneyimi. Bu tür etki alanlarına yönelik tooling otomatik etki alanına katılma de kullanabilirsiniz.
* **Azure AD dizini başına tek bir etki alanı örnek:** her Azure AD dizini için tek bir Active Directory etki alanı oluşturabilirsiniz.
* **Özel adlara sahip etki alanları oluşturun:** özel adları (örneğin, ' contoso100.com') olan Azure AD Etki Alanı Hizmetleri'ni kullanarak etki alanları oluşturabilirsiniz. Her iki doğrulanmış veya doğrulanmamış etki alanı adlarını kullanabilirsiniz. İsteğe bağlı olarak, bir etki alanının yerleşik etki alanı sonekiyle oluşturabilirsiniz (diğer bir deyişle, ' *. onmicrosoft.com') Azure AD dizininizi tarafından sunulan.
* **Azure AD ile tümleşik:** yapılandırmak veya Azure AD etki alanı Hizmetleri çoğaltma yönetmek gerekmez. Kullanıcı hesapları, grup üyelikleri ve kullanıcı kimlik bilgilerini (Parolalar) Azure AD dizininizi Azure AD Etki Alanı Hizmetleri'nde otomatik olarak kullanılabilir. Yeni kullanıcıların, grupların veya özniteliklere yapılan değişiklikleri Azure AD kiracınıza veya şirket içi dizininizi Azure AD etki alanı Hizmetleri için otomatik olarak eşitlenir.
* **NTLM ve Kerberos kimlik doğrulaması:** NTLM ve Kerberos kimlik doğrulaması için desteği, Windows tümleşik kimlik doğrulamasını kullanan uygulamalar dağıtabilirsiniz.
* **Kurumsal kimlik bilgileri/parolalarınızı kullanın:** kullanıcıların Azure ad parolalarını Azure AD etki alanı Hizmetleri ile iş Kiracı. Kullanıcıların etki alanına katılma makinelere şirket kimlik bilgilerini kullanmak, etkileşimli olarak veya Uzak Masaüstü üzerinden oturum açın ve yönetilen etki alanına göre kimlik doğrulaması.
* **LDAP bağ & LDAP okuma desteği:** LDAP bağlamalar Azure AD etki alanı Hizmetleri tarafından hizmet verilen etki alanlarındaki kullanıcılar kimlik doğrulaması kullanan uygulamalar kullanabilirsiniz. Ayrıca, LDAP okuma işlemleri için sorgu kullanıcı/bilgisayar öznitelikleri dizinden kullanan uygulamalar ayrıca Azure AD Etki Alanı Hizmetleri'ne karşı çalışabilirsiniz.
* **Güvenli LDAP (LDAPS):** dizinine erişimi güvenli LDAP (LDAPS üzerinden) etkinleştirebilirsiniz. Güvenli LDAP erişim varsayılan olarak sanal ağ içinde kullanılabilir. Ancak, güvenli LDAP erişim ayrıca isteğe bağlı olarak internet üzerinden etkinleştirebilirsiniz.
* **Grup İlkesi:** kullanıcılar ve bilgisayarlar için tek bir yerleşik GPO'yu her kullanabileceğiniz kullanıcı hesapları ve etki alanına katılmış bilgisayarlar için güvenlik ilkeleri ile uyumluluğu zorlamak üzere kapsayıcıları gerekli. Ayrıca, kendi özel GPO oluşturma ve özel kuruluş birimlerine atamanız [grup ilkesini yönetmenize](active-directory-ds-admin-guide-administer-group-policy.md).
* **DNS yönetme:** 'AAD DC Yöneticiler' grubunun üyeleri, DNS Yönetim MMC ek bileşenini gibi bilinen DNS yönetim araçları kullanarak yönetilen etki alanınız için DNS yönetebilir.
* **Özel Kuruluş birimlerini (OU) oluşturun:** 'AAD DC Yöneticiler' grubunun üyeleri, yönetilen etki alanında özel OU'ları oluşturabilirsiniz. Bunlar Ekle/Kaldır böylece hizmet hesaplarını, bilgisayarlar, grupların vb. Bu özel OU içinde bu kullanıcıların özel OU'ları üzerinde tam yönetimsel ayrıcalıklara verilir.
* **Birden çok Azure bölgelerinde kullanılabilir:** bkz [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services/) Azure AD etki alanı Hizmetleri olduğu kullanılabilir olduğu Azure bölgelerini öğrenmek için sayfa.
* **Yüksek Kullanılabilirlik:** Azure AD etki alanı Hizmetleri etki alanınız için yüksek kullanılabilirlik sağlar. Bu özellik yüksek hizmeti çalışır durumda kalma süresi ve esnekliği hatalarına garanti sunar. Yerleşik durum teklifleri izleme hatalardan düzeltme başarısız örnekleri değiştirmek için ve etki alanınız için sürekli hizmet sağlamak için yeni örnekleri oluşturan dönen tarafından otomatik.
* **Tanıdık yönetim araçlarını kullanın:** yönetilen etki alanlarını yönetmek için Active Directory Yönetim Merkezi'ni veya Active Directory PowerShell gibi bilinen Windows Server Active Directory yönetim araçlarını kullanabilirsiniz.
