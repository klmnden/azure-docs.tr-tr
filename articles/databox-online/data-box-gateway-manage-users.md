---
title: Azure Data Box Gateway kullanıcılarını yönetme | Microsoft Docs
description: Azure portalı kullanarak Azure Data Box Gateway kullanıcılarını yönetme adımları.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/09/2018
ms.author: alkohli
ms.openlocfilehash: 3bd368cf19edf1370aaeef54d3ba0e4fd26ac239
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49070808"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-gateway"></a>Azure portalı kullanarak Azure Data Box Gateway kullanıcılarını yönetme 

Bu makalede Azure Data Box Gateway kullanıcılarını yönetme adımları açıklanmaktadır. Azure Data Box Gateway’i Azure portal veya yerel web kullanıcı arabirimiyle yönetebilirsiniz. Kullanıcı ekleme, değiştirme ve silme işlemleri için Azure portalı kullanın.

> [!IMPORTANT]
> - Data Box Gateway önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

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

1. Data Box Gateway kaynağınızın Azure portal sayfasında **Genel bakış** bölümüne gidin. Komut çubuğunda **+ Kullanıcı ekle**'ye tıklayın.

    ![Kullanıcı ekle'ye tıklayın](media/data-box-gateway-manage-users/add-user-1.png)

2. Eklemek istediğiniz kullanıcının kullanıcı adını ve parolasını belirtin. Parolayı onaylayın ve **Ekle**'ye tıklayın.

    ![Kullanıcı ekle'ye tıklayın](media/data-box-gateway-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Bu kullanıcılar sistem tarafından ayrılmıştır ve kullanılmamalıdır: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Kullanıcı oluşturma işlemi başladığında ve tamamlandığında bildirim alırsınız. Kullanıcı oluşturulduktan sonra komut çubuğunda **Yenile**'ye tıklayarak güncel kullanıcı listesini görüntüleyebilirsiniz.


## <a name="modify-user"></a>Kullanıcıyı değiştirme

Kullanıcı oluşturulduktan sonra parolasını değiştirebilirsiniz. Kullanıcı listesinden seçin ve tıklayın. Yeni parolayı girin ve onaylayın. Değişiklikleri kaydedin.
 
![Kullanıcıyı değiştirme](media/data-box-gateway-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Kullanıcı silme

Kullanıcı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Kullanıcı listesinden bir kullanıcı seçin ve **Sil**'e tıklayın.  

   ![Kullanıcı silme](media/data-box-gateway-manage-users/delete-user-1.png)

2. Sorulduğunda silme işlemini onaylayın. 

   ![Kullanıcı silme](media/data-box-gateway-manage-users/delete-user-2.png)

Kullanıcı listesi silinen kullanıcıya göre güncelleştirilir.

![Kullanıcı silme](media/data-box-gateway-manage-users/delete-user-3.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-gateway-manage-bandwidth-schedules.md) öğrenin.
