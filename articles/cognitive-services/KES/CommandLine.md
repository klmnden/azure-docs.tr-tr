---
title: Bilgi Bankası araştırması hizmet komut satırı arabirimi | Microsoft Docs
description: Yapılandırılmış verileri dizin ve dilbilgisi dosyaları oluşturmak için KES komut satırı arabirimini kullanın ve bunları Microsoft Bilişsel hizmetler web Hizmetleri olarak dağıtabilirsiniz.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/24/2016
ms.author: paulhsu
ms.openlocfilehash: ffa42ac73b42a8271004d2d45d7a80f3307ef059
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351802"
---
# <a name="command-line-interface"></a>Command Line Interface
KES komut satırı arabirimi yapılandırılmış verilerden dizin ve dilbilgisi dosyalarını oluşturmak ve bunları web Hizmetleri olarak dağıtma olanağı sağlar.  Genel söz dizimini kullanır: `kes.exe <command> <required_args> [<optional_args>]`.  Çalıştırabilirsiniz `kes.exe` komutlarının listesini görüntülemek için bağımsız değişkenler olmadan veya `kes.exe <command>` belirtilen komut için bağımsız değişkenlerin listesini görüntülemek için.  Kullanılabilir komutlar listesi aşağıdadır:
* build_index
* build_grammar
* host_service
* deploy_service
* describe_index
* describe_grammar

<a name="build_index-command"></a>
## <a name="buildindex-command"></a>build_index komutu
**Build_index** komutu bir şema tanımı dosyası ve bir veri dosyası dizine nesnelerin ikili dizin dosyası oluşturur.  Sonuçta elde edilen dizin dosyası yapılandırılmış sorgu ifadeleri değerlendirmek veya doğal dil sorguları yorumlar derlenmiş dilbilgisi dosyasıyla birlikte oluşturmak için kullanılabilir.

`kes.exe build_index <schemaFile> <dataFile> <indexFile> [options]`

| Parametre      | Açıklama               |
|----------------|---------------------------|
| `<schemaFile>` | Giriş şemasını yolu |
| `<dataFile>`   | Giriş veri yolu   |
| `<indexFile>`  | Çıktı dizini yolu |
| `--description <description>` | Açıklama dizesi |
| `--remote <vmSize>`           | VM boyutu uzak yapı için |

Bu dosyalar, yerel dosya yolları veya Azure BLOB'ları için URL yolları tarafından belirtilebilir.  Şema dosyası desteklenmesi işlemlerinin yanı sıra dizinlenen nesnelerin yapısı açıklar (bkz [şema biçimi](SchemaFormat.md)).  Veri dosyası nesneleri ve dizine öznitelik değerlerini numaralandırır (bkz [veri biçimi](DataFormat.md)).  Yapı başarılı olduğunda, istenen işlemlerini destekleyen giriş verileri sıkıştırılmış bir gösterimini çıkış dizin dosyası içerir.  

Açıklama dizesi kullanarak bir ikili dizin sonradan tanımlamak için isteğe bağlı olarak belirtilebilir **describe_index** komutu.  

Varsayılan olarak, yerel makinede dizin oluşturulur.  Azure ortamı dışındaki yerel yapılar 10.000 nesneler içeren veri dosyalarını sınırlıdır.  Zaman uzaktan bayrağı belirtilmediyse, bir geçici olarak oluşturulan Azure VM belirtilen boyuttaki dizin oluşturulacak.  Bu, daha fazla belleğe sahip Azure sanal makineleri verimli şekilde kullanma oluşturulacak büyük dizinler sağlar.  Derleme işlemini yavaşlatır disk belleği önlemek için bir VM ile RAM miktarını 3 kez giriş veri dosyası boyutu kullanmanızı öneririz.  Kullanılabilir VM boyutları listesi için bkz: [sanal makineler için Boyutlar](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

> [!TIP] 
> Daha hızlı derlemeler için olasılık azaltarak veri dosyasındaki nesneleri presort.

<a name="build_grammar-command"></a>
## <a name="buildgrammar-command"></a>build_grammar komutu
**Build_grammar** komutu bir ikili dilbilgisi dosyaya XML dosyasında belirtilen dilbilgisi derler.  Sonuçta elde edilen dilbilgisi dosyasını birlikte doğal dil sorguları yorumlar oluşturmak için bir dizin dosyası ile kullanılabilir.

`kes.exe build_grammar <xmlFile> <grammarFile>`

| Parametre       | Açıklama               |
|-----------------|---------------------------|
| `<xmlFile>`     | Giriş XML dilbilgisi belirtimi yolu |
| `<grammarFile>` | Çıkış derlenmiş dilbilgisi yolu         |

Bu dosyalar, yerel dosya yolları veya Azure BLOB'ları için URL yolları tarafından belirtilebilir.  Dilbilgisi belirtimi ağırlıklı doğal dil ifadeleri ve bunların anlamsal yorumlar açıklar (bkz [dilbilgisi biçimini](GrammarFormat.md)).  Yapı başarılı olduğunda, çıktı dilbilgisi dosyası hızlı kod çözme etkinleştirmek için dilbilgisi belirtimi ikili gösterimidir içerir.

<a name="host_service-command"/>
## <a name="hostservice-command"></a>host_service komutu
**Host_service** komutu barındıran yerel makine üzerinde KES hizmetinin bir örneği.

`kes.exe host_service <grammarFile> <indexFile> [options]`

| Parametre       | Açıklama                |
|-----------------|----------------------------|
| `<grammarFile>` | Giriş ikili dilbilgisi yolu         |
| `<indexFile>`   | Giriş ikili dizin yolu           |
| `--port <port>` | Yerel bağlantı noktası numarası.  Varsayılan: 8000 |

Bu dosyalar, yerel dosya yolları veya Azure BLOB'ları için URL yolları tarafından belirtilebilir.  Bir web hizmeti konumunda barındırılan http://localhost:&lt; bağlantı noktası&gt;/.  Bkz: [Web API'leri](WebAPI.md) desteklenen işlemler listesi.

Azure dışında ortamı, yerel olarak barındırılan hizmetler dizin sınırlı boyutu, saniye başına 10 istekleri ve 1000 toplam çağrı sayısı en çok 1 MB dosyaları.  Bu sınırlamaların üstesinden gelmek için çalıştırın **host_service** bir Azure VM içinde veya bir Azure bulut hizmeti için dağıtmanızı **deploy_service**.

<a name="deploy_service-command"/>
## <a name="deployservice-command"></a>deploy_service komutu
**Deploy_service** komutu KES hizmetinin bir örneği için bir Azure bulut hizmeti dağıtır.

`kes.exe deploy_service <grammarFile> <indexFile> <serviceName> <vmSize>[options]`

| Parametre       | Açıklama                  |
|-----------------|------------------------------|
| `<grammarFile>` | Giriş ikili dilbilgisi yolu           |
| `<indexFile>`   | Giriş ikili dizin yolu             |
| `<serviceName>` | Hedef bulut hizmeti adı |
| `<vmSize>`      | Bulut hizmeti VM boyutu     |
| `--slot <slot>` | Bulut hizmeti yuva: "Hazırlama" (varsayılan), "üretim" |

Bu dosyalar, yerel dosya yolları veya Azure BLOB'ları için URL yolları tarafından belirtilebilir.  Hizmet adı önceden yapılandırılmış Azure bulut hizmeti belirtir (bkz [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md)).  Komut, belirtilen boyut Vm'leri kullanarak belirtilen Azure bulut hizmeti için KES hizmeti otomatik olarak dağıtacaktır.  Performansı önemli ölçüde azaltır disk belleği önlemek için bir VM 1 GB ile kullanarak giriş dizini dosya boyutundan daha fazla RAM öneririz.  Kullanılabilir VM boyutları listesi için bkz: [Cloud Services boyutları](../../../articles/cloud-services/cloud-services-sizes-specs.md).

Varsayılan olarak, hizmet hazırlama ortamına dağıtıldıktan yuva parametresi aracılığıyla isteğe bağlı olarak geçersiz kılınan.  Bkz: [Web API'leri](WebAPI.md) desteklenen işlemler listesi.

<a name="describe_index-command"/>
## <a name="describeindex-command"></a>describe_index komutu
**Describe_index** komutu açıklama ve şema dahil olmak üzere bir dizin dosyası hakkında bilgi verir.

`kes.exe describe_index <indexFile>`

| Parametre     | Açıklama      |
|---------------|------------------|
| `<indexFile>` | Giriş dizini yolu |

Bu dosya yerel bir dosya yolu veya bir Azure blob için bir URL yolu tarafından belirtilebilir.  Çıkış açıklama dizesi kullanılarak belirtilebilir açıklama parametresinin **build_index** komutu.

<a name="describe_grammar-command"/>
## <a name="describegrammar-command"></a>describe_grammar komutu
**Describe_grammar** komutu ikili dilbilgisi oluşturmak için kullanılan özgün dilbilgisi belirtimi çıkarır.

`kes.exe describe_grammar <grammarFile>`

| Parametre       | Açıklama      |
|-----------------|------------------|
| `<grammarFile>` | Giriş dilbilgisi yolu |

Bu dosya yerel bir dosya yolu veya bir Azure blob için bir URL yolu tarafından belirtilebilir.

