---
title: "Azure AD Self Servis parola sıfırlama genel bakış | Microsoft Docs"
description: "Kuruluşunuz için hangi can Azure AD Self Servis parola sıfırlama musunuz?"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: f577ee95c34f9bc3065c026bc943d30622b9d658
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD Self Servis parola sıfırlama için BT Uzmanı

Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) ne zaman ve nerede için nasıl parolalarını sıfırlama denetim Yöneticiler verirken gereksinim duydukları parolalarını kendi başlarına bağlanmasını sağlar. Kullanıcılar artık yalnızca parolasını sıfırlamak için yardım masasına çağırmanız gerekir.

* **Self Servis parola değişikliği** – kullanıcının parolasını bilir, ancak yeni bir şey için değiştirmek istiyor.
* **Self Servis parola sıfırlama** – kullanıcı oturum açamıyor ve bir veya daha fazla aşağıdaki doğrulanan kimlik doğrulama yöntemlerini kullanarak kendi parolasını sıfırlamak istiyor.
   * Doğrulanmış bir cep telefonuna SMS mesajı
   * Doğrulanmış bir cep telefonu numarası veya office telefona telefon araması
   * Doğrulanmış ikincil e-posta hesabı için e-posta
   * Güvenlik sorularını yanıtlar
* **Self Servis hesabının kilidini** – kullanıcı kurulamıyor parolalarını oturum kilitlendi ve kendi kimlik doğrulama yöntemlerini kullanarak yönetici müdahalesi olmadan hesaplarının kilidini istersiniz.

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Neden Azure seçin AD Self Servis parola sıfırlama

* **Maliyetleri azaltmak** Yardım Masası Yardımlı parola hesabı % 20'sini BT kuruluşun destek aramaları için sıfırlar. 
* **Son kullanıcı deneyimleri geliştirmek** ve **Yardım Masası miktarının azaltılmasını** son kullanıcıların kendi parola aynı anda bir Yardım Masası çağırma veya bir destek isteği açma çözümlenebileceği güç vererek.
* **Mobility sürücü** kullanıcılar oldukları yerlerde gelen parolalarını sıfırlayabilir gibi.
* **Denetimi korumak** güvenlik ilkesinin. Yöneticiler, bugün sahip oldukları ilkelerini zorlamak devam edebilirsiniz.

Hazır olduğunuzda, Azure AD SSPR'yi ile başlayabilirsiniz kullanarak bizim [Hızlı Başlangıç Kılavuzu](active-directory-passwords-getting-started.md) ve hızlı bir şekilde kullanıcılarınızın kendi parolalarını sıfırlama sahip.

## <a name="azure-ad-self-service-password-reset-availability"></a>Kullanılabilirlik Azure AD Self Servis parola sıfırlama

Azure AD Self Servis parola sıfırlama, aboneliğinize bağlı olarak üç katmanı de vardır.

* **Azure AD serbest** – yalnızca bulut yöneticiler kendi parolalarını sıfırlayın.
* **Azure AD temel** veya tüm **Ücretli Office 365 aboneliği** – yalnızca bulut kullanıcıların kendi parolalarını sıfırlayın.
* **Azure AD Premium** – herhangi bir kullanıcı veya yönetici salt bulut dahil olmak üzere, Federasyon veya parola kullanıcılar eşitlenmiş, kendi parolalarını sıfırlayabilir. Şirket içi parolaları parola geri yazma etkin olmasını gerektirir.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD fiyatlandırma, SLA, güncelleştirmeleri ve yol haritası

İzleyen siteler lisans, fiyatlandırma ve gelecekteki planları hakkında daha fazla ayrıntı bulunabilir.

* [**Azure AD fiyatlandırma ayrıntıları**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Office 365 fiyatlandırma**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Azure hizmet düzeyi sözleşmeleri**](https://azure.microsoft.com/support/legal/sla/)
* [**Microsoft Çevrimiçi Hizmetler için hizmet düzeyi sözleşmesi**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure güncelleştirmeleri**](https://azure.microsoft.com/updates/)
* [**Azure yol haritası**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Sonraki adımlar

* SSPR ile çalışmaya başlamak hazır mısınız? [Azure AD Kurulumu self servis parola sıfırlama](active-directory-passwords-getting-started.md).
* Bulunan yönergeleri kullanarak kullanıcılarınıza başarılı bir SSPR dağıtım planlama bizim [ **sunum Kılavuzu**](active-directory-passwords-best-practices.md).
* [Sıfırlama veya parolanızı değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self Servis parola sıfırlama için kaydetme](active-directory-passwords-reset-register.md).