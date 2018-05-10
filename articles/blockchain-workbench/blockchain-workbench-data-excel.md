---
title: Microsoft Excel'de Azure Blockchain çalışma ekranı verileri kullanma
description: Yük ve Azure Blockchain çalışma ekranı SQL DB verileri Microsoft Excel'de görüntüleme hakkında bilgi edinin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 70297bd0af6322d0f3ac2c719d1827e4bc5898cd
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="view-azure-blockchain-workbench-data-with-microsoft-excel"></a>Microsoft Excel ile Azure Blockchain çalışma ekranı verileri görüntüleme

Microsoft Excel verilerini Azure Blockchain çalışma ekranı 's SQL DB içinde görüntülemek için kullanabilirsiniz. Bu makale için gereken adımları sağlar:

* Microsoft Excel'den Blockchain çalışma ekranı veritabanına bağlan
* Blockchain çalışma ekranı veritabanı tabloları ve görünümleri bakın
* Excel'e Blockchain çalışma ekranı görünüm veri yükleme

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain çalışma ekranı veritabanına bağlan

Bir Blockchain çalışma ekranı veritabanına bağlanmak için:

1. Microsoft Excel'i açın.
2. Üzerinde **veri** sekmesinde, seçin **Veri Al**.
3. Seçin **Azure** ve ardından **Azure SQL veritabanından**.

   ![Azure SQL Veritabanı'na bağlanma](media/blockchain-workbench-data-excel/connect-sql-db.png)

4. İçinde **SQL Server veritabanı** iletişim kutusunda:

    * İçin **Server**, Blockchain çalışma ekranı sunucunun adını girin.
    * İçin **veritabanı (isteğe bağlı)**, veritabanının adını girin.

   ![Veritabanı sunucusu ve veritabanı sağlar](media/blockchain-workbench-data-excel/provide-server-db.png)

5. İçinde **SQL Server veritabanı** iletişim gezinti çubuğu, select **veritabanı**. Girin, **kullanıcıadı** ve **parola**ve ardından **Bağlan**.

    > [!NOTE]
    > Azure Blockchain çalışma ekranının dağıtım işlemi sırasında oluşturulan kimlik bilgileri kullanıyorsanız **kullanıcı adı** olan `dbadmin`. **Parola** Blockchain çalışma ekranı dağıtıldığında bir oluşturulur.
    
   ![Veritabanına erişmek için kimlik bilgilerini belirtin](media/blockchain-workbench-data-excel/provide-credentials.png)

## <a name="look-at-database-tables-and-views"></a>Veritabanı tabloları ve görünümleri bakın

Veritabanına bağlandıktan sonra Excel Gezgini iletişim kutusunu açar. Tabloları ve görünümleri veritabanında bakmak için Gezgini kullanın. Görünümleri raporlama için tasarlanmıştır ve adlarını önekine sahip **vw**.

   ![Excel Gezgini Önizleme görünümü](media/blockchain-workbench-data-excel/excel-navigator.png)

## <a name="load-view-data-into-an-excel-workbook"></a>Bir Excel çalışma kitabına görünüm veri yükleme

Sonraki örnek bir Excel çalışma kitabına görünümden verileri nasıl yükleyebilirsiniz gösterir.

1. İçinde **Gezgini** kaydırma çubuğu, select **vwContractAction** görünümü. **VwContractAction** Önizleme Blockchain çalışma ekranı veritabanındaki bir sözleşme ile ilgili tüm eylemleri gösterir.
2. Seçin **yük** görünümdeki tüm verileri almak ve Excel çalışma kitabınızı yerleştirin.

   ![Bir görünümden yüklenen verileri](media/blockchain-workbench-data-excel/view-data.png)

Yüklenen veriler sahip olduğunuza göre Azure Blockchain çalışma ekranı veritabanından meta verileri ve işlem verilerini kullanan kendi raporlarınızı oluşturmak için Excel özelliklerini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain çalışma ekranındaki veritabanı görünümleri](blockchain-workbench-database-views.md)