---
title: Power BI ile SQL Data Warehouse verilerini görselleştirme | Microsoft Azure
description: Power BI ile SQL Data Warehouse verilerini görselleştirme
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 52581a87caac419a79caab647cc9c5a4ee7453ba
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="visualize-data-with-power-bi"></a>Power BI ile verileri görselleştirme
Bu öğreticide SQL Data Warehouse'a bağlanmak ve birkaç temel görselleştirme oluşturmak üzere Power BI'ı nasıl kullanacağınız gösterilmiştir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

* AdventureWorksDW veritabanı önceden yüklenmiş bir SQL Data Warehouse. Veri ambarı sağlamak için bkz: [SQL Data Warehouse oluşturma](create-data-warehouse-portal.md) ve örnek verileri yüklemek seçin. Zaten bir veri ambarı var ancak örnek verileriniz yoksa yapabilecekleriniz [WideWorldImportersDW yük](load-data-wideworldimportersdw.md).

## <a name="1-connect-to-your-database"></a>1. Veritabanınıza bağlanın
Power BI'ı açmak ve AdventureWorksDW veritabanınıza bağlanmak için şunları yapın:

1. [Azure portal](https://portal.azure.com/) oturum açın.
2. **SQL veritabanları** seçeneğine tıklayın ve AdventureWorks SQL Data Warehouse veritabanınızı seçin.
   
    ![Veritabanınızı bulma](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png)
3. "Power BI'da aç" düğmesine tıklayın.
   
    ![Power BI düğmesi](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png)
4. Bu işlemin ardından veritabanınızın web adresinin görüntülendiği SQL Data Warehouse bağlantı sayfası açılır. İleri'ye tıklayın.
   
    ![Power BI bağlantısı](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png)
5. Azure SQL server kullanıcı adı ve parola girin.
   
    ![Power BI oturum açma](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png)
6. Veritabanını açmak için Sol dikey penceredeki AdventureWorksDW veri kümesine tıklayın.
   
    ![Power BI AdventureWorksDW'yi açma](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png)

## <a name="2-create-a-report"></a>2. Bir rapor oluşturun
Artık AdventureWorksDW örnek verilerinizi Power BI kullanarak çözümlemeye hazırsınız. AdventureWorksDW'de çözümlemenin gerçekleştirileceği AggregateSales adlı bir görünüm vardır. Bu görünüm, şirket satışlarının çözümlenmesine yönelik ana ölçümlerden birkaçını içerir.

1. Posta koduna göre satış tutarlarının bir haritasını oluşturmak için sağ taraftaki alanlar bölmesinde AggregateSales görünümüne tıklayıp görünümü genişletin. PostalCode ve SalesAmount sütünlarını tıklayarak seçin.
   
    ![Power BI AggregateSales seçer](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png)
   
    Power BI otomatik olarak coğrafi veriler tanınmıyor ve sizin için bir harita yerleştirin.
   
    ![Power BI haritası](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png)

2. Bu adımda müşteri geliri başına satış tutarını gösteren bir çubuk grafiği oluşturulur. Çubuk grafik oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount alanına tıklayın. Müşteri Geliri alanını sola sürükleyip Eksene bırakın.
   
    ![Power BI eksen seçer](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png)
   
    Çubuk grafiğini sola üzerinden.
   
    ![Power BI çubuğu](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png)
3. Bu adımda, sipariş tarihi başına satış tutarını gösteren bir çizgi grafiği oluşturulur. Çizgi grafiği oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount ve OrderDate öğelerine tıklayın Görselleştirmeler sütununun ikinci satırındaki ilk simge olan çizgi grafiği simgesine tıklayın.
   
    ![Power BI çizgi grafiği seçer](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png)
   
    Artık verilerin üç farklı görselleştirmesini gösteren bir raporunuz var.
   
    ![Power BI çizgi grafiği](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png)

**Dosya**'ya tıklayıp **Kaydet**'i seçerek ilerleme durumunuzu istediğiniz zaman kaydedebilirsiniz.

## <a name="using-direct-connnect"></a>Doğrudan bağlantı kullanma
Olarak Azure SQL veritabanı ile SQL veri ambarı doğrudan bağlantı Power BI analitik yeteneklerini yanında mantıksal aşağı itme sağlar. Veri keşfetmenizde doğrudan bağlantı ile sorgular, Azure SQL Data Warehouse'da dön gerçek zamanlı gönderilir.  SQL Data Warehouse, ölçeği ile birlikte, bu özellik terabayt veri göre dakika cinsinden dinamik raporlar oluşturmanıza olanak sağlar. Ayrıca, Power BI düğmesi Aç giriş doğrudan Power BI kendi SQL veri ambarı'na Azure diğer bölümlerden bilgilerini toplamadan bağlanmasına olanak sağlar.

Doğrudan bağlantı kullanırken:

* Bağlanırken tam sunucu adını belirtin.
* Veritabanı için güvenlik duvarı kuralları Azure hizmetlerine erişime izin ver için yapılandırılmış emin olun.
* Bir sütunun seçilmesi veya bir filtre eklemeden gibi her eylem doğrudan veri ambarı sorgular.
* Döşeme otomatik olarak ve yaklaşık 15 dakikada bir yenilenir.
* Soru- cevap veri kümeleri doğrudan bağlanmak için kullanılamıyor.
* Şema değişiklikleri otomatik olarak birleştirilir.
* Tüm doğrudan bağlanmak sorguları 2 dakika sonra zaman aşımına uğrar.

Deneyimleri geliştirmek gibi bu kısıtlamaları ve notlar değişebilir.

## <a name="next-steps"></a>Sonraki adımlar
Size örnek verilerle alıştırma yapmanız için biraz zaman tanıdığımıza göre, [geliştirme](sql-data-warehouse-overview-develop.md), [yükleme](design-elt-data-loading.md) veya [aktarma](sql-data-warehouse-overview-migrate.md) işlemlerini nasıl gerçekleştireceğinize göz atın. Veya [Power BI web sitesi](http://www.powerbi.com/)'ni ziyaret edin.
