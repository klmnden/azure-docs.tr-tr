---
title: StorSimple sanal dizisi birimlerde yönetme | Microsoft Docs
description: StorSimple cihaz Yöneticisi'ni ve StorSimple Virtual Array'iniz birimlerde yönetmek için kullanma işlemini açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: a233a9deb58a7c1abc87a622a4f1f2581ee2e477
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62125806"
---
# <a name="use-storsimple-device-manager-service-to-manage-volumes-on-the-storsimple-virtual-array"></a>StorSimple sanal dizisi birimlerde yönetmek için StorSimple cihaz Yöneticisi'ni kullanın hizmeti

## <a name="overview"></a>Genel Bakış

Bu öğreticide, oluşturmak ve StorSimple Virtual Array'iniz birimlerde yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanmayı açıklar.

StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzü tek bir web arabiriminden yönetmenizi sağlayan Azure portalında bir uzantısıdır. Paylaşımları ve birimler yönetmenin yanı sıra görüntülemek ve cihazları yönetmek, uyarıları görüntüleyin ve görüntülemek ve yedekleme ilkelerini ve yedekleme kataloğunu yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanabilirsiniz.

## <a name="volume-types"></a>Birim türü

StorSimple birimlerini olabilir:

* **Yerel olarak sabitlenmiş**: Bu birimlerdeki veriler dizide her zaman kalır ve buluta dağıtılmamasını.
* **Katmanlı**: Bu birimlerdeki verileri buluta sığdırmaya. Katmanlı birim oluşturduğunuzda, yerel katmanında yaklaşık % 10 alanı sağlandığında ve bulutta % 90 ' alanı sağlandığında. Örneğin, 1 TB'lik bir birimi sağladıysanız, 100 GB yerel alanı bulunması ve 900 GB bulutta kullanılabilir olduğunda veri katmanları. Bu sırayla cihazda yerel alanın tümünü dışında çalıştırırsanız, (% 10 yerel katmanına gerekli kullanılamaz çünkü), bir katmanlı birim sağlayamayacağınızı anlamına gelir.

### <a name="provisioned-capacity"></a>Sağlanan kapasite
Her birim türü için sağlanan kapasite üst sınırı için aşağıdaki tabloya bakın.

| **Sınır tanımlayıcı**                                       | **Sınırı**     |
|------------------------------------------------------------|---------------|
| En düşük katmanlı birim boyutu                            | 500 GB        |
| Katmanlı birim en büyük boyutu                            | 5 TB          |
| Yerel olarak sabitlenmiş bir birim, en küçük boyut                    | 50 GB         |
| Yerel olarak sabitlenmiş bir birim, en büyük boyutu                    | 500 GB        |

## <a name="the-volumes-blade"></a>Birimleri dikey penceresi
**Birimleri** StorSimple hizmeti Özet dikey menüsünde verilen bir StorSimple dizi üzerinde depolama birimleri listesini görüntüler ve bunları yönetmenize olanak sağlar.

![Birimleri dikey penceresi](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

Birim öznitelikleri bir dizi oluşur:

* **Birim adı** – benzersiz olmalıdır ve birim belirlemeye yardımcı olur ve açıklayıcı bir ad.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir birimi çevrimdışı ise, birim kullanmak erişim izni verilen başlatıcılara (Sunucuları) görünür değil.
* **Tür** – birimin olup olmadığını belirten **katmanlı** (varsayılan) veya **yerel olarak sabitlenmiş**.
* **Kapasite** – (sunucu) başlatıcı tarafından depolanan verilerin toplam miktarı karşılaştırıldığında kullanılan veri miktarını belirtir.
* **Yedekleme** – StorSimple sanal dizisi, tüm birimler için Yedekleme otomatik olarak etkin durumda.
* **Bağlı Konaklar** – bu birimi erişim izni verilen başlatıcıları (Sunucuları) belirtir.

![Hacimleri ayrıntıları](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Yönergeleri Bu öğreticide aşağıdaki görevleri gerçekleştirmek için kullanın:

* Birim Ekle
* Bir birimi Değiştir
* Bir birimi çevrimdışı duruma getirin
* Birim Sil

## <a name="add-a-volume"></a>Birim Ekle

1. StorSimple hizmet özeti dikey penceresinden tıklayın **+ birim Ekle** komut çubuğundan. Bu açılır **birim ekleme** dikey penceresi.
   
    ![Birim ekle](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. İçinde **birim ekleme** dikey penceresinde aşağıdakileri yapın:
   
   * İçinde **birim adı** biriminiz için benzersiz bir ad girin. Ad 3 ile 127 karakter içeren bir dize olmalıdır.
   * İçinde **türü** açılan listesinde, oluşturulup oluşturulmayacağını belirtin bir **katmanlı** veya **yerel olarak sabitlenmiş** birim. Yerel GARANTİLERİN, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için seçin **birim'yerel olarak sabitlenmiş**. Diğer tüm veriler için seçin **katmanlı** birim.
   * İçinde **kapasite** alanında, birimin boyutunu belirtin. Katmanlı birim 5 TB ve 500 GB arasında olmalıdır ve yerel olarak sabitlenmiş bir birim, 50 GB ile 500 GB arasında olmalıdır.
   * * Tıklayın **bağlı Konaklar**, bu birime bağlanmak ve ardından istediğiniz iSCSI başlatıcısı için karşılık gelen bir erişim denetimi kaydı (ACR) seçin **seçin**.
3. Yeni bağlı bir konak eklemek için tıklatın **yeni Ekle**, konak ve kendi iSCSI için bir ad girin tam adı (IQN) ve ardından **Ekle**.
   
    ![Birim ekle](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. Toplu yapılandırma işlemini tamamladığınızda tıklayın **Oluştur**. Belirtilen ayarlarla bir birim oluşturulur ve oluşturma başarılı aynı bir bildirim görürsünüz. Varsayılan olarak, yedekleme birimi için etkinleştirilecek.
5. Birimi başarıyla oluşturulduğunu doğrulamak için şu adrese gidin **birimleri** dikey penceresi. Listelenen birimleri görmeniz gerekir.
   
    ![Birim oluşturma başarılı](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>Bir birimi Değiştir

Bir birim birime konakların değiştirmeniz gerektiğinde değiştirin. Diğer öznitelikleri biriminin birim oluşturulduktan sonra değiştirilemez.

#### <a name="to-modify-a-volume"></a>Bir birimi değiştirme

1. Gelen **birimleri** , değiştirmek istediğiniz birim bulunduğu sanal diziyi StorSimple hizmeti Özet dikey penceresinde ayarını seçin.
2. **Seçin** tıklatın ve birim **bağlı Konaklar** şu anda bağlı konak görüntülemek ve farklı bir sunucuya değiştirmek için.
   
    ![Toplu düzenleme](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. Değişiklikleri Kaydet'e tıklayarak **Kaydet** komut çubuğu. Belirtilen ayarlar uygulanmaz ve bir bildirim görürsünüz.

## <a name="take-a-volume-offline"></a>Bir birimi çevrimdışı duruma getirin

Değiştirmek veya silmek planlama yaparken bir birimi çevrimdışı duruma gerekebilir. Bir birimi çevrimdışı olduğunda, okuma-yazma erişimi için kullanılabilir değil. Cihaz yanı sıra konak birimi çevrimdışı duruma gerekecektir.

#### <a name="to-take-a-volume-offline"></a>Bir birimi çevrimdışı duruma getirmek için

1. Söz konusu birimi çevrimdışı duruma getirmeden önce kullanımda olmadığından emin olun.
2. Birimi çevrimdışı konakta ilk yararlanın. Bu, olası tüm birim üzerindeki veri bozulması riskini ortadan kaldırır. Belirli adımlar için ana bilgisayar işletim sisteminiz için yönergelerine bakın.
3. Konaktaki birimi çevrimdışı duruma geldikten sonra birimi aşağıdaki adımları uygulayarak dizide çevrimdışı duruma:
   
   * Gelen **birimleri** çevrimdışına size istediğiniz birim bulunduğu sanal diziyi StorSimple hizmeti Özet dikey penceresinde ayarını seçin.
   * **Seçin** tıklatın ve birim **...**  (Alternatif olarak bu satıra sağ tıklayın) ve bağlam menüsünden **çevrimdışına**.
     
        ![Çevrimdışı birim](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * Bilgileri gözden **çevrimdışına** dikey penceresinde ve işlemin onaylamış olursunuz. Tıklayın **çevrimdışına** birimi çevrimdışı duruma getirmek için. Devam eden işlemin bir bildirim görürsünüz.
   * Birimi başarıyla çevrimdışı bırakıldığını olduğunu onaylamak için Git **birimleri** dikey penceresi. Birim durumu çevrimdışı olarak görmeniz gerekir.
     
       ![Çevrimdışı toplu onaylama](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Birim Sil

> [!IMPORTANT]
> Yalnızca çevrimdışı olduğunda, bir birim silebilirsiniz.
> 
> 

Bir birimi silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için

1. Gelen **birimleri** , silmek istediğiniz birim bulunduğu sanal diziyi StorSimple hizmeti Özet dikey penceresinde ayarını seçin.
2. **Seçin** tıklatın ve birim **...**  (Alternatif olarak bu satıra sağ tıklayın) ve bağlam menüsünden **Sil**.
   
    ![Birim Sil](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Silmek istediğiniz birim durumunu denetleyin. Silmek istediğiniz birim çevrimdışı değilse, ilk olarak yer alan adımları uygulayarak çevrimdışına [bir birimi çevrimdışı duruma](#take-a-volume-offline).
4. İçinde onaylamanız istendiğinde **Sil** dikey penceresinde, onayı kabul edin ve tıklayın **Sil**. Birim artık silinir ve **birimleri** dikey güncelleştirilmiş sanal dizi içinde birimlerin listesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [bir StorSimple biriminin kopyalama](storsimple-virtual-array-clone.md).

