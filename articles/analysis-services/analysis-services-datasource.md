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
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 5ba7ef5aa2cccad7cda3cb39459a5a5722516524
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Azure Analysis Services içinde desteklenen veri kaynakları
Azure Analysis Services sunucuları, bulut veri kaynaklarında ve şirket içi kuruluşunuzdaki bağlanmasını destekler. Ek desteklenen veri kaynaklarının her zaman eklenir. Geri sık sık kontrol edin. 

Aşağıdaki veri kaynakları şu anda desteklenir:

| Bulut  |
|---|
| Azure Blob Depolama *  |
| Azure SQL Database  |
| Azure veri ambarı |


| Şirket içi  |   |   |   |
|---|---|---|---|
| Access veritabanı  | Klasör * | Oracle Veritabanı  | Teradata Database |
| Active Directory *  | JSON belgesini *  | Postgre SQL veritabanı *  |XML tablo * |
| Analysis Services  | Satırlarından ikili *  | SAP HANA *  |
| Analiz platformu sistemi  | MySQL Veritabanı  | SAP Business Warehouse *  | |
| Dynamics CRM *  | OData akışı *  | SharePoint *  |
| Excel çalışma kitabı  | ODBC sorgu  | SQL Veritabanı  |
| Exchange *  | OLE DB  | Sybase veritabanı  |

\*Yalnızca tablolu 1400 modeller için. 

> [!IMPORTANT]
> Şirket içi veri kaynaklarına bağlanma gerektiren bir [şirket içi veri ağ geçidi](analysis-services-gateway.md) ortamınızdaki bir bilgisayara yüklenmiş.

## <a name="data-providers"></a>Veri sağlayıcıları

Azure Analysis Services veri modelleri farklı veri sağlayıcıları belirli veri kaynaklarına bağlanırken gerektirebilir. Bazı durumlarda, tablolu modeller SQL Server Native Client (SQLNCLI11) gibi yerel sağlayıcılarını kullanarak veri kaynaklarına bağlanırken bir hata döndürebilir.

Bulut veri bağlanmak veri modelleri için kaynak gibi Azure SQL veritabanı, yerel sağlayıcıları SQLOLEDB dışında kullanırsanız, hata iletisi görebilirsiniz: **"Sağlayıcı 'SQLNCLI11.1' kayıtlı değil."** Ya da yerel sağlayıcıları kullanıyorsanız, şirket içi veri kaynaklarına bağlanma DirectQuery modeli varsa hata iletisi görebilirsiniz: **"OLE DB satır kümesi oluşturulurken hata oluştu. 'Sınırı' yakınındaki sözdizimi yanlış "**.

Aşağıdaki veri kaynağı sağlayıcıları, bulutta veya şirket içi veri kaynaklarına bağlanırken bellek içi veya DirectQuery veri modelleri için desteklenir:

### <a name="cloud"></a>Bulut
| **Veri kaynağı** | **Bellek içi** | **DirectQuery** |
|  --- | --- | --- |
| Azure SQL Veri Ambarı |SQL Server için .NET framework veri sağlayıcısı |SQL Server için .NET framework veri sağlayıcısı |
| Azure SQL Database |SQL Server için .NET framework veri sağlayıcısı |SQL Server için .NET framework veri sağlayıcısı | |

### <a name="on-premises-via-gateway"></a>Şirket içi (yoluyla ağ geçidi)
|**Veri kaynağı** | **Bellek içi** | **DirectQuery** |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0 |SQL Server için .NET framework veri sağlayıcısı |
| SQL Server |SQL Server için Microsoft OLE DB sağlayıcısı |SQL Server için .NET framework veri sağlayıcısı | |
| SQL Server |SQL Server için .NET framework veri sağlayıcısı |SQL Server için .NET framework veri sağlayıcısı | |
| Oracle |Oracle için Microsoft OLE DB sağlayıcısı |.NET için Oracle veri sağlayıcısı | |
| Oracle |.NET için Oracle veri sağlayıcısı |.NET için Oracle veri sağlayıcısı | |
| Teradata |Teradata için OLE DB sağlayıcısı |.NET için Teradata veri sağlayıcısı | |
| Teradata |.NET için Teradata veri sağlayıcısı |.NET için Teradata veri sağlayıcısı | |
| Analiz platformu sistemi |SQL Server için .NET framework veri sağlayıcısı |SQL Server için .NET framework veri sağlayıcısı | |

> [!NOTE]
> 64-bit sağlayıcıları, şirket içi ağ geçidi kullanırken yüklü olduğundan emin olun.
> 
> 

Bir şirket içi SQL Server Analysis Services tablolu model Azure Analysis Services geçirirken, sağlayıcı değiştirmek gerekli olabilir.

**Bir veri kaynağı sağlayıcısı belirtmek için**

1. Ssdt'de > **tablolu Model Gezgini** > **veri kaynakları**, bir veri kaynağı bağlantısı sağ tıklayın ve ardından **veri kaynağını Düzenle**.
2. İçinde **bağlantı Düzenle**, tıklatın **Gelişmiş** Gelişmiş Özellikler penceresini açın.
3. İçinde **gelişmiş özelliklerini ayarla** > **sağlayıcıları**, uygun sağlayıcıyı seçin.

## <a name="impersonation"></a>Kimliğe bürünme
Bazı durumlarda, farklı kimliğe bürünme hesabı belirtmeniz gerekebilir. Kimliğe bürünme hesabı SSDT veya SSMS belirtilebilir.

Şirket içi veri kaynakları için:

* SQL kimlik doğrulamasını kullanıyorsanız, kimliğe bürünme hizmet hesabı olması gerekir.
* Windows kimlik doğrulamasını kullanıyorsanız, Windows kullanıcı/parola ayarlayın. SQL Server için Windows kimlik doğrulaması belirli kimliğe bürünme hesabı ile yalnızca bellek içi veri modelleri için desteklenir.

Bulut veri kaynakları için:

* SQL kimlik doğrulamasını kullanıyorsanız, kimliğe bürünme hizmet hesabı olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Şirket içi veri kaynakları varsa, yüklediğinizden emin olun [şirket içi ağ geçidi](analysis-services-gateway.md).   
Sunucunuzu SSDT veya SSMS yönetme hakkında daha fazla bilgi için bkz: [sunucunuzu yönetin](analysis-services-manage.md).

