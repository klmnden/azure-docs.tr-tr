---
title: Uygulamaları Azure Blockchain Workbench uygulamasında kullanma
description: Uygulamayı kullanmak nasıl Azure Blockchain Workbench uygulamasında daraltır.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: e17a9275792e3a7fdbea6e3b95e596eaa15f4359
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105820"
---
# <a name="using-applications-in-azure-blockchain-workbench"></a>Uygulamaları Azure Blockchain Workbench uygulamasında kullanma

Blockchain Workbench'i oluşturup sözleşmelerinde eylemleri için kullanabilirsiniz. Ayrıca görüntüleyebilirsiniz Sözleşme durumu ve işlem geçmişi gibi ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

* Blockchain Workbench'i dağıtımı. Daha fazla bilgi için [Azure Blockchain Workbench dağıtım](blockchain-workbench-deploy.md) dağıtımı hakkında ayrıntılı bilgi için
* Blockchain Workbench'i içinde bir dağıtılan blok zinciri uygulaması. Bkz: [Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma](blockchain-workbench-create-app.md)

[Blockchain Workbench'i açın](blockchain-workbench-deploy.md#blockchain-workbench-web-url) tarayıcınızda.

![Blockchain Workbench](media/blockchain-workbench-use/workbench.png)

Blockchain Workbench'i üyesi olarak oturum açmanız gerekir. Listelenen hiçbir uygulamalar varsa, Blockchain Workbench'i üyesi ancak herhangi bir uygulama üyesi olursunuz. Blockchain Workbench'i yönetici üyeleri için uygulamaları atayabilirsiniz.

## <a name="create-new-contract"></a>Yeni sözleşme oluşturma 

Yeni bir sözleşme oluşturmak için bir sözleşme belirtilen bir üyesi olmanız gerekir **Başlatıcı**. Uygulama rolleri ve sözleşmenin Başlatıcı tanımlama bilgi için bkz: [yapılandırmasına genel bakış akışlarında](blockchain-workbench-configuration-overview.md#workflows). Uygulama rollerine üye atama hakkında daha fazla bilgi için bkz: [üye için uygulama ekleme](blockchain-workbench-manage-users.md#add-member-to-application).

1. Blockchain Workbench'i uygulama bölümünde, oluşturmak istediğiniz sözleşme içeren uygulama kutucuğu seçin. Etkin sözleşme listesi görüntülenir.

2. Yeni bir sözleşme oluşturmak için Seç **yeni sözleşme**.

    ![Yeni sözleşme düğmesi](media/blockchain-workbench-use/contract-list.png)

3. **Yeni sözleşme** bölmesi görüntülenir. İlk parametre değerlerini belirtin. **Oluştur**’u seçin.

    ![Yeni sözleşme bölmesi](media/blockchain-workbench-use/new-contract.png)

    Yeni oluşturulan sözleşme etkin bir sözleşme birlikte listede görüntülenir.

    ![Etkin sözleşme listesi](media/blockchain-workbench-use/active-contracts.png)

## <a name="take-action-on-contract"></a>Sözleşme eylem

Durumuna bağlı olarak bir sözleşmedir, üyeleri sözleşmenin sonraki duruma geçiş eylemleri gerçekleştirebilirsiniz. Eylemler olarak tanımlanan [geçişleri](blockchain-workbench-configuration-overview.md#transitions) içinde bir [durumu](blockchain-workbench-configuration-overview.md#states). Geçiş için izin verilen bir uygulama veya örnek rolüne ait olan üyeleri işlemleri gerçekleştirebilir. 

1. Blockchain Workbench'i uygulama bölümünde, eylemi gerçekleştirmek için sözleşme içeren uygulama kutucuğu seçin.
2. Sözleşme listeden seçin. Anlaşma ayrıntılarını farklı bölümlerde görüntülenir. 

    ![Anlaşma ayrıntıları](media/blockchain-workbench-use/contract-details.png)

    | Section  | Açıklama  |
    |---------|---------|
    | Durum | Sözleşme aşaması geçerli ilerleme durumunu listeler |
    | Ayrıntılar | Sözleşme geçerli değerleri |
    | Eylem | Son eylem hakkındaki ayrıntıları |
    | Etkinlik | Sözleşmenin işlem geçmişi |
    
3. İçinde **eylem** bölümünden **harekete**.

4. Sözleşme geçerli durumuyla ilgili ayrıntıları bir bölmede görüntülenir. Açılan menü gerçekleştirmek istediğiniz eylemi seçin. 

    ![Eylem seçin](media/blockchain-workbench-use/choose-action.png)

5. Seçin **harekete** eylemi başlatmak için.
6. Eylem için parametreler gerekiyorsa, eylem değerlerini belirtin.

    ![İşlem yap](media/blockchain-workbench-use/take-action.png)

7. Seçin **harekete** eylemi yürütülemedi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench sorunlarını giderme](blockchain-workbench-troubleshooting.md)
