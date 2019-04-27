---
title: İşleme uygulamaları - Azure Batch kullanın
description: Azure Batch işleme uygulamaları kullanma
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 4c93abdfb5c523d48ce115ed7d3251a346937f5f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60775398"
---
# <a name="rendering-applications"></a>Uygulamaları oluşturma

İşleme uygulamaları toplu işleri ve görevleri oluşturarak kullanılır. Görev komut satırı özelliği ve uygun komut satırı parametreleri belirtir.  Belirtilen Batch Gezgini şablonları kullanmaktır iş görevleri oluşturmak için en kolay yolu olan [bu makalede](https://docs.microsoft.com/azure/batch/batch-rendering-using#using-batch-explorer).  Şablonlar görüntülenebilir ve gerekirse sürümlerinde değiştirdi.

Bu makalede her işleme uygulamanın nasıl çalıştığını kısa bir açıklamasını sağlar.

## <a name="rendering-with-autodesk-3ds-max"></a>İşleme ile Autodesk 3ds Max

### <a name="renderer-support"></a>Oluşturucu desteği

3ds yerleşik işleyicileri yanı sıra en büyük aşağıdaki Oluşturucu işleme VM görüntüleri kullanılabilir ve 3ds Max sahnesini dosyası tarafından başvurulabilir:

* Autodesk Arnold
* Chaos Group V-Ray

### <a name="task-command-line"></a>Görevin komut satırı

Çağırma `3dsmaxcmdio.exe` uygulama havuzu düğümde komut satırı işleme gerçekleştirmek için.  Görev çalıştırıldığında bu uygulama yoludur. `3dsmaxcmdio.exe` Uygulama sahip aynı kullanılabilir parametreleri `3dsmaxcmd.exe` belgelenen uygulama [3ds Max Yardım belgeleri](https://help.autodesk.com/view/3DSMAX/2018/ENU/) (işleme | Komut satırı işleme bölümü).

Örneğin:

```
3dsmaxcmdio.exe -v:5 -rfw:0 -start:{0} -end:{0} -bitmapPath:"%AZ_BATCH_JOB_PREP_WORKING_DIR%\sceneassets\images" -outputName:dragon.jpg -w:1280 -h:720 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scenes\dragon.max"
```

Notlar:

* Varlık dosyaları bulunamadı emin olmak için çok dikkatli olunması gerekir.  Yolları olduğundan doğru ve göreli kullanarak olun **varlık izleme** penceresi ya da kullanım `-bitmapPath` komut satırında parametresi.
* Kontrol ederek, varlıkları bulmak için bağlanamama gibi işleme ile ilgili sorunlar olup olmadığını `stdout.txt` 3ds tarafından yazılan dosya en fazla görev çalıştırıldığında.

### <a name="batch-explorer-templates"></a>Batch Explorer şablonları

Havuzu ve işini şablonları erişilebilir **galeri** Batch Explorer.  Şablon kaynak dosyaları kullanılabilir [Batch Gezgini veri deposu github'da](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/3dsmax).

## <a name="rendering-with-autodesk-maya"></a>Autodesk Maya ile işleme

### <a name="renderer-support"></a>Oluşturucu desteği

Maya yerleşik işleyicileri, ek olarak aşağıdaki Oluşturucu işleme VM görüntüleri kullanılabilir ve 3ds Max sahnesini dosyası tarafından başvurulabilir:

* Autodesk Arnold
* Chaos Group V-Ray

### <a name="task-command-line"></a>Görevin komut satırı

`renderer.exe` Komut satırı Oluşturucu, görev komut satırında kullanılır. Komut satırı Oluşturucu belgelenen [Maya yardımcı](https://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=GUID-EB558BC0-5C2B-439C-9B00-F97BCB9688E4).

Aşağıdaki örnekte, iş hazırlama görevi Sahne dosyalarınız ve varlıklarınız iş hazırlığı çalışma dizinine kopyalamak için kullanılır ve bir çıkış klasörü işleme resminin depolamak için kullanılan çerçeve 10 işlenir.

```
render -renderer sw -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

V-Ray işleme için Maya Sahne dosyasını normalde V-Ray Oluşturucu olarak belirtmeniz gerekir.  Ayrıca komut satırında belirtilebilir:

```
render -renderer vray -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

Arnold için işleme Maya Sahne dosyasını normalde Arnold Oluşturucu olarak belirtmeniz gerekir.  Ayrıca komut satırında belirtilebilir:

```
render -renderer arnold -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

### <a name="batch-explorer-templates"></a>Batch Explorer şablonları

Havuzu ve işini şablonları erişilebilir **galeri** Batch Explorer.  Şablon kaynak dosyaları kullanılabilir [Batch Gezgini veri deposu github'da](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/maya).

## <a name="next-steps"></a>Sonraki adımlar

Havuzu ve işini şablonlardan kullanın [veri GitHub deposunda](https://github.com/Azure/BatchExplorer-data/tree/master/ncj) Batch Gezgini'ni kullanma.  Gerektiğinde, yeni şablonlar oluşturamaz veya sağlanan şablonlardan birini değiştirin.