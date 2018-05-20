---
title: Uygulamaları Azure Blockchain çalışma ekranı içinde kullanma
description: Uygulamayı kullanmak nasıl Azure Blockchain çalışma ekranı içinde sözleşme.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 8732d1b87acaa6673ae92b3302fb257dcb134217
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="using-applications-in-azure-blockchain-workbench"></a>Uygulamaları Azure Blockchain çalışma ekranı içinde kullanma

Oluşturma ve sözleşmelerinde eylemleri Blockchain çalışma ekranı kullanabilirsiniz. Ayrıca görüntüleyebilirsiniz Sözleşme durumu ve işlem geçmişi gibi ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

* Blockchain çalışma ekranı dağıtımı. Daha fazla bilgi için bkz: [Azure Blockchain çalışma ekranının dağıtım](blockchain-workbench-deploy.md) dağıtımı hakkında ayrıntılı bilgi için
* Blockchain çalışma ekranı dağıtılan blockchain uygulamada. Bkz: [blockchain uygulama içinde Azure Blockchain çalışma ekranı oluşturma]()

[Blockchain çalışma ekranı açmak](blockchain-workbench-deploy.md#blockchain-workbench-web-url) tarayıcınızda.

![Blockchain Workbench](media/blockchain-workbench-use/workbench.png)

Blockchain çalışma ekranı bir üyesi olarak oturum açmanız. Listelenen hiçbir uygulamaları varsa, Blockchain çalışma ekranı üyesi ancak üyesi olmayan herhangi bir uygulama değildir. Blockchain çalışma ekranı yönetici üyeleri uygulamalara atayabilirsiniz.

## <a name="create-new-contract"></a>Yeni sözleşme oluşturma 

Yeni bir sözleşme oluşturmak için bir üyesi olmanız gerekir **AllowedInstanceRoles** rol. 

1. Blockchain çalışma ekranı uygulama bölümünde, oluşturmak istediğiniz sözleşme içeren uygulama kutucuk seçin. Etkin sözleşmeleri bir listesi görüntülenir.

2. Yeni bir sözleşme oluşturmak için seçin **yeni sözleşme**.

    ![Yeni sözleşme düğmesi](media/blockchain-workbench-use/contract-list.png)

3. **Yeni sözleşme** bölmesinde görüntülenir. İlk parametre değerlerini belirtin. **Oluştur**’u seçin.

    ![Yeni sözleşme bölmesi](media/blockchain-workbench-use/new-contract.png)

    Yeni oluşturulan bir sözleşme diğer etkin sözleşmeleri listesinde görüntülenir.

    ![Etkin sözleşme listesi](media/blockchain-workbench-use/active-contracts.png)

## <a name="take-action-on-contract"></a>Sözleşme üzerinde eylem

1. Blockchain çalışma ekranı uygulama bölümünde eyleme sözleşme içeren uygulama döşeme seçin.
2. Sözleşme listeden seçin. Anlaşma ayrıntılarını farklı bölümlerde görüntülenir. 

    ![Anlaşma ayrıntıları](media/blockchain-workbench-use/contract-details.png)

    | Bölüm  | Açıklama  |
    |---------|---------|
    | Durum | Geçerli ilerleme içinde sözleşme aşamaları listeler |
    | Ayrıntılar | Sözleşmenin geçerli değerleri |
    | action | Son eylemi hakkındaki ayrıntıları |
    | Etkinlik | Sözleşmenin işlem geçmişi |
    
3. İçinde **eylem** bölümünde, select **ele eylem**.

4. Sözleşme geçerli durumuyla ilgili ayrıntıları Bölmesi'nde görüntülenir. Açılan gerçekleştirmek istediğiniz eylemi seçin. 

    ![Eylem seçin](media/blockchain-workbench-use/choose-action.png)

5. Seçin **ele eylem** eylemi başlatmak için.
6. Eylem için parametreler gerekiyorsa, eylemi için değerleri belirtin.

    ![Eylem gerçekleştirin](media/blockchain-workbench-use/take-action.png)

7. Seçin **ele eylem** eylemi yürütülemedi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain çalışma ekranı ile ilgili sorunları giderme](blockchain-workbench-troubleshooting.md)