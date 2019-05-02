---
title: Azure Data Lake depolama Gen2'ye kullanmak URI'si
description: Azure Data Lake depolama Gen2'ye kullanmak URI'si
services: storage
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: 3f486da121927be23a6bd86e8567574cd95c541e
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939279"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Azure Data Lake depolama Gen2'ye kullanmak URI'si

[Hadoop dosya sistemi](https://www.aosabook.org/en/hdfs.html) Azure Data Lake depolama Gen2'ile uyumlu sürücü, düzen tanımlayıcısı tarafından bilinen `abfs` (Azure Blob dosya sistemi). Diğer Hadoop dosya sistemi sürücüleri ile tutarlı ABFS sürücü adresi dosyalara ve dizinlere bir Data Lake depolama Gen2 özellikli hesabında bir URI biçimi kullanır.

## <a name="uri-syntax"></a>URI söz dizimi

Data Lake depolama Gen2 varsayılan dosya sistemi olarak sağlamak için depolama hesabınızın olup olmadığını kümesi URI'si söz dizimi için Data Lake depolama Gen2'ye bağlıdır.

Data Lake depolama Gen2 özellikli hesabı adresine isterseniz **değil** URI'si söz dizimi toplu sonra hesap oluşturma sırasında varsayılan dosya sistemi olarak ayarlayın:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Düzen tanımlayıcısı**: `abfs` Protokolü, düzen tanımlayıcısı kullanılır. İle veya bir Güvenli Yuva Katmanı (SSL) bağlantısı olmadan bağlanma seçeneği var. Kullanım `abfss` bir Güvenli Yuva Katmanı bağlantısı kurmak için.

2. **Dosya sistemi**: Dosya ve klasörleri tutar üst konum. Bu kapsayıcılar Azure depolama BLOB'ları hizmetinde aynıdır.

3. **Hesap adı**: Oluşturma sırasında depolama hesabınıza verilen adı.

4. **Yolları**: Bir eğik ayrılmış (`/`) dizin yapısı temsili.

5. **Dosya adı**: Tek tek dosya adı. Bu parametre bir dizin ele alır, isteğe bağlıdır.

Ancak, adresine istediğiniz hesabın hesap oluşturma sırasında varsayılan dosya sistemi olarak ayarlanırsa, ardından toplu URI'si söz dizimi şöyledir:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Yol**: Bir eğik ayrılmış (`/`) dizin yapısı temsili.

2. **Dosya adı**: Tek tek dosya adı.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
