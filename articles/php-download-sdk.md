---
title: PHP için Azure SDK'sını indirme
description: İndirme ve PHP için Azure SDK'sını yükleme hakkında bilgi edinin.
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
ms.openlocfilehash: f6b21f288b94e06414fe66ff775ebb264368c0b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65411589"
---
# <a name="download-the-azure-sdk-for-php"></a>PHP için Azure SDK'sını indirme

## <a name="overview"></a>Genel Bakış

PHP için Azure SDK'sı, geliştirmek, dağıtmak ve Azure için PHP uygulamaları yönetmenize olanak tanıyan bileşenlerini içerir. Özellikle, PHP için Azure SDK aşağıdakileri içerir:

* **Azure için PHP istemci kitaplıkları**. Bu sınıf kitaplıkları, Veri Yönetimi Hizmetleri gibi Azure özelliklere erişmek için bir arabirim sağlar ve bulut Hizmetleri.
* **Mac, Linux ve Windows (Azure CLI) için Azure komut satırı arabirimi**. Bu, Azure Web siteleri ve Azure sanal makineler gibi Azure hizmetlerini dağıtıp yönetmeye yönelik komutlar kümesidir. Azure CLI iş Mac, Linux ve Windows dahil olmak üzere herhangi bir platformda.
* **Azure PowerShell (yalnızca Windows)** . Bu, bulut Hizmetleri ve sanal makineler gibi Azure hizmetlerini dağıtıp yönetmeye yönelik PowerShell cmdlet'leri kümesidir.
* **Azure Öykünücüleri (yalnızca Windows)** . İşlem ve depolama öykünücüsünü bulut Hizmetleri ve bir uygulamayı yerel olarak test olanak tanıyan veri yönetim hizmetlerinin yerel öykünücüleri ' dir. Azure Öykünücüleri yalnızca Windows üzerinde çalıştırın.

Aşağıdaki bölümler, yukarıda açıklanan bileşenlere karşıdan yüklenip kurulacak açıklanmaktadır.

Bu konudaki yönergeler sahip olduğunuzu varsayın [PHP] [ install-php] yüklü.

> [!NOTE]
> Azure için PHP istemci kitaplıklarını kullanmanız için PHP 5.5 ya da daha yüksek olmalıdır.
>
>

## <a name="php-client-libraries-for-azure"></a>Azure için PHP istemci kitaplıkları

Azure için PHP istemci kitaplıkları, Veri Yönetimi Hizmetleri gibi Azure özelliklere erişmek için bir arabirim sağlar ve bulut hizmetlerinden herhangi bir işletim sistemi. Bu kitaplıklar Oluşturucusu yüklenebilir.

Azure için PHP istemci kitaplıklarını kullanma hakkında daha fazla bilgi için bkz: [Blob hizmetini kullanma][blob-service], [tablo hizmetini kullanma] [ table-service]ve [kuyruk hizmeti kullanmayı][queue-service].

### <a name="install-via-composer"></a>Oluşturucu yükleme

1. [Git'i yükleyin][install-git]. Windows üzerinde çalıştırılabilir Git PATH ortam değişkeninize eklemeniz gerekir.

2. Adlı bir dosya oluşturun **composer.json** projenizin kökünde ve aşağıdaki kodu ekleyin:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. İndirme **[composer.phar] [ composer-phar]** proje kökünüze içinde.

4. Bir komut istemi açın ve bu proje kökünüze yürütün

        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell ve Azure Öykünücüleri

Azure PowerShell (bulut Hizmetleri ve sanal makineler için gibi) Azure hizmetlerini dağıtıp yönetmeye yönelik PowerShell cmdlet'leri kümesidir. Azure Öykünücüleri bulut Hizmetleri ve bir uygulamayı yerel olarak test olanak tanıyan veri yönetim hizmetlerinin öykünücüleri ' dir. Bu bileşenler desteklenen yalnızca Windows.

Azure PowerShell ve Azure Öykünücüleri yüklemek için önerilen yöntem kullanmaktır [Microsoft Web Platformu yükleyicisi][download-wpi]. Ayrıca, PHP, SQL Server, SQL Server için PHP ve WebMatrix için Microsoft Drivers gibi diğer geliştirme bileşenlerini yüklemek seçebileceğiniz olduğunu unutmayın.

Azure PowerShell'in nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure PowerShell kullanmak için nasıl][powershell-tools].

## <a name="azure-cli"></a>Azure CLI

Azure CLI, Azure Web siteleri ve Azure sanal makineler gibi Azure hizmetlerini dağıtıp yönetmeye yönelik komutlar kümesidir. Azure CLI yükleme hakkında daha fazla bilgi için bkz: [Azure CLI'yı yükleme](cli-install-nodejs.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[install-php]: https://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: https://getcomposer.org/composer.phar
[nodejs-org]: https://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: https://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: https://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: https://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: https://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: https://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: https://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: https://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: https://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: https://git-scm.com/book/en/Getting-Started-Installing-Git
