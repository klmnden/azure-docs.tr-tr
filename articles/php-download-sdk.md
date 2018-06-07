---
title: PHP için Azure SDK'sını indirme
description: Azure SDK'sı için PHP yükleyip öğrenin.
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: ''
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
ms.openlocfilehash: cfcf908145e8a384782953e045f9e10fd3c0e8f9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639478"
---
# <a name="download-the-azure-sdk-for-php"></a>PHP için Azure SDK'sını indirme

## <a name="overview"></a>Genel Bakış

PHP için Azure SDK'sı, geliştirme, dağıtma ve PHP uygulamaları için Azure yönetmenize olanak sağlayan bileşenleri içerir. Özellikle, PHP için Azure SDK aşağıdakileri içerir:

* **Azure için PHP istemci kitaplıkları**. Bu sınıf kitaplıkları Veri Yönetimi Hizmetleri gibi Azure özelliklere erişmek için bir arabirim sağlar ve bulut Hizmetleri.
* **Mac, Linux ve Windows (Azure CLI) için Azure komut satırı arabirimi**. Bu, dağıtmak ve Azure Web siteleri ve Azure sanal makineler gibi Azure hizmetleri yönetmek için komutlar kümesidir. Mac, Linux ve Windows dahil olmak üzere herhangi bir platform üzerinde Azure CLI çalışma.
* **Azure PowerShell (yalnızca Windows)**. Bu, dağıtmak ve bulut Hizmetleri ve sanal makineler gibi Azure hizmetleri yönetmek için PowerShell cmdlet'leri kümesidir.
* **(Yalnızca Windows) Azure öykünücüsünü**. İşlem ve depolama öykünücüsünü bulut Hizmetleri ve bir uygulamayı yerel olarak test etmenize izin veri yönetimi hizmetleri yerel Öykünücüler ' dir. Azure öykünücüsünü yalnızca Windows üzerinde çalıştırın.

Aşağıdaki bölümler, yukarıda açıklanan bileşenlerini yükleyip açıklar.

Bu konudaki yönergeler sahip olduğunuzu varsaymaktadır [PHP] [ install-php] yüklü.

> [!NOTE]
> Azure için PHP istemci kitaplıkları kullanmak için PHP 5.5 ya da daha yüksek olması gerekir.
>
>

## <a name="php-client-libraries-for-azure"></a>Azure için PHP istemci kitaplıkları

Azure için PHP istemci kitaplıkları, Veri Yönetimi Hizmetleri gibi Azure özelliklere erişmek için bir arabirim sağlar ve bulut hizmetlerinden herhangi bir işletim sistemi. Bu kitaplıklar Oluşturucusu yüklenebilir.

Azure için PHP istemci kitaplıkları kullanma hakkında daha fazla bilgi için bkz: [Blob hizmeti kullanmak nasıl][blob-service], [tablo hizmetini kullanmayı] [ table-service]ve [kuyruk hizmetini kullanmayı][queue-service].

### <a name="install-via-composer"></a>Oluşturucu yükleyin

1. [Git'i yükleyin][install-git]. Windows, PATH ortam değişkenine yürütülebilir Git eklemeniz gerekir.

2. Adlı bir dosya oluşturun **composer.json** projenizi kök ve aşağıdaki kodu ekleyin:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Karşıdan **[composer.phar] [ composer-phar]** proje kök.

4. Bir komut istemi açın ve bu proje kök dizininde yürütme

        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell ve Azure öykünücüsünü

Azure PowerShell, dağıtma ve Azure Hizmetleri (örneğin, bulut Hizmetleri ve sanal makineler) yönetmek için PowerShell cmdlet'leri kümesidir. Azure öykünücüsünü Öykünücüler bulut Hizmetleri ve bir uygulamayı yerel olarak test etmenize izin Veri Yönetimi Hizmetleri ' dir. Bu bileşenlerin desteklenen yalnızca Windows.

Azure PowerShell ve Azure öykünücüsünü yüklemek için önerilen yöntem kullanmaktır [Microsoft Web Platformu yükleyicisi][download-wpi]. Ayrıca, PHP, SQL Server, PHP ve WebMatrix için SQL Server için Microsoft Drivers gibi diğer geliştirme bileşenleri yüklemek seçebileceğiniz olduğunu unutmayın.

Azure PowerShell'in nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Azure PowerShell kullanmak için nasıl][powershell-tools].

## <a name="azure-cli"></a>Azure CLI

Azure CLI, dağıtma ve Azure Web siteleri ve Azure sanal makineler gibi Azure hizmetleri yönetmek için komutlar kümesidir. Azure CLI yükleme hakkında daha fazla bilgi için bkz: [Azure CLI yükleme](cli-install-nodejs.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
