---
title: Şirket içi uygulamaları Cloud App Security - Azure Active Directory ile tümleştirin. | Microsoft Docs
description: Microsoft Cloud App Security (MCAS) ile çalışmak için Azure Active Directory'de şirket içi bir uygulamayı yapılandırın. MCAS koşullu erişim uygulaması denetimi izlemek için kullanın ve gerçek zamanlı denetim oturumları koşullu erişim ilkelerine bağlı. Azure Active Directory (Azure AD) uygulama ara sunucusu kullanan şirket içi uygulamalar için bu ilkeler uygulayabilirsiniz.
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c614d636e572eb261ec28c55ac49fec0e2b58b2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65783599"
---
# <a name="configure-real-time-application-access-monitoring-with-microsoft-cloud-app-security-and-azure-active-directory"></a>Microsoft Cloud App Security ve Azure Active Directory ile gerçek zamanlı uygulama erişim izlemeyi yapılandırma
Microsoft Cloud App Security (MCAS) kullanmak için bir şirket içi uygulamanızı Azure Active Directory'de (Azure AD), gerçek zamanlı izleme için yapılandırma. İzlemek için koşullu erişim uygulaması denetimi MCAS kullanır ve gerçek zamanlı denetim oturumları koşullu erişim ilkelerine bağlı. Azure Active Directory (Azure AD) uygulama ara sunucusu kullanan şirket içi uygulamalar için bu ilkeler uygulayabilirsiniz.

MCAS ile oluşturduğunuz ilke türleri bazı örnekleri aşağıda verilmiştir:

- Engelleme veya yönetilmeyen cihazlarda hassas belgeleri indirilmesini koruyun.
- Yüksek riskli kullanıcılar uygulamalara oturum açmak ve ardından kendi eylemlerden içinde oturumu zamana yönelik İzleyici. Bu bilgiyle oturum ilkeleri uygulamak nasıl belirlemek için kullanıcı davranışını analiz edebilirsiniz.
- İstemci sertifikaları ya da cihaz uyumluluğunu yönetilmeyen cihazlardan belirli uygulamalara erişimi engellemek için kullanın.
- Kullanıcı oturumlarını, şirket dışı ağlardan kısıtlayın. Şirket ağınızın dışından gelen bir uygulamaya erişen kullanıcılar için kısıtlı erişim verebilirsiniz. Örneğin, bu kısıtlı erişim hassas belgeler indiriliyor kullanıcının engelleyebilirsiniz.

Daha fazla bilgi için [Microsoft Cloud App Security koşullu erişim uygulaması denetimi ile uygulamaları koruma](/cloud-app-security/proxy-intro-aad).

## <a name="requirements"></a>Gereksinimler

Lisans:

- EMS E5 lisansı veya 
- Azure Active Directory Premium P1 ve MCAS tek başına.

Şirket içi uygulama:

- Şirket içi uygulama Kerberos Kısıtlı temsilci (KCD) kullanmanız gerekir

Uygulama proxy'si yapılandırın:

- Azure AD uygulama ara sunucusu bağlayıcısını yükleme ve ortamınızı hazırlama da dahil olmak üzere uygulama ara sunucusunu kullanacak şekilde yapılandırın. Bir öğretici için bkz. [Azure AD'de uygulama ara sunucusu üzerinden uzaktan erişim için bir şirket içi uygulamalara ekleme](application-proxy-add-on-premises-application.md). 

## <a name="add-on-premises-application-to-azure-ad"></a>Şirket içi uygulamanızı Azure AD'ye ekleme

Şirket içi bir uygulamayı Azure AD'ye ekleyin. Hızlı Başlangıç için bkz: [şirket içi bir uygulamayı Azure AD'ye ekleme](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). Uygulama eklerken, aşağıdaki iki ayarı ayarladığınızdan emin olun **şirket içi uygulamanızı ekleme** dikey penceresinde:

- **Ön kimlik doğrulaması**: Girin **Azure Active Directory**.
- **Uygulama gövdesindeki URL'leri**: Seçin **Evet**.

Bu iki ayar, uygulamanın MCAS ile çalışmak gereklidir.

## <a name="test-the-on-premises-application"></a>Şirket içi uygulamayı test etme

Uygulamanızı Azure AD'ye eklendikten sonra içindeki adımları kullanın [uygulamayı test etme](application-proxy-add-on-premises-application.md#test-the-application) test etmek için kullanıcı ekleme ve oturum açmayı test edin. 

## <a name="deploy-conditional-access-app-control"></a>Koşullu erişim uygulama Denetimi'ni dağıtma

Koşullu erişim uygulama denetimi ile uygulamanızı yapılandırmak için yönergeleri izleyin. [dağıtma koşullu erişim uygulama denetimi için Azure AD uygulamaları](/cloud-app-security/proxy-deployment-aad).


## <a name="test-conditional-access-app-control"></a>Test koşullu erişim uygulama denetimi

Azure AD uygulamaları koşullu erişim uygulama denetimi ile dağıtımı test etmek için'ndaki yönergeleri izleyin. [dağıtımı için Azure AD uygulamaları test etme](/cloud-app-security/proxy-deployment-aad).





