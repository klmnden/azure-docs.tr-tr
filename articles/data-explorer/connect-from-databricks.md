---
title: Python kullanarak Azure Databricks'ten Azure veri Gezgini'ne bağlanma
description: Bu konuda iki kimlik doğrulama yöntemlerinden birini kullanarak Python Kitaplığı'nda Azure Databricks, Azure Veri Gezgini (ADX) verilerine erişmek için kullanma işlemini gösterir.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 53f51db8d1b495f9a6faec86450d2b4e08a4fb72
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52428525"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-using-python"></a>Python kullanarak Azure Databricks'ten Azure veri Gezgini'ne bağlanma

[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks), Microsoft Azure bulut hizmetleri platformu için iyileştirilen Apache Spark tabanlı bir analiz platformudur. Bu makalede Python Kitaplığı'nda Azure Databricks, Azure Veri Gezgini (ADX) verilerine erişmek için kullanma gösterilmektedir. Cihaz oturum açma ve Azure Active Directory (Azure AD) uygulama da dahil olmak üzere ADX ile kimlik doğrulaması yapmak için birkaç yolu vardır.

## <a name="prerequisites"></a>Önkoşullar

- [Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](/azure/data-explorer/create-cluster-database-portal)
- [Bir Azure Databricks çalışma alanı oluşturma](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace)

    Altında **Azure Databricks hizmeti**, **fiyatlandırma katmanı** açılır menüsünde, select **Premium**. Bu kimlik bilgilerinizi depolamak ve bunlara not defterlerini ve işleri başvurmak için Azure Databricks parolaları kullanmanızı sağlar.

- [Küme oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html) aşağıdaki belirtimler (örnek not defterlerini çalıştırmak için gereken en düşük ayarları) ile Azure databricks'te:

![Küme oluşturma](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-python-library-on-your-azure-databricks-cluster"></a>Azure Databricks kümenizde Python Kitaplığı'nı yükleyin

Yüklenecek [Python Kitaplığı](/azure/kusto/api/python/kusto-python-client-library) Azure Databricks kümenizdeki:

1. Azure Databricks çalışma alanınıza gidin ve [bir kitaplığı oluşturma](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)
2. [Bir Python Pypı paket ya da Python Yumurta karşıya yükleme](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg)
    - Karşıya yükleme, yükleyin ve kitaplığı, Databricks kümesine ekleyin.
    - Pypı adını girin: *azure kusto veri*

## <a name="connect-to-adx-using-device-login"></a>Cihaz oturum açma kullanarak ADX için Bağlan

[Bir not defteri alma](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) kullanarak [ADX cihaz oturumu sorgu](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb) bağlanmak için kimlik bilgilerinizi kullanarak ADX için Not Defteri.

## <a name="connect-to-adx-using-azure-ad-app"></a>Bağlanmak için Azure AD uygulaması kullanarak ADX

1. Azure AD uygulaması tarafından oluşturma [bir AAD uygulaması sağlama](/azure/kusto/management/access-control/how-to-provision-aad-app).
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

Uygulama kimliğini doğrulamak için Azure Veri Gezgini, Azure AD Kiracı kimliğinizi kullanır Kiracı kimliğinizi bulmak için aşağıdaki URL'yi kullanın ve *YourDomain* yerine kendi etki alanınızı yazın.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Örneğin, etki alanınız *contoso.com* olduğunda URL şöyle olur: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Sonuçları görmek için bu URL'ye tıklayın; ilk satır aşağıdaki gibidir. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Kiracınızın kimliği `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Store ve Azure AD uygulama kimliği ve anahtarı güvenliğini sağlama 

Store ve Azure AD uygulama kimliği ve anahtarı Azure Databricks kullanarak güvenli [gizli dizileri](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) gibi:
1. [CLI'yı ayarlama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli)
1. [CLI'yı yükleme](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli) 
1. [Kimlik doğrulamasını ayarlama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication).
1. Yapılandırma [gizli dizileri](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) kullanarak aşağıdaki örnek komutlar:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Bir not defteri alma
[Bir not defteri alma](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) kullanarak [sorgu ADX AAD uygulama](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb) ADX için bağlanmak için Not Defteri. Küme adı, veritabanı adı ve Azure AD Kiracı kimliği ile yer tutucu değerlerini güncelleştirin.