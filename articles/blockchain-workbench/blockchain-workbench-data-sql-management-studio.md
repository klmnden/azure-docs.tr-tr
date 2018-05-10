---
title: SQL Server Management Studio ile Azure Blockchain çalışma ekranı verileri kullanma
description: SQL Server Management Studio içinde Azure Blockchain çalışma ekranı kullanıcının SQL veritabanına bağlanmak öğrenin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 1c9ee1e94ba1195db027c3edfab2e2a96f181ca3
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>SQL Server Management Studio ile Azure Blockchain çalışma ekranı verileri kullanma

Microsoft SQL Server Management Studio hızla yazma ve Azure Blockhain çalışma ekranı 's SQL DB sorguları test etme olanağı sağlar. Bu bölüm, SQL Server Management Studio içinde Azure Blockchain çalışma ekranı kullanıcının SQL veritabanına bağlanmak nasıl bir adım adım kılavuz içerir.

## <a name="prerequisites"></a>Önkoşullar

* Karşıdan [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>SQL Server Management Studio Azure Blockchain çalışma ekranındaki verilere bağlanma

1. SQL Server Management Studio'yu açın ve seçin **Bağlan**.
2. Seçin **veritabanı altyapısı**.

    ![Veritabanı altyapısı](media/blockchain-workbench-data-sql-management-studio/database-engine.png)

3. İçinde **sunucuya Bağlan** iletişim kutusunda, sunucu adını ve veritabanı kimlik bilgilerinizi girin.

    Azure Blockchain çalışma ekranının dağıtım işlemi tarafından oluşturulan kimlik bilgileri kullanıyorsanız kullanıcı adı olacaktır **dbadmin** ve dağıtım sırasında sağlanan bir parola olacaktır.

    ![SQL kimlik bilgilerini girin](media/blockchain-workbench-data-sql-management-studio/sql-creds.png)

 4. SQL Server Management Studio Azure Blockchain çalışma ekranı veritabanında veritabanlarını, veritabanı görünümleri ve saklı yordamlar listesini görüntüler.

    ![Veritabanı listesi](media/blockchain-workbench-data-sql-management-studio/db-list.png)

5. Herhangi bir veritabanı görünümleri ile ilişkili verileri görüntülemek için aşağıdaki adımları kullanarak bir select deyimi otomatik olarak oluşturabilir.
6. Herhangi bir nesne Gezgini'nde veritabanı görünümleri sağ tıklayın.
7. Seçin **komut görünüm olarak**.
8. Seçin **seçin**.
9. Seçin **yeni sorgu Düzenleyicisi penceresini**.
10. Yeni bir sorgu seçerek oluşturulabilir **yeni sorgu**.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain çalışma ekranındaki veritabanı görünümleri](blockchain-workbench-database-views.md)