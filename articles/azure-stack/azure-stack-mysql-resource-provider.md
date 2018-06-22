---
title: MySQL veritabanları PaaS Azure yığında kullanın | Microsoft Docs
description: MySQL kaynak sağlayıcı dağıtmak ve Azure yığında bir hizmet olarak MySQL veritabanları sağlar nasıl öğrenin.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 24ba595413cde07c420a94de234d7926e0eb0e7f
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309844"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığında MySQL veritabanları kullanın

MySQL kaynak sağlayıcı Azure yığınında dağıtılan MySQL veritabanları kullanmak için API dağıtabilirsiniz. Kaynak sağlayıcısı API hakkında daha fazla bilgi için bkz: [Windows Azure Pack MySQL kaynak sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/dn528442.aspx).

MySQL veritabanları üzerindeki Web siteleri yaygın olarak bulunur ve birçok Web sitesi platformları destekler. Örneğin, Web uygulamaları platform hizmet (PaaS) eklentisi kullanılarak WordPress Web siteleri oluşturabilirsiniz.

Kaynak sağlayıcısı dağıttıktan sonra şunları yapabilirsiniz:

* MySQL sunucuları ve Azure Resource Manager dağıtım şablonları kullanarak veritabanları oluşturun.
* MySQL veritabanları hizmet olarak sağlar.  

## <a name="mysql-resource-provider-adapter-architecture"></a>MySQL kaynak sağlayıcı bağdaştırıcısı mimarisi

Kaynak sağlayıcısı aşağıdaki bileşenlere sahiptir:

* **MySQL kaynak sağlayıcı bağdaştırıcısı sanal makine (VM)**, sağlayıcı hizmetlerini çalıştıran bir Windows Server VM olduğu.
* **Kaynak sağlayıcısı**isteklerini işler ve erişimleri veritabanı kaynakları.
* **MySQL Server barındıran sunucular**, barındırma sunucuları adı verilen veritabanları için kapasite sağlar. MySQL örneklerini kendiniz oluşturmanız veya dış MySQL örnekleri için erişim sağlayın. [Azure yığın hızlı başlama Galerisi](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) için kullanabileceğiniz bir örnek şablon vardır:

  * MySQL sunucusu için oluşturun.
  * İndirin ve MySQL Server Azure Marketi'nden dağıtın.

> [!NOTE]
> Barındırma Azure yığın üzerinde yüklü olan sunucuları tümleşik sistemleri Kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Bunlar, Kiracı portalından veya bir uygun oturum açma ile bir PowerShell oturumu oluşturulmalıdır. Tüm barındırma sunucuları Faturalanabilir VM'ler ve lisansları olması gerekir. Hizmet Yöneticisi Kiracı aboneliğin sahibi olabilir.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar

Sistem hesabı aşağıdaki ayrıcalıklara sahip olmalıdır:

* **Veritabanı:** oluşturma, bırakma
* **Oturum açma:** oluşturmak, ayarlama, bırakma, vermek, iptal etme  

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcı dağıtma](azure-stack-mysql-resource-provider-deploy.md)
