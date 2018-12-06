---
title: Azure Data Lake depolama Gen2 önizlemesini URI kullanma
description: Azure Data Lake depolama Gen2 önizlemesini URI kullanma
services: storage
author: jamesbak
ms.topic: conceptual
ms.author: jamesbak
ms.date: 12/06/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: e596123cb218a542166d80b53916a73034f71760
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975259"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Azure Data Lake depolama Gen2'ye kullanmak URI'si

[Hadoop dosya sistemi](http://www.aosabook.org/en/hdfs.html) Azure Data Lake depolama Gen2 önizlemesi ile uyumlu sürücü, düzen tanımlayıcısı tarafından bilinen `abfs` (Azure Blob dosya sistemi). Diğer Hadoop dosya sistemi sürücüleri ile tutarlı ABFS sürücü adresi dosyalara ve dizinlere bir Data Lake depolama Gen2 özellikli hesabında bir URI biçimi kullanır.

## <a name="uri-syntax"></a>URI söz dizimi

Data Lake depolama Gen2 varsayılan dosya sistemi olarak sağlamak için depolama hesabınızın olup olmadığını kümesi URI'si söz dizimi için Data Lake depolama Gen2'ye bağlıdır.

Data Lake depolama Gen2 özellikli hesabı adresine isterseniz **değil** URI'si söz dizimi toplu sonra hesap oluşturma sırasında varsayılan dosya sistemi olarak ayarlayın:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Düzen tanımlayıcısı**: `abfs` protokolü, düzen tanımlayıcısı kullanılır. İle veya bir Güvenli Yuva Katmanı (SSL) bağlantısı olmadan bağlanma seçeneği var. Kullanım `abfss` bir Güvenli Yuva Katmanı bağlantısı kurmak için.

2. **Dosya sistemi**: dosya ve klasörleri tutan üst konumu. Bu kapsayıcılar Azure depolama BLOB'ları hizmetinde aynıdır.

3. **Hesap adı**: oluşturma sırasında depolama hesabınıza verilen ad.

4. **Yolları**: bir eğik ayrılmış (`/`) dizin yapısı temsili.

5. **Dosya adı**: bireysel dosyasının adı. Bu parametre bir dizin ele alır, isteğe bağlıdır.

Ancak, adresine istediğiniz hesabın hesap oluşturma sırasında varsayılan dosya sistemi olarak ayarlanırsa, ardından toplu URI'si söz dizimi şöyledir:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Yol**: bir eğik ayrılmış (`/`) dizin yapısı temsili.

2. **Dosya adı**: bireysel dosyasının adı.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](data-lake-storage-use-hdi-cluster.md)
