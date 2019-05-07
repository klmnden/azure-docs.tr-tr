---
title: include dosyası
description: giriş sayfaları (arka plan programı, Web uygulaması, Web API'si) gizli istemci senaryosu için dosyası Ekle
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 9ee7422b372993d60c629524eb036b9678e5776c
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074658"
---
## <a name="registration-of-secrets-or-certificates"></a>Gizli dizileri ya da sertifika kaydı

Gibi herhangi bir gizli bir istemci uygulama için bir parola veya sertifika kayıt olmanız gerekir. Uygulama gizli anahtarlarınız etkileşimli deneyim aracılığıyla ya da kaydedebilirsiniz [Azure portalında](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview), ya da (PowerShell gibi) komut satırı araçlarını kullanarak

### <a name="registering-client-secrets-using-the-application-registration-portal"></a>Uygulama kayıt Portalı'nı kullanarak istemci gizli dizileri kaydediliyor

İçinde istemci kimlik bilgileri yönetimi olur **sertifikaları ve parolaları** sayfa bir uygulama için:

![image](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png)

- Uygulama gizli anahtarı (Ayrıca adlandırılmış istemci gizli anahtarı), gizli bir istemci uygulamasının kaydı sırasında Azure AD tarafından oluşturulur. Seçtiğiniz bu nesil olur **yeni gizli**. Bu noktada, gizli dizesini kullanmak için Panodaki uygulamanızda seçmeden önce kopyalamanız gereken **Kaydet**. Bu dize artık sunulan olmaz.
- Sertifika kaydı kullanılarak uygulama karşıya **sertifikayı karşıya yükle** düğmesi

Ayrıntılar için bkz [hızlı başlangıç: Web API'leri erişmek için bir istemci uygulamasını yapılandırmak | Kimlik bilgilerini uygulamanıza ekleme](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application)

### <a name="registering-client-secrets-using-powershell"></a>PowerShell kullanarak istemci gizli dizileri kaydediliyor

Alternatif olarak, uygulamanızı Azure AD ile komut satırı araçlarını kullanarak kaydedebilirsiniz. [Active-directory-dotnetcore-arka plan programı-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) örnek bir uygulama gizli dizisi veya bir sertifika ile Azure AD uygulaması kaydetme gösterir:

- Bir uygulama gizli anahtarı kaydetme hakkında daha fazla bilgi için bkz: [AppCreationScripts/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190)
- Bir sertifika uygulama ile kaydetme hakkında daha fazla bilgi için bkz: [AppCreationScripts-withCert/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178)