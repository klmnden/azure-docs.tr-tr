---
title: Azure işlevleri (Deneysel) için dış tablo bağlama
description: Azure işlevleri'nde dış tablo bağlamaları kullanma
services: functions
author: craigshoemaker
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: cshoe
ms.openlocfilehash: 62924488b776a1a89e1abf492db1881a44585b1a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997819"
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Azure işlevleri (Deneysel) için dış tablo bağlama

Bu makalede, Sharepoint ve Dynamics, Azure işlevleri'nde gibi SaaS sağlayıcıları tablo verileriyle çalışacak açıklanmaktadır. Giriş ve çıkış bağlamaları dış tablolar için Azure işlevleri destekler.

> [!IMPORTANT]
> Dış tablo bağlama, Deneysel ve genel kullanıma (GA) durumu hiçbir zaman ulaşın. Yalnızca Azure'da bulunan 1.x işlevleri ve Azure işlevleri'ne ekleyerek bir plan yok 2.x. SaaS sağlayıcıları verilere erişim gerektiren senaryolar için kullanmayı [işlevlerini çağıran logic apps'i](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API bağlantıları

Tablo bağlamaları, üçüncü taraf SaaS sağlayıcıları ile kimlik doğrulaması için dış API bağlantıları yararlanın. 

Bağlama atarken yeni bir API bağlantısı oluşturabilir veya var olan bir API bağlantısı aynı kaynak grubunda kullanın.

### <a name="available-api-connections-tables"></a>Kullanılabilir API bağlantıları (tablolar)

|Bağlayıcı|Tetikleyici|Girdi|Çıktı|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Operasyonlar için Dynamics 365](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Google E-Tablolar](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 for Financials için](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Veritabanı](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Common Data Service'e](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](https://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x

> [!NOTE]
> Dış tablo bağlantılar da kullanılabilir [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="creating-an-api-connection-step-by-step"></a>API bağlantısı oluşturuluyor: adım adım

1. İşlev uygulamanız için Azure portal sayfasında bulunan artı işaretini seçin (**+**) bir işlev oluşturmak için.

1. İçinde **senaryo** kutusunda **Deneysel**.

1. Seçin **dış tablo**.

1. Bir dil seçin.

2. Altında **dış tablo bağlantısı**, varolan bir bağlantı seçin ya da seçin **yeni**.

1. Yeni bir bağlantı için ayarları yapılandırın ve seçin **Authorize**.

1. Seçin **Oluştur** işlevi oluşturmak için.

1. Seçin **tümleştirme > Dış tablo**.

1. Bağlantıyı hedef tablonuz kullanacak şekilde yapılandırın. Bu ayarlar, SaaS sağlayıcıları arasında farklılık gösterir. Örnekler, aşağıdaki bölümde dahil edilir.

## <a name="example"></a>Örnek

Bu örnek kimliği, Soyadı ve FirstName sütunlarla "ilgili kişi" adlı bir tablo bağlanır. Kod tablosunda kişi varlıkları listeler ve ilk ve son adları günlüğe kaydeder.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retrieve table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

### <a name="sql-server-data-source"></a>SQL Server veri kaynağı

SQL Server'ın bu örneği kullanmak için bir tablo oluşturmak için bir komut dosyası aşağıdadır. `dataSetName` "varsayılan."

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets-data-source"></a>Google e-tabloları veri kaynağı

Bu örnekte Google Docs ile kullanılacak bir tablo oluşturmak için bir elektronik tablo adlı bir çalışma sayfası ile oluşturmak `Contact`. Bağlayıcı, elektronik tablo görünen adı kullanamazsınız. Örneğin dataSetName kullanılacak iç adı (kalın) gereksinimlerini: `docs.google.com/spreadsheets/d/` **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** sütun adlarını ekleyin `Id`, `LastName`, `FirstName` ilk satırın ardından şirket verilerini doldurun sonraki satır.

### <a name="salesforce"></a>Salesforce

Salesforce ile bu örneği kullanmak için `dataSetName` "varsayılan."

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|----------------------|
|**type** | Ayarlanmalıdır `apiHubTable`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | Ayarlanmalıdır `in`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | İşlev kodunu olay öğeyi temsil eden değişken adı. | 
|**bağlantı**| API bağlantı dizesi depolar uygulama ayarı tanımlar. Tümleştir UI içinde bir API bağlantısı eklediğinizde, uygulama ayarı otomatik olarak oluşturulur.|
|**dataSetName**|Okunacak tablo içeren veri kümesinin adı.|
|**TableName**|Tablonun adı|
|**Entityıd**|Tablo bağlamaları için boş olmalıdır.

Her bir veri kümesi tablolarını içeren ve veri kümelerini tablo bir bağlayıcı sağlar. "Varsayılan" varsayılan veri kümesinin adıdır Bir veri kümesi ve bir tablodaki çeşitli SaaS sağlayıcıları konu başlıklarını aşağıda listelenmiştir:

|Bağlayıcı|Veri kümesi|Tablo|
|:-----|:---|:---| 
|**SharePoint**|Site|SharePoint listesi
|**SQL**|Database|Tablo 
|**Google sayfası**|Elektronik tablo|Çalışma sayfası 
|**Excel**|Excel dosyası|Sayfa 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
