---
title: Salt okunur çoğaltmalar için Azure veritabanı, PostgreSQL - Azure portalında tek bir sunucu için yönetme
description: PostgreSQL - Azure portalında tek bir sunucu için Azure veritabanı salt okunur çoğaltmalar yönetmeyi öğrenin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 87371f91d9ea1f556d0f78beebd73b8a28977b71
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510373"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>Oluşturma ve PostgreSQL - Azure portalında tek bir sunucu için Azure veritabanı'nda salt okunur çoğaltmalar yönetme

Bu makalede, oluşturma ve Azure veritabanı'nda salt okunur çoğaltmalar PostgreSQL için Azure portalından yönetme öğrenin. Salt okunur çoğaltmalar hakkında daha fazla bilgi için bkz: [genel bakış](concepts-read-replicas.md).

> [!IMPORTANT]
> Salt okunur bir çoğaltması, ana sunucunuz ile aynı bölgede ya da diğer Azure bölgesinde, tercih ettiğiniz oluşturabilirsiniz. Bölgeler arası çoğaltma şu anda genel Önizleme aşamasındadır.


## <a name="prerequisites"></a>Önkoşullar
Bir [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-portal.md) ana sunucu olarak.

## <a name="prepare-the-master-server"></a>Ana sunucu hazırlama
Bu adımlar, genel amaçlı veya bellek için iyileştirilmiş katmanlarındaki ana sunucu hazırlamak için kullanılmalıdır. Ana sunucu azure.replication_support parametresini ayarlayarak, çoğaltma için hazırlanır. Çoğaltma parametre değiştiğinde, değişikliğin etkili olması için sunucunun yeniden başlatılması gereklidir. Azure portalında tek bir düğmeye tarafından bu iki adımı Kapsüllenen **çoğaltma desteğini etkinleştir**.

1. Azure portalında mevcut bir şablonu kullanmak için Azure veritabanı PostgreSQL sunucusu seçin.

2. Sunucu Kenar çubuğunda altında **ayarları**seçin **çoğaltma**.

3. Seçin **çoğaltma desteğini etkinleştirin**. 

   ![Çoğaltma desteğini etkinleştir](./media/howto-read-replicas-portal/enable-replication-support.png)

4. Çoğaltma desteğini etkinleştirmek istediğinizi onaylayın. Bu işlem, ana sunucu yeniden başlatılır. 

   ![Çoğaltma desteğini etkinleştir onaylayın](./media/howto-read-replicas-portal/confirm-enable-replication.png)
   
5. İşlem tamamlandıktan sonra iki Azure portalı bildirimleri alırsınız. Sunucu parametresi güncelleştirmek için bir bildirim yoktur. Hemen izleyen sunucunun yeniden başlatılması için başka bir bildirim yoktur.

   ![Başarılı bildirimler - etkinleştir](./media/howto-read-replicas-portal/success-notifications-enable.png)

6. Çoğaltma araç güncelleştirmek için Azure portal sayfasındaki yenileme. Artık bu sunucu için salt okunur çoğaltmalar oluşturabilirsiniz.

   ![Güncelleştirilmiş araç çubuğu](./media/howto-read-replicas-portal/updated-toolbar.png)
   
Çoğaltma desteğini etkinleştirme, ana sunucu başına tek seferlik bir işlemdir. A **devre dışı çoğaltma desteği** düğmesi, size kolaylık olması için sağlanmıştır. Bu ana sunucuda hiçbir zaman bir çoğaltma oluşturacak olmadıkça çoğaltma desteğini devre dışı bırakmaya önerilmemektedir. Mevcut çoğaltmaları ana sunucunuz varken, çoğaltma desteği devre dışı bırakamazsınız.


## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma
Salt okunur bir çoğaltma oluşturmak için aşağıdaki adımları izleyin:

1. Mevcut ana sunucu olarak kullanmak için Azure veritabanı PostgreSQL sunucusu seçin. 

2. Sunucu Kenar çubuğunda altında **ayarları**seçin **çoğaltma**.

3. Seçin **çoğaltma ekleme**.

   ![Bir çoğaltma ekleme](./media/howto-read-replicas-portal/add-replica.png)

4. Okuma çoğaltması için bir ad girin. 

    ![Ad çoğaltma](./media/howto-read-replicas-portal/name-replica.png)

5. Çoğaltma için bir konum seçin. Bir çoğaltma, herhangi bir Azure bölgesinde oluşturabilirsiniz. Varsayılan konumu, ana sunucu ile aynıdır.

    ![Konum seçin](./media/howto-read-replicas-portal/location-replica.png)

6. Seçin **Tamam** oluşturma işlemini onaylamak için.

Çoğaltma Yöneticisi olarak aynı sunucu yapılandırmasını kullanarak oluşturulur. Bir çoğaltma oluşturulduktan sonra birkaç ayar bağımsız olarak ana sunucu ile değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı da ayrı ayrı değiştirilebilir ya da temel katmandan hariç.

> [!IMPORTANT]
> Bir ana sunucu yapılandırması için yeni değerleri güncelleştirilmeden önce çoğaltma yapılandırması eşit veya daha fazla değerlerle güncelleştirin. Bu eylem, çoğaltma ana dala yapılan değişiklikler ile koruyabilirsiniz sağlar.

Salt okunur çoğaltma oluşturulduktan sonra bunu görüntülenebilir **çoğaltma** penceresi:

![Yeni çoğaltma çoğaltma penceresinde görüntüleme](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Çoğaltmayı durdur
Bir ana sunucu ve bir salt okunur çoğaltma arasında çoğaltmayı durdurabilirsiniz.

> [!IMPORTANT]
> Çoğaltma için ana sunucu ve salt okunur bir çoğaltması durdurduktan sonra geri alınamaz. Salt okunur çoğaltma hem okuma hem de yazma işlemleri destekleyen bir tek başına sunucuya olur. Tek başına sunucu ile bir çoğaltma yeniden yapılamıyor.

Bir ana sunucu ve Azure portalından bir salt okunur çoğaltma arasında çoğaltmayı durdurmak için aşağıdaki adımları izleyin:

1. Azure portalında, ana Azure veritabanınızı PostgreSQL sunucusuna seçin.

2. Sunucu menüsünde altında **ayarları**seçin **çoğaltma**.

3. Çoğaltma sunucusu için çoğaltma durdurma kendisine seçin.

   ![Yineleme seçin](./media/howto-read-replicas-portal/select-replica.png)
 
4. Seçin **çoğaltmayı durdurma**.

   ![Çoğaltmayı durdurma seçin](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. Seçin **Tamam** çoğaltma durdurma.

   ![Çoğaltma durdurma onaylayın](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Bir ana sunucu silme
Ana sunucu silmek için bir PostgreSQL sunucusu için Azure veritabanı başına silmek için aynı adımları kullanın. 

> [!IMPORTANT]
> Ana sunucu sildiğinizde tüm salt okunur çoğaltmalar için çoğaltma durdurulur. Salt okunur çoğaltmaların artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucuları olur.

Azure portaldan sunucu silmek için bu adımları izleyin:

1. Azure portalında, ana Azure veritabanınızı PostgreSQL sunucusuna seçin.

2. Açık **genel bakış** sunucu sayfası. **Sil**’i seçin.

   ![Ana sunucu silmek için sunucu genel bakış sayfasında'ı seçin](./media/howto-read-replicas-portal/delete-server.png)
 
3. Silmek için ana sunucu adını girin. Seçin **Sil** ana sunucu silme işlemini onaylamak için.

   ![Ana sunucu silmek için onaylayın](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Bir çoğaltmayı Sil
Ana sunucu nasıl silmek için benzer bir okuma çoğaltması silebilirsiniz.

- Azure portalında açın **genel bakış** okuma çoğaltma sayfası. **Sil**’i seçin.

   ![Kopya genel görünümü sayfasında, çoğaltmayı silmeyi seçin](./media/howto-read-replicas-portal/delete-replica.png)
 
Okuma çoğaltmadan silebilirsiniz **çoğaltma** penceresinde aşağıdaki adımları izleyerek:

1. Azure portalında, ana Azure veritabanınızı PostgreSQL sunucusuna seçin.

2. Sunucu menüsünde altında **ayarları**seçin **çoğaltma**.

3. Salt okunur bir çoğaltmayı silmek için seçin.

   ![Çoğaltmayı silmek için seçin](./media/howto-read-replicas-portal/select-replica.png)
 
4. Seçin **çoğaltmayı Sil**.

   ![Çoğaltma silmeyi seçme](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. Çoğaltmayı silmek için adı girin. Seçin **Sil** çoğaltmayı silme işlemini onaylamak için.

   ![Metin çoğaltmayı Silmeyi Onayla](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Bir yineleme izleme
İki ölçüm salt okunur çoğaltmalar izlemek kullanılabilir.

### <a name="max-lag-across-replicas-metric"></a>Ölçüm en fazla gecikme arasında çoğaltma sayısı
**Arasında en fazla gecikme çoğaltmaları** ölçüm bayt ana sunucu ve çoğu İzolasyonu çoğaltma arasındaki gecikme gösterir. 

1.  Azure portalında PostgreSQL sunucusu için ana Azure veritabanını seçin.

2.  **Ölçümler**’i seçin. İçinde **ölçümleri** penceresinde **arasında en fazla gecikme çoğaltmaları**.

    ![En fazla gecikme genelinde çoğaltma izleme](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  İçin **toplama**seçin **Max**.


### <a name="replica-lag-metric"></a>Çoğaltma gecikmesi ölçüm
**Çoğaltma gecikmesi** ölçüm son bir çoğaltma üzerinde işlem durumdayken bu yana zamanı gösterir. Ana şablonunuzu üzerinde gerçekleşen işlem varsa, bu zaman gecikmesini ölçüm yansıtır.

1. Azure portalında çoğaltma okuma PostgreSQL için Azure veritabanını seçin.

2. **Ölçümler**’i seçin. İçinde **ölçümleri** penceresinde **çoğaltma gecikmesi**.

   ![İzleyici çoğaltma gecikmesi](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. İçin **toplama**seçin **Max**. 
 
## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [PostgreSQL için Azure veritabanı'nda çoğaltmaları okuma](concepts-read-replicas.md).