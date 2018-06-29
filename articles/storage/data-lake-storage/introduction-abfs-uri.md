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
ms.openlocfilehash: 75edf6dc7382a8a2ece7c25edd09aeacfe1c5189
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060068"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Azure Data Lake Storage Gen2 kullanmak URI

[Hadoop dosya sistemi](http://www.aosabook.org/en/hdfs.html) Azure Data Lake Storage Gen2 Önizleme ile uyumlu sürücü düzeni tanımlayıcısıyla bilinen `abfs` (Azure Blob dosya sistemi). Diğer Hadoop dosya sistemi sürücüleri ile tutarlı ABFS sürücü adres dosyaları ve dizinleri bir Data Lake Storage Gen2 özellikli hesabı içinde URI biçimi kullanır.

## <a name="uri-syntax"></a>URI sözdizimi

Data Lake Storage Gen2 URI sözdizimi Data Lake Storage Gen2 varsayılan dosya sistemi olarak olacak şekilde depolama hesabınız olup olmadığına ayarlanmış üzerinde bağlıdır.

Adresine istediğiniz Data Lake Storage Gen2 özellikli hesabı hesabı oluşturma sırasında varsayılan dosya sistemi olarak ayarlanır, toplu URI sözdizimi şöyledir:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Yol**: ayrılmış bir eğik çizgi (`/`) dizin yapısını gösterimi.

2. **Dosya adı**: tek tek dosyasının adı.

Data Lake Storage Gen2 özellikli hesabı adresine isterseniz *değil* varsayılan dosya sistemi URI sözdizimi aşağıdaki gibidir:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.widows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Düzen tanımlayıcısı**: `abfs` Protokolü düzeni tanımlayıcı olarak kullanılır. Güvenli Yuva Katmanı (SSL) bağlantısı olan veya olmayan bağlanma seçeneğine sahip. Kullanım `abfss` bir Güvenli Yuva Katmanı bağlantısı kurmak için.

2. **Dosya sistemi**: dosya ve klasörleri tutan üst konumu. Bu kapsayıcı Azure Storage Bloblarında hizmetinde aynıdır.

3. **Hesap adı**: oluşturma sırasında depolama hesabınıza verilen adı.

4. **Yollar**: ayrılmış bir eğik çizgi (`/`) dizin yapısını gösterimi.

5. **Dosya adı**: tek tek dosyasının adı. Bu parametre bir dizin adresleme, isteğe bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Hdınsight kümeleri ile Azure Data Lake Storage Gen2 kullanın](use-hdi-cluster.md)