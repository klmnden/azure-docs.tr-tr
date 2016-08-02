<properties
   pageTitle="SQL Data Warehouse için Visual Studio'yu ve SSDT'yi Yükleme | Microsoft Azure"
   description="Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/01/2016"
   ms.author="sonyama;barbkess"/>

# SQL Data Warehouse için Visual Studio 2015'i ve SSDT'yi Yükleme

SQL Data Warehouse için uygulama geliştirmek üzere Visual Studio 2015'i, SQL Server Veri Araçları'nın (SSDT) en son sürümüyle birlikte kullanmanızı öneririz.  Geriye dönük uyumluluk için SSDT ile Visual Studio 2013 de desteklenir.  

Visual Studio'yu SSDT ile kullanmak; sorgu çalıştırmanın yanı sıra, SQL Server Nesne Gezgini'ni kullanarak SQL Data Warehouse'unuzda bulunan tabloları, görünümleri, saklı yordamları ve daha birçok nesneyi görsel olarak araştırmanıza olanak sağlar.

> [AZURE.NOTE] SQL Data Warehouse, henüz Visual Studio Veritabanı Projelerini desteklemiyor.  Bu özellik sonraki bir sürümde eklenecek.

## 1. Adım: Visual Studio 2015'i yükleme

Visual Studio 2015'i indirmek ve yüklemek için bu bağlantıları izleyin. Visual Studio 2013 veya 2015 zaten yüklü durumdaysa şuraya ilerleyebilirsiniz: 2. Adım: SSDT'yi Yükleme

1. [Visual Studio 2015’i İndirme][]
2. MSDN'de bulunan [Visual Studio’yu Yükleme][] kılavuzunu izleyin ve varsayılan yapılandırmaları seçin.

## 2. Adım: SSDT'yi yükleme

Visual Studio için SSDT yüklemek üzere aşağıdaki adımları izleyerek Visual Studio'da bir SSDT güncelleştirmesinin olup olmadığını kontrol edin.

1. Visual Studio'da **Araçlar** / **Uzantılar ve Güncelleştirmeler…** / **Güncelleştirmeler**'e tıklayın
2. **Ürün Güncelleştirmeleri**'ni seçip **Veritabanı araçları için Microsoft SQL Server Güncelleştirmesi** olup olmadığına bakın.

Güncelleştirme yoksa en son sürümün yüklü olduğu anlamına gelir.  SSDT'nin yüklendiğini doğrulamak **Yardım** / **Microsoft Visual Studio Hakkında**'ya tıklayın ve SQL Server Veri Araçları'nın listede olup olmadığına bakın.  14.0.60525.0 SSDT'nin en son sürümüdür.  Visual Studio'da yükleme seçeneği yoksa SSDT'yi el ile indirip yüklemek için [SSDT İndirme][] sayfasını da ziyaret edebilirsiniz.

## Sonraki adımlar

SSDT'nin en son sürümüne sahip olduğunuza göre SQL Data Warehouse'unuza [bağlanmaya][] hazırsınız.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[bağlanmaya]: ./sql-data-warehouse-get-started-connect.md

<!--Other-->
[Visual Studio 2015’i İndirme]: https://www.visualstudio.com/downloads/
[Visual Studio’yu Yükleme]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT İndirme]: https://msdn.microsoft.com/library/mt204009.aspx



<!--HONumber=Jun16_HO2-->


