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
ms.openlocfilehash: 63888d5dd103b585cf18e4787d3e779810163e3d
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Azure SQL Data warehouse'da (Azure portalı) işlem güç yönetimi
> [!div class="op_single_selector"]
> * [Genel Bakış](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>Ölçek işlem gücü
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

İşlem kaynakları değiştirmek için:

1. Açık [Azure portal][Azure portal], veritabanınızı açın ve'ı tıklatın **ölçek**.

    ![Ölçek'ı tıklatın][1]
2. Ölçek dikey penceresinde, kaydırıcıyı sola veya sağa DWU ayarını değiştirmek taşıyın.

    ![Kaydırıcıyı taşıyın][2]
3. **Kaydet** düğmesine tıklayın. Bir onay iletisi görüntülenir. Tıklatın **Evet** onaylamak için veya **hiçbir** iptal etmek için.

    ![Kaydet’e tıklayın.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Duraklatma işlem
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Bir veritabanı duraklatmak için:

1. Açık [Azure portal] [ Azure portal] ve veritabanınızı açın. Durum olduğuna dikkat edin **çevrimiçi**.

    ![Çevrimiçi durumu][6]
2. İşlem ve bellek kaynakları askıya almak için tıklayın **duraklatma**, ve ardından bir onay iletisi görüntülenir. Tıklatın **Evet** onaylamak için veya **hiçbir** iptal etmek için.

    ![Duraklatma onaylayın][7]
3. SQL veri ambarı veritabanı başlatılırken durumudur **duraklatma**.
4. Durum olduğunda **duraklatıldı**, duraklatma işlemi yapılır ve, artık Dwu için ücretlendirilirsiniz.

    ![Duraklatma durumu][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Resume işlem
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Bir veritabanı devam ettirmek için:

1. Açık [Azure portal] [ Azure portal] ve veritabanınızı açın. Durum olduğuna dikkat edin **duraklatıldı**.

    ![Duraklatma veritabanı][4]
2. Veritabanı sürdürmek için **Başlat**, ve ardından bir onay iletisi görüntülenir. Tıklatın **Evet** onaylamak için veya **hiçbir** iptal etmek için.

    ![Resume onaylayın][5]
3. SQL veri ambarı veritabanı başlatılırken durum "Sürdürülüyor" olur.
4. Durum olduğunda **çevrimiçi**, hazır bir veritabanıdır.

    ![Çevrimiçi durumu][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [yönetimine genel bakış][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
