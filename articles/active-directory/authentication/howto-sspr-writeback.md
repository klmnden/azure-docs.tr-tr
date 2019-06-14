---
title: Nasıl yapılır, Azure AD SSPR - Azure Active Directory parola geri yazmayı yapılandırma
description: Azure AD kullanın ve şirket içi dizine parolaları geri yazma için Azure AD Connect
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17a2661883dd069e8cb719672f6b92442f1a8a0a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60357515"
---
# <a name="how-to-configure-password-writeback"></a>Nasıl yapılır: Parola geri yazmayı yapılandırın

Zaten yapılandırdığınız Azure AD Connect, ortamınızda kullanarak aşağıdaki adımları varsayar [Express](../hybrid/how-to-connect-install-express.md) veya [özel](../hybrid/how-to-connect-install-custom.md) ayarları.

1. Parola geri yazma özelliğini yapılandırmak ve etkinleştirmek için Azure AD Connect sunucunuzda oturum açın ve **Azure AD Connect** yapılandırma sihirbazını başlatın.
2. **Hoş Geldiniz** sayfasında, **Yapılandır**’ı seçin.
3. **Ek görevler** sayfasında **Eşitleme seçeneklerini özelleştirme**'yi ve ardından **İleri**'yi seçin.
4. **Azure AD'ye bağlan** sayfasına bir genel yöneticinin kimlik bilgilerini girin ve **İleri**'yi seçin.
5. **Dizinleri bağla** ve **Etki Alanı/OU** filtreleme sayfalarında **İleri**'yi seçin.
6. **İsteğe bağlı özellikler** sayfasında **Parola geri yazma** özelliğinin yanındaki kutuyu işaretleyin ve **İleri**'yi seçin.
   ![Azure AD CONNECT'te parola geri yazmayı etkinleştirme][Writeback]
7. **Yapılandırma için hazır** sayfasında **Yapılandır**'ı seçin ve işlemin tamamlanmasını bekleyin.
8. Yapılandırma tamamlandığında **Çıkış**'ı seçin.

Parola geri yazma için ilgili genel sorun giderme görevleri görmek için bölüm [parola geri yazma sorunlarını gidermek](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) sorun giderme makalemizde.

> [!WARNING]
> Parola geri yazma, Azure AD Connect sürüm 1.0.8641.0 ve eski olduğunda kullanan müşteriler için çalışma durdurur [Azure erişim denetimi hizmeti (ACS) 7 Kasım 2018'de kullanımdan](../develop/active-directory-acs-migration.md). Azure AD Connect sürüm 1.0.8641.0 eski ve bunlar üzerinde ACS işlevselliği için bağımlı olduğundan parola geri yazma o anda artık izin verir.
>
> Hizmette, yeni bir sürüme bir Azure AD Connect'in önceki sürümünden yükseltme kesinti yaşanmasını önlemek için bu makaleye bakın [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](../hybrid/how-to-upgrade-previous-version.md)
>

## <a name="licensing-requirements-for-password-writeback"></a>Parola geri yazma için lisans gereksinimleri

**Self Servis parola sıfırlama/değiştirme/kilidini açma şirket içi geri yazma ile Azure AD premium özelliğidir**. Lisanslama hakkında daha fazla bilgi için bkz. [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

Parola geri yazma özelliğini kullanmak için kiracınızda atanan aşağıdaki lisanslardan birine sahip olmalıdır:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 veya A3
* Enterprise Mobility + Security E5'e veya A5
* Microsoft 365 E3 veya A3
* Microsoft 365 E5 veya A5
* Microsoft 365 F1
* Microsoft 365 İş

> [!WARNING]
> Tek başına Office 365 planları lisanslama *"Self Servis parola sıfırlama/değiştirme/kilidini açma şirket içi geri yazma ile" desteklemeyen* ve çalışmak bu işlev için önceki planlardan birine sahip olması gerekir.
>

## <a name="active-directory-permissions"></a>Active Directory izinleri

Belirtilen hesaba SSPR kapsamında olmasını istiyorsanız Azure AD Connect yardımcı programında şu öğeleri ayarlamanız gerekir:

* **Parola sıfırlama** 
* **Parola değiştirme** 
* **Yazma izinleri** üzerinde `lockoutTime`
* **Yazma izinleri** üzerinde `pwdLastSet`
* **Hakları genişletilmiş** ya da üzerinde:
   * Kök nesnenin *her etki alanı* o ormandaki
   * SSPR kapsamında olmasını istediğiniz kullanıcı kuruluş birimi (OU)

Emin değilseniz ne açıklanan hesabın hesap, Azure Active Directory Connect yapılandırma kullanıcı arabirimini açın ve seçin başvurduğu **geçerli yapılandırmayı görüntüleme** seçeneği. Altında listelendiğini izni eklemek istediğiniz hesap **eşitlenen dizinler**.

Bu izinleri ayarlarsanız, her ormana yönelik MA hizmet hesabının bu orman içindeki kullanıcı hesapları adına parolaları yönetebilirsiniz. 

> [!IMPORTANT]
> Bu izinleri atamazsanız geri yazma doğru yapılandırılmış gibi görünüyor olsa bile kullanıcılar buluttan şirket içi parolalarını yönetme girişiminde bulunduğunuzda daha sonra kullanıcılar hataları karşılaşır.
>

> [!NOTE]
> Bu bir saat veya daha fazla bilgi için dizininizdeki tüm nesneler için çoğaltılması için bu izinlere kadar sürebilir.
>

Parola geri yazmanın gerçekleşmesini sağlamak için uygun izinleri ayarlamak için aşağıdaki adımları tamamlayın:

1. Active Directory Kullanıcıları ve bilgisayarları uygun etki alanı yönetim izinleri olan bir hesap ile açın.
2. Gelen **görünümü** menüsünde emin **Gelişmiş Özellikler** açıktır.
3. Sol bölmede bulunan seçin ve etki alanı kökünde temsil eden nesneye sağ tıklayın **özellikleri** > **güvenlik** > **Gelişmiş**.
4. Gelen **izinleri** sekmesinde **Ekle**.
5. İzinler (Azure AD Connect kurulumunun) uygulanmakta olan bir hesabı seçin.
6. İçinde **uygulandığı** aşağı açılan listesinden **alt kullanıcı nesneleri**.
7. Altında **izinleri**, aşağıdaki seçenekleri seçin:
    * **Parola değiştirme**
    * **Parola sıfırlama**
8. Altında **özellikleri**, aşağıdaki seçenekleri seçin:
    * **LockoutTime yazma**
    * **PwdLastSet yazma**
9. Seçin **Uygula/Tamam** değişiklikleri uygulamak ve açık bir iletişim kutusu çıkın.

## <a name="next-steps"></a>Sonraki adımlar

[Parola geri yazma nedir?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Azure AD CONNECT'te parola geri yazmayı etkinleştirme"
