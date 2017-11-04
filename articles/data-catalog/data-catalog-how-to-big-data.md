---
title: "'Büyük veri' veri kaynağı ile çalışma | Microsoft Docs"
description: "Nasıl yapılır makalesi Azure Blob Storage, Azure Data Lake ve Hadoop HDFS dahil 'büyük veri' veri kaynaklarıyla Azure veri Kataloğu'nu kullanarak desenleri vurgulama."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/15/2017
ms.author: maroche
ms.openlocfilehash: ce40c90af65a8872db61ee01f99200d75fc78053
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Büyük veri kaynaklarında Azure veri Kataloğu ile çalışmaya nasıl
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** bir kayıt ve sistemi kurumsal veri kaynakları için bulma görevi gören bir tam olarak yönetilen bir bulut hizmetidir. Tüm yardımcı kişilerle ilgili bulmak, anlamak ve daha fazla değer büyük veri dahil olmak üzere kendi mevcut veri kaynaklarından veri almak için veri kaynakları ve yardımcı kuruluşlar kullanın içindir.

**Azure veri Kataloğu** Azure blogu depolama BLOB'ları ve dizinleri yanı sıra Hadoop HDFS dosyaları ve dizinleri kaydını destekler. Bu veri kaynaklarının yarı yapılandırılmış yapısını büyük esneklik sağlar. Ancak, bunları kaydetme en çok değer almak için **Azure veri Kataloğu**, kullanıcıların veri kaynaklarını nasıl düzenlendiği düşünmeniz gerekir.

## <a name="directories-as-logical-data-sets"></a>Mantıksal veri kümeleri olarak dizinler
Büyük veri kaynakları düzenlemek için genel bir desen dizinleri mantıksal veri kümeleri davran sağlamaktır. Üst düzey dizinleri, bir veri kümesinin alt bölüm tanımlayın ve içerdikleri dosyalar verileri depolamak tanımlamak için kullanılır.

Bu desen örneği olabilir:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

Bu örnekte, mantıksal veri kümeleri vehicle_maintenance_events ve location_tracking_events temsil eder. Bu klasörlerinin her biri, ay ve yıl alt klasörler halinde düzenlenmiştir veri dosyalarını içerir. Bu klasörlerinin her biri, yüzlerce veya binlerce dosya büyük olasılıkla içerebilir.

Bu modelinde tek tek dosyalarıyla kaydetme **Azure veri Kataloğu** büyük olasılıkla mantıklı değildir. Bunun yerine, verileri ile çalışma kullanıcılara anlamlı veri kümeleri temsil dizinleri kaydedin.

## <a name="reference-data-files"></a>Başvuru veri dosyaları
Tek tek dosyalar olarak başvuru veri kümelerini depolamak için tamamlayıcı bir düzen yöneliktir. Bu veri kümeleri, büyük veri 'küçük' yan düşünülen ve genellikle bir analitik veri modelindeki boyutlara benzer olarak. Başvuru veri dosyaları için başka bir yerde büyük veri deposunda depolanan veri dosyaları toplu bağlamı sağlamak için kullanılan kayıtları içerir.

Bu desen örneği olabilir:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Bir analisti ya da veri Bilimcisi büyük dizin yapıları içinde yer alan verilerle çalışırken, bu başvuru dosyalardaki veriler yalnızca ada veya Kimliğe göre daha büyük veri kümesinde başvurulan varlıklar için daha ayrıntılı bilgi sağlamak için kullanılabilir.

Bu modelinde tek referans veri dosyalarıyla kaydetmek için mantıklıdır **Azure veri Kataloğu**. Her dosyanın bir veri kümesini temsil eder ve her bir açıklama ve tek tek bulunan.

## <a name="alternate-patterns"></a>Alternatif desenleri
Büyük veri deposu düzenlenebilir yalnızca iki olası yolları önceki bölümde açıklanan desenleri alır, ancak her farklı bir uygulamasıdır. Nasıl veri kaynaklarınızı, büyük veri kaynaklarıyla kaydedilirken yapılandırılmıştır bağımsız olarak **Azure veri Kataloğu**, odak dosyaları ve diğerlerinin içinde değerinin olan veri kümeleri temsil dizinleri kaydetmede, Kuruluş. Tüm dosyaları ve dizinleri kaydetme ne ihtiyaç duydukları bulmak kullanıcılar için daha zor hale getirme katalog daraltabilir.

## <a name="summary"></a>Özet
Veri kaynakları ile kaydetme **Azure veri Kataloğu** bulunmasını ve anlaşılmasını daha kolay hale getirir. Kaydetme ve büyük veri dosyaları ve mantıksal veri kümelerini temsil dizinleri yorumlama ihtiyaç duydukları büyük veri kaynaklarını bulabilir ve kullanıcıların yardımcı olur.
