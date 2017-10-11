---
title: "Azure AD Connect eşitleme: Azure AD Connect eşitleme hizmeti hesabını değiştirme | Microsoft Docs"
description: "Bu konuda belge şifreleme anahtarını ve parolalar değiştirildikten sonra abandon açıklar."
services: active-directory
keywords: "Azure AD eşitleme hizmeti hesabını, parola"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: bf6234d0810f870909957ee1c1e33c225a4922b9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="changing-the-azure-ad-connect-sync-service-account-password"></a>Azure AD Connect eşitleme hizmeti hesabı parolasını değiştirme
Azure AD Connect eşitleme hizmeti hesabı parolasını değiştirirseniz, şifreleme anahtarını terk ve Azure AD Connect eşitleme hizmeti hesabının parolasını yeniden kadar eşitleme hizmeti mümkün başlangıç doğru olmaz. 

Azure AD Connect Eşitleme hizmetlerini bir parçası olarak AD DS ve Azure AD hizmet hesaplarının parolaları depolamak için bir şifreleme anahtarı kullanır.  Bu hesaplar veritabanında depolanan önce şifrelenir. 

Kullanılan şifreleme anahtarını kullanarak güvenliği [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx). DPAPI koruyan şifreleme anahtarı kullanarak **Azure AD Connect eşitleme hizmeti hesabının parolasını**. 

Hizmet hesabı parolasını değiştirmeniz gerekiyorsa yordamları kullanabilirsiniz [Azure AD Connect eşitleme şifreleme anahtarını bırakıp](#abandoning-the-azure-ad-connect-sync-encryption-key) bunu gerçekleştirmek için.  Şifreleme anahtarı herhangi bir nedenle abandon gerekiyorsa, bu yordamları da kullanılmalıdır.

##<a name="issues-that-arise-from-changing-the-password"></a>Parola değiştirme ortaya çıkan sorunları
Hizmet hesabı parolasını değiştirdiğinizde yapılmasına gerek iki nokta vardır.

İlk olarak, altında Windows hizmet denetimi yöneticisi parolasını değiştirmeniz gerekir.  Bu sorun çözülene kadar aşağıdaki hatalar görürsünüz:


- Windows Hizmet Denetimi Yöneticisi'nde eşitleme hizmetini başlatmayı denerseniz, şu hatayı alıyorsunuz "**Windows yerel bilgisayarda Microsoft Azure AD eşitleme hizmeti başlatılamadı**". **Hata 1069: Bir oturum açma hatası nedeniyle hizmeti başlatılmadı.** "
- Windows Olay Görüntüleyicisi'ni altında sistem olay günlüğüne bir hata ile içeren **olay kimliği 7038** ve ileti "**ADSync hizmeti şu hata nedeniyle şu anda yapılandırılmış parola olarak oturum açamadı: kullanıcı adı veya parola doğru değil.** "

Parola güncelleştirilirse, ikinci olarak, belirli koşullar altında eşitleme hizmeti artık DPAPI aracılığıyla şifreleme anahtarını alabilir. Şifreleme anahtarı olmadan, eşitleme hizmeti AD ve Azure AD şirket içi denetleyicisinden eşitlemek için gerekli olan parolaları şifresi çözülemiyor.
Hataları gibi görürsünüz:

- Windows Hizmet Denetimi Yöneticisi altında eşitleme hizmeti başlatmayı deneyin ve şifreleme anahtarını alamıyor hatasıyla başarısız "** Windows yerel bilgisayarda Microsoft Azure AD eşitleme başlatılamadı. Daha fazla bilgi için sistem olay günlüğünü gözden geçirin. Microsoft dışı hizmeti varsa hizmet satıcısını ve hizmete özgü hata koduna başvurun **-21451857952 ***. "
- Windows Olay Görüntüleyicisi'ni altında uygulama olay günlüğüne bir hata ile içeren **olay kimliği 6028** ve hata iletisi *"**sunucu şifreleme anahtarına erişilemiyor.* *"*

Bu hatalar almazsınız emin olmak için konusundaki yordamları izleyin [Azure AD Connect eşitleme şifreleme anahtarını bırakıp](#abandoning-the-azure-ad-connect-sync-encryption-key) parola değiştirme.
 
## <a name="abandoning-the-azure-ad-connect-sync-encryption-key"></a>Azure AD Connect eşitleme şifreleme anahtarını bırakıp
>[!IMPORTANT]
>Aşağıdaki yordamlar yalnızca Azure AD Connect yapı 1.1.443.0 uygulanır veya daha eski.

Şifreleme anahtarını iptal etmek için aşağıdaki yordamları kullanın.

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a>Şifreleme anahtarını iptal ihtiyacınız varsa yapmanız gerekenler

Şifreleme anahtarını iptal ihtiyacınız varsa, bunu gerçekleştirmek için aşağıdaki yordamları kullanın.

1. [Var olan şifreleme anahtarı iptal](#abandon-the-existing-encryption-key)

2. [AD DS hesabı için parola sağlayın](#provide-the-password-of-the-ad-ds-account)

3. [Azure AD eşitleme hesabının parolasını yeniden Başlat](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [Eşitleme hizmetini Başlat](#start-the-synchronization-service)

#### <a name="abandon-the-existing-encryption-key"></a>Var olan şifreleme anahtarı iptal
Bu yeni bir şifreleme anahtarı oluşturulabilmesi varolan şifreleme anahtarını bırakın:

1. Azure AD Connect sunucunuza yönetici olarak oturum açın.

2. Yeni bir PowerShell oturumu başlatın.

3. Klasöre gidin:`$env:Program Files\Microsoft Azure AD Sync\bin\`

4. Komutu çalıştırın:`./miiskmu.exe /a`

![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-account"></a>AD DS hesabı için parola sağlayın
Veritabanı içinde depolanan var olan parolaların artık şifresi çözülebilir gibi AD DS hesap parolası ile eşitleme hizmeti sağlamanız gerekir. Eşitleme hizmetinin yeni bir şifreleme anahtarı kullanarak parolalarını şifreler:

1. Eşitleme Hizmeti Yöneticisi'ni (Başlangıç → eşitleme hizmeti) başlatın.
</br>![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  
2. Git **Bağlayıcılar** sekmesi.
3. Seçin **AD Bağlayıcısı** şirket içi için karşılık gelen AD. Birden fazla AD bağlayıcısı varsa, bunların her biri için aşağıdaki adımları yineleyin.
4. Altında **Eylemler**seçin **özellikleri**.
5. Açılan iletişim kutusunda seçin **Active Directory ormanına Bağlan**:
6. AD DS hesap parolasını girin **parola** metin kutusu. Parolasını bilmiyorsanız, bu adımı uygulamadan önce bilinen bir değere ayarlamalısınız.
7. Tıklatın **Tamam** yeni parolayı Kaydet ve açılır iletişim kutusunu kapatın.
![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](media/active-directory-aadconnectsync-encryption-key/key6.png)

#### <a name="reinitialize-the-password-of-the-azure-ad-sync-account"></a>Azure AD eşitleme hesabının parolasını yeniden Başlat
Azure AD hizmet hesabı için parola eşitleme hizmetine doğrudan sağlayamaz. Bunun yerine, cmdlet kullanmanıza gerek **Ekle ADSyncAADServiceAccount** Azure AD hizmet hesabını yeniden başlatmak için. Cmdlet, hesap parolasını sıfırlar ve eşitleme hizmeti için kullanılabilir hale getirir:

1. Azure AD Connect sunucuda yeni bir PowerShell oturumu başlatın.
2. Cmdlet'i çalıştırmak `Add-ADSyncAADServiceAccount`.
3. Açılan iletişim kutusunda, Azure AD kiracınız için Azure AD genel yönetici kimlik bilgilerini sağlayın.
![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](media/active-directory-aadconnectsync-encryption-key/key7.png)
4. Başarılı olursa, PowerShell komut istemi görürsünüz.

#### <a name="start-the-synchronization-service"></a>Eşitleme hizmetini Başlat
Eşitleme hizmeti için şifreleme anahtarı ve gerekli tüm parolaları erişimi, Windows Hizmet Denetimi Yöneticisi'ndeki hizmetini yeniden başlatabilirsiniz:


1. Windows Hizmet Denetimi Yöneticisi (Başlangıç → Hizmetleri) gidin.
2. Seçin **Microsoft Azure AD eşitleme** ve yeniden Başlat'ı tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
