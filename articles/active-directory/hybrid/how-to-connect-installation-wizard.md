---
title: Azure AD Connect'i yeniden çalıştırma Yükleme Sihirbazı | Microsoft Docs
description: Yükleme Sihirbazı'nı ikinci nasıl çalıştığını açıklayan, çalışma zamanı.
keywords: Azure AD Connect Yükleme Sihirbazı'nı çalıştırmadan ikinci kez bakım ayarlarını yapılandırmanıza olanak sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ff2caae7cb387f4f0d88cf059d01ad28861b9ad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60348459"
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect eşitleme: Yükleme sihirbazını ikinci kez çalıştırma
Azure AD Connect Yükleme Sihirbazı ilk çalıştırıldığında bu, yüklemenizi yapılandırmak hakkında bilgi vermektedir. Yükleme Sihirbazı'nı yeniden çalıştırırsanız, bakım için seçenekler sunar.

Yükleme Sihirbazı'nı adlı Başlat menüsünde bulabilirsiniz **Azure AD Connect**.

![Başlat menüsü](./media/how-to-connect-installation-wizard/startmenu.png)

Yükleme sihirbazını başlatmak için bu seçenekleri içeren bir sayfa görürsünüz:

![Ek görevler listesi sayfası](./media/how-to-connect-installation-wizard/additionaltasks.png)

Azure AD Connect ile AD FS yüklü değilse, daha da fazla seçeneğiniz vardır. Sahip ADFS bölümünde belgelendirilen için ek seçenekler [ADFS Yönetim](how-to-connect-fed-management.md#manage-ad-fs).

Görevlerden birini seçin ve tıklayın **sonraki** devam etmek için.

> [!IMPORTANT]
> Yükleme Sihirbazı'nı açın sahip olsa da, eşitleme altyapısı tüm işlemleri askıya alınır. Yapılandırma Değişikliklerinizi tamamladıktan hemen sonra yükleme sihirbazını kapatmak emin olun.
>
>

## <a name="view-current-configuration"></a>Geçerli yapılandırmayı görüntüleme
Bu seçenek şu anda yapılandırılmış seçeneklerinizi hızlı bir görünümünü sağlar.

![Tüm seçenekleri ve bunların durumunu Listesi Sayfası](./media/how-to-connect-installation-wizard/viewconfig.png)

Tıklayın **önceki** geri dönmek için. Seçerseniz **çıkış**, Yükleme Sihirbazı'nı kapatın.

## <a name="customize-synchronization-options"></a>Eşitleme seçeneklerini özelleştirme
Bu seçenek, eşitleme yapılandırmada değişiklik yapmak için kullanılır. Özel yapılandırma yükleme yolundan seçenekler kümesini görürsünüz. Hızlı yükleme başlangıçta kullanılsa bile bu seçeneği görürsünüz.

* [Dizin Ekle](how-to-connect-install-custom.md#connect-your-directories). Bir dizin kaldırmak için bkz: [bir bağlayıcıyı silmeyi](how-to-connect-sync-service-manager-ui-connectors.md#delete).
* [Etki alanı ve OU filtreleme](how-to-connect-install-custom.md#domain-and-ou-filtering).
* Grup filtresini kaldırın.
* [İsteğe bağlı özellikler değiştirme](how-to-connect-install-custom.md#optional-features).

Diğer seçenekleri ilk yüklemesinden değiştirilemez ve kullanılabilir değil. Bu seçenekler şunlardır:

* UserPrincipalName ve sourceAnchor için kullanılacak özniteliği değiştirin.
* Katılma yöntemi nesneler için farklı bir ormandan değiştirin.
* Grup tabanlı filtreleme etkinleştirin.

## <a name="refresh-directory-schema"></a>Dizin şemasını yenile
Şema şirket içi birini değiştirdiyseniz, bu seçenek kullanılır AD DS ormanlarını. Örneğin,'ü Exchange yüklemiş veya cihaz nesneleri ile bir Windows Server 2012 şema için yükseltilir. Bu durumda, şemayı yeniden AD DS'den okuma ve önbelleğinde güncelleştirmek için Azure AD Connect istemek gerekir. Bu eylem ayrıca eşitleme kuralları yeniden oluşturur. Örneğin Exchange şema eklerseniz, Exchange için eşitleme kuralları yapılandırmaya eklenir.

Bu seçeneği belirlediğinizde, yapılandırmanızda tüm dizinleri listelenir. Varsayılan ayar tutun ve tüm ormanlarda yenileyin veya bunlardan bazıları seçimini kaldırın.

![Ortamdaki tüm dizinler Listesi Sayfası](./media/how-to-connect-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Hazırlama modunu yapılandırma
Bu seçenek, etkinleştirmek ve sunucuda hazırlama modunu devre dışı bırakmak sağlar. Hazırlama modu ve nasıl kullanıldıkları hakkında daha fazla bilgi bulunabilir [Operations](how-to-connect-sync-staging-server.md).

Seçeneği, hazırlama şu anda etkin olup olmadığını gösterir:  
![Hazırlama modunu geçerli durumunu da gösterildiğini seçeneği](./media/how-to-connect-installation-wizard/stagingmodecurrentstate.png)

Durumu değiştirmek için bu seçeneği belirleyin ve seçin veya onay kutusunun işaretini kaldırın.  
![Hazırlama modunu geçerli durumunu da gösterildiğini seçeneği](./media/how-to-connect-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Kullanıcı oturumunu değiştir
Bu seçenek için ve parola karma eşitlemesi, geçişli kimlik doğrulaması ya da Federasyon oturum açma kullanıcı yöntemi değiştirmenize olanak sağlar. Değerine değiştirilemiyor **yapılandırmayın**.

Bu seçenek hakkında daha fazla bilgi için bkz. [kullanıcı oturum açma](plan-connect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Sonraki adımlar
* Azure AD Connect eşitleme tarafından kullanılan yapılandırma modeli hakkında daha fazla bilgi [anlama bildirim temelli sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md).

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
