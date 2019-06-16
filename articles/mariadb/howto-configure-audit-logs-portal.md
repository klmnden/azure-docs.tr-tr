---
title: Yapılandırma ve Denetim günlükleri için Azure veritabanı MariaDB için Azure portalında erişim
description: Bu makalede, Azure veritabanı denetim günlüklerinde MariaDB için Azure portalından erişmek ve yapılandırma açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: c9ee106972cc78a08709d2ce55d2dfddc96edbf7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079242"
---
# <a name="configure-and-access-audit-logs-in-the-azure-portal"></a>Azure portalında denetim günlüklerine erişmek ve yapılandırma

Yapılandırabileceğiniz [denetim günlüklerini MariaDB için Azure veritabanı](concepts-audit-logs.md) ve Azure Portalı'ndan tanılama ayarları.

> [!IMPORTANT]
> Denetim günlüğü işlevselliği şu anda Önizleme aşamasındadır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:

- [MariaDB için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

Etkinleştirin ve denetim günlüğü yapılandırın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. MariaDB için Azure veritabanı sunucunuza seçin.

1. Altında **ayarları** kenar seçme bölümünde **sunucu parametreleri**.
    ![Sunucu parametreleri](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. Güncelleştirme **audit_log_enabled** on parametresi.
    ![Denetim günlüklerini etkinleştirme](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. Güncelleştirerek günlüğe kaydedilecek olayları seçin **audit_log_events** parametresi.
    ![Denetim günlüğü olayları](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. MariaDB kullanıcıları güncelleştirerek günlüğünden hariç tutulacak ekleyin **audit_log_exclude_users** parametresi. MariaDB kullanıcı adlarına sağlayarak kullanıcıları belirtin.
    ![Denetim günlüğü dışlama kullanıcılar](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Parametreleri değiştirildi. bir kez tıklayabilirsiniz **Kaydet**. Veya **at** yaptığınız değişiklikleri.
    ![Kaydet](./media/howto-configure-audit-logs-portal/save-parameters.png)

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Altında **izleme** kenar seçme bölümünde **tanılama ayarları**.

1. Tıklayın "+ tanılama ayarı ekleme" ![tanılama ayarı ekleme](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Tanılama ayarı adı sağlayın.

1. Denetim günlüklerini (depolama hesabı, olay hub'ı ve/veya Log Analytics çalışma alanı) göndermek için hangi veri havuzlarını belirtin.

1. Günlük türü olarak "MySqlAuditLogs"'ni seçin.
![Tanılama ayarını yapılandırın](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Denetim günlükleri'ne araca taşımak veri havuzları yapılandırdıktan sonra tıklayabilirsiniz **Kaydet**.
![Tanılama ayarını kaydedin](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png)

1. Denetim günlükleri, yapılandırdığınız veri havuzları içinde inceleyerek erişin. Bu günlükleri görüntülenecek 10 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [denetim günlükleri](concepts-audit-logs.md) MariaDB için Azure veritabanı'nda.