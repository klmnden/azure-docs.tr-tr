---
title: Azure veri kutusu Edge paylaşımı Yönetim | Microsoft Docs
description: Azure veri kutusu Ucunuzdaki paylaşımlarında yönetmek için Azure portalını kullanmayı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/20/2019
ms.author: alkohli
ms.openlocfilehash: ec5fbffdf7df5ef3a952e21b79ab02f355fb8e29
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403655"
---
# <a name="use-the-azure-portal-to-manage-shares-on-your-azure-data-box-edge"></a>Azure veri kutusu Ucunuzdaki paylaşımlarında yönetmek için Azure portalını kullanma

Bu makalede, Azure veri kutusu Ucunuzdaki paylaşımlarında yönetmek açıklar. Azure veri kutusu Edge Azure portal aracılığıyla yönetmenize veya yerel web kullanıcı Arabirimi. Ekle, Sil, paylaşımlar veya eşitleme paylaşımıyla ilişkili depolama hesabı için depolama anahtarı yenileme için Azure portalını kullanın.

## <a name="about-shares"></a>Paylaşımlar hakkında

Verileri Azure'a aktarmak için Azure veri kutusu Ucunuzdaki paylaşımları oluşturmanız gerekir. Veri kutusu Edge cihazında eklediğiniz paylaşımları yerel paylaşımları veya Bulut veri gönderme paylaşımları olabilir.

 - **Yerel paylaşımları**: Cihazda yerel olarak işlenecek verileri istediğinizde bu paylaşımları kullanın.
 - **Paylaşımları**: Cihaz verileri bulut depolama hesabınıza otomatik olarak gönderilen istediğinizde bu paylaşımları kullanın. Tüm bulut işlevleri gibi **Yenile** ve **eşitleme depolama anahtarlarını** paylaşımlarına uygulanır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşımı silme
> * Paylaşımları yenileme
> * Depolama anahtarını eşitleme


## <a name="add-a-share"></a>Paylaşım ekleme

Paylaşım oluşturmak için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **ağ geçidi > paylaşımları**. Seçin **+ Ekle paylaşımı** komut çubuğunda.

    ![Paylaşım Ekle'yi seçin](media/data-box-edge-manage-shares/add-share-1.png)

2. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın.
    
    Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.

3. Paylaşım için **Tür** seçin. Tür **SMB** veya **NFS** olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

4. Paylaşımın duracağı **Depolama hesabını** sağlamanız gerekir. Henüz kapsayıcı yoksa, depolama hesabında paylaşım adıyla bir kapsayıcı oluşturulur. Kapsayıcı zaten varsa, bu var olan kapsayıcı kullanılır.

5. Açılan listeden seçin **depolama hizmeti** blok blobu, sayfa blobu ya da dosyaları. Seçilen hizmetin türü, verilerin Azure'da hangi biçimde tutulmasını istediğinize bağlıdır. Örneğin, bu örnekte, blok blobları olarak Azure'da yer alan verileri istiyoruz, bu nedenle seçiyoruz **blok blobu**. Tercih **sayfa blobu**, verilerinizin 512 bayt hizalı olmasını sağlamalısınız. Kullanım **sayfa blobu** VHD veya VHDX 512 bayt hizalı her zaman olan.

6. Bu adım SMB paylaşımı mı yoksa NFS paylaşımı mı oluşturduğunuza bağlıdır.
    - **SMB paylaşımı oluşturuyorsanız** - **Tüm ayrıcalıklara sahip yerel kullanıcı** alanında **Yeni oluştur**'u veya **Var olanı kullan**'ı seçin. Yeni bir yerel kullanıcı oluşturuluyorsa, **kullanıcı adı** ve **parola** sağlayın, sonra da parolayı onaylayın. Bu, yerel kullanıcıya izinleri atar. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.

        ![SMB paylaşımı ekleme](media/data-box-edge-manage-shares/add-smb-share.png)

        Bu paylaşımın verileri için Yalnızca okuma işlemlerine izin ver'i işaretlerseniz, salt okuma kullanıcıları belirtebilirsiniz.
    - **NFS paylaşımı oluşturuluyorsa** - Paylaşıma erişmesine **izin verilen istemcilerin IP adreslerini** sağlamanız gerekir.

        ![NFS paylaşımı ekleme](media/data-box-edge-manage-shares/add-nfs-share.png)

7. Kolayca Edge işlem modüllerden paylaşımlara erişmek için yerel bağlama noktası kullanın. Seçin **paylaşımı ile Edge işlem kullanmasını** otomatik olarak paylaşımıdır böylece bundan sonra oluşturulan bağlı. Bu seçenek belirlendiğinde, Edge modülü ile yerel bağlama noktası işlem de kullanabilirsiniz.

8. Paylaşımı oluşturmak için **Oluştur**'a tıklayın. Paylaşım oluşturma işleminin devam ettiği size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir.

## <a name="add-a-local-share"></a>Yerel bir paylaşım ekleyin

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **ağ geçidi > paylaşımları**. Seçin **+ Ekle paylaşımı** komut çubuğunda.

    ![Paylaşım Ekle'yi seçin](media/data-box-edge-manage-shares/add-local-share-1.png)

2. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın.
    
    Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.

3. Paylaşım için **Tür** seçin. Tür **SMB** veya **NFS** olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

4. Kolayca Edge işlem modüllerden paylaşımlara erişmek için yerel bağlama noktası kullanın. Seçin **paylaşımı ile Edge işlem kullanmasını** böylece Edge modülü ile yerel bağlama noktası işlem kullanabilirsiniz.

5. Seçin **Edge yerel paylaşımları olarak yapılandırma**. Yerel paylaşımlarında verileri yerel olarak cihazda kalır. Bu verileri yerel olarak işleyebilir.

6. İçinde **tüm ayrıcalıklı yerel kullanıcı** alanında, aralarından seçim **Yeni Oluştur** veya **var olanı kullan**.

7. **Oluştur**’u seçin. 

    ![Yerel paylaşımı oluşturma](media/data-box-edge-manage-shares/add-local-share-2.png)

    Paylaşımı oluşturma sürüyor bir bildirim görürsünüz. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir.

    ![Paylaşımları dikey güncelleştirmeleri görüntüle](media/data-box-edge-manage-shares/add-local-share-3.png)
    
    Bu paylaşım için Edge işlem modüller için yerel mountpoint görüntülemek için paylaşım seçin.

    ![Yerel paylaşım ayrıntılarını görüntüle](media/data-box-edge-manage-shares/add-local-share-4.png)


## <a name="unmount-a-share"></a>Bir paylaşımı çıkarın

Azure portalında bir paylaşımını ayırmak için aşağıdaki adımları uygulayın.

1. Azure portalında veri kutusu Edge kaynağınıza gidin ve ardından Git **ağ geçidi > paylaşımları**.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/select-share-unmount.png)

2. Paylaşımları listesinden kaldırmak istediğiniz paylaşımı seçin. Çıkarma, paylaşımı modüllerin tarafından kullanılmadığından emin olmanız gerekir. Paylaşım modülü tarafından kullanılıyorsa, karşılık gelen modülü ile sorunları görürsünüz. Seçin **çıkarın**.

    ![Select çıkarın](media/data-box-edge-manage-shares/select-unmount.png)

3. Onayınız istendiğinde seçin **Evet**. Bu paylaşım çıkarmanız.

    ![Onayla çıkarın](media/data-box-edge-manage-shares/confirm-unmount.png)

4. Kaldırılan paylaşımıdır sonra paylaşımları listesine gidin. Göreceksiniz **işlem için kullanılan** sütun olarak paylaşım durumu gösterilir **devre dışı bırakılmış**.

    ![Paylaşım çıkarılamadı](media/data-box-edge-manage-shares/share-unmounted.png)

## <a name="delete-a-share"></a>Paylaşımı silme

Paylaşımı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Paylaşım listesinde silmek istediğiniz paylaşımı seçin ve üzerine tıklayın.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/delete-share-1.png)

2. **Sil**'e tıklayın.

    ![Sil'e tıklayın](media/data-box-edge-manage-shares/delete-share-2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın.

    ![Silmeyi onayla](media/data-box-edge-manage-shares/delete-share-3.png)

Silinmesini yansıtacak şekilde paylaşımları güncelleştirmeleri listesi.


## <a name="refresh-shares"></a>Paylaşımları yenileme

Yenileme özelliği, bir paylaşım içeriğini yenilemek sağlar. Bir paylaşımı yenilediğinizde bloblar ve dosyalar dahil olmak üzere son yenileme işleminden sonra buluta eklenmiş olan tüm Azure nesnelerini bulmak için bir arama başlatılır. Bu ek dosyalar, ardından cihaz paylaşımında içeriğini yenilemek için indirilir.

> [!IMPORTANT]
> Yerel paylaşımlar yeniden yenilenemiyor.

Paylaşımı yenilemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1.  Azure portalda **Paylaşımlar** sayfasına gidin. Yenilemek istediğiniz paylaşımı seçin ve üzerine tıklayın.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/refresh-share-1.png)

2.  **Yenile**'ye tıklayın. 

    ![Yenile'ye tıklayın](media/data-box-edge-manage-shares/refresh-share-2.png)
 
3.  Onayınız istendiğinde **Evet**’e tıklayın. Şirket içi paylaşımın içeriğinin yenilenmesi için bir iş başlatılır.

    ![Yenilemeyi onaylama](media/data-box-edge-manage-shares/refresh-share-3.png)
 
4.  Yenileme işlemi devam ederken bağlam menüsündeki Yenile seçeneği gri renkte gösterilir. Yenileme işinin durumunu görüntülemek için iş bildirimine tıklayın.

5.  Yenileme süresi Azure kapsayıcısındaki ve cihazdaki dosya sayısına göre değişir. Yenileme işlemi başarıyla tamamlandıktan sonra paylaşım zaman damgası güncelleştirilir. Yenilemede kısmi hatalar olsa da işlem başarılı kabul edilir ve zaman damgası güncelleştirilir. Yenileme hatası günlükleri de güncellenir.

    ![Güncelleştirilmiş zaman damgası](media/data-box-edge-manage-shares/refresh-share-4.png)
 
Hata varsa bir uyarı görüntülenir. Uyarıda sorunun nedeni ve düzeltme adımları yer alır. Uyarıda ayrıca güncelleştirme veya silme işleminin başarısız olduğu dosyalar da dahil olmak üzere hatanın ayrıntılı bir özetinin yer aldığı bir dosyaya bağlantı da verilir.


## <a name="sync-storage-keys"></a>Depolama anahtarlarını eşitleme

Depolama anahtarlarınız değiştirildiyse eşitlemeniz gerekir. Eşitleme, cihazın depolama hesabınızın en son anahtarlarını almasına yardımcı olur.

Depolama erişim anahtarınızı eşitlemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Kaynağınızın **Genel bakış** sayfasına gidin. Paylaşım listesinden eşitlemek istediğiniz depolama hesabıyla ilişkilendirilmiş olan bir paylaşımı seçin ve üzerine tıklayın.

    ![Paylaşım ile ilgili depolama hesabı seçin](media/data-box-edge-manage-shares/sync-storage-key-1.png)

2. **Depolama anahtarını eşitle**'ye tıklayın. Onayınız istendiğinde **Evet**’e tıklayın.

     ![Eşitleme depolama anahtarını seçin](media/data-box-edge-manage-shares/sync-storage-key-2.png)

3. Eşitleme tamamlandıktan sonra iletişim kutusunu kapatın.

>[!NOTE]
> Bu işlemi her depolama hesabı için yalnızca bir kez yapmanız gerekir. Tüm paylaşımlar aynı depolama hesabına aitse bu eylemi tüm paylaşımlar için tekrarlamanız gerekmez.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla kullanıcıları yönetme](data-box-edge-manage-users.md) hakkında bilgi edinin.
