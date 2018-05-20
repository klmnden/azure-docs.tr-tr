---
title: 'Azure AD Connect: Karma Azure AD birleştirme sonrası yapılandırma görevlerini | Microsoft Docs'
description: Karma Azure AD birleştirme sonrası yapılandırma görevleri için gereken bu belge ayrıntıları tamamlayın
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: samueld
editor: billmath
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: anandyadavmsft
ms.openlocfilehash: 02f1cbd1f2b8f7b0bec2f8016a300ede1d6f0439
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="post-configuration-tasks-for-hybrid-azure-ad-join"></a>Yapılandırma karma Azure AD katılım için sonrası görevleri

Kuruluşunuz karma Azure AD katılım için yapılandırmak için Azure AD Connect çalıştırdıktan sonra bu kurulumu tamamlamak için tamamlamanız gereken bazı ek adımlar vardır.  Cihazlar için geçerli adımları uygulayın.

## <a name="1-configure-controlled-rollout-optional"></a>1. Denetimli sunum (isteğe bağlı) yapılandırma
Windows 10 ve Windows Server 2016 otomatik olarak çalıştıran tüm etki alanına katılmış cihazlar tüm yapılandırma adımları tamamlandıktan sonra Azure AD ile kaydedin. Bu otomatik kaydı yerine bir denetlenen sunum tercih ederseniz, seçmeli olarak etkinleştirmek veya otomatik dağıtımı devre dışı bırakmak için Grup İlkesi'ni kullanabilirsiniz.  Bu Grup İlkesi diğer yapılandırma adımları: Azure AD başlatmadan önce ayarlanmalıdır.
* Grup İlkesi nesnesini Active Directory'de oluşturun.
* (Ex-karma Azure AD birleştirme) adlandırın.
* Düzen & gidin: Bilgisayar Yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenlerini > cihaz kaydı.

>[!NOTE]
>İlke ayarları altındadır 2012R2 için **bilgisayar yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenlerini > çalışma alanına katılma > otomatik olarak çalışma alanına katılma istemci bilgisayarlar**

* Bu ayarı devre dışı: bilgisayarların etki alanına katılmış cihazlar olarak kaydedin.
* Uygulama ve Tamam'ı tıklatın.
* Tercih ettiğiniz (kuruluş birimi, güvenlik grubu veya etki alanı için tüm aygıtlar için) konumuna GPO'yu bağlayın.

## <a name="2-configure-network-with-device-registration-endpoints"></a>2. Ağ aygıtı kayıt uç ile yapılandırma
Aşağıdaki URL'ler için Azure AD kaydı için kuruluş ağınızdaki bilgisayarlardan erişilebilir olduğundan emin olun:

* https://enterpriseregistration.windows.net
* https://login.microsoftonline.com
* https://device.login.microsoftonline.com 

## <a name="3-implement-wpad-for-windows-10-devices"></a>3. WPAD Windows 10 cihazlar için uygulama
Kuruluşunuzun Internet bir giden proxy üzerinden erişirse, Azure AD ile kaydetmek Windows 10 bilgisayarları etkinleştirmek için Web Proxy Otomatik Bulma (WPAD) uygular.

## <a name="4-configure-the-scp-in-any-forests-that-were-not-configured-by-azure-ad-connect"></a>4. Azure AD Connect tarafından yapılandırılmamış ormanlarda SCP yapılandırın 

Hizmet bağlantı noktası (SCP) aygıtlarınız otomatik kaydı için tarafından kullanılan Azure AD Kiracı bilgilerinizi içerir.  Azure AD Connect'ten indirdiğiniz ConfigureSCP.ps1, PowerShell komut dosyasını çalıştırın.

## <a name="5-configure-any-federation-service-that-was-not-configured-by-azure-ad-connect"></a>5. Azure AD Connect tarafından yapılandırılmadı. tüm Federasyon hizmetini yapılandır

Kuruluşunuz Azure AD ile oturum açmak için bir Federasyon Hizmeti kullanıyorsa, Azure AD bağlı olan taraf güveni talep kuralları cihaz kimlik doğrulamasını izin vermesi gerekir. AD FS ile Federasyon kullanıyorsanız, Git [AD FS Yardım](https://aka.ms/aadrptclaimrules) talep kuralları oluşturmak için. Bir Microsoft dışı Federasyon çözümü kullanıyorsanız, bu sağlayıcı için yönergeler başvurun.  

>[!NOTE]
>Windows alt düzey aygıtlarınız varsa, hizmet istekleri için Azure AD güveni alırken authenticationmethod ve wiaormultiauthn talep verme desteklemesi gerekir. AD FS'de kimlik doğrulama yöntemini geçişleri üzerinden bir verme dönüştürme kuralı eklemeniz gerekir.

## <a name="6-enable-azure-ad-seamless-sso-for-windows-down-level-devices"></a>6. Azure AD sorunsuz SSO Windows için alt düzey aygıtları etkinleştirin

Kuruluşunuz Azure AD ile oturum açmak için parola karma eşitlemesi veya doğrudan kimlik doğrulaması kullanıyorsa, Windows alt düzey cihazların kimliğini doğrulamak için oturum açma yöntemi Azure AD sorunsuz SSO etkinleştirin: https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso. 

## <a name="7-set-azure-ad-policy-for-windows-down-level-devices"></a>7. Windows alt düzey cihazlar için Azure AD İlkesi ayarlama

Windows alt düzey cihazları kaydetmek için Azure AD İlkesi cihazlarını kaydetmek kullanıcılara izin verdiğinden emin olmanız gerekir. 

* Günlük hesabınıza Azure portalında bileşeni.
* Gidin: Azure Active Directory > cihazlar > cihaz ayarları
* "Kullanıcıların cihazlarını Azure AD ile kaydedebilir" Tümü olarak ayarlayın.
* Kaydet’e tıklayın.

## <a name="8-add-azure-ad-endpoint-to-windows-down-level-devices"></a>8. Windows alt düzey aygıtları için Azure AD uç nokta ekleyin

Yerel Intranet bölgeleri aygıtları doğrulanırken sertifika istemleri önlemek için cihazlarınızda alt düzey Windows Azure AD cihaz kimlik doğrulama uç noktası ekleyin: https://device.login.microsoftonline.com 

Kullanıyorsanız [sorunsuz SSO](https://aka.ms/hybrid/sso), ayrıca bu bölge etkinleştir "komut dosyası durum çubuğu güncelleştirmelerine izin ver" ve şu uç nokta ekleyin: https://autologon.microsoftazuread-sso.com 

## <a name="9-install-microsoft-workplace-join-on-windows-down-level-devices"></a>9. Windows alt düzey cihazlarda yüklemeyi Microsoft çalışma alanına katılma

Bu yükleyici aygıt sistemde kullanıcının bağlamında çalışan zamanlanmış bir görev oluşturur. Kullanıcı Windows açtığında görevi tetiklenir. Görev sessizce aygıt tümleşik Windows kimlik doğrulaması kullanarak kimlik doğrulama sonra Azure AD kullanıcı kimlik bilgileri ile birleştirir. İndirme Merkezi'nden altındadır https://www.microsoft.com/en-us/download/details.aspx?id=53554. 

## <a name="10-configure-group-policy-to-allow-device-registration"></a>10. Cihaz kaydı izin vermek için Grup İlkesi yapılandırma

* Grup İlkesi nesnesini Active Directory'de--zaten oluşturduysanız oluşturun.
* (Ex-karma Azure AD birleştirme) adlandırın.
* Düzen & gidin: Bilgisayar Yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenlerini > cihaz kaydı
* Etkinleştirme: etki alanına katılmış bilgisayarları cihaz olarak kaydetme
* Uygulama ve Tamam'ı tıklatın.
* Tercih ettiğiniz (kuruluş birimi, güvenlik grubu veya etki alanı için tüm aygıtlar için) konumuna GPO'yu bağlayın.

>[!NOTE]
>İlke ayarları altındadır 2012R2 için **bilgisayar yapılandırması > ilkeler > Yönetim Şablonları > Windows bileşenlerini > çalışma alanına katılma > otomatik olarak çalışma alanına katılma istemci bilgisayarlar**

## <a name="next-steps"></a>Sonraki adımlar
[Cihaz geri yazma özelliğini yapılandırın](./active-directory-aadconnect-feature-device-writeback.md)
