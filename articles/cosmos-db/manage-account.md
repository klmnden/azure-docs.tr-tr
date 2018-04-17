---
title: Azure Portalı aracılığıyla Azure Cosmos DB hesap yönetme | Microsoft Docs
description: Azure Portalı aracılığıyla Azure Cosmos DB hesabınızı yönetmeyi öğrenin. Görüntüleme, kopyalama, silme ve hesaplarına erişim için Azure Portal'ı kullanma hakkında bir kılavuz bulabilirsiniz.
keywords: Azure Portal, azure, Microsoft azure
services: cosmos-db
documentationcenter: ''
author: kirillg
manager: kfile
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: kirillg
ms.openlocfilehash: 9c8126c7f902beec348419c0362722a692cb393e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Bir Azure Cosmos DB hesabı yönetme
Genel tutarlılık ayarlamak, anahtarları ile çalışır ve Azure portalda bir Azure Cosmos DB hesabı silme hakkında bilgi edinin.

## <a id="consistency"></a>Azure Cosmos DB tutarlılık ayarlarını yönet
Sağ tutarlılık düzeyi seçerek uygulamanızı semantiği bağlıdır. Azure Cosmos veritabanı kullanılabilir tutarlılık düzeylerini okuyarak tanıyın [kullanılabilirliğini ve Azure Cosmos veritabanı performansını en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma][consistency]. Azure Cosmos DB tutarlılık, kullanılabilirlik ve veritabanı hesabınız için kullanılabilir her tutarlılık düzeyinde performans garantileri sağlar. Güçlü tutarlılık düzeyi olan veritabanı hesabınızı yapılandırma verilerinizi tek bir Azure bölgesine yalıtılmış ve genel olarak kullanılabilir olmasını gerektirir. Diğer yandan, gevşek tutarlılık düzeylerini - sınırlanmış eskime durumu, session veya son etkinleştir, herhangi bir sayıda Azure bölgeleri veritabanı hesabınızla ilişkilendirilecek. Aşağıdaki basit adımları veritabanı hesabınız için varsayılan tutarlılık düzeyi gösterilmektedir.

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>Bir Azure Cosmos DB hesap için varsayılan tutarlılık belirtmek için
1. İçinde [Azure portal](https://portal.azure.com/), Azure Cosmos DB hesabınıza erişemiyor.
2. Hesap sayfasında tıklatın **varsayılan tutarlılık**.
3. İçinde **varsayılan tutarlılık** sayfasında, yeni tutarlılık düzeyi seçin ve tıklayın **kaydetmek**.
    ![Varsayılan tutarlılık oturumu][5]

## <a id="keys"></a>Görüntüleme, kopyalama ve erişim anahtarları ve parolaları yeniden oluştur
Bir Azure Cosmos DB hesabı oluşturduğunuzda, iki ana erişim tuşları (veya MongoDB API hesaplar için iki parolaları) hizmeti oluşturur kullanılabilecek kimlik doğrulaması için Azure Cosmos DB hesap erişildiğinde. İki erişim tuşu sağlayarak Azure Cosmos DB kesinti olmadan Azure Cosmos DB hesabınıza anahtarları yeniden sağlar. 

İçinde [Azure portal](https://portal.azure.com/), erişim **anahtarları** sayfasında kaynak menüsünden **Azure Cosmos DB hesap** sayfasını görüntülemek, kopyalamak ve için kullanılan erişim anahtarlarını yeniden oluşturmak için Azure Cosmos DB hesabınıza erişemiyor. MongoDB API hesapları için erişim **bağlantı dizesi** görüntülemek, kopyalamak ve hesabınıza erişmek için kullanılan parolalar yeniden oluşturmak için kaynak menüsünden Sayfa.

![Azure portal ekran, anahtarları sayfası](./media/manage-account/keys.png)

> [!NOTE]
> **Anahtarları** sayfa de hesabınızdan bağlanmak için kullanılan birincil ve ikincil bağlantı dizeleri içerir [veri geçiş aracı](import-data.md).
> 
> 

Salt okunur anahtarları, bu sayfada de kullanılabilir. Okuma ve salt okunur işlemler while, siler, oluşturur ve değiştirir değil sorgular.

### <a name="copy-an-access-key-or-password-in-the-azure-portal"></a>Azure portalında bir erişim anahtarı veya parola kopyalama
Üzerinde **anahtarları** sayfasında (veya **bağlantı dizesi** sayfa MongoDB API hesapları için), tıklatın **kopyalama** anahtarı veya parolayı kopyalamak istediğiniz sağındaki düğmeyi.

![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar sayfası](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys-and-passwords"></a>Erişim anahtarları ve parolaları yeniden oluştur
Erişim tuşları (ve MongoDB API hesaplar için parolaları) Azure Cosmos DB hesabınıza düzenli aralıklarla bağlantılarınızı daha güvenli tutmaya yardımcı olmak için değiştirmeniz gerekir. İki erişim anahtarları/parolalarını bir erişim anahtarı kullanırken, bir erişim anahtarı yeniden Azure Cosmos DB hesabına bağlantılar sağlamanıza olanak tanıyan atanmış.

> [!WARNING]
> Erişim anahtarlarını yeniden geçerli anahtara bağımlı olan tüm uygulamaları etkiler. Azure Cosmos DB hesabına erişmek için erişim tuşunu kullanan tüm istemciler yeni anahtarı kullanmak üzere güncelleştirilmelidir.
> 
> 

Uygulamaları veya Azure Cosmos DB hesabı kullanarak bulut Hizmetleri varsa, anahtarları, yeniden yüklerseniz, anahtarları toplamazsanız bağlantıları kaybedeceksiniz. Aşağıdaki adımları anahtarları/parolalarınızı çalışırken söz konusu sürecini özetlemektedir.

1. Erişim anahtarı Azure Cosmos DB hesabının ikincil erişim anahtarını başvurmak için uygulama kodunuzda güncelleştirin.
2. Azure Cosmos DB hesabınız için birincil erişim tuşunu yeniden oluşturun. İçinde [Azure portal](https://portal.azure.com/), Azure Cosmos DB hesabınıza erişemiyor.
3. İçinde **Azure Cosmos DB hesabı** sayfasında, **anahtarları** (veya **bağlantı dizesi** için MongoDB hesapları **).
4. Üzerinde **anahtarları**/**bağlantı dizesi** sayfasında, üretme düğmesini tıklatın ve ardından **Tamam** yeni bir anahtar oluşturmak istediğinizi onaylamak için.
    ![Erişim anahtarlarını yeniden oluştur](./media/manage-account/regenerate-keys.png)
5. Yeni anahtar kullanımı (yaklaşık beş dakika sonra yeniden üretme) için kullanılabilir olduğunu doğruladıktan sonra yeni birincil erişim anahtarını başvurmak için uygulama kodunuzda erişim tuşu güncelleştirin.
6. İkincil erişim tuşunu yeniden oluşturun.
   
    ![Erişim anahtarlarını yeniden oluştur](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Yeni oluşturulan anahtarı Azure Cosmos DB hesabınıza erişmek için kullanılmadan önce birkaç dakika sürebilir.
> 
> 

## <a name="get-the-connection-string"></a>Bağlantı dizesi alma
Bağlantı dizesini almak için aşağıdakileri yapın: 

1. İçinde [Azure portal](https://portal.azure.com), Azure Cosmos DB hesabınıza erişemiyor.
2. Kaynak menüye tıklayın **anahtarları** (veya **bağlantı dizesi** MongoDB API hesapları için).
3. Tıklatın **kopya** düğmesine **birincil bağlantı dizesi** veya **ikincil bağlantı dizesi** kutusu. 

Bağlantı dizesinde kullanıyorsanız [Azure Cosmos DB veritabanı geçiş aracı](import-data.md), veritabanı adının bağlantı dizesine sonuna. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a> Bir Azure Cosmos DB hesabını silme
Artık kullanmadığınız Azure portaldan Azure Cosmos DB hesabı kaldırmak için hesap adına sağ tıklayın ve'ı tıklatın **hesabı Sil**.

![Azure portalda bir Azure Cosmos DB hesabı silme](./media/manage-account/deleteaccount.png)

1. İçinde [Azure portal](https://portal.azure.com/), silmek istediğiniz Azure Cosmos DB hesap erişim.
2. Üzerinde **Azure Cosmos DB hesabı** sayfasında, hesabını sağ tıklatın ve ardından **hesabı Sil**. 
3. Sonuçta elde edilen onay sayfasında, hesabı silmek istediğinizi onaylamak için Azure Cosmos DB hesap adını yazın.
4. Tıklatın **silmek** düğmesi.

![Azure portalda bir Azure Cosmos DB hesabı silme](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Sonraki adımlar
Bilgi edinmek için nasıl [Azure Cosmos DB hesabınız ile çalışmaya başlama](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
