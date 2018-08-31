---
title: PIM'de Azure kaynak rol ayarlarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rol ayarlarını yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 901eb5ef43ddb2840ed7a3d83fc08f2f05849461
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189744"
---
# <a name="configure-azure-resource-role-settings-in-pim"></a>PIM'de Azure kaynak rol ayarlarını yapılandırma

Rol ayarlarını yapılandırdığınızda, Privileged Identity Management (PIM) ortamında atamaları uygulanan varsayılan ayarları tanımlar. Kaynağınız için bu ayarları tanımlamak için seçin **rol ayarları** sol bölmeden sekmesi. Siz de rol ayarları düğmesi eylem çubuğunda (herhangi bir rolü) geçerli seçeneklerini görüntülemek için seçebilirsiniz.

## <a name="overview"></a>Genel Bakış

Onay iş akışı ile Privileged Identity Management (PIM), Azure kaynak rolleri için Yöneticiler daha fazla koruyabilir veya kritik kaynaklara erişimi kısıtlayın. Diğer bir deyişle, yöneticileri, rol atamalarını etkinleştirmek için onay isteyebilir. 

Azure kaynak rolleri için benzersiz bir kaynak hiyerarşisinin kavramdır. Bu hiyerarşi üst kapsayıcı içindeki tüm alt kaynaklar için rol atamalarını aşağı üst kaynak nesneden devralınmasını sağlar. 

Örneğin: Bob, bir kaynak yöneticisi, uygun bir üye olarak Esra'nın Contoso abonelik sahibi rolüne atamak için PIM kullanır. Bu atama ile Esra'nın Contoso Abonelikteki tüm kaynak grubu kapsayıcıları bir uygun sahibidir. Gamze ayrıca bir uygun tüm kaynakları (sanal makineler gibi) abonelik her bir kaynak grubu içinde sahibidir. 

Contoso abonelikte üç kaynak grubunuz yok varsayalım: Fabrikam Test, Fabrikam geliştirme ve Fabrikam ürün. Bu kaynak gruplarının her biri tek bir sanal makine içeriyor.

PIM ayarları, her bir kaynağın rolünü için yapılandırılır. Atamaları aksine bu ayarları yok devralınır ve kesinlikle Kaynak rolü için geçerlidir. [Uygun atamalar ve Kaynak görünürlüğü hakkında daha fazla bilgiyi](pim-resource-roles-eligible-visibility.md).

Örneğiyle devam etmesini: Bob PIM Contoso abonelik isteği onay sahibi roldeki tüm üyeleri etkinleştirilmesini istemek için kullanır. Fabrikam Prod kaynak grubundaki kaynakları korumak için Bob Ayrıca bu kaynağın sahip rolünün üyeleri için onay gerektirir. Fabrikam Test ve geliştirme Fabrikam sahip roller, etkinleştirme için onay gerekmez.

Alice Contoso abonelik için sahip rolünü etkinleştirme isteğinde bulunduğunda, bir onaylayan onaylayabilir veya o roldeki etkinleştirilmeden önce her bir isteği reddetmek gerekir. Alice karar verirse [her etkinleştirme kapsam](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) Fabrikam Prod kaynak grubunu, bir onaylayan gerekir onaylayın veya çok bu isteği reddedin. Ancak, Esra'nın Fabrikam Test veya geliştirme Fabrikam biri veya her ikisi için her etkinleştirme kapsam karar verirse, onayı gerekli değildir.

Onay iş akışı, bir rolün tüm üyeleri için gerekli olmayabilir. Burada, kuruluşunuzun bir Azure aboneliğinde çalıştırılan uygulamanın geliştirilmesine yardımcı olmak için birkaç sözleşme ilişkilendirir hires bir senaryo düşünün. Bir kaynak yöneticisi, gerekli onay uygun erişim sağlamak için çalışanların istediğiniz, ancak sözleşme ilişkilendirir onay istemelisiniz. Onay iş akışı için yalnızca sözleşme ilişkilendirir yapılandırmak için atanmış çalışanlara rolle aynı izinlere sahip bir özel rol oluşturabilirsiniz. Bu özel rolü etkinleştirmek için onay gerektirebilir. [Özel roller hakkında daha fazla bilgi](pim-resource-roles-custom-role-policy.md).

Onay iş akışını yapılandırın ve kimler onaylayabilir veya istekleri reddetme belirtmek için aşağıdaki yordamları kullanın.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektir

1. PIM için Azure portalında göz atın ve listeden bir kaynak seçin.

   ![Seçilen kaynak "Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. Sol bölmeden **rol ayarları**.

3. İçin arama yapın ve bir rol seçin ve ardından **Düzenle** ayarlarını değiştirmek için.

   !["Düzenle" düğmesine operatörü rolü için](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. İçinde **etkinleştirme** bölümünden **etkinleştirmek için onay gerektir** onay kutusu.

   ![Rol ayarları "Etkinleştirme" bölümü](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Onaylayanlar belirtin

Tıklayın **onaylayanları seçin** açmak için **bir kullanıcı veya Grup Seç** bölmesi.

>[!NOTE]
>En az bir kullanıcı veya grup ayarını güncelleştirmek için seçmeniz gerekir. Hiçbir varsayılan onaylayanlar vardır.

Kaynak yöneticileri, kullanıcıları ve grupları herhangi bir birleşimini onaylayanlar listesine ekleyebilirsiniz. 

![Kullanıcı tarafından seçilen bir "bir kullanıcı veya Grup Seç" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Etkinleştirmek için onay isteyin

Onay isteme üyesi etkinleştirmek için izlemeniz gereken yordam üzerinde hiçbir etkisi olmaz. [Bir rolü etkinleştirmek için adımları gözden](pim-resource-roles-activate-your-roles.md).

Onay gerektiren bir rolü etkinleştirmesi üyesi istenen ve rol artık gerekli değildir, üye PIM isteğini iptal edebilirsiniz.

İptal etmek için PIM ve seçin için Gözat **isteklerim**. Seçin ve istek bulun **iptal**.

!["İsteklerim" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri için çok faktörlü kimlik doğrulaması gerektir](pim-resource-roles-require-mfa.md)
- [Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma](pim-resource-roles-configure-alerts.md)
