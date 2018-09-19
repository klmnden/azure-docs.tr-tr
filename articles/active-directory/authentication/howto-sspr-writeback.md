---
title: Nasıl yapılır için Azure AD SSPR'yi parola geri yazmayı yapılandırma
description: Azure AD kullanın ve şirket içi dizine parolaları geri yazma için Azure AD Connect
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 1ae74f7c43e763962224683954b28e5941136c08
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295827"
---
# <a name="how-to-configure-password-writeback"></a>Nasıl yapılır: parola geri yazmayı yapılandırın

Otomatik güncelleştirme özelliğini kullanmanızı öneririz [Azure AD Connect](../hybrid/how-to-connect-install-express.md) parola geri yazma özelliğini kullanırken.

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
6. İçinde **uygulandığı** aşağı açılan listesinden **Descendent kullanıcı** nesneleri.
7. Altında **izinleri**, aşağıdakileri seçin:
    * **Parola sıfırlama**
    * **Parola değiştirme**
    * **LockoutTime yazma**
    * **PwdLastSet yazma**
8. Seçin **Uygula/Tamam** değişiklikleri uygulamak ve açık bir iletişim kutusu çıkın.

## <a name="next-steps"></a>Sonraki adımlar

[Parola geri yazma nedir?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Azure AD CONNECT'te parola geri yazmayı etkinleştirme"
