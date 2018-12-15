---
title: Batch AI depolanması, girdi ve çıktı ile Azure depolama iş | Microsoft Docs
description: Azure depolama için hızlı ve kolay bir bulut depolama giriş ve çıkış dosyalarının Batch AI ile kullanma
services: batch-ai
documentationcenter: ''
author: kevwang1
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 1e9a4c6355c60b18bb78aae362c1e2f142e2d864
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408009"
---
# <a name="store-batch-ai-job-input-and-output-with-azure-storage"></a>Batch AI işi girdi ve çıktı Azure depolama ile Store

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Bu kılavuz, bir iş çalışırken giriş ve Çıkış dosyalarını depolamak için Azure depolama kullanmayı açıklar. Azure depolama, Batch AI tarafından desteklenen çeşitli depolama seçenekleri biridir. Batch AI, bulutta depolanan dosyalar için sorunsuz erişim veren bir Batch AI işi veya küme dosya sistemi, Azure depolama sistemine bağlayarak Azure depolama ile tümleştirilir. 

## <a name="introduction-to-azure-storage"></a>Azure Depolama’ya Giriş

Azure Depolama, Microsoft’un bulut depolama çözümüdür. Azure Blob kapsayıcıları ve Batch AI işleri veya kümeleri için Azure dosya paylaşımları oluşturma, dosyaları yerel dosya sistemi değilmiş gibi bir işten erişilmesine izin vererek batch AI destekler. Batch AI ile Azure Blob kapsayıcıları bağlar [blobfuse](https://github.com/Azure/azure-storage-fuse)ve Azure dosya paylaşımları SMB protokolü aracılığıyla. Azure depolama hakkında daha fazla bilgi için bkz. [Azure Storage'a giriş](../storage/common/storage-introduction.md).

## <a name="store-datasets-and-input-scripts-in-azure-storage"></a>Azure Depolama'da veri kümeleri ve giriş betikleri Store

Batch AI ortamınız için Azure Depolama'yı seçtiğinizde, daha yüksek aktarım hızı olan bir Blob kapsayıcısında (örneğin, veri kümeleri) giriş dosyalarınızı depolamak eğitim çıkışınızı (izin verme akış destekleyen bir dosya paylaşımında depoladığınız öneririz. Çıktı günlüklerini okumak) iş eşzamanlı olarak çalışırken. 

Azure Depolama'yı kullanabilmeniz için önce şunları yapmalısınız [bir Azure depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md). Batch AI destekler bağlama birimlerden hem genel amaçlı v1 (GPv1) ve genel amaçlı v2 (GPv2) Azure depolama hesapları. Azure depolama hesabı, birden çok Blob kapsayıcıları tutun veya dosya paylaşımı örneklerini. Oluşturulacak depolama hesabı türünü seçerken, maliyet ve performans gereksinimlerinizi göz önünde bulundurun. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md). 

Bir Blob kapsayıcı oluşturun ve Veri kümenizi bir Azure Blob kapsayıcısını karşıya yüklemek için aşağıdaki yöntemlerden birini seçin:
- [Azure portalında](../storage/blobs/storage-quickstart-blobs-portal.md) web tabanlı bir GUI ile karşıya yükleme. Küçük sayıda dosyayı karşıya yüklemek için Azure portalında basit işlemi sağlar.
- [Azure depolama CLI](../storage/blobs/storage-quickstart-blobs-cli.md) (destekler directory karşıya yükleme) komut satırı karşıya yükleme. Dosyaların yüklemek için kullandığınız `az storage blob upload-batch`.
- [diğer teknikleri](../storage/common/storage-moving-data.md), uygulama SDK'ları kullanma dahil olmak üzere.

Benzer şekilde, Azure dosya paylaşımını oluşturmak için aşağıdaki yöntemlerden birini seçin:
- [Azure portal](../storage/files/storage-how-to-use-files-portal.md)
- [Azure depolama CLI](../storage/files/storage-how-to-use-files-cli.md)
- [diğer teknikleri](../storage/common/storage-moving-data.md)

### <a name="auto-storage-with-batch-ai"></a>Batch AI ile otomatik depolama

Alternatif olarak, bir Azure dosya paylaşımı ve Blob kapsayıcısı ile bir Azure depolama hesabı oluşturabilir (ve otomatik olarak bağlama için bir Batch AI kümesi bu birimleri) kullanarak `--use-auto-storage` parametresiyle `az batchai cluster create`. Daha fazla bilgi için [burada](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#auto-storage-account).

## <a name="mount-azure-storage"></a>Azure depolama bağlama 

### <a name="mount-volumes-to-a-job"></a>Bir iş bağlama birimleri

Bir Azure depolama birimine bağlama her iş için oluşturulan dosya sistemi üzerinden erişilmesine izin verir. Bu nedenle, bir iş yerel dosyaları değilmiş gibi depolama sorunsuz bir şekilde buluta dosyaları okuyup yazabilir.

Azure CLI ile oluşturulmuş bir işi bir Azure depolama birimine bağlamak `mountVolumes` özelliğinde, `job.json` dosyasını çalıştırırken `az batchai job create`. Bir örnek için bkz. [dağıtılmış Tensorflow GPU tarif](https://github.com/Azure/BatchAI/blob/master/recipes/TensorFlow/TensorFlow-GPU-Distributed/job.json). İçin şemayı `mountVolumes` olan:
```json
{
    "mountVolumes": {
        "azureFileShares": [{
            "azureFileUrl": "https://<STORAGE_ACCOUNT_NAME>.file.core.windows.net/<FILE_SHARE_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }],
        "azureBlobFileSystems": [{
            "accountName": "<STORAGE_ACCOUNT_NAME>",
            "containerName": "<BLOB_CONTAINER_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }]
    }
}
```

`azureFileShares` ve `azureBlobFileSystems` hem dizilerdir bağlamak için birimlerin temsil eden nesne. Yer tutucuları açıklamaları:

- < RELATIVE_MOUNT_PATH > - birim bu yola bağlı. Örneğin, `relativeMountPath` olduğu `jobs`, birim şu adreste yer alacaktır `$AZ_BATCHAI_JOB_MOUNT_ROOT/jobs`)
- < depolama_hesabı_adı > - dosya paylaşımı veya Blob kapsayıcısı tutan Azure depolama hesabı adı
- < FILE_SHARE_NAME > - Dosya Paylaşımı adı
- < BLOB_CONTAINER_NAME > - Blob kapsayıcısı adı

Azure Batch AI SDK'ları ile Azure depolama birimleri bağlamak için ayarlanmış `mount_volumes` (Python) veya `MountVolumes` (C#, Java) özelliği `JobCreateParameters`. Azure Batch AI SDK'ları birimlerle bağlarken depolama hesabının kimlik bilgilerini sağlamanız gerekir. Birimleri, bağlama şemalarını görüntüleme [Python](https://docs.microsoft.com/python/api/azure-mgmt-batchai/azure.mgmt.batchai.models.MountVolumes?view=azure-python), [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.batchai.models.mountvolumes?view=azure-dotnet), ve [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.management.batchai._mount_volumes?view=azure-java-stable).

### <a name="mount-volumes-to-a-cluster"></a>Bir kümeye bağlı birimleri

Batch AI, bir Batch AI kümesi için Azure depolama birimleri bağlama da destekler. Bir birim için bir küme bağlandığında, bu küme üzerinde çalışan tüm işleri bu kümeye takılı birimleri kullanabilir. İş düzeyi bağlama (farklı takılan birimler her bir iş olanak tanır) en üst düzeyde esneklik sağlasa da küme düzeyi bağlama basit senaryolar için yeterli olabilir.

Bir kümesi Azure CLI ile bir Azure depolama birimine bağlamak `mountVolumes` özelliğinde, `cluster.json` dosyasını çalıştırırken `az batchai cluster create`. İçin şemayı `mountVolumes` küme bağlama olduğunda aynı projeye bağlarken. 

Benzer şekilde, kullanabileceğiniz `mount_volumes` (Python) veya `MountVolumes` (C#, Java) özelliği `ClusterCreateParameters` Azure Batch AI SDK'ları ile bağlarken. 

## <a name="access-mounted-filesystem-in-a-job"></a>Bir İşte erişim bağlı dosya sistemi

### <a name="azbatchaijobmountroot-environment-variable"></a>$AZ_BATCHAI_JOB_MOUNT_ROOT ortam değişkeni

İşin yürütme ortamı içinde ile bağlı depolama sistemleri içeren dizine erişilebilir `$AZ_BATCHAI_JOB_MOUNT_ROOT` (iş düzeyi bağlama kullanıyorsanız) ortam değişkeni. Küme düzeyi bağlama kullanırsanız, bu ortam değişkenidir `$AZ_BATCHAI_MOUNT_ROOT`. Aşağıdaki örnekler, iş düzeyi bağlama kullandığınız varsayılır.

Bağlı birimlerdeki veriler yolunu sağlamak için ortam değişkenini kullanmak `$AZ_BATCHAI_JOB_MOUNT_ROOT` birlikte bağlı yolu. Örneğin, eğitim betiğini `train.py` için bir Azure dosya karşıya yüklendi paylaşımı görece bağlama yolu bağlı `scripts`, dosya kullanılabilir olmasını `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py`.

Eğitim betiğinizi bir yol bilgisi gerektiriyorsa, bir komut satırı bağımsız değişkeni geçmelidir. Örneğin, verilerinizi adlı bir klasöre depolanırsa `train_data` bir Azure Blob kapsayıcısında takılı yolunda `data`, çağrılsaydı `--data-dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/data/train_data` komut için komut satırı bağımsız değişkeni olarak. Buna uygun olarak, komut satırı bağımsız değişkenleri kabul etmek için kodunuzu değiştirmeniz gerekir.

### <a name="abbreviate-input-paths"></a>Giriş yollarından kısaltma

Giriş yollarından bir ortam değişkeni kısaltma için kullanın `inputDirectories` özelliği, `job.json` dosya (veya `models.JobCreateParameters.input_directories` Batch AI SDK'sını kullanılıyorsa). Şemasını `inputDirectories` olan:

```json
{
    "inputDirectories": [{
        "id": "<ID>",
        "path": "<PATH>" 
    }]
}
```

Adlı bir ortam değişkeninde belirtilen her bir yol yerleştirilecek `$AZ_BATCHAI_INPUT_<ID>`. Bu yöntem kullanılarak dosyaları veya dizinleri girişi için yol basitleştirebilir. Örneğin, bir eğitim betiği yolu kısaltma için: varsa `"id"` olduğu `"SCRIPT"` ve `"path"` olduğu `"$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py"`, söz konusu yoldaki kullanılabildiği durumda `$AZ_BATCHAI_INPUT_SCRIPT` işin yürütme ortamında.

Daha fazla bilgi için [burada](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#input-directories).

### <a name="abbreviate-output-paths"></a>Çıktı yollarında kısaltma

Bir ortam değişkeni çıktı yollarında kısaltma için kullanın `outputDirectories` özelliği, `job.json` dosya (veya `models.JobCreateParameters.output_directories` Batch AI SDK'sını kullanılıyorsa). Bu yöntemi kullanarak Çıkış dosyalarını yollarını basitleştirebilir. Şemasını `outputDirectories` olan:

```json
{
    "outputDirectories": [{
        "id": "<ID>",
        "pathPrefix": "<PATH_PREFIX>",
        "pathSuffix": "<PATH_SUFFIX>"
    }]
}
```

Adlı bir ortam değişkeninde belirtilen her bir yol yerleştirilecek `$AZ_BATCHAI_OUTPUT_<ID>`. `pathPrefix` Çıktısını depolamak için bağlı birimi belirler (örneğin, `"$AZ_BATCHAI_JOB_MOUNT_ROOT/output"`). `pathSuffix` Çıkış klasör adı belirler (örneğin, `"logs"`, `"checkpoints"`). Gerçek çıkış dizilerin bir bitiştirmesi olan yoludur `pathPrefix`, `jobOutputDirectoryPathSegment` (otomatik oluşturulan her iş için) ve `pathSuffix`.

Daha fazla bilgi için [burada](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#output-directories).

## <a name="retrieve-job-output-from-azure-storage"></a>İş çıktısını Azure Depolama'dan alın

### <a name="use-azure-portal"></a>Azure portalı kullanma

Azure portalında bir GUI dosya Gezgini'ni kullanarak işleri çıktısını görüntülemek için kullanışlı bir yoldur. Bununla birlikte, Stdout ve Stderr veya bir yolda çıkışı görüntülemek istiyorsanız `outputDirectories`, dosyalar, Azure depolama birimindeki bir otomatik olarak oluşturulan yol yerleştirilir. Daha fazla bilgi için aşağıya bakın.

### <a name="access-stdout-and-stderr-output"></a>Erişim Stdout ve Stderr çıktısı

Kullanım `stdOutErrPathPrefix` özelliği `job.json` işi iş yürütme günlükleri ve Stdout ve Stderr çıktısı yerleştirileceği yeri söylemek için. Örneğin, bir dosya paylaşımını görece bağlama yolu takılı `outputs`, ve belirttiğiniz `stdOutErrPathPrefix` olmasını `"$AZ_BATCHAI_JOB_MOUNT_ROOT/outputs"`, Stdout ve Stderr iş çıktısı adresinde sonra `{subscription id}/{resource group}/workspaces/{workspace name}/experiments/{experiment name}/jobs/{job name}/{job uuid}/stdouterr` , birimin takılı. Bu otomatik olarak oluşturulan yol, aynı ada sahip işler arasındaki çıkış çakışmalarını önlemek için kullanılır.

Daha fazla bilgi için [burada](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#standard-and-error-output).

### <a name="list-the-output-files"></a>Çıkış dosyalarının listesi

Azure CLI ile bir işin kullanılabilir çıktı dosyaları listelemek için kullanabileceğiniz `az batchai job file list` komutu. Örneğin, bir işin standart çıkış dizinindeki dosyaları listelemek için kullanın `az batchai job file list -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME>`.

Daha fazla bilgi ve örnekler için bkz. [burada](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

### <a name="stream-output-files"></a>Stream çıktı dosyaları

Azure CLI ile stream dosyaları için kullanabileceğiniz `az batchai job file stream` komutu. Örneğin, görüntülemek için şunu yazın:
- STDOUT: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stdout.txt`
- Stderr: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stderr.txt`

Daha fazla bilgi ve örnekler için bkz. [burada](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

## <a name="next-steps"></a>Sonraki adımlar
- Azure depolama ile arabirim oluşturmak için CLI komutları hakkında daha fazla bilgi görüntülemek [Azure Batch AI CLI belgeleri](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md).
- Batch AI, depolama monte ederek ve çıkış dosyaları okuma da dahil olmak üzere daha fazla kullanım örneklerini bulmak için bkz: [Batch AI için Jupyter not defteri tarifleri](https://github.com/Azure/BatchAI).
- Depolama, bağlama için diğer seçenekleri keşfedin dahil olmak üzere [bir NFS sunucusunun bağlama](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs) ve [NFS veya CIFS GlusterFS kendi kümenizi oluşturma](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs)