---
title: Uygulamaları Azure Blockchain Workbench uygulamasında kullanma
description: Uygulamayı kullanma hakkında öğretici, Azure Blockchain Workbench uygulamasında daraltır.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 04/15/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 89c83ed6d02a60978bd54fb97d37063e34f6c0c7
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578859"
---
# <a name="tutorial-using-applications-in-azure-blockchain-workbench"></a>Öğretici: Uygulamaları Azure Blockchain Workbench uygulamasında kullanma

Blockchain Workbench'i oluşturup sözleşmelerinde eylemleri için kullanabilirsiniz. Ayrıca görüntüleyebilirsiniz Sözleşme durumu ve işlem geçmişi gibi ayrıntıları.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Yeni sözleşme oluşturma
> * Bir sözleşmede bir eylem

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Blockchain Workbench'i dağıtımı. Daha fazla bilgi için [Azure Blockchain Workbench dağıtım](deploy.md) dağıtımı hakkında ayrıntılı bilgi için
* Blockchain Workbench'i içinde bir dağıtılan blok zinciri uygulaması. Bkz: [Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma](create-app.md)

[Blockchain Workbench'i açın](deploy.md#blockchain-workbench-web-url) tarayıcınızda.

![Blockchain Workbench](./media/use/workbench.png)

Blockchain Workbench'i üyesi olarak oturum açmanız gerekir. Listelenen hiçbir uygulamalar varsa, Blockchain Workbench'i üyesi ancak herhangi bir uygulama üyesi olursunuz. Blockchain Workbench'i yönetici üyeleri için uygulamaları atayabilirsiniz.

## <a name="create-new-contract"></a>Yeni sözleşme oluşturma

Yeni bir sözleşme oluşturmak için bir sözleşme belirtilen bir üyesi olmanız gerekir **Başlatıcı**. Uygulama rolleri ve sözleşmenin Başlatıcı tanımlama bilgi için bkz: [yapılandırmasına genel bakış akışlarında](configuration.md#workflows). Uygulama rollerine üye atama hakkında daha fazla bilgi için bkz: [üye için uygulama ekleme](manage-users.md#add-member-to-application).

1. Blockchain Workbench'i uygulama bölümünde, oluşturmak istediğiniz sözleşme içeren uygulama kutucuğu seçin. Etkin sözleşme listesi görüntülenir.

2. Yeni bir sözleşme oluşturmak için Seç **yeni sözleşme**.

    ![Yeni sözleşme düğmesi](./media/use/contract-list.png)

3. **Yeni sözleşme** bölmesi görüntülenir. İlk parametre değerlerini belirtin. **Oluştur**’u seçin.

    ![Yeni sözleşme bölmesi](./media/use/new-contract.png)

    Yeni oluşturulan sözleşme etkin bir sözleşme birlikte listede görüntülenir.

    ![Etkin sözleşme listesi](./media/use/active-contracts.png)

## <a name="take-action-on-contract"></a>Sözleşme eylem

Durumuna bağlı olarak bir sözleşmedir, üyeleri sözleşmenin sonraki duruma geçiş eylemleri gerçekleştirebilirsiniz. Eylemler olarak tanımlanan [geçişleri](configuration.md#transitions) içinde bir [durumu](configuration.md#states). Geçiş için izin verilen bir uygulama veya örnek rolüne ait olan üyeleri işlemleri gerçekleştirebilir. 

1. Blockchain Workbench'i uygulama bölümünde, eylemi gerçekleştirmek için sözleşme içeren uygulama kutucuğu seçin.
2. Sözleşme listeden seçin. Anlaşma ayrıntılarını farklı bölümlerde görüntülenir. 

    ![Anlaşma ayrıntıları](./media/use/contract-details.png)

    | Section  | Açıklama  |
    |---------|---------|
    | Durum | Sözleşme aşaması geçerli ilerleme durumunu listeler |
    | Ayrıntılar | Sözleşme geçerli değerleri |
    | Eylem | Son eylem hakkındaki ayrıntıları |
    | Etkinlik | Sözleşmenin işlem geçmişi |
    
3. İçinde **eylem** bölümünden **harekete**.

4. Sözleşme geçerli durumuyla ilgili ayrıntıları bir bölmede görüntülenir. Açılan menü gerçekleştirmek istediğiniz eylemi seçin. 

    ![Eylem seçin](./media/use/choose-action.png)

5. Seçin **harekete** eylemi başlatmak için.
6. Eylem için parametreler gerekiyorsa, eylem değerlerini belirtin.

    ![İşlem yap](./media/use/take-action.png)

7. Seçin **harekete** eylemi yürütülemedi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench uygulama sürümü](version-app.md)
