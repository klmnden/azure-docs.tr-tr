---
title: Komut satırı arabirimi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Yapılandırılmış verileri dizini ve dil bilgisi dosyalarından oluşturmak için komut satırı arabirimini kullanın ve bunları web Hizmetleri olarak dağıtabilirsiniz.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/24/2016
ms.author: paulhsu
ms.openlocfilehash: 018552982a8ece3bbbaea2d60e2a6e64f681f822
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60815145"
---
# <a name="command-line-interface"></a>Command Line Interface

Bilgi keşfetme hizmeti (KES) komut satırı arabirimi yapılandırılmış verileri dizini ve dil bilgisi dosyalarından oluşturmak ve bunları web Hizmetleri olarak dağıtma olanağı sağlar.  Genel söz dizimini kullanır: `kes.exe <command> <required_args> [<optional_args>]`.  Çalıştırabileceğiniz `kes.exe` komutları listesini görüntülemek için bağımsız değişkenler olmadan veya `kes.exe <command>` belirtilen komut için kullanılabilen bağımsız değişkenler listesini görüntüleyin.  Kullanılabilir komutların bir listesi aşağıdadır:

* build_index
* build_grammar
* host_service
* deploy_service
* describe_index
* describe_grammar

<a name="build_index-command"></a>

## <a name="buildindex-command"></a>build_index komutu

**Build_index** komut bir şema tanımı dosyası ve bir veri dosyasını dizine nesnelerin bir ikili dizin dosyası oluşturur.  Sonuçta elde edilen dizin dosyası, yapılandırılmış sorgu ifadeleri değerlendirmek için veya doğal dil sorguları ınterpretations derlenmiş dilbilgisi dosyasıyla birlikte oluşturmak için kullanılabilir.

`kes.exe build_index <schemaFile> <dataFile> <indexFile> [options]`

| Parametre      | Açıklama               |
|----------------|---------------------------|
| `<schemaFile>` | Giriş şemasını yolu |
| `<dataFile>`   | Giriş veri yolu   |
| `<indexFile>`  | Çıkış dizini yolu |
| `--description <description>` | Açıklama dizesi |
| `--remote <vmSize>`           | Uzak derleme için VM boyutu |

Bu dosyalar, yerel dosya yolu veya URL yolu Azure BLOB'ları için belirtilebilir.  Şema dosyası desteklenecek işlemlerinin yanı sıra dizinlenen nesneleri yapısını tanımlar (bkz [şema biçimi](SchemaFormat.md)).  Veri dosyasını dizine öznitelik değerleri ve nesneleri numaralandırır (bkz [veri biçimi](DataFormat.md)).  Derleme başarılı olduğunda, çıktı dizini dosyası istenen işlemleri destekleyen giriş verileri sıkıştırılmış bir gösterimini içerir.  

Bir açıklama dizesi kullanarak bir ikili dizin sonradan tanımlamak için isteğe bağlı olarak belirtilebilir **describe_index** komutu.  

Varsayılan olarak, dizin yerel makine üzerinde oluşturulmuştur.  Azure ortamının dışında en fazla 10.000 nesneler içeren veri dosyaları için yerel yapılar sınırlıdır.  Zaman--uzak bayrağı belirtilirse, belirtilen boyutta geçici olarak oluşturulan Azure VM üzerinde dizin oluşturulacak.  Bu, verimli bir şekilde daha fazla belleğe sahip Azure sanal makinelerini kullanarak oluşturulacak büyük dizinler sağlar.  Yapı işlemini yavaşlatır disk belleği önlemek için bir VM ile RAM miktarını 3 kez giriş verilerini dosya boyutu kullanmanızı öneririz.  Kullanılabilir VM boyutları listesi için bkz. [Sanal makinelerin boyutları](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

> [!TIP] 
> Daha hızlı derlemeler için olasılık azaltarak veri dosyasındaki nesneleri presort.

<a name="build_grammar-command"></a>

## <a name="buildgrammar-command"></a>build_grammar komutu

**Build_grammar** komut dilbilgisi XML içinde belirtilen bir ikili dilbilgisi dosyasına derler.  Sonuçta elde edilen gramatický soubor birlikte ınterpretations doğal dil sorguları oluşturmak için bir dizin dosyası ile kullanılabilir.

`kes.exe build_grammar <xmlFile> <grammarFile>`

| Parametre       | Açıklama               |
|-----------------|---------------------------|
| `<xmlFile>`     | Giriş XML dilbilgisi belirtimi yolu |
| `<grammarFile>` | Çıkış derlenmiş dilbilgisi yolu         |

Bu dosyalar, yerel dosya yolu veya URL yolu Azure BLOB'ları için belirtilebilir.  Dil bilgisi belirtimi ağırlıklı doğal dili ifadeleri ve bunların anlam ınterpretations kümesini açıklar (bkz [dil bilgisi biçimi](GrammarFormat.md)).  Derleme başarılı olduğunda, çıktı gramatický soubor nenalezen hızlı kod çözme etkinleştirmek için dil bilgisi belirtimi ikili gösterimini içerir.

<a name="host_service-command"/>

## <a name="hostservice-command"></a>host_service komutu

**Host_service** komutu, yerel makinede KES Hizmeti'nin bir örneğini barındıran.

`kes.exe host_service <grammarFile> <indexFile> [options]`

| Parametre       | Açıklama                |
|-----------------|----------------------------|
| `<grammarFile>` | Giriş ikili dilbilgisi yolu         |
| `<indexFile>`   | Giriş ikili dizin yolu           |
| `--port <port>` | Yerel bağlantı noktası numarası.  Varsayılan: 8000 |

Bu dosyalar, yerel dosya yolu veya URL yolu Azure BLOB'ları için belirtilebilir.  Bir web hizmeti, barındırılan http://localhost:&lt ; bağlantı noktası&gt; /.  Bkz: [Web API'leri](WebAPI.md) desteklenen işlemler listesi.

Azure dışında en çok 1 MB boyut, saniyede 10 istekleri ve toplam çağrı sayısı 1000 ortamı, yerel olarak barındırılan hizmetler, dizin sınırlıdır dosyaları.  Bu sınırlamaların üstesinden gelmek için çalıştırın **host_service** bir Azure VM içinde veya bir Azure bulut hizmetini kullanmayı dağıtma **deploy_service**.

<a name="deploy_service-command"/>

## <a name="deployservice-command"></a>deploy_service komutu

**Deploy_service** komutu, bir Azure bulut hizmetinde KES Hizmeti'nin bir örneğini dağıtır.

`kes.exe deploy_service <grammarFile> <indexFile> <serviceName> <vmSize>[options]`

| Parametre       | Açıklama                  |
|-----------------|------------------------------|
| `<grammarFile>` | Giriş ikili dilbilgisi yolu           |
| `<indexFile>`   | Giriş ikili dizin yolu             |
| `<serviceName>` | Hedef bulut hizmeti adı |
| `<vmSize>`      | Bulut hizmeti VM boyutu     |
| `--slot <slot>` | Bulut hizmeti yuvası: "Hazırlama" (varsayılan), "üretim" |

Bu dosyalar, yerel dosya yolu veya URL yolu Azure BLOB'ları için belirtilebilir.  Önceden yapılandırılmış Azure bulut hizmeti hizmet adını belirtir (bkz [bir bulut hizmeti oluşturma ve dağıtma konusunda](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md)).  Komut, belirtilen boyutta sanal makineleri kullanarak belirtilen Azure bulut hizmetine KES hizmeti otomatik olarak dağıtır.  Performansı önemli ölçüde azaltır, disk belleği önlemek için 1 GB ile kullanarak bir sanal giriş dizin dosya boyutundan daha fazla RAM öneririz.  Kullanılabilir VM boyutlarını gösteren liste için bkz: [Cloud Services boyutları](../../../articles/cloud-services/cloud-services-sizes-specs.md).

Varsayılan olarak, hizmet hazırlama ortamına dağıtıldıktan yuva parametresi aracılığıyla isteğe bağlı olarak geçersiz kılınmış.  Bkz: [Web API'leri](WebAPI.md) desteklenen işlemler listesi.

<a name="describe_index-command"/>

## <a name="describeindex-command"></a>describe_index komutu

**Describe_index** komut şema ve açıklama içeren bir dizin dosyası hakkında bilgi verir.

`kes.exe describe_index <indexFile>`

| Parametre     | Açıklama      |
|---------------|------------------|
| `<indexFile>` | Giriş dizin yolu |

Bu dosya yerel bir dosya yolu veya URL yolu Azure blob'a belirtilebilir.  Çıkış bir açıklama dizesi kullanılarak belirtilebilir. açıklama parametresi **build_index** komutu.

<a name="describe_grammar-command"/>

## <a name="describegrammar-command"></a>describe_grammar komutu

**Describe_grammar** komut ikili dil bilgisi oluşturmak için kullanılan özgün dil bilgisi belirtimi çıkarır.

`kes.exe describe_grammar <grammarFile>`

| Parametre       | Açıklama      |
|-----------------|------------------|
| `<grammarFile>` | Giriş dilbilgisi yolu |

Bu dosya yerel bir dosya yolu veya URL yolu Azure blob'a belirtilebilir.

