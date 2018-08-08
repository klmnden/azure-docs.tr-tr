---
title: Karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazlarda yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 9ffc84009adfca60e9ae6b188b65b15e874e7d9c
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622179"
---
# <a name="how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Cihazlarınızı hibrit Azure AD'ye katılma denetleme

Hibrit Azure AD'ye katılma, Azure AD ile şirket içi etki alanına katılmış cihazlarınızı otomatik olarak kaydedilecek bir işlemdir. Otomatik olarak kaydetmek için tüm cihazlar burada istemediğiniz durumlar vardır. Bu örnek için her şeyin beklendiği gibi çalıştığını doğrulamak için ilk dağıtım sırasında true'dur.

Bu makalede, cihazlarınızı hibrit Azure AD'ye katılımı nasıl denetleyebileceğiniz hakkında rehberlik sağlar. 


## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar:

-  [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)
 
-  [Hibrit Azure Active Directory'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)

-  Hibrit Azure Active Directory join için yapılandırma [yönetilen etki alanları](hybrid-azuread-join-managed-domains.md) veya [Federasyon etki alanları](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Windows cihazları denetleme

Windows masaüstü işletim sistemini çalıştıran cihazlar için desteklenen sürüm Windows 10 Yıldönümü Güncelleştirmesi (sürüm 1607) olan veya üzeri. En iyi uygulama, Windows 10 'un en son sürüme yükseltin.

Tüm windows cihazları cihaz başlangıç veya kullanıcı Azure AD ile otomatik olarak kaydedilecek oturum açma. Bir Grup İlkesi nesnesi (GPO) veya System Center Configuration Manager ile bu davranışı denetleyebilirsiniz.

Windows cihazları kontrol etmek için gerekir: 

1.  **Tüm cihazlara**: otomatik cihaz kaydını devre dışı bırakın.
2.  **Seçili cihazlara**: otomatik cihaz kaydını etkinleştirin.

Her şeyin beklendiği gibi çalıştığını doğruladıktan, tüm cihazlar için otomatik cihaz kaydını yeniden etkinleştirmek hazır olursunuz.



## <a name="group-policy-object"></a>Grup İlkesi nesnesi 

Aşağıdaki GPO dağıtarak cihazlarınızı cihaz kayıt davranışını kontrol edebilirsiniz: **bilgisayarları etki alanına katılmış cihaz olarak kaydetme**

**GPO ayarlanacak**:

1.  Açık **Sunucu Yöneticisi'ni**ve ardından **Araçları \> Grup İlkesi Yönetimi**.

2.  Otomatik kayıt devre dışı bırakmanıza/etkinleştirmenize istediğiniz etki alanına karşılık gelen etki alanı düğümüne gidin.

3.  Sağ **Grup İlkesi nesneleri**ve ardından **yeni**.

4.  Bir ad yazın (örneğin, *hibrit Azure AD'ye katılma*) Grup İlkesi nesneniz için. 

5.  **Tamam** düğmesine tıklayın.

6.  Yeni GPO'nuza sağ tıklayın ve ardından **Düzenle**.

7.  Git **Bilgisayar Yapılandırması \> ilkeleri \> Yönetim Şablonları \> Windows bileşenleri \> cihaz kaydı**. 

8.  Sağ **bilgisayarları etki alanına katılmış cihaz olarak kaydetme**ve ardından **Düzenle**.

    > [!NOTE] 
    > Bu Grup İlkesi şablonu Grup İlkesi Yönetimi konsolunun önceki sürümlerinden yeniden adlandırıldı. Konsol önceki bir sürümünü kullanıyorsanız, Git **Bilgisayar Yapılandırması \> ilkeleri \> Yönetim Şablonları \> Windows bileşenleri \> çalışma alanına katılma \> Otomatik olarak çalışma alanına katılma istemci bilgisayarları**. 

9.  Aşağıdaki ayarlardan birini seçin ve ardından **Uygula**:

    - **Devre dışı bırakılmış** - otomatik cihaz kaydını önlemek için
    - **Etkin** - otomatik cihaz kaydını etkinleştirmek için

10. **Tamam** düğmesine tıklayın.

Seçtiğiniz bir konuma GPO bağlamak gerekir. Örneğin, kuruluşunuzda etki alanına katılmış tüm geçerli cihazlar için bu ilke ayarlamak için GPO'yu etki alanına bağlayın. Denetlenen bir dağıtım yapmak için bir kuruluş birimi veya güvenlik grubuna ait olan etki alanına katılmış Windows cihazları için bu ilke ayarlayın.

### <a name="configuration-manager-controlled-deployment"></a>Configuration Manager'ın denetlenen dağıtımı 

Aşağıdaki istemci ayarını yapılandırarak geçerli cihazları cihaz kayıt davranışını kontrol edebilirsiniz: ** yeni Windows 10 etki alanına katılan cihazları azure Active Directory ile otomatik olarak kaydedilecek **


**İstemci ayarını ayarlamak için**:

1.  Açık **Configuration Manager**ve ardından **Cloud Services**.

2.  Altında **cihaz ayarları**, için aşağıdaki ayarlardan birini seçin **yeni Windows 10 etki alanına katılan cihazları azure Active Directory ile otomatik olarak kaydedilecek**:

    - **Hayır** - otomatik cihaz kaydını önlemek için
    - **Evet** - otomatik cihaz kaydını etkinleştirmek için


3.  **Tamam** düğmesine tıklayın.
    

Bu istemci ayarı, tercih ettiğiniz bir konuma bağlamak gerekir. Örneğin, kuruluşunuzdaki tüm Windows geçerli cihazlar için bu ayarı yapılandırmak için istemci ayarı etki alanına bağlayın. Denetlenen bir dağıtım yapmak için istemci geçerli bir kuruluş birimi veya güvenlik grubuna ait cihazlar için Windows etki alanına katılmış ayarını yapılandırabilirsiniz.

> [!Important]
> Yukarıdaki yapılandırma varolan ilgilenirken etki alanına katılmış Windows 10 cihazlar, yeni etki alanına katılan cihazlar için eksiksiz hibrit Azure AD'ye katılım gerçek bir uygulamada Grup İlkesi'nin olası gecikme nedeniyle hala denemek için olası yoktur veya Yapılandırma Yöneticisi ayarlarını yeni Windows 10 cihaz etki alanına katılmış. Bunu önlemek için daha önce hiç hibrit Azure AD'ye katılmış bir CİHAZDAN (örneğin bir sağlama yöntemi için kullanılır) yeni bir sysprep görüntüsü oluşturma ve zaten uygulanan Yukarıdaki Grup İlkesi ayarı veya Configuration Manager istemcisi yüklü olduğunu önerilir. uygulanan ayarı. Kuruluşunuzun etki alanına yeni bilgisayarlar sağlamak için yeni görüntüyü kullanmanız gerekir. 

## <a name="control-windows-down-level-devices"></a>Windows alt düzey cihazları denetleme

Windows alt düzey cihazları kaydetmek için karşıdan yükle ve Itanium tabanlı sistemler için Windows Installer paketi (.msi) İndirme Merkezi'nden yüklemek için ihtiyaç duyduğunuz [Microsoft Workplace Join Windows 10 bilgisayarlar için](https://www.microsoft.com/en-us/download/details.aspx?id=53554) sayfası.

System Center Configuration Manager gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz. Paket sessiz parametresiyle standart sessiz yükleme seçeneklerini destekler. [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager) geçerli dal tamamlanmış kayıtları izleme yeteneği gibi daha önceki sürümlerin ek avantajlar sunar.

Yükleyici, kullanıcının bağlamında çalışan sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, kullanıcı kimlik bilgileriyle Azure AD ile Azure AD kimliklerini doğruladıktan sonra cihazla sessizce birleştirir.


Cihaz kaydı denetlemek için Windows alt düzey cihazları yalnızca seçili bir grup için Windows Installer paketi dağıtmanız gerekir. Her şeyin beklendiği gibi çalıştığını doğruladıktan, dağıtım paketi tüm alt düzey cihazları için hazır olursunuz.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)



