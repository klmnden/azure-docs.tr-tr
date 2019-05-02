---
title: Çalışma zamanı ortam değişkenlerini - Azure Batch görevi | Microsoft Docs
description: Görev çalışma zamanı ortamı değişken Kılavuzu ve Azure Batch analizi için başvuru.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/23/2019
ms.author: lahugh
ms.openlocfilehash: c46f75c447becc8b15d4a6b8f979330db7ab95c7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64575566"
---
# <a name="azure-batch-runtime-environment-variables"></a>Azure Batch çalışma zamanı ortam değişkenleri

[Azure Batch hizmeti](https://azure.microsoft.com/services/batch/) işlem düğümlerinde aşağıdaki ortam değişkenlerini ayarlar. Bu ortam değişkenleri, görev komut satırlarında ve programları başvurabilir ve komut satırları tarafından betikleri çalıştırın.

Ortam değişkenleri Batch ile birlikte kullanma hakkında ek bilgi için bkz: [görevler için ortam ayarları](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Ortam değişkeni görünürlük

Bu ortam değişkenleri yalnızca bağlamında görülebilir **görev kullanıcı**, kullanıcı hesabı altında bir görev yürütmüş düğümde. Bir işlem düğümüne Uzak Masaüstü Protokolü (RDP) veya Güvenli Kabuk (SSH) aracılığıyla [uzaktan bağlanıp](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) ortam değişkenlerini listelerseniz bunları *göremezsiniz*. Bunun nedeni, uzak bağlantı için kullanılan kullanıcı hesabının görev tarafından kullanılan hesapla aynı olmamasıdır.

Bir ortam değişkeninin geçerli değerini almak için başlatma `cmd.exe` işlem düğümü üzerinde bir Windows veya `/bin/sh` Linux düğümde:

`cmd /c set <ENV_VARIABLE_NAME>`

`/bin/sh printenv <ENV_VARIABLE_NAME>`

## <a name="command-line-expansion-of-environment-variables"></a>Ortam değişkenlerini komut satırı genişletme

Görevleri tarafından yürütülen komut satırları bilgi işlem düğümleri bir kabuk altında çalışmaz. Bu nedenle, bu komut satırları yerel olarak ortam değişken genişletmesi gibi Kabuk özelliklerinden yararlanamaz (Bu içerir `PATH`). Bu özelliklerden yararlanmak için **Kabuk çağırma** komut satırında. Örneğin, başlatma `cmd.exe` üzerinde Windows işlem düğümleri veya `/bin/sh` Linux düğümlerinde:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Ortam değişkenleri

| Değişken adı                     | Açıklama                                                              | Kullanılabilirlik | Örnek |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | Görevin ait bir Batch hesabı adı.                  | Tüm görevler.   | mybatchaccount |
| AZ_BATCH_ACCOUNT_URL            | Batch hesabı URL'si. | Tüm görevler. | `https://myaccount.westus.batch.azure.com` |
| AZ_BATCH_APP_PACKAGE            | Tüm uygulama paketi ortam değişkenlerini önek. Örneğin, "Foo" Sürüm "1" uygulama havuzu yüklüyse, ortam değişkenini AZ_BATCH_APP_PACKAGE_FOO_1 olur. (Klasör) AZ_BATCH_APP_PACKAGE_FOO_1 noktaları, paket konumuna indirildi. | Herhangi bir görev ile ilişkili uygulama paketi. Ayrıca, uygulama paketleri düğüm varsa, tüm görevler için de kullanılabilir. | AZ_BATCH_APP_PACKAGE_FOO_1 |
| AZ_BATCH_AUTHENTICATION_TOKEN   | Batch hizmet işlemlerine sınırlı sayıda erişim veren bir kimlik doğrulama belirteci. Bu ortam değişkeni yalnızca var ise [authenticationTokenSettings](/rest/api/batchservice/task/add#authenticationtokensettings) zaman ayarlanır [görev eklendiğinde](/rest/api/batchservice/task/add#request-body). Belirteç değeri, bir Batch istemcisi gibi oluşturmak için kimlik bilgileri olarak Batch API'leri kullanılır [BatchClient.Open() .NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_). | Tüm görevler. | OAuth2 erişim belirteci |
| AZ_BATCH_CERTIFICATES_DIR       | Bir dizin içinde [görev çalışma dizini] [ files_dirs] işlem düğümleri hangi sertifikaların Linux için depolanır. İşlem düğümleri unutmayın, bu ortam değişkenini Windows için geçerli değildir.                                                  | Tüm görevler.   |  /mnt/batch/Tasks/workitems/batchjob001/Job-1/task001/certs |
| AZ_BATCH_HOST_LIST              | İçin ayrılan düğümler listesi bir [çok örnekli görev] [ multi_instance] biçimde `nodeIP,nodeIP`. | Çok örnekli birincil ve alt görevler otomatiktir. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Geçerli düğüm için ana düğüm olup olmadığını belirten bir [çok örnekli görev][multi_instance]. Olası değerler `true` ve `false`.| Çok örnekli birincil ve alt görevler otomatiktir. | `true` |
| AZ_BATCH_JOB_ID                 | Görevin ait olduğu işin kimliği. | Başlangıç görevi dışında tüm görevler. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | İş hazırlama tam yolunu [görev dizininde] [ files_dirs] düğümde. | Başlangıç görevi ve iş hazırlama görevi dışında tüm görevler. Yalnızca iş bir iş hazırlama görevi ile yapılandırılmışsa kullanılabilir. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | İş hazırlama tam yolunu [görev çalışma dizini] [ files_dirs] düğümde. | Başlangıç görevi ve iş hazırlama görevi dışında tüm görevler. Yalnızca iş bir iş hazırlama görevi ile yapılandırılmışsa kullanılabilir. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_MASTER_NODE            | IP adresi ve bağlantı noktası işlem düğümünde birincil görevin bir [çok örnekli görev] [ multi_instance] çalıştırır. | Çok örnekli birincil ve alt görevler otomatiktir. | `10.0.0.4:6000` |
| AZ_BATCH_NODE_ID                | Görev atanan düğüm kimliği. | Tüm görevler. | tvm-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_IS_DEDICATED      | Varsa `true`, geçerli düğüm adanmış bir düğümdür. Varsa `false`, bunun bir [düşük öncelikli düğüm](batch-low-pri-vms.md). | Tüm görevler. | `true` |
| AZ_BATCH_NODE_LIST              | İçin ayrılan düğümler listesi bir [çok örnekli görev] [ multi_instance] biçimde `nodeIP;nodeIP`. | Çok örnekli birincil ve alt görevler otomatiktir. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_NODE_ROOT_DIR          | Tüm kök tam yolunu [toplu dizinleri] [ files_dirs] düğümde. | Tüm görevler. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | Tam yolunu [paylaşılan dizine] [ files_dirs] düğümde. Bir düğümde yürütülen tüm görevler bu dizinde okuma/yazma erişimi. Diğer düğümlerde çalıştırılacak görevleri uzak ("paylaşılan" ağ dizinine değildir) bu dizine erişiminiz yok. | Tüm görevler. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | Tam yolunu [görev directory başlangıç] [ files_dirs] düğümde. | Tüm görevler. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | Görevin çalıştığı havuzun kimliği. | Tüm görevler. | batchpool001 |
| AZ_BATCH_TASK_DIR               | Tam yolunu [görev dizininde] [ files_dirs] düğümde. Bu dizin içeren `stdout.txt` ve `stderr.txt` görev ve AZ_BATCH_TASK_WORKING_DIR. | Tüm görevler. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | Geçerli görevin kimliği. | Başlangıç görevi dışında tüm görevler. | task001 |
| AZ_BATCH_TASK_SHARED_DIR | Birincil görev ve her alt görevi için aynı olan bir dizin yolu bir [çok örnekli görev][multi_instance]. Yolun var olduğundan, çok örnekli görev çalıştırır ve okuma/yazma ile ilgili düğümde çalışan görev komutları için erişilebilir olduğundan her düğüme (hem [koordinasyon komut] [ coord_cmd] ve [ Uygulama komutu][app_cmd]). Alt görevler veya diğer düğümlerde çalıştırılacak birincil görevi, uzaktan erişim ("paylaşılan" ağ dizinine değildir) bu dizine yoktur. | Çok örnekli birincil ve alt görevler otomatiktir. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_TASK_WORKING_DIR       | Tam yolunu [görev çalışma dizini] [ files_dirs] düğümde. Şu anda çalışan görev bu dizinde okuma/yazma erişimi vardır. | Tüm görevler. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | Düğümler için ayrılmış düğüm başına çekirdek sayısı ve listesi bir [çok örnekli görev][multi_instance]. Düğümler ve çekirdek biçiminde listelenir `numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, burada düğüm sayısını ardından bir veya daha fazla düğüm IP adresleri ve çekirdek sayısı tarafından her biri için. |  Çok örnekli birincil ve alt görevler otomatiktir. |`2 10.0.0.4 1 10.0.0.5 1` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
