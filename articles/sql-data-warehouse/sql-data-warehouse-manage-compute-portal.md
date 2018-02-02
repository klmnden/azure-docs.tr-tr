---
title: "Azure SQL Data warehouse'da (Azure portalı) işlem güç yönetimi | Microsoft Docs"
description: "İşlem gücüne yönetmek için azure portal görevleri. Dwu ayarlayarak işlem kaynaklarını ölçeklendirme. Veya, duraklatma ve sürdürme işlem kaynaklarını maliyet tasarrufu sağlamak."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: f56e62576cae0c594f26bcddf44528032bd5ea69
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Azure SQL Data warehouse'da (Azure portalı) işlem güç yönetimi
Ölçek Azure portalını kullanarak Azure SQL Data Warehouse kaynakları işlem.

## <a name="scale-compute-power"></a>Ölçek işlem gücü
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

İşlem kaynakları değiştirmek için:

1. Açık [Azure portal][Azure portal], veritabanınızı açın ve'ı tıklatın **ölçek**.

    ![Ölçek'ı tıklatın][1]
2. Ölçek dikey penceresinde, kaydırıcıyı sola veya sağa DWU ayarını değiştirmek taşıyın.

    ![Kaydırıcıyı taşıyın][2]
3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Tıklatın **Evet** onaylamak için veya **hiçbir** iptal etmek için.

    ![Kaydet’e tıklayın.][3]


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [yönetimine genel bakış][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png


<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
