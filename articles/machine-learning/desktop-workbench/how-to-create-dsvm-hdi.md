---
title: DSVM ve HDI hedefleri için Azure ML işlem olarak oluşturma
description: Azure ML denemesi için hedef işlem olarak DSVM ve HDI Spark kümesi oluşturun.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 211f60b9c25b4bd20769f6a4840afaecf8373b9f
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782354"
---
# <a name="create-dsvm-and-hdi-spark-cluster-as-compute-targets"></a>Hedef işlem olarak DSVM ve HDI Spark kümesi oluşturma

Kolayca artırın veya Azure HDInsight kümesi için Ubuntu tabanlı DSVM (veri bilimi sanal makinesi) ve Apache Spark gibi ek işlem hedefleri ekleyerek machine learning denemenizi ölçeklendirin. Bu makalede Yürüyüşü bunları oluşturmaya yönelik adımlar boyunca size Azure hedef işlem. Azure ML işlem hedefleri üzerinde daha fazla bilgi için [Azure Machine Learning deneme hizmeti'ne genel bakış](experimentation-service-configuration.md).

>[!NOTE]
>VM ve HDI kümeleri gibi kaynakları oluşturmak için uygun izinlere sahip olmak gerekir. devam etmeden önce azure'da. Ayrıca bu kaynakların her ikisi de yapılandırmanıza bağlı olarak çok sayıda işlem çekirdeği kullanabilir. Aboneliğiniz sanal CPU çekirdekleri için yeterli kapasiteye sahip olduğundan emin olun. Ayrıca, bir abonelikte izin verilen çekirdek sayısını artırmak için Azure desteğine başvurun her zaman alabilir.

## <a name="create-an-ubuntu-dsvm-in-azure-portal"></a>Azure portalında bir Ubuntu DSVM oluşturma

Azure portalından bir DSVM oluşturabilirsiniz. 

1. Azure portalında oturum açın https://portal.azure.com
2. Tıklayarak **+ yeni** bağlantı ve "veri bilimi sanal makinesi Linux için" arayın.
    ![Ubuntu](media/how-to-create-dsvm-hdi/ubuntu_dsvm.png)
4. Seçin **Linux (Ubuntu) için veri bilimi sanal makinesi** listesi ve izleme ekran DSVM oluşturma için yönergeler.

>[!IMPORTANT]
>Seçtiğinizden emin olun **parola** olarak _kimlik doğrulama türü_.

![PWD kullanın](media/how-to-create-dsvm-hdi/use_pwd.png)

## <a name="create-an-ubuntu-dsvm-using-azure-cli"></a>Azure CLI kullanarak bir Ubuntu DSVM oluşturma

Bir Azure kaynak yönetimi şablonu ayrıca bir DSVM dağıtmak için de kullanabilirsiniz.

>[!NOTE]
>Aşağıdaki komutların bir Azure ML projesinin kök klasöründen verilmesi varsayılır.

İlk olarak, oluşturun bir `mydsvm.json` metin düzenleyiciyi kullanarak dosya `docs` klasör. (Sahip değilseniz bir `docs` proje kök klasöründeki bir klasör oluşturun.) Azure kaynak yönetimi şablonu için bazı temel parametreleri yapılandırmak için bu dosyayı kullanın. 

İçine aşağıdaki JSON kod parçacığını kopyalayıp `mydsvm.json` dosya ve uygun değerleri doldurun:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "<admin username>"},
     "adminPassword": { "value" : "<admin password>"},
     "vmName": { "value" : "<vm name>"},
     "vmSize": { "value" : "<vm size>"}
  }
}
```

İçin _vmSize_ alanında listelenen tüm suppported VM boyutları kullanabilirsiniz [Ubuntu DSVM Azure kaynak yönetimi şablonu](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/multiazuredeploywithext.json). Aşağıdakilerden birini kullanmanızı öneririz hedefleri için Azure ML işlem olarak boyutları altında. 


>[!TIP]
> İçin [derin öğrenme iş yüklerinin](how-to-use-gpu.md) dağıtabileceğiniz GPU desteklenen VM'ler.

- [Genel amaçlı sanal makineleri](../../virtual-machines/linux/sizes-general.md)
  - Standard_DS2_v2 
  - Standard_DS3_v2 
  - Standard_DS4_v2 
  - Standard_DS12_v2 
  - Standard_DS13_v2 
  - Standard_DS14_v2 
- [GPU desteklenen VM'ler](../../virtual-machines/linux/sizes-gpu.md)
  - Standard_NC6 
  - Standard_NC12 
  - Standard_NC24 
 

Bunlar hakkında daha fazla bilgiyi [azure'da Linux sanal makine boyutları](../../virtual-machines/linux/sizes.md) ve bunların [fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

CLI penceresine tıklayarak Azure ML Workbench uygulamasından başlatma **dosya** --> **komut istemini Aç**, veya **açık PowerShell** menü öğesi. 

>[!NOTE]
>Ayrıca az CLI yüklü olduğu tüm komut satırı ortamında bunu yapabilirsiniz.

Komut satırı penceresinde girin aşağıdaki komutları:

```azurecli
# first make sure you have a valid Azure authentication token
$ az account get-access-token

# if you don't have a valid token, please log in to Azure first. 
# if you already do, you can skip this step.
$ az login

# list all subscriptions you have access to
$ az account list -o table

# make sure you set the subscription you want to use to create DSVM as the current subscription
$ az account set -s <subscription name or Id>

# it is always a good idea to create a resource group for the VM and associated resources to live in.
# you can use any Azure region, but it is best to create them in the region where your Azure ML Experimentation account is, e.g. eastus2, westcentralus or australiaeast.
# also, only certain Azure regions has GPU-equipped VMs available.
$ az group create -n <resource group name> -l <azure region>

# now let's create the DSVM based on the JSON configuration file you created earlier.
# note we assume the mydsvm.json config file is placed in the "docs" sub-folder.
$ az group deployment create -g <resource group name> --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters @docs/mydsvm.json

# find the FQDN (fully qualified domain name) of the VM just created. 
# you can also use IP address from the next command if FQDN is not set.
$ az vm show -g <resource group name> -n <vm name> --query "fqdns"

# find the IP address of the VM just created
$ az vm show -g <resource group name> -n <vm name> --query "publicIps"
```
## <a name="attach-a-dsvm-compute-target"></a>DSVM işlem hedefi ekleme
DSVM oluşturulduktan sonra artık Azure ML projenize ekleyebilirsiniz.

```azurecli
# attach the DSVM compute target
# it is a good idea to use FQDN in case the IP address changes after you deallocate the VM and restart it
$ az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password> 

# prepare the Docker image on the DSVM 
$ az ml experiment prepare -c <compute target name>
```
Artık bu DSVM'nin denemeleri çalıştırmak hazır olmanız gerekir.

## <a name="deallocate-a-dsvm-and-restart-it-later"></a>Bir DSVM serbest bırakın ve daha sonra yeniden başlatın
Azure ML işlem görevler bitirdikten sonra DSVM serbest bırakın. Bu eylem, işlem kaynaklarını serbest bırakır VM'yi kapatır, ancak sanal diskleri korur. VM serbest bırakıldığında işlem maliyeti için ücretlendirilmez.

Bir VM'yi serbest bırakma için:

```azurecli
$ az vm deallocate -g <resource group name> -n <vm name>
```

VM'yi geri hayata geçirmek için kullanmak `az ml start` komutu:

```azurecli
$ az vm start -g <resource group name> -n <vm name>
```

## <a name="expand-the-dsvm-os-disk"></a>DSVM işletim sistemi diskini genişletme
Ubuntu DSVM bir işletim sistemi disk 50 GB ve 100 GB veri diski ile birlikte gelir. Daha fazla alan kullanılabilir olduğundan docker resimlerini verileri diskte depolar. Azure ML için hedef işlem olarak kullanıldığında, bu disk Docker görüntülerini çekme ve bunun üstünde conda Katmanlar oluşturma Docker altyapısı tarafından kullanılabilir. Bir yürütme ortasında çalışırken "tam disk" hatasını önlemek için daha büyük bir boyuta (örneğin, 200 GB) disk genişletmeniz gerekebilir. Başvuru [nasıl genişletileceği Azure CLI ile Linux sanal makinesinde sanal sabit diskleri](../../virtual-machines/linux/expand-disks.md) kolayca azure clı'dan bunun nasıl yapılacağını öğrenin. 

## <a name="create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal"></a>Azure portalını kullanarak Azure HDInsight kümesi için bir Apache Spark oluşturma

Ölçek genişletme Spark işleri çalıştırma için Azure portalını kullanarak Azure HDInsight kümesi için bir Apache Spark oluşturma gerekir.

1. Azure portalında oturum açın https://portal.azure.com
2. Tıklayarak **+ yeni** bağlantı ve "HDInsight" arayın.

    ![hdı bulun](media/how-to-create-dsvm-hdi/hdi.png)
    
3. Seçin **HDInsight** listesini ve ardından şirket **Oluştur** düğmesi.
4. İçinde **Temelleri** yapılandırma ekranında, **küme türü** ayarlarını seçtiğinizden emin olun **Spark** olarak _küme türü_, **Linux** olarak _işletim sistemi_, ve **Spark 2.1.0 (HDI 3.6)** _sürüm olarak.

    ![hdı yapılandırın](media/how-to-create-dsvm-hdi/configure_hdi.png)

    >[!IMPORTANT]
    >Yukarıdaki ekran bildirim küme sahip bir _küme oturum açma kullanıcı_ alan ve _güvenli Kabuk (SSH) kullanıcı adı_ alan. Kolaylık sağlamak için her iki oturum açma bilgileri için aynı parolayı belirtebilirsiniz olsa da bunlar iki farklı kullanıcı kimlikleri olur. _Küme oturum açma kullanıcı_ Yönetim web kullanıcı Arabirimine HDI küme oturum açmak için kullanılır. _SSH oturum açma kullanıcı adı_ kümenin baş düğümüne oturum açmak için kullanılır ve Spark işleri göndermek Azure ML için gereken budur.

5. Düğüm boyutu gerekiyor ve Oluşturma Sihirbazı'nı tamamlamak ve küme boyutu seçin. Bu kümenin sağlama tamamlanması 30 dakika kadar sürebilir. 

## <a name="attach-an-hdi-spark-cluster-compute-target"></a>Bir HDI Spark kümesi işlem hedefi ekleme

HDI Spark kümesi oluşturulduktan sonra artık Azure ML projenize ekleyebilirsiniz.

```azurecli
# attach the HDI compute target
$ az ml computetarget attach cluster --name <compute target name> --address <cluster name, such as myhdicluster123.azurehdinsight.net> --username <ssh username> --password <ssh password> 

# prepare the conda environment on HDI
$ az ml experiment prepare -c <compute target name>
```
Artık bu Spark kümesinde denemeleri çalıştırmak hazır olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:
- [Azure Machine Learning deneme hizmeti'ne genel bakış](experimentation-service-configuration.md)
- [Azure Machine Learning Workbench deneme hizmeti yapılandırma dosyaları](experimentation-service-configuration-reference.md)
- [Azure HDInsight kümesi için Apache Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/)
- [Veri bilimi sanal makinesi](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)
