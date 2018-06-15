---
title: Azure Blockchain Workbench verilerini Microsoft Power BI’da kullanma
description: Azure Blockchain Workbench SQL DB verilerini Microsoft Power BI’da yüklemeyi ve görüntülemeyi öğrenin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 321a34589277d62290c2fde680bb461de34b4568
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075179"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Azure Blockchain Workbench verilerini Microsoft Power BI ile kullanma

Microsoft Power BI, Power BI Desktop uygulamasını kullanarak SQL DB veritabanlarından kolayca güçlü raporlar oluşturma ve bunları [https://www.powerbi.com](http://www.powerbi.com) üzerinde yayımlama olanağı sağlar.

Bu makale, Azure Blockchain Workbench'in SQL Veritabanı’na PowerBI Desktop’ın içinden bağlanma, bir rapor oluşturma ve raporu powerbi.com’a dağıtma sürecine ilişkin adım adım bir kılavuz içerir.

## <a name="prerequisites"></a>Ön koşullar

* [PowerBI Desktop](https://aka.ms/pbidesktopstore)’ı indirin.

## <a name="connecting-powerbi-to-data-in-azure-blockchain-workbench"></a>PowerBI uygulamasını Azure Blockchain Workbench’teki verilere bağlama

1.  Power BI Desktop’ı açın.
2.  **Veri Al**’ı seçin.

    ![Verileri alma](media/blockchain-workbench-data-powerbi/get-data.png)
3.  Veri kaynağı türleri listesinden **SQL Server**’ı seçin.

4.  İletişim kutusunda sunucu ve veritabanı adını belirtin. Veri içeri aktarmak istediğinizi mi yoksa bir **DirectQuery** mi gerçekleştirmek istediğinizi belirtin. **Tamam**’ı seçin.

    ![SQL Server'ı seçin](media/blockchain-workbench-data-powerbi/select-sql.png)

5.  Azure Blockchain Workbench’e erişmek için veritabanı kimlik bilgilerini sağlayın. **Veritabanı**’nı seçip kimlik bilgilerinizi girin.

    Azure Blockchain Workbench dağıtım işlemi tarafından oluşturulan kimlik bilgilerini kullanıyorsanız kullanıcı adı **dbadmin**, parola ise dağıtım sırasında belirttiğiniz paroladır.

    ![SQL DB ayarları](media/blockchain-workbench-data-powerbi/db-settings.png)

6.  Veritabanına bağlandığınızda **Gezgin** iletişim kutusunda veritabanındaki tablolar ve görünümler görüntülenir. Görünümler raporlama için tasarlanmıştır ve hepsinin önünde **vw** eki olur.

    ![Gezgin](media/blockchain-workbench-data-powerbi/navigator.png)

7.  Dahil etmek istediğiniz görünümleri seçin. Tanıtım amacıyla, bir sözleşmede gerçekleşen tüm eylemlerin ayrıntılarını sağlayan **vwContractAction** görünümünü ekleriz.

    ![Görünümleri seçin](media/blockchain-workbench-data-powerbi/select-views.png)

Artık Power BI ile normalde yaptığınız gibi rapor oluşturup yayımlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’te veritabanı görünümleri](blockchain-workbench-database-views.md)