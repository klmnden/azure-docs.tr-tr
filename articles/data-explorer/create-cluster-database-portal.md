---
title: 'Hızlı Başlangıç: Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma'
description: Bu hızlı başlangıçta Azure Veri Gezgini kümesi ve veritabanı oluşturup veri almayı (yüklemeyi) öğreneceksiniz.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: 1ab6fe13df111b5f56da1f368fc0dacf0a6206fc
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408842"
---
# <a name="quickstart-create-an-azure-data-explorer-cluster-and-database"></a>Hızlı Başlangıç: Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir *küme* ve bu kümenin içinde bir veya daha fazla *veritabanı* oluşturmanız gerekir. Ardından veritabanına veri *alarak* (yükleyerek) sorgu çalıştırabilirsiniz. Bu hızlı başlangıçta bir küme ve bir veritabanı oluşturacaksınız. Veri alma adımları sonraki makalelerde gösterilecektir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-cluster"></a>Küme oluşturma

Azure Veri Gezgini kümesini işlem ve depolama kaynakları belirlenmiş olan bir Azure kaynak grubunda oluşturabilirsiniz.

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

1. *Azure Veri Gezgini* için arama yapın.

   ![Kaynak arama](media/create-cluster-database-portal/search-resources.png)

1. **Azure Veri Gezgini**'nin altında, ekranın en alt bölümünde **Oluştur**'u seçin.

1. Kümeniz için benzersiz bir ad girin, aboneliğinizi seçin ve *test-resource-group* adlı bir kaynak grubu oluşturun.

    ![Kaynak grubu oluşturma](media/create-cluster-database-portal/create-resource-group.png)

1. Formu aşağıdaki bilgilerle doldurun.

   ![Küme formu oluşturma](media/create-cluster-database-portal/create-cluster-form.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Küme adı | Benzersiz küme adı | Kümenizi tanımlayan benzersiz bir ad seçin. Örneğin, *mytestcluster*. Girdiğiniz küme adının sonuna *[bölge].servicebus.windows.net* etki alanı adı eklenir. Ad yalnızca küçük harf ve sayı içerebilir. 3-22 karakter arası uzunlukta olmalıdır.
    | Abonelik | Aboneliğiniz | Kümeniz için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | *test-resource-group* | Yeni bir kaynak grubu oluşturun. |
    | Konum | *Batı ABD* | Bu hızlı başlangıç için *Batı ABD* değerini seçin. Üretim sisteminde ihtiyaçlarınıza en uygun bölgeyi seçmeniz gerekir.
    | İşlem belirtimleri | *D13_v2* | Bu hızlı başlangıç için en düşük fiyatlı özelliği seçin. Üretim sisteminde ihtiyaçlarınıza en uygun özelliği seçmeniz gerekir.
    | | |

1. Kümeyi sağlamak için **Oluştur**’u seçin. Sağlama işlemi genellikle on dakika kadar sürer. Araç çubuğunda **Bildirimler**’i seçip sağlama işlemini izleyin.

    ![Bildirimler](media/create-cluster-database-portal/notifications.png)

1. İşlem tamamlandığında **Bildirimler**'i ve ardından **Kaynağa git**'i seçin.

    ![Kaynağa git](media/create-cluster-database-portal/notification-resource.png)

## <a name="create-a-database"></a>Veritabanı oluşturma

Artık işlemin ikinci adımı olan veritabanı oluşturma bölümüne geçebilirsiniz.

1. **Genel bakış** sekmesinde **Veritabanı oluştur**'u seçin.

    ![İkinci adım: Veritabanı oluşturma](media/create-cluster-database-portal/database-creation.png)

1. Formu aşağıdaki bilgilerle doldurun.

    ![Veritabanı oluşturma formu](media/create-cluster-database-portal/create-database.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veritabanı adı | *TestDatabase* | Veritabanı adı küme içinde benzersiz olmalıdır.
    | Bekletme süresi | *3650* | Kendisi için bu verileri sorgulamak kullanılabilen tutulur sağlanır zaman aralığı (gün cinsinden). Zaman aralığı verilerin alındığı andan itibaren hesaplanır.
    | Önbellek süresi | *31* | Zaman aralığı (gün) SSD depolama veya RAM, sık sorgulanan veriler kullanılabilir tutmak istediğiniz yerine daha uzun vadeli depolama.
    | | | |

1. Veritabanını oluşturmak için **Kaydet**'i seçin. Oluşturma işlemi genellikle bir dakikadan kısa sürer. İşlem tamamlandığında yeniden kümenin **Genel bakış** sekmesi görüntülenir.

## <a name="run-basic-commands-in-the-database"></a>Veritabanında temel komutları çalıştırma

Artık bir kümeye ve veritabanına sahip olduğunuza göre sorgu ve komut çalıştırabilirsiniz. Veritabanında henüz veri yok ancak yine de araçların nasıl çalıştığını görebilirsiniz.

1. Kümenizde **Sorgu**'yu seçin.

    ![Veritabanı sorgusu](media/create-cluster-database-portal/query-database.png)

1. Şu komutu sorgu penceresine kopyalayın: `.show databases`, ardından **Çalıştır**'ı seçin.

    ![Veritabanlarını göster komutu](media/create-cluster-database-portal/show-databases.png)

    Sonuç kümesi, kümedeki tek veritabanı olan **TestDatabase** veritabanını gösterir.

1. Şu komutu sorgu penceresine kopyalayın: `.show tables`, ardından pencerede komutu seçin. **Çalıştır**'ı seçin.

    Henüz bir tablonuz olmadığı için bu komut boş sonuç kümesi döndürür. Bu dizinin bir sonraki makalesinde tablo ekleyeceksiniz.

## <a name="stop-and-restart-the-cluster"></a>Kümeyi durdurma ve yeniden başlatma

İhtiyaç duymanız halinde kümeyi durdurup yeniden başlatabilirsiniz.

1. Kümeyi durdurmak için **Genel bakış** sekmesinin en üstünden **Durdur**'u seçin.

    Küme durdurulduğunda veriler sorgulanamaz ve yeni veri alınamaz.

1. Kümeyi yeniden başlatmak için **Genel bakış** sekmesinin en üstünden **Başlat**'ı seçin.

    Küme yeniden başlatıldığında kullanılabilir duruma gelmesi (ilk sağlandığında olduğu gibi) yaklaşık on dakika sürebilir. Verilerin sık erişim önbelleğine yüklenmesi daha uzun zaman alabilir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarımızı ve öğreticilerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları tutun. Aksi takdirde maliyet oluşmasını önlemek için **test-resource-group** adlı grubu silin.

1. Azure portalında, en solda bulunan **Kaynak grupları**’nı ve ardından oluşturduğunuz kaynak grubunu seçin.  

    Soldaki menü daraltılmışsa, genişletmek için ![Genişletme düğmesi](media/create-cluster-database-portal/expand.png) öğesine tıklayın.

   ![Silinecek kaynak grubunu seçin](media/create-cluster-database-portal/delete-resources-select.png)

1. **test-resource-group** altında **Kaynak grubunu sil**'i seçin.

1. Yeni pencerede silinecek kaynak grubunun adını yazın (*test-resource-group*) ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure veri Gezgini'ne olay Hub'ından veri alma](ingest-data-event-hub.md)


