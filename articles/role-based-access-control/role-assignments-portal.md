---
title: Azure portalında Rol Tabanlı Access Control | Microsoft Docs
description: Azure Portal'da Rol Tabanlı Erişim Denetimi ile erişim yönetimine başlayın. Kaynaklarınıza izinler atamak için rol atamalarını kullanın.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 35cb4be5c1ee24bba8026cbe66d2316753f9ed32
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-role-based-access-control-to-manage-access-to-your-azure-subscription-resources"></a>Azure abonelik kaynaklarınıza erişimi yönetmek için Rol Tabanlı Erişim Denetimi kullanma
> [!div class="op_single_selector"]
> * [Kullanıcı veya gruba göre erişimi yönetme](role-assignments-users.md)
> * [Kaynağa göre erişimi yönetme](role-assignments-portal.md)

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz. Bu makale, Azure portalında RBAC ile çalışmaya başlamanıza yardımcı olur. RBAC'nin erişimi yönetmenize nasıl yardımcı olduğu konusunda daha fazla bilgi isterseniz bkz. [Rol Tabanlı Erişim Denetimi Nedir?](overview.md).

Her abonelikte en fazla 2000 rol ataması verebilirsiniz. 

## <a name="view-access"></a>Erişimi görüntüleme
Kimin bir kaynağa, kaynak grubuna veya aboneliğe erişimi olduğunu [Azure portal](https://portal.azure.com)'da ana dikey penceresinde görebilirsiniz. Örneğin, kimin kaynak gruplarımızdan birine erişimi olduğunu görmek istiyoruz:

1. Soldaki gezinti çubuğunda **Kaynak grupları** seçeneğini belirleyin.  
    ![Kaynak grupları - simge](./media/role-assignments-portal/resourcegroups_icon.png)
2. **Kaynak grupları** dikey penceresinde kaynak grubunun adını seçin.
3. Soldaki menüden **Erişim denetimi (IAM)** öğesini seçin.  
4. Erişim denetimi dikey penceresi, kaynak grubuna erişim verilen tüm kullanıcıları, grupları ve uygulamaları listeler.  
   
    ![Kullanıcılar dikey penceresi - devralınmış veya atanmış erişim ekran görüntüsü](./media/role-assignments-portal/view-access.png)

Bazı rollerin kapsamı **Bu kaynak** olarak belirlenmişken diğerlerinin başka bir kapsamdan **Devralınmış** olduğuna dikkat edin. Erişim, özellikle kaynak grubuna atanmış veya üst aboneliğe yapılan atamadan devralınmış olabilir.

> [!NOTE]
> Yeni RBAC modelinde, klasik abonelik yöneticileri ve ortak yöneticileri aboneliğin sahipleri olarak kabul edilir.

## <a name="add-access"></a>Erişim Ekleme
Rol atamasının kapsamı olan kaynak, kaynak grubu veya abonelik içinden erişim verebilirsiniz.

1. Erişim denetimi dikey penceresinde **Ekle**’yi seçin.  
2. **Rol seçin** dikey penceresinde atamak istediğiniz rolü seçin.
3. Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.  
   
    ![Kullanıcı ekle dikey penceresi - arama ekran görüntüsü](./media/role-assignments-portal/grant-access2.png)
4. Atamayı oluşturmak için **Tamam**'ı seçin. **Kullanıcı ekleme** açılır penceresi ilerleme durumunu izler.  
    ![Kullanıcı ekleniyor ilerleme çubuğu - ekran görüntüsü](./media/role-assignments-portal/addinguser_popup.png)

Bir rol ataması başarıyla eklendikten sonra **Kullanıcılar** dikey penceresinde görüntülenir.

## <a name="remove-access"></a>Erişimi Kaldırma
1. İmlecinizi kaldırmak istediğiniz atama adının üzerine getirin. Adın yanında bir onay kutusu görüntülenir.
2. Onay kutularını kullanarak bir veya daha fazla rol ataması seçin.
2. **Kaldır**’ı seçin.  
3. Kaldırma işlemini onaylamak için **Evet**'i seçin.

Devralınmış atamalar kaldırılamaz. Devralınmış bir atamayı kaldırmanız gerekiyorsa bu işlemi rol atamasının oluşturulduğu kapsamda gerçekleştirmeniz gerekir. **Kapsam** sütunundaki **Devralınmış** seçeneğinin yanında, sizi rolün oluşturulduğu kaynaklara götüren bir bağlantı yer alır. Rol atamasını kaldırmak için orada listelenen kaynağa gidin.

![Kullanıcılar dikey penceresi - devralınmış erişim kaldır düğmesini devre dışı bırakır ekran görüntüsü](./media/role-assignments-portal/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Erişimi yönetmeye yönelik diğer araçlar
Azure portal dışındaki araçlarda Azure RBAC komutları ile roller atayabilir ve erişimi yönetebilirsiniz.  Önkoşullar hakkında daha fazla bilgi edinmek ve Azure RBAC komutlarını kullanmaya başlamak için bağlantıları izleyin.

* [Azure PowerShell](role-assignments-powershell.md)
* [Azure Komut Satırı Arabirimi](role-assignments-cli.md)
* [REST API](role-assignments-rest.md)

## <a name="next-steps"></a>Sonraki Adımlar
* [Erişim değişiklik geçmişi raporu oluşturma](change-history-report.md)
* Bkz. [RBAC yerleşik rolleri](built-in-roles.md)
* Kendiniz için [Azure RBAC'de özel roller](custom-roles.md) tanımlama

