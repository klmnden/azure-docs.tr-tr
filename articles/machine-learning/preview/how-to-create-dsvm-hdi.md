---
title: "DSVM ve hedefler için Azure ML işlem olarak HDI oluşturma"
description: "Azure ML deneme hedefleri işlem olarak DSVM ve HDI Spark kümesi oluşturun."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: c28d92ad23e42401d42d9510fd7d07429929ade7
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="create-dsvm-and-hdi-spark-cluster-as-compute-targets"></a>Hedefleri işlem olarak DSVM ve HDI Spark kümesi oluşturma

Kolayca artırın veya Azure Hdınsight kümesi için ek işlem hedefleri Ubuntu tabanlı DSVM (veri bilimi sanal makine) ve Apache Spark gibi ekleyerek machine learning denemenizi ölçeklendirin. Bu makale, bunlar oluşturma adımları boyunca Azure hedeflerini işlem yetenekte. Azure ML işlem hedefleri hakkında daha fazla bilgi için bkz [Azure Machine Learning deneme hizmetine genel bakış](experimentation-service-configuration.md).

>[!NOTE]
>Kaynak VM ve HDI kümeleri gibi oluşturmak için uygun izinlere sahip olmanız gerekir, devam etmeden önce Azure. Ayrıca bu kaynakları her ikisi de yapılandırmanıza bağlı olarak çok sayıda işlem çekirdek kullanmasını sağlayabilirsiniz. Aboneliğiniz sanal CPU çekirdekleri için yeterli kapasitesi olduğundan emin olun. Çekirdek aboneliğinizde izin verilen maksimum sayısını artırmak için Azure desteğine başvurun her zaman alabilir.

## <a name="create-an-ubuntu-dsvm-in-azure-portal"></a>Azure portalında bir Ubuntu DSVM oluşturma

Azure portalından bir DSVM oluşturabilirsiniz. 

1. Https://portal.azure.com Azure portalında oturum açın
2. Tıklayın **+ yeni** bağlantı ve "veri bilimi için sanal makine Linux" arayın.
    ![Ubuntu](media/how-to-create-dsvm-hdi/ubuntu_dsvm.png)
4. Seçin **Linux (Ubuntu) için veri bilimi sanal makine** listesi ve izleme ekran DSVM oluşturmaya ilişkin yönergeler.

>[!IMPORTANT]
>Seçtiğiniz emin olun **parola** olarak _kimlik doğrulama türü_.

![PWD kullanın](media/how-to-create-dsvm-hdi/use_pwd.png)

## <a name="create-an-ubuntu-dsvm-using-azure-cli"></a>Azure CLI kullanarak bir Ubuntu DSVM oluşturma

Bir Azure kaynak yönetimi şablonu, bir DSVM dağıtmak için de kullanabilirsiniz.

>[!NOTE]
>Tüm aşağıdaki komutlar bir Azure ML proje kök klasörden verilmesi varsayılır.

İlk olarak, oluşturma bir `mydsvm.json` sık kullandığınız metin düzenleyicinizde kullanarak dosya `docs` klasör. (Elinizde yoksa bir `docs` klasörü proje kök klasöründe bir Web sitesi oluşturun.) Azure kaynak yönetimi şablonu için temel bazı parametreleri yapılandırmak için bu dosyayı kullanırız. 

İçine aşağıdaki JSON parçacığını kopyalayıp `mydsvm.json` dosya ve uygun değerleri doldurun:

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

İçin _vmSize_ alan, listelenen tüm suppported VM boyutu kullanabilir [Ubuntu DSVM Azure kaynak yönetimi şablonu](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/multiazuredeploywithext.json). Aşağıdakilerden birini kullanmanızı öneririz hedefleri için Azure ML işlem olarak boyutları aşağıda. 


>[!TIP]
> İçin [derin iş yükleri öğrenme](how-to-use-gpu.md) dağıtabileceğiniz GPU VM'ler sağlanmıştır.

- [Genel amaçlı VM'ler](/virtual-machines/linux/sizes-general.md)
  - Standard_DS2_v2 
  - Standard_DS3_v2 
  - Standard_DS4_v2 
  - Standard_DS12_v2 
  - Standard_DS13_v2 
  - Standard_DS14_v2 
- [GPU VM'ler güç kaynağı](/virtual-machines/linux/sizes-gpu.md)
  - Standard_NC6 
  - Standard_NC12 
  - Standard_NC24 
 

Bunlar hakkında daha fazla bilgiyi [azure'daki Linux sanal makineler için Boyutlar](../../virtual-machines/linux/sizes.md) ve bunların [fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

CLI penceresi tıklayarak Azure ML çalışma ekranı uygulama başlatma **dosya** --> **komut istemini açın**, veya **açık PowerShell** menü öğesi. 

>[!NOTE]
>Bu az CLI yüklü olduğu tüm komut satırı ortamında da yapabilirsiniz.

Komut satırı penceresinde girdiğiniz komutlar aşağıda:

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
## <a name="attach-a-dsvm-compute-target"></a>DSVM işlem hedef ekleme
DSVM oluşturulduktan sonra artık Azure ML projenize ekleyebilirsiniz.

```azurecli
# attach the DSVM compute target
# it is a good idea to use FQDN in case the IP address changes after you deallocate the VM and restart it
$ az ml computetarget attach --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password> --type remotedocker

# prepare the Docker image on the DSVM 
$ az ml experiment prepare -c <compute target name>
```
Artık bu DSVM üzerinde denemeler çalıştırılmaya hazır olması gerekir.

## <a name="deallocate-a-dsvm-and-restart-it-later"></a>Bir DSVM ayırması ve daha sonra yeniden başlatın
Azure ML işlem görevleri tamamladığınızda, DSVM serbest bırakma. Bu eylem kapatır VM, işlem kaynaklarını serbest bırakır, ancak sanal diskleri korur. VM serbest bırakıldığında işlem maliyetini ücretlendirilen değil.

Bir VM serbest bırakma için:

```azurecli
$ az vm deallocate -g <resource group name> -n <vm name>
```

VM yaşama geri getirmek için kullanmak `az ml start` komutu:

```azurecli
$ az vm start -g <resource group name> -n <vm name>
```

## <a name="expand-the-dsvm-os-disk"></a>DSVM işletim sistemi diski Genişlet
Azure Linux VM'de genellikle 30 GB işletim sistemi diski ile birlikte gelir. Hedef için Azure ML işlem olarak kullanıldığında, hızlı bir şekilde Docker görüntüleri çekmek ve onu üstünde conda Katmanlar oluşturma Docker altyapısı tarafından eaten. İşletim sistemi diski (örneğin, 200 GB) daha büyük boyutta bir yürütme ortasında çalışırken "tam disk" hatasını önlemek için genişletmek için iyi bir fikirdir. Başvuru [bir Linux VM Azure CLI ile sanal sabit disklerde genişletmek nasıl](../../virtual-machines/linux/expand-disks.md) azure-cli bunu kolayca öğrenin. 

## <a name="create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal"></a>Azure portalında bir Apache Spark Azure Hdınsight kümesi için oluşturma

Genişleme Spark işlerini çalıştırmak için Azure portalında Azure Hdınsight kümesi için bir Apache Spark oluşturmanız gerekir.

1. Https://portal.azure.com Azure portalında oturum açın
2. Tıklayın **+ yeni** bağlantı ve "Hdınsight" arayın.

    ![hdı Bul](media/how-to-create-dsvm-hdi/hdi.png)
    
3. Seçin **Hdınsight** listesi ve ardından üzerinde **oluşturma** düğmesi.
4. İçinde **Temelleri** yapılandırma ekranında, **küme türü** ayarları, seçtiğiniz emin olun **Spark** olarak _küme türü_, **Linux** olarak _işletim sistemi_, ve **Spark 2.1.0 (HDI 3.6)** _Version olarak.

    ![hdı yapılandırın](media/how-to-create-dsvm-hdi/configure_hdi.png)

    >[!IMPORTANT]
    >Yukarıdaki ekran bildirimde küme sahip bir _küme oturum açma kullanıcı_ alan ve _güvenli Kabuk (SSH) kullanıcıadı_ alan. Kolaylık sağlamak için her iki oturum açma için aynı parolayı belirtebilirsiniz olsa da iki farklı kullanıcı kimlikleri, bunlar. _Küme oturum açma kullanıcı_ Yönetim web kullanıcı Arabirimine HDI kümesi oturum açmak için kullanılır. _SSH oturum açma kullanıcı_ küme baş düğümüne oturum açmak için kullanılır ve Azure ML Spark işlerinin gönderilmesi gereken budur.

5. Küme boyutu ve gerek ve Oluşturma Sihirbazı'nı bitirmek düğüm boyutu seçin. Kümenin sağlama son 30 dakika kadar sürebilir. 

## <a name="attach-an-hdi-spark-cluster-compute-target"></a>Bir HDI Spark küme işlem hedef ekleme

Spark HDI kümesi oluşturulduktan sonra artık Azure ML projenize ekleyebilirsiniz.

```azurecli
# attach the HDI compute target
$ az ml computetarget attach --name <compute target name> --address <cluster name, such as myhdicluster123.azurehdinsight.net> --username <ssh username> --password <ssh password> --type cluster

# prepare the conda environment on HDI
$ az ml experiment prepare -c <compute target name>
```
Artık bu Spark kümesi üzerinde denemeler çalıştırılmaya hazır olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin:
- [Azure Machine Learning deneme hizmetine genel bakış](experimentation-service-configuration.md)
- [Azure Machine Learning çalışma ekranı deneme hizmeti yapılandırma dosyaları](experimentation-service-configuration-reference.md)
- [Azure Hdınsight kümesi için Apache Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/)
- [Veri bilimi sanal makine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)
