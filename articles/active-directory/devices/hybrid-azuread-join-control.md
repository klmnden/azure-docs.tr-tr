---
title: Hibrit Azure AD'ye katılma - Azure AD, denetimli doğrulama
description: Tek seferde tüm kuruluş genelinde etkinleştirmeden önce hibrit Azure AD'ye katılım denetimli bir doğrulama yapma hakkında bilgi
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: d59104bf9c7675fdac2c245fff89ab1483b96b67
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481716"
---
# <a name="controlled-validation-of-hybrid-azure-ad-join"></a>Hibrit Azure AD’ye katılıma yönelik denetimli doğrulama

Tüm önkoşulların yerinde olduğundan, Azure AD kiracınızda cihaz olarak Windows cihazları otomatik olarak kaydeder. Bu cihaz kimliklerini Azure AD'de durumunu hibrit Azure AD'ye katılım adlandırılır. Bu makalede ele alınan kavramları hakkında daha fazla bilgi makalelerde bulunabilir [Azure Active Directory'de cihaz yönetimine giriş](overview.md) ve [hibrit Azure Active Directory join uygulamanızı planlama ](hybrid-azuread-join-plan.md).

Kuruluşlar, tek seferde tüm kuruluşlarındaki etkinleştirmeden önce hibrit Azure AD'ye katılım denetimli bir doğrulama yapmak isteyebilirsiniz. Bu makalede, hibrit Azure AD'ye katılımı, denetimli bir doğrulama gerçekleştirmek nasıl anlatılmıştır.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-current-devices"></a>Denetimli doğrulama geçerli Windows cihazlarda hibrit Azure AD'ye katılma

Windows masaüstü işletim sistemini çalıştıran cihazlar için desteklenen sürüm Windows 10 Yıldönümü Güncelleştirmesi (sürüm 1607) olan veya üzeri. En iyi uygulama, Windows 10 'un en son sürüme yükseltin.

Hibrit Azure AD'ye katılımı geçerli Windows cihazlarda, denetimli bir doğrulama yapmak için gerekir:

1. Varsa, hizmet bağlantı noktası (SCP) Active Directory'den (AD) girişi Temizle
1. Bir Grup İlkesi nesnesi (GPO) kullanarak etki alanına katılmış bilgisayarlarda SCP için istemci tarafı kayıt defteri ayarı yapılandırın
1. AD FS kullanıyorsanız, bir GPO kullanarak AD FS sunucunuza SCP istemci-tarafı kayıt defteri ayarını da yapılandırmalısınız  



### <a name="clear-the-scp-from-ad"></a>Ad SCP Temizle

AD'de SCP nesneleri değiştirmek için Active Directory Hizmet Arabirimleri Düzenleyicisi (ADSI Düzenle aracını) kullanın.

1. Başlatma **ADSI** Masaüstü uygulamasından ve iş istasyonu veya bir etki alanı denetleyicisi bir kuruluş yöneticisi olarak.
1. Bağlanma **yapılandırma adlandırma bağlamı** etki alanınızın.
1. Göz atın **CN = Configuration, DC = contoso, DC = com** > **CN = Services** > **CN = cihaz kaydı yapılandırma**
1. Yaprak nesnesinin altında sağ tıklayın **CN = cihaz kaydı yapılandırma** seçip **özellikleri**
   1. Seçin **anahtar sözcükleri** gelen **Öznitelik Düzenleyicisi** penceresini açın ve **Düzenle**
   1. Değerleri seçin **azureADId** ve **azureADName** (teker teker) tıklayıp **Kaldır**
1. Kapat **ADSI Düzenleme**


### <a name="configure-client-side-registry-setting-for-scp"></a>İstemci tarafı kayıt defteri ayarı için SCP'yi yapılandırın

Bir kayıt defteri ayarı, cihaz kayıt defterinde bir SCP giriş yapılandırma dağıtmak için Grup İlkesi nesnesi (GPO) oluşturma için aşağıdaki örneği kullanın.

1. Bir Grup İlkesi Yönetimi konsolunu açın ve etki alanınızda yeni bir Grup İlkesi nesnesi oluşturun.
   1. Yeni oluşturulan GPO'NUZUN bir ad (örneğin, ClientSideSCP) sağlayın.
1. GPO'yu düzenleyin ve aşağıdaki yolu bulun: **Bilgisayar Yapılandırması** > **tercihleri** > **Windows ayarları** > **kayıt defteri**
1. Sağ tıklatın ve kayıt defteri **yeni** > **kayıt defteri öğesi**
   1. Üzerinde **genel** sekmesinde, aşağıdakileri yapılandırın
      1. Eylem: **Güncelleştirme**
      1. Hive: **HKEY_LOCAL_MACHINE**
      1. Anahtar yolu: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Değer adı: **TenantId**
      1. Değer türü: **REG_SZ**
      1. Değer verisi: GUID veya **dizin kimliği** örneğinizin Azure AD (Bu değer bulunabilir **Azure portalında** > **Azure Active Directory**  >   **Özellikleri** > **dizin kimliği**)
   1. **Tamam**’a tıklayın.
1. Sağ tıklatın ve kayıt defteri **yeni** > **kayıt defteri öğesi**
   1. Üzerinde **genel** sekmesinde, aşağıdakileri yapılandırın
      1. Eylem: **Güncelleştirme**
      1. Hive: **HKEY_LOCAL_MACHINE**
      1. Anahtar yolu: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Değer adı: **tenantname değeri:**
      1. Değer türü: **REG_SZ**
      1. Değer verisi: Doğrulanmış **etki alanı adı** Azure AD'de (örneğin, `contoso.onmicrosoft.com` veya herhangi bir doğrulanmış etki alanı ad dizininizdeki)
   1. **Tamam**’a tıklayın.
1. Yeni oluşturulan GPO için düzenleyiciyi kapatın
1. İstenen kuruluş, denetlenen piyasaya çıkma popülasyona ait bir etki alanına katılmış bilgisayarları içeren yeni oluşturulan GPO'ya bağlantı

### <a name="configure-ad-fs-settings"></a>AD FS ayarlarını yapılandırma

AD FS kullanıyorsanız, önce yukarıdaki belirtiliyor ancak bu GPO, AD FS sunucuları için bağlama yönergeleri kullanarak istemci tarafı SCP'yi yapılandırmanız gerekir. Bu yapılandırma, cihaz kimliklerini kaynağı Azure AD kurmak AD FS için gereklidir.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-down-level-devices"></a>Windows alt düzey cihazlar karma Azure AD join denetimli doğrulama

Windows alt düzey cihazları kaydetmek için kuruluşlar yüklemelisiniz [Microsoft Workplace Join Windows 10 bilgisayarlar için](https://www.microsoft.com/download/details.aspx?id=53554) Microsoft Download Center üzerinde kullanılabilir.

Gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Paket sessiz parametresiyle standart sessiz yükleme seçeneklerini destekler. Geçerli dal Configuration Manager'ın önceki sürümlerinde, tamamlanan kayıtları izleme yeteneği gibi üzerinden avantaj sunar.

Yükleyici, kullanıcı bağlamında çalışacak sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, kullanıcı kimlik bilgileriyle Azure AD ile Azure AD kimliklerini doğruladıktan sonra cihazla sessizce birleştirir.

Cihaz kaydı denetlemek için Windows Installer paketi, seçilen Windows alt düzey cihaz grubuna dağıtmanız gerekir.

> [!NOTE]
> AD içinde bir SCP yapılandırılmamış sonra aynı yaklaşımı anlatıldığı şekilde izlemelidir [SCP için istemci tarafı kayıt defteri ayarını Yapılandırmamız](#configure-client-side-registry-setting-for-scp)) bir Grup İlkesi nesnesi (GPO) kullanarak etki alanına katılmış bilgisayarlara.


Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra otomatik olarak geçerli ve alt düzey Windows cihazlarınızı geri kalanı tarafından Azure AD ile kaydedebilirsiniz [Azure AD Connect kullanarak SCP yapılandırma](hybrid-azuread-join-managed-domains.md#configure-hybrid-azure-ad-join).

## <a name="next-steps"></a>Sonraki adımlar

[Hibrit Azure Active Directory join uygulamanızı planlama](hybrid-azuread-join-plan.md)
