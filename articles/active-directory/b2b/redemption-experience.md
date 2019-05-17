---
title: B2B işbirliği - Azure Active Directory içinde Davetiyesi kullanımı | Microsoft Docs
description: Gizlilik koşulları sözleşmesini dahil olmak üzere, son kullanıcılar için Azure AD B2B işbirliği davet kullanım deneyimi açıklanmıştır.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: d00808295d39247729f6e843ac59ad7b23407148
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65785413"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B işbirliği Davetiyesi kullanımı

Azure Active Directory (Azure AD) B2B işbirliği aracılığıyla iş ortağı kuruluşlardan kullanıcılarla işbirliği yapmak için paylaşılan uygulamalara erişmek için konuk kullanıcılar davet edebilirsiniz. Konuk kullanıcı, kullanıcı arabirimi aracılığıyla dizinine eklenir veya PowerShell aracılığıyla kullanıcı davet sonra konuk kullanıcılar burada kullanıcının kabul etmesi için ilk kez onay sürecinde gitmelidir [gizlilik koşulları](#privacy-policy-agreement). Bu işlem aşağıdaki yollardan biriyle olur:

- Konuk davet eden, paylaşılan bir uygulamanın doğrudan bağlantısını gönderir. Davetli oturum açmak için bağlantıya tıklar, gizlilik koşullarını kabul eder ve sorunsuz bir şekilde paylaşılan kaynağa erişir. (Konuk kullanıcı hala kullanım URL içeren bir davet e-posta alır, ancak bazı özel durumlar dışında artık davet e-postayı kullanmanız zorunludur.)  
- Konuk kullanıcı davet e-posta alır ve kullanım URL tıklar. İlk kez oturum açma işleminin bir parçası olarak bunlar gizlilik koşullarını kabul etmeniz istenir.

## <a name="redemption-through-a-direct-link"></a>Doğrudan bağlantı üzerinden kullanma

Konuk davet eden göndererek Konuk kullanıcı davet edebilir bir [paylaşılan bir uygulamanın doğrudan bağlantısını](../manage-apps/end-user-experiences.md#direct-sign-on-links). Konuk kullanıcı için kullanım deneyimi kendileriyle paylaşılan uygulamasında kadar kolaydır. Bunlar uygulama bağlantısını tıklayın, gözden geçirin ve gizlilik koşullarını kabul edin ve uygulamaya sorunsuz bir şekilde erişmek. Çoğu durumda, Konuk kullanıcıları davet e-posta iletisinde kullanım URL'yi artık gerekir.

Kullanıcı arabirimi aracılığıyla Konuk kullanıcılar davet ya da PowerShell davet deneyiminin bir parçası davet e-posta göndermek seçtiğiniz, davet edilen kullanıcı yine de bir davet e-posta alır. Bu e-posta, aşağıdaki özel durumlar için kullanışlıdır:

- Kullanıcı, bir Azure AD hesabı veya Microsoft hesabı (MSA) sahip değil. Bu durumda, kullanıcı bir MSA önce bunların bağlantısını tıklattığınızda veya kullanım URL'sini davet e-postada kullanabilirsiniz oluşturmanız gerekir. Alma işlemi, otomatik olarak bir MSA oluşturmak için kullanıcıya sorar.
- Bazen davet edilen kullanıcı nesnesi bir kişi nesnesi (örneğin, bir Outlook ilgili nesne) ile çakışması nedeniyle bir e-posta adresi olmayabilir. Bu durumda, kullanıcı davet e-posta kullanım URL'de tıklamanız gerekir.
- Kullanıcı, davet edilen e-posta adresi diğer ad ile oturum açabilirsiniz. (Diğer bir e-posta hesabıyla ilişkili bir ek e-posta adresidir.) Bu durumda, kullanıcı davet e-posta kullanım URL'de tıklamanız gerekir.

Bu özel durumlar, kuruluşunuz için önemliyse, davet e-posta göndermeye devam yöntemleri kullanarak kullanıcıları davet öneririz. Bir kullanıcı bu özel durumlarından biri altında kalan değil, ayrıca, bunlar yine de erişim elde etmek için bir davet e-posta URL'de tıklayabilirsiniz.

## <a name="redemption-through-the-invitation-email"></a>Davet e-posta ile kullanma

Davet e-posta gönderen bir yöntem aracılığıyla davet, kullanıcılar ayrıca bir davet e-posta davetini. Davet edilen kullanıcıları e-posta, kullanım URL'yi tıklayın ve sonra gözden geçirin ve gizlilik koşullarını kabul edin. İşlem, aşağıda daha ayrıntılı olarak açıklanmıştır:

1.  Davet sonra davetli davetiye gönderildiği e-posta aracılığıyla alan **Microsoft Invitations**.
2.  Davetli seçer **Başlarken** e-posta.
3.  Davetli bir Azure AD hesabı veya bir MSA yoksa, bunlar bir MSA oluşturmanız istenir.
4.  Davetli yönlendireceği **izinleri gözden** ekran, burada, davet eden kuruluşun gizlilik bildirimini gözden geçirebilir ve koşullarını kabul edin.

## <a name="privacy-policy-agreement"></a>Gizlilik İlkesi sözleşmesini

İlk kez bir iş ortağı kuruluşunda bulunan kaynaklara erişmek herhangi bir Konuk kullanıcı oturum açtıktan sonra göreceği bir **izinleri gözden** ekran. Burada, davet eden kuruluşun gizlilik bildirimini gözden geçirebilirsiniz. Bir kullanıcı kendi bilgilerinin anlaşmalara uygun şekilde devam etmek için davet eden bir kuruluşun gizlilik ilkelerini kabul etmelidir.

![Erişim panelinde kullanıcı ayarları gösteren ekran görüntüsü](media/redemption-experience/ConsentScreen.png) 

Kiracı Yöneticisi olarak, kuruluşunuzun gizlilik bildirimini oluşturmak için nasıl bağlantı hakkında daha fazla bilgi için bkz: [nasıl yapılır: Azure Active Directory'de, kuruluşunuzun gizlilik bilgisi eklemek](https://aka.ms/adprivacystatement).

## <a name="terms-of-use"></a>Kullanım koşulları

Kullanım koşullarını kullanım Özelliği Azure AD koşullarını kullanarak ilk alma işlemi sırasında Konuk kullanıcıya sunabilir. Azure Active Directory'de altında bu özelliği erişebileceğiniz **Yönet** > **kuruluş ilişkileri** > **kullanım koşullarını** veya altında **Güvenlik** > **koşullu erişim** > **kullanım koşullarını**. Ayrıntılar için bkz [kullanım Özelliği Azure AD kullanım koşulları](../conditional-access/terms-of-use.md).

![Yeni Kullanım Koşulları'nı gösteren ekran görüntüsü](media/redemption-experience/organizational-relationships-terms-of-use.png) 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure portalında Azure Active Directory B2B işbirliği kullanıcıları ekleme](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcıları için Azure Active Directory nasıl eklerim?](add-users-information-worker.md)
- [PowerShell kullanarak Azure Active Directory B2B işbirliği kullanıcıları ekleme](customize-invitation-api.md#powershell)
- [Konuk kullanıcı olarak kuruluştan Ayrıl](leave-the-organization.md)
