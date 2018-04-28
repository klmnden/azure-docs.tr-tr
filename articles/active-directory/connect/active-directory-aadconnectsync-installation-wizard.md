---
title: Azure AD Connect'i yeniden çalıştırmayı Yükleme Sihirbazı'nı | Microsoft Docs
description: Yükleme Sihirbazı'nı ikinci nasıl çalıştığını açıklar, çalışma zamanı.
keywords: Azure AD Connect Yükleme Sihirbazı'nı ikinci çalıştırdığınızda bakım ayarlarını yapılandırmanıza olanak sağlar
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 56cc38275a23eb4529558b876db619768a885a25
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect eşitleme: Yükleme Sihirbazı'nı ikinci kez çalıştırma
İlk kez Azure AD Connect Yükleme Sihirbazı'nı çalıştırdığınızda, yüklemenizi yapılandırma konusunda aracılığıyla açıklanmaktadır. Yükleme Sihirbazı'nı yeniden çalıştırın, Bakım seçenekleri sunar.

Yükleme Sihirbazı'nı adlı Başlat menüsünde bulabilirsiniz **Azure AD Connect**.

![Başlat menüsü](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Yükleme Sihirbazı'nı başlatmak için bu seçenekleri içeren bir sayfa görürsünüz:

![Ek görevler listesi sayfası](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Azure AD Connect ile ADFS yüklediyseniz daha da fazla seçeneğiniz vardır. ADFS belgelenmiştir için sahip olduğunuz ek seçenekler [ADFS Yönetim](active-directory-aadconnect-federation-management.md#manage-ad-fs).

Görevlerinden birini seçin ve tıklatın **sonraki** devam etmek için.

> [!IMPORTANT]
> Yükleme Sihirbazı'nı açın sahip olsa da, eşitleme Altyapısı'ndaki tüm işlemleri askıya alınır. Yapılandırma değişiklikleri tamamladıktan hemen sonra Yükleme Sihirbazı'nı kapatın emin olun.
>
>

## <a name="view-current-configuration"></a>Geçerli yapılandırmayı görüntüleme
Bu seçenek şu anda yapılandırılmış seçeneklerinizi hızlı bir görünümünü sağlar.

![Tüm seçenekler ve durumlarına Listesi Sayfası](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Tıklatın **önceki** geri dönmek için. Seçerseniz **çıkış**, Yükleme Sihirbazı'nı kapatın.

## <a name="customize-synchronization-options"></a>Eşitleme seçeneklerini özelleştirme
Bu seçenek, eşitleme yapılandırma değişiklikleri yapmak için kullanılır. Özel yapılandırma yükleme yolundan seçeneklerden bir alt bakın. Hızlı yükleme başlangıçta kullanılan olsa bile, bu seçenek görürsünüz.

* [Daha fazla dizinler eklemek](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Bir dizin kaldırmak için bkz: [bir bağlayıcıyı silmek](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
* [Değişiklik etki alanı ve OU filtreleme](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
* Grup filtresini kaldırın.
* [İsteğe bağlı özellikler değiştirmek](active-directory-aadconnect-get-started-custom.md#optional-features).

İlk yükleme diğer seçenekler değiştirilemez ve kullanılabilir değil. Bu seçenekler şunlardır:

* UserPrincipalName ve sourceAnchor için kullanılacak özniteliği değiştirin.
* Katılma yöntemi nesneler için farklı ormandan değiştirin.
* Grup tabanlı filtreleme etkinleştirin.

## <a name="refresh-directory-schema"></a>Dizin şemasını yenile
Şema şirket içi birini değiştirdiyseniz, bu seçenek kullanılır AD DS orman. Örneğin, Exchange yüklediyseniz veya bir Windows Server 2012 şemasına aygıt nesneleri ile yükseltme. Bu durumda, şema yeniden AD DS'den okuma ve önbelleğinde güncelleştirmek için Azure AD Connect istemeniz gerekir. Bu eylem ayrıca eşitleme kuralları yeniden oluşturur. Örnek olarak Exchange şema eklerseniz, Exchange için eşitleme kurallarını yapılandırma eklenir.

Bu seçeneği belirlediğinizde, yapılandırmanızda tüm dizinleri listelenir. Varsayılan ayar tutun ve tüm ormanlarda yenileyin veya bunlardan bazıları seçimini kaldırın.

![Ortamdaki tüm dizinlerin listesini sayfası](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Hazırlama modunu yapılandırma
Bu seçeneği etkinleştirmek ve sunucuda hazırlama modunu devre dışı bırakmak sağlar. Hazırlama modu ve nasıl kullanıldığı hakkında daha fazla bilgi bulunabilir [Operations](active-directory-aadconnectsync-operations.md#staging-mode).

Seçenek hazırlama şu anda etkin veya devre dışı olmadığını gösterir:  
![Ayrıca hazırlama modunu geçerli durumunu gösteren seçeneği](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Durumu değiştirmek için bu seçeneği belirleyin ve seçin veya onay kutusunun seçimini kaldırın.  
![Ayrıca hazırlama modunu geçerli durumunu gösteren seçeneği](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Kullanıcı oturumunu değiştir
Bu seçenek için ve parola karma eşitlemesi, geçişli kimlik doğrulaması ya da Federasyon oturum açma kullanıcı yöntemini değiştirmenize olanak tanır. Çeviremezsiniz **yapılandırmayın**.

Bu seçenek hakkında daha fazla bilgi için bkz: [kullanıcı oturum açma](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Sonraki adımlar
* Azure AD Connect eşitleme tarafından kullanılan yapılandırma modeli hakkında daha fazla bilgi [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
