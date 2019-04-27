---
title: Özellikler - Azure Batch işleme
description: Azure Batch işleme özellikleri kullanma
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 2dff44f0b5b4b02c39c4c63f23ff64d55ca9d833
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337616"
---
# <a name="using-azure-batch-rendering"></a>Azure Batch işleme kullanma

Azure Batch işleme kullanmak için birkaç yol vardır:

* API'ler:
  * Batch API'lerini kullanarak kod yazın.  Geliştiriciler tümleştirilebilir Azure Batch özelliklerine kendi mevcut uygulamalarınız veya iş akışı, ister bulutta veya şirket tabanlı.
* Komut satırı araçları:
  * [Azure komut satırı](https://docs.microsoft.com/cli/azure/) veya [PowerShell](https://docs.microsoft.com/powershell/azure/overview) betik Batch kullanmak için kullanılabilir.
  * Özellikle, [Batch CLI şablon Destek](https://docs.microsoft.com/azure/batch/batch-cli-templates) havuzları oluşturmak ve göndermek çok daha kolay hale getirir.
* Batch Gezgini kullanıcı Arabirimi:
  * [Batch Explorer](https://github.com/Azure/BatchLabs) de Batch hesapları, izlenen ve yönetilecek sağlayan bir platformlar arası istemci aracıdır.
  * Kolayca havuzları oluşturmak ve göndermek için kullanılabilir her işleme uygulamaları için bir havuzu ve işini şablon sayısı sağlanır.  Bir dizi şablon Github'dan erişilen şablon dosyaları ile kullanıcı Arabirimi, uygulama içinde listelenir.
  * Özel şablonlar sıfırdan yazılabilir veya sağlanan şablonlar github'dan kopyaladığınız ve değiştirdi.
* İstemci uygulama eklentiler:
  * Eklentileri kullanılabilir doğrudan istemci tasarım içinde kullanılmak üzere Batch işleme ve modelleme uygulamalarının verin.  Eklentileri, çoğunlukla geçerli 3B modeli hakkında bağlamsal bilgiler Batch Gezgini uygulamasıyla çağırın ve varlıklarını yönetmenize yardımcı olmak için özellikler içerir.

En iyi yolu Azure Batch işleme deneyin ve geliştiriciler ve Azure uzmanlarından olmayan, son kullanıcılar için en basit yolu Batch Gezgini uygulama ya da doğrudan kullanmak veya bir istemci uygulamasından eklenti çağrılır.

## <a name="using-batch-explorer"></a>Batch Gezgini'ni kullanma

Batch Explorer'ı kullanarak işleme bakın gerçekleştirmeyi için adım adım bir öğretici için [Blender öğretici](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender).

### <a name="download-and-install"></a>İndirme ve yükleme

Batch Explorer [indirmeler kullanılabilir](https://azure.github.io/BatchExplorer/) Windows, OSX ve Linux için.

### <a name="using-templates-to-create-pools-and-run-jobs"></a>Şablonları kullanarak havuzları oluşturma ve işleri çalıştırma

Kapsamlı bir dizi havuzlar oluşturma ve havuzları oluşturmak için gereken tüm özellikleri belirtmek zorunda kalmadan çeşitli işleme uygulamaları için iş, işler ve görevler ile doğrudan göndermek kolaylaştıran bir Batch Gezgini ile kullanılabilir Batch.  Batch Explorer şablanların depolanan ve görünür [bir GitHub deposuna](https://github.com/Azure/BatchExplorer-data/tree/master/ncj).

![Batch Explorer Galerisi](./media/batch-rendering-using/batch-explorer-gallery.png)

Şablonlar, tüm Market işleme VM görüntüleri uygulamalar sunmak için değiştirebileceğiniz sağlanır.  Her uygulama için birden fazla şablon, CPU ve GPU havuzları, Windows ve Linux için havuzları gereksinimini karşılamak için havuzu şablonları dahil olmak üzere mevcut; Proje şablonları, tam çerçeve dahil etmek veya Blender'ı oluşturma ve V-Ray dağıtılan işleme döşeli. Sağlanan şablonları kümesini havuzu otomatik ölçeklendirme gibi diğer Batch özellikleri için gereksinimini karşılamak için zaman içinde genişletilir.

Sıfırdan veya sağlanan şablonları değiştirerek üretilmesi özel şablonlar için da mümkündür. Batch Explorer'ın 'Galeri' bölümünde 'Yerel Şablonları' öğesi'i seçerek özel şablonlar kullanılabilir.

### <a name="file-system-and-data-movement"></a>Dosya sistemi ve veri taşıma

Batch Gezgini 'Data' bölümünde, bir yerel dosya sistemi ve Azure depolama hesapları arasında kopyalanacak dosyaları sağlar.

## <a name="client-application-plug-ins"></a>İstemci uygulama eklentileri

Eklentiler bazı istemci uygulamaları için kullanılabilir.  Eklentilere izin ver havuzlar ve işler doğrudan uygulamadan oluşturulması veya Batch Gezgini çağırmak için.

* [Blender'ı](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)
* [Autodesk 3ds Max](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
* [Autodesk Maya](https://github.com/Azure/azure-batch-maya)

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme deneme iki öğreticiler, örnekler için:

* [Azure CLI kullanarak oluşturma](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Batch Explorer'ı kullanarak oluşturma](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)