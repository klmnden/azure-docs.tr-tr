---
title: Power BI ile SQL Data Warehouse verilerini görselleştirme | Microsoft Azure
description: Power BI ile SQL Data Warehouse verilerini görselleştirme
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 94ec38bc2cad3566fad88dc2ac56648f79aa16b2
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873369"
---
# <a name="visualize-data-with-power-bi"></a>Power BI ile verileri görselleştirme
Bu öğreticide SQL Data Warehouse'a bağlanmak ve birkaç temel görselleştirme oluşturmak üzere Power BI'ı nasıl kullanacağınız gösterilmiştir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

* AdventureWorksDW veritabanı önceden yüklenmiş bir SQL Data Warehouse. Veri ambarı sağlamak için bkz: [SQL veri ambarı oluşturma](create-data-warehouse-portal.md) ve örnek verileri yüklemeyi seçin. Zaten bir veri ambarınız olmasına karşın örnek verileriniz yoksa [Wideworldımportersdw yük](load-data-wideworldimportersdw.md).

## <a name="1-connect-to-your-database"></a>1. Veritabanınıza bağlanın
Power BI'ı açmak ve AdventureWorksDW veritabanınıza bağlanmak için şunları yapın:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **SQL veritabanları** seçeneğine tıklayın ve AdventureWorks SQL Data Warehouse veritabanınızı seçin.
   
    ![Veritabanınızı bulma](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png)
3. "Power BI'da aç" düğmesine tıklayın.
   
    ![Power BI düğmesi](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png)
4. Bu işlemin ardından veritabanınızın web adresinin görüntülendiği SQL Data Warehouse bağlantı sayfası açılır. İleri'ye tıklayın.
   
    ![Power BI bağlantısı](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png)
5. Azure SQL sunucusu kullanıcı adı ve parolayı girin.
   
    ![Power BI oturum açma](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png)
6. Veritabanı'nı açmak için Sol dikey penceredeki AdventureWorksDW veri kümesine tıklayın.
   
    ![Power BI AdventureWorksDW'yi açma](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png)

## <a name="2-create-a-report"></a>2. Bir rapor oluşturun
Artık AdventureWorksDW örnek verilerinizi Power BI kullanarak çözümlemeye hazırsınız. AdventureWorksDW'de çözümlemenin gerçekleştirileceği AggregateSales adlı bir görünüm vardır. Bu görünüm, şirket satışlarının çözümlenmesine yönelik ana ölçümlerden birkaçını içerir.

1. Posta koduna göre satış tutarlarının bir haritasını oluşturmak için sağ taraftaki alanlar bölmesinde AggregateSales görünümüne tıklayıp görünümü genişletin. PostalCode ve SalesAmount sütünlarını tıklayarak seçin.
   
    ![Power BI AggregateSales seçer.](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png)
   
    Power BI, otomatik olarak coğrafi veri tanınan ve sizin için bir eşlem içinde yerleştirin.
   
    ![Power BI haritası](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png)

2. Bu adımda müşteri geliri başına satış tutarını gösteren bir çubuk grafiği oluşturulur. Çubuk grafiği oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount alanına tıklayın. Müşteri Geliri alanını sola sürükleyip Eksene bırakın.
   
    ![Power BI, eksen seçer.](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png)
   
    Sol üzerinde çubuk grafiği.
   
    ![Power BI çubuğu](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png)
3. Bu adımda, sipariş tarihi başına satış tutarını gösteren bir çizgi grafiği oluşturulur. Çizgi grafik oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount ve OrderDate öğelerine tıklayın Görselleştirmeler sütununun ikinci satırındaki ilk simge olan çizgi grafiği simgesine tıklayın.
   
    ![Power BI, çizgi grafik seçer.](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png)
   
    Artık verilerin üç farklı görselleştirmesini gösteren bir raporunuz var.
   
    ![Power BI çizgi grafiği](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png)

**Dosya**'ya tıklayıp **Kaydet**'i seçerek ilerleme durumunuzu istediğiniz zaman kaydedebilirsiniz.

## <a name="using-direct-connect"></a>Doğrudan kullanarak bağlan
Olarak Azure SQL veritabanı ile SQL veri ambarı doğrudan bağlanma analitik özellikleri Power BI'ın yanı sıra mantıksal itme sağlar. Verileri araştırırken doğrudan bağlantı ile sorgular Azure SQL Data Warehouse'da dön gerçek zamanlı gönderilir.  SQL veri ambarı, Ölçek ile birlikte bu özellik, terabaytlarca veriden birkaç dakika içinde dinamik raporlar oluşturmanıza olanak sağlar. Ayrıca, Power BI düğmesi açık sunulmasıyla doğrudan Power BI, SQL veri ambarı'na Azure diğer bölümlerden bilgi toplanmasına gerek kalmadan bağlanmasına olanak sağlar.

Doğrudan bağlantı kullanırken:

* Bağlanırken tam sunucu adını belirtin.
* Veritabanı için güvenlik duvarı kuralları, Azure hizmetlerine erişime izin ver için yapılandırıldığından emin olun.
* Sütun seçme veya filtre ekleme gibi her işlem, veri ambarına doğrudan sorgular.
* Kutucuklar otomatik olarak ve yaklaşık her 15 dakikada bir yenilenir.
* Soru- cevap, veri kümeleri doğrudan bağlanmak için kullanılamaz.
* Şema değişiklikleri otomatik olarak dahil edilir.
* Tüm sorguları doğrudan bağlanmak, 2 dakika sonra zaman aşımına uğrar.

Bu kısıtlamalar ve notlar deneyimleri iyileştirmeye olarak değişebilir.

## <a name="next-steps"></a>Sonraki adımlar
Size örnek verilerle alıştırma yapmanız için biraz zaman tanıdığımıza göre, [geliştirme](sql-data-warehouse-overview-develop.md), [yükleme](design-elt-data-loading.md) veya [aktarma](sql-data-warehouse-overview-migrate.md) işlemlerini nasıl gerçekleştireceğinize göz atın. Veya [Power BI web sitesi](https://www.powerbi.com/)'ni ziyaret edin.
