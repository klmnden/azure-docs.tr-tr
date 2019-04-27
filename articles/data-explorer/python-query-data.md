---
title: 'Hızlı Başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak verileri Sorgulama'
description: Bu hızlı başlangıçta, Python kullanarak Azure Veri Gezgini'ndeki verileri sorgulamayı öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/16/2018
ms.openlocfilehash: 4de8f68e0384742cea4ce50ccd23a7455b186893
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60827162"
---
# <a name="quickstart-query-data-using-the-azure-data-explorer-python-library"></a>Hızlı Başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak verileri Sorgulama

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini [Python için bir veri istemci kitaplığı](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-data) sağlar. Bu kitaplık kodunuzdan verileri sorgulamanıza olanak tanır. Bu hızlı başlangıçta, eğitime yardımcı olması için ayarladığımız *yardım kümesindeki* bir tabloya bağlanırsınız. Ardından bu kümedeki tabloyu sorgular ve sonuçları döndürürsünüz.

Bu hızlı başlangıç bir [Azure Not Defteri](https://notebooks.azure.com/ManojRaheja/libraries/KustoPythonSamples/html/QueryKusto.ipynb) olarak da sağlanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure Active Directory (AAD) üyesi olan bir kuruluş e-posta hesabı

* Geliştirme bilgisayarınıza yüklenmiş [Python](https://www.python.org/downloads/)

## <a name="install-the-data-library"></a>Veri kitaplığını yükleme

*azure-kusto-data*'yı yükleyin.

```
pip install azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>İçeri aktarma deyimlerini ve sabitlerini ekleme

Kitaplıktan sınıfları ve ayrıca bir veri analizi kitaplığı olan *pandas*'ı içeri aktarın.

```python
from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
from azure.kusto.data.exceptions import KustoServiceError
from azure.kusto.data.helpers import dataframe_from_result_table
import pandas as pd
```

Azure Veri Gezgini uygulamanın kimliğini doğrulamak için AAD kiracı kimliğinizi kullanır. Kiracı kimliğinizi bulmak için aşağıdaki URL'yi kullanın ve *YourDomain* yerine kendi etki alanınızı yazın.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Örneğin, etki alanınız *contoso.com* olduğunda URL şöyle olur: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Sonuçları görmek için bu URL'ye tıklayın; ilk satır aşağıdaki gibidir.

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Bu örnekte kiracı kimliği `6babcaad-604b-40ac-a9d7-9fd97c0b779f` değeridir. Bu kodu çalıştırmadan önce AAD_TENANT_ID değerini ayarlayın.

```python
AAD_TENANT_ID = "<TenantId>"
KUSTO_CLUSTER = "https://help.kusto.windows.net/"
KUSTO_DATABASE  = "Samples"
```

Şimdi bağlantı dizesini hazırlayın. Bu örnekte kümeye erişmek için cihaz kimlik doğrulaması kullanılır. Ayrıca [AAD uygulama sertifikası](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L24), [AAD uygulama anahtarı](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L20), ve [AAD kullanıcı adı ve parola](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L34).

```python
KCSB = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_CLUSTER)
KCSB.authority_id = AAD_TENANT_ID
```

## <a name="connect-to-azure-data-explorer-and-execute-a-query"></a>Azure Veri Gezgini'ne bağlanma ve sorgu yürütme

Kümede bir sorgu yürütün ve çıkışı bir veri çerçevesinde depolayın. Bu kod çalıştığında, aşağıdaki gibi bir ileti döndürür: *Oturum açmak için bir web tarayıcısı kullanarak https://microsoft.com/devicelogin ve F3W4VWZDM kimliğini doğrulamak için kodu girin*. Adımları izleyerek oturum açın, sonra da dönüp bir sonraki kod bloğunu çalıştırın.

```python
KUSTO_CLIENT  = KustoClient(KCSB)
KUSTO_QUERY  = "StormEvents | sort by StartTime desc | take 10"

RESPONSE = KUSTO_CLIENT.execute(KUSTO_DATABASE, KUSTO_QUERY)
```

## <a name="explore-data-in-dataframe"></a>DataFrame'de verileri inceleme

Siz oturum açma bilgilerini girdikten sonra sorgu sonuçları döndürür ve bunlar veri çerçevesinde depolanır. Diğer herhangi bir veri çerçevesinde yaptığınız gibi sonuçlarla çalışabilirsiniz.

```python
df = dataframe_from_result_table(RESPONSE.primary_results[0])
df
```

StormEvents tablosunda ilk on sonucu görüyor olmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak veri alma](python-ingest-data.md)
