---
title: "Azure AD Connect: Hibrit Azure AD'ye katılma sonrası yapılandırma görevleri | Microsoft Docs"
description: Hibrit Azure AD'ye katılma sonrası yapılandırma görevleri için gereken bu belge ayrıntıları tamamlayın
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9af969700f4f2dfbedc4833badd7e7349696302
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60244587"
---
# <a name="post-configuration-tasks-for-hybrid-azure-ad-join"></a>Hibrit Azure AD’ye katılmada yapılandırma sonrası görevler

Kuruluşunuz için hibrit Azure AD'ye katılma yapılandırmak için Azure AD Connect çalıştırdıktan sonra kurulumu tamamlamak için tamamlamanız gereken bazı ek adımlar vardır.  Cihazlarınız için uygulanacak adımları uygulayın.

## <a name="1-configure-controlled-rollout-optional"></a>1. Denetlenen piyasaya çıkma (isteğe bağlı) yapılandırma
Tüm etki alanına katılmış cihazlar otomatik olarak Windows 10 ve Windows Server 2016 çalıştıran tüm yapılandırma adımları tamamlandıktan sonra Azure AD'ye kaydedin. Bu otomatik kaydı yerine denetimli bir şekilde kademeli tercih ederseniz, seçmeli olarak etkinleştirmek veya otomatik dağıtımı devre dışı bırakmak için Grup İlkesi'ni kullanabilirsiniz.  Bu grup ilkesini başka yapılandırma adımları: Azure AD başlatmadan önce ayarlanmalıdır.
* Active Directory'de bir Grup İlkesi nesnesi oluşturun.
* (Ex-hibrit Azure AD katılımı) adlandırın.
* Düzenlemek & gidin:  Bilgisayar Yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenleri > cihaz kaydı.

>[!NOTE]
>İlke ayarları altındadır 2012R2 için **bilgisayar yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenleri > Workplace Join > otomatik olarak çalışma alanına katılma istemci bilgisayarlar**

* Bu ayar devre dışı bırakın:  Bilgisayarları etki alanına katılmış cihazlar olarak kaydedin.
* Uygulama ve Tamam'a tıklayın.
* Kendi tercih ettiğiniz (kuruluş birimi, güvenlik grubu veya etki alanı için tüm cihazlar için) konumuna GPO'yu bağlayın.

## <a name="2-configure-network-with-device-registration-endpoints"></a>2. Ağ cihazı kayıt uç noktaları ile yapılandırma
Aşağıdaki URL'ler kaydı için Kurumsal ağınızdaki bilgisayarların Azure AD'ye erişilebilir olduğundan emin olun:

* https://enterpriseregistration.windows.net
* https://login.microsoftonline.com
* https://device.login.microsoftonline.com 

## <a name="3-implement-wpad-for-windows-10-devices"></a>3. WPAD Windows 10 cihazlar için uygulama
Kuruluşunuz Internet bir giden proxy üzerinden erişirse, Azure AD'ye kaydettirmek Windows 10 bilgisayarlarını etkinleştirmek için Web Proxy Otomatik Bulma (WPAD) uygulayın.

## <a name="4-configure-the-scp-in-any-forests-that-were-not-configured-by-azure-ad-connect"></a>4. Azure AD Connect tarafından yapılandırılmamış tüm ormanlardaki SCP'yi yapılandırın 

Hizmet bağlantı noktası (SCP), cihazlarınız için otomatik kaydı tarafından kullanılan Azure AD Kiracı bilgilerinizi içerir.  Azure AD Connect'ten indirdiğiniz Configurescp.ps1'i PowerShell betiğini çalıştırın.

## <a name="5-configure-any-federation-service-that-was-not-configured-by-azure-ad-connect"></a>5. Azure AD Connect tarafından yapılandırılmadı herhangi bir Federasyon Hizmeti yapılandırma

Azure AD bağlı olan taraf güveni talep kuralları, kuruluşunuzun, Azure AD'de oturum açmak için bir Federasyon Hizmeti kullanıyorsa, cihaz kimlik doğrulaması izin vermelidir. AD FS ile Federasyon kullanıyorsanız, Git [AD FS Yardım](https://aka.ms/aadrptclaimrules) talep kuralları oluşturmak için. Bir Microsoft dışı Federasyon çözümü kullanıyorsanız, bu sağlayıcı için yönergeler başvurun.  

>[!NOTE]
>Windows alt düzey cihazlarınız varsa, hizmet istekleri için Azure AD güveni alırken authenticationmethod ve wiaormultiauthn talep verme desteklemesi gerekir. AD FS'de kimlik doğrulama yöntemini geçişleri bütünlüklü bir verme dönüştürme kuralı eklemeniz gerekir.

## <a name="6-enable-azure-ad-seamless-sso-for-windows-down-level-devices"></a>6. Azure AD sorunsuz SSO için Windows alt düzey cihazları etkinleştirme

Kuruluşunuz Azure AD'de oturum açmak için parola karması eşitleme veya doğrudan kimlik doğrulaması kullanıyorsa, Windows alt düzey cihazların kimliklerini doğrulamak için oturum açma yöntemi Azure AD sorunsuz çoklu oturum açmayı etkinleştir: https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso. 

## <a name="7-set-azure-ad-policy-for-windows-down-level-devices"></a>7. İçin Windows alt düzey cihazları Azure AD'ye İlkesi ayarlama

Windows alt düzey cihazları kaydetmek için Azure AD İlkesi cihazları kaydedin açmasına izin verdiğinden emin olmak gerekir. 

* Azure Portalı'ndaki hesabınızda oturum.
* Şuraya gidin:  Azure Active Directory > cihazlar > cihaz ayarları
* "Kullanıcıların cihazlarını Azure AD'ye kaydedebilir" seçeneğini tümü olarak ayarlayın.
* Kaydet’e tıklayın.

## <a name="8-add-azure-ad-endpoint-to-windows-down-level-devices"></a>8. Azure AD uç noktası için Windows alt düzey cihazları ekleme

Azure AD cihaz kimlik doğrulama uç noktası cihazları doğrulanırken sertifika istemleri önlemek için Windows alt düzey cihazlarınızda yerel Intranet bölgeleri ekleyin: https://device.login.microsoftonline.com 

Kullanıyorsanız [sorunsuz çoklu oturum açma](how-to-connect-sso.md), ayrıca bu bölgeye etkinleştir "durum çubuğu komut güncelleştirmelerine izin ver" ve aşağıdaki uç noktayı ekleyin: https://autologon.microsoftazuread-sso.com 

## <a name="9-install-microsoft-workplace-join-on-windows-down-level-devices"></a>9. Microsoft Workplace Join Windows alt düzey cihazlarda yükleyin

Bu yükleyici, kullanıcının bağlamında çalışan cihazın sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, kullanıcı kimlik bilgileriyle tümleşik Windows kimlik doğrulamasını kullanarak kimlik doğrulaması sonra Azure AD ile cihaz sessizce birleştirir. İndirme Merkezi'nde altındadır https://www.microsoft.com/download/details.aspx?id=53554. 

## <a name="10-configure-group-policy-to-allow-device-registration"></a>10. Cihaz kaydı izin vermek için Grup İlkesi yapılandırma

* Grup İlkesi nesnesini Active Directory'nizde--zaten oluşturduysanız oluşturun.
* (Ex-hibrit Azure AD katılımı) adlandırın.
* Düzenlemek & gidin:  Bilgisayar Yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenleri > cihaz kaydı
* Etkinleştirin:  Bilgisayarları etki alanına katılmış cihaz olarak kaydetme
* Uygulama ve Tamam'a tıklayın.
* Kendi tercih ettiğiniz (kuruluş birimi, güvenlik grubu veya etki alanı için tüm cihazlar için) konumuna GPO'yu bağlayın.

>[!NOTE]
>İlke ayarları altındadır 2012R2 için **bilgisayar yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenleri > Workplace Join > otomatik olarak çalışma alanına katılma istemci bilgisayarlar**

## <a name="next-steps"></a>Sonraki adımlar
[Cihaz geri yazmayı yapılandırın](how-to-connect-device-writeback.md)
