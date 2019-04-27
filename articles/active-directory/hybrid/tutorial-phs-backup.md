---
title: 'Öğretici:  PHS, Azure AD Connect, AD FS için yedek olarak ayarlama | Microsoft Docs'
description: Yedek olarak ve AD FS için parola karması eşitleme devre dışı bırakma gösterir.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 01/30/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2281bc451a5acf9e4e634a124161a3e8b0734deb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60455729"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Öğretici:  PHS, Azure AD Connect, AD FS için yedek olarak ayarlama

Aşağıdaki öğreticiye yedekleme ve AD FS için yük devretme olarak parola karması eşitlemeyi ayarlamanız konusunda size yol gösterir.  Bu belge, AD FS başarısız veya kullanılamaz hale birincil kimlik doğrulama yöntemi olarak parola karma eşitlemesini etkinleştirme konusunda da gösterilecektir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici sırasında yapılar [Öğreticisi: Bulutta tek bir AD ormanı ortamı federasyona](tutorial-federation.md) ve bu öğreticiye başlamadan önce bir başına-önkoşul olan.  Bu öğreticide tamamlamadıysanız, bu belgedeki adımlarda denemeden önce bunu yapın.

## <a name="enable-phs-in-azure-ad-connect"></a>Azure AD'de PHS etkinleştirme bağlanma
Biz, Federasyon, kullanarak bir Azure AD Connect ortama sahip olduğunuza göre ilk adım, parola karması eşitleme açın ve Azure AD Connect'i karmalarını eşitleyecek şekilde izin sağlamaktır.

Şunları yapın:

1.  Masaüstü üzerinde oluşturulan Azure AD Connect simgesini çift tıklatın
2.  **Yapılandır**'a tıklayın.
3.  Ek Görevler sayfasında **eşitleme seçeneklerini özelleştirme** tıklatıp **sonraki**.
4.  İçin genel yönetici kullanıcı adı ve parola girin.  Bu hesabın oluşturulduğu [burada](tutorial-federation.md#create-a-global-administrator-in-azure-ad) önceki öğreticide.
5.  Üzerinde **dizinlerinizi bağlama** ekranında **sonraki**.
6.  Üzerinde **etki alanı ve OU filtreleme** ekranında **sonraki**.
7.  Üzerinde **isteğe bağlı özellikler** onay ekranında **parola karması eşitleme** tıklatıp **sonraki**.
![Seç](media/tutorial-phs-backup/backup1.png)</br>
8.  Üzerinde **yapılandırma için hazır** ekran tıklatın **yapılandırma**.
9.  Yapılandırma tamamlandıktan sonra tıklayın **çıkış**.
10. İşte bu kadar!  Gerçekleştirilir.  Parola Karması eşitleme artık ortaya çıkar ve AD FS kullanılamaz duruma gelirse, yedekleme olarak kullanılabilir.

## <a name="switch-to-password-hash-synchronization"></a>Parola Karması eşitleme için geçiş
Şimdi, parola karma eşitlemesini geçebilir nasıl göstereceğiz. Başlamadan önce hangi koşullar altında geçiş yapmak göz önünde bulundurun. Anahtarı, ağ kesintisi, ikincil bir AD FS sorun veya kullanıcılarınızın bir alt etkileyen bir sorun gibi geçici nedeniyle yapmayın. Sorun giderme çok uzun sürer çünkü geçiş yapmak karar verirseniz, aşağıdakileri yapın:

1. Masaüstü üzerinde oluşturulan Azure AD Connect simgesini çift tıklatın
2.  **Yapılandır**'a tıklayın.
3.  Seçin **değiştirme kullanıcı oturum açma** tıklatıp **sonraki**.
![Değişiklik](media/tutorial-phs-backup/backup2.png)</br>
4.  İçin genel yönetici kullanıcı adı ve parola girin.  Bu hesabın oluşturulduğu [burada](tutorial-federation.md#create-a-global-administrator-in-azure-ad) önceki öğreticide.
5.  Üzerinde **kullanıcı oturum açma** ekranındayken **parola karması eşitleme** ve işaretleyin **kullanıcı hesaplarını dönüştürme** kutusu.  
6.  Varsayılan değeri bırakın **çoklu oturum açmayı etkinleştirme** tıklatın ve seçili **sonraki**.
7.  Üzerinde **çoklu oturum açmayı etkinleştirme** ekran tıklatın **sonraki**.
8.  Üzerinde **yapılandırma için hazır** ekranında **yapılandırma**.
9.  Yapılandırma tamamlandıktan sonra tıklayın **çıkış**.
10. Kullanıcılar artık kullanıcıların parolalarını Azure ve Azure için oturum açmak için kullanabilir Hizmetleri.

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızın biriyle oturum açma testi

1. Göz atın [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Sunduğumuz yeni kiracıda oluşturduğunuz kullanıcı hesabınızla oturum açın.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Şirket içi ad'nizde oturum açmak için kullanıcının kullandığı aynı parolayı kullanın.</br>
   ![Doğrulayın](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>Sonraki Adımlar


- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Parola karması eşitlemesi](how-to-connect-password-hash-synchronization.md)|
