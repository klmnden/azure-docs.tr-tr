---
title: Batch AI kaynak - Azure hakkında | Microsoft Docs
description: Çalışma alanları, kümeleri, dosya sunucuları, denemeler ve Microsoft Azure Batch AI hizmeti işlerinde genel bakış.
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 4a9e3529f9d68ecdc614ea69cffc6897891f4548
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057286"
---
# <a name="overview-of-resources-in-batch-ai"></a>Batch AI kaynakların genel bakış

Batch AI hizmeti kullanılarak ilk kez başlattığınızda kullanılabilir Batch AI kaynak öğrenmek istersiniz. Diğer Azure Hizmetleri ile bir veya daha fazla Azure'da Batch AI kaynak oluşturmak gibi *kaynak grupları*. Bir veya daha fazla Batch AI oluşturma *çalışma alanları* bir kaynak grubunda. Her bir çalışma alanı Batch AI bir karışımını içeren *kümeleri*, *dosya sunucuları*, ve *denemeleri*. Bir Batch AI deneme kapsayan bir grup *işleri*.

Aşağıdaki görüntüde bir örnek kaynak hiyerarşisi için Batch AI gösterilmektedir. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

Aşağıdaki bölümlerde, Batch AI kaynaklar hakkında daha fazla ayrıntıya gidin.

## <a name="workspace"></a>Çalışma alanı

Bir çalışma alanında, Batch AI, Batch AI kaynak rest en üst düzey bir koleksiyonudur. Çalışma alanları yardımcı iş ayırmak için farklı gruplara veya projelere ait. Örneğin, bir geliştirme ve test çalışma alanı oluşturabilirsiniz.

## <a name="cluster"></a>Küme

Batch AI kümesinde işleri çalıştırmak için işlem kaynaklarını içerir. Bir kümedeki tüm düğümlerin aynı VM boyutu ve işletim sistemi görüntüsü vardır. Batch AI kümeleri farklı ihtiyaçları için özelleştirilmiş oluşturmak için birçok seçenek sunar. Genellikle, bir projeyi tamamlamak için gereken power işleme her kategori için farklı bir kümeye ayarlarsınız. Yukarı ve aşağı Batch AI kümeleri ölçek, isteğe bağlı ve bütçe göre. Daha fazla bilgi için [Batch AI kümeleri ile çalışma](clusters.md).

## <a name="file-server"></a>Dosya sunucusu

İsteğe bağlı olarak betikler, eğitim verileri depolamak ve çıkış günlüklerini için Batch AI bir dosya sunucusu oluşturabilir. Bir Batch AI dosya sunucusu küme düğümlerine işleri için bir kolayca ve merkezi olarak erişilebilir depolama konumu sağlamak için otomatik olarak bağlanabilir bir yönetilen tek düğümlü NFS ' dir. Çoğu durumda, yalnızca bir dosya sunucusu bir çalışma alanında gereklidir ve farklı dizinlere, eğitim işleri için verileri ayırabilirsiniz. NFS iş yükleriniz için uygun değilse, Batch AI dahil olmak üzere diğer depolama seçeneklerini destekler. [Azure depolama](use-azure-storage.md) veya Gluster veya Lustre bir dosya sistemi gibi özel çözümler.

## <a name="experiment"></a>Deneme

Bir deney, sorgulama ve yönetme birlikte ilgili işleri koleksiyonunu gruplandırır. Her bir çalışma alanı, her deneme belirli bir sorunu çözmek girişimde bulunduğu birden çok deneme olabilir.

## <a name="job"></a>İş

Bir işi, tek bir görev veya örneğin bir derin öğrenme modeli eğitmek için yürütülmesi gereken betik silinir. Her proje, çalışma alanında bir kümede belirli bir betik yürütür. (Betik Batch AI dosya sunucusu veya başka bir depolama çözümü üzerinde depolanabilir.) Her Batch AI işi ile ilişkili bir framework türü vardır: TensorFlow, Horovod, CNTK, Caffe, Caffe2, pyTorch, bağlayıcı, özel MPI veya özel. Her bir çerçeve için Batch AI hizmeti, gerekli altyapı Kurulumu ayarlar ve iş süreçlerini yönetir. Her deneme farklı parametreler için birkaç değişiklik dışında benzerdir birden çok iş olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* İlk çalıştırma [Batch AI eğitim işini](quickstart-tensorflow-training-cli.md).

* Farklı çerçeveler için örnek [eğitim tariflerine](https://github.com/Azure/BatchAI/tree/master/recipes) göz atın.