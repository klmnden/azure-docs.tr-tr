---
title: Azure hızlı başlangıç - oluşturma ve Windows Vm'lerinde Azure dosyaları paylaşımına kullanma | Microsoft Docs
description: Bu hızlı başlangıçta, Azure portalında bir Azure dosya paylaşımı kurulumu ve bir Windows sanal makineye bağlanın. Dosya paylaşımına bağlanma, dosyaları paylaşıma bir dosya yükleyecek. Dosya Paylaşımı anlık, Dosya paylaşımındaki dosya değiştirme sonra dosya paylaşımının önceki bir anlık görüntüye geri yükleyin.
services: storage
author: roygara
ms.service: storage
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 5109f4e801c1e34b2026cff8f8dd83558618e153
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61482602"
---
# <a name="quickstart-create-and-manage-azure-files-share-with-windows-virtual-machines"></a>Hızlı Başlangıç: Oluşturma ve Azure dosya paylaşımı ile Windows sanal makineleri yönetme

Bu makalede, Azure dosyaları oluşturma ve kullanma için temel adımlar paylaşmak gösterilmektedir. Karşılaşabileceğiniz bu hızlı başlangıçta, Vurgu hızla bir Azure dosya paylaşımı ayarlama hizmetinin nasıl çalıştığı olduğundan. Daha ayrıntılı yönergelere ihtiyacınız varsa, oluşturma ve Azure'ı kullanmak için kendi ortamınızda dosya paylaşımları, bkz: [Windows ile Azure dosya paylaşımını kullanma](storage-how-to-use-files-windows.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

Bu hızlı başlangıçta, aşağıdaki öğeleri ayarlayın:

- Bir Azure depolama hesabı ve bir Azure dosya paylaşımı
- Bir Windows Server 2016 Datacenter VM

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir Azure dosya paylaşımı ile çalışabilmek için bir Azure depolama hesabı oluşturmak zorunda. Genel amaçlı v2 depolama hesabı tüm Azure depolama hizmetlerine erişim sağlar: blobları, dosyalar, kuyruklar ve tablolar. Bu hızlı başlangıçta bir genel amaçlı v2 depolama hesabı oluşturur ancak herhangi bir türde depolama hesabı oluşturmak için adımları da buradakilere benzer. Bir depolama hesabında sınırsız sayıda paylaşım olabilir. Bir paylaşım, depolama hesabının kapasite limitlerine kadar sınırsız sayıda dosyayı depolayabilir.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Sonra, bir dosya paylaşımı oluşturacaksınız.

1. Azure depolama hesabı dağıtımı tamamlandığında seçin **kaynağa Git**.
1. Seçin **dosyaları** depolama hesabı bölmesinden.

    ![Dosyaları seçin](./media/storage-files-quick-create-use-windows/click-files.png)

1. Seçin **+ dosya paylaşımı**.

    ![Dosya Paylaşımı ekleme düğmesine seçin](./media/storage-files-quick-create-use-windows/create-file-share.png)

1. Yeni dosya paylaşımı adı *qsfileshare* > için "1" girin **kota** > seçin **Oluştur**. Kota en çok 5 TiB olabilir, ancak bu hızlı başlangıç için yalnızca 1 GiB gerekir.
1. Adında yeni bir txt dosyası oluşturma *qsTestFile* yerel makinenizde.
1. Yeni dosya paylaşımı seçin ve ardından dosya paylaşımı konumu seçin **karşıya**.

    ![Dosyayı karşıya yükleme](./media/storage-files-quick-create-use-windows/create-file-share-portal5.png)

1. Oluşturduğunuz .txt dosyanızın konumuna göz atın > seçin *qsTestFile.txt* > seçin **karşıya**.

Şu ana kadar azure'da da bir dosya ile bir Azure depolama hesabına ve dosya paylaşımı oluşturdunuz. Ardından bu hızlı başlangıçta şirket içi sunucusunu temsil edecek Windows Server 2016 Datacenter ile Azure VM oluşturmayı öğreneceksiniz.

### <a name="deploy-a-vm"></a>VM'yi dağıtma

1. Ardından, portalın sol tarafındaki menüyü genişletin ve Azure portalın sol üst köşesindeki **Kaynak oluştur**’u seçin.
1. **Azure Market** kaynaklarının listesi üzerindeki arama kutusunda, **Windows Server 2016 Datacenter**’ı arayıp seçin, ardından **Oluştur**’u seçin.
1. İçinde **Temelleri** sekmesindeki **proje ayrıntıları**, bu hızlı başlangıç için oluşturduğunuz kaynak grubunu seçin.

   ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/storage-files-quick-create-use-windows/vm-resource-group-and-subscription.png)

1. Altında **örnek ayrıntıları**, VM adı *qsVM*.
1. **Bölge**, **Kullanılabilirlik seçenekleri**, **Görüntü** ve **Boyut** için varsayılan ayarları değiştirmeden bırakın.
1. Altında **yönetici hesabı**, ekleme *VMadmin* olarak **kullanıcıadı** girin bir **parola** VM için.
1. **Gelen bağlantı noktası kuralları** altında **Seçilen bağlantı noktalarına izin ver**'i, sonra aşağı açılan listeden **RDP (3389)** ve **HTTP** değerlerini seçin.
1. **İncele ve oluştur**’u seçin.
1. **Oluştur**’u seçin. Yeni bir sanal makinenin oluşturulması birkaç dakika sürebilir.

1. VM dağıtımınız tamamlandıktan sonra seçin **kaynağa Git**.

Bu noktada yeni bir sanal makine oluşturdunuz ve bir veri diskini kullanıma açtınız. Şimdi VM’ye bağlanmanız gerekir.

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

1. Seçin **Connect** sanal makine özellikleri sayfasında.

   ![Portaldan bir Azure sanal makinesine bağlanma](./media/storage-files-quick-create-use-windows/connect-vm.png)

1. İçinde **sanal makineye bağlanma** sayfasında, saklama tarafından bağlanmak için varsayılan seçenekleri **IP adresi** üzerinden **bağlantı noktası numarası** *3389* seçin **İndirme RDP dosyası**.
1. İndirilen RDP dosyasını açın ve seçin **Connect** istendiğinde.
1. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adı olarak yazın *localhost\username*burada &lt;kullanıcıadı&gt; , oluşturduğunuz sanal makine için sanal makine yönetici kullanıcı adı. Sanal makine için oluşturduğunuz parolayı girin ve ardından **Tamam**.

   ![Diğer seçenekler](./media/storage-files-quick-create-use-windows/local-host2.png)

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** veya **devam** bağlantı oluşturmak için.

## <a name="map-the-azure-file-share-to-a-windows-drive"></a>Azure dosya paylaşımı için bir Windows sürücüsüne

1. Azure portalında gidin *qsfileshare* dosya paylaşımını seçip **Connect**.
1. İkinci kutunun, içeriğini kopyalayın ve yapıştırın **not defteri**.

   ![Azure Dosyaları Bağlan bölmesinden UNC adı](./media/storage-files-quick-create-use-windows/portal_netuse_connect2.png)

1. VM'yi açın **dosya Gezgini** seçip **bu PC** penceresinde. Bu seçim, Şeritteki kullanılabilir menüleri değiştirir. Üzerinde **bilgisayar** menüsünde **harita ağ sürücüsü**.
1. Sürücü harfini seçin ve UNC adını girin. Bu hızlı başlangıçta adlandırma önerileri izlediğinizden, kopyalama  *\\qsstorageacct.file.core.windows.net\qsfileshare* gelen **not defteri**.

   İki onay kutusunun işaretli olduğundan emin olun.

   ![“Ağ Sürücüsüne Bağlan” iletişim kutusunun ekran görüntüsü](./media/storage-files-quick-create-use-windows/mountonwindows10.png)

1. **Son**’u seçin.
1. İçinde **Windows Güvenlik** iletişim kutusunda:

   - Depolama hesabı adı ile AZURE\ $a Not Defteri'nden kopyalayın ve yapıştırın **Windows Güvenlik** iletişim kutusu yapılandırmalıdır. Bu hızlı başlangıçta adlandırma önerileri izlediğinizden, kopyalama *AZURE\qsstorageacct*.
   - Depolama hesabı anahtarını Not Defteri'nden kopyalayın ve yapıştırın **Windows Güvenlik** parolası iletişim kutusu.

      ![Azure Dosyaları Bağlan bölmesinden UNC adı](./media/storage-files-quick-create-use-windows/portal_netuse_connect3.png)

## <a name="create-a-share-snapshot"></a>Paylaşım anlık görüntüsü oluşturma

Sürücü eşleştirdik, bir anlık görüntüsü oluşturabilirsiniz.

1. Portalda, uygulamanızın dosya paylaşımına gidin ve seçin **oluşturma anlık görüntüsü**.

   ![Anlık görüntü oluşturma](./media/storage-files-quick-create-use-windows/create-snapshot.png)

1. VM'yi açın *qstestfile.txt* ve "Bu dosya değiştirildi" yazın > dosyasını kaydedin ve kapatın.
1. Başka bir anlık görüntüsünü oluşturun.

## <a name="browse-a-share-snapshot"></a>Paylaşım anlık görüntüsü Gözat

1. Dosya paylaşımınızı seçin **anlık görüntüleri görüntüle**.
1. Üzerinde **dosya paylaşımı anlık görüntüleri** bölmesinde seçin ilk anlık görüntü listesinde.

   ![Zaman damgaları listesinde seçilen anlık görüntü](./media/storage-files-quick-create-use-windows/snapshot-list.png)

1. Bu anlık görüntü için bölmeden *qsTestFile.txt*.

## <a name="restore-from-a-snapshot"></a>Anlık görüntüden geri yükleme

1. Dosya Paylaşımı anlık görüntüsü dikey penceresinden sağ *qsTestFile*seçip **geri** düğmesi.
1. Seçin **özgün dosyanın üzerine yaz**.

   ![Yükleme ve geri düğmeleri](./media/storage-files-quick-create-use-windows/snapshot-download-restore-portal.png)

1. Bir VM'de, dosyayı açın. Değiştirilmemiş sürümle geri yüklendi.

## <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme

1. Dosya paylaşımınızı seçin **anlık görüntüleri görüntüle**.
1. Üzerinde **dosya paylaşımı anlık görüntüleri** bölmesinde son anlık görüntünün listeden seçin ve tıklayın **Sil**.

   ![Sil düğmesi](./media/storage-files-quick-create-use-windows/portal-snapshots-delete.png)

## <a name="use-a-share-snapshot-in-windows"></a>Windows paylaşım anlık görüntüsünü kullanın

Olduğu gibi şirket içi VSS anlık görüntüler sayesinde, anlık görüntüleri bağlanan Azure dosya paylaşımınızı önceki sürümler sekmesini kullanarak görüntüleyebilirsiniz.

1. Dosya Gezgini'nde, bağlı paylaşım bulun.

   ![Dosya Gezgini'nde bağlı paylaşım](./media/storage-files-quick-create-use-windows/snapshot-windows-mount.png)

1. Seçin *qsTestFile.txt* ve > sağ tıklayıp **özellikleri** menüsünde.

   ![Seçilen dizin için sağ tıklama menüsü](./media/storage-files-quick-create-use-windows/snapshot-windows-previous-versions.png)

1. Bu dizine ait paylaşım anlık görüntülerinin listesini görmek için **Önceki Sürümler**'i seçin.

1. Seçin **açın** anlık görüntü açın.

   ![Önceki Sürümler sekmesi](./media/storage-files-quick-create-use-windows/snapshot-windows-list.png)

## <a name="restore-from-a-previous-version"></a>Önceki sürümü geri yükleme

1. Seçin **geri**. Bu eylem tüm dizini yinelemeli olarak içeriğini paylaşım anlık görüntünün oluşturulduğu anda özgün konuma kopyalar.

   ![Geri Yükle düğmesi uyarı iletisi](./media/storage-files-quick-create-use-windows/snapshot-windows-restore.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure dosya paylaşımını Windows'da kullanma](storage-how-to-use-files-windows.md)
