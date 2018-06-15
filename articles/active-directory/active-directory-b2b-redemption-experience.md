---
title: B2B işbirliği - Azure Active Directory içinde davet kullanım | Microsoft Docs
description: Gizlilik koşullarını anlaşmasına dahil olmak üzere son kullanıcılar için Azure AD B2B işbirliği davet kullanım deneyimi açıklanır.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 2e354bc4ae06e86afd5d14e87ef796fce942521b
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34074790"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B işbirliği davet kullanım

Azure Active Directory (Azure AD) B2B işbirliği ile iş ortağı kuruluşlardan kullanıcılarla işbirliği yapmak için paylaşılan uygulamalara erişmek için konuk kullanıcıları davet edebilirsiniz. Kullanıcı arabirimi aracılığıyla dizinine Konuk kullanıcı eklenir veya PowerShell aracılığıyla kullanıcı davet sonra konuk kullanıcılar nerede kullanıcının kabul etmesi için ilk kez onay işlemiyle gitmeniz gerekir [gizlilik koşullarını](#privacy-policy-agreement). Bu işlem aşağıdaki yollardan biriyle gerçekleşir:

- Konuk davet eden paylaşılan bir uygulamaya doğrudan bağlantı gönderir. Davet edilene oturum açmak için bağlantıya tıklar, gizlilik koşullarını kabul eder ve sorunsuz bir şekilde paylaşılan kaynağa erişir. (Konuk kullanıcıya bir davet e-posta kullanım URL ile hala alır, ancak bazı özel durumlar dışında artık davet e-posta kullanmak için gerekli olan.)  
- Konuk kullanıcıya bir davet e-posta alır ve kullanım URL tıklar. Kapsamında ilk kez oturum açma, bunlar gizlilik koşullarını kabul etmeniz istenir.

## <a name="redemption-through-a-direct-link"></a>Doğrudan bir bağlantı üzerinden kullanım

Bir konuk davet eden, paylaşılan bir uygulamaya doğrudan bağlantı göndererek Konuk kullanıcı davet edebilirsiniz. Konuk kullanıcı için kullanım deneyimi bunlarla paylaşıldı uygulama oturum açma kadar kolaydır. Bunlar bir uygulamanın bağlantısını tıklatın, gözden geçirin ve gizlilik koşullarını kabul ve uygulamaya sorunsuz bir şekilde erişmek. Çoğu durumda, Konuk kullanıcılar artık kullanım URL davet e tıklamanız gerekir.

Konuk kullanıcılar kullanıcı arabirimi aracılığıyla davet ya da PowerShell davet deneyimi bir parçası olarak davet e-posta göndermek seçtiğiniz davet edilen kullanıcı hala bir davet e-posta alır. Bu e-posta, aşağıdaki özel durumlar için yararlıdır:

- Kullanıcı, bir Azure AD hesabı veya bir Microsoft hesabı (MSA) sahip değil. Bu durumda, kullanıcı bir MSA bağlantıya tıklayın ya da kullanım URL davet e-postayla kullanabileceklerini önce oluşturmanız gerekir. Alma işlemi otomatik olarak bir MSA oluşturmak için kullanıcıya sorar.
- Bazen davet edilen kullanıcı nesnesinin bir e-posta adresi kişi nesnesi (örneğin, bir Outlook ilgili kişi nesnesi) ile bir çakışma olduğundan olmayabilir. Bu durumda, kullanıcı davet e-posta kullanım URL'de tıklatmalısınız.
- Kullanıcı Davet edilen e-posta adresini diğer ad ile oturum. (Diğer bir e-posta hesabıyla ilişkili bir ek e-posta adresidir.) Bu durumda, kullanıcı davet e-posta kullanım URL'de tıklatmalısınız.

Bu özel durumlar, kuruluşunuz için önemliyse davet e-posta göndermeye devam yöntemleri kullanarak kullanıcıları davet öneririz. Bir kullanıcı bu özel durumlar biri altında kalan değil, ayrıca, bunlar hala erişmek için bir davet e-posta URL'de tıklatabilirsiniz.

## <a name="redemption-through-the-invitation-email"></a>Davet e-postayla kullanım

Bir davet e-posta gönderen bir yöntem davet, kullanıcılar ayrıca bir davet davet e-posta yoluyla almak. Davet edilen kullanıcı e-posta, kullanım URL'yi tıklatın ve sonra gözden geçirin ve gizlilik koşullarını kabul edin. İşlem aşağıda ayrıntılı olarak açıklanmıştır:

1.  Davet sonra davet edilene davetiye gönderilen e-posta aracılığıyla alan **Microsoft Invitations**.
2.  Davet edilene seçer **Get Started** e-posta.
3.  Davet edilene bir Azure AD hesabı veya bir MSA yoksa, bunlar bir MSA oluşturmanız istenir.
4.  Davet edilene yönlendireceği **gözden izinleri** burada bunlar davet kuruluşunuzun gizlilik bildirimini gözden geçirebilir ve koşulları kabul ekran.

## <a name="privacy-policy-agreement"></a>Gizlilik İlkesi Sözleşmesi

İlk kez bir iş ortağı kuruluşunda bulunan kaynaklara erişmek herhangi bir Konuk kullanıcı oturum açtıktan sonra gördükleri bir **gözden izinleri** ekran. Burada, davet kuruluşunuzun gizlilik bildirimini gözden geçirebilirsiniz. Bir kullanıcı devam etmek için davet kuruluşunuzun gizlilik ilkelerini uygun kendi bilgilerinin kullanılmasını kabul etmeniz gerekir.

![Kullanıcı ayarlarını erişim panelinde gösteren ekran görüntüsü](media/active-directory-b2b-redemption-experience/ConsentScreen.png) 

Kiracı Yöneticisi olarak size, kuruluşunuzun gizlilik bildirimine nasıl bağlantı hakkında daha fazla bilgi için bkz: [nasıl yapılır: Azure Active Directory'de, kuruluşunuzun gizlilik bilgisi eklemek](https://aka.ms/adprivacystatement).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Azure portalında Azure Active Directory B2B işbirliği kullanıcı ekleme](active-directory-b2b-admin-add-users.md)
- [Bilgi çalışanları nasıl Azure Active Directory B2B işbirliği kullanıcılar eklenir?](active-directory-b2b-iw-add-users.md)
- [PowerShell kullanarak Azure Active Directory B2B işbirliği kullanıcı ekleme](active-directory-b2b-api.md#powershell)
- [Kuruluş Konuk kullanıcı olarak bırakın](active-directory-b2b-leave-the-organization.md)