---
title: 'Hızlı başlangıç: Microsoft Azure Data Box Disk | Microsoft Docs'
description: Azure Data Box Disk'inizi Azure portalda hızlıca dağıtmak için bu hızlı başlangıçtan faydalanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: quickstart
ms.date: 09/07/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to quickly deploy Data Box Disk so as to import data into Azure.
ms.openlocfilehash: fbe286313bce9b735665b134a5ea18ba4995548b
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712271"
---
# <a name="quickstart-deploy-azure-data-box-disk-using-the-azure-portal-preview"></a>Hızlı başlangıç: Azure portalı kullanarak Azure Data Box Disk'i dağıtma (Önizleme)

Bu hızlı başlangıçta Azure portalı kullanarak Azure Data Box Disk'i dağıtma adımları anlatılmaktadır. Bu adımlar sipariş oluşturma, diskleri alma, paketini açma, bağlama, disklere veri kopyalama ve Azure'a yükleme işlemlerini kapsamaktadır. 

Ayrıntılı dağıtım ve takip talimatları için bkz. [Öğretici: Azure Data Box Disk'i sipariş etme](data-box-disk-deploy-ordered.md). 

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!IMPORTANT]
> Data Box Disk önizleme aşamasındadır. Bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- Aboneliğinizde Azure Data Box hizmetinin etkinleştirildiğinden emin olun. Bu hizmeti aboneliğinizde etkinleştirmek için bkz. [Hizmete kaydolma](https://aka.ms/azuredataboxfromdiskdocs).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[http://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs) adresinden Azure portalında oturum açın.

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

1. Sürücüde *PageBlob*, *BlockBlob* ve *DataBoxDiskImport* klasörleri bulunur. Blok blobu olarak içeri aktarılması gereken verileri sürükleyip *BlockBlob* klasörüne bırakın. Benzer şekilde VHD/VHDX gibi verileri de sürükleyip *PageBlob* klasörüne bırakın.

    *BlockBlob* ve *PageBlob* klasörlerinin altındaki her klasör için Azure depolama hesabında bir kapsayıcı oluşturulur. *BlockBlob* ve *PageBlob* klasörlerinin altındaki tüm dosyalar Azure Depolama hesabındaki varsayılan `$root` kapsayıcısına kopyalanır.

    > [!NOTE] 
    > - Tüm kapsayıcıların ve blobların [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions) uygun olması gerekir. Bu kurallara uyulmaması halinde veriler Azure'a yüklenemez.
    > - Dosya boyutlarının blok blobları için en fazla ~4,75 TiB, sayfa blobları için ise en fazla ~8 TiB olduğundan emin olun.

2. (İsteğe bağlı) Kopyalama işlemini tamamladıktan sonra *DataBoxDiskImport* klasöründe bulunan `DataBoxDiskValidation.cmd` uygulamasını çalıştırarak doğrulama için sağlama toplamı oluşturmanız önerilir. Bu adım verilerinizin boyutuna bağlı olarak uzun sürebilir. 
3. Sürücüyü çıkarın. 

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
