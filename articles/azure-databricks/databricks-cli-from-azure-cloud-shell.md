---
title: 'Azure Cloud Shell’den Databricks CLI kullanma '
description: Azure Cloud shell'den Databricks CLI'yı kullanmayı öğrenin.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: mamccrea
ms.openlocfilehash: dae481fb477223f149404c6a09cad024bc15cd90
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62108746"
---
# <a name="use-databricks-cli-from-azure-cloud-shell"></a>Azure Cloud Shell’den Databricks CLI kullanma

Databricks üzerinde işlem gerçekleştirmek için Azure Cloud shell'den Databricks CLI'yı kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Azure Databricks çalışma alanı ve küme. Yönergeler için [Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md). 

* Databricks içinde bir kişisel erişim belirteci ayarlayın. Yönergeler için [belirteç Yönetim](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="use-the-azure-cloud-shell"></a>Azure Cloud Shell kullanma

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
 
2. Sağ üst köşesdeki **Cloud Shell** simgesi.

   ![Cloud Shell'i başlatmak](./media/databricks-cli-from-azure-cloud-shell/launch-azure-cloud-shell.png "Azure Cloud Shell'i Başlat")

3. Seçtiğinizden emin olun **Bash** Cloud Shell ortamını için. Aşağıdaki ekran görüntüsünde gösterildiği gibi açılan seçeneğinden seçebilirsiniz.

   ![Bash Cloud Shell ortamını seçin](./media/databricks-cli-from-azure-cloud-shell/select-bash-for-shell.png "Bash seçin") 

4. Databricks CLI yükleyebilmek için bir sanal ortamı oluşturun. Aşağıdaki kod parçacığında, adlı bir sanal ortam oluşturma `databrickscli`.

       virtualenv -p /usr/bin/python2.7 databrickscli

5. Oluşturduğunuz sanal bir ortama geçiş yapın.

       source databrickscli/bin/activate

6. Databricks CLI'yı yükleyin.

       pip install databricks-cli

7. Databricks ile kimlik doğrulaması, oluşturmuş olmanız gerekir, erişim belirtecini kullanarak listelenen ön koşulların bir parçası ayarlayın. Aşağıdaki komutu kullanın:

       databricks configure --token

    Aşağıdaki komut istemleri almayacaklar:

    * İlk olarak, Databricks konak girmeniz istenir. Değer şu biçimde girin `https://eastus2.azuredatabricks.net`. Burada, **Doğu ABD 2** Azure Databricks çalışma alanınızda oluşturduğunuz Azure bölgesi.

    * Ardından, bir belirteç girmeniz istenir. Daha önce oluşturduğunuz belirteci girin.

Bu adımları tamamladıktan sonra Azure Cloud shell'den Databricks CLI kullanmaya başlayabilirsiniz.

## <a name="use-databricks-cli"></a>Databricks CLI kullanma

Artık, Databricks CLI'yı kullanarak başlayabilirsiniz. Örneğin, çalışma alanınızda sahip olduğunuz tüm Databricks kümelerini listelemek için aşağıdaki komutu çalıştırın.

    databricks clusters list

Ayrıca, Databricks dosya sistemine (DBFS) erişmek için aşağıdaki komutu kullanabilirsiniz.

    databricks fs ls


Açma komutlarının tam başvuru için bkz: [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).


## <a name="next-steps"></a>Sonraki adımlar

* Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI'yı genel bakış](../cloud-shell/overview.md)
* İçin Azure CLI komutları listesini görmek için bkz: [Azure CLI başvurusu](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
* İçin Databricks CLI komutları listesini görmek için bkz: [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html)


