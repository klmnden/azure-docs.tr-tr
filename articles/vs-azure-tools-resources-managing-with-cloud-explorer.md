---
title: Bulut Gezgini ile Azure kaynaklarını yönetme | Microsoft Docs
description: Cloud Explorer göz atın ve Visual Studio içinde Azure kaynaklarınızı yönetmek için nasıl kullanılacağını öğrenin.
services: visual-studio-online
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/25/2017
ms.author: ghogen
ms.openlocfilehash: f5205bb158141a3f8e0296fefe2528d1bc5ea64c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="manage-the-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>Visual Studio Cloud Explorer'da, Azure hesaplarıyla ilişkili kaynakları yönetme
Cloud Explorer Azure kaynakları ve kaynak gruplarını görüntülemek özelliklerini inceleyin ve Visual Studio içinde anahtar Geliştirici tanılama eylemleri gerçekleştirmek sağlar. 

Gibi [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer Azure Kaynak Yöneticisi yığınında oluşturulur. Bu nedenle, Cloud Explorer anlar Azure kaynak gruplarını gibi kaynakları ve Logic apps ve API uygulamaları gibi Azure Hizmetleri ve destekliyorsa [rol tabanlı erişim denetimi](role-based-access-control/role-assignments-portal.md) (RBAC). 

## <a name="prerequisites"></a>Önkoşullar
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) ile **Azure iş yükü** seçili, ya da Visual Studio ile önceki bir sürümünü [.NET 2.9 için Microsoft Azure SDK'sı](https://www.microsoft.com/en-us/download/details.aspx?id=51657).
- Microsoft Azure hesabı - bir hesabınız yoksa, şunları yapabilirsiniz [ücretsiz deneme için kaydolun](http://go.microsoft.com/fwlink/?LinkId=623901) veya [Visual Studio abone Avantajlarınızı etkinleştirebilir](http://go.microsoft.com/fwlink/?LinkId=623901).

> [!NOTE]
> Cloud Explorer görüntülemek için seçin **Görünüm** > **Cloud Explorer** menü çubuğunda.   
> 
> 

## <a name="add-an-azure-account-to-cloud-explorer"></a>Bir Azure eklemek hesabı bulut Gezgini
Bir Azure hesabı ile ilişkili kaynakları görüntülemek için hesabı bulut Gezgini'ne eklemeniz gerekir. 

1. İçinde **Cloud Explorer**seçin **Azure hesap ayarlarını**.

    ![Cloud Explorer Azure hesabı ayarları simgesi](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Seçin **yeni hesap eklemek**. 

    ![Cloud Explorer hesabı Ekle bağlantı](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. Gözatmak istediğiniz kaynakları Azure hesabı için oturum açın. 

1. Bir Azure hesabına oturum sonra bu hesapla ilişkili abonelikleri görüntüler. Göz atın ve ardından istediğiniz hesap aboneliklerinin onay kutularını işaretleyin **Uygula**. 
 
    ![Cloud Explorer: görüntülemek için Azure aboneliklerini seçin](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. Kaynakları gözatmak istediğiniz abonelikleri seçtikten sonra bu abonelikleri ve kaynakları bulut Gezgini'nde görüntüler.

    ![Cloud Explorer kaynak bir Azure hesabı için listeleme](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>Bir Azure hesabı bulut gezgininden Kaldır 

1. İçinde **Cloud Explorer**seçin **Azure hesap ayarlarını**.

    ![Cloud Explorer Azure hesabı ayarları simgesi](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Kaldırmak istediğiniz hesabı yanındaki seçin **kaldırmak**.

    ![Cloud Explorer Azure hesabı ayarları simgesi](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a>Kaynak türleri veya kaynak gruplarını görüntüleme
Azure kaynaklarınızı görüntülemeyi ya da seçebilir **kaynak türleri** veya **kaynak grupları** görünümü.

1. İçinde **Cloud Explorer**, kaynak görünümü açılır seçin.

    ![İstenen kaynakları görünüm seçmek için bulut Explorer açılır listesi](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. Bağlam menüsünden istediğiniz görünümü seçin: 

    - **Kaynak türleri** view - üzerinde kullanılan genel görünüm [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), web uygulamaları, depolama hesapları ve sanal makineler gibi kendi türüne göre kategorize Azure kaynaklarınızı gösterir. 
    - **Kaynak grupları** view - kategorilere ayıran Azure kaynakları ile bunların ilişkili Azure kaynak grubu tarafından. Bir kaynak grubu genellikle belirli bir uygulama tarafından kullanılan Azure kaynaklarını paketidir. Azure kaynak grupları hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](./azure-resource-manager/resource-group-overview.md).

    Aşağıdaki görüntü iki kaynak görünümleri karşılaştırması göstermektedir:

    ![Cloud Explorer kaynak görünümleri karşılaştırma](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>Görüntülemek ve bulut Explorer'da kaynaklara gidin
Bir Azure kaynağı gidin ve Cloud Explorer'da bilgilerini görüntülemek için öğenin türü veya ilişkili kaynak grubunu genişletin ve ardından kaynak seçin. Bir kaynak seçtiğinizde, bilgiler iki sekmeleri - görünür **Eylemler** ve **özellikleri** - Cloud Explorer'ın altındaki. 

- **Eylemler** sekmesi - Cloud Explorer'da seçilen kaynak için gerçekleştirebileceğiniz eylemleri listeler. Bu seçenekler, bağlam menüsü görüntülemek için kaynak sağ tıklayarak da görüntüleyebilirsiniz.

- **Özellikler** sekmesi - olduğu ilişkili türü, yerel ayar ve kaynak grubu gibi kaynak özelliklerini gösterir.

Aşağıdaki resimde, her bir sekmede bir uygulama hizmeti için gördüğünüz bir örnek karşılaştırması gösterilmektedir:

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

Her kaynak eyleme sahip **portalında açık**. Bu eylem seçtiğinizde, seçilen kaynak Cloud Explorer görüntüler [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). **Portalında açık** özelliktir iç içe kaynaklarına gezinmek için kullanışlı.

Ek eylemleri ve özellik değerlerini de Azure kaynak tabanlı görünebilir. Örneğin, web uygulamaları ve mantıksal uygulamalar eylemleri de **tarayıcıda aç** ve **hata ayıklayıcısını** ek olarak **portalında açık**. Depolama hesabı blob, kuyruk veya tablo seçtiğinizde, düzenleyiciler açmak için Eylemler görüntülenir. Azure uygulamaları **URL** ve **durum** depolama kaynaklarını anahtar ve bağlantı dizesi özellikleri varken özellikleri.

## <a name="find-resources-in-cloud-explorer"></a>Cloud Explorer'da kaynakları bulun
Azure hesabı aboneliklerinizi belirli bir ada sahip kaynaklarını bulmak için ad girin **arama** Cloud Explorer'da kutusu.

![Cloud Explorer'da kaynakları bulma](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

Karakter girerken **arama** kutusu, bu karakterler karşılayan kaynakları kaynak ağacında görünür.
