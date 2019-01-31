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
ms.reviewer: igorstan
ms.openlocfilehash: 31ec1537a0075f486130a3bcf1017d35b47e499d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463659"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında Azure SQL veri ambarı'nda ölçek işlem

Azure portalından Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için [işlem ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu için işlem ölçeğini daraltın. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Önceden var olan bir veri ambarını ölçeklendirebilir veya [Hızlı Başlangıç: Oluşturma ve bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanarak **mySampleDataWarehouse** adlı bir veri ambarı oluşturabilirsiniz.  Bu hızlı başlangıç, **mySampleDataWarehouse** öğesini ölçeklendirir.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL Veri Ambarı’nda, veri ambarı birimlerini ayarlayarak işlem kaynaklarını artırabilir veya azaltabilirsiniz. [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünde **mySampleDataWarehouse** oluşturuldu ve 400 DWU ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

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
Artık veri ambarınız için işlemin nasıl ölçeklendirileceğini öğrendiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
