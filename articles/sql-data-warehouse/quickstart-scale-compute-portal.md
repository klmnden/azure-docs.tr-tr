---
title: "Hızlı Başlangıç: Azure SQL veri ambarı - Azure Portalı'nda işlem ölçeğini | Microsoft Docs"
description: Azure portalından Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için işlem ölçeğini genişletin veya maliyet tasarrufu için işlem ölçeğini daraltın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: b02259e2eaf497fb1bfefc4c1ed7611a22394d48
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61475447"
---
## <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında Azure SQL veri ambarı'nda ölçek işlem

Azure portalından Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için [işlem ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu için işlem ölçeğini daraltın. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Zaten sahip veya kullanan bir veri ambarını ölçeklendirebilir [hızlı başlangıç: oluşturma ve bağlanma - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**.  Bu hızlı başlangıç, **mySampleDataWarehouse** öğesini ölçeklendirir.

>[!Note]
>Veri ambarınızın ölçeğini için çevrimiçi olmalıdır. 

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL veri ambarı işlem kaynakları, artan veya azalan veri ambarı birimleri ölçeklendirilebilir. [Oluşturma ve bağlanma - portal] oluşturulan quickstart(create-data-warehouse-portal.md) **mySampleDataWarehouse** ve 400 Dwu ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için:

1. Azure portalının sol taraftaki sayfasında **SQL veri ambarları**’na tıklayın.
2. **SQL veri ambarları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Veri ambarı açılır.
3. **Ölçek** seçeneğine tıklayın.

    ![Ölçek seçeneğine tıklayın](media/quickstart-scale-compute-portal/click-scale.png)

2. Ölçek panelinde kaydırıcıyı sola veya sağa hareket ettirerek DWU ayarını değiştirin.

    ![Kaydırıcıyı Taşıyın](media/quickstart-scale-compute-portal/scale-dwu.png)

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Onaylamak için **evet**’e, iptal etmek için **hayır**’a tıklayın.

    ![Kaydet’e tıklayın.](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Sonraki adımlar
Artık veri ambarınız ölçeklendirileceğini öğrendiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
