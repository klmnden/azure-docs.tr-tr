---
title: Azure portalında salt okunur çoğaltmalar için Azure veritabanı PostgreSQL için yönetme
description: Bu makalede, Azure portalında çoğaltma okuma PostgreSQL için Azure veritabanı'nı yönetme açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/17/2019
ms.openlocfilehash: 6c1a0a4a13a70daec157ede98f850f87150f8d93
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54383531"
---
# <a name="how-to-create-and-manage-read-replicas-in-the-azure-portal"></a>Azure portalında çoğaltmalar oluşturmak ve yönetmek nasıl okuyun
Bu makalede, oluşturmak ve yönetmek için Azure veritabanı Azure portalını kullanarak PostgreSQL hizmeti salt okunur çoğaltma öğreneceksiniz. Salt okunur çoğaltmalar hakkında daha fazla bilgi edinmek için [kavramları belgeleri okuyun](concepts-read-replicas.md).

## <a name="prerequisites"></a>Önkoşullar
- Bir [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-portal.md) ana sunucusu olacaktır.

## <a name="prepare-the-master-server"></a>Ana sunucu hazırlama
Bu ana hazırlama adımı yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için geçerlidir.

**Azure.replication_support** parametre ayarlanmış ana sunucuya ÇOĞALTMAYA gerekir. Bu parametre değiştirme etkili olması için sunucunun yeniden başlatılmasını gerektirir.

1. Azure Portal'da bir şablonu kullanmak istediğiniz bir PostgreSQL sunucusu için mevcut Azure veritabanını seçin.

2. Seçin **sunucu parametreleri** sol taraftaki menüden.

3. Arama **azure.replication_support**.

   ![-PostgreSQL için Azure veritabanı azure.replication_support](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Ayarlama **azure.replication_support** çoğaltmaya. **Kaydet** Değiştir.

   ![Kaydet ve PostgreSQL - çoğaltma için Azure veritabanı](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Kaydetme işlemi tamamlandıktan sonra bir bildirim alırsınız.

   ![Bildirim Kaydet - PostgreSQL için Azure veritabanı](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Kaydedildikten sonra değişikliği uygulamak için sunucuyu yeniden başlatın. Bkz: [yeniden belgeleri](howto-restart-server-portal.md) sunucuyu yeniden hakkında bilgi edinmek için.

## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma
Okunur çoğaltmalar, aşağıdaki adımları kullanarak oluşturulabilir:
1.  Bir şablonu kullanmak istediğiniz bir PostgreSQL sunucusu için mevcut Azure veritabanı'nı seçin. 

2.  Çoğaltma ayarları menüsünde seçin.

   Ayarlamadıysanız **azure.replication_support** Çoğaltmaya genel amaçlı veya bellek için iyileştirilmiş ana ve yeniden sunucu bunu yapmak için yönergelerini içeren bir ileti görürsünüz. Oluşturma işlemine devam etmeden önce bunu yapın.

3.  Çoğaltma Ekle'yi seçin.

   ![Azure veritabanı - PostgreSQL için çoğaltma ekleme](./media/howto-read-replicas-portal/add-replica.png)

4.  Çoğaltma sunucusu için bir ad girin ve çoğaltma oluşturulmasını onaylamak için Tamam'ı seçin.

   ![PostgreSQL - ad çoğaltma için Azure veritabanı](./media/howto-read-replicas-portal/name-replica.png) 

> [!IMPORTANT]
> Okuma çoğaltmaları aynı sunucu yapılandırma yöneticisi olarak oluşturulur. Bir çoğaltma oluşturduktan sonra fiyatlandırma katmanını (temel gelen ve giden hariç), işlem oluşturma, sanal çekirdek, depolama ve yedekleme bekletme süresi değiştirilebilir bağımsız olarak ana sunucu ile.

> [!IMPORTANT]
> Sunucu Yapılandırma Yöneticisi'nin yeni değerlere güncelleştirilmeden önce çoğaltmaları yapılandırma eşit veya daha büyük değerler için güncelleştirilmesi gerekir. Bunu yapma girişimi, aksi takdirde bir hata neden olur. Bu, çoğaltmaları ana dala yapılan değişiklikleri takip edin mümkün olmasını sağlar. 


Çoğaltma sunucusu oluşturulduktan sonra çoğaltma penceresinden görüntülenebilir.

![PostgreSQL - yeni çoğaltma için Azure veritabanı](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Çoğaltmayı durdur

> [!IMPORTANT]
> Bir sunucuya çoğaltma durdurma işlemi geri alınamaz. Bir ana ve çoğaltma arasında çoğaltmayı durdurdu sonra geri alınamaz. Çoğaltma sunucusu bir tek başına sunucu olur ve artık hem okuma hem de yazma işlemleri destekler. Bu sunucu bir yinelemeye yeniden yapılamıyor.

Bir ana ve Azure portalından bir çoğaltma arasında çoğaltmayı durdurmak için aşağıdaki adımları kullanın:
1.  Azure portalında, ana Azure veritabanınızı PostgreSQL sunucusuna seçin.

2.  Çoğaltma ayarları menüsünde seçin.

3.  Çoğaltma sunucusu için çoğaltma durdurma istediğiniz seçin.

   ![PostgreSQL - Select çoğaltma için Azure veritabanı](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Çoğaltmayı durdurma seçin.

   ![-Select çoğaltmasını olan PostgreSQL için Azure veritabanı](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5.  Tamam'a tıklayarak çoğaltma bırakmak istediğinizi onaylayın.

   ![Azure veritabanı - PostgreSQL için Onayla çoğaltma durdurma](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master"></a>Bir şablonu Sil

> [!IMPORTANT]
> Ana sunucu silme çoğaltma tüm çoğaltma sunucuları için durdurur. Artık hem okuma hem de yazma işlemleri destekleyen tek başına sunucular çoğaltma sunucusu olur.
Bir şablonu silmek için tek başına veritabanı Azure PostgreSQL sunucusu için aynı adımları izlemektedir. Azure portaldan sunucu silmek için aşağıdakileri yapın:

1.  Azure portalında, ana Azure veritabanınızı PostgreSQL sunucusuna seçin.

2.  Genel Bakış'tan Sil'i seçin.

   ![Azure PostgreSQL için veritabanı - sunucuyu silme](./media/howto-read-replicas-portal/delete-server.png)
 
3.  Ana sunucunun adını yazın ve ana sunucu silme işlemini onaylamak için Sil'i seçin.

   ![Azure veritabanını Postgresql'ye - Silmeyi Onayla](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Bir çoğaltmayı Sil
Salt okunur bir çoğaltmayı silmek için aynı adımları izleyin yukarıdaki ana sunucu olarak silme ile. İlk Çoğaltmaya genel bakış sayfasını açın, ardından Sil'i seçin.

   ![PostgreSQL - çoğaltmayı Sil'için Azure veritabanı](./media/howto-read-replicas-portal/delete-replica.png)
 
Alternatif olarak, çoğaltma penceresinden silebilirsiniz.
1.  Azure portalında, ana Azure veritabanınızı PostgreSQL sunucusuna seçin.

2.  Çoğaltma ayarları menüsünde seçin.

3.  Silmek istediğiniz çoğaltma sunucusunu seçin. 

   ![PostgreSQL - Select çoğaltma için Azure veritabanı](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Çoğaltmayı Sil'ı seçin.

   ![Çoğaltma - PostgreSQL için Azure veritabanı silmeyi seçme](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5.  Çoğaltma adını yazın ve çoğaltmayı silme işlemini onaylamak için Sil'i seçin.

   ![Azure veritabanını Postgresql'ye - onaylayın çoğaltmayı Sil](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Bir yineleme izleme
### <a name="max-lag-across-replicas"></a>Yinelemeler boyunca en fazla gecikme
**Yinelemeler boyunca en fazla gecikme** ölçüm ana en İzolasyonu çoğaltma arasındaki gecikme süresini gösterir. 

1.  Azure portalında **ana** PostgreSQL sunucusu için Azure veritabanı.

2.  Ölçümleri seçin. Ölçümleri penceresinde **arasında en fazla gecikme çoğaltmaları**.

    ![-İzleyici en fazla gecikme yinelemeler genelinde olan PostgreSQL için Azure veritabanı](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Seçin **Max** , toplama olarak. 

### <a name="replica-lag"></a>Çoğaltma gecikmesi
**Çoğaltma gecikmesi** ölçüm Bu çoğaltma işlemi son durumdayken bu yana zamanı gösterir. Ana şablonunuzu üzerinde gerçekleşen işlem varsa, bu zaman gecikmesini ölçüm yansıtır.

1.  Azure portalında bir **çoğaltma** PostgreSQL sunucusu için Azure veritabanı.

2.  Ölçümleri seçin. Ölçümleri penceresinde **çoğaltma gecikmesi**.

   ![-İzleyici çoğaltma gecikmesi olan PostgreSQL için Azure veritabanı](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3.  Seçin **Max** , toplama olarak. 
 
## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [PostgreSQL için Azure veritabanı'nda çoğaltmaları okuma](concepts-read-replicas.md).