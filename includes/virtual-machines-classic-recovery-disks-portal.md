---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 5490bdd3934b438a683ce4271fbec20b3d13735d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770863"
---
Azure’daki sanal makineniz (VM) bir önyükleme veya disk bir hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Başarısız olan bir uygulama güncelleştirmesinin, VM’yi önyükleme işleminin başarılı bir şekilde tamamlanmasını engellemesi yaygın bir örnektir. Bu makalede hatalarını düzeltmek ve orijinal VM'yi yeniden oluşturmak amacıyla sanal sabit diskinizi başka bir VM'ye bağlanmak için Azure Portal’ı nasıl kullanacağınız açıklanmıştır.


## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sorun yaşayan VM’yi silin, ancak sanal sabit diskleri koruyun.
2. Sanal sabit diski, sorun giderme amacıyla başka bir VM’ye bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Orijinal sanal sabit diskteki hataları düzeltmek için ilgili dosyaları düzenleyin veya ilgili araçları çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Orijinal sanal sabit diski kullanarak bir VM oluşturun.

## <a name="delete-the-original-vm"></a>Orijinal VM’yi silme
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Sanal sabit disk, işletim sistemi, uygulamalar ve yapılandırmaların depolandığı yerdir. VM, boyutu ve konumu tanımlayan meta verilerdir ve sanal sabit disk veya sanal ağ arabirimi kartı (NIC) gibi kaynaklara başvurur. Bir VM’ye eklenen her sanal sabit diske bir kira atanır. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Kiralama durumu, VM durdurulmuş ve serbest bırakılmış olsa bile işletim sistemi diskinin VM ile ilişkisini sürdürür.

VM’nizi kurtarmanın ilk adımı, VM kaynağını silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra hatalarını gidermek için sanal sabit diski başka bir VM’ye ekleyebilirsiniz. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Sayfanın sol tarafındaki menüde **Sanal Makineler (klasik)** seçeneğine tıklayın.
3. Sorunlu VM’yi seçin, **Diskler**’e tıklayın ve sanal sabit diskin adını belirleyin. 
4. İşletim sistemi sanal sabit diskini seçin ve **Konum**’a bakarak sanal sabit diski içeren depolama hesabını belirleyin. Aşağıdaki örnekte, ".blob.core.windows.net" dizesinin hemen önündeki dize, depolama hesabının adıdır.

    ```
    https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd
    ```

    ![VM'nin konumu hakkında görüntü](./media/virtual-machines-classic-recovery-disks-portal/vm-location.png)

5. VM'ye sağ tıklayın ve ardından **Sil**’i seçin. VM’yi silerken disklerin seçili olmadığından emin olun.
6. Yeni bir kurtarma VM’si oluşturun. Bu VM, sorunlu VM ile aynı bölge ve kaynak grubunda (bulut hizmeti) olmalıdır.
7. Kurtarma VM’sini seçin ve ardından **Diskler** > **Var Olanı Ekle**’yi seçin.
8. Mevcut sanal sabit diskinizi seçmek için **VHD Dosyası**’na tıklayın:

    ![Mevcut bir VHD'ye göz atma](./media/virtual-machines-classic-recovery-disks-portal/select-vhd-location.png)

9. Depolama hesabını > VHD kapsayıcısını > sanal sabit diski seçin, seçiminizi onaylamak için **Seç** düğmesine tıklayın.

    ![Mevcut VHD’nizi seçme](./media/virtual-machines-classic-recovery-disks-portal/select-vhd.png)

10. VHD’niz seçiliyken mevcut sanal sabit diski eklemek için **Tamam**’ı seçin.
11. Birkaç saniye sonra sanal makineniz için **Diskler** bölmesinde, mevcut sanal sabit diskiniz veri diski olarak bağlanmış görünür:

    ![Veri diski olarak bağlı olan sanal sabit disk](./media/virtual-machines-classic-recovery-disks-portal/attached-disk.png)

## <a name="fix-issues-on-the-original-virtual-hard-disk"></a>Orijinal sanal sabit diskte sorunları giderme
Mevcut sanal sabit disk bağlandığında, artık tüm bakım ve sorun giderme adımlarını gereken şekilde gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.

## <a name="unmount-and-detach-the-original-virtual-hard-disk"></a>Orijinal sanal sabit diski çıkarma
Hataları çözümlendikten sonra sanal sabit diski sorun giderme VM’nizden çıkarın. Sanal sabit diskin sorun giderme VM’siyle kira ilişkisini sonlandırana kadar sanal sabit diski başka bir VM’de kullanamazsınız.  

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Sayfanın sol tarafındaki menüde **Sanal Makineler (klasik)** seçeneğine tıklayın.
3. Kurtarma VM’sini bulun. Diskler’i seçin, diske sağ tıklayın ve ardından **Ayır**’ı seçin.

## <a name="create-a-vm-from-the-original-hard-disk"></a>Orijinal sabit diskten bir VM oluşturma

Özgün sanal sabit diskten bir VM oluşturmak için kullanın [Azure portalında](https://portal.azure.com).

1. [Azure portalda](https://portal.azure.com) oturum açın.
2. En sol üst köşesindeki portallarına select **kaynak Oluştur** > **işlem** > **sanal makine** > **gelen Galeri**.
3. **Bir görüntü seçin** bölümünde, **Disklerim**’i ve ardından orijinal sanal sabit diski seçin. Konum bilgileri kontrol edin. Bu, VM’nin dağıtılması gereken bölgedir. İleri düğmesini seçin.
4. **Sanal makine yapılandırması** bölümünde VM’nin adını yazın ve VM için bir boyut seçin.
