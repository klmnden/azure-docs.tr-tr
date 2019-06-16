---
title: Genel Bakış - Azure Batch işleme
description: Giriş işleme ve Azure Batch işleme özelliklerine genel bakış için Azure'ı kullanma
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: d4423b22c4c8afea5afa9c7040e081665b17ba87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60774038"
---
# <a name="rendering-using-azure"></a>Azure ile işleme

İşleme, 3B modelleri almak ve bunları 2B görüntülere dönüştürme işlemidir. 3B Sahne dosyaları yazılan uygulamalardaki gibi Autodesk 3ds Max, Autodesk Maya ve Blender'ı.  Autodesk Maya ve Autodesk Arnold, Chaos Group V-Ray ve Blender döngüleri gibi işleme uygulamalarını 2B görüntüleri oluşturur.  Bazen tek bir görüntü Sahne dosyaları oluşturulur. Ancak, model ve birden çok görüntülerin ve animasyon birleştirip sonra yaygındır.

İşleme iş yükü, medya ve eğlence sektöründe (VFX) özel efekt için yoğun olarak kullanılır. İşleme, reklam, perakende, Petrol ve gaz ve üretim gibi diğer birçok sektörle da kullanılıyor.

İşlem bakımından yoğun işleme işlemidir; üretmek için birçok çerçeve/görüntülerinden olabilir ve her bir görüntü oluşturmak için saatler sürebilir.  İşleme, bu nedenle birçok işleme paralel olarak çalıştırmak için Azure ve Azure Batch yararlanabilen bir mükemmel toplu işleme iş yüküne sahiptir.

## <a name="why-use-azure-for-rendering"></a>Neden Azure, işleme için kullanılır?

Çeşitli nedenlerle işleme mükemmel bir şekilde Azure ve Azure Batch için uygun bir iş yüküne sahiptir:

* İşleme işleri birden fazla sanal makine kullanarak işlemi paralel olarak çalıştırılabilir birçok parçalara ayrılabilir:
  * Birçok çerçeve animasyonları oluşur ve her karede paralel olarak işlenebilir.  Her kare, daha hızlı tüm çerçeveleri ve animasyon üretilen işlemine daha fazla VM.
  * Bazı işleme yazılımı, kutucukları veya dilimleri gibi birden çok parçalara bölünür tek bir çerçeve sağlar.  Her parça ayrı olarak işlenen sonra parçaların tamamladığınızda son görüntüye birleştirilmiş.  Daha fazla kullanılabilir VM'ler, daha hızlı bir çerçeve işlenebilir.
* Proje oluşturma, devasa ölçek gerektirebilir:
  * Tek tek çerçeveleri, karmaşık ve saatlerce, hatta Gelişmiş donanım üzerinde işlemek için gerekli; animasyonları binlerce çerçevelerinin yüzlerce oluşabilir.  İşlem büyük bir miktarını, yüksek kaliteli animasyonları makul bir süre içinde işlemek için gereklidir.  Bazı durumlarda, 100.000 çekirdek binlerce çerçevelerinin paralel işlemek için kullanılır.
* İşleme projeler, proje tabanlıdır ve değişen miktarda işlem gerektirir:
  * Gerekli olduğunda işlem ve depolama kapasitesini ayırmak, ölçeklendirme yukarı veya aşağı doğru göre yük bir proje ve proje bittiğinde kaldırın.
  * Ayrılmış kapasitesi için ödeme, ancak hiçbir yük projeler arasında olduğu gibi zaman için ödeme yapmayın.
  * Beklenmeyen değişiklikleri nedeniyle ani artışlar için değiştirebileceğiniz; beklenmeyen bir projedeki geç ve bu değişiklikler varsa daha yüksek ölçek, sıkı bir zamanlamaya göre işlenmesi gerekir.
* Uygulama, iş yükü ve zaman dilimi göre donanımı geniş arasından seçim yapabilirsiniz:
  * Donanım geniş ayrılmış ve yönetilen Azure Batch ile kullanılabilir.
  * Proje bağlı olarak, en iyi fiyat/performans veya en iyi genel performansı için gereksinim olabilir.  Farklı görünümler ve/veya işleme uygulamaları farklı bellek gereksinimleri vardır.  Bazı işleme uygulaması, en iyi performansı veya belirli özellikleri için GPU yararlanabilirsiniz. 
* Düşük öncelikli VM'ler, maliyetleri düşürün:
  * Düşük öncelikli VM'ler, normal isteğe bağlı Vm'lere göre büyük bir indirim için kullanılabilir ve bazı proje türleri için uygundur.
  * Düşük öncelikli VM'ler, bunlar çok sayıda gereksinimleri için gereksinimini karşılamak için nasıl kullanılacağını üzerinde esneklik Batch ile Azure Batch ile ayrılabilir.  Batch havuzları, VM türlerinin bir karışımını herhangi bir zamanda değiştirmek mümkün olan onunla hem adanmış hem de düşük öncelikli sanal makinelerin oluşabilir.

## <a name="options-for-rendering-on-azure"></a>Azure üzerinde işleme seçenekleri

İşleme iş yükleri için kullanılabilecek çeşitli Azure özellikleri vardır.  Hangi özellikleri kullanmak için tüm mevcut ortamlarına ve gereksinimlerine bağlıdır.

### <a name="existing-on-premises-rendering-environment-using-a-render-management-application"></a>Varolan bir işleme yönetim uygulaması kullanarak işleme ortamı şirket

Var olmasını grubu PipelineFX Qube, Royal işlemek veya Thinkbox son tarihi gibi işleme yönetim uygulaması tarafından yönetilen bir mevcut şirket içi işleme için en yaygın bir durumdur.  Şirket içi işleme grubunu kapasiteyi Azure Vm'leri genişletmek için zorunludur.

Kullanılabilir Azure destek ekleyen eklentiler vermiyoruz ya da işleme yönetim yazılımı ya da Azure desteği yerleşik olarak bulunur. Desteklenen hakkında daha fazla bilgi işlemek için yöneticileri ve etkin işlevselliği, makaleye bakın [kullanarak işleme yöneticileri](https://docs.microsoft.com/azure/batch/batch-rendering-render-managers).

### <a name="custom-rendering-workflow"></a>Özel işleme iş akışı

Sanal makineler için mevcut bir işleme çiftliği genişletmek zorunludur.  Azure Batch havuzları, VM, çok sayıda ayırabilirsiniz izin kullanılabilir ve dinamik olarak otomatik tam fiyatlı Vm'lerle ölçeklendirilmiş için düşük öncelikli VM'ler ve popüler işleme uygulamaları için Kullandıkça Öde lisansı sağlayın.

### <a name="no-existing-render-farm"></a>Var olan bir işleme grubunu

İstemci iş istasyonları, işleme gerçekleştiriyor olabileceğini, ancak işleme iş yükü artırır ve yalnızca iş istasyonu kapasite kullanmak için çok uzun sürüyor.  Azure Batch, hem tahsis işleme grubu işlem isteğe bağlı hem de Azure işleme çiftliği işleme işlerini zamanlamak için kullanılabilir.

## <a name="azure-batch-rendering-capabilities"></a>Azure Batch işleme özellikleri

Azure Batch, paralel iş yüklerini Azure'da çalışmasına izin verir.  Bu, oluşturulmasını ve yönetimini, çok sayıda uygulamaları yüklü ve çalıştıran sanal makineler sağlar.  Kapsamlı iş zamanlama sıraya alma VM'ler için uygulama izleme, görevlerin atanmasını sağlamak, bu uygulamaların örnekleri çalıştırmak ve benzeri için özellikleri de sağlar.

Azure Batch, birçok iş yükleri için kullanılır, ancak aşağıdaki özellikleri, özellikle daha kolay ve daha hızlı işleme iş yüklerini çalıştırmak yapmak kullanılabilir.

* Önceden yüklenmiş grafik ve işleme uygulamalarıyla VM görüntüleri:
  * Azure Market VM görüntülerini kullanılabilir içeren popüler grafik ve işleme uygulamaları, uygulamaları kendiniz yükleyin veya yüklü uygulamalar ile kendi özel görüntülerinizi oluşturmak için gereğinden kurtulursunuz. 
* Kullanım başına ödeme işleme uygulamaları için lisans:
  * Uygulamalar tarafından önler, bilgi işlem için sanal makineleri, ödeme ek dakika, lisans satın alın ve olabilecek uygulamalar için bir lisans sunucusu yapılandırmak zorunda için ödeme yapmayı seçebilirsiniz.  Kullanım için ödeme yapmak da değil sabit sayıda lisansları olduğu için değişen ve beklenmeyen yük gereksinimini karşılamak mümkün olduğunu gösterir.
  * Önceden yüklenmiş uygulamalara kendi lisansları olan ve olmayan lisans kullanım başına ödeme kullanmak mümkündür. Bunu yapmak için genellikle şirket içi veya Azure tabanlı yüklemeniz sunucu lisansı ve lisans sunucusuna işleme havuzu için bir Azure sanal ağı kullanın.
* Eklentileri istemci tasarım ve modelleme uygulamalarının için:
  * Eklentilere izin ver doğrudan istemci uygulamasından Azure Batch kullanmaya son kullanıcılar, Autodesk Maya havuzları oluşturmak bunları etkinleştirmek, iş göndermek ve yapmak gibi daha fazla kullanımını işlem kapasitesini daha hızlı işleme gerçekleştirmek için.
* Tümleşmesi işleme:
  * Azure Batch işleme Yönetim uygulamaları tümleştirilmiştir veya eklentiler, Azure Batch tümleştirmesi sağlamak kullanılabilir.

Her biri de Azure Batch işleme için geçerli Azure Batch'i nasıl kullanacağınızı birkaç yolu vardır.

* API'ler:
  * Kod kullanarak yazma [REST](https://docs.microsoft.com/rest/api/batchservice), [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/batch), [Python](https://docs.microsoft.com/python/api/overview/azure/batch), [Java](https://docs.microsoft.com/java/api/overview/azure/batch), veya desteklenen diğer API'ler.  Geliştiriciler tümleştirilebilir Azure Batch özelliklerine kendi mevcut uygulamalarınız veya iş akışı, ister bulutta veya şirket tabanlı.  Örneğin, [Autodesk Maya eklentisi](https://github.com/Azure/azure-batch-maya) oluşturma ve havuzları yönetme, işleri ve görevleri gönderme ve durumu izleme Batch çağırmak için Batch Python API kullanır.
* Komut satırı araçları:
  * [Azure komut satırı](https://docs.microsoft.com/cli/azure/) veya [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) betik Batch kullanmak için kullanılabilir.
  * Özellikle, Batch CLI şablon destek havuzları oluşturmak ve göndermek kolaylaşır.
* Kullanıcı arabirimleri:
  * [Batch Explorer](https://github.com/Azure/BatchExplorer) de Batch hesapları, izlenen ve yönetilecek sağlayan bir platformlar arası istemci araçtır, ancak Azure portalı kullanıcı arabirimini kıyasla bazı daha zengin özellikler sunar.  Havuz ve proje şablonları kümesi koşuluyla desteklenen her uygulama için uygun olan ve kolayca havuzları oluşturmak ve göndermek için kullanılabilir.
  * Azure portalında, yönetmek ve Azure Batch izlemek için kullanılabilir.
* İstemci uygulama eklentinin:
  * Eklentileri kullanılabilir doğrudan istemci tasarım içinde kullanılmak üzere Batch işleme ve modelleme uygulamalarının verin. Eklentileri, çoğunlukla geçerli 3B modeli hakkında bağlamsal bilgiler Batch Gezgini uygulamasıyla çağırın.
  * Aşağıdaki eklentileri kullanılabilir:
    * [Maya için Azure Batch](https://github.com/Azure/azure-batch-maya)
    * [3DS Max](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
    * [Blender'ı](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)

## <a name="getting-started-with-azure-batch-rendering"></a>Azure Batch işleme ile çalışmaya başlama

Azure Batch işleme denemek için aşağıdaki tanıtım öğreticilerine bakın:

* [Blender'ı bir Sahneyi işleme için Batch Gezgini'ni kullanma](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)
* [Autodesk 3ds Max sahnesini işlemek için Batch CLI kullanma](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)

## <a name="next-steps"></a>Sonraki adımlar

İşleme uygulamaları ve Azure Market VM görüntülerini dahil sürümleri listesini belirlemek [bu makalede](https://docs.microsoft.com/azure/batch/batch-rendering-applications).