---
title: "Azure AD Self Servis parola sıfırlama genel bakış | Microsoft Docs"
description: "Kuruluşunuz için hangi can Azure AD Self Servis parola sıfırlama musunuz?"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 074a22d0d3d6f4218be431409dcb0e516d226335
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD Self Servis parola sıfırlama için BT Uzmanı

Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) ile kullanıcılar kendi zaman ve nerede için gereksinim duydukları parolalarını sıfırlayın. Aynı zamanda, nasıl bir kullanıcının parolasını sıfırlamak Yöneticiler kontrol edebilirsiniz. Kullanıcılar artık yalnızca parolasını sıfırlamak için bir Yardım Masası çağırmanız gerekir. Azure AD SSPR'yi içerir:

* **Self Servis parola değişikliği**: kullanıcı parolasını bilir, ancak yeni bir şey için değiştirmek istiyor.
* **Self Servis parola sıfırlama**: kullanıcı oturum açamıyor ve bir veya daha fazla aşağıdaki doğrulanan kimlik doğrulama yöntemlerini kullanarak parolalarını sıfırlamak istiyor:
   * Doğrulanmış cep telefonuna SMS mesajı gönderecek.
   * Doğrulanmış bir cep telefonu numarası veya office telefon bir telefon araması yapın.
   * Doğrulanmış ikincil e-posta hesabı için bir e-posta gönderin.
   * Güvenlik sorularını yanıtlayın.
* **Self Servis hesabının kilidini**: kullanıcı kendi parolasıyla oturum açmanız ve görüntüleyemedi kilitlendi. Kullanıcı kendi kimlik doğrulama yöntemlerini kullanarak yönetici müdahalesi olmadan hesaplarının kilidini istiyor.

## <a name="why-choose-azure-ad-sspr"></a>Neden Azure AD SSPR'yi seçin

Azure AD SSPR'yi size yardımcı olur:

* **Maliyetleri azaltmak** Yardım Masası Yardımlı parola hesabı % 20'sini BT kuruluşun destek aramaları için sıfırlar. 
* **Son kullanıcı deneyimleri geliştirmek** ve **yardım masasına miktarının azaltılmasını** son kullanıcıların kendi parola sorunlarını gidermek için güç vererek. Yardım Masasını arayın veya bir destek isteği açın gerek yoktur.
* **Mobility sürücü** kullanıcılar oldukları yerlerde gelen parolalarını sıfırlayabilir gibi.
* **Denetimi korumak** güvenlik ilkesinin. Yöneticiler, bugün sahip oldukları ilkelerini zorlamak devam edebilirsiniz.

Hazır olduğunuzda, Azure AD SSPR'yi ile kullanarak başlayabiliriz bizim [Hızlı Başlangıç Kılavuzu](active-directory-passwords-getting-started.md). Kullanıcılarınızın kendi parolalarını sıfırlama olanağı için hızlı bir şekilde verebilirsiniz.

## <a name="azure-ad-sspr-availability"></a>Azure AD SSPR'yi kullanılabilirliği

Azure AD SSPR'yi aboneliğinize bağlı olarak üç katmanı bulunur:

* **Azure AD serbest**: yalnızca bulut yöneticiler kendi parolalarını sıfırlayın.
* **Azure AD temel** veya tüm **Ücretli Office 365 aboneliği**: yalnızca bulut kullanıcıların kendi parolalarını sıfırlayın.
* **Azure AD Premium**: herhangi bir kullanıcı veya yönetici salt bulut dahil olmak üzere, Federasyon veya parola kullanıcılar eşitlenmiş, kendi parolalarını sıfırlayabilir. Şirket içi parolaları parola geri yazma etkin olmasını gerektirir.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD fiyatlandırma, SLA, güncelleştirmeleri ve yol haritası

Aşağıdaki sitelerde lisans, fiyatlandırma ve gelecekteki planları hakkında daha fazla ayrıntı bulunabilir:

* [Azure AD fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/active-directory/)
* [Office 365 fiyatlandırma](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/)
* [Microsoft Çevrimiçi Hizmetler için hizmet düzeyi sözleşmesi](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/)
* [Azure yol haritası](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Sonraki adımlar

* SSPR ile çalışmaya başlamak hazır mısınız? [Self Servis Azure AD parolasını ayarla Sıfırla](active-directory-passwords-getting-started.md).
* Bulunan yönergeleri kullanarak kullanıcılarınıza başarılı bir SSPR dağıtım planlama bizim [sunum Kılavuzu](active-directory-passwords-best-practices.md).
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
