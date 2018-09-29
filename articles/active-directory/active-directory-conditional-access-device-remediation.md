---
title: Azure portalında bir Windows cihazından bu uygulamaya buradan erişemezsiniz | Microsoft Docs
description: Bu uygulamaya buradan erişemezsiniz iletisinin nereden geldiği ve bu iletişim kutusunun görünmesini engellemek için hangi denetimleri yapabileceğiniz hakkında bilgi edinin.
services: active-directory
keywords: cihaz temelli koşullu erişim, cihaz kaydı, cihaz kaydını etkinleştirme, cihaz kaydı ve MDM
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 31e9e6343b8359de40eaac5238058e1bd2bbe692
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063889"
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a>Bir Windows cihazında bu uygulamaya buradan erişemezsiniz

Örneğin, kuruluşunuzun SharePoint Online intranetine erişim denemesi sırasında, *Oraya buradan ulaşamazsınız* ifadesini içeren bir sayfa ile karşılaşabilirsiniz. Bu sayfayı görmenizin nedeni, yöneticinizin bazı koşullarda kuruluşunuza ait kaynaklara erişimi engelleyen bir koşullu erişim ilkesi yapılandırmış olmasıdır. Bu sorunu çözmek için yardım masası veya yöneticinize başvurmanız gerekli olabilse de, ilk olarak kendi başınıza deneyebileceğiniz birkaç çözüm vardır.

**Windows** cihaz kullanıyorsanız aşağıdakileri denetlemeniz gerekir:

- Desteklenen bir tarayıcı mı kullanıyorsunuz?

- Cihazınızda desteklenen bir Windows sürümü çalıştırıyor musunuz?

- Cihazınız uyumlu mu?






## <a name="supported-browser"></a>Desteklenen tarayıcı

Yöneticiniz bir koşullu erişim ilkesi yapılandırdıysa, kuruluşunuzun kaynaklarına yalnızca desteklenen bir tarayıcı kullanarak erişebilirsiniz. Bir Windows cihazda yalnızca **Internet Explorer** ve **Edge** desteklenir.

Hata sayfasının ayrıntılar bölümüne bakarak, bir kaynağa erişememe nedeninizin desteklenmeyen tarayıcı olup olmadığını kolayca belirleyebilirsiniz:

![Desteklenmeyen tarayıcılar için "Oraya buradan ulaşamazsınız" iletisi](./media/active-directory-conditional-access-device-remediation/02.png "Senaryo")

Tek düzeltme seçeneği, cihaz platformunuz için uygulamanın desteklediği bir tarayıcı kullanılmasıdır. Desteklenen tarayıcıların tam listesi için bkz. [desteklenen tarayıcılar](active-directory-conditional-access-supported-apps.md).  


## <a name="supported-versions-of-windows"></a>Desteklenen Windows sürümleri

Cihazınızda Windows işletim sistemi ile ilgili aşağıdakilerin doğru olması gerekir: 

- Cihazınızda bir Windows masaüstü işletim sistemi çalıştırıyorsanız, sürümü Windows 7 veya üzeri olmalıdır.
- Cihazınızda bir Windows sunucu işletim sistemi çalıştırıyorsanız, sürümü Windows Server 2008 R2 veya üzeri olmalıdır. 


## <a name="compliant-device"></a>Uyumlu cihaz

Yöneticiniz, kuruluşunuzun kaynaklarına yalnızca uyumlu cihazlardan erişim izni veren bir koşullu erişim ilkesi yapılandırmış olabilir. Cihazınızın uyumlu olması için şirket içi Active Directory’nize katılmış veya Azure Active Directory’nize katılmış olması gerekir.

Hata sayfasının ayrıntılar bölümüne bakarak, bir kaynağa erişememe nedeninizin uyumlu olmayan bir cihaz olup olmadığını kolayca belirleyebilirsiniz:
 
![Kaydedilmemiş cihazlar için "Oraya buradan ulaşamazsınız" iletileri](./media/active-directory-conditional-access-device-remediation/01.png "Senaryo")


### <a name="is-your-device-joined-to-an-on-premises-active-directory"></a>Cihazınız şirket içi Active Directory'ye katılmış mı?

**Cihazınız, şirket içi Active Directory’ye katılmışsa:**

1. Windows'da iş hesabınızı (Active Directory hesabınız) kullanarak oturum açtığınızdan emin olun.
2. Kurumsal ağınıza bir sanal özel ağ (VPN) veya DirectAccess aracılığıyla bağlanın.
3. Bağlandıktan sonra, Windows oturumunuzu kilitlemek için Windows logosu tuşu ile birlikte L tuşuna basın.
4. İş hesabınızın kimlik bilgilerini girerek Windows oturumunuzun kilidini açın.
5. Bir dakika bekleyin ve ardından uygulamaya veya hizmete tekrar erişmeyi deneyin.
6. Aynı sayfayı görürseniz **More details** (Diğer ayrıntılar) bağlantısına tıklayın ve ardından, bu ayrıntılarla birlikte yöneticinizle iletişime geçin.


### <a name="is-your-device-not-joined-to-an-on-premises-active-directory"></a>Cihazınız şirket içi Active Directory'ye katılmamış mı?

Cihazınız şirket içi Active Directory'ye katılmamışsa ve Windows 10 çalıştırıyorsa, iki seçeneğiniz vardır:

* Azure AD'ye Katılım’ı çalıştırmak
* Windows'a iş veya okul hesabınızı eklemek

Bu iki seçenek arasındaki farklar hakkında bilgi edinmek için bkz. [Çalışma alanınızda Windows 10 cihazlarını kullanma](active-directory-azureadjoin-windows10-devices.md).  
Cihazınız:

- Kuruluşunuza aitse Azure AD Join’i çalıştırmanız gerekir.
- Kişisel bir cihaz veya bir Windows phone ise, iş veya okul hesabınızı Windows’a eklemeniz gerekir 



#### <a name="azure-ad-join-on-windows-10"></a>Windows 10’da Azure AD Join

Cihazınızı Azure AD’ye ekleme adımları, çalıştırdığınız Windows 10 sürümüne bağlıdır. Windows 10 işletim sisteminizin sürümünü belirlemek için **winver** komutunu çalıştırın: 

![Windows sürümü](./media/active-directory-conditional-access-device-remediation/03.png )


**Windows 10 Yıldönümü Güncelleştirmesi (Sürüm 1607):**

1. **Ayarlar** uygulamasını başlatın.
2. **Hesaplar** > **İş veya okul erişimi** öğesine tıklayın.
3. **Bağlan**'a tıklayın.
4. **Bu cihazı Azure AD'ye ekle**'ye tıklayın.
5. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
6. Oturumu kapatın ve iş hesabınızla oturum açın.
7. Uygulamaya erişmeyi tekrar deneyin.

**Windows 10 Kasım 2015 Güncelleştirmesi (Sürüm 1511):**

1. **Ayarlar** uygulamasını başlatın.
2. **Sistem** > **Hakkında** öğesine tıklayın.
3. **Azure AD'ye Katıl**’a tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. Oturumu kapatın ve ardından iş hesabınızla (Azure AD hesabınız) oturum açın.
6. Uygulamaya erişmeyi tekrar deneyin.


#### <a name="workplace-join-on-windows-81"></a>Windows 8.1’de Workplace Join

Cihazınız etki alanına katılmamışsa ve Windows 8.1 çalıştırıyorsa Workplace Join özelliğini kullanmak ve Microsoft Intune'a kaydolmak için şu adımları uygulayın:

1. **PC Ayarları**'nı açın.
2. **Ağ** > **Çalışma Alanı**’na tıklayın.
3. **Katıl**’a tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. **Aç**’a tıklayın.
6. Uygulamaya erişmeyi tekrar deneyin.



#### <a name="add-your-work-or-school-account-to-windows"></a>Windows'a iş veya okul hesabınızı eklemek 


**Windows 10 Yıldönümü Güncelleştirmesi (Sürüm 1607):**

1. **Ayarlar** uygulamasını başlatın.
2. **Hesaplar** > **İş veya okul erişimi** öğesine tıklayın.
3. **Bağlan**'a tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. Uygulamaya erişmeyi tekrar deneyin.


**Windows 10 Kasım 2015 Güncelleştirmesi (Sürüm 1511):**

1. **Ayarlar** uygulamasını başlatın.
2. **Hesaplar** > **Hesaplarınız** öğesine tıklayın.
3. **İş veya okul hesabı ekle**’ye tıklayın.
4. Kuruluşunuzun kimliğini doğrulayın, istenirse çok faktörlü kimlik doğrulaması sağlayın ve gösterilen adımları uygulayın.
5. Uygulamaya erişmeyi tekrar deneyin.





## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory koşullu erişimi](active-directory-conditional-access-azure-portal.md)

