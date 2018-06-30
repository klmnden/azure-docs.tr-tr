---
title: Azure Data Lake Storage Gen2 Önizleme URI kullanın
description: Azure Data Lake Storage Gen2 Önizleme URI kullanın
services: storage
keywords: ''
author: jamesbak
ms.topic: article
ms.author: jamesbak
manager: jahogg
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: a6130d8440b16e5a72c939fc07f6bf32c0946418
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114301"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Azure Data Lake Storage Gen2 kullanmak URI

[Hadoop dosya sistemi](http://www.aosabook.org/en/hdfs.html) Azure Data Lake Storage Gen2 Önizleme ile uyumlu sürücü düzeni tanımlayıcısıyla bilinen `abfs` (Azure Blob dosya sistemi). Diğer Hadoop dosya sistemi sürücüleri ile tutarlı ABFS sürücü adres dosyaları ve dizinleri bir Data Lake Storage Gen2 özellikli hesabı içinde URI biçimi kullanır.

## <a name="uri-syntax"></a>URI sözdizimi

Data Lake Storage Gen2 URI sözdizimi Data Lake Storage Gen2 varsayılan dosya sistemi olarak olacak şekilde depolama hesabınız olup olmadığına ayarlanmış üzerinde bağlıdır.

Data Lake Storage Gen2 özellikli hesabı adresine isterseniz **değil** URI sözdizimi toplu özelliktir sonra hesabı oluşturma sırasında varsayılan dosya sistemi olarak ayarlayın:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.widows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Düzen tanımlayıcısı**: `abfs` Protokolü düzeni tanımlayıcı olarak kullanılır. Güvenli Yuva Katmanı (SSL) bağlantısı olan veya olmayan bağlanma seçeneğine sahip. Kullanım `abfss` bir Güvenli Yuva Katmanı bağlantısı kurmak için.

2. **Dosya sistemi**: dosya ve klasörleri tutan üst konumu. Bu kapsayıcı Azure Storage Bloblarında hizmetinde aynıdır.

3. **Hesap adı**: oluşturma sırasında depolama hesabınıza verilen adı.

4. **Yollar**: ayrılmış bir eğik çizgi (`/`) dizin yapısını gösterimi.

5. **Dosya adı**: tek tek dosyasının adı. Bu parametre bir dizin adresleme, isteğe bağlıdır.

Ancak, adresine istediğiniz hesap hesabı oluşturma sırasında varsayılan dosya sistemi olarak ayarlanırsa, ardından URI sözdizimi toplu özelliktir:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Yol**: ayrılmış bir eğik çizgi (`/`) dizin yapısını gösterimi.

2. **Dosya adı**: tek tek dosyasının adı.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Hdınsight kümeleri ile Azure Data Lake Storage Gen2 kullanın](use-hdi-cluster.md)