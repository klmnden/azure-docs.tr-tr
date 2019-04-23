---
title: Python kullanarak Azure Databricks'ten Azure veri Gezgini'ne bağlanma
description: Bu konuda iki kimlik doğrulama yöntemlerinden birini kullanarak Azure veri Gezgini'nde verilere erişmek için bir Python Kitaplığı'nda Azure Databricks kullanmayı gösterir.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 55257d441916971b505432247f28033d6222c3be
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59789966"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-by-using-python"></a>Python kullanarak Azure Databricks'ten Azure veri Gezgini'ne bağlanma

[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) Microsoft Azure platformu için en iyi duruma getirilmiş bir Apache Spark temelli analiz platformudur. Bu makalede, Azure databricks'te bir Python kitaplığı verilere erişmek için Azure veri Gezgini'nde kullanmayı gösterir. Bir cihaz oturum açma ve Azure Active Directory (Azure AD) uygulama dahil olmak üzere Azure Veri Gezgini ile kimlik doğrulaması yapmak için birkaç yolu vardır.

## <a name="prerequisites"></a>Önkoşullar

- [Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](/azure/data-explorer/create-cluster-database-portal).
- [Bir Azure Databricks çalışma alanı oluşturma](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace). Altında **Azure Databricks hizmeti**, **fiyatlandırma katmanı** aşağı açılan listesinden **Premium**. Bu seçim, kimlik bilgilerinizi depolamak ve bunlara not defterlerini ve işleri başvurmak için Azure Databricks gizli dizileri kullanmanıza olanak sağlar.

- [Küme oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html) aşağıdaki belirtimler (örnek not defterlerini çalıştırmak için gereken en düşük ayarları) ile Azure databricks'te:

   ![Bir küme oluşturmak için belirtimleri](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-the-python-library-on-your-azure-databricks-cluster"></a>Python kitaplığı kullanarak Azure Databricks kümenizde yükleyin

Yüklenecek [Python Kitaplığı](/azure/kusto/api/python/kusto-python-client-library) Azure Databricks kümenizdeki:

1. Azure Databricks çalışma alanınıza gidin ve [bir kitaplığı oluşturma](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library).
2. [Python Pypı paket veya Python Yumurta karşıya](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg).
   - Karşıya yükleme, yükleyin ve kitaplığı, Databricks kümesine ekleyin.
   - Pypı adını girin: **azure kusto veri**.

## <a name="connect-to-azure-data-explorer-by-using-a-device-login"></a>Bir cihaz kimliği kullanarak Azure veri Gezgini'ne bağlanma

[Bir not defteri alma](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) kullanarak [ADX cihaz oturumu sorgu](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb) dizüstü bilgisayar. Ardından, Azure veri Gezgini'ne kimlik bilgilerinizi kullanarak bağlanabilirsiniz.

## <a name="connect-to-adx-by-using-an-azure-ad-app"></a>ADX için bir Azure AD uygulamasını kullanarak bağlanma

1. Azure AD uygulaması tarafından oluşturma [bir Azure AD uygulaması sağlama](/azure/kusto/management/access-control/how-to-provision-aad-app).
1. Azure AD uygulamanız aşağıdaki gibi Azure Veri Gezgini veritabanınızdaki izni verin:

    ```kusto
    .set database <DB Name> users ('aadapp=<AAD App ID>;<AAD Tenant ID>') 'AAD App to connect Spark to ADX
    ```
    |   |   |
    | - | - |
    | ```DB Name``` | Veritabanı adınız |
    | ```AAD App ID``` | Azure AD uygulama Kimliğiniz |
    | ```AAD Tenant ID``` | Azure AD Kiracı Kimliğinizi |

### <a name="find-your-azure-ad-tenant-id"></a>Azure AD Kiracı Kimliğinizi bulun

Uygulama kimliğini doğrulamak için Azure Veri Gezgini, Azure AD Kiracı kimliğinizi kullanır Kiracı Kimliğinizi bulmak için şu URL'yi kullanın. Etki alanınız için alternatif *etkialanınız*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Örneğin, etki alanınız *contoso.com* olduğunda URL şöyle olur: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Sonuçları görmek için bu URL'yi seçin. İlk satırı aşağıdaki gibidir: 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Kiracınızın kimliği `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Store ve Azure AD uygulama kimliği ve anahtarı güvenliğini sağlama 

Store ve Azure AD uygulama kimliği ve anahtarı Azure Databricks kullanarak güvenli hale getirme [gizli dizileri](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) gibi:
1. [CLI'yı ayarlama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli).
1. [CLI'yı yükleme](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 
1. [Kimlik doğrulamasını ayarlama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication).
1. Yapılandırma [gizli dizileri](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) kullanarak aşağıdaki örnek komutlar:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Bir not defteri alma
[Bir not defteri alma](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) kullanarak [sorgu ADX AAD uygulama](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb) Azure veri Gezgini'ne bağlanmak için Not Defteri. Küme adı, veritabanı adı ve Azure AD Kiracı kimliği ile yer tutucu değerlerini güncelleştirin.
