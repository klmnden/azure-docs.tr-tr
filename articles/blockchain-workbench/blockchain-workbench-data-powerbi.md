---
title: Microsoft Power BI'da Azure Blockchain çalışma ekranı verileri kullanma
description: Yük ve Microsoft Power BI'da Azure Blockchain çalışma ekranı SQL DB verilerini görüntüleme hakkında bilgi edinin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 2b909c0a8441010b87c913e5937d25c8127058f1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Microsoft Power BI ile Azure Blockchain çalışma ekranı verileri kullanma

Microsoft Power BI kolayca Power BI Desktop kullanarak SQL DB veritabanlarından güçlü raporları oluşturmak ve bunları yayımlama olanağı sağlar [ https://www.powerbi.com ](http://www.powerbi.com).

Bu makale Desktop'a içinde Azure Blockchain çalışma ekranı kullanıcının SQL veritabanına bağlanmak, bir rapor oluşturmak ve raporu powerbi.com'da dağıtmak nasıl bir adım adım kılavuz içerir.

## <a name="prerequisites"></a>Önkoşullar

* Karşıdan [Desktop'a](https://aka.ms/pbidesktopstore).

## <a name="connecting-powerbi-to-data-in-azure-blockchain-workbench"></a>Powerbı Azure Blockchain çalışma ekranındaki verilere bağlanma

1.  Power BI Desktop açın.
2.  Seçin **veri alma**.

    ![Verileri alma](media/blockchain-workbench-data-powerbi/get-data.png)
3.  Seçin **SQL Server** veri kaynağı türleri listesinden.

4.  İletişim kutusunda sunucu ve veritabanı adını belirtin. Veri içeri aktarın veya gerçekleştirmek istediğinizi belirtin bir **DirectQuery**. **Tamam**’ı seçin.

    ![SQL Server'ı seçin](media/blockchain-workbench-data-powerbi/select-sql.png)

5.  Azure Blockchain çalışma ekranı erişmek için veritabanı kimlik bilgilerini sağlayın. Seçin **veritabanı** ve kimlik bilgilerinizi girin.

    Azure Blockchain çalışma ekranının dağıtım işlemi tarafından oluşturulan kimlik bilgileri kullanıyorsanız kullanıcı adı olan **dbadmin** ve dağıtım sırasında sağlanan bir paroladır.

    ![SQL DB ayarları](media/blockchain-workbench-data-powerbi/db-settings.png)

6.  Veritabanına bağlandığında **Gezgini** iletişim kutusunu görüntüler tabloları ve görünümleri veritabanı içinde kullanılabilir. Görünümleri raporlama ve önekli tüm öğeler için tasarlanmış **vw**.

    ![Gezgin](media/blockchain-workbench-data-powerbi/navigator.png)

7.  Dahil etmek istediğiniz görünümleri seçin. Tanıtım amacıyla, eklediğimiz **vwContractAction**, tüm bir sözleşme üzerinde gerçekleştirilen eylemler hakkında ayrıntılar sağlar.

    ![Görünümleri seçin](media/blockchain-workbench-data-powerbi/select-views.png)

Şimdi oluşturun ve Power BI ile normalde yaptığınız gibi raporları yayımlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain çalışma ekranındaki veritabanı görünümleri](blockchain-workbench-database-views.md)