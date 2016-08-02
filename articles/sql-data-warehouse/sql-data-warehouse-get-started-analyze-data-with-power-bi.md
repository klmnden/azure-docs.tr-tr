<properties
   pageTitle="SQL Data Warehouse verilerini Power BI ile görselleştirme | Microsoft Azure"
   description="Power BI ile SQL Data Warehouse verilerini görselleştirme"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="03/03/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Power BI ile verileri görselleştirme

Bu öğreticide SQL Data Warehouse'a bağlanmak ve birkaç temel görselleştirme oluşturmak üzere Power BI'ı nasıl kullanacağınız gösterilmiştir.

> [AZURE.NOTE] Bu öğreticiyi tamamlamanız için önceden AdventureWorksDW örnek veritabanı yüklenmiş bir SQL Data Warehouse veritabanınızın olması gerekir. [SQL Data Warehouse oluşturma](sql-data-warehouse-get-started-provision.md) makalesinde bu işlemi nasıl gerçekleştireceğiniz gösterilmiştir.
>
> Bir SQL Data Warehouse veritabanınız var ancak örnek verileriniz yoksa [örnek verileri el ile yükleyebilirsiniz][].

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## AdventureWorksDW veritabanına bağlanma

Power BI'ı açmak ve AdventureWorksDW veritabanınıza bağlanmak için şunları yapın:

1. [Azure portalında][] oturum açın.
2. **SQL veritabanları** seçeneğine tıklayın ve AdventureWorks SQL Data Warehouse veritabanınızı seçin.

    ![Veritabanınızı bulma][1]

3. "Power BI'da aç" düğmesine tıklayın.

    ![Power BI düğmesi][2]

4. Bu işlemin ardından veritabanınızın web adresinin görüntülendiği SQL Data Warehouse bağlantı sayfası açılır. İleri'ye tıklayın

    ![Power BI bağlantısı][3]

6. Azure SQL sunucusuna ilişkin kullanıcı adı ve parolanızı girdikten sonra SQL Data Warehouse veritabanınıza tamamen bağlanmış olursunuz.

    ![Power BI'da oturum açma][4]

1. Power BI'da oturum açtıktan sonra sol dikey penceredeki AdventureWorksDW veri kümesine tıklayın. Bu işlemin ardından veritabanı açılır.

    ![Power BI AdventureWorksDW'yi açma][5]



## Örnek verileri çözümlemek üzere bir Power BI raporu oluşturma

Artık AdventureWorksDW örnek verilerinizi Power BI kullanarak çözümlemeye hazırsınız. AdventureWorksDW'de çözümlemenin gerçekleştirileceği AggregateSales adlı bir görünüm vardır. Bu görünüm, şirket satışlarının çözümlenmesine yönelik ana ölçümlerden birkaçını içerir.

1. Posta koduna göre satış tutarlarının bir haritasını oluşturmak için sağ taraftaki alanlar bölmesinde AggregateSales görünümüne tıklayıp görünümü genişletin. PostalCode ve SalesAmount sütünlarını tıklayarak seçin.

    ![Power BI AggregateSales görünümünü seçme][6]

    Power BI otomatik olarak bu verilerin coğrafi veriler olduğunu tanır ve verileri sizin için bir haritaya yerleştirir.

    ![Power BI haritası][7]

2. Bu adımda müşteri geliri başına satış tutarını gösteren bir çubuk grafiği oluşturulur. Bu grafiği oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount alanına tıklayın. Müşteri Geliri alanını sola sürükleyip Eksene bırakın.

    ![Power BI eksen seçme][8]

    Çubuk grafiğini sola taşıdık.

    ![Power BI çubuğu][9]

3. Bu adımda, sipariş tarihi başına satış tutarını gösteren bir çizgi grafiği oluşturulur. Bu grafiği oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount ve OrderDate öğelerine tıklayın Görselleştirmeler sütununun ikinci satırındaki ilk simge olan Çizgi Grafiği simgesine tıklayın.

    ![Power BI çizgi grafiğini seçme][10]

    Artık verilerin üç farklı görselleştirmesini gösteren bir raporunuz var.

    ![Power BI çizgi grafiği][11]

**Dosya**'ya tıklayıp **Kaydet**'i seçerek ilerleme durumunuzu istediğiniz zaman kaydedebilirsiniz.

## Sonraki adımlar
Size örnek verilerle alıştırma yapmanız için biraz zaman tanıdığımıza göre, [geliştirme][], [yükleme][] veya [aktarma][] işlemlerini nasıl gerçekleştireceğinize göz atın.

<!--Image references-->
[1]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-find-database.png
[2]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-button.png
[3]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-connect-to-azure.png
[4]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-sign-in.png
[5]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-open-adventureworks.png
[6]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-aggregatesales.png
[7]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-map.png
[8]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-chooseaxis.png
[9]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-bar.png
[10]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-prepare-line.png
[11]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-line.png
[12]:./media/sql-data-warehouse-get-started-analyze-data-with-power-bi/pbi-save.png

<!--Article references-->
[geçirme]: ./sql-data-warehouse-overview-migrate.md
[geliştirme]: ./sql-data-warehouse-overview-develop.md
[yükleme]: ./sql-data-warehouse-overview-load.md
[örnek verileri el ile yükleyebilirsiniz]: ./sql-data-warehouse-get-started-manually-load-samples.md
[Azure portalında]: https://portal.azure.com/
[Power BI]: http://www.powerbi.com/
[SQL Data Warehouse'a bağlanma]: ./sql-data-warehouse-integrate-power-bi.md
[SQL Data Warehouse oluşturma]: ./sql-data-warehouse-get-started-provision.md



<!--HONumber=Jun16_HO2-->


