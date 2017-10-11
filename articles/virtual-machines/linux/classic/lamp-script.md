---
title: "Bir Linux VM CustomScript uzantısını kullanın | Microsoft Docs"
description: "CustomScript uzantısını uygulamalar üzerinde Azure Klasik dağıtım modeli kullanılarak oluşturulan Linux sanal makineleri dağıtmak için nasıl kullanılacağını öğrenin."
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.openlocfilehash: cb1fc9a44dc9e57d9cc9f1c546ad937d67e63c2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Linux için Azure CustomScript Uzantısı’nı kullanarak bir LAMP uygulaması dağıtma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modelini kullanarak bir AMPUL yığını dağıtma hakkında daha fazla bilgi için bkz: [burada](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Linux için Microsoft Azure CustomScript uzantısını (örneğin, Python ve Bash) VM tarafından desteklenen tüm komut dosyası dilinde yazılmış rastgele kod çalıştırarak sanal makineleri (VM'ler) özelleştirmek için bir yol sağlar. Bu, birden fazla makine için uygulama dağıtımı otomatik hale getirmek için çok esnek bir şekilde sağlar.

CustomScript uzantısını Azure portalı, Windows PowerShell veya Azure komut satırı arabirimi (Azure CLI) kullanarak dağıtabilirsiniz.

Kullanacağız. Bu makalede Klasik dağıtım modeli kullanılarak bir basit bir Ubuntu VM AMPUL uygulamayı dağıtmak için Azure CLI oluşturulur.

## <a name="prerequisites"></a>Ön koşullar
Bu örnekte, ilk iki Azure Ubuntu 14.04 veya sonraki sürümünü çalıştıran VM oluşturun. Sanal makineleri adlı *betik vm* ve *ampul vm*. Sanal makineleri oluştururken benzersiz adlar kullanın. CLI komutları çalıştırmak için kullanılır ve bir AMPUL uygulama dağıtmak için kullanılır.

Ayrıca bir Azure Storage hesabını ve (Bu Azure portalından alabilirsiniz) erişmek için bir anahtar gerekir.

Linux sanal makineleri Azure üzerinde oluşturma yardıma gereksinim duyarsanız başvurmak [çalıştıran bir sanal makine Linux oluşturma](createportal.md).

Ubuntu yükleme komutları varsayar ancak desteklenen tüm Linux distro yüklemede uyarlayabilirsiniz.

Komut dosyası vm VM Azure CLI, Azure çalışan bir bağlantı yüklenmiş olmalıdır. Bu Yardım başvurmak için [Azure komut satırı arabirimi yükleyip](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>Bir komut dosyasını karşıya yükle
AMPUL yığını yüklemek ve bir PHP sayfası oluşturmak için uzak bir VM üzerinde bir komut dosyasını çalıştırmak için CustomScript uzantısını kullanacağız. Betik yerden erişmek için size bir Azure blob'u olarak karşıya.

### <a name="script-overview"></a>Komut dosyası genel bakış
Komut dosyası örneği AMPUL yığını Ubuntu (MySQL sessiz bir yükleme ayarlama dahil) yükler, basit bir PHP dosya yazar ve Apache başlatır.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Komut dosyasını karşıya yükle
Komut dosyası gibi bir metin dosyası olarak kaydetmeniz *install_lamp.sh*ve ardından Azure Storage'a yükler. Kolayca Azure CLI ile bunu yapabilirsiniz. Aşağıdaki örnekte "betikleri" adlı bir depolama kapsayıcısı dosyayı yükler. Kapsayıcı yoksa önce oluşturmanız gerekir.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Ayrıca komut dosyası Azure depolama biriminden indirmek nasıl açıklayan bir JSON dosyası oluşturun. Bu olarak Kaydet *public_config.json* ("mystorage" değerini depolama hesabınızın adıyla değiştirin):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Uzantısı dağıtma
Artık Azure CLI kullanarak uzaktan VM Linux CustomScript uzantısını dağıtmak için sonraki komutunu kullanabilirsiniz.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Önceki komutu yükler ve çalıştırır *install_lamp.sh* adlı VM üzerinde komut dosyası *ampul vm*.

Uygulamasını içeren bir web sunucusu olduğundan, bir HTTP dinleme bağlantı noktası uzaktan VM sonraki komutla açın unutmayın.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme
Ne kadar iyi uzaktan VM günlük dosyasını bakarak özel bir komut dosyası çalıştığı kontrol edebilirsiniz. SSH için *ampul vm* ve günlük dosyası İleri komutu ile kuyruk.

    cd /var/log/azure/customscript
    tail -f handler.log

CustomScript uzantısını çalıştırdıktan sonra bilgi için oluşturduğunuz PHP sayfasına göz atabilirsiniz. Bu makalede örneğin PHP sayfasıdır *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Ek kaynaklar
Daha karmaşık uygulamaları dağıtmak için aynı temel adımları kullanabilirsiniz. Bu örnekte, Azure storage'da bir ortak BLOB yükleme betiği kaydedildi. Daha güvenli bir seçenek ile güvenli bir BLOB yükleme betiği depolamak uygulanacak bir [güvenli erişim imzası](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Ek kaynaklar için Azure CLI, Linux ve CustomScript uzantısını sonraki listelenir.

[CustomScript uzantısını kullanarak Linux VM özelleştirme görevlerini otomatik hale getirme](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux Uzantıları (GitHub)](https://github.com/Azure/azure-linux-extensions)