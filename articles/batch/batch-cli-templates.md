---
title: Azure Batch işleri uçtan uca (Önizleme) kod yazmadan çalıştırma | Microsoft Docs
description: Batch havuzları, işleri ve görevleri oluşturmak Azure CLI için şablon dosyalarını oluşturun.
services: batch
author: mscurrell
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 12/18/2017
ms.author: markscu
ms.openlocfilehash: c991a1535b82a76a3d0b9bdbf4ede8f3e22bc866
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Azure Batch CLI Şablonlarını ve Dosya Aktarımı (Önizleme) özelliğini kullanma

Azure CLI kullanarak kod yazmadan toplu işlerini çalıştırmak mümkün değildir.

Oluşturun ve toplu havuzlar, işler ve görevler oluşturmak için Azure CLI ile şablon dosyalarını kullanın. İş giriş dosyaları, yüklenen toplu işlem hesabı ve iş çıktı dosyaları ile ilişkilendirilmiş depolama hesabına kolayca yüklenebilir.

## <a name="overview"></a>Genel Bakış

Azure CLI uzantısı toplu kullanılan uçtan uca geliştiriciler olmayan kullanıcılar tarafından olmasını sağlar. Bir havuz oluşturulabilir, giriş verilerini karşıya, işler ve oluşturulan, ilişkili görevler ve sonuçta elde edilen çıktı verilerini – kod gerekli, CLI indirilen doğrudan kullanılan veya komut dosyalarına tümleştirilmekte.

Toplu şablonları yapı [Azure CLI mevcut toplu desteği](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) havuzlar, işler, görevler ve diğer öğeleri oluşturulması için özellik değerlerini belirlemek JSON dosyaları sağlar. Toplu işlem şablonları ile aşağıdaki özellikleri JSON dosyalarıyla olası nedir üzerinden eklendi:

-   Parametreleri tanımlanabilir. Şablon kullanıldığında, yalnızca parametre değerlerini öğesi başka bir öğeyle şablon gövdesinde belirtilen özellik değerlerini oluşturmak için belirtilir. Batch özelliğini algılayan bir kullanıcıyı ve toplu işi tarafından çalıştırılacak uygulamaların şablonları, havuz, iş ve görev özellik değerleri belirtme oluşturabilirsiniz. Bir kullanıcı daha az bilmiyorsanız toplu ve/veya uygulamalar yalnızca tanımlanan parametrelerin değerlerini belirtin gerekir.

-   İş görevi oluşturucuları bir iş, oluşturulacak birçok görev tanımları gereksinimini önleme ve önemli ölçüde iş gönderme basitleştirme ilişkili bir veya daha fazla görev oluşturun.


Girdi veri dosyalarını işleri sağlanması gerekir ve çıktı veri dosyaları çoğunlukla üretilir. Bir depolama hesabı, varsayılan olarak, her Batch hesabıyla ilişkilendirilir ve dosyalar kolayca hiçbir kodlama ile CLI kullanarak bu depolama hesabı gelen ve giden aktarılabilir ve hiçbir depolama kimlik bilgileri gerekir.

Örneğin, [ffmpeg](http://ffmpeg.org/) , ses ve video dosyaları işler yaygın bir uygulamadır. Azure Batch CLI kodlamasını kaynak video dosyaları farklı çözümler için ffmpeg çağırmak için kullanılabilir.

-   Bir havuzu şablonu oluşturulur. Şablon oluşturma kullanıcı ffmpeg uygulama ve onun gereksinimlerine çağırmak nasıl bilir; uygun işletim sistemi, VM boyutu nasıl ffmpeg yüklü (bir uygulama paketi veya örneğin bir paket Yöneticisi'ni kullanarak) ve diğer belirtin havuz özellik değerleri. Şablon kullanıldığında, yalnızca Havuz kimliği ve VM'lerin sayısını belirtilmesi gereken şekilde parametreleri oluşturulur.

-   Bir proje şablonu oluşturulur. Şablon oluşturma kullanıcı nasıl kodlamasını kaynak farklı bir çözüm video çağrılacak ffmpeg gerektiğini biliyor ve görev komut satırını belirtir; Bunlar, ayrıca her giriş dosyası için gerekli bir görev kaynak video dosyaları içeren bir klasör olduğunu bilirsiniz.

-   Video dosyaları için dönüştürme son kullanıcı ilk olarak yalnızca Havuz kimliği ve gerekli VM'lerin sayısını belirtme havuzu şablonu kullanarak bir havuz oluşturur. Bunlar daha sonra kaynak dosyalarını kodlamasını karşıya yükleyebilirsiniz. Bir işi yalnızca Havuz kimliği ve karşıya kaynak dosyalarının konumunu belirtme proje şablonunu kullanarak ardından gönderilebilir. Toplu işlem giriş dosyası oluşturulan başına bir görev ile oluşturulur. Son olarak, kod çevrimi çıktı dosyaları karşıdan yüklenebilir.

## <a name="installation"></a>Yükleme

Şablon ve dosya aktarımı özellikleri uzantı yüklü olmasını gerektirir.

Azure CLI yükleme hakkında yönergeler için bkz [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

Azure CLI yüklendikten sonra toplu uzantısı'nın en son sürümü aşağıdaki CLI komutu kullanılarak yüklenebilir:

```azurecli
az extension add --source https://github.com/Azure/azure-batch-cli-extensions/releases/download/azure-batch-cli-extensions-2.0.1/azure_batch_cli_extensions-2.0.1-py2.py3-none-any.whl
```

Toplu uzantıları hakkında daha fazla bilgi için bkz: [için Microsoft Azure toplu işlem CLI uzantıları Windows, Mac ve Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).

## <a name="templates"></a>Şablonlar

Azure Batch CLI havuzlar, işler ve özellik adları ve değerleri içeren bir JSON dosyası belirterek oluşturulacak görevler gibi öğeleri sağlar. Örneğin:

```azurecli
az batch pool create –-json-file AppPool.json
```

Azure toplu işlem şablonları, işlevsellik ve sözdizimi Azure Resource Manager şablonları benzerdir. Öğe özellik adları ve değerleri içerir, ancak aşağıdaki kavramlarla eklemek JSON dosyaları bunlar:

-   **Parametreler**

    -   Gövde bölümünde, yalnızca şablon kullanıldığında, sağlanan gerek parametre değerleri ile belirtilmesi için özellik değerlerini izin verin. Örneğin, bir havuz için tam tanımı gövdesi ve havuzu kimliği için tanımlanan yalnızca bir parametre yerleştirilemedi; yalnızca bir havuz kimliği dizesi bu nedenle bir havuz oluşturmak üzere sağlanmalıdır.
        
    -   Şablon gövde toplu ve toplu işi tarafından çalıştırılacak uygulamaların bilgisine sahip biri tarafından yazılabilir; Şablon kullanıldığında, yalnızca yazar tarafından tanımlanan parametreler için değerler sağlanmalıdır. Ayrıntılı toplu ve/veya uygulama bilgisi olmayan bir kullanıcı, bu nedenle şablonları kullanabilirsiniz.

-   **Değişkenler**

    -   Tek bir yerde belirtilen ve şablon gövdesi bir veya daha fazla yerde kullanılan basit veya karmaşık parametre değerlerini izin verin. Değişkenleri basitleştirmek ve şablon boyutunu küçültmek yanı daha rahat değeri değişebilir özelliklerini değiştirmek için bir konum sağlayarak kolaylaştırır.

-   **Daha yüksek düzeyli yapıları**

    -   Bazı daha yüksek düzeyli yapıları Batch API'leri henüz kullanılamıyor şablon kullanılabilir. Örneğin, bir görev fabrikası ortak görev tanımı kullanarak iş için birden çok görevleri oluşturan iş şablonunda tanımlanabilir. Örneğin bir paket Yöneticisi üzerinden uygulamaları yüklemek için gereken kod dinamik olarak görev başına bir dosyayı gibi birden çok JSON dosyası oluşturun yanı sıra komut dosyaları oluşturmak için bu yapıları kaçının.

    -   Belirli bir noktada geçerli olduğunda, bu yapıları Batch hizmeti eklenebilir ve kullanılabilir Batch API'leri, Uı'lar, vb. olabilir.

### <a name="pool-templates"></a>Havuzu şablonları

Parametreler ve değişkenler standart şablon özelliklere ek olarak aşağıdaki üst düzey yapıları havuzu şablon tarafından desteklenir:

-   **Paket referanslarını**

    -   İsteğe bağlı olarak yazılım paketi yöneticileri kullanarak havuzu düğümlerine kopyalanacak sağlar. Paket Yöneticisi ve paket kimliği belirtilmedi. Bir veya daha fazla paket gerekli paketleri alır bir komut dosyası oluşturma ihtiyacını ortadan kaldırır bildirmek erişebildiklerinden, komut dosyası yükleyin ve her bir havuz düğümde komut dosyasını çalıştırın.

Bir havuz kimlik dizesi ve kullanmak için sağlanacak VM'ler yüklü ve yalnızca Linux VM'ler havuzu ile ffmpeg oluşturan bir şablon örneği gerektirir verilmiştir:

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
                    "sku": "16.04.0-LTS",
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

Şablon dosyası adlandırırsanız _havuzu ffmpeg.json_, şablon gibi çağrıldığı sonra:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a>Proje şablonları

Parametreler ve değişkenler standart şablon özelliklere ek olarak aşağıdaki üst düzey yapıları proje şablonu tarafından desteklenir:

-   **Görev Fabrika**

    -   Bir iş için birden çok görevleri bir görev tanımından oluşturur. Üç tür görev Fabrika desteklenir – parametrik, dosya başına görev sweep ve görev koleksiyonu.

Her kaynak video dosyası oluşturulan bir görev ile kodlamasını MP4 video dosyaları için iki alt çözümlerden birini ffmpeg kullanan bir işi oluşturan bir şablonu bir örnek verilmiştir:

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

Şablon dosyası adlandırırsanız _iş ffmpeg.json_, şablon gibi çağrıldığı sonra:

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a>Dosya grupları ve dosya aktarımı

Çoğu işler ve görevler girdi dosyası gerektirir ve çıktı dosyaları üretir. Her ikisi de girişi dosyaları ve çıktı dosyaları genellikle düğüme istemcisinden veya istemci düğüme aktarılması gerekir. Azure Batch CLI uzantısı koyma dosya aktarımı soyutlar ve her toplu işlem hesabı için varsayılan olarak oluşturulan depolama hesabı kullanır.

Bir dosya grubu, Azure depolama hesabında oluşturulan bir kapsayıcı karşılık gelir. Dosya grubunun alt klasörleri olabilir.

Toplu CLI uzantısını komutları belirtilen dosya grubu istemci yükleme dosyalarını ve bir istemci için belirtilen dosya grubundan karşıdan dosya sağlar.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Havuz ve proje şablonları kopya havuzu düğümleri üzerine veya bir dosya grubuna geri havuzu düğümleri kapatmak için belirtilen dosya grupları depolanan dosyaları sağlar. Örneğin, kod düğümünü üzerine aşağı kopyalanan kaynak video dosyalarının konumu olarak daha önce belirtilen iş şablonunda dosya grubu "ffmpeg Giriş" için görev üreteci belirtilir; Dosya grubu "ffmpeg-output" nerede kod çevrimi çıktı dosyaları düğümde çalışan her görev kopyalanır konum olarak kullanılır.

## <a name="summary"></a>Özet

Şablon ve dosya aktarımı desteği şu anda eklenmiş yalnızca Azure CLI. Toplu Araştırmacıları, BT kullanıcıları gibi Batch API'lerini kullanarak kod geliştirme ve benzeri gerekmez kullanıcılar için kullanabileceğiniz İzleyici genişletmek için belirtilir. Kodlama olmadan Azure, toplu ve toplu işi tarafından çalıştırılacak uygulamaların bilgisine sahip kullanıcılar havuzu ve iş oluşturma için şablonlar oluşturabilirsiniz. Şablon parametreleri ile toplu ve uygulamalar ile ilgili ayrıntılı bilgi olmayan kullanıcılar şablonları kullanabilirsiniz.

Out toplu uzantısı için Azure CLI deneyin ve bize geri bildirim ya da önerileri, ya da bu makale için veya aracılığıyla açıklamalarında sağlamak [Azure Batch forumunu](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).

## <a name="next-steps"></a>Sonraki adımlar

- Toplu işlem şablonları blog gönderisine bakın: [çalışan Azure Batch işleri Azure CLI – gerekli kod kullanarak](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).
- Ayrıntılı Kurulum ve kullanım belgeler, örnekler ve kaynak kodu bulunan [Azure GitHub deposunu](https://github.com/Azure/azure-batch-cli-extensions).
