---
title: "Azure Analysis Services içinde desteklenen veri kaynakları | Microsoft Docs"
description: "Azure Analysis Services veri modelleri için desteklenen veri kaynakları açıklar."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/27/2018
ms.author: owend
ms.openlocfilehash: 9f2a4acdd0a2b29bc1485f62c0049f0065cbf711
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Azure Analysis Services içinde desteklenen veri kaynakları

Veri kaynakları ve Veri Al veya Visual Studio'da İçeri Aktarma Sihirbazı'nda gösterilen bağlayıcıları Azure Analysis Services ve SQL Server Analysis Services için gösterilir. Ancak, tüm veri kaynakları ve gösterilen bağlayıcıları Azure Analysis Services içinde desteklenir. Uyumluluk düzeyi, kullanılabilir veri bağlayıcılar, kimlik doğrulama türü, sağlayıcıları ve şirket içi veri ağ geçidi desteği gibi birçok faktöre bağlıdır bağlandığınız veri kaynağı türleri model. 

## <a name="azure-data-sources"></a>Azure veri kaynakları

|Veri kaynağı  |Bellek içi  |DirectQuery  |
|---------|---------|---------|
|Azure SQL Database     |   Evet      |    Evet      |
|Azure SQL Veri Ambarı     |   Evet      |   Evet       |
|Azure Blob Depolama *     |   Evet       |    Hayır      |
|Azure tablo depolama *    |   Evet       |    Hayır      |
|Azure Cosmos DB (Beta)*     |  Evet        |  Hayır        |
|Azure Data Lake Store*     |   Evet       |    Hayır      |
|Azure Hdınsight HDFS *     |     Evet     |   Hayır       |
|Azure Hdınsight Spark (Beta) *     |   Evet       |   Hayır       |
|Azure veritabanı için MySQL (Önizleme) *     |   Evet       |   Hayır      |
|Azure veritabanı PostgreSQL (Önizleme) için *     | Evet         |  Hayır       |
||||

\* Yalnızca tablolu 1400 modeller için.

**Sağlayıcı**   
Bellek içi ve Azure veri kaynaklarına bağlanma DirectQuery modellerinde SQL Server için .NET Framework veri sağlayıcısı kullanın.

## <a name="on-premises-data-sources"></a>Şirket içi veri kaynakları

Bağlanan veri kaynaklarından ve Azure AS server bir şirket içi ağ geçidi gerektiren şirket içi. Bir ağ geçidi kullanırken, 64-bit sağlayıcıları gereklidir.

### <a name="in-memory-and-directquery"></a>Bellek içi ve DirectQuery

|Veri kaynağı | Bellek içi sağlayıcısı | DirectQuery provider |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0, SQL Server için Microsoft OLE DB sağlayıcısı, SQL Server için .NET Framework veri sağlayıcısı | SQL Server için .NET framework veri sağlayıcısı |
| SQL Server veri ambarı |SQL Server Native Client 11.0, SQL Server için Microsoft OLE DB sağlayıcısı, SQL Server için .NET Framework veri sağlayıcısı | SQL Server için .NET framework veri sağlayıcısı |
| Oracle |Oracle, .NET için Oracle veri sağlayıcısı için Microsoft OLE DB sağlayıcısı |.NET için Oracle veri sağlayıcısı | |
| Teradata |Teradata için .NET Teradata veri sağlayıcısı için OLE DB sağlayıcısı |.NET için Teradata veri sağlayıcısı | |
| | | |

### <a name="in-memory-only"></a>Bellek içi yalnızca

|Veri kaynağı  |  
|---------|---------|
|Access veritabanı     |  
|Active Directory*     |  
|Analysis Services     |  
|Analiz platformu sistemi     |  
|Dynamics CRM*     |  
|Excel çalışma kitabı     |  
|Exchange*     |  
|Klasör *     | 
|JSON belgesini *     |  
|Satırlarından ikili *     | 
|MySQL Veritabanı     | 
|OData akışı *     |  
|ODBC sorgu     | 
|OLE DB     |   
|Postgre SQL veritabanı *    | 
|SAP HANA*    |  
|SAP Business Warehouse *    |  
|SharePoint*     |   
|Sybase Veritabanı     |  
|XML tablo *    |  
|||
 
\* Yalnızca tablolu 1400 modeller için.

## <a name="specifying-a-different-provider"></a>Farklı bir sağlayıcı belirtme

Azure Analysis Services veri modelleri farklı veri sağlayıcıları belirli veri kaynaklarına bağlanırken gerektirebilir. Bazı durumlarda, tablolu modeller SQL Server Native Client (SQLNCLI11) gibi yerel sağlayıcılarını kullanarak veri kaynaklarına bağlanırken bir hata döndürebilir. SQLOLEDB dışındaki yerel sağlayıcıları kullanıyorsanız, hata iletisi görebilirsiniz: **Sağlayıcı 'SQLNCLI11.1' kayıtlı değil**. Ya da şirket içi veri kaynaklarına bağlanma DirectQuery modeli varsa ve yerel sağlayıcıları kullanır, hata iletisi görebilirsiniz: **OLE DB satır kümesi oluşturulurken hata oluştu. 'Sınırı' yakınındaki sözdizimi yanlış**.

Bir şirket içi SQL Server Analysis Services tablolu model Azure Analysis Services geçirirken, sağlayıcı değiştirmek gerekli olabilir.

**Bir sağlayıcı belirtmek için**

1. Ssdt'de > **tablolu Model Gezgini** > **veri kaynakları**, bir veri kaynağı bağlantısı sağ tıklayın ve ardından **veri kaynağını Düzenle**.
2. İçinde **bağlantı Düzenle**, tıklatın **Gelişmiş** Gelişmiş Özellikler penceresini açın.
3. İçinde **gelişmiş özelliklerini ayarla** > **sağlayıcıları**, uygun sağlayıcıyı seçin.

## <a name="impersonation"></a>Kimliğe bürünme
Bazı durumlarda, farklı kimliğe bürünme hesabı belirtmeniz gerekebilir. Kimliğe bürünme hesabı, Visual Studio (SSDT) veya SSMS belirtilebilir.

Şirket içi veri kaynakları için:

* SQL kimlik doğrulamasını kullanıyorsanız, kimliğe bürünme hizmet hesabı olması gerekir.
* Windows kimlik doğrulamasını kullanıyorsanız, Windows kullanıcı/parola ayarlayın. SQL Server için Windows kimlik doğrulaması belirli kimliğe bürünme hesabı ile yalnızca bellek içi veri modelleri için desteklenir.

Bulut veri kaynakları için:

* SQL kimlik doğrulamasını kullanıyorsanız, kimliğe bürünme hizmet hesabı olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi ağ geçidi](analysis-services-gateway.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)   

