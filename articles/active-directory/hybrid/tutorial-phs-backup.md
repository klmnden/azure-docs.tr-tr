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
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e5ad7badfa44a006fd7e71d3b0e42ee95ac698d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919000"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Öğretici:  PHS, Azure AD Connect, AD FS için yedek olarak ayarlama

Aşağıdaki öğreticiye yedekleme ve AD FS için yük devretme olarak parola karması eşitlemeyi ayarlamanız konusunda size yol gösterir.  Bu belge, AD FS başarısız veya kullanılamaz hale birincil kimlik doğrulama yöntemi olarak parola karma eşitlemesini etkinleştirme konusunda da gösterilecektir.

>[!NOTE] 
>Bu adımları sırasında Acil Durum veya kesinti durumlarda genellikle gerçekleştirilen olsa da, test adımları ve kesinti gerçekleşmeden önce yordamlarınızın doğrulamak önerilir.

>[!NOTE]
>Azure AD Connect sunucusu için erişimi olmaması veya sunucunun internet erişimi yok, olay, başvurabilirsiniz [Microsoft Support](https://support.microsoft.com/en-us/contactus/) değişiklikler Azure AD'ye tarafına yardımcı olmak için.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici sırasında yapılar [Öğreticisi: Bulutta tek bir AD ormanı ortamı federasyona](tutorial-federation.md) ve bu öğreticiye başlamadan önce bir başına-önkoşul olan.  Bu öğreticide tamamlamadıysanız, bu belgedeki adımlarda denemeden önce bunu yapın.

>[!IMPORTANT]
>PHS için geçiş öncesinde AD FS ortamınızı bir yedeğini oluşturmalısınız.  Bu yapılabilir kullanarak [AD FS hızlı geri yükleme aracı](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool#how-to-use-the-tool).

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

> [!IMPORTANT]
> Bu karma Azure AD ile eşitlemek için parola için biraz zaman alabilir dikkat edin.  Bu, eşitleme tamamlamak 3 saat'kurmak sürebilir ve bu, parola karmaları kullanarak kimlik doğrulaması önce başlatabilirsiniz anlamına gelir.

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

## <a name="switch-back-to-federation"></a>Federasyon hizmetine geçiş
Artık, Federasyon hizmetine geçiş yapma göstereceğiz.  Bunu yapmak için aşağıdakileri yapın:

1.  Masaüstü üzerinde oluşturulan Azure AD Connect simgesini çift tıklatın
2.  **Yapılandır**'a tıklayın.
3.  Seçin **değiştirme kullanıcı oturum açma** tıklatıp **sonraki**.
4.  İçin genel yönetici kullanıcı adı ve parola girin.  Bu oluşturulan hesaptır [burada](tutorial-federation.md#create-a-global-administrator-in-azure-ad) önceki öğreticide.
5.  Üzerinde **kullanıcı oturum açma** ekranındayken **AD FS ile Federasyon** tıklatıp **sonraki**.  
6. Etki alanı yöneticisi kimlik bilgileri sayfasında contoso\Administrator kullanıcı adını ve parolasını girin ve tıklayın **sonraki.**
7. AD FS grubu ekrana tıklayın **sonraki**.
8. Üzerinde **Azure AD etki alanı** ekran, açılan listeden etki alanını seçin ve tıklayın **sonraki**.
9. Üzerinde **yapılandırma için hazır** ekranında **yapılandırma**.
10. Yapılandırma tamamlandıktan sonra tıklayın **sonraki**.
![Yapılandırma](media/tutorial-phs-backup/backup4.png)</br>
11. Üzerinde **Federasyon bağlantısını doğrula** ekranında **doğrulama**.  (A ve AAAA kayıt eklemek) DNS kayıtlarını başarıyla tamamlanması için yapılandırmanız gerekebilir.
![Doğrulayın](media/tutorial-phs-backup/backup5.png)</br>
12. **Çıkış**'a tıklayın.

## <a name="reset-the-ad-fs-and-azure-trust"></a>AD FS ve Azure güven Sıfırla
Artık AD FS ile Azure arasında güveni sıfırlamak ihtiyacımız var.

1.  Masaüstü üzerinde oluşturulan Azure AD Connect simgesini çift tıklatın
2.  **Yapılandır**'a tıklayın.
3.  Seçin **yönetme Federasyon** tıklatıp **sonraki**.
4.  Seçin **sıfırlama Azure AD güven** tıklatıp **sonraki**.
![Sıfırlama](media/tutorial-phs-backup/backup6.png)</br>
5.  Üzerinde **Azure ad Connect** ekran, genel yönetici kullanıcı adı ve parola girin.
6.  Üzerinde **AD FS için Connect** ekranında, contoso\Administrator kullanıcı adını ve parolasını girin ve tıklatın **sonraki.**
7.  Üzerinde **sertifikaları** ekranında **sonraki**.

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızın biriyle oturum açma testi

1.  Göz atın [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Sunduğumuz yeni kiracıda oluşturduğunuz kullanıcı hesabı ile oturum.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Kullanıcının oturum açmak için kullandığı aynı parolayı kullanan şirket içi.
![Doğrulayın](media/tutorial-password-hash-sync/verify1.png)

Şimdi başarıyla test edin ve hangi Azure sunduğu ile kendinizi alıştırın için kullanabileceğiniz bir karma kimlik ortamı vardır.

## <a name="next-steps"></a>Sonraki adımlar


- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Parola karması eşitleme](how-to-connect-password-hash-synchronization.md)
