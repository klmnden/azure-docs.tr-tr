---
title: Dosya paylaşımının içeriğini SharePoint Online'a geçirmek için Azure veri kutusu ağır kullanın | Microsoft Docs
description: Dosya Paylaşımı içeriği paylaşma noktası, Azure veri kutusu ağır Online'a kullanarak geçirme hakkında bilgi edinmek için bu öğreticiyi kullanın.
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 06/05/2019
ms.author: alkohli
ms.openlocfilehash: 1c432ee5851115e029b55722b6b238b4672e8345
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446710"
---
# <a name="use-the-azure-data-box-heavy-to-migrate-your-file-share-content-to-sharepoint-online"></a>SharePoint Online'a erişimi, dosya paylaşımı içeriğinizi geçirmek için Azure veri kutusu ağır kullanın

SharePoint Online ve OneDrive için dosya paylaşımı içeriğinizi kolayca geçirmek için Azure veri kutusu yoğun ve SharePoint Geçiş Aracı (SPMT) kullanın. Veri kutusu ağır kullanarak, verileri aktarmak için geniş alan ağı (WAN) bağlantısında bağımlılık kaldırabilirsiniz.

Microsoft Azure Data Box cihazı Microsoft Azure portalından sipariş olanak sağlayan bir hizmettir. Terabaytlarca veriyi cihaza sunucularınızdan kopyalayın. Microsoft'a geri sevk sonra verilerinizi Azure'a kopyalanır. Veri aktarmak için istediğinize boyutuna bağlı olarak, arasından seçim yapabilirsiniz:

- [Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview) ile küçük ve orta veri kümeleri için Sipariş başına 35 TB kullanılabilir kapasite.
- [Veri kutusu](https://docs.microsoft.com/azure/databox/data-box-overview) Orta ila büyük ölçekli veri kümeleri için cihaz başına 80 TB kullanılabilir kapasite ile.
- [Veri kutusu ağır](https://docs.microsoft.com/azure/databox/data-box-heavy-overview) ile büyük veri kümeleri için cihaz başına 770 TB kullanılabilir kapasite. Veri kutusu ağır şu anda Önizleme aşamasındadır.

Bu makalede özellikle veri kutusu ağır SharePoint Online'a erişimi, dosya paylaşımı içeriğinizi geçirmek için nasıl kullanılacağı hakkında konuşuyor.

## <a name="requirements-and-costs"></a>Gereksinimleri ve maliyetler

### <a name="for-data-box-heavy"></a>Veri kutusu ağır için

- Veri kutusu ağır yalnızca Kurumsal Sözleşme (EA), bulut çözümü sağlayıcısı (CSP) veya kullanılabilir Azure sponsorluğu sunar. Aboneliğinizi yükseltin veya Microsoft Support aboneliğinizi yukarıdaki türlerinden birinin içinde kalmıyorsa başvurun [Azure aboneliğinin fiyatlandırma](https://azure.microsoft.com/pricing/).
- Veri kutusu ağır kullanmak için bir ücret yoktur. Gözden geçirdiğinizden emin olun [veri kutusu ağır fiyatlandırma](https://azure.microsoft.com/pricing/details/databox/heavy/).


### <a name="for-sharepoint-online"></a>SharePoint Online

- Gözden geçirme [SharePoint Geçiş Aracı (SPMT) için en düşük gereksinimleri](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool).

## <a name="workflow-overview"></a>İş akışı genel bakış

Bu iş akışı çevrimiçi paylaşım noktası yanı sıra Azure veri kutusu ağır üzerinde adımları gerçekleştirmeniz gerekir.
Aşağıdaki adımlar, Azure veri kutusu ağır için ilgilidir.

1. Azure veri kutusu ağır sırası.
2. Cihazınızı ayarlama ve alırsınız.
3. Şirket içi dosyanızdan veri kopyalama, cihazınızdaki Azure dosyaları için klasörü paylaşın.
4. Kopyalama tamamlandıktan sonra cihaz yönergeler doğrultusunda geri gönderin.
5. Tamamen yüklemek veri bekleyin.

Aşağıdaki adımlar, paylaşım noktası çevrimiçi ilgilidir.

6. Azure portalında bir VM oluşturun ve üzerinde Azure dosya paylaşımını bağlama.
7. Azure sanal makinesinde SPMT Aracı'nı yükleyin.
8. Azure dosya paylaşımı olarak kullanarak SPMT aracı *kaynak*.
9. Aracı son adımları tamamlayın.
10. Emin olun ve verilerinizi doğrulayın.

## <a name="use-data-box-heavy-to-copy-data"></a>Veri kutusu yoğun veri kopyalamak için kullanma

Verileri, veri kutusu ağır için kopyalamak için aşağıdaki adımları uygulayın.

1. [Veri kutusu ağır sipariş](data-box-heavy-deploy-ordered.md).
2. Veri kutusu yoğun aldıktan sonra [veri kutusu ağır kümesi](data-box-heavy-deploy-set-up.md). Kablo ve her iki düğüm Cihazınızda yapılandırmak.
3. [Verileri Azure veri kutusu ağır kopyalama](data-box-heavy-deploy-copy-data.md). Kopyalanırken emin olun:

    - Yalnızca *AzureFile* veri kutusu verileri kopyalamak için ağır klasöründe. Blok blobları veya sayfa blobları, Azure dosya paylaşımını düştüğünden verileri istediğiniz olmasıdır.
    - Bir klasördeki dosyaları kopyalayın *AzureFile* klasör. Bir alt klasörde *AzureFile* klasörüne bir dosya paylaşımı oluşturur. Doğrudan kopyalanan dosya *AzureFile* klasör başarısız olur ve bu blok blobları olarak karşıya yüklendi. Sonraki adımda vm'nizde bağlama dosya paylaşımıdır.
    - Verileri, veri kutusu ağır, her iki düğüme kopyalayın.
3. Çalıştırma [göndermeye hazırlama](data-box-heavy-deploy-picked-up.md#prepare-to-ship) Cihazınızda. Başarılı bir hazırlama göndermeye başarılı bir karşıya yükleme dosyalarının azure'a sağlar.
4. [Cihazın iade edilmesi](data-box-heavy-deploy-picked-up.md#ship-data-box-heavy-back).
5. [Azure veri yükleme doğrulayın](data-box-heavy-deploy-picked-up.md#verify-data-upload-to-azure).

## <a name="use-spmt-to-migrate-data"></a>Verileri geçirmek için SPMT kullanın

Veri kopyalama tamamlandığında Azure veri takımdan onay aldıktan sonra artık SharePoint Online verilerinizi geçirme geçebilirsiniz.

En iyi performans ve bağlantı için bir Azure sanal makine (VM) oluşturmanızı öneririz.

1. Azure Portalı'nda oturum açın ve ardından [sanal makine oluşturma](../virtual-machines/windows/quick-create-portal.md).
2. [Azure dosya paylaşımını VM üzerine](../storage/files/storage-how-to-use-files-windows.md#mount-the-azure-file-share-with-file-explorer).
3. [SharePoint Geçiş Aracı'nı indirme](https://spmtreleasescus.blob.core.windows.net/install/default.htm) ve Azure sanal Makinenize yükleyin.
4. SharePoint geçiş aracını başlatın. Tıklayın **oturum** ve Office 365 kullanıcı kimliğiniz ve parolanızı girin.
5. İstendiğinde **verileriniz nerede?** seçin **dosya paylaşımı**. Yol, verilerinizin bulunduğu Azure dosya paylaşımınızı girin.
6. Hedef konumunuz dahil olmak üzere normal, kalan yönergeleri izleyin. Daha fazla bilgi için Git [SharePoint geçiş aracının nasıl kullanılacağını](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool).

> [!IMPORTANT]
> - Verilerinizi Azure'da zaten varsa, verileri SharePoint Online alınır hızını bakılmaksızın çeşitli faktörler tarafından etkilenir. Bu faktörlerin anlama, planlama ve geçişinizi verimliliğini en üst düzeye yardımcı olur.  Daha fazla bilgi için Git [SharePoint Online ve OneDrive geçiş hızı](/sharepointmigration/sharepoint-online-and-onedrive-migration-speed).
> - SharePoint Online için veri geçişi sırasında dosyalarda mevcut izinleri kaybetme riski yoktur. Gibi belirli meta veriler kaybedebilir *tarafından oluşturulan* ve *değiştirme tarihi*.

## <a name="next-steps"></a>Sonraki adımlar

[Veri kutusu ağır sipariş](./data-box-heavy-deploy-ordered.md)