---
title: "Azure portal: SQL veritabanı dinamik veri maskeleme | Microsoft Docs"
description: "Nasıl SQL veritabanını dinamik veri maskeleme Azure portalında kullanmaya başlama"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Inactive
ms.date: 11/22/2016
ms.author: ronitr
ms.openlocfilehash: 20d344bc6ae971012bd181d14d130432263a3b76
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>SQL veritabanı dinamik veri maskeleme Azure portalıyla kullanmaya başlama

Bu makalede nasıl uygulandığını gösterir [dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md) Azure portal ile. Dinamik veri maskeleme kullanarak uygulayabileceğiniz [Azure SQL veritabanı cmdlet'leri](https://msdn.microsoft.com/library/azure/mt574084.aspx) veya [REST API](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dinamik veri maskeleme Azure Portalı'nı kullanarak, veritabanınız için ayarlama
1. Azure portalında başlatma [https://portal.azure.com](https://portal.azure.com).
2. Hassas verileri maskelemek istediğiniz içeren veritabanı ayarları sayfasına gidin.
3. Tıklatın **dinamik veri maskeleme** başlatır döşeme **dinamik veri maskeleme** yapılandırma sayfası.
   
   * Alternatif olarak, aşağı kaydırabilirsiniz **Operations** 'ye tıklayın **dinamik veri maskeleme**.
     
     ![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. İçinde **dinamik veri maskeleme** yapılandırma sayfasında, öneriler altyapısı maskeleme için işaretlenmiş bazı veritabanı sütunlarını karşılaşabilirsiniz. Önerileri kabul etmek için yalnızca tıklatın **Maskesi Ekle** bir veya daha fazla sütun ve bir maskesi için bu sütun için varsayılan türünü temel alınarak oluşturulur. Maskeleme kuralı tıklayarak ve maskeleme düzenleme maskeleme işlevi değiştirebileceğiniz farklı bir biçime tercih ettiğiniz alan biçimi. Tıklattığınızdan emin olun **kaydetmek** ayarlarınızı kaydetmek için.
   
    ![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Üstündeki veritabanınızda herhangi bir sütun için bir maskesi eklemek için **dinamik veri maskeleme** yapılandırma sayfasında, tıklatın **Maskesi Ekle** açmak için **maskeleme Kuralı Ekle** yapılandırma sayfası .
   
    ![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Seçin **şema**, **tablo** ve **sütun** maskeleme için belirtilen alan tanımlamak için.
7. Seçin bir **maskeleme alan biçimini** hassas verileri maskeleme Kategoriler listesinden.
   
    ![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Tıklatın **kaydetmek** veri maskeleme kuralları ilkenin maskeleme dinamik veri maskeleme kümesini güncelleştirmek için kural sayfa içinde.
9. SQL kullanıcıları veya maskeleme işlemi bırakılmalıdır ve maskelenmemiş hassas verilere erişimi AAD kimlikleri yazın. Bu kullanıcılar noktalı virgülle ayrılmış bir listesi olmalıdır. Yönetici ayrıcalıklarına sahip kullanıcılar, her zaman özgün maskelenmemiş veri erişimine sahip.
   
    ![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Uygulama katmanı uygulama ayrıcalıklı kullanıcılar için hassas verileri görüntüleyebilmesi yapmak için SQL kullanıcı eklemek veya AAD kimlik uygulama veritabanını sorgulamak için kullanır. Bu liste gizli verilerin açığa en aza indirmek için ayrıcalıklı kullanıcıların en az bir sayı içermesi önerilir.
   > 
   > 
10. Tıklatın **kaydetmek** yeni veya güncelleştirilmiş kaydetmek için yapılandırma sayfası maskeleme verilerde ilkenin maskeleme.


## <a name="next-steps"></a>Sonraki adımlar

* Dinamik veri maskeleme genel bakış için bkz: [dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md).
* Dinamik veri maskeleme kullanarak uygulayabileceğiniz [Azure SQL veritabanı cmdlet'leri](https://msdn.microsoft.com/library/azure/mt574084.aspx) veya [REST API](https://msdn.microsoft.com/library/dn505719.aspx).
