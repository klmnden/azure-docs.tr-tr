---
title: Temel ilke Hizmet Yönetimi (Önizleme) - Azure Active Directory için MFA gerekir
description: Azure Resource Manager için MFA gerektirmek için koşullu erişim ilkesi
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24b54a3645fe97903219841dd148c0942dfcda76
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112380"
---
# <a name="baseline-policy-require-mfa-for-service-management-preview"></a>Temel ilke: MFA istemek için Hizmet Yönetimi (Önizleme)

Kuruluşunuzdaki çeşitli Azure hizmetlerini kullanıyor olabilir. Bu hizmetler, Azure Resource Manager API'si yönetilebilir:

* Azure portal
* Azure PowerShell
* Azure CLI

Hizmetlerinizi yönetmek üzere Azure Resource Manager kullanarak üst düzeyde ayrıcalıklı bir işlemdir. Azure Resource Manager, Kiracı genelindeki yapılandırmalar, hizmet ayarları ve abonelik faturalama gibi değiştirebilirsiniz. Tek faktörlü kimlik doğrulaması, kimlik avı ve parola ilaç gibi saldırıları çeşitli savunmasızdır. Bu nedenle, erişime izin vermeden önce çok faktörlü kimlik doğrulaması gerektirerek yapılandırmaları, güncelleştirme ve Azure Resource Manager'a erişmek isteyen kullanıcıların kimliğini doğrulamak önemlidir.

**Hizmet Yönetimi için mfa'yı gerekli** olduğu bir [temel ilke](concept-baseline-protection.md) Azure portalı, Azure PowerShell veya Azure CLI erişen herhangi bir kullanıcı için mfa'yı gerekebilir. Bu ilke, bunlar yöneticisiyseniz bağımsız olarak, Azure Resource Manager, erişen tüm kullanıcılar için geçerlidir.

Bu ilke, bir kiracıda etkinleştirildikten sonra Azure yönetim kaynakları açan tüm kullanıcılar multi-Factor authentication ile sınanır. MFA için kullanıcı kayıtlı değilse, kullanıcı devam etmek için Microsoft Authenticator uygulamasını kullanarak kaydolmanız gerekir.

![Azure Resource Manager için MFA gerektirme](./media/howto-baseline-protect-azure/baseline-policy-require-mfa-for-service-management.png)

Etkileşimli oturum açma kullanarak gerçekleştirmek için [Azure Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps), kullanın [Connect AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet'i.

```PowerShell
Connect-AzAccount
```

Bu cmdlet çalıştırıldığında bir belirteç dizesi sunar. Oturum açmak için bu dizesini kopyalayın ve yapıştırın [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin)  bir tarayıcıda. Azure’a bağlanmak için PowerShell oturumunuzun kimliği doğrulanır.

Etkileşimli oturum açma kullanarak gerçekleştirmek için [Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)çalıştırın [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) komutu.

```azurecli
az login
```

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, tarayıcı sayfasını açın ve ayrıldıktan sonra bir yetkilendirme kodu girmek için komut satırında yönergeleri için ihtiyaç duyduğunuz [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) tarayıcınızda. Daha sonra tarayıcıda hesabı kimlik bilgilerinizle oturum açın.

## <a name="deployment-considerations"></a>Dağıtma konuları

Çünkü **hizmet yönetimi için MFA gerektiren** İlkesi tüm Azure Resource Manager kullanıcıları için geçerlidir, çeşitli konuları sorunsuz bir dağıtım sağlamak için yapılması gerekir. Kullanıcılar ve uygulamalar ve modern kimlik doğrulamayı desteklemeyen, kuruluşunuz tarafından kullanılan istemcilerin yanı sıra MFA'yı gerçekleştirmemelisiniz veya Azure AD'de hizmet ilkeleri tanımlayan bu konuları içerir.

### <a name="user-exclusions"></a>Kullanıcı dışlamaları

Bu temel ilke kullanıcılar dışında seçeneği sağlar. Kiracınız için ilke etkinleştirmeden önce aşağıdaki hesapları hariç öneririz:

* **Acil Durum erişim** veya **sonu cam** Kiracı genelinde hesap kilitleme önlemek için hesaplar. Tüm Yöneticiler, kiracınızın dışında kilitli olduğundan olası senaryoda, Acil Durum erişimi yönetici hesabınızın erişim kurtarmak için Kiracı alma adımları oturum kullanılabilir.
   * Daha fazla bilgi makalesinde bulunabilir [Azure AD'de Acil Durum erişim hesapları yönetme](../users-groups-roles/directory-emergency-access.md).
* **Hizmet hesapları** ve **servis ilkeleri**, Azure AD Connect eşitleme hesabı gibi. Hizmet, belirli bir kullanıcıya bağlı değil, etkileşimli olmayan hesaplar hesaplarıdır. Bunlar genellikle arka uç Hizmetleri tarafından kullanılan ve uygulamalar için programlı erişim izni. Hizmet hesapları, MFA programlı bir şekilde tamamlanamıyor beri hariç tutulması gerekir.
   * Kuruluşunuz, betikleri veya kodları kullanımda bu hesapları varsa, bunları ile değiştirmeyi göz önüne alın [yönetilen kimlikleri](../managed-identities-azure-resources/overview.md). Geçici bir çözüm, bu belirli hesapların temel ilkesinden hariç tutabilirsiniz.
* Sahip değil veya akıllı telefonunuz kullanmanız mümkün olmayacaktır kullanıcılar.
   * Bu ilke, kullanıcıların MFA için Microsoft Authenticator uygulamasını kullanarak kaydolmasını gerektirir.

## <a name="enable-the-baseline-policy"></a>Temel ilke etkinleştir

İlke **temel ilke: Hizmet Yönetimi (Önizleme) için mfa'yı gerekli** önceden yapılandırılmış olarak gelir ve Azure portalında koşullu erişim dikey penceresine gittiğinizde en üstünde gösterilir.

Bu ilkeyi etkinleştirmek ve yöneticileriniz korumak için:

1. Oturum **Azure portalında** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.
1. Gözat **Azure Active Directory** > **koşullu erişim**.
1. İlkeler listesinde seçin **temel ilke: Hizmet Yönetimi (Önizleme) için mfa'yı gerekli**.
1. Ayarlama **ilkesini etkinleştir** için **ilkeyi hemen kullan**.
1. Herhangi bir kullanıcı özel tıklayarak Ekle **kullanıcılar** > **dışlanan kullanıcılar seçin** ve hariç tutulması gerektiğini kullanıcıları seçme. Tıklayın **seçin** ardından **Bitti**.
1. Tıklayın **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızın güvenliğini sağlamak için beş adım](../../security/azure-ad-secure-steps.md)
* [Azure Active Directory'de koşullu erişim nedir?](overview.md)