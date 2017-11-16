---
title: "Azure Machine Learning modeli yönetim komut satırı arabirimi başvurusu | Microsoft Docs"
description: "Azure Machine Learning modeli yönetim komut satırı arabirimi başvurusu."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/08/2017
ms.openlocfilehash: 373abb8f40a8acf557b7cd4a0d0b3fb55f4a545c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="model-management-command-line-interface-reference"></a>Model yönetim komut satırı arabirimi başvurusu

## <a name="base-cli-concepts"></a>Temel CLI Kavramları:

    account : Manage model management accounts. 
    env     : Manage compute environments.
    image   : Manage operationalization images.
    manifest: Manage operationalization manifests.
    model   : Manage operationalization models.
    service : Manage operationalized services.

## <a name="account-commands"></a>Hesap komutları
Bir model yönetim hesabı dağıtmak ve modelleri yönetmek izin hizmetleri kullanmak için gereklidir. Kullanım `az ml account modelmanagement -h` aşağıdaki listesini görmek için:

    create: Create a Model Management Account.
    delete: Delete a specified Model Management Account.
    list  : Gets the Model Management Accounts in the current subscriptiong.
    set   : Set the active Model Management Account.
    show  : Show a Model Management Account.
    update: Update an existing Model Management Account.

**Model yönetim hesabı oluşturun**

Aşağıdaki komutu kullanarak bir model yönetim hesabı oluşturun. Bu hesap için fatura kullanılır.

`az ml account modelmanagement create --location [Azure region e.g. eastus2] --name [new account name] --resource-group [resource group name to store the account in]`

Yerel değişkenleri:

    --location -l       [Required]: Resource location.
    --name -n           [Required]: Name of the model management account.
    --resource-group -g [Required]: Resource group to create the model management account in.
    --description -d              : Description of the model management account.
    --sku-instances               : Number of instances of the selected SKU. Must be between 1 and
                                    16 inclusive.  Default: 1.
    --sku-name                    : SKU name. Valid names are S1|S2|S3|DevTest.  Default: S1.
    --tags -t                     : Tags for the model management account.  Default: {}.
    -v                            : Verbosity flag.

## <a name="environment-commands"></a>Ortam komutları

    cluster        : Switch the current execution context to 'cluster'.
    delete         : Delete an MLCRP-provisioned resource.
    get-credentials: List the keys for an environment.
    list           : List all environments in the current subscription.
    local          : Switch the current execution context to 'local'.
    set            : Set the active MLC environment.
    setup          : Sets up an MLC environment.
    show           : Show an MLC resource; if resource_group or cluster_name are not provided, shows
                     the active MLC env.

**Dağıtım ortamını ayarlama**

Kurulum komutu katkıda bulunan abonelik erişimi gerektirir. Yoksa, en az içine dağıtıyorsanız kaynak grubuna katkıda bulunan erişmeniz gerekir. İkinci yapmak için kurulum komutunu kullanarak bir parçası olarak kaynak grubu adı belirtmeniz gerekir `-g` bayrağı. 

Dağıtımı için iki seçenek vardır: *yerel* ve *küme*. Ayarı `--cluster` (veya `-c`) bayrağı ACS küme hazırlar Küme dağıtımı sağlar. Temel kurulum söz dizimi aşağıdaki gibidir:

`az ml env setup [-c] --location [location of environment resources] --name[name of environment]`

Bu depolama hesabı, ACR kayıt defteri ve App Insights hizmeti, aboneliğinizde oluşturuldu ortamıyla öğrenme makinenizi Azure başlatır. Bayrak belirtilmezse varsayılan olarak, ortamın yerel dağıtımları için yalnızca (ACS yok) başlatıldı. Hizmeti ölçeklendirmek gerekiyorsa, belirtin `--cluster` (veya `-c`) bir ACS küme oluşturmak için bayrak.

Komut ayrıntıları:

    --location -l        [Required]: Location for environment resources; an Azure region, e.g. eastus2.
    --name -n            [Required]: Name of environment to provision.
    --acr -r                       : ARM ID of ACR to associate with this environment.
    --agent-count -z               : Number of agents to provision in the ACS cluster. Default: 3.
    --cert-cname                   : CNAME of certificate.
    --cert-pem                     : Path to .pem file with certificate bytes.
    --cluster -c                   : Flag to provision ACS cluster. Off by default; specify this to force an ACS cluster deployment.
    --key-pem                      : Path to .pem file with certificate key.
    --master-count -m              : Number of master nodes to provision in the ACS cluster. Acceptable values: 1, 3, 5. Default: 1.
    --resource-group -g            : Resource group in which to create compute resource. Will be created if it does not exist.
                                     If not provided, resource group will be created with 'rg' appended to 'name.'.
    --service-principal-app-id -a  : App ID of service principal to use for configuring ML compute.
    --service-principal-password -p: Password associated with service principal.
    --storage -s                   : ARM ID of storage account to associate with this environment.
    --yes -y                       : Flag to answer 'yes' to any prompts. Command will fail if user is not logged in.

Genel bağımsız değişkenler
```
    --debug                        : Increase logging verbosity to show all debug logs.
    --help -h                      : Show this help message and exit.
    --output -o                    : Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query                        : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose                      : Increase logging verbosity. Use --debug for full debug logs.
```
## <a name="model-commands"></a>Model komutları

    list
    register
    show

**Bir model Kaydet**

Model kaydetmek için komutu.

`az ml model register --model [path to model file] --name [model name]`

Komut ayrıntıları:

    --model -m [Required]: Model to register.
    --name -n  [Required]: Name of model to register.
    --description -d     : Description of the model.
    --tag -t             : Tags for the model. Multiple tags can be specified with additional -t arguments.
    -v                   : Verbosity flag.

Genel bağımsız değişkenler

    --debug              : Increase logging verbosity to show all debug logs.
    --help -h            : Show this help message and exit.
    --output -o          : Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query              : JMESPath query string. See http://jmespath.org/ for more information and
                           examples.
    --verbose            : Increase logging verbosity. Use --debug for full debug logs.

## <a name="manifest-commands"></a>Bildirim komutları

    create: Create an Operationalization Manifest. This command has two different
            sets of required arguments, depending on if you want to use previously registered
            model/s.
    list
    show

**Bildirimi oluşturma**

Model için bir bildirim dosyası oluşturur. 

`az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]`

Komut ayrıntıları:

    --manifest-name -n [Required]: Name of the manifest to create.
    -f                 [Required]: The code file to be deployed.
    -r                 [Required]: Runtime of the web service. Valid runtimes are spark-py|python.
    --conda-file -c              : Path to Conda Environment file.
    --dependency -d              : Files and directories required by the service. Multiple
                                   dependencies can be specified with additional -d arguments.
    --manifest-description       : Description of the manifest.
    --schema-file -s             : Schema file to add to the manifest.
    -p                           : A pip requirements.txt file needed by the code file.
    -v                           : Verbosity flag.

Kayıtlı modeli bağımsız değişkenler

    --model-id -i                : [Required] Id of previously registered model to add to manifest.
                                   Multiple models can be specified with additional -i arguments.

Kaydı modeli bağımsız değişkenler

    --model-file -m              : [Required] Model file to register. If used, must be combined with
                                   model name.

Genel bağımsız değişkenler

    --debug                      : Increase logging verbosity to show all debug logs.
    --help -h                    : Show this help message and exit.
    --output -o                  : Output format.  Allowed values: json, jsonc, table, tsv.
                                   Default: json.
    --query                      : JMESPath query string. See http://jmespath.org/ for more
                                   information and examples.
    --verbose                    : Increase logging verbosity. Use --debug for full debug logs.


## <a name="image-commands"></a>Görüntü komutları

    create: Creates a docker image with the model and its dependencies. This command has two different sets of
            required arguments, depending on if you want to use a previously created manifest.
    list
    show
    usage

**Görüntü oluşturma**

Önce kendi bildirimi oluşturduktan seçeneği bir görüntü oluşturabilirsiniz. 

`az ml image create -n [image name] --manifest-id [the manifest ID]`

Veya bildirimi oluşturma ve tek bir komutla görüntü. 

`az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]`

Komut ayrıntıları:

    --image-name -n [Required]: The name of the image being created.
    --image-description       : Description of the image.
    --image-type              : The image type to create. Defaults to "Docker".
    -v                        : Verbosity flag.

Kayıtlı bildirim bağımsız değişkenler

    --manifest-id             : [Required] Id of previously registered manifest to use in image creation.

Kaydı bildirim bağımsız değişkenler

    --conda-file -c           : Path to Conda Environment file.
    --dependency -d           : Files and directories required by the service. Multiple dependencies can
                                be specified with additional -d arguments.
    --model-file -m           : [Required] Model file to register.
    --schema-file -s          : Schema file to add to the manifest.
    -f                        : [Required] The code file to be deployed.
    -p                        : A pip requirements.txt file needed by the code file.
    -r                        : [Required] Runtime of the web service. Valid runtimes are python|spark-py.


## <a name="service-commands"></a>Hizmeti komutları
Aşağıdaki komutları hizmeti için desteklenir. Her komut parametreleri görmek için -h seçeneğini kullanın. Örneğin, `az ml service create realtime -h` görmek için komutu ayrıntıları oluşturun.

    create
    delete
    keys
    list
    logs
    run
    show
    update
    usage

**Hizmet oluşturma**

Önceden oluşturulmuş bir görüntüyle bir hizmet oluşturmak için aşağıdaki komutu kullanın:

`az ml service create realtime --image-id [image to deploy] -n [service name]`

Bir hizmet oluşturmak için bildirim ve tek bir komut görüntüsüyle aşağıdaki komutu kullanın:

`az ml service create realtime --model-file [path to model file(s)] -f [path to model scoring file, e.g. score.py] -n [service name] -r [run time included in the image, e.g. spark-py]`

Komutları ayrıntıları:

    -n                                : [Required] Webservice name.
    --autoscale-enabled               : Enable automatic scaling of service replicas based on request demand.
                                        Allowed values: true, false. False if omitted.  Default: false.
    --autoscale-max-replicas          : If autoscale is enabled - sets the maximum number of replicas.
    --autoscale-min-replicas          : If autoscale is enabled - sets the minimum number of replicas.
    --autoscale-refresh-period-seconds: If autoscale is enabled - the interval of evaluating scaling demand.
    --autoscale-target-utilization    : If autoscale is enabled - target utilization of replicas time.
    --collect-model-data              : Enable model data collection. Allowed values: true, false. False if omitted.  Default: false.
    --cpu                             : Reserved number of CPU cores per service replica (can be fraction).
    --enable-app-insights -l          : Enable app insights. Allowed values: true, false. False if omitted.  Default: false.
    --memory                          : Reserved amount of memory per service replica, in M or G. (ex. 1G, 300M).
    --replica-max-concurrent-requests : Maximum number of concurrent requests that can be routed to a service replica.
    -v                                : Verbosity flag.
    -z                                : Number of replicas for a Kubernetes service.  Default: 1.

Kayıtlı görüntü bağımsız değişkenler

    --image-id                        : [Required] Image to deploy to the service.

Kaydı görüntü bağımsız değişkenler

    --conda-file -c                   : Path to Conda Environment file.
    --image-type                      : The image type to create. Defaults to "Docker".
    --model-file -m                   : [Required] The model to be deployed.
    -d                                : Files and directories required by the service. Multiple dependencies can be specified
                                        with additional -d arguments.
    -f                                : [Required] The code file to be deployed.
    -p                                : A pip requirements.txt file of package needed by the code file.
    -r                                : [Required] Runtime of the web service. Valid runtimes are python|spark-py.
    -s                                : Input and output schema of the web service.

Genel bağımsız değişkenler

    --debug                           : Increase logging verbosity to show all debug logs.
    --help -h                         : Show this help message and exit.
    --output -o                       : Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query                           : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose                         : Increase logging verbosity. Use --debug for full debug logs.


Üzerinde Not `-d` bağımlılıkları ekleme bayrağı: adını geçirirseniz zaten bir dizin paketlenmiş (ZIP, tar, vb.), bu dizine otomatik olarak tar'ed alır ve bunların sonra otomatik olarak diğer tarafta unbundled geçirilir. 

Önceden paketlenmiş bir dizinde geçirirseniz, biz bir dosya olarak ele alın ve bunu boyunca olarak geçirin. Bu otomatik olarak unbundled olmaz; Kodunuzda işleyen beklenir.

**Hizmet Ayrıntıları**

URL, kullanımı (bir şema oluşturduysanız örnek veriler dahil) dahil olmak üzere hizmet ayrıntıları alın.

`az ml service show realtime --name [service name]`

Komut ayrıntıları:

    --id -i    : The service id to show.
    --name -n  : Webservice name.
    -v         : Verbosity flag.

Genel bağımsız değişkenler

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.

**Hizmetin çalıştırılması**

`az ml service run realtime -n [service name] -d [input_data]`

Komut ayrıntıları:

    --id -i    : The service id to show.
    --name -n  : Webservice name.
    -v         : Verbosity flag.

Genel bağımsız değişkenler

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.

Komut

    az ml service run realtime

Bağımsız değişkenler--kimliği -i: [gerekli] karşı Puanlama amacıyla hizmet kimliği.
-d: web hizmetini çağırmak için kullanılacak veri.
-v: ayrıntı bayrağı.

Genel bağımsız değişkenler

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.
