---
title: "Hızlı Başlangıç: Ölçek genişletme Azure SQL Data Warehouse - Azure portalında işlem | Microsoft Docs"
description: "İşlem gücüne yönetmek için azure portal görevleri. Dwu ayarlayarak işlem kaynaklarını ölçeklendirme. Veya, duraklatma ve sürdürme işlem kaynaklarını maliyet tasarrufu sağlamak."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: cbe2f2d17f309e01e831aa9ee31e01e044896d10
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Hızlı Başlangıç: İşlem ölçeklendirme Azure SQL Data Warehouse Azure portalında

Azure portalında Azure SQL Data Warehouse ölçek işlem. İşlem daha iyi performans için genişletme veya ölçek geri işlem maliyet tasarrufu sağlamak. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Zaten yüklü veya kullanan bir veri ambarı ölçeklendirebilirsiniz [hızlı başlangıç: oluşturun ve bağlanın - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**.  Bu hızlı başlangıç ölçeklendirir **mySampleDataWarehouse**.

## <a name="scale-compute"></a>Bilgi işlem

SQL veri ambarı'nda artırın veya işlem kaynakları data warehouse birimleri ayarlayarak azaltın. [Oluşturma ve Connect - portal](create-data-warehouse-portal.md) oluşturulan **mySampleDataWarehouse** ve 400 Dwu ile başlatıldı. Dwu için aşağıdaki adımları ayarlamak **mySampleDataWarehouse**.

Data warehouse birimleri değiştirmek için:

1. Tıklatın **SQL veritabanları** Azure portalının sol sayfasındaki.
2. Seçin **mySampleDataWarehouse** gelen **SQL veritabanları** sayfası. Veri ambarı açar.
3. Tıklatın **ölçek**.

    ![Ölçek'ı tıklatın](media/quickstart-scale-compute-portal/click-scale.png)

2. Ölçek Masası'nda, kaydırıcıyı sola veya sağa DWU ayarını değiştirmek taşıyın.

    ![Kaydırıcıyı taşıyın](media/quickstart-scale-compute-portal/scale-dwu.png)

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Tıklatın **Evet** onaylamak için veya **hiçbir** iptal etmek için.

    ![Kaydet’e tıklayın.](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Sonraki adımlar
Şimdi, veri ambarı için işlem ölçeklendirmek öğrendiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
