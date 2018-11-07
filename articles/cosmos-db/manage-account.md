---
title: Azure portalı üzerinden bir Azure Cosmos DB hesabı yönetme | Microsoft Docs
description: Azure Portalı aracılığıyla Azure Cosmos DB hesabınızın yönetmeyi öğrenin. Görüntüleme, kopyalama, silme ve hesaplarına erişim için Azure portalını kullanarak bir kılavuzu bulabilirsiniz.
keywords: Azure portalı, azure, Microsoft azure
services: cosmos-db
author: kirillg
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: kirillg
ms.openlocfilehash: abcf51c6bd196c2ffb0bb35e2df161531a53972d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229399"
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Bir Azure Cosmos DB hesabını yönetme
Azure portalında Azure Cosmos DB hesabı silmeye genel tutarlılık ayarlamak ve anahtarları ile çalışma hakkında bilgi edinin.

## <a id="consistency"></a>Azure Cosmos DB tutarlılık ayarlarını yönetme
Doğru tutarlılık düzeyi seçme, uygulamanızın semantiği bağlıdır. Azure Cosmos DB'de kullanılabilen tutarlılık düzeyleri okuyarak planladığınızdan [kullanılabilirlik ve Azure Cosmos DB'de performans en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma][consistency]. Azure Cosmos DB, tutarlılık, kullanılabilirlik ve veritabanı hesabınız için kullanılabilir tüm tutarlılık düzeyinde performans garantileri sağlar. Veritabanı hesabınızı güçlü tutarlılık düzeyi olan yapılandırma verilerinizi tek bir Azure bölgesine yalıtılmış ve genel olarak kullanılabilir olmasını gerektirir. Diğer yandan, gevşek tutarlılık düzeyleri - sınırlanmış eskime durumu, oturum ve nihai etkinleştir dilediğiniz sayıda Azure bölgesinde veritabanı hesabınız ile ilişkilendirmenizi. Aşağıdaki basit adımları nasıl seçileceğini veritabanı hesabınız için varsayılan tutarlılık düzeyini gösterir.

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>Bir Azure Cosmos DB hesabının varsayılan tutarlılığı belirtmek için
1. İçinde [Azure portalında](https://portal.azure.com/), Azure Cosmos DB hesabınıza erişin.
2. Hesap sayfasındaki tıklayın **varsayılan tutarlılık**.
3. İçinde **varsayılan tutarlılık** sayfasında yeni tutarlılık düzeyi seçin ve tıklayın **Kaydet**.
    ![Varsayılan tutarlılık oturumu][5]

## <a id="keys"></a>Görüntüleme, kopyalama ve erişim anahtarları ve parolaları yeniden oluştur
Azure Cosmos DB hesabı oluşturduğunuzda, hizmet iki ana erişim anahtarları (veya MongoDB API hesabı için iki parola) oluşturur. kullanılabilen kimlik doğrulaması için Azure Cosmos DB hesabına erişim sağlandığında. İki erişim tuşu sağlayarak, Azure Cosmos DB, kesinti olmadan Azure Cosmos DB hesabınız için anahtarları yeniden sağlar. 

İçinde [Azure portalında](https://portal.azure.com/), erişim **anahtarları** sayfasında kaynak menüsünden **Azure Cosmos DB hesabı** sayfa görüntüleme, kopyalama ve için kullanılan erişim anahtarlarını yeniden oluşturmak için Azure Cosmos DB hesabınıza erişin. MongoDB API hesabı için erişim **bağlantı dizesi** sayfa görüntüleme, kopyalama ve hesabınıza erişmek için kullanılan parolaları yeniden için kaynak menüsünde.

![Azure portalı ekran görüntüsü, anahtarlar sayfası](./media/manage-account/keys.png)

> [!NOTE]
> **Anahtarları** sayfa hesabınızdan bağlanmak için kullanılan birincil ve ikincil bağlantı dizelerini de içerir [veri geçiş aracı](import-data.md).
> 
> 

Bu sayfada, salt okunur anahtarları de mevcuttur. Okuma ve salt okunur işlemler, süre, siler, oluşturur ve değiştirir değil sorgular.

### <a name="copy-an-access-key-or-password-in-the-azure-portal"></a>Azure portalında bir erişim anahtarı veya parolayı kopyalayın
Üzerinde **anahtarları** sayfasında (veya **bağlantı dizesi** sayfa MongoDB API'si hesapları için), tıklayın **kopyalama** anahtarı veya parolayı kopyalamak istediğiniz sağındaki düğmeye.

![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar sayfası](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys-and-passwords"></a>Erişim anahtarları ve parolaları yeniden oluştur
Erişim anahtarlarını (ve MongoDB API'si hesaplar için parolaları), Azure Cosmos DB hesabınıza düzenli aralıklarla bağlantılarınızı daha güvenli olmasını sağlamak için değiştirmeniz gerekir. Bir erişim anahtarı yeniden oluşturmak, bir erişim anahtarı kullanarak Azure Cosmos DB hesabına bağlantıları sağlamak için iki erişim anahtarları/parolaları atanır.

> [!WARNING]
> Erişim tuşlarınızı yeniden oluşturmak için geçerli anahtara bağımlı olan tüm uygulamaları etkiler. Azure Cosmos DB hesabına erişmesi için erişim tuşunu kullanan tüm istemciler yeni anahtarı kullanacak şekilde güncelleştirilmesi gerekir.
> 
> 

Uygulamalar veya Bulut Hizmetleri Azure Cosmos DB hesabı kullanarak varsa, anahtarları, yeniden anahtarları toplamazsanız, bağlantıları kaybedersiniz. Aşağıdaki adımlar, anahtarları/parolaları sıralı işlem özetlemektedir.

1. Uygulama kodunuzda Azure Cosmos DB hesabının ikincil erişim tuşunu referans için erişim anahtarı güncelleştirin.
2. Azure Cosmos DB hesabınız için birincil erişim tuşunu yeniden oluşturun. İçinde [Azure portalında](https://portal.azure.com/), Azure Cosmos DB hesabınıza erişin.
3. İçinde **Azure Cosmos DB hesabı** sayfasında **anahtarları** (veya **bağlantı dizesi** ** MongoDB hesapları için).
4. Üzerinde **anahtarları**/**bağlantı dizesi** sayfasında, yeniden Oluştur düğmesine tıklayın ve ardından tıklayın **Tamam** yeni bir anahtar oluşturmak istediğinizi onaylamak için.
    ![Erişim anahtarlarını yeniden oluştur](./media/manage-account/regenerate-keys.png)
5. Yeni anahtar kullanımı (yaklaşık beş dakika sonra yeniden oluşturma) için kullanılabilir olduğunu doğruladıktan sonra uygulama kodunuzda yeni birincil erişim tuşunu referans için erişim anahtarı güncelleştirin.
6. İkincil erişim tuşunu yeniden oluşturun.
   
    ![Erişim anahtarlarını yeniden oluştur](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Uygulamanın Azure Cosmos DB hesabınıza erişmek için yeni oluşturulan anahtarı kullanılabilmesi için önce birkaç dakika sürebilir.
> 
> 

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma
Bağlantı dizesini almak için aşağıdakileri yapın: 

1. İçinde [Azure portalında](https://portal.azure.com), Azure Cosmos DB hesabınıza erişin.
2. Kaynak menüden **anahtarları** (veya **bağlantı dizesi** MongoDB API'si hesapları için).
3. Tıklayın **kopyalama** düğmesinin yanındaki **PRIMARY CONNECTION Strıng'i** veya **ikincil bağlantı dizesi** kutusu. 

Bağlantı dizesinde kullanıyorsanız [Azure Cosmos DB veritabanı geçiş aracı](import-data.md), veritabanı adını, bağlantı dizesi sona ekleyin. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a> Bir Azure Cosmos DB hesabını Sil
Azure Cosmos DB hesabı artık kullanmakta olduğunuz Azure Portalından kaldırmak için hesap adına sağ tıklayın ve tıklayın **hesabını Sil**.

![Azure portalında bir Azure Cosmos DB hesabını silme](./media/manage-account/deleteaccount.png)

1. İçinde [Azure portalında](https://portal.azure.com/), silmek istediğiniz Azure Cosmos DB hesabına erişme.
2. Üzerinde **Azure Cosmos DB hesabı** sayfasında hesabına sağ tıklayın ve ardından **hesabı Sil**. 
3. Sonuçta elde edilen onay sayfasında, hesabı silmek istediğinizi onaylamak için Azure Cosmos DB hesap adını yazın.
4. Tıklayın **Sil** düğmesi.

![Azure portalında bir Azure Cosmos DB hesabını silme](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Sonraki adımlar
Bilgi edinmek için nasıl [Azure Cosmos DB hesabınız ile çalışmaya başlama](https://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
