---
title: Azure Data Factory kullanarak birden çok kapsayıcılardan dosyaları kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak birden çok kapsayıcılardan dosyaları kopyalamak için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/1/2018
ms.openlocfilehash: a52729adf8d6df3f4e44e561b45b854db433628c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60635212"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Azure Data Factory ile birden çok kapsayıcı dosyaları Kopyala

Bu makalede dosya depolama alanları arasında birden çok kapsayıcı dosyaları kopyalamak için kullanabileceğiniz bir çözüm şablonu açıklanır. Örneğin, veri gölü, Azure Data Lake Store için AWS S3'ten geçirmek için kullanabilirsiniz. Veya bir Azure Blob Depolama hesabından diğerine her şeyi çoğaltmak için şablonu kullanabilirsiniz.

> [!NOTE]
> Tek bir kapsayıcıdan dosyaları kopyalamak isterseniz, kullanılacak daha verimli olur [kopyalama veri aracı](copy-data-tool.md) tek bir kopyalama etkinlikli bir işlem hattı oluşturursunuz. Bu makalede, basit bir senaryo için ihtiyacınız olandan daha fazla şablonudur.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, kapsayıcılar, kaynak depolama deposundan numaralandırır. Ardından bu kapsayıcıların hedef deposuna kopyalar.

Şablon üç etkinlikleri içerir:
- **GetMetadata** kaynak depolama deponuza tarar ve kapsayıcı listesini alır.
- **ForEach** kapsayıcı listeden alır **GetMetadata** etkinliği ve bir listesi yinelenir ve her kapsayıcı kopyalama etkinliğine geçirir.
- **Kopyalama** her kapsayıcı kaynak depolama deposundan hedef deposuna kopyalar.

Şablon iki parametre tanımlar:
- *SourceFilePath* nereden edinebileceğiniz kapsayıcıları listesini veri kaynağı deposu yolu. Çoğu durumda, birden çok kapsayıcı klasörleri içeren kök dizin yoludur. Bu parametrenin varsayılan değeri `/`.
- *DestinationFilePath* dosyaların kopyalanacağı için hedef deponuzda yoludur. Bu parametrenin varsayılan değeri `/`.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu kullanma

1. Git **birden çok dosya kapsayıcı dosya depoları arasında kopyalama** şablonu. Oluşturma bir **yeni** kaynak depolama deponuza bağlantı. Kaynak depolama birden çok kapsayıcılardan dosyalarını kopyalamak istediğiniz deposudur.

    ![Kaynak için yeni bir bağlantı oluşturun](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Oluşturma bir **yeni** hedef depolama deponuza bağlantı.

    ![Hedef için yeni bir bağlantı oluşturun](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Seçin **bu şablonu kullan**.

    ![Bu şablonu kullanın.](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Aşağıdaki örnekte olduğu gibi işlem hattını görürsünüz:

    ![İşlem hattı Göster](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Seçin **hata ayıklama**, girin **parametreleri**ve ardından **son**.

    ![İşlem hattını çalıştırma](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Sonucu gözden geçirin.

    ![Sonucu gözden geçir](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory ile denetim tablosunu kullanarak bir veritabanından toplu kopyalama](solution-template-bulk-copy-with-control-table.md)

- [Azure Data Factory ile birden çok kapsayıcı dosyaları Kopyala](solution-template-copy-files-multiple-containers.md)