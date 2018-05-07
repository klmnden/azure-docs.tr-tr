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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: b1cc1fad6b0831bcf0bab5ba4f37b753c3cf33ca
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığın üzerinde SQL veritabanları kullanın
SQL veritabanları hizmet olarak kullanıma sunmak için SQL Server Kaynak sağlayıcısı bağdaştırıcısını kullanmak [Azure yığın](azure-stack-poc.md). Kaynak sağlayıcısını yüklemek ve bir veya daha fazla SQL Server örneklerine bağlamak sonra siz ve kullanıcılarınız oluşturabilirsiniz:
- Veritabanları bulut yerel uygulamalar için.
- SQL tabanlı Web siteleri.
- SQL tabanlı iş yükleri.
Bir sanal makine (VM) barındıran SQL Server her zaman sağlamak zorunda değilsiniz.

Kaynak sağlayıcısı tüm veritabanı yönetim özelliklerini desteklemez [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/). Örneğin, esnek veritabanı havuzları ve veritabanı performansını yukarı ve aşağı otomatik arama özelliği kullanılabilir değil. Ancak, kaynak sağlayıcısı yoksa desteği benzer oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleridir. API SQL veritabanı ile uyumlu değil.

## <a name="sql-resource-provider-adapter-architecture"></a>SQL kaynak sağlayıcı bağdaştırıcısı mimarisi
Kaynak sağlayıcısı üç bileşenden oluşur:

- **SQL kaynak sağlayıcısı bağdaştırıcısı VM**, sağlayıcı hizmetlerini çalıştıran bir Windows sanal makine olduğu.
- **Kaynak sağlayıcısı**, sağlama isteklerini işler ve kullanıma sunar kaynaklarını veritabanı.
- **SQL Server'ı barındıran sunucular**, veritabanları için kapasite sağlayan barındırma sunucuları denir.

Siz bir (veya daha fazla) SQL Server örneklerini oluşturmak veya dış SQL Server örnekleri için erişim sağlamak.

> [!NOTE]
> Barındırma Azure yığın üzerinde yüklü olan sunucuları tümleşik sistemleri Kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Bunlar, Kiracı portalından veya bir uygun oturum açma ile bir PowerShell oturumu oluşturulmalıdır. Tüm barındırma sunucuları ücrete tabi VM'ler ve uygun izinlere sahip olmalıdır. Hizmet Yöneticisi Kiracı aboneliğin sahibi olabilir.


## <a name="next-steps"></a>Sonraki adımlar

[SQL Server Kaynak sağlayıcısı dağıtma](azure-stack-sql-resource-provider-deploy.md)
