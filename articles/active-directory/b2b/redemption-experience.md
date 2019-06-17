---
title: B2B işbirliği - Azure Active Directory içinde Davetiyesi kullanımı | Microsoft Docs
description: Gizlilik koşulları sözleşmesini dahil olmak üzere, son kullanıcılar için Azure AD B2B işbirliği davet kullanım deneyimi açıklanmıştır.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: a80eaa134130195fce00ee6a4d68851e478c4532
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052504"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B işbirliği Davetiyesi kullanımı

Bu makalede, kaynaklarınızı ve bunlar karşılaşacağınız onay işlemi Konuk kullanıcılar erişebilir yolları açıklanır. Konuk davet e-posta göndermek ise davet Konuk kullanılabilecek bir bağlantı içerir. uygulamanızı veya portalına erişim elde etmek için. Davet e-posta Konukları kaynaklarınıza erişimi edinme yöntemlerinden biridir. Alternatif olarak, konuklar dizininize eklemek ve bunları portal ya da paylaşmak istediğiniz uygulamanın doğrudan bağlantısını verin. Kullandıkları yönteminden bağımsız olarak Konukları ilk kez onay sürecinde kılavuzluk edilir. Bu işlem konuklarınız gizlilik koşullarını kabul etmeniz ve tüm kabul sağlar [kullanım koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) ayarlamış olduğunuz.

Konuk kullanıcı dizininize eklemek, başlangıçta ayarlamak için onay durumunu (PowerShell'de görünebilir) Konuk kullanıcı hesabı sahip **PendingAcceptance**. Bu ayar Konuk daveti kabul eder ve kullanım koşullarını ve gizlilik ilkesini kabul edene kadar kalır. Bundan sonra onay durumu değişerek **kabul edilen**, ve onay sayfaları artık Konuk sunulur.

## <a name="redemption-through-the-invitation-email"></a>Davet e-posta ile kullanma

Dizininize göre Konuk kullanıcı eklediğinizde [Azure portalını kullanarak](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal), işlemdeki Konuk davet e-posta gönderilir. İşiniz davet e-postaları göndermek de seçebilirsiniz [PowerShell kullanarak](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell) dizininize Konuk kullanıcıları eklemek için. Bunlar e-postadaki bağlantıya şifrenizi kullandığınızda konuğun deneyimi açıklaması aşağıdadır.

1. Konuk alır bir [davet e-postası](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) gelen gönderilen **Microsoft Invitations**.
2. Konuk seçer **Başlarken** e-posta.
3. Konuk bir Azure AD hesabı, bir Microsoft hesabı (MSA) veya bir e-posta hesabı yoksa ve bir Federal kuruluş bunlar bir MSA oluşturmanız istenir (sürece [bir kerelik geçiş kodu](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) özelliği etkinleştirildiğinde, bir MSA olmasını gerektirmez ).
4. Konuk aracılığıyla destekli [deneyimi onay](#consent-experience-for-the-guest) aşağıda açıklanmıştır.

## <a name="redemption-through-a-direct-link"></a>Doğrudan bağlantı üzerinden kullanma

Davet e-postasını alternatif, uygulamanızı veya portalı için bir Konuk doğrudan bağlantısını verebilirsiniz. İlk dizininizi Konuk kullanıcı ekleme gerekir [Azure portalında](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal) veya [PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell). Herhangi birini kullanabilmeniz için sonra [kullanıcılara uygulama dağıtmak için özelleştirilebilir yol](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences), oturum açma doğrudan bağlantılar dahil olmak üzere. Konuk davet e-posta yerine doğrudan bir bağlantı kullandığında, bunlar yine de ilk kez onayı deneyimi destekli.

> [!IMPORTANT]
> Doğrudan bağlantı kiracıya özgü olmalıdır. Diğer bir deyişle, bir kiracı kimliği içermelidir veya paylaşılan app bulunduğu kiracınızda Konuk kimliğinin doğrulanabilmesi etki alanını doğruladıysanız. Genel bir URL ister https://myapps.microsoft.com için konuk kimlik doğrulaması için giriş, kiracılarının yönlendirilecek çünkü çalışmaz. Kiracı bağlamı ile doğrudan bağlantılar bazı örnekleri aşağıda verilmiştir:
 > - Uygulama erişim paneli: https://myapps.microsoft.com/?tenantid=&lt; Kiracı kimliği&gt; 
 > - Doğrulanmış bir etki alanı için erişim paneli uygulamaları: https://myapps.microsoft.com/&lt; doğrulanmış etki alanı&gt;
 > - Azure portalı: https://portal.azure.com/&lt; Kiracı kimliği&gt;
 > - Tek tek uygulama: kullanma hakkında bilgi bir [doğrudan oturum açma bağlantısı](../manage-apps/end-user-experiences.md#direct-sign-on-links)

Davet e-posta doğrudan bağlantı burada önerilen bazı durumlar vardır. Bu özel durumlar, kuruluşunuz için önemliyse, davet e-posta göndermeye devam yöntemleri kullanarak kullanıcıları davet öneririz:
 - Kullanıcı bir Federal kuruluş bir Azure AD hesabı, bir MSA veya bir e-posta hesabı yok. Bir kerelik geçiş kodu özelliği kullanmadığınız sürece bir MSA oluşturma adımlarının üzerinden gösterilmesini istiyorsanız davet e-posta kullanmak Konuk gerekir.
 - Bazen davet edilen kullanıcı nesnesi bir kişi nesnesi (örneğin, bir Outlook ilgili nesne) ile çakışması nedeniyle bir e-posta adresi olmayabilir. Bu durumda, kullanıcı davet e-posta kullanım URL'de tıklamanız gerekir.
 - Kullanıcı, davet edilen e-posta adresi diğer ad ile oturum açabilirsiniz. (Diğer bir e-posta hesabıyla ilişkili bir ek e-posta adresidir.) Bu durumda, kullanıcı davet e-posta kullanım URL'de tıklamanız gerekir.

## <a name="consent-experience-for-the-guest"></a>Konuk için onayı deneyimi

Bir konuk ilk kez bir iş ortağı kuruluşunda bulunan kaynaklara erişmek oturum açtığında aşağıdaki sayfalarla destekli. 

1. Konuk incelemeleri **izinleri gözden** davet eden kuruluşun gizlilik bildirimini tanımlayan sayfa. Bir kullanıcı gerekir **kabul** anlaşmalara uygun şekilde devam etmek için davet eden bir kuruluşun gizlilik ilkeleri, bilgileri.

   ![Gözden geçirme izinleri sayfasını gösteren ekran görüntüsü](media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > Kiracı Yöneticisi olarak, kuruluşunuzun gizlilik bildirimini oluşturmak için nasıl bağlantı hakkında daha fazla bilgi için bkz: [nasıl yapılır: Azure Active Directory'de, kuruluşunuzun gizlilik bilgisi eklemek](https://aka.ms/adprivacystatement).

2. Kullanım koşullarını yapılandırıldıysa, Konuk açar ve kullanım koşullarını inceler ve ardından seçer **kabul**. 

   ![Yeni Kullanım Koşulları'nı gösteren ekran görüntüsü](media/redemption-experience/terms-of-use-accept.png) 

   > [!NOTE]
   > Bkz: yapılandırabileceğiniz [kullanım koşullarını](../governance/active-directory-tou.md) içinde **Yönet** > **kuruluş ilişkileri** > **kullanımkoşullarını**.

3. Aksi belirtilmediği sürece, Konuk Konuk erişip uygulamaları listeler uygulama erişim paneli yönlendirilir.

   ![Uygulama erişim paneli gösteren ekran görüntüsü](media/redemption-experience/myapps.png) 

Dizininizde, Konuk kullanıcının **davet kabul edildi** değeri **Evet**. Bir MSA oluşturuldu, konuğun **kaynak** gösterir **Microsoft Account**. Konuk kullanıcı hesabı özellikleri hakkında daha fazla bilgi için bkz: [bir Azure AD B2B işbirliği kullanıcısı özelliklerini](user-properties.md). 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure portalında Azure Active Directory B2B işbirliği kullanıcıları ekleme](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcıları için Azure Active Directory nasıl eklerim?](add-users-information-worker.md)
- [PowerShell kullanarak Azure Active Directory B2B işbirliği kullanıcıları ekleme](customize-invitation-api.md#powershell)
- [Konuk kullanıcı olarak kuruluştan Ayrıl](leave-the-organization.md)
