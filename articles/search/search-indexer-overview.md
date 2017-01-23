---
title: "Azure Search&quot;te dizin oluşturucular | Microsoft Belgeleri"
description: "Aranabilir verileri ayıklamak ve bir Azure Search dizinini doldurmak için Azure SQL Database, DocumentDB veya Azure depolama alanında gezinin."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 34a7694c-8fd9-46b1-8900-cefdd7236323
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 4bcd31a200024a182ee3d5a21bcbcb621fed595f
ms.openlocfilehash: fd46641709d260f8b468556972aae14205fdb515

---

# <a name="indexers-in-azure-search"></a>Azure Search'te dizin oluşturucular
> [!div class="op_single_selector"]
>
> * [Genel Bakış](search-indexer-overview.md)
> * [Portal](search-import-data-portal.md)
> * [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
> * [DocumentDB](search-howto-index-documentdb.md)
> * [Blob Depolama (önizleme)](search-howto-indexing-azure-blob-storage.md)
> * [Tablo Depolama (önizleme)](search-howto-indexing-azure-tables.md)
>
>

Azure Search'te **dizin oluşturucu**, dış bir veri kaynağından aranabilir verileri ve meta verileri ayıklayan ve dizin ile veri kaynağınız arasında alandan alana eşlemeleri temel alan bir dizini dolduran gezgindir. Bir dizine veri gönderen herhangi bir kodun yazılması gerekmeden hizmetin verileri çekmesi nedeniyle bu yaklaşıma bazen "çekme modeli" de denir.

Bir dizin oluşturucusunu yalnızca veri alımı amacıyla kullanabilir veya dizininize alanların yalnızca bazılarını yüklemek için bir dizin oluşturucu kullanımını içeren bir teknikler birleşimini kullanabilirsiniz.

Dizin oluşturucuları isteğe bağlı olarak veya her on beş dakikada bir çalıştırılan bir yinelenen veri yenileme zamanlamasıyla çalıştırabilirsiniz. Daha sık güncelleştirmeler için hem Azure Search'te hem de dış veri kaynağınızda verileri aynı anda güncelleştiren bir gönderme modeli gerekir.

## <a name="approaches-for-creating-and-managing-indexers"></a>Dizin oluşturucular oluşturma ve yönetme yaklaşımları
Azure SQL veya DocumentDB gibi genel kullanıma açık dizin oluşturucular için bu yaklaşımları kullanarak dizin oluşturucular oluşturabilir ve bunları yönetebilirsiniz:

* [Portal > Veri Alma Sihirbazı](search-get-started-portal.md)
* [Hizmet REST API'si](https://msdn.microsoft.com/library/azure/dn946891.aspx)
* [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

## <a name="basic-configuration-steps"></a>Temel yapılandırma adımları
Dizin oluşturucular veri kaynağına özgü özellikler sunabilir. Bu bakımdan, dizin oluşturucu veya veri kaynağı yapılandırmasının bazı boyutları dizin oluşturucu türüne göre farklılık gösterir. Bununla birlikte, tüm dizin oluşturucuların temel birleşimi ve gereksinimleri aynıdır. Tüm dizin oluşturucularda ortak olan adımlar aşağıda ele alınmıştır.

### <a name="step-1-create-an-index"></a>1. Adım: Dizin oluşturma
Dizin oluşturucu veri alımıyla ilgili bazı görevleri otomatikleştirir, ancak dizin oluşturma bu görevlerden biri değildir. Bir önkoşul olarak dış veri kaynağınızdaki alanlarla eşleşen alanlara sahip önceden tanımlı bir dizininiz olmalıdır. Bir dizini yapılandırma konusunda daha fazla bilgi için bkz. [Dizin oluşturma (Azure Search REST API’si)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### <a name="step-2-create-a-data-source"></a>2. Adım: Veri kaynağı oluşturma
Dizin oluşturucu, bağlantı dizesi gibi bilgileri içeren bir **veri kaynağından** veri çeker. Şu anda aşağıdaki veri kaynakları desteklenmektedir:

* [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [DocumentDB](search-howto-index-documentdb.md)
* [Azure Blob depolama ](search-howto-indexing-azure-blob-storage.md): PDF, Office belgeleri, HTML veya XML’den metin ayıklamak için kullanılır
* [Azure Table Storage](search-howto-indexing-azure-tables.md)

Veri kaynakları, bunları kullanan dizin oluşturuculardan bağımsız olarak yapılandırılır ve yönetilir. Bu da bir veri kaynağının, bir seferde birden çok dizin yüklemek amacıyla birden çok dizin oluşturucu tarafından kullanılabileceği anlamına gelir.

### <a name="step-3create-and-schedule-the-indexer"></a>3. Adım: Dizin oluşturucuyu oluşturma ve zamanlama
Dizin oluşturucu tanımı dizini, veri kaynağını ve bir zamanlamayı belirten bir yapıdır. Bir dizin oluşturucu, aynı abonelikten olduğu sürece başka bir hizmetteki bir veri kaynağına başvurabilir. Bir dizin oluşturucuyu yapılandırma konusunda daha fazla bilgi için bkz. [Dizin Oluşturucu Oluşturma (Azure Search REST API’si)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Artık temel fikri anladığınıza göre, atmanız gereken bir sonraki adım her bir veri kaynağı türüne özgü gereksinimleri ve görevleri incelemektir.

* [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [DocumentDB](search-howto-index-documentdb.md)
* [Azure Blob Depolama ](search-howto-indexing-azure-blob-storage.md): PDF, Office belgeleri, HTML veya XML’den metin ayıklamak için kullanılır
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Azure Search Blob dizin oluşturucu (Önizleme) kullanarak CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
* [Azure Search Blob dizin oluşturucu (Önizleme) ile JSON bloblarını dizine ekleme](search-howto-index-json-blobs.md)



<!--HONumber=Jan17_HO3-->


