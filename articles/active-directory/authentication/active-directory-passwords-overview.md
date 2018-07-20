---
title: Azure AD Self Servis parola sıfırlama genel bakış | Microsoft Docs
description: Hangi Azure AD Self Servis parola sıfırlama yapabilir, kuruluşunuz için?
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: da63278ad3ae0ec34aa484794e9c910554da1de2
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158769"
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD Self Servis parola sıfırlama için BT Uzmanı

Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) ile kullanıcılar, kendi ve gerekli konumlara kendi parolalarını sıfırlayabilir. Aynı anda yöneticileri bir kullanıcının parolasını sıfırlama denetleyebilir. Kullanıcılar artık yalnızca kullanıcının parolasını sıfırlamak için Yardım Masasını arayın gerekmez. Azure AD SSPR içerir:

* **Self Servis parola değişimi**: kullanıcı parolasını bilir, ancak yeni bir şeyle değiştirmek istiyor.
* **Self Servis parola sıfırlama**: kullanıcı oturum açamıyor ve bir veya daha fazla aşağıdaki doğrulanmış kimlik doğrulama yöntemlerini kullanarak kullanıcının parolasını sıfırlamak istiyor:
   * Doğrulanmış bir cep telefonuna bir kısa mesaj gönderin.
   * Doğrulanmış bir cep telefonu numarası veya office telefona bir telefon araması yapın.
   * Bir ikincil doğrulanmış e-posta hesabı için bir e-posta gönderin.
   * Güvenlik sorularını yanıtlayın.
* **Self Servis hesap kilidini açma**: kullanıcı, bir parolayla oturum açmasını belirleyemez ve kilitlendi. Kullanıcı kendi kimlik doğrulama yöntemlerini kullanarak yönetici müdahalesi olmadan hesaplarının kilidini istiyor.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

## <a name="why-choose-azure-ad-sspr"></a>Neden Azure AD SSPR'ı seçin

Azure AD SSPR size yardımcı olur:

* **Maliyetleri azaltmak** Yardım Masası Yardımlı parola hesabı için bir BT kuruluşunun destek çağrılarının %20 sıfırlar. 
* **Son kullanıcı deneyimlerini iyileştirmenin** ve **Yardım Masası miktarının azaltılmasını** tarafından son kullanıcılar kendi parola sorunları gidermek için gücüne sahip olmanızı sağlar. Yardım Masasını arayın veya bir destek isteği açmak için gerek yoktur.
* **Mobility sürücü** gibi kullanıcılar parolalarını istedikleri yerden sıfırlayabilir.
* **Denetimi elinizde** güvenlik ilkesi. Yöneticiler, bugün sahip oldukları ilkelerini zorlamak devam edebilirsiniz.

Hazır olduğunuzda, Azure AD SSPR ile kullanarak oluşturabileceğinize dair bizim [hızlı başlangıç kılavuzunu](quickstart-sspr.md). Hızlı bir şekilde, kullanıcılarınıza kendi parolalarını sıfırlama olanağı verebilirsiniz.

## <a name="azure-ad-sspr-availability"></a>Azure AD SSPR kullanılabilirlik

Azure AD SSPR, aboneliğinize bağlı olarak üç katmanda kullanılabilir:

* **Azure AD ücretsiz**: yalnızca bulut yöneticileri kendi parolalarını sıfırlama.
* **Azure AD temel** veya tüm **Ücretli Office 365 aboneliği**: sadece bulutta yer alan kullanıcılar kendi parolalarını sıfırlama.
* **Azure AD Premium**: herhangi bir kullanıcı veya yönetici, yalnızca bulutta yer alan Federasyon, geçişli kimlik doğrulaması veya parola karması eşitlenmiş kullanıcılar dahil olmak üzere kendi parolalarını sıfırlayabilir. Şirket içi parolaları etkinleştirilmesi için parola geri yazma özelliğini gerektirir.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD fiyatlandırma ve SLA'sı, güncelleştirmeleri ve yol haritası

Lisans, fiyatlandırma ve gelecekteki planları hakkında daha fazla ayrıntı için aşağıdaki sitelerde bulunabilir:

* [Azure AD fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/active-directory/)
* [Office 365 fiyatlandırması](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/)
* [Microsoft Online Hizmetleri için hizmet düzeyi sözleşmesi](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/)
* [Azure yol haritası](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Sonraki adımlar

* SSPR ile başlamaya hazır mısınız? [Azure AD Self Servis parolasını ayarla sıfırlama](quickstart-sspr.md).
* Bulunan yönergeleri kullanarak kullanıcılarınıza başarılı bir SSPR dağıtım planlama bizim [Dağıtım Kılavuzu](howto-sspr-deployment.md).
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md).
