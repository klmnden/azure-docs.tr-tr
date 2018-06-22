---
title: SQL veritabanları Azure yığında kullanarak | Microsoft Docs
description: SQL veritabanları Azure yığını ve hızlı adımlar SQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak nasıl dağıtabileceğini öğrenin.
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
ms.openlocfilehash: 55d0e51606e8768a01c0b5a7766dbafe24d97a0d
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307834"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığın üzerinde SQL veritabanları kullanın

SQL veritabanları hizmet olarak kullanıma sunmak için SQL Server Kaynak sağlayıcısı bağdaştırıcısı API kullanın [Azure yığın](azure-stack-poc.md). Kaynak sağlayıcısını yüklemek ve bir veya daha fazla SQL Server örneklerine bağlamak sonra siz ve kullanıcılarınız oluşturabilirsiniz:

- Veritabanları bulut yerel uygulamalar için.
- SQL kullanan Web siteleri.
- SQL kullanan iş yükleri.

Kaynak sağlayıcısı tüm veritabanı yönetim yeteneklerini sağlamaz [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/). Örneğin, otomatik olarak kaynakları tahsis esnek havuzlar desteklenmez. Ancak, benzer kaynak sağlayıcısı destekler oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleridir bir SQL Server veritabanı. Kaynak sağlayıcısı API hakkında daha fazla bilgi için bkz: [Windows Azure paketi SQL Server Kaynak sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/dn528529.aspx).

>[!NOTE]
SQL Server Kaynak sağlayıcısı API Azure SQL veritabanı ile uyumlu değil.

## <a name="sql-resource-provider-adapter-architecture"></a>SQL kaynak sağlayıcı bağdaştırıcısı mimarisi

Kaynak sağlayıcısı aşağıdaki bileşenlerden oluşur:

- **SQL kaynak sağlayıcısı bağdaştırıcısı sanal makine (VM)**, sağlayıcı hizmetlerini çalıştıran bir Windows Server VM olduğu.
- **Kaynak sağlayıcısı**isteklerini işler ve erişimleri veritabanı kaynakları.
- **SQL Server'ı barındıran sunucular**, veritabanları için kapasite sağlayan barındırma sunucuları denir.

SQL Server'ın en az bir örnek oluşturmak veya dış SQL Server örnekleri için erişim sağlamanız gerekir.

> [!NOTE]
> Barındırma Azure yığın üzerinde yüklü olan sunucuları tümleşik sistemleri Kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Bunlar, Kiracı portalı veya uygun ile oturum açma PowerShell kullanarak oluşturulmalıdır. Tüm barındırma sunucuları Faturalanabilir sanal makineler ve lisansları olması gerekir. Hizmet Yöneticisi Kiracı aboneliğin sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server Kaynak sağlayıcısı dağıtma](azure-stack-sql-resource-provider-deploy.md)
