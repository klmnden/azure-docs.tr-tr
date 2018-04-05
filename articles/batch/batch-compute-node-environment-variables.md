---
title: Azure toplu işlem düğüm ortam değişkenleri | Microsoft Docs
description: Düğüm ortam değişkeni başvurusu Azure toplu analizi için işlem.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/05/2017
ms.author: danlep
ms.openlocfilehash: ca8d6a6484cd1f145e7d807681bf2d012f2399e0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-batch-compute-node-environment-variables"></a>Azure toplu işlem düğüm ortam değişkenleri
[Azure Batch hizmeti](https://azure.microsoft.com/services/batch/) işlem düğümlerinde aşağıdaki ortam değişkenlerini ayarlar. Bu ortam değişkenleri görev komut satırları ve programları başvurusu yapabilir ve komut dosyaları tarafından komut satırları çalıştırmak.

Ortam değişkenleri Batch ile kullanma hakkında ek bilgi için bkz: [görevler için ortam ayarları](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Ortam değişkeni görünürlüğü

Bu ortam değişkenleri yalnızca bağlamında görünür **görev kullanıcı**, kullanıcı hesabı altında bir görev yürütüldüğünde düğümde. Bir işlem düğümüne Uzak Masaüstü Protokolü (RDP) veya Güvenli Kabuk (SSH) aracılığıyla [uzaktan bağlanıp](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) ortam değişkenlerini listelerseniz bunları *göremezsiniz*. Bunun nedeni, uzak bağlantı için kullanılan kullanıcı hesabının görev tarafından kullanılan hesapla aynı olmamasıdır.

## <a name="command-line-expansion-of-environment-variables"></a>Ortam değişkenleri komut satırı genişletme

Komut görevler tarafından yürütülen satırları bir kabuk altında düğümleri çalıştırmayın işlem. Bu nedenle, bu komut satırlarından ortam değişkeni genişletmesi gibi Kabuk özelliklerinden yerel alamıyor (Bu içerir `PATH`). Özelliklerden yararlanmak için size gereken **Kabuk çağırma** komut satırında. Örneğin, başlatma `cmd.exe` Windows işlem düğümleri veya `/bin/sh` Linux düğümleri üzerinde:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Ortam değişkenleri

| Değişken adı                     | Açıklama                                                              | Kullanılabilirlik | Örnek |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | Görevin ait olduğu toplu işlem hesabının adı.                  | Tüm görevler.   | mybatchaccount |
| AZ_BATCH_CERTIFICATES_DIR       | Bir dizin içinde [görev çalışma dizini] [ files_dirs] işlem düğümleri hangi sertifikaların Linux için depolanır. Bu ortam değişkenini Windows uygulanmaz Not işlem düğümlerini.                                                  | Tüm görevler.   |  /mnt/batch/tasks/workitems/batchjob001/job-1/task001/certs |
| AZ_BATCH_JOB_ID                 | Görevin ait olduğu işin kimliği. | Başlangıç görevi dışında tüm görevler. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | İş hazırlama tam yolunu [görev dizini] [ files_dirs] düğümde. | Başlangıç görevi ve iş hazırlama görevi dışında tüm görevler. Yalnızca iş iş hazırlama görevi ile yapılandırılmışsa, kullanılabilir. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | İş hazırlama tam yolunu [görev çalışma dizini] [ files_dirs] düğümde. | Başlangıç görevi ve iş hazırlama görevi dışında tüm görevler. Yalnızca iş iş hazırlama görevi ile yapılandırılmışsa, kullanılabilir. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_NODE_ID                | Görev atandığı düğümün kimliği. | Tüm görevler. | tvm-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_ROOT_DIR          | Tüm kök tam yolunu [toplu dizinleri] [ files_dirs] düğümde. | Tüm görevler. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | Tam yolunu [paylaşılan dizine] [ files_dirs] düğümde. Bir düğümde yürütülen tüm görevler bu dizinde okuma/yazma erişimi. Diğer düğümlerinde yürütülen görevleri uzaktan (bunu bir "paylaşılan" ağ dizin değil) bu dizine erişiminiz yok. | Tüm görevler. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | Tam yolunu [görev dizini Başlat] [ files_dirs] düğümde. | Tüm görevler. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | Görevin çalıştığı havuzun kimliği. | Tüm görevler. | batchpool001 |
| AZ_BATCH_TASK_DIR               | Tam yolunu [görev dizini] [ files_dirs] düğümde. Bu dizin içerir `stdout.txt` ve `stderr.txt` görev ve AZ_BATCH_TASK_WORKING_DIR. | Tüm görevler. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | Geçerli görevin kimliği. | Başlangıç görevi dışında tüm görevler. | task001 |
| AZ_BATCH_TASK_WORKING_DIR       | Tam yolunu [görev çalışma dizini] [ files_dirs] düğümde. Şu anda çalışan görev bu dizinde okuma/yazma erişimi. | Tüm görevler. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | Düğümler için ayrılan düğümü başına çekirdek sayısı ve listesi bir [çok örnekli görev][multi_instance]. Düğümler ve çekirdek biçiminde listelenir `numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, burada düğüm sayısını ardından bir veya daha fazla düğüm IP adresleri ve çekirdek sayısı tarafından her biri için. |  Çok örnekli birincil ve alt görevler. |`2 10.0.0.4 1 10.0.0.5 1` |
| AZ_BATCH_NODE_LIST              | İçin ayrılan düğümler listesi bir [çok örnekli görev] [ multi_instance] biçimde `nodeIP;nodeIP`. | Çok örnekli birincil ve alt görevler. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_HOST_LIST              | İçin ayrılan düğümler listesi bir [çok örnekli görev] [ multi_instance] biçimde `nodeIP,nodeIP`. | Çok örnekli birincil ve alt görevler. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_MASTER_NODE            | IP adresi ve bağlantı noktası, üzerinde işlem düğümünün birincil görevi bir [çok örnekli görev] [ multi_instance] çalıştırır. | Çok örnekli birincil ve alt görevler. | `10.0.0.4:6000`|
| AZ_BATCH_TASK_SHARED_DIR | Birincil görev ve her görevinin için aynı olan bir dizin yolu bir [çok örnekli görev][multi_instance]. Üzerinde çok örnekli görev çalıştırır ve okuma/yazma bu düğümde çalışan görev komutları için erişilebilir olan her bir düğümde yol yok (hem [koordinasyon komutu] [ coord_cmd] ve [uygulama komutu][app_cmd]). Alt görevler veya diğer düğümlerde yürütmek birincil bir görev, uzaktan erişim (bunu bir "paylaşılan" ağ dizin değil) bu dizine yoktur. | Çok örnekli birincil ve alt görevler. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Geçerli düğüm için ana düğüm olup olmadığını belirtir bir [çok örnekli görev][multi_instance]. Olası değerler şunlardır: `true` ve `false`.| Çok örnekli birincil ve alt görevler. | `true` |
| AZ_BATCH_NODE_IS_DEDICATED | Varsa `true`, geçerli düğüm adanmış bir düğümdür. Varsa `false`, bu bir [düşük öncelikli düğüm](batch-low-pri-vms.md). | Tüm görevler. | `true` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
