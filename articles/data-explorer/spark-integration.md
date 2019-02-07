---
title: Apache Spark için Azure Veri Gezgini Bağlayıcısı
description: Bu makale için Apache Spark Azure Veri Gezgini bağlayıcıyı kullanmak üzere nasıl gösterir.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/05/2018
ms.openlocfilehash: 6f115df97d0b790c94d6dc07f3f40feeceb1a7cc
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824323"
---
# <a name="azure-data-explorer-connector-for-apache-spark"></a>Apache Spark için Azure Veri Gezgini Bağlayıcısı

Azure Veri Gezgini ve Apache Spark'ı kullanarak, machine learning (ML), Ayıkla-Dönüştür-yükle (ETL) ve Log Analytics gibi senaryoları verilerle hedefleyen hızlı ve ölçeklendirilebilir uygulamalar oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bağlayıcıyı kullanmak üzere uygulamanızın olması gerekir:

* 1.8 Java SDK'sı
* Maven 3.x
* Spark sürümü 2.3.2 veya daha yüksek

## <a name="link-to-data-explorer"></a>Veri Gezgini bağlantı

Scala ve Java uygulamalarını Maven kullanmak için tanımları proje, şu yapı ile uygulamanızı bağlayın:

```java
groupId = com.microsoft.azure
artifactId = spark-kusto-connector
version = 1.0.0-Beta-01 
```

Maven, aşağıdaki bağlantı:

```Maven
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-kusto-connector</artifactId>
    <version>1.0.0-Beta-01</version>
  </dependency>
```

## <a name="build-commands"></a>Yapı komutları

Jar oluşturmak ve tüm testleri çalıştırmak için:

```
mvn clean package
```

Jar oluşturmak için tüm test çalıştırması ve yerel Maven deponuza jar yükleyin:

```
mvn clean install
```