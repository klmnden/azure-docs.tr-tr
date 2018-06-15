---
title: Azure içeri/dışarı aktarma aracı - v1 kullanarak | Microsoft Docs
description: İçeri/dışarı aktarma aracı sabit sürücüler için içeri aktarma işi hazırlama, içe aktarma işi onarmak veya bir dışarı aktarma işinin onarmak için nasıl kullanılacağını öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: f77535bb-d577-438a-bdd3-e15a82e0c543
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/15/2017
ms.author: muralikk
ms.openlocfilehash: 4ce2273cc0dcc456c2edc8c5dd2fc22496f20380
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23873691"
---
# <a name="using-the-azure-importexport-tool-classic-deployment-model"></a>Azure içeri/dışarı aktarma Aracı'nı (Klasik dağıtım modeli) kullanma

Azure içeri/dışarı aktarma Aracı'nı (WAImportExport.exe), böylece içine veya dışına Azure Blob Storage büyük miktarlarda veri aktarmak Azure içeri/dışarı aktarma hizmeti işleri oluşturmak ve yönetmek için kullanılır.

Bu belge Azure içeri/dışarı aktarma aracı Klasik dağıtım modeli için ' dir. Aracın en son sürümünü kullanma hakkında daha fazla bilgi için bkz: [Azure içeri/dışarı aktarma aracını kullanarak](../storage-import-export-tool-how-to.md).

Aşağıdaki makaleler şunları nasıl yapacağınızı için:

- Yükleyin ve içeri/dışarı aktarma aracı ayarlamak.
- Burada, sürücülerden verileri Azure Blob depolama alanına içeri bir iş için sabit sürücüler hazırlayın.
- Bir işin durumu ile kopyalama günlük dosyalarını inceleyin. 
- İçe aktarma işi onarın. 
- Bir dışarı aktarma işinin onarın. 
- İşlemi sırasında bir sorun oluştu durumunda Azure içeri/dışarı aktarma aracı sorunlarını giderin. 

## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport Aracı'nı ayarlama](../storage-import-export-tool-how-to.md)