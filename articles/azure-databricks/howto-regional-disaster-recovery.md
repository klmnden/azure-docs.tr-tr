---
title: Azure Databricks için bölgesel bir olağanüstü durum kurtarma
description: Bu makalede, Azure databricks'te olağanüstü durum kurtarma yapmaya bir yaklaşım açıklanmaktadır.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/13/2019
ms.openlocfilehash: bd91d9201e81c884b48b41de27146c186eeb9598
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60784675"
---
# <a name="regional-disaster-recovery-for-azure-databricks-clusters"></a>Azure Databricks kümeleri için bölgesel bir olağanüstü durum kurtarma

Bu makalede, Azure Databricks kümeleri için kullanışlı bir olağanüstü durum kurtarma mimarisi ve bu tasarımı gerçekleştirmek için gereken adımları açıklar.

## <a name="azure-databricks-architecture"></a>Azure Databricks mimarisi

Azure portalından, bir Azure Databricks çalışma alanı oluşturduğunuzda bir yüksek düzeyde bir [yönetilen Gereci](../managed-applications/overview.md) seçtiğiniz Azure bölgesinde (örneğin, Batı ABD) Aboneliğinize bir Azure kaynağı olarak dağıtılır. Bu gereç dağıtıldığı bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) ile bir [ağ güvenlik grubu](../virtual-network/manage-network-security-group.md) ve aboneliğinizde kullanılabilen bir Azure depolama hesabı. Sanal ağ, Databricks çalışma alanına çevre düzey güvenlik sağlar ve ağ güvenlik grubu korunur. Çalışma alanı içinde çalışan ve sürücü VM türü ve Databricks çalışma zamanı sürümü sağlayarak Databricks kümeler oluşturabilirsiniz. Kalıcı verileri Azure Blob Depolama veya Azure Data Lake Store, depolama hesabınızdaki kullanılabilir. Küme oluşturulduktan sonra belirli bir kümeye ekleyerek dizüstü bilgisayarlar, REST API'ler, ODBC/JDBC uç noktaları aracılığıyla işleri çalıştırabilirsiniz.

Databricks denetim düzlemi yönetir ve Databricks çalışma ortamı izler. Herhangi bir yönetim işlem kümesi oluşturma gibi denetim düzlemi başlatılacak. Zamanlanmış işleri gibi tüm meta veriler, hataya dayanıklılık için coğrafi çoğaltma ile bir Azure veritabanı'nda depolanır.

![Databricks mimarisi](media/howto-regional-disaster-recovery/databricks-architecture.png)

Bu mimarinin avantajı kullanıcılar Azure Databricks hesabında herhangi bir depolama kaynağına bağlanabildiğinizi biridir. Önemli bir avantajı, her ikisi de (Azure Databricks) işlem ve depolama birbirinden bağımsız olarak ölçeklendirilebilir ' dir.

## <a name="how-to-create-a-regional-disaster-recovery-topology"></a>Bölgesel bir olağanüstü durum kurtarma topolojisi oluşturma

Önceki mimari açıklamasında fark gibi çeşitli Azure Databricks ile büyük veri işlem hattı için kullanılan bileşenler vardır:  Azure depolama, Azure veritabanı ve diğer veri kaynakları. Azure Databricks olduğu *işlem* için büyük veri işlem hattı. Bu *kısa ömürlü* verilerinizi Azure Depolama'da hala kullanılabilir olsa da doğası gereği, yani *işlem* (Azure Databricks kümesiyle) sonlandırıldı ödeme yapmak zorunda kalmazsınız ne zaman işlem, gerekmez. *İşlem* (Azure Databricks) ve depolama kaynakları, aynı bölgede olmalıdır, böylece işleri yüksek gecikme deneyimi yok.  

Kendi bölgesel bir olağanüstü durum kurtarma topolojisi oluşturmak için bu gereksinimleri izleyin:

   1. Farklı Azure bölgelerindeki birden çok Azure Databricks çalışma alanı sağlayın. Örneğin, Doğu ABD 2 Birincil Azure Databricks çalışma alanı oluşturun. Batı ABD gibi ayrı bir bölgede ikincil olağanüstü durum kurtarma Azure Databricks çalışma alanı oluşturun.

   2. Kullanım [coğrafi olarak yedekli depolama](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage). İlişkili Azure Databricks varsayılan olarak Azure Depolama'da depolanan veriler. İşlenen verileri, dayanıklı ve küme sonlandırıldıktan sonra yüksek oranda kullanılabilir kalır. böylece, Databricks işleri sonuçlardan de Azure Blob Depolama alanında saklanır. Depolama ve Databricks küme olarak birlikte bulunan, böylece birincil bölgeye artık erişilebilir değilse, ikincil bölgede verilere erişilebilir, coğrafi olarak yedekli depolama kullanmanız gerekir.

   3. İkincil bölgeye oluşturulduktan sonra kullanıcılar, kullanıcı klasörleri, dizüstü bilgisayarlar, küme yapılandırması, işleri yapılandırma, kitaplıkları, depolama, init betikleri geçirme ve erişim denetimi yeniden yapılandırmanız gerekir. Ek Ayrıntılar aşağıdaki bölümde açıklanmıştır.

## <a name="detailed-migration-steps"></a>Ayrıntılı geçiş adımları

1. **Bilgisayarınızda Databricks komut satırı arabirimi ayarlama**

   Bu makale, Azure Databricks REST API'si bir kolayca kullanıcı sarmalayıcı olduğundan otomatik adımlar, çoğu için komut satırı arabirimi kullanan kod örnekleri sayısı.

   Tüm geçiş adımları gerçekleştirmeden önce databricks CLI, masaüstü bilgisayarınızda veya kitaplıklarımızı planladığınız bir sanal makineye yükleyin. Daha fazla bilgi için [Databricks CLI yükleme](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html)

   ```bash
   pip install databricks-cli
   ```

   > [!NOTE]
   > Bu makalede verilen herhangi bir python komut dosyası ile Python 2.7 + çalışması beklenir < 3.x.

2. **İki profillerini yapılandırın.**

   Birincil çalışma alanı için bir tane ve başka bir ikincil çalışma alanı için yapılandırın:

   ```bash
   databricks configure --profile primary
   databricks configure --profile secondary
   ```

   Bu makalede anahtardaki karşılık gelen çalışma komutunu kullanarak her bir sonraki adımda profilleri arasında kod engeller. Oluşturduğunuz profillerinin adlarını her kod bloğu içine başvurulduğunda emin olun.

   ```python
   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"
   ```

   Gerekirse komut satırında el ile geçiş yapabilirsiniz:

   ```bash
   databricks workspace ls --profile primary
   databricks workspace ls --profile secondary
   ```

3. **Azure Active Directory Kullanıcıları geçirme**

   El ile aynı Azure Active Directory Kullanıcıları için birincil çalışma alanında mevcut ikincil çalışma alanına ekleyin.

4. **Dizüstü bilgisayarlar ve kullanıcı klasörleri geçirme**

   İç içe bir klasör yapısını ve kullanıcı başına not defterleri dahil korumalı kullanıcı ortamları geçirmek için aşağıdaki python kodu kullanın.

   > [!NOTE]
   > Bu temel alınan API desteklemediğinden kitaplıkları üzerinden bu adımda, kopyalanmaz.

   Kopyalayın ve aşağıdaki python betiğini bir dosyaya kaydedin ve Databricks komut satırında çalıştırın. Örneğin, `python scriptname.py`.

   ```python
   from subprocess import call, check_output

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get a list of all users
   user_list_out = check_output(["databricks", "workspace", "ls", "/Users", "--profile", EXPORT_PROFILE])
   user_list = user_list_out.splitlines()

   # Export sandboxed environment (folders, notebooks) for each user and import into new workspace.
   # Libraries are not included with these APIs / commands.

   for user in user_list:
     print "Trying to migrate workspace for user " + user

     call("mkdir -p " + user, shell=True)
     export_exit_status = call("databricks workspace export_dir /Users/" + user + " ./" + user + " --profile " + EXPORT_PROFILE, shell=True)

     if export_exit_status==0:
       print "Export Success"
       import_exit_status = call("databricks workspace import_dir ./" + user + " /Users/" + user + " --profile " + IMPORT_PROFILE, shell=True)
       if import_exit_status==0:
         print "Import Success"
       else:
         print "Import Failure"
     else:
       print "Export Failure"

   print "All done"
   ```

5. **Küme yapılandırmalarını geçirme**

   Not defterlerini geçirildikten sonra isteğe bağlı olarak yeni bir çalışma alanı için küme yapılandırmaları geçirebilirsiniz. Seçmeli küme yapılandırma geçişi yapmak istediğiniz sürece, databricks-cli kullanarak neredeyse bir tam otomatik adımdır yerine tüm.

   > [!NOTE]
   > Ne yazık ki oluşturma küme yapılandırma uç nokta yoktur ve bu betiği her küme hemen oluşturmaya çalışır. Aboneliğinizde kullanılabilen yeterli çekirdek yoksa, küme oluşturma işlemi başarısız olabilir. Yapılandırma başarılı bir şekilde aktarılır sürece hata yoksayılabilir.

   Sağlanan aşağıdaki betiği bir eşleme (için daha sonra var olan kümeleri kullanmak üzere yapılandırılmış iş) geçiş işi için kullanılabilecek kimlikleri, yeni küme için eski yazdırır.

   Kopyalayın ve aşağıdaki python betiğini bir dosyaya kaydedin ve Databricks komut satırında çalıştırın. Örneğin, `python scriptname.py`.

   ```python
   from subprocess import call, check_output import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get all clusters info from old workspace 
   clusters_out = check_output(["databricks", "clusters", "list", "--profile", EXPORT_PROFILE]) clusters_info_list = clusters_out.splitlines()

   # Create a list of all cluster ids 
   clusters_list = [] for cluster_info in clusters_info_list:   clusters_list.append(cluster_info.split(None, 1)[0])

   # Optionally filter cluster ids out manually, so as to create only required ones in new workspace

   # Create a list of mandatory / optional create request elements 
   cluster_req_elems = ["num_workers","autoscale","cluster_name","spark_version","spark_conf"," node_type_id","driver_node_type_id","custom_tags","cluster_log_conf","sp ark_env_vars","autotermination_minutes","enable_elastic_disk"]

   # Try creating all / selected clusters in new workspace with same config as in old one.
   cluster_old_new_mappings = {} for cluster in clusters_list:   print "Trying to migrate cluster " + cluster

   cluster_get_out = check_output(["databricks", "clusters", "get", "--cluster-id", cluster, "--profile", EXPORT_PROFILE])
   print "Got cluster config from old workspace"

   # Remove extra content from the config, as we need to build create request with allowed elements only
   cluster_req_json = json.loads(cluster_get_out)    
   cluster_json_keys = cluster_req_json.keys()   

   for key in cluster_json_keys:     
      if key not in cluster_req_elems:       
         cluster_req_json.pop(key, None)
  
   # Create the cluster, and store the mapping from old to new cluster ids
   cluster_create_out = check_output(["databricks", "clusters", "create", "--json", json.dumps(cluster_req_json), "--profile", IMPORT_PROFILE]) 
   cluster_create_out_json = json.loads(cluster_create_out)   
   cluster_old_new_mappings[cluster] = cluster_create_out_json['cluster_id']

   print "Sent cluster create request to new workspace successfully"

   print "Cluster mappings: " + json.dumps(cluster_old_new_mappings)
   print "All done"
   ```

6. **Geçiş işleri yapılandırma**

   Önceki adımda küme yapılandırmalarını geçirdiyseniz, iş yapılandırmalarını yeni bir çalışma alanına geçirmek için seçebilirsiniz. Bu pencere için tüm işleri yapmak yerine seçmeli iş yapılandırma geçişi yapmak istediğiniz sürece databricks-cli kullanarak tam otomatik bir adımdır.

   > [!NOTE]
   > Varsayılan olarak, geçiş yaptı hemen sonra göre yapılandırılmış zamanlama çalışmaya başlar bu nedenle yapılandırmayı zamanlanmış bir iş için "zamanlama" bilgileri de içerir. Bu nedenle, aşağıdaki kod bloğu (eski ve yeni çalışma alanları arasında yinelenen çalıştırmalarını önlemek için) geçiş sırasında herhangi bir zamanlama bilgilerini kaldırır. Hazır olduğunuzda bu tür işlerinin zamanlamalarını yapılandırın tam geçişi.

   Proje yapılandırma ayarları için yeni veya mevcut bir kümeye gerektirir. Mevcut küme kullanıyorsanız, yeni küme kimliğiyle eski küme kimliği değiştirmek aşağıdaki betiği /code dener

   Kopyalayın ve aşağıdaki python betiğini bir dosyaya kaydedin. Değeri Değiştir `old_cluster_id` ve `new_cluster_id`, önceki adımda gerçekleştirilen küme geçişi çıktısı ile. Databricks CLI komut satırınızda, örneğin, çalıştırma `python scriptname.py`.

   ```python
   from subprocess import call, check_output
   import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Please replace the old to new cluster id mappings from cluster migration output
   cluster_old_new_mappings = {"old_cluster_id": "new_cluster_id"}

   # Get all jobs info from old workspace
   try:
     jobs_out = check_output(["databricks", "jobs", "list", "--profile", EXPORT_PROFILE])
     jobs_info_list = jobs_out.splitlines()
   except:
     print "No jobs to migrate"
     sys.exit(0)

   # Create a list of all job ids
   jobs_list = []
   for jobs_info in jobs_info_list:
     jobs_list.append(jobs_info.split(None, 1)[0])

   # Optionally filter job ids out manually, so as to create only required ones in new workspace

   # Create each job in the new workspace based on corresponding settings in the old workspace

   for job in jobs_list:
     print "Trying to migrate " + job

     job_get_out = check_output(["databricks", "jobs", "get", "--job-id", job, "--profile", EXPORT_PROFILE])
     print "Got job config from old workspace"

     job_req_json = json.loads(job_get_out)  
     job_req_settings_json = job_req_json['settings']

     # Remove schedule information so job doesn't start before proper cutover
     job_req_settings_json.pop('schedule', None)

     # Replace old cluster id with new cluster id, if job configured to run against an existing cluster
     if 'existing_cluster_id' in job_req_settings_json:
       if job_req_settings_json['existing_cluster_id'] in cluster_old_new_mappings:
         job_req_settings_json['existing_cluster_id'] = cluster_old_new_mappings[job_req_settings_json['existing_cluster_id']]
       else:
         print "Mapping not available for old cluster id " + job_req_settings_json['existing_cluster_id']
         continue

     call(["databricks", "jobs", "create", "--json", json.dumps(job_req_settings_json), "--profile", IMPORT_PROFILE])
     print "Sent job create request to new workspace successfully"

   print "All done"
   ```

7. **Kitaplıklarının geçişini yapın**

   Şu anda kitaplıkları bir çalışma alanından diğerine geçirmek için kolay bir yolu yoktur. Bunun yerine, bu kitaplıkları yeni çalışma alanına el ile yeniden yükleyin. Bir bileşimini kullanarak otomatik hale getirmek mümkündür [DBFS CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples) çalışma alanına özel kitaplıklarına yüklenecek ve [kitaplıkları CLI](https://github.com/databricks/databricks-cli#libraries-cli).

8. **Azure blob depolama ve Azure Data Lake Store takar geçirme**

   Tüm el ile yeniden bağlamak [Azure Blob Depolama](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html) ve [Azure Data Lake Store (Gen 2)](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html) bağlama noktalarını not defteri tabanlı bir çözüm kullanarak. Depolama kaynaklarını birincil çalışma alanında bağlanmış ve bu ikincil çalışma alanında yinelenmesi gerekir. Hiçbir dış API başlatmalar için yoktur.

9. **Küme init betikleri geçirme**

   Küme başlatma komut dosyalarını kullanarak yeni çalışma alanı eski geçirilebilir [DBFS CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples). İlk olarak, gerekli komut dosyalarından kopyalamak `dbfs:/dat abricks/init/..` yerel Masaüstü veya sanal makine. Ardından, bu komut yeni bir çalışma alanı aynı yola kopyalayın.

   ```bash
   // Primary to local
   dbfs cp -r dbfs:/databricks/init ./old-ws-init-scripts --profile primary

   // Local to Secondary workspace
   dbfs cp -r old-ws-init-scripts dbfs:/databricks/init --profile secondary
   ```

10. **El ile yeniden yapılandırın ve erişim denetimi uygulayın.**

    Var olan, birincil çalışma alanı Premium Katmanı (SKU) kullanmak için yapılandırılmışsa, ayrıca kullandığınız olma olasılığı yüksektir [erişim denetimi özelliği](https://docs.azuredatabricks.net/administration-guide/admin-settings/index.html#manage-access-control).

    El ile erişim denetimi özelliğini kullanırsanız, erişim denetimi (not defterleri, kümeler, işler, tabloları) kaynaklarına yeniden uygulayın.

## <a name="disaster-recovery-for-your-azure-ecosystem"></a>Olağanüstü durum kurtarma için Azure ekosistemi

Diğer Azure Hizmetleri kullanılıyorsa, olağanüstü durum kurtarma için en iyi hizmetlerin çok uygulandığından emin olun. Örneğin, bir dış Hive meta veri deposu örneği kullanmayı seçerseniz, olağanüstü durum kurtarma için dikkate almanız gereken [Azure SQL Server](../sql-database/sql-database-disaster-recovery.md), [Azure HDInsight](../hdinsight/hdinsight-high-availability-linux.md), ve/veya [MySQL için Azure veritabanı ](../mysql/concepts-business-continuity.md). Olağanüstü durum kurtarma hakkında genel bilgi için bkz: [Azure uygulamaları için olağanüstü durum kurtarma](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Azure Databricks belgeleri](https://docs.azuredatabricks.net/user-guide/index.html).
