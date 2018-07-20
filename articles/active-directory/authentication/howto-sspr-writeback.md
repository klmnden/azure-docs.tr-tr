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
ms.openlocfilehash: e613ff742096077fe1765d4b855b6c7d409cc228
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158956"
---
# <a name="how-to-configure-password-writeback"></a>Nasıl yapılır: parola geri yazmayı yapılandırın

Otomatik güncelleştirme özelliğini kullanmanızı öneririz [Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md) parola geri yazma özelliğini kullanırken.

Zaten yapılandırdığınız Azure AD Connect, ortamınızda kullanarak aşağıdaki adımları varsayar [Express](./../connect/active-directory-aadconnect-get-started-express.md) veya [özel](./../connect/active-directory-aadconnect-get-started-custom.md) ayarları.

1. Parola geri yazma özelliğini etkinleştirmek ve yapılandırmak için Azure AD Connect sunucusu için oturum açın ve başlangıç **Azure AD Connect** Yapılandırma Sihirbazı.
2. Üzerinde **Hoş Geldiniz** sayfasında **yapılandırma**.
3. Üzerinde **ek görevler** sayfasında **eşitleme seçeneklerini özelleştirme**ve ardından **sonraki**.
4. Üzerinde **Azure ad Connect** sayfasında genel yönetici kimlik bilgilerini girin ve ardından **sonraki**.
5. Üzerinde **dizinleri Bağla** ve **etki alanı/OU** sayfa filtresi seçin **sonraki**.
6. Üzerinde **isteğe bağlı özellikler** sayfasında, yanındaki kutuyu işaretleyin **parola geri yazma** seçip **sonraki**.
   ![Azure AD CONNECT'te parola geri yazmayı etkinleştirme][Writeback]
7. Üzerinde **yapılandırma için hazır** sayfasında **yapılandırma** işleminin tamamlanması için bekleyin.
8. Son yapılandırma gördüğünüzde, seçmek **çıkış**.

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
