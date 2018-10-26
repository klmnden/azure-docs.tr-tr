---
title: Azure Stack'te SQL veritabanlarını kullanma | Microsoft Docs
description: Azure Stack ve hızlı adımları SQL Server Kaynak sağlayıcısı bağdaştırıcısını dağıtmak için bir hizmet olarak SQL veritabanlarını nasıl dağıtılacağı hakkında bilgi edinin.
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
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 3d608843ef31a1ed665fcb1fd90b822f34f77fdd
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086361"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure Stack'te SQL veritabanlarını kullanma

SQL veritabanları hizmet olarak sunmak için SQL Server Kaynak sağlayıcısı bağdaştırıcısını kullanın [Azure Stack](azure-stack-poc.md). Kaynak sağlayıcısını yüklemek ve bir veya daha fazla SQL Server örneklerine bağlanabilirsiniz sonra siz ve kullanıcılarınız oluşturabilirsiniz:

- Bulutta yerel uygulamalar için veritabanları.
- SQL kullanan Web siteleri.
- SQL kullanan iş yükleri.

Kaynak sağlayıcısı veritabanı yönetim yeteneklerini sağlamaz [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/). Örneğin, otomatik olarak kaynak tahsis elastik havuzlar desteklenmez. Ancak, kaynak sağlayıcısı destekler benzer oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri SQL Server veritabanı. 

## <a name="sql-resource-provider-adapter-architecture"></a>SQL kaynak sağlayıcısı bağdaştırıcısını mimarisi

Kaynak sağlayıcısı, aşağıdaki bileşenlerden oluşur:

- **SQL kaynak sağlayıcısı bağdaştırıcısını sanal makine (VM)**, sağlayıcı hizmetlerini çalıştıran bir Windows Server VM olduğu.
- **Kaynak sağlayıcısı**isteklerini işler ve veritabanı kaynaklara erişir.
- **SQL Server'ı barındıran sunucular**, veritabanları için kapasite sunan barındırma sunucuları denir.

SQL Server'ın en az bir örnek oluşturun veya dış SQL Server örneğine erişebilmesi gerekir.

> [!NOTE]
> Barındıran Azure Stack üzerinde yüklü bir sunucu tarafından sunulan tümleşik sistemler Kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı aboneliği oluşturulamıyor. Bunlar, Kiracı portalı veya PowerShell ilgili oturum açma ile kullanarak oluşturulmalıdır. Tüm barındırma sunucuları Faturalanabilir sanal makineler ve lisansına sahip olması gerekir. Hizmet Yöneticisi, Kiracı aboneliğin sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server Kaynak sağlayıcısı dağıtma](azure-stack-sql-resource-provider-deploy.md)
