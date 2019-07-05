---
title: Yapılandırma ve Denetim günlükleri için Azure veritabanı MySQL için Azure portalında erişim
description: Bu makalede, yapılandırmak ve denetim günlüklerini Azure veritabanı'nda MySQL için Azure portalından erişmek açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: a2f44b52c6d34adb1bebf666ff8453b17f7778a5
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448479"
---
# <a name="configure-and-access-audit-logs-in-the-azure-portal"></a>Azure portalında denetim günlüklerine erişmek ve yapılandırma

Yapılandırabileceğiniz [denetim günlüklerini MySQL için Azure veritabanı](concepts-audit-logs.md) ve Azure Portalı'ndan tanılama ayarları.

> [!IMPORTANT]
> Denetim günlüğü işlevselliği şu anda Önizleme aşamasındadır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:

- [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

Etkinleştirin ve denetim günlüğü yapılandırın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. MySQL sunucusu için Azure veritabanı'nı seçin.

1. Altında **ayarları** kenar seçme bölümünde **sunucu parametreleri**.
    ![Sunucu parametreleri](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. Güncelleştirme **audit_log_enabled** on parametresi.
    ![Denetim günlüklerini etkinleştirme](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. Seçin [olay türleri](concepts-audit-logs.md#configure-audit-logging) güncelleştirerek günlüğe kaydedilecek **audit_log_events** parametresi.
    ![Denetim günlüğü olayları](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. MySQL kullanıcıları güncelleştirerek günlüğünden hariç tutulacak ekleyin **audit_log_exclude_users** parametresi. MySQL kullanıcı adı sağlayarak kullanıcıları belirtin.
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

- Daha fazla bilgi edinin [denetim günlükleri](concepts-audit-logs.md) MySQL için Azure veritabanı'nda.