---
title: 'Hızlı başlangıç: Microsoft Azure Data Box Disk | Microsoft Docs'
description: Azure Data Box Disk'inizi Azure portalda hızlıca dağıtmak için bu hızlı başlangıçtan faydalanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: quickstart
ms.date: 02/26/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to quickly deploy Data Box Disk so as to import data into Azure.
ms.openlocfilehash: 65bf4e973ce33b2898abf585fe306a8bc85c64a0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477795"
---
::: zone target="docs"

# <a name="quickstart-deploy-azure-data-box-disk-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure Data Box Disk dağıtma

::: zone-end

::: zone target="chromeless"

# <a name="get-started-with-azure-data-box-disk-using-azure-portal"></a>Azure Data Box Azure portalını kullanarak Disk ile çalışmaya başlama

::: zone-end

::: zone target="docs"

Bu hızlı başlangıçta Azure portalı kullanarak Azure Data Box Disk'i dağıtma adımları anlatılmaktadır. Bu adımlar sipariş oluşturma, diskleri alma, paketini açma, bağlama, disklere veri kopyalama ve Azure'a yükleme işlemlerini kapsamaktadır.

Ayrıntılı adım adım dağıtım ve izleme yönergeleri için Git [Öğreticisi: Azure Data Box Disk sipariş](data-box-disk-deploy-ordered.md). 

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

::: zone-end

::: zone target="chromeless"

Bu hızlı başlangıçta Azure portalı kullanarak Azure Data Box Disk'i dağıtma adımları anlatılmaktadır. Adımları gözden geçirme Önkoşullar dahil etme, diskleri kilidini, bağlanın ve Azure'a yükler, böylece verileri disklere kopyalayın.

::: zone-end

::: zone target="docs"

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- Aboneliğinizde Azure Data Box hizmetinin etkinleştirildiğinden emin olun. Bu hizmeti aboneliğinizde etkinleştirmek için bkz. [Hizmete kaydolma](https://aka.ms/azuredataboxfromdiskdocs).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs) adresinden Azure portalında oturum açın.

::: zone-end

::: zone target="chromeless"

## <a name="prerequisites"></a>Önkoşullar

- Kullanma Data Box Disk Siparişiniz yerleştirdiğiniz [Öğreticisi: Azure Data Box Disk sipariş](data-box-disk-deploy-ordered.md).
- Disklerinizi aldınız ve portaldaki iş durumu **Teslim Edildi** olarak güncelleştirildi.
- Kullanılabilir bir istemci bilgisayar, verileri kopyalamak sahip. İstemci bilgisayarınızda:

    - Çalıştıran bir [işletim sistemi desteklenen](data-box-disk-system-requirements.md#supported-operating-systems-for-clients).
    - Sahip [diğer gerekli yazılımı](data-box-disk-system-requirements.md#other-required-software-for-windows-clients) Windows istemci ise yüklü.

::: zone-end

::: zone target="docs"

## <a name="order"></a>Sipariş verme

Bu adım yaklaşık 5 dakika sürer.

1. Azure portalda yeni bir Azure Data Box kaynağı oluşturun. 
2. Bu hizmetin etkinleştirildiği bir aboneliği seçin ve aktarım türünü **İçeri aktarma** olarak belirleyin. Verilerin bulunduğu **Kaynak ülkeyi** ve veri aktarımı için **Azure hedef bölgesini** seçin.
3. **Data Box Disk**'i seçin. Maksimum çözüm kapasitesi 35 TB olarak belirlenmiştir ve daha büyük veriler için birden fazla disk siparişi oluşturabilirsiniz.  
4. Sipariş ayrıntılarını ve sevkiyat bilgilerini girin. Hizmet bölgenizde kullanılabilir durumdaysa bildirim e-posta adreslerini girin, özeti gözden geçirin ve siparişi oluşturun.

Sipariş oluşturulduktan sonra diskler gönderilmek üzere hazırlanır.

## <a name="unpack"></a>Paketi açma

Bu adım yaklaşık 5 dakika sürer.

Data Box Disk, UPS Express Box içinde gönderilir. Kutuyu açın ve içinde şunların bulunduğundan emin olun:

- Baloncuklu ambalaja sarılı 1 ile 5 USB disk.
- Disk başına bir bağlantı kablosu.
- İade için sevkiyat etiketi.

## <a name="connect-and-unlock"></a>Bağlama ve kilidini açma

Bu adım yaklaşık 5 dakika sürer.

1. Diski desteklenen bir işletim sisteminin çalıştırıldığı Windows/Linux bilgisayarına bağlamak için, verilen kabloyu kullanın. Desteklenen işletim sistemi sürümleri hakkında daha fazla bilgi için bkz. [Azure Data Box Disk sistem gereksinimleri](data-box-disk-system-requirements.md). 
2. Disk kilidini açmak için:

    1. Azure portalında **Genel > Cihaz Ayrıntıları**'na gidin ve destek anahtarını alın.
    2. İşletim sistemine özgü Data Box Disk kilit açma aracını disklere veri kopyalamak için kullanılacak bilgisayara indirin ve ayıklayın. 
    3. Data Box Disk kilit açma aracını çalıştırın ve destek anahtarını sağlayın. Yeniden takılan tüm diskler için kilit açma aracını tekrar çalıştırın ve destek anahtarını sağlayın. **Disk kilidini açmak için BitLocker iletişim kutusunu veya BitLocker anahtarını kullanmayın.** Diskleri kilidini açma hakkında daha fazla bilgi için Git [Windows istemci disklerde kilidini](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) veya [Linux istemci disklerde kilidini](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client).
    4. Diske araç tarafından atanan sürücü harfi görüntülenir. Disk sürücü harfini not edin. Bu harf sonraki adımlarda kullanılacaktır.

## <a name="copy-data-and-validate"></a>Verileri kopyalama ve doğrulama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Sürücüsünde yer *PageBlob*, *BlockBlob*, *AzureFile*, *ManagedDisk*, ve *DataBoxDiskImport* klasörleri. Blok blobu olarak içeri aktarılması gereken verileri sürükleyip *BlockBlob* klasörüne bırakın. Benzer şekilde, sürükle ve bırak VHD/VHDX gibi verileri *PageBlob* klasörü ve uygun verilerin *AzureFile*. Yönetilen disklere altında bir klasör olarak karşıya yüklemek istediğiniz VHD kopyalama *ManagedDisk*.

    *BlockBlob* ve *PageBlob* klasörlerinin altındaki her klasör için Azure depolama hesabında bir kapsayıcı oluşturulur. Bir dosya paylaşımı için bir alt klasörü altında oluşturulur *AzureFile*.

    *BlockBlob* ve *PageBlob* klasörlerinin altındaki tüm dosyalar Azure Depolama hesabındaki varsayılan `$root` kapsayıcısına kopyalanır. Bir klasördeki dosyaları kopyalayın *AzureFile*. Doğrudan kopyaladığınız dosyalarla *AzureFile* klasör başarısız olur ve bu blok blobları olarak karşıya yüklendi.

    > [!NOTE]
    > - Tüm kapsayıcıları, blobları ve dosyaları uyması gereken [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). Bu kurallara uyulmaması halinde veriler Azure'a yüklenemez.
    > - Dosyaları ~4.75 TiB blok blobları, sayfa blobları için ~ 8 TiB ve Azure dosyaları için yaklaşık 1 TiB aşmamasını sağlayın.

2. **(İsteğe bağlı ancak önerilir)**  Kopyalama tamamlandıktan sonra en azından, çalıştırmanızı öneririz `DataBoxDiskValidation.cmd` sağlanan *DataBoxDiskImport* klasörü ve select seçenek vstemplate dosyalarını doğrulamak için 1. Ayrıca, zaman sorgulamasına öneririz 2. seçenek de sağlama toplamı doğrulama için oluşturmak için kullandığınız (sürebilir veri boyutuna bağlı olarak). Bu adımları verileri Azure'a karşıya yüklenirken hataları olasılığını en aza indirin.
3. Güvenli bir şekilde sürücüsünü kaldır.

## <a name="ship-to-azure"></a>Azure'a gönderme

Bu adımın tamamlanması yaklaşık 5-7 dakika sürer.

1. Tüm diskleri orijinal paketine yerleştirin. Paketle gönderilen sevkiyat etiketini kullanın. Etiket hasar gördüyse veya kaybolduysa portaldan indirin. **Genel bakış**'a gidin ve komut çubuğundan **Sevkiyat etiketini indirin**'e tıklayın.
2. Kapattığınız paketi kargoya verin.  

Data Box Disk hizmeti bir e-posta bildirimi gönderir ve Azure portalında sipariş durumunu güncelleştirir.

## <a name="verify-your-data"></a>Verilerinizi doğrulama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Data Box Disk, Azure veri merkezi ağına bağlandığında veriler otomatik olarak Azure'a yüklenir.
2. Veri kopyalama işlemi tamamlandığında Azure Data Box hizmeti Azure portaldan bildirim gönderir.
    
    1. Hata günlüklerini kontrol ederek hata olup olmadığını kontrol edin ve gerekli eylemleri gerçekleştirin.
    2. Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu adımın tamamlanması 2-3 dakika sürer.

Temizlemek için Data Box siparişini iptal edebilir ve ardından silebilirsiniz.

- Data Box siparişini işleme alınmadan önce Azure portaldan iptal edebilirsiniz. Siparişler işleme alındıktan sonra iptal edilemez. Sipariş, tamamlanma aşamasına gelene kadar ilerler.

    Siparişi iptal etmek için **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.  

- Siparişin durumu **Tamamlandı** veya **İptal edildi** olduğunda Azure portaldan silebilirsiniz.

    Siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure'a veri aktarımı konusunda yardım almak için Azure Data Box Disk'i dağıttınız. Azure Data Box Disk yönetimi hakkında daha fazla bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Azure portalı kullanarak Data Box Disk'i yönetme](data-box-portal-ui-admin.md)

::: zone-end
