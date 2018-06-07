---
title: Azure bulut Kabuğu'ndan Databricks CLI kullanın | Microsoft Docs
description: Azure bulut Kabuğu'ndan Databricks CLI kullanmayı öğrenin.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: c20ad02f962fbee22bb16653c5eab351d9f3de17
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598734"
---
# <a name="use-databricks-cli-from-azure-cloud-shell"></a>Azure Cloud Shell’den Databricks CLI kullanma

Azure bulut Kabuğu'ndan Databricks CLI Databricks üzerinde işlem gerçekleştirmek için nasıl kullanılacağını öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Databricks çalışma ve küme. Yönergeler için bkz: [Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md). 

* Kişisel erişim belirteci Databricks olarak ayarlayın. Yönergeler için bkz: [belirteci Yönetim](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="use-the-azure-cloud-shell"></a>Azure bulut kabuğunu kullanın

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
 
2. Sağ üst köşesinden tıklatın **bulut Kabuk** simgesi.

   ![Bulut Kabuk başlatma](./media/databricks-cli-from-azure-cloud-shell/launch-azure-cloud-shell.png "Excel ODBC'den başlatma")

3. Seçtiğinizden emin olun **Bash** bulut Kabuk enviornment için. Aşağıdaki ekran görüntüsünde gösterildiği gibi açılır seçeneğinden seçebilirsiniz.

   ![Bulut Kabuk başlatma](./media/databricks-cli-from-azure-cloud-shell/select-bash-for-shell.png "Excel ODBC'den başlatma") 

4. Databtricks CLI yükleyebilmek için bir sanal ortamı oluşturun. Adlı bir sanal ortam oluşturacak parçacığında `databrickscli`.

       virtualenv -p /usr/bin/python2.7 databrickscli

5. Oluşturduğunuz sanal bir ortama geçin.

       source databrickscli/bin/activate

6. Databricks CLI yükleyin.

       pip install databricks-cli

7. Databricks ile kimlik doğrulaması, oluşturmuş olmanız gerekir, erişim belirteci kullanarak Önkoşullar bir parçası olarak ayarlayın. Aşağıdaki komutu kullanın:

       databricks configure --token

    Aşağıdaki sorular alırsınız:

    * Databricks konak girmeniz istenir. Telefo biçimindeki değerini `https://eastus2.azuredatabricks.net`. Burada, **Doğu ABD 2** Azure Databricks çalışma alanınızı oluşturulduğu Azure bölgesi.

    * Bir kullanıcı adı girmeniz istenir. Girin **belirteci**.

    * Son olarak, parolayı girmeniz istenir. Daha önce oluşturduğunuz belirteci girin.

Bu adımları tamamladıktan sonra Azure bulut Kabuğu'ndan Databricks CLI kullanmaya başlayabilirsiniz.

## <a name="use-databricks-cli"></a>Databricks CLI kullanın

Şimdi Databricks CLI kullanarak da başlatabilirsiniz. Örneğin, çalışma alanınızda sahip tüm Databricks kümeleri listelemek için aşağıdaki komutu çalıştırın.

    databricks clusters list

Aşağıdaki komut, Databricks dosya sistemi (DBFS) erişmek için de kullanabilirsiniz.

    databricks fs ls


Komutlar hakkında tam başvuru için bkz: [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).


## <a name="next-steps"></a>Sonraki adımlar

* Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI genel bakış](../cloud-shell/overview.md)
* Azure CLI komutlarının bir listesini görmek için bkz: [Azure CLI başvurusu](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
* Databricks CLI komutlarının bir listesini görmek için bkz: [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html)


