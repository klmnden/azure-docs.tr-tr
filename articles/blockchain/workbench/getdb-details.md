---
title: Azure Blockchain Workbench veritabanı ayrıntılarını alma
description: Azure Blockchain Workbench veritabanı ve veritabanı sunucusu bilgilerini almayı öğrenin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/09/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 42d119acd8880458eadc1760a7cb9713f91e3f6f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65509973"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Azure Blockchain Workbench veritabanınızla ilgili bilgi edinin

Bu makalede Azure Blockchain Workbench veritabanınız hakkında nasıl ayrıntılı bilgi edinebileceğiniz gösterilir.

## <a name="overview"></a>Genel Bakış

Uygulamalar, iş akışları ve akıllı sözleşme yürütme ile ilgili bilgiler Blockchain Workbench SQL DB’deki veritabanı görünümleri kullanılarak sağlanır. Geliştiriciler, Microsoft Excel, Power BI, Visual Studio ve SQL Server Management Studio gibi araçlar kullanarak bu bilgileri kullanabilirsiniz.

Bir geliştiricinin veritabanına bağlanabilmesi için önce şunlar gerekir:

* Veritabanı güvenlik duvarında dış istemci erişimine izin verilmesi. Veritabanı güvenlik duvarı yapılandırmayla ilgili bu makalede erişime nasıl izin verileceği açıklanır.
* Veritabanı sunucu adı ve veritabanı adı.

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain Workbench veritabanına bağlanma

Veritabanına bağlanmak için:

1. Azure portalında olan bir hesapla oturum açın **sahibi** Azure Blockchain Workbench kaynaklar için izinleri.
2. Sol gezinti bölmesinden **Kaynak Grupları**'nı seçin.
3. Blockchain Workbench dağıtımınıza yönelik kaynak grubunun adını seçin.
4. Kaynak listesini sıralamak için **Tür**’ü, sonra da **SQL sunucunuzu** seçin. Bir sonraki ekran görüntüsünde yer alan sıralı listede "master" adlı bir veritabanının yanı sıra **Kaynak ön eki** olarak "lhgn" kullanan bir veritabanı şeklinde iki SQL veritabanı gösterilir.

   ![Sıralı Blockchain Workbench kaynak listesi](./media/getdb-details/sorted-workbench-resource-list.png)

5. Blockchain Workbench veritabanıyla ilgili ayrıntılı bilgileri görüntülemek üzere Blockchain Workbench’i dağıtmak için sağladığınız **Kaynak ön ekine** sahip veritabanının bağlantısını seçin.

   ![Veritabanı ayrıntıları](./media/getdb-details/workbench-db-details.png)

Veritabanı sunucu adı ve veritabanı adı, geliştirme ya da raporlama aracınızı kullanarak Blockchain Workbench veritabanına bağlanmanıza imkan tanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’te veritabanı görünümleri](database-views.md)