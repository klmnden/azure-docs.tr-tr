---
title: İşleri çalıştırmak için uçtan uca şablonlar - Azure Batch kullanarak | Microsoft Docs
description: Batch havuzları, işleri ve görevleri şablon dosyaları ve Azure CLI ile oluşturun.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 12/07/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 80d2e995a18a2d6dafbb8d92fdd5996b10eab17c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60783747"
---
# <a name="use-azure-batch-cli-templates-and-file-transfer"></a>Azure Batch CLI şablonlarını ve dosya aktarımı kullanın

Bir Azure toplu işlem uzantısı Azure CLI kullanarak Batch işlerini kod yazmadan çalıştırmak mümkündür.

Oluşturun ve Batch havuzları, işleri ve görevleri oluşturmak için Azure CLI ile JSON şablon dosyalarını kullanın. Kolayca depolama hesabına işin giriş dosyalarını yüklemek için CLI uzantı komutları kullanın, Batch hesabı ve indirme iş Çıkış dosyalarını ilişkili.

## <a name="overview"></a>Genel Bakış

Azure CLI uzantısı, Batch kullanılan uçtan uca Geliştirici olmayan kullanıcılar tarafından olmasını sağlar. Yalnızca CLI komutları ile havuz oluşturma, girdi verilerini karşıya yükleme, işleri ve ilişkili görevleri oluşturun ve elde edilen çıktı verilerini indirin. Ek kod gereklidir. CLI komutları doğrudan çalıştırın veya bunları betikleri ayrıntılı tümleştirin.

Batch şablonlarını oluşturmak [mevcut Batch desteği Azure clı'da](batch-cli-get-started.md#json-files-for-resource-creation) havuzlar, işler, görevler ve diğer öğeleri oluştururken özellik değerlerini belirtmek JSON dosyaları için. Batch şablonlarını aşağıdaki özellikleri ekleyin:

-   Parametreleri tanımlanabilir. Şablonu kullanıldığında, parametre değerleri ile diğer öğesi özellik değerleri şablon gövdesinde belirtilen öğe oluşturmak için belirtilir. Batch anlayan bir kullanıcıyı ve Batch tarafından çalıştırılacak uygulamaları şablonları, havuz, iş ve görev özellik değerleri belirtme oluşturabilirsiniz. Bir kullanıcının daha az bilinen toplu ve/veya uygulamalar ile tanımlanan parametrelerin değerlerini belirlemek yalnızca gerekir.

-   Proje Görev oluşturucular bir iş, oluşturulacak birçok görev tanımları gereksinimini ortadan kaldırır ve iş gönderme önemli ölçüde basitleştirme ilişkili bir veya daha fazla görevleri oluşturun.


İşler genellikle girdi veri dosyalarını kullanmak ve çıkış veri dosyaları oluşturur. Bir depolama hesabı, varsayılan olarak, her Batch hesabıyla ilişkilidir. Kodlama ve herhangi bir depolama kimlik bilgisi ile CLI'yı kullanarak bu depolama hesabına gelen ve giden dosyaları aktarma.

Örneğin, [ffmpeg](https://ffmpeg.org/) , ses ve video dosyaları işler yaygın bir uygulamadır. Kaynak görüntü dosyalarını farklı çözümler için ffmpeg çağırmak için Azure Batch CLI ile adımlar aşağıda verilmiştir.

-   Bir havuzu şablonu oluşturun. Ffmpeg uygulaması ve gereksinimleri çağırmak nasıl şablon oluşturma kullanıcının bildiği; uygun bir işletim sistemi, VM boyutu nasıl ffmpeg (Başlangıç, bir uygulama paketi veya örneğin bir paket Yöneticisi'ni kullanarak) yüklü ve diğer belirtin havuzu özellik değerleri. Şablon kullanıldığında, yalnızca Havuz kimliği ve sanal makine sayısı belirtilmesine gerek parametreleri oluşturulur.

-   Bir proje şablonu oluşturun. Şablonu oluşturan kullanıcıya nasıl ffmpeg kaynağına dönüştürme için farklı bir çözünürlüğü video çağrılması gerekir bilir ve görevin komut satırı belirtir; Bunlar, ayrıca her giriş dosyası için gerekli bir görev kaynak görüntü dosyalarını içeren bir klasör olduğunu biliyorsunuz.

-   Son kullanıcı ile video dosyaları için dönüştürme ilk yalnızca Havuz kimliği ve gereken VM sayısını belirten havuzu şablonu kullanarak bir havuz oluşturur. Bunlar e kodlamasını kaynak dosyalarını yükleyebilir. Bir işi daha sonra yalnızca Havuz kimliği ve karşıya yüklenen kaynak dosyalarının konumunu belirtme proje şablonunu kullanarak da gönderilebilir. Toplu işlem ile oluşturulan giriş dosya başına tek bir görev oluşturulur. Son olarak, kod çevrimi Çıkış dosyalarını indirilebilir.

## <a name="installation"></a>Yükleme

Azure Batch CLI uzantı yüklemeniz [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli), veya Azure CLI'yı çalıştırmak [Azure Cloud Shell](../cloud-shell/overview.md).

Aşağıdaki Azure CLI komutunu kullanarak toplu işlem uzantısı en son sürümünü yükleyin:

```azurecli
az extension add --name azure-batch-cli-extensions
```

Batch CLI uzantısını ve ek yükleme seçenekleri hakkında daha fazla bilgi için bkz: [GitHub deposunu](https://github.com/Azure/azure-batch-cli-extensions).


CLI uzantısı özellikleri kullanmak için Azure Batch hesabı gerekir ve depolamaya ve depolamadan bağlı depolama hesabındaki dosya aktarımı komutları.

Azure CLI ile bir Batch hesabı ile oturum açmak için bkz: [Azure CLI ile Batch kaynaklarını yönetme](batch-cli-get-started.md).

## <a name="templates"></a>Şablonlar

Azure Batch şablonları, Azure Resource Manager şablonları, işlevsellik ve söz dizimi benzerdir. Öğe özellik adlarını ve değerlerini içerir, ancak aşağıdaki kavramlarla ekleme JSON dosyaları bunlar:

-   **Parametreler**

    -   Özellik değerleri yalnızca şablon kullanıldığında, sağlanan gerek kalmadan parametre değerleriniz ile bir gövde bölümüne belirtilmesini sağlar. Örneğin, bir havuz için tam tanımı gövdesi ve yalnızca bir parametresi havuzu kimliği için tanımlanan konabilir; yalnızca bir havuzu kimlik dizesi, bu nedenle havuz oluşturmak için sağlanması gerekir.
        
    -   Şablon gövdesi, Batch ve Batch tarafından çalıştırılacak uygulamaları bilgisine sahip biri tarafından yazılabilir; şablonu kullanıldığında yalnızca yazar tarafından tanımlanan parametreler için değerler sağlanmalıdır. Ayrıntılı toplu ve/veya uygulama bilgisi olmayan bir kullanıcı, bu nedenle şablonları kullanabilirsiniz.

-   **Değişkenler**

    -   Tek bir yerde belirtilen ve bir veya daha fazla yerde şablon gövdesinde kullanılan basit veya karmaşık bir parametre değerlerini sağlar. Değişkenleri basitleştirin ve şablon azaltın, yapabilir özelliklerini değiştirmek için bir konum sağlayarak daha sürdürülebilir hale.

-   **Daha yüksek düzeyli yapıları**

    -   Bazı daha yüksek düzeyli yapıları, Batch API'leri henüz kullanılamıyor şablonda kullanılabilir. Örneğin, bir görev fabrikasını ortak görev tanımı kullanarak işi için birden çok görevi oluşturan iş şablonunda tanımlanabilir. Bu yapılar, Paket Yöneticisi üzerinden uygulamaları yüklemek için gereken dinamik olarak görev başına bir dosya gibi birden çok JSON dosyası oluşturun yanı sıra komut dosyaları oluşturmak için koduna kaçının.

    -   Belirli bir noktada bu yapılar, Batch hizmeti eklenebilir ve kullanılabilir Batch API'leri, kullanıcı arabirimleri, vb. olabilir.

### <a name="pool-templates"></a>Havuzu şablonları

Havuzu şablonları, parametreler ve değişkenler standart şablon özelliklerini destekler. Bunlar ayrıca aşağıdaki üst düzey yapısını destekler:

-   **Paket başvuruları**

    -   İsteğe bağlı olarak paket yöneticilerini kullanarak havuz düğümlerine kopyalanacak yazılım sağlar. Paket Yöneticisi ve paket kimliği belirtilmedi. Bir veya daha fazla paket bildirerek, gerekli paketleri alır bir betik oluşturma, komut dosyası yüklemek ve betik her havuzu düğüm üzerinde çalışan kaçının.

Yüklü ffmpeg ile Linux sanal makinelerinin bir havuzu oluşturan bir şablon örneği verilmiştir. Bunu kullanmak için yalnızca bir havuz kimliği dizesi ve havuzdaki VM sayısını sağlayın:

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "The number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The pool ID "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

Şablon dosyası adlandırırsanız _havuzu ffmpeg.json_, ardından şablonu şu şekilde Çağır:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

CLI için değerleri girmenizi ister `poolId` ve `nodeCount` parametreleri. Bir JSON dosyası parametrelerini de sağlayabilirsiniz. Örneğin:

```json
{
  "poolId": {
    "value": "mypool"
  },
  "nodeCount": {
    "value": 2
  }
}
```

Parametreler JSON dosyası adlandırırsanız *havuzu-parameters.json*, ardından şablonu şu şekilde Çağır:

```azurecli
az batch pool create --template pool-ffmpeg.json --parameters pool-parameters.json
```

### <a name="job-templates"></a>Proje şablonları

Proje şablonları, parametreler ve değişkenler standart şablon özelliklerini destekler. Bunlar ayrıca aşağıdaki üst düzey yapısını destekler:

-   **Görev fabrikasını**

    -   Birden çok görev bir iş için bir görev tanımını oluşturur. Görev fabrikasını üç tür desteklenen – parametrik tarama, dosya başına görev ve görev koleksiyonu.

Ffmpeg iki daha düşük çözünürlükler birine dönüştürme MP4 video dosyaları için bir iş oluşturur bir şablon örneği verilmiştir. Video kaynak dosya başına tek bir görev oluşturur. Bkz: [dosya gruplarını ve dosya aktarımı](#file-groups-and-file-transfer) giriş ve çıkış işi için dosya grupları hakkında daha fazla bilgi için.

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch pool which runs the job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

Şablon dosyası adlandırırsanız _iş ffmpeg.json_, ardından şablonu şu şekilde Çağır:

```azurecli
az batch job create --template job-ffmpeg.json
```

Olarak daha önce CLI parametrelerin değerlerini sağlamasını ister. Bir JSON dosyası parametrelerini de sağlayabilirsiniz.

### <a name="use-templates-in-batch-explorer"></a>Batch Explorer şablonlarını kullanma

Batch CLI şablona yükleyebilirsiniz [Batch Gezgini](https://github.com/Azure/BatchExplorer) bir Batch havuzu veya iş oluşturmak üzere masaüstü uygulaması (eski adıyla çağrılan BatchLabs). Ayrıca Batch Gezgini galerideki önceden tanımlanmış havuzu ve işini şablonları arasından seçim yapabilirsiniz.

Bir şablonu karşıya yüklemek için:

1. Batch Gezgini içinde seçin **galeri** > **yerel şablonları**.

2. Seçin ya da sürükle ve bırak, yerel havuz veya proje şablonu.

3. Seçin **bu şablonu kullan**, izlenebilmesini ekrandaki ister.

## <a name="file-groups-and-file-transfer"></a>Dosya grupları ve dosya aktarımı

Çoğu işleri ve görevleri giriş dosyaları gerektirir ve çıkış dosyaları üretir. Genellikle, girdi dosyalarını ve çıkış dosyalarının, istemciden düğüme veya istemci düğüme aktarılır. Azure Batch CLI uzantı koyma dosya aktarımı soyutlar ve her Batch hesabı ile ilişkilendirmek depolama hesabı kullanır.

Dosya grubu Azure depolama hesabında oluşturulan bir kapsayıcı karşılık gelir. Alt klasörleri dosya grubuna sahip olabilir.

Batch CLI uzantısını belirtilen dosya grubu için istemci dosyaları karşıya yükleme ve bir istemci için belirtilen dosya grubundan dosyaları indirmek için komutlar sağlar.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Kopyalama havuz düğümleri üzerine veya havuz düğümlerine geri dosya grubu için belirtilen dosya grupları depolanan dosyaların havuzu ve işini şablonları sağlar. Örneğin, işin içinde şablon daha önce dosya grubuna belirtilen *ffmpeg girişi* görev fabrikasını için kodlama dönüştürme düğümü aşağı kopyalanan kaynağı video dosya konumu olarak belirtilir. Dosya grubu *ffmpeg çıkış* kotanızdan çıkış dosyalarının her bir görevi çalıştırma düğümden kopyalandığından burada konumdur.

## <a name="summary"></a>Özet

Şablon ve dosya aktarımı desteği şu anda eklenmiş yalnızca Azure CLI için. Batch Araştırmacıları ve BT kullanıcıları gibi Batch API'lerini kullanarak kod geliştirin gerekmeyen kullanıcılar kullanabilirsiniz hedef kitle genişletin olmaktır. Kodlama olmadan Azure, Batch ve Batch tarafından çalıştırılacak uygulamaları bilen kullanıcılara havuzu ve işini oluşturmak için şablonları oluşturabilirsiniz. Şablon parametreleri ile toplu işlem ve uygulamalar ile ilgili ayrıntılı bilgi olmayan kullanıcıların şablonları kullanabilirsiniz.

Toplu işlem uzantısı için Azure CLI'yı deneyin ve bize geri bildirim veya öneriler, ya da bu makalede veya aracılığıyla açıklamalarda sağlayın [Batch topluluk depo](https://github.com/Azure/Batch).

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı yükleme ve kullanım belgeler, örnekler ve kaynak kodu kullanılabilir [Azure GitHub deposunda](https://github.com/Azure/azure-batch-cli-extensions).

- Kullanma hakkında daha fazla bilgi edinin [Batch Gezgini](https://github.com/Azure/BatchExplorer) Batch kaynaklarını oluşturmak ve yönetmek için.
