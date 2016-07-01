<properties
    pageTitle="Linux VM oluşturmanın farklı yolları | Microsoft Azure"
    description="Azure üzerinde bir Linux sanal makine oluşturmanın farklı yollarını sıralar ve ayrıntılı yönergeler için bağlantılar verir"
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
    ms.date="04/12/2016"
    ms.author="iainfou"/>

# Resource Manager ile Linux sanal makine oluşturmanın farklı yolları

Farklı kullanıcılara ve amaçlara uyması için Azure Resource Manager geliştirme modelini kullanarak bir VM oluşturmanın farklı yollarını sunar. Bu makale söz konusu farkları ve kendi Linux sanal makinelerinizi (VM’ler) oluşturmak için yapabileceğiniz seçimleri özetler.

## Araç seçenekleri

### Komut kabuğu: Azure CLI 

CLI üzerinden Azure komut satırı arabirimini kullanın. npm, Docker kapsayıcısı veya yükleme betiği ile [Azure CLI yükleme](../xplat-cli-install.md) hakkında daha fazla bilgi edinebilirsiniz. Aşağıdaki öğretiler Azure CLI kullanma ile ilgili örnekler sunar:

* [Geliştirme ve test için Azure CLI’dan bir Linux VM oluşturma](virtual-machines-linux-quick-create-cli.md) 

* [Bir Azure şablonu kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

* [Azure CLI kullanarak sıfırdan bir Linux VM oluşturma](virtual-machines-linux-create-cli-complete.md)

### GUI: Azure Portal

[Azure portal](https://portal.azure.com)’ın grafik kullanıcı arabirimi, özellikle Azure ile yeni başlıyorsanız, sisteminize yüklenecek hiç bir şey olmadığından bir VM’i denemek için basit bir yol sunar. VM oluşturmak için Azure portalını kullanma:

* [Azure portalını kullanarak Linux çalıştıran bir sanal makine oluşturma](virtual-machines-linux-quick-create-portal.md) 

## İşletim sistemi ve görüntü seçimi

Her iki yöntemde de çalıştırmak istediğiniz işletim sistemine dayanan bir görüntü seçebilirsiniz. Azure ve ortakları, bazıları önyüklü uygulamalar ve araçlarla gelen çok sayıda görüntü sağlar. Veya, kendi görüntülerinizden birini yükleyebilirsiniz.

### Azure görüntüleri

Yukarıdaki makalelerin tümünde bir VM oluşturmak ve ağ oluşturma, yük dengeleme ve daha fazlası için özelleştirmek üzere mevcut bir Azure görüntüsünü kolayca kullanabilirsiniz. Portal Azure’nin sağladığı görüntüler için Azure Market sunar. Komut satırını kullanarak benzer listeleri edinebilirsiniz. Örneğin Azure CLI’da konum ve yayıncıya göre kullanılabilen tüm görüntülerin listesini almak için `azure vm image list` çalıştırın. Gezinme ve geçerli görüntüleri kullanma ile ilgili örnekler için bkz. [Azure CLI ile Azure Virtual Machine görüntülerine erişin ve seçin](virtual-machines-linux-cli-ps-findimage.md).

### Kendi görüntünüzü kullanma

Belirli özelleştirmelere ihtiyaç duyuyorsanız, mevcut bir Azure VM’i temel alan bir görüntü kullanabilirsiniz, bunun için söz konusu VM’i *yakalayabilirsiniz* veya sanal sabit sürücüde (VHD) depoladığınız kendi görüntünüzü yükleyebilirsiniz. Desteklenen distro’lar ve kendi görüntünüzü kullanma ile ilgili daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure destekli dağıtımlar](virtual-machines-linux-endorsed-distros.md)

* [Desteklenmeyen dağıtımlarla ilgili bilgiler](virtual-machines-linux-create-upload-generic.md)

* [Linux sanal makinesini Resource Manager şablonu olarak yakalama](virtual-machines-linux-capture-image.md). 

## Sonraki adımlar

* [CLI](virtual-machines-linux-quick-create-cli.md) ile [portal](virtual-machines-linux-quick-create-portal.md) üzerinden veya Azure Resource Manager [şablon](virtual-machines-linux-cli-deploy-templates.md)u kullanarak bir Linux VM oluşturmak üzere öğretilerden birini deneyin.

* Bir Linux VM oluşturduktan sonra [bir veri diski ekleyin](virtual-machines-linux-add-disk.md).

* [Parola veya SSH anahtarlarını sıfırlama ve kullanıcıları yönetme](virtual-machines-linux-using-vmaccess-extension.md) ile ilgili hızlı adımlar



<!--HONumber=Jun16_HO2-->


