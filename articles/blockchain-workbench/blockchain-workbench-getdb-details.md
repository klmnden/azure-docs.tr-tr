---
title: Azure Blockchain çalışma ekranı veritabanı ayrıntıları alma
description: Azure Blockchain çalışma ekranı veritabanı ve veritabanı sunucusu bilgilerini alma hakkında bilgi.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/4/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: bf7cc85e823e6630dbd3278bc91fba85f404059f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Azure Blockchain çalışma ekranı veritabanınızı hakkında bilgi edinin

Bu makalede Azure Blockchain çalışma ekranı veritabanınızı hakkında ayrıntılı bilgi almak nasıl gösterilmektedir.

## <a name="overview"></a>Genel Bakış

Veritabanı görünümleri Blockchain çalışma ekranı SQL DB kullanarak uygulamalar, iş akışları ve akıllı sözleşme yürütme hakkında bilgi sağlanır. Geliştiriciler, Microsoft Excel, Powerbı, Visual Studio ve SQL Server Management Studio gibi araçları kullanarak, bu bilgileri kullanabilir.

Bir geliştirici veritabanına bağlanmadan önce bunlar gerekir:

* Veritabanı Güvenlik Duvarı'nda izin verilen dış istemci erişim. Bu makalede bir veritabanı güvenlik duvarı makalesi yapılandırma hakkında daha fazla erişim izni açıklanmaktadır.
* Veritabanı sunucusu adını ve veritabanı adı.

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain çalışma ekranı veritabanına bağlan

Veritabanına bağlanmak için:

1. Azure portalına sahip bir hesapla oturum açın **sahibi** Azure Blockchain çalışma ekranı kaynaklar için izinleri.
2. Sol gezinti bölmesinde seçin **kaynak grupları**.
3. Kaynak grubu adı Blockchain çalışma ekranı dağıtımınız için seçin.
4. Seçin **türü** kaynak sıralamak ve seçin, **SQL server**. Sıralanmış sonraki ekran görüntüsünde iki SQL veritabanları, "ana" ve "lhgn" kullanan olarak gösterir **kaynak önek**.

   ![Sıralanmış Blockchain çalışma ekranı kaynak listesi](media/blockchain-workbench-getdb-details/sorted-workbench-resource-list.png)

5. Blockchain çalışma ekranı veritabanı hakkında ayrıntılı bilgi için veritabanıyla bağlantısını seçin **kaynak önek** Blockchain çalışma ekranı dağıtmak için sağlanan.

   ![Veritabanı ayrıntıları](media/blockchain-workbench-getdb-details/workbench-db-details.png)

Veritabanı sunucusu adını ve veritabanı adı geliştirme kullanarak veya Raporlama Aracı Blockchain çalışma ekranı veritabanına bağlanmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain çalışma ekranındaki veritabanı görünümleri](blockchain-workbench-database-views.md)