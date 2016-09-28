<properties
    pageTitle="Linux VM oluşturmanın farklı yolları | Microsoft Azure"
    description="Azure üzerinde bir Linux sanal makine oluşturmanın farklı yollarını listeler ve her bir yöntemin araç ve öğreticilerinin bağlantılarını verir."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="iainfou"/>


# Resource Manager ile Linux sanal makine oluşturmanın farklı yolları

Farklı kullanıcılara ve amaçlara uyması için Azure Resource Manager geliştirme modelini kullanarak bir VM oluşturmanın farklı yollarını sunar. Bu makale söz konusu farkları ve kendi Linux sanal makinelerinizi (VM’ler) oluşturmak için yapabileceğiniz seçimleri özetler.

## Azure CLI 

Azure CLI bir npm paketi, distro ile sağlanan paketler veya Docker kapsayıcısı üzerinden çeşitli platformlarda kullanılabilir. [Azure CLI yükleme ve yapılandırma](../xplat-cli-install.md) hakkında daha fazla bilgi alabilirsiniz. Aşağıdaki öğretiler Azure CLI kullanma ile ilgili örnekler sunar. Gösterilen CLI hızlı başlatma komutlarına ilişkin daha fazla ayrıntı için her bir makaleyi okuyun:

* [Geliştirme ve test için Azure CLI’dan bir Linux VM oluşturma](virtual-machines-linux-quick-create-cli.md)

    ```bash
    azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
    ```

* [Bir Azure şablonu kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

* [Azure CLI kullanarak sıfırdan bir Linux VM oluşturma](virtual-machines-linux-create-cli-complete.md)

* [Linux VM'ye disk ekleme](virtual-machines-linux-add-disk.md)

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM <size-in-GB>
    ```

## Azure portal

[Azure portal](https://portal.azure.com)’ın grafik kullanıcı arabirimi, özellikle Azure ile yeni başlıyorsanız, sisteminize yüklenecek hiç bir şey olmadığından bir VM’i denemek için basit bir yol sunar. VM oluşturmak için Azure portalını kullanma:

* [Azure portal kullanarak Linux VM oluşturma](virtual-machines-linux-quick-create-portal.md) 
* [Azure portal kullanarak disk bağlama](virtual-machines-linux-attach-disk-portal.md)

## İşletim sistemi ve görüntü seçimi
Bir sanal makine oluşturulurken çalıştırmak istediğiniz işletim sistemini temel alan bir görüntü seçebilirsiniz. Azure ve ortakları, bazıları önyüklü uygulamalar ve araçlarla gelen çok sayıda görüntü sağlar. Veya kendi görüntülerinizden birini yükleyebilirsiniz (aşağıya bakın).

### Azure görüntüleri
Yayımcı, distro sürümü ve derlemelere göre kullanılabilir seçenekleri görmek için `azure vm image` CLI komutlarını kullanabilirsiniz.

Kullanılabilir yayımcıların listesi:

```bash
azure vm image list-publishers --location WestUS
```

Belirli bir yayımcının kullanılabilir ürünlerinin (teklifler) listesi:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Belirli bir teklif için kullanılabilir SKU’ların (distro sürümleri) listesi:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Belirli bir sürüm için kullanılabilir tüm görüntülerin listesi:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Gezinme ve geçerli görüntüleri kullanma ile ilgili daha fazla örnek için bkz. [Azure CLI ile Azure Virtual Machine görüntülerine erişin ve seçin](virtual-machines-linux-cli-ps-findimage.md).

`azure vm quick-create` ve `azure vm create` komutları ayrıca daha yaygın distro’lara ve en son sürümlerine hızlıca erişmek için kullanabileceğiniz bazı diğer adlara sahiptir. Bunun yapılması yayımcı, teklif, SKU ve sürümün bir sanal makine oluşturulan her durumda belirtilmesinden daha kolaydır:

| Diğer ad     | Yayımcı | Sunduğu        | SKU         | Sürüm |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | en son  |
| CoreOS    | CoreOS    | CoreOS       | Dengeli      | en son  |
| Debian    | credativ  | Debian       | 8           | en son  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | en son  |
| RHEL      | RedHat    | RHEL         | 7.2         | en son  |
| SLES      | SLES      | SLES         | 12 SP1      | en son  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | en son  |

### Kendi görüntünüzü kullanma

Belirli özelleştirmelere ihtiyaç duyuyorsanız, mevcut bir Azure VM’i temel alan bir görüntü kullanabilirsiniz, bunun için söz konusu VM’i *yakalayabilirsiniz* veya sanal sabit sürücüde (VHD) depoladığınız kendi görüntünüzü yükleyebilirsiniz. Desteklenen distro’lar ve kendi görüntünüzü kullanma ile ilgili daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure destekli dağıtımlar](virtual-machines-linux-endorsed-distros.md)

* [Desteklenmeyen dağıtımlarla ilgili bilgiler](virtual-machines-linux-create-upload-generic.md)

* [Linux sanal makinesini Resource Manager şablonu olarak yakalama](virtual-machines-linux-capture-image.md). Hızlı başlatma komutları:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## Sonraki adımlar

* [CLI](virtual-machines-linux-quick-create-cli.md) ile [portal](virtual-machines-linux-quick-create-portal.md) üzerinden veya Azure Resource Manager [şablon](virtual-machines-linux-cli-deploy-templates.md)u kullanarak bir Linux VM oluşturmak üzere öğretilerden birini deneyin.

* Bir Linux VM oluşturduktan sonra [bir veri diski ekleyin](virtual-machines-linux-add-disk.md).

* [Parola veya SSH anahtarlarını sıfırlama ve kullanıcıları yönetme](virtual-machines-linux-using-vmaccess-extension.md) ile ilgili hızlı adımlar



<!--HONumber=Sep16_HO3-->


