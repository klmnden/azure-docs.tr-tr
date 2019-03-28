---
title: Cihazlarınızı hibrit Azure AD'ye katılma denetim | Microsoft Docs
description: Cihazlarınızı Azure Active Directory'de hibrit Azure AD'ye katılımı denetlemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93afc6f748ca9f464261c59e037a603ab6113bf8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518176"
---
# <a name="control-the-hybrid-azure-ad-join-of-your-devices"></a>Cihazlarınızın hibrit Azure AD katılımını denetleme

Hibrit Azure Active Directory (Azure AD) birleştirme, Azure AD ile şirket içi etki alanına katılmış cihazlarınızı otomatik olarak kaydedilecek bir işlemdir. Otomatik olarak kaydedilecek, tüm cihazlar burada istemediğiniz durumlar vardır. Bu örneğin, ilk dağıtım sırasında her şeyin beklendiği gibi çalıştığını doğrulamak için geçerlidir.

Bu makalede, cihazlarınızı hibrit Azure AD'ye katılımı nasıl kontrol edebilir hakkında rehberlik sağlanır. 


## <a name="prerequisites"></a>Önkoşullar

Bu makalede, alışık olduğunuz varsayılır:

-  [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)
 
-  [Hibrit Azure Active Directory join uygulamanızı planlama](hybrid-azuread-join-plan.md)

-  [Yönetilen etki alanları için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-managed-domains.md) veya [yapılandırma hibrit Azure Active Directory join Federasyon etki alanları için](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Windows cihazları denetleme

Windows masaüstü işletim sistemini çalıştıran cihazlar için desteklenen sürüm Windows 10 Yıldönümü Güncelleştirmesi (sürüm 1607) olan veya üzeri. En iyi uygulama, Windows 10 'un en son sürüme yükseltin.

Tüm Windows cihazları cihaz başlangıç veya kullanıcı Azure AD ile otomatik olarak kaydedilecek oturum açma. Bir Grup İlkesi nesnesi (GPO) veya System Center Configuration Manager'ı kullanarak bu davranışı denetleyebilirsiniz.

Windows cihazları kontrol etmek için gerekir: 


1.  **Tüm cihazlara**: Otomatik cihaz kaydını devre dışı bırakın.
2.  **Seçili cihazlara**: Otomatik cihaz kaydını etkinleştirin.

Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra tüm cihazlar için otomatik cihaz kaydını yeniden etkinleştirmek hazırsınız.



### <a name="group-policy-object"></a>Grup İlkesi nesnesi 

Aşağıdaki GPO dağıtarak cihazlarınızı cihaz kayıt davranışını kontrol edebilirsiniz: **Bilgisayarları etki alanına katılmış cihaz olarak kaydetme**.

GPO ayarlamak için:

1.  Açık **Sunucu Yöneticisi'ni**ve ardından **Araçları** > **Grup İlkesi Yönetimi**.

2.  Otomatik kaydı etkinleştirmek veya devre dışı bırakmak istediğiniz etki alanına karşılık gelen etki alanı düğümüne gidin.

3.  Sağ **Grup İlkesi nesneleri**ve ardından **yeni**.

4.  Bir ad girin (örneğin, **hibrit Azure AD'ye katılma**) Grup İlkesi nesneniz için. 

5.  **Tamam**’ı seçin.

6.  Yeni GPO'nuza sağ tıklayın ve ardından **Düzenle**.

7.  Git **Bilgisayar Yapılandırması** > **ilkeleri** > **Yönetim Şablonları** > **Windows Bileşenleri** > **cihaz kaydı**. 

8.  Sağ **bilgisayarları etki alanına katılmış cihaz olarak kaydetme**ve ardından **Düzenle**.

    > [!NOTE] 
    > Bu Grup İlkesi şablonu Grup İlkesi Yönetimi konsolunun önceki sürümlerinden yeniden adlandırıldı. Konsol önceki bir sürümünü kullanıyorsanız, Git **Bilgisayar Yapılandırması** > **ilkeleri** > **Yönetim Şablonları**  >  **Windows bileşenleri** > **cihaz kaydı** > **kayıt etki alanına katılmış, bilgisayarı cihaz olarak**. 

9.  Aşağıdaki ayarlardan birini seçin ve ardından **Uygula**:

    - **Devre dışı bırakılmış**: Otomatik cihaz kaydını önlemek için.
    - **Etkin**: Otomatik cihaz kaydını etkinleştirmek için.

10. **Tamam**’ı seçin.

Seçtiğiniz bir konuma GPO bağlamak gerekir. Örneğin, kuruluşunuzda etki alanına katılmış tüm geçerli cihazlar için bu ilke ayarlamak için GPO'yu etki alanına bağlayın. Denetlenen bir dağıtım yapmak için bir kuruluş birimi veya güvenlik grubuna ait olan etki alanına katılmış Windows cihazları için bu ilke ayarlayın.

### <a name="configuration-manager-controlled-deployment"></a>Configuration Manager'ın denetlenen dağıtımı 

Aşağıdaki istemci ayarını yapılandırarak geçerli cihazları cihaz kayıt davranışını kontrol edebilirsiniz: **Yeni Windows 10 etki alanına katılan cihazları Azure Active Directory ile otomatik olarak kaydedilecek**.

İstemci ayarını yapılandırmak için:

1.  Açık **Configuration Manager**seçin **Yönetim**ve ardından **istemci ayarları**.

2.  Özelliklerini açın **varsayılan istemci ayarları** seçip **Cloud Services**.

3.  Altında **cihaz ayarları**, için aşağıdaki ayarlardan birini seçin **yeni Windows 10 etki alanına katılan cihazları Azure Active Directory ile otomatik olarak kaydedilecek**:

    - **Hayır**: Otomatik cihaz kaydını önlemek için.
    - **Evet**: Otomatik cihaz kaydını etkinleştirmek için.

4.  **Tamam**’ı seçin.

Bu istemci ayarı, tercih ettiğiniz bir konuma bağlamak gerekir. Örneğin, kuruluşunuzdaki tüm Windows geçerli cihazlar için bu ayarı yapılandırmak için istemci ayarı etki alanına bağlayın. Denetlenen bir dağıtım yapmak için istemci geçerli bir kuruluş birimi veya güvenlik grubuna ait cihazlar için Windows etki alanına katılmış ayarını yapılandırabilirsiniz.

> [!Important]
> Önceki yapılandırma mevcut etki alanına katılmış Windows 10 cihazları üstlenir ancak yeni etki alanına katılmak cihazların hibrit Azure AD'ye katılma, Grup İlkesi uygulama olası gecikme nedeniyle tamamlamak hala deneyebilir veya Configuration Manager ayarlarını cihazlarda. 
>
> Bunu önlemek için (örneğin bir sağlama yöntemi için kullanılır) yeni bir Sysprep görüntüsü oluşturmanızı öneririz. Hibrit Azure AD'ye katılmış ve Grup İlkesi zaten var, daha önce hiç ayarlamasına veya Configuration Manager istemci ayarı olarak uygulanan bir CİHAZDAN oluşturun. Kuruluşunuzun etki alanına yeni bilgisayarlar sağlamak için yeni görüntüyü kullanmanız gerekir. 

## <a name="control-windows-down-level-devices"></a>Windows alt düzey cihazlarını denetleme

Windows alt düzey cihazları kaydetmek için karşıdan yükleme ve İndirme Merkezi'nden Windows Installer (.msi) paketi yüklemek gereken [Microsoft Workplace Join Windows 10 bilgisayarlar için](https://www.microsoft.com/download/details.aspx?id=53554) sayfası.

Gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Paket sessiz parametresiyle standart sessiz yükleme seçeneklerini destekler. Geçerli dal Configuration Manager'ın önceki sürümlerinde, tamamlanan kayıtları izleme yeteneği gibi üzerinden avantaj sunar.

Yükleyici, kullanıcının bağlamında çalışan sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, kullanıcı kimlik bilgileriyle Azure AD ile Azure AD kimliklerini doğruladıktan sonra cihazla sessizce birleştirir.

Cihaz kaydı denetlemek için Windows alt düzey cihazları yalnızca seçili bir grup için Windows Installer paketi dağıtmanız gerekir. Her şeyin beklendiği gibi çalıştığını doğruladıktan ise tüm alt düzey cihazlara paket alma hazırsınız.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)



