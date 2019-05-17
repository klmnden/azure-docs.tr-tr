---
title: Şirket iş hesapları ve iş ortağı Merkezi
description: Şirketiniz bir iş hesabı var olup olmadığını denetlemek nasıl Microsoft ile ayarlayın, yeni bir iş hesabı oluşturmasına veya iş ortağı Merkezi ile kullanmak için birden çok iş hesaplarını ayarlama.
author: mattwojo
manager: evansma
ms.author: parthp
ms.service: marketplace
ms.topic: how-to
ms.date: 05/30/2019
ms.openlocfilehash: df8ab370e8874e07f8985ecfb3a772848a2e2b21
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806289"
---
# <a name="company-work-accounts-and-partner-center"></a>Şirket iş hesapları ve iş ortağı Merkezi

İş ortağı Merkezi, birden çok kullanıcılar, Denetim izinleri, konak grupları ve uygulamaları için hesabı erişimini yönetmek ve profil verileri korumak için şirket iş hesapları, Azure Active Directory (AD) Kiracı olarak da bilinen kullanır. İş ortağı merkezi hesabınız, şirketinizin iş e-posta hesabı etki alanı bağlantılandırarak, şirketinizin çalışanları için iş ortağı merkezi Market teklifleri, kendi iş hesabı kullanıcı adları ve parolaları kullanarak yönetmek için oturum açabilir.

## <a name="check-whether-your-company-already-has-a-work-account"></a>Şirketinizin zaten bir iş hesabı var olup olmadığını denetleyin

Şirket, Azure, Microsoft Intune veya Office 365 gibi Microsoft bulut hizmetine abone olduğu ardından zaten iş ortağı Merkezi ile kullanılabilen (Azure Active Directory kiracısı da bilinir) bir iş e-posta hesabı etki alanı varsa.

Denetlemek için aşağıdaki adımları izleyin:
1. Azure yönetim portalında oturum açın https://portal.azure.com.
2. Seçin **Azure Active Directory** sol gezinti menüsünden ve ardından **özel etki alanı adları**.
3. Bir iş hesabı zaten varsa, etki alanı adınız listede görüntülenir.

Şirketinizin zaten bir iş hesabı yoksa, bir sizin için iş ortağı merkezi kayıt işlemi sırasında oluşturulur.

## <a name="set-up-multiple-work-accounts"></a>Birden çok iş hesaplarını ayarlama

Mevcut bir iş hesabı kullanmaya karar vermeden önce kaç kullanıcı iş hesabında iş ortağı merkezi erişmeniz göz önünde bulundurun. Kullanıcı iş hesabında iş ortağı merkezi erişmesi gereken olmaz varsa, iş ortağı Merkezi erişim sağlaması gereken kullanıcılara yalnızca belirli bir hesapta gösterilir. böylece, birden çok iş hesabı oluşturmayı göz önünde bulundurun isteyebilirsiniz.

## <a name="create-a-new-work-account"></a>Yeni bir iş hesabı oluşturma

Şirketiniz için yeni bir iş hesabı oluşturmak için aşağıdaki adımları izleyin. Kişi yönetici izinleri, şirketinizin Microsoft Azure hesabı olan Yardım isteğinde gerekebilir.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.
2. Soldaki menüden **Azure Active Directory** -> **kullanıcılar**.
3. Seçin **yeni kullanıcı** ve bir ad ve e-posta adresini girerek yeni bir Azure iş hesabı oluşturun. Emin **dizin rolü** ayarlanır **kullanıcı** seçip **parola Göster** bölümünün en altında görüntülemek ve otomatik olarak oluşturulan parolayı not edin.
4. Seçin **Oluştur** yeni kullanıcı kaydetmek için.

Kullanıcı hesabı için e-posta adresi, dizininizdeki bir doğrulanmış etki alanı adı olmalıdır. Seçerek dizininizde tüm doğrulanmış etki alanları listeleyebilirsiniz **Azure Active Directory** -> **özel etki alanı adları** sol gezinti menüsünde.

Azure Active Directory'de özel etki alanları ekleme hakkında daha fazla bilgi edinmek için [ekleme veya ilişkilendirme Azure AD'de bir etki alanı](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain).

## <a name="troubleshoot-work-email-sign-in"></a>İş e-posta oturum açma sorunlarını giderme

İş hesabınızı (Azure AD kiracınıza olarak da bilinir) oturum açma ile ilgili sorunlar yaşıyorsanız, durumunuza en iyi Aşağıdaki diyagramda senaryo eşleşen bulun ve önerilen adımları izleyin.

![İş hesabı oturum açma sorunlarını giderme için diyagramı](./media/onboarding-aad-flow.png)

## <a name="next-steps"></a>Sonraki adımlar

- [İş ortağı Merkezi'nde ticari Market hesabınızı yönetin](./manage-account.md) 
