---
title: Azure Analysis Services'da desteklenen veri kaynakları | Microsoft Docs
description: Azure Analysis Services veri modelleri için desteklenen veri kaynakları açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e1a001a60151136be6bde9de38f971807cf0c288
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188411"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Azure Analysis Services'da desteklenen veri kaynakları

Azure Analysis Services ve SQL Server Analysis Services için veri kaynakları ve bağlayıcılar Veri Al veya Visual Studio içeri aktarma sihirbazında gösterilen gösterilir. Ancak, tüm veri kaynakları ve bağlayıcılar gösterilen Azure Analysis Services'de desteklenir. Uyumluluk düzeyi, kullanılabilir veri bağlayıcıları, kimlik doğrulama türü, sağlayıcıları ve şirket içi veri ağ geçidi desteği gibi birçok faktöre bağlıdır bağlanabileceğiniz veri kaynakları türlerini model. 

## <a name="azure-data-sources"></a>Azure veri kaynakları

|Veri kaynağı  |Bellek içi  |DirectQuery  |
|---------|---------|---------|
|Azure SQL Database     |   Evet      |    Evet      |
|Azure SQL Veri Ambarı     |   Evet      |   Evet       |
|Azure Blob Depolama *     |   Evet       |    Hayır      |
|Azure tablo depolama *    |   Evet       |    Hayır      |
|Azure Cosmos DB *     |  Evet        |  Hayır        |
|Azure Data Lake Store *     |   Evet       |    Hayır      |
|Azure HDInsight HDFS *     |     Evet     |   Hayır       |
|Azure HDInsight Spark *     |   Evet       |   Hayır       |
||||

\* Yalnızca tablosal 1400 modelleri için.

**Sağlayıcı**   
Bellek içi ve Azure veri kaynaklarına bağlanma DirectQuery modellerinde SQL Server için .NET Framework veri sağlayıcısı kullanın.

## <a name="on-premises-data-sources"></a>Şirket içi veri kaynakları

Bağlanan veri kaynaklarından ve Azure AS sunucusuna bir şirket içi ağ geçidi gerektiren şirket içi. 64-bit sağlayıcıları, ağ geçidi kullanılırken gereklidir.

### <a name="in-memory-and-directquery"></a>Bellek içi ve DirectQuery

|Veri kaynağı | Bellek içi sağlayıcısı | DirectQuery sağlayıcısı |
|  --- | --- | --- |
| SQL Server |SQL Server yerel istemcisi 11.0, SQL Server için Microsoft OLE DB sağlayıcısı, SQL Server için .NET Framework veri sağlayıcısı | SQL Server için .NET framework veri sağlayıcısı |
| SQL Server veri ambarı |SQL Server yerel istemcisi 11.0, SQL Server için Microsoft OLE DB sağlayıcısı, SQL Server için .NET Framework veri sağlayıcısı | SQL Server için .NET framework veri sağlayıcısı |
| Oracle |Oracle, .NET için Oracle veri sağlayıcısı için Microsoft OLE DB sağlayıcısı |.NET için Oracle veri sağlayıcısı | |
| Teradata |Teradata için .NET Teradata veri sağlayıcısı için OLE DB sağlayıcısı |.NET için Teradata veri sağlayıcısı | |
| | | |

### <a name="in-memory-only"></a>Bellek içi yalnızca

|Veri kaynağı  |  
|---------|---------|
|Access veritabanı     |  
|Active Directory *     |  
|Analysis Services     |  
|Çözümleme Platform sistemi     |  
|Dynamics CRM *     |  
|Excel çalışma kitabı     |  
|Exchange *     |  
|Klasör *     |
|IBM Informix * (Beta) |
|JSON belgesi *     |  
|Satırlardan ikili *     | 
|MySQL Veritabanı     | 
|OData akışı *     |  
|ODBC sorgu     | 
|OLE DB     |   
|Postgre SQL veritabanı *    | 
|Salesforce nesneleri * |  
|Salesforce raporları * |
|SAP HANA *    |  
|SAP Business Warehouse *    |  
|SharePoint *     |   
|Sybase Veritabanı     |  
|XML tablosu *    |  
|||
 
\* Yalnızca tablosal 1400 modelleri için.

## <a name="specifying-a-different-provider"></a>Farklı bir sağlayıcı belirtme

Azure Analysis Services veri modelleri, belirli veri kaynaklarına bağlanırken görüntülenen farklı veri sağlayıcıları gerektirebilir. Bazı durumlarda, tablosal modeller SQL Server Native Client (SQLNCLI11) gibi yerel sağlayıcılarını kullanarak veri kaynaklarına bağlanan bir hata döndürebilir. SQLOLEDB dışındaki yerel sağlayıcılarını kullanarak hata iletisini görebilirsiniz: **Sağlayıcı 'SQLNCLI11.1' kayıtlı değil**. Ya da şirket içi veri kaynaklarına bağlanma bir DirectQuery modeli kullandığınız ve yerel sağlayıcıları kullanıyorsanız hata iletisini görebilirsiniz: **OLE DB satır kümesi oluşturulurken hata oluştu. 'LIMIT' yakınındaki sözdizimi yanlış**.

Azure Analysis Services için bir şirket içi SQL Server Analysis Services tablolu modeli geçiş yaparken, sağlayıcı değiştirmek gerekli olabilir.

**Bir sağlayıcısını belirtmek için**

1. SSDT > **Tablosal Model Gezgini** > **veri kaynakları**bir veri kaynağı bağlantısı sağ tıklayın ve ardından **veri kaynağını Düzenle**.
2. İçinde **bağlantı Düzenle**, tıklayın **Gelişmiş** Gelişmiş Özellikler penceresini açın.
3. İçinde **gelişmiş özelliklerini ayarla** > **sağlayıcıları**, ardından uygun sağlayıcıyı seçin.

## <a name="impersonation"></a>Kimliğe bürünme
Bazı durumlarda, farklı kimliğe bürünme hesabı belirtmeniz gerekebilir. Kimliğe bürünme hesabı Visual Studio (SSDT) veya SSMS belirtilebilir.

Şirket içi veri kaynakları için:

* SQL kimlik doğrulaması kullanıyorsanız, kimliğe bürünme hizmet hesabı olması gerekir.
* Windows kimlik doğrulamasını kullanıyorsanız, Windows kullanıcı/parola ayarlayın. SQL Server için Windows kimlik doğrulaması ile belirli bir kimliğe bürünme hesabı yalnızca bellek içi veri modelleri için desteklenir.

Bulut veri kaynakları için:

* SQL kimlik doğrulaması kullanıyorsanız, kimliğe bürünme hizmet hesabı olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi ağ geçidi](analysis-services-gateway.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)   

