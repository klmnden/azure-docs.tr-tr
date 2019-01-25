---
title: SMB üzerinden Microsoft Azure Data Box için veri kopyalama | Microsoft Docs
description: Azure Data Box aracılığıyla veri kopyası hizmeti için veri kopyalama hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/24/2019
ms.author: alkohli
ms.openlocfilehash: 9d271642a432d8a149fbe468087a0598c91e7c36
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902388"
---
# <a name="tutorial-use-data-copy-service-to-directly-ingest-data-into-azure-data-box-preview"></a>Öğretici: Doğrudan Azure Data Box (Önizleme) içinde veri almak için veri kopyası hizmeti kullanın

Bu öğreticide, bir ara ana gerek olmadan veri kopyalama hizmetini kullanarak veri alımı açıklar. Veri kopyalama hizmeti, Data Box üzerinde yerel olarak çalışır, NAS cihazınızın SMB üzerinden bağlanır ve verileri Data Box'a kopyalar.

Veri kopyalama hizmeti kullanın:

- Ağa bağlı depolama (NAS) ortamları nerede ara ana bilgisayar kullanılamıyor olabilir.
- Alımı ve veri yükleme için hafta geçmesi küçük dosyaları ile. Bu hizmet, alma ve karşıya yükleme süresini önemli ölçüde artırır.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Data Box'a veri kopyalama


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladınız [Öğreticisi: Azure Data Box ' ayarlamak](data-box-deploy-set-up.md).
2. Data Box'ınızı aldınız ve sipariş durumu Portalı'nda **teslim edildi**.
3. Bağlandığınız kaynak NAS cihazının veri kopyalama için kimlik bilgileri var.
4. Yüksek hızlı bir ağa bağlı olursunuz. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı kullanılabilir değilse, 1 GbE veri bağlantı kullanır, ancak kopyalama hızı etkilenir.

## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

NAS için bağlandıktan sonra sonraki adıma veri kopyalamaktır. Veri kopyalama başlamadan önce aşağıdaki konuları gözden geçirin:

- Veri kopyalama sırasında veri boyutu için açıklanan boyutu sınırları uyduğundan emin olun [Azure depolama ve Data Box sınırları](data-box-limits.md).
- Ardından, Data Box tarafından karşıya yüklenen veriler karşıya aynı anda, Data Box dışında başka uygulamalar tarafından bu karşıya yükleme işi hataları ve veri bozulmasına neden olabilir.
- Veri kopyalama hizmeti okuma gibi veri çoğaltılmaları, hatalar veya veri bozulması görebilir.

Veri kopyalama hizmetini kullanarak verileri kopyalamak için bir iş oluşturmak gerekir. Verileri kopyalamak için bir iş oluşturmak için aşağıdaki adımları izleyin.

1. Yerel web kullanıcı Arabiriminden Data Box'ınızı, Git **Yönet > veri kopyalama**.
2. İçinde **veri kopyalama** sayfasında **Oluştur**.

    ![Tıklayın ** oluşturun ** seçeneğine ** veri ** sayfayı Kopyala](media/data-box-deploy-copy-data-via-copy-service/click-create.png)

3. İçinde **yapılandırma ve başlatma** iletişim kutusunda, aşağıdaki girişleri sağlayın.
    
    |Alan                          |Değer    |
    |-------------------------------|---------|
    |İş adı                       |Benzersiz bir ad 230'dan az karakter için bir proje. İş adı - şu karakterleri izin verilmeyen \<, \>, \|, \?, \*, \\, \:, \/, ve \\\.         |
    |Kaynak konum                |Veri kaynağı biçimde SMB yolunu belirtin: `\\<ServerIPAddress>\<ShareName>` veya `\\<ServerName>\<ShareName>`.        |
    |Kullanıcı adı                       |Kullanıcı adını `\\<DomainName><UserName>` veri kaynağına erişmek için kullanılan biçim.        |
    |Parola                       |Veri kaynağına erişmek için parola.           |
    |Hedef depolama hesabı    |Aşağı açılan listeden verileri karşıya yüklemek için hedef depolama hesabını seçin.         |
    |Hedef depolama türü       |Hedef depolama türü blok blobu, sayfa blobu veya Azure dosyaları'nı seçin.        |
    |Hedef kapsayıcı/paylaşım    |Kapsayıcı adını girin ya da hedef depolama hesabınızdaki veriler, karşıya yüklemek için paylaşabilirsiniz. Ad, bir paylaşım adı veya kapsayıcı adı olabilir. Örneğin, `myshare` veya `mycontainer`. Ayrıca biçiminde girebilirsiniz `sharename\directory_name` veya `containername\virtual_directory_name` bulutta.        |
    |Desenle eşleşen dosyaları kopyala    | Dosya adı eşleşen deseni, aşağıdaki iki şekilde girin.<ul><li>**Joker karakter ifadeleri kullanma** yalnızca `*` ve `?` joker ifadelerde desteklenir. Örneğin, bu ifade `*.vhd` .vhd uzantılı tüm dosyaları eşleşir. Benzer şekilde, `*.dl?` uzantısı olan ya da tüm dosyaları eşleşen `.dl` veya `.dll`. Ayrıca, `*foo` adları ile bitemez tüm dosyaları eşleşecektir `foo`.<br>Bu gibi durumlarda, joker karakter ifadesini doğrudan alanında girebilirsiniz. Varsayılan olarak, alana girilen değer joker karakter ifadesini kabul edilir.</li><li>**Normal ifadeleri kullanma** -POSIX tabanlı normal ifadeler desteklenir. Örneğin, bir normal ifade `.*\.vhd` sahip tüm dosyaları eşleşecektir `.vhd` uzantısı. Normal ifade için sağlamak `<pattern>` doğrudan olarak `regex(<pattern>)`. <li>Normal ifadeler hakkında daha fazla bilgi için Git [normal ifade dili - hızlı başvuru](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference).</li><ul>|
    |Dosya iyileştirme              |Etkin olduğunda, dosya 1 MB'tan az paketlenmiş alma. Bu, küçük dosyaları için veri kopyalama hızlandırır. Dosya sayısı şu ana kadar dizinler sayısını aştığında önemli zamandan ettiğimiz tasarruf görülür.        |
 
4. **Başlat**'a tıklayın. Girişleri doğrulanır ve doğrulama başarılı olursa, bir iş başlatır. Bu işin başlatılması birkaç dakika sürebilir.

    ![Bir iş Yapılandır işi başlatın ve iletişim kutusunu başlatın](media/data-box-deploy-copy-data-via-copy-service/configure-and-start.png)

5. Belirtilen ayarlarla bir iş oluşturulur. Onay kutusunu işaretleyin ve ardından duraklatma ve sürdürme, iptal edebilir ya da bir işi yeniden başlatın.

    ![Bir işi kopyalama veri sayfası aracılığıyla yönetme](media/data-box-deploy-copy-data-via-copy-service/select-job.png)
    
    - Yoğun saatlerde NAS kaynakları etkilenip varsa bu işin duraklatabilirsiniz.

        ![Bir iş duraklatılamadı](media/data-box-deploy-copy-data-via-copy-service/pause-job.png)

        Daha sonra sırasında kapalı yoğun işini sürdürebilirsiniz saat.

        ![Bir işi sürdürme](media/data-box-deploy-copy-data-via-copy-service/resume-job.png)

    - Bir işi dilediğiniz zaman iptal edebilirsiniz.

        ![Bir işi iptal](media/data-box-deploy-copy-data-via-copy-service/cancel-job.png) bir işi iptal ettiğinizde bir onay gereklidir.

        ![İşi iptal işlemini Onayla](media/data-box-deploy-copy-data-via-copy-service/confirm-cancel-job.png)

        Bir işi iptal etmek karar verirseniz, zaten kopyalanan veriler silinmez. Data Box'ınızı üzerinde kopyaladığınız herhangi bir veri silmek için cihazı sıfırlayın.

        ![Cihazı sıfırla](media/data-box-deploy-copy-data-via-copy-service/reset-device.png)

        >[!NOTE]
        > İptal etme ya da bir işi duraklatmak, kopyalanan büyük dosyaları yarı kopyalanan kalabilir. Bu dosyalar aynı durumda azure'a yüklenir. Dosyalarınızı çalışırken iptal etmek veya duraklatma doğrulamak düzgün olarak kopyalanamadı. Vstemplate dosyalarını doğrulamak için SMB paylaşımları arayın ya BOM dosyasını indirin.

    - Bir sorun gibi geçici bir hata nedeniyle aniden başarısız olması durumunda, bir işi yeniden başlatabilirsiniz. Gibi başarıyla tamamlanıp tamamlanmadığını veya hatalarla tamamlandı terminal durumuna ulaştı, bir iş yeniden başlatılamıyor. Hataları, dosya adlandırma veya dosya boyutu sorunlarından dolayı olabilir. Bu hatalar kaydedilir ancak işlem tamamlandıktan sonra işi yeniden başlatılamıyor.

        ![Başarısız işi yeniden başlatın](media/data-box-deploy-copy-data-via-copy-service/restart-failed-job.png)

        Bir hatayla karşılaşabilir ve işi yeniden başlatılamıyor ise hata günlüklerini indir ve hata günlük dosyalarına bakın. Sorunu düzelttikten sonra dosyaları kopyalamak için yeni bir proje oluşturabilirsiniz. Ayrıca [SMB üzerinden dosyaları kopyalama](data-box-deploy-copy-data.md).
    
    - Bu sürümde, bir iş silinemiyor.
    
    - Sınırsız işleri oluşturabilirsiniz, ancak en fazla 10 işleri belirli bir zamanda paralel olarak çalıştırmak.
    - En iyi duruma getirme dosya açıksa, küçük dosyaları paketlenir kopyalama performansını artırmak için alma. Bu gibi durumlarda, bir paket dosyası (GUID adı olarak) görürsünüz. Bu dosyayı karşıya yükleme sırasında paketten çıkarılan olmayacağından, bu dosyayı silmeyin.

6. İş üzerinde devam ederken **veri kopyalama** sayfası:

    - İçinde **durumu** sütun kopyalama işinin durumunu görüntüleyebilirsiniz. Durum olabilir:
        - **Çalıştıran**
        - **Başarısız**
        - **Başarılı oldu**
        - **Duraklatma**
        - **Duraklatıldı**
        - **İptal ediliyor**
        - **İptal edildi**
        - **Hatalarla tamamlandı**
    - İçinde **dosyaları** sütun sayısı ve kopyalanan toplam dosya boyutu görebilirsiniz.
    - İçinde **işlenen** sütun sayısı ve boyutu işlenir dosyaları görebilirsiniz.
    - İçinde **ayrıntıları** sütun tıklayın **görünümü** iş ayrıntılarını görmek için.
    - Kopyalama işlemi sırasında gösterildiği gibi herhangi bir hata varsa, **# hataları** sütun, Git **hata günlüğünü** sütun ve hata, sorun giderme için günlükleri indirin.

Son kopyalama işlerinin tamamlanmasını bekleyin. Bazı hatalar yalnızca günlüğe kaydedilir gibi **Bağlan ve Kopyala** sayfasında, sonraki adıma geçmeden önce hatasız kopyası işleri tamamlandığından emin emin olun.

![Herhangi bir hata ** Bağlan ve Kopyala ** sayfası](media/data-box-deploy-copy-data-via-copy-service/verify-no-errors-on-connect-and-copy.png)

Veri bütünlüğünü sağlamak için sağlama toplamı veri kopyalama sırasında satır içinde hesaplanır. Kopyalama tamamlandıktan sonra cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.
    
![Panoda boş ve kullanılan alanı doğrulama](media/data-box-deploy-copy-data-via-copy-service/verify-used-space-dashboard.png)

Kopyalama işi tamamlandığında gidebilirsiniz **göndermeye hazırlama**.

>[!NOTE]
> Göndermeye hazırlama kopyası işleri devam ederken çalıştırılamaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Data Box'a veri kopyalama


Data Box'ınızı Microsoft'a göndermeye hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)

