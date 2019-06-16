---
title: Azure veri kutusu Edge kullanıcıları yönetme | Microsoft Docs
description: Azure veri kutusu Ucunuzdaki kullanıcıları yönetmek için Azure portalını kullanmayı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/11/2019
ms.author: alkohli
ms.openlocfilehash: 68f8ad903f967812c4a416c732b35fa1712404cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60756703"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-edge"></a>Azure veri kutusu Ucunuzdaki kullanıcıları yönetmek için Azure portalını kullanma

Bu makalede, Azure veri kutusu edge'de kullanıcıları nasıl yöneteceğinizi açıklar. Azure veri kutusu Edge Azure portal aracılığıyla yönetmenize veya yerel web kullanıcı Arabirimi. Kullanıcı ekleme, değiştirme ve silme işlemleri için Azure portalı kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kullanıcı ekleme
> * Kullanıcıyı değiştirme
> * Kullanıcı silme

## <a name="about-users"></a>Kullanıcılar hakkında

Kullanıcılara salt okunur erişim veya tam ayrıcalık verilebilir. Adından anlaşılacağı üzere salt okunur kullanıcılar paylaşım verilerini yalnızca görüntüleyebilir. Tam ayrıcalık sahibi kullanıcılar paylaşım verilerini okuyabilir, bu paylaşımlara veri yazabilir, paylaşım verilerini değiştirebilir veya silebilir.

 - **Tam ayrıcalıklı kullanıcı**: Tam erişime sahip yerel kullanıcıdır.
 - **Salt okunur kullanıcı**: Salt okunur erişime sahip yerel kullanıcıdır. Bu kullanıcılar yalnızca salt okunur işlemlere izin veren paylaşımlarla ilişkilendirilir.

Kullanıcı izinleri, paylaşım oluşturma sırasında kullanıcı oluşturulurken tanımlanır. Bir kullanıcıya atanan izinler Dosya Gezgini kullanılarak değiştirilebilir. 


## <a name="add-a-user"></a>Kullanıcı ekleme

Kullanıcı eklemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **genel bakış > kullanıcılar**. Seçin **+ Ekle kullanıcı** komut çubuğunda.

    ![Kullanıcı Ekle'yi seçin](media/data-box-edge-manage-users/add-user-1.png)

2. Eklemek istediğiniz kullanıcının kullanıcı adını ve parolasını belirtin. Parolayı onaylayın ve seçin **Ekle**.

    ![Kullanıcı adı ve parolayı belirtin](media/data-box-edge-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Bu kullanıcılar, sistem tarafından ayrılmıştır ve kullanılmamalıdır: Yönetici, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, konuk.  

3. Kullanıcı oluşturma başlar ve olduğunda bir bildirim gösterilir tamamlandı. Komut çubuğundan kullanıcı oluşturulduktan sonra seçin **Yenile** kullanıcıların güncelleştirilmiş listesini görüntülemek için.


## <a name="modify-user"></a>Kullanıcıyı değiştirme

Kullanıcı oluşturulduktan sonra parolasını değiştirebilirsiniz. Kullanıcılar listesinden seçin. Girin ve yeni parolayı onaylayın. Değişiklikleri kaydedin.
 
![Kullanıcıyı değiştirme](media/data-box-edge-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Kullanıcı silme

Kullanıcı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.


1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **genel bakış > kullanıcılar**.

    ![Silmek için kullanıcı seçin](media/data-box-edge-manage-users/delete-user-1.png)

2. Kullanıcıların listeden bir kullanıcı seçin ve ardından **Sil**.  

   ![Sil'i seçin](media/data-box-edge-manage-users/delete-user-2.png)

3. Sorulduğunda silme işlemini onaylayın. 

   ![Silmeyi onayla](media/data-box-edge-manage-users/delete-user-3.png)

Kullanıcı listesi silinen kullanıcıya göre güncelleştirilir.

![Güncelleştirilmiş kullanıcı listesi](media/data-box-edge-manage-users/delete-user-4.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.
