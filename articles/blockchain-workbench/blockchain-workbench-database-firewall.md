---
title: Azure Blockchain çalışma ekranı SQL DB Güvenlik Duvarı'nı yapılandırma
description: Azure Blockchain çalışma ekranı SQL DB Güvenlik Duvarı'nı yapılandırma hakkında bilgi edinin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/4/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: dc22f212c014ab1d6622eff3491d669b21ca6f47
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Azure Blockchain çalışma ekranı veritabanı Güvenlik Duvarı'nı yapılandırma

Bu makalede, Azure portalını kullanarak bir güvenlik duvarı kuralı yapılandırma gösterilmektedir. Dış istemcilerin güvenlik duvarı kuralları izin ya da uygulamaları Azure Blockchain çalışma ekranı veritabanınıza bağlanın.

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain çalışma ekranı veritabanına bağlan

Bir kural yapılandırmak istediğiniz veritabanına bağlanmak için:

1. Azure portalına sahip bir hesapla oturum açın **sahibi** Azure Blockchain çalışma ekranı kaynaklar için izinleri.
2. Sol gezinti bölmesinde seçin **kaynak grupları**.
3. Kaynak grubu adı Blockchain çalışma ekranı dağıtımınız için seçin.
4. Seçin **türü** kaynakları sıralamak ve seçin, **SQL server**.
5. Kaynak listesi örneği aşağıdaki ekran görüntüsünde iki veritabanı gösterir: *ana* ve *lsgn-sdk*. Güvenlik duvarı kuralı yapılandırın *lsgn-sdk*.

![Liste Blockchain çalışma ekranı kaynakları](media/blockchain-workbench-database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Veritabanı güvenlik duvarı kuralı oluşturma

Bir güvenlik duvarı kuralı oluşturmak için:

1. "Lsgn-SDK'sı" veritabanı bağlantısını seçin.
2. Menü çubuğunda seçin **ayarlayın sunucu Güvenlik Duvarı**.

   ![Sunucu güvenlik duvarı ayarla](media/blockchain-workbench-database-firewall/configure-server-firewall.png)

3. Kuruluşunuz için bir kural oluşturmak için:

   * Girin bir **kural adı**
   * İçin bir IP adresi girin **başlangıç IP** bir adres aralığı
   * İçin bir IP adresi girin **bitiş IP** bir adres aralığı

   ![Güvenlik duvarı kuralı oluşturma](media/blockchain-workbench-database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Yalnızca, bilgisayarınızın IP adresi eklemek istiyorsanız, tercih **+ istemci IP'si Ekle**.
        
1. Güvenlik duvarı yapılandırmanızı kaydetmek için seçin **kaydetmek**.
2. Bir uygulama veya aracı bağlanarak veritabanı için yapılandırdığınız IP adresi aralığı sınayın. Örneğin, SQL Server Management Studio.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain çalışma ekranındaki veritabanı görünümleri](blockchain-workbench-database-views.md)