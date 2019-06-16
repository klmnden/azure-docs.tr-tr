---
title: Azure Data Factory için şablonlarına genel bakış | Microsoft Docs
description: Azure Data factory'yi kullanmaya hızlıca başlamak için önceden tanımlanmış bir şablon kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/04/2019
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: 4bd38991b2452bdda65a7647f844dcc17fdfb125
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60786993"
---
# <a name="templates"></a>Şablonlar

Data Factory ile hızla çalışmaya başlamanızı sağlayan önceden tanımlanmış bir Azure Data Factory işlem hatlarını şablonlardır. Şablonları, Data Factory için yeni ve hızlı bir şekilde kullanmaya başlamak istediğinizde yararlıdır. Bu şablonlar, böylece iyileştirme Geliştirici üretkenliğini veri tümleştirme projeleri oluşturmak için geliştirme süresini azaltır.

## <a name="create-data-factory-pipelines-from-templates"></a>Data Factory işlem hatlarını şablonlardan oluşturma

Data Factory işlem hattı iki şekilde aşağıdaki şablonundan oluşturmaya başlayabilirsiniz:

1.  Seçin **şablondan işlem hattı Oluştur** Şablon Galerisi açmak için genel bakış sayfasında.

    ![Genel Bakış sayfasından şablon galerisinde Aç](media/solution-templates-introduction/templates-intro-image1.png)

1.  Yazar sekmesinde kaynak Gezgini'nde seçin **+** , ardından **şablon hattından** şablon galerisinde açın.

    ![Yazar sekmesinden şablon galerisinde Aç](media/solution-templates-introduction/templates-intro-image2.png)

## <a name="template-gallery"></a>Şablon Galerisi

![Şablon Galerisi](media/solution-templates-introduction/templates-intro-image3.png)

### <a name="out-of-the-box-data-factory-templates"></a>Dışında kutusu Data Factory şablonları

Veri fabrikası, veri fabrikası işlem hattı şablonları kaydetmek için Azure Resource Manager şablonları kullanır. İçin hazır Data Factory şablonlar dışında kullanılan bildirim dosyası ile birlikte tüm Resource Manager şablonları, gördüğünüz [resmi Azure Data Factory GitHub deposunda](https://github.com/Azure/Azure-DataFactory/tree/master/templates). Microsoft tarafından sağlanan önceden tanımlanmış şablonları içerir, ancak aşağıdaki öğeler için sınırlı değildir:

-   Şablonları kopyalayın:

    -   [Toplu kopyalama veritabanından](solution-template-bulk-copy-with-control-table.md)
    
    -   [Yeni dosyaları LastModifiedDate tarafından Kopyala](solution-template-copy-new-files-lastmodifieddate.md)

    -   [Birden çok dosya kapsayıcıları arasında dosya tabanlı depoları kopyalayın](solution-template-copy-files-multiple-containers.md)

    -   [Delta kopya veritabanından](solution-template-delta-copy-with-control-table.md)

    -   Kopyalama \<kaynak\> için \<hedef\>

        -   Azure Data Lake Store için Amazon S3'den 2 kuşağı

        -   Azure Data Lake Store için Google büyük sorgudan 2 kuşağı

        -   Azure Data Lake Store Gen 2 için HDF

        -   Azure Data Lake Store Gen 1 Netezza'dan

        -   Şirket içi Azure SQL veritabanı için SQL Server'dan

        -   Şirket içi Azure SQL veri ambarı SQL Server'dan

        -   Oracle'dan Azure SQL veri ambarı şirket

-   SSIS şablonları

    -   Azure-SSIS Integration Runtime'ı, SSIS paketlerini yürütmek için zamanlama

-   Şablonlarını Dönüştür

    -   [Azure Databricks ile ETL](solution-template-databricks-notebook.md)

### <a name="my-templates"></a>Şablonlarım

Seçerek bir şablon olarak bir işlem hattı kaydedebilirsiniz **şablon olarak Kaydet** işlem hattı sekmesinde.

![Bir işlem hattı şablon olarak Kaydet](media/solution-templates-introduction/templates-intro-image4.png)

Şablon olarak kaydedilen işlem hatları görüntüleyebileceğiniz **Şablonlarım** Şablon Galerisi bölümü. Bunları da görebilirsiniz **şablonları** kaynak Gezgini'nde bölümü.

![Şablonlarım](media/solution-templates-introduction/templates-intro-image5.png)

> [!NOTE]
> Şablonlarım bu özelliği kullanmak için GIT tümleştirmesi etkinleştirmek zorunda. Azure DevOps GIT ve GitHub hem desteklenir.
