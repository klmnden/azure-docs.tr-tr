---
title: Azure Data Box Gateway'i VMware'de sağlama öğreticisi | Microsoft Docs
description: İkinci Azure Data Box Gateway dağıtma öğreticisinde VMware'de sanal cihaz sağlama adımları anlatılmaktadır.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: d0c6f8723909b71501894c9363932c752c1e130c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989864"
---
# <a name="tutorial-provision-azure-data-box-gateway-in-vmware-preview"></a>Öğretici: Azure Data Box Gateway'i VMware'de sağlama (Önizleme)

## <a name="overview"></a>Genel Bakış

Bu öğreticide Data Box Gateway'i VMware ESXi 6.0 veya 6.5 çalıştıran bir ana bilgisayarda sağlama adımları anlatılmaktadır. 

Sanal cihaz sağlamak ve bağlantı kurmak için yönetici ayrıcalıklarına sahip olmanız gerekir. Sağlama ve ilk kurulum adımlarını tamamlamak yaklaşık 10 dakika sürecektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ana bilgisayarın minimum cihaz gereksinimlerini karşıladığından emin olma
> * Hiper yöneticide bir sanal cihaz sağlama
> * Sanal cihazı başlatma ve IP adresini alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!IMPORTANT]
> - Data Box Gateway önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

VMware ESXi 6.0 veya 6.5 çalıştıran ana bilgisayar sisteminde sanal cihaz sağlama önkoşulları aşağıda belirtilmiştir.

### <a name="for-the-data-box-gateway-resource"></a>Data Box Gateway kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* [Portalı Data Box Gateway için hazırlama](data-box-gateway-deploy-prep.md) adımlarını tamamladınız.
* [Portalı Data Box Gateway için hazırlama](data-box-gateway-deploy-prep.md) bölümünde anlatılan şekilde Azure portaldan VMware sanal cihaz görüntüsünü indirdiniz.

  > [!IMPORTANT]
  > Data Box Gateway üzerinde çalıştırılan yazılım yalnızca Data Box Gateway kaynağıyla kullanılabilir.

### <a name="for-the-data-box-gateway-virtual-device"></a>Data Box Gateway sanal cihazı için

Sanal cihazı dağıtmadan önce şunlardan emin olun:

* VMware (ESXi 6.0 veya 6.5) çalıştıran ve cihaz sağlamak için kullanılabilecek bir ana bilgisayar sistemine erişiminiz var.
* Ana bilgisayar sistemi sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabiliyor:

  * En az 4 çekirdek.
  * En az 8 GB RAM.
  * Bir ağ arabirimi.
  * 250 GB işletim sistemi diski.
  * Sistem verileri için 2 TB sanal disk.

### <a name="for-the-network-in-datacenter"></a>Veri merkezindeki ağ için

Başlamadan önce:

- Data Box Gateway dağıtma ağ gereksinimlerini gözden geçirin ve veri merkezi ağını gereksinimlere göre yapılandırın. Daha fazla bilgi için bkz. [Data Box Gateway ağ gereksinimleri](data-box-gateway-system-requirements.md#networking-requirements).
- Cihazın en iyi şekilde çalışması için İnternet bant genişliğinin minimumda 20 Mb/sn olduğundan emin olun.

## <a name="check-the-host-system"></a>Ana bilgisayar sistemini denetleyin

Sanal cihaz oluşturmak için şunlara ihtiyacınız vardır:

* VMware ESXi Server 6.0 veya 6.5 çalıştıran bir ana bilgisayara erişim. Ana bilgisayar sistemi sanal cihazınız için aşağıdaki kaynakları ayırabiliyor:
 
  * En az 4 çekirdek.
  * En az 8 GB RAM. 
  * İnternet trafiği için ağa bağlı bir ağ arabirimi. 
  * 250 GB işletim sistemi diski.
  * Veriler için 2 TB sanal disk.
* ESXi ana bilgisayarını yönetmek için sisteminizde VMware vSphere istemcisi yüklü.


## <a name="provision-a-virtual-device-in-hypervisor"></a>Hiper yöneticide bir sanal cihaz sağlama

Hiper yöneticinizde sanal cihaz sağlamak için aşağıdaki adımları gerçekleştirin.

1. Sanal cihaz görüntüsünü sisteminize kopyalayın. Bu sanal görüntüyü (iki dosya) Azure portaldan indirmiştiniz. Bu görüntüyü yordamın ilerleyen bölümlerinde kullanacağınız için kopyaladığınız konumu not edin.

2. vSphere istemcisini kullanarak ESXi sunucusunda oturum açın. Sanal makine oluşturmak için yönetici ayrıcalıklarınızın olması gerekir.

   ![](./media/data-box-gateway-deploy-provision-vmware/image1.png)
  
3. VMDK dosyasını ESXi sunucusuna yükleyin. Gezinti bölmesinde **Storage** (Depolama) öğesini seçin.

   ![](./media/data-box-gateway-deploy-provision-vmware/image2.png)

4. Sağ taraftaki bölmede **Datastores** (Veri depoları) öğesini seçerek VMDK dosyasını yüklemek istediğiniz yeri belirleyin. Veri deposu türü VMFS 5 olmalıdır. Ayrıca veri deposunda işletim sistemi ve veri diskleri için yeterli boş alan bulunmalıdır.
   
5. Sağ tıklayıp **Browse Datastore** (Veri Deposuna Göz At) öğesini seçin.

   ![](./media/data-box-gateway-deploy-provision-vmware/image3.png)

6. **Datastore Browser** (Veri Deposu Tarayıcısı) penceresi açılır.

   ![](./media/data-box-gateway-deploy-provision-vmware/image4.png)

7. Araç çubuğunda **Create directory** (Dizin oluştur) simgesine tıklayarak yeni bir klasör oluşturun. Klasör adını belirtin ve not edin. Sanal makine oluştururken bu klasör adını kullanacaksınız (önerilen yöntemdir). **Create directory** (Dizin oluştur) öğesine tıklayın.

   ![](./media/data-box-gateway-deploy-provision-vmware/image5.png)

8. Yeni klasör **Datastore Browser** (Veri Deposu Tarayıcısı) penceresinin sol tarafında görünür. **Upload** (Karşıya Yükle) simgesine tıklayın ve **Upload File** (Karşıya Dosya Yükle) öğesini seçin.

    ![](./media/data-box-gateway-deploy-provision-vmware/image6.png)

9. İndirdiğiniz VMDK dosyalarını bulun. İki dosya vardır. Karşıya yüklemek için dosyalardan birini seçin.

    ![](./media/data-box-gateway-deploy-provision-vmware/image7.png)

10. **Aç**'a tıklayın. VMDK dosyası belirtilen veri deposuna yüklenmeye başlar. Dosyanın karşıya yüklenmesi birkaç dakika sürebilir.
11. Karşıya yükleme işlemi tamamlandıktan sonra dosyayı oluşturduğunuz veri deposunda görebilirsiniz. Şimdi ikinci VMDK dosyasını da aynı ver deposuna yükleyin. İki dosya da yüklendikten sonra tek bir dosya olacak şekilde birleştirilir. Bu işlemin ardından dizinde tek bir dosya görürsünüz.

    ![](./media/data-box-gateway-deploy-provision-vmware/image8.png)

12. vSphere istemcisi penceresine dönün. Gezinti bölmesinde **Virtual Machines** (Sanal Makineler) öğesini seçin. Sağ taraftaki bölmede **Create/Register VM** (VM Oluştur/Kaydet) öğesine tıklayın.

    ![](./media/data-box-gateway-deploy-provision-vmware/image9.png)

13. **New Virtual Machine** (Yeni Sanal Makine) penceresi açılır. Select creation type (Oluşturma türü seçin) bölümünde **Create a new virtual machine** (Yeni sanal makine oluştur) öğesine ve ardından **Next** (İleri) öğesine tıklayın.
    ![](./media/data-box-gateway-deploy-provision-vmware/image10.png)

14. **Select a Name and OS Name and Location** (Ad, İşletim Sistemi Adı ve Konum Seçin) sayfasının **Name** (Ad) alanına sanal makineniz için bir ad girin. Bu adın 7. adımda oluşturduğunuz klasörün adıyla aynı olması gerekir (önerilen yöntemdir). **Guest OS family** (Konuk işletim sistemi ailesi) bölümünde Windows, **Guest OS version** (Konuk işletim sistemi sürümü) bölümünde ise Microsoft Windows Server 2016 (64-bit) seçimini yapın. **İleri**’ye tıklayın.

    ![](./media/data-box-gateway-deploy-provision-vmware/image11.png)

15. **Select storage** (Depolama alanı seçin) sayfasında VM'nizi sağlamak için kullanmak istediğiniz veri deposunu seçin. **İleri**’ye tıklayın.

    ![](./media/data-box-gateway-deploy-provision-vmware/image12.png)
16. **Customize settings** (Ayarları özelleştirin) sayfasında **CPU** ayarını 4, **Memory** (Bellek) ayarını 8192 MB (veya üzeri), **Hard disk 1** (Sabit disk 1) ayarını 2 TB (veya üzeri) olarak belirtin. Eklenecek **SCSI hard disk** (SCSI sabit disk) türünü seçin. Bu örnekte LSI Logic SAS seçilmiştir. **Statik IDE diskler desteklenmez.** **Hard disk 1** (Sabit disk 1), sanal veri diskidir. Sağlanan diskin boyutunu küçültemeyeceğinizi unutmayın.

    ![](./media/data-box-gateway-deploy-provision-vmware/image13.png)

    Aynı sayfada **Add hard disk** (Sabit disk ekle) öğesine tıklayıp **Existing hard disk** (Var olan sabit disk) seçeneğini belirtin. İşletim sistemi diski eklenir. 

     ![](./media/data-box-gateway-deploy-provision-vmware/image14.png)

    **New hard disk** (Yeni sabit disk) girişini görene kadar sayfayı kaydırın ve girişi genişleterek ayarları görüntüleyin. **Virtual Device Node** (Sanal Cihaz Düğümü) ayarını **IDE controller 0** (IDE denetleyicisi 0) olarak belirleyin. **İleri**’ye tıklayın.

     ![](./media/data-box-gateway-deploy-provision-vmware/image15.png)

27. **Ready to Complete** (Tamamlanmak İçin Hazır) sayfasında yeni sanal makineyle ilgili tüm ayarları gözden geçirin. CPU için 4, bellek için 8192 MB, ağ arabirimi için 1 ve Sabit disk 2 için IDE denetleyicisi 0 değerlerinin seçili olduğunu doğrulayın. **Son**'a tıklayın. 
   
    ![](./media/data-box-gateway-deploy-provision-vmware/image16.png)
    ![](./media/data-box-gateway-deploy-provision-vmware/image17.png)

Sanal makineniz sağlanır. Durumu belirten bir ileti görüntülenir ve yeni sanal makine VM listesine eklenir. 

![](./media/data-box-gateway-deploy-provision-vmware/image17.png)

Bir sonraki adım bu makineyi açmak ve IP adresini almaktır.

> [!NOTE]
> Sanal makinenize (yukarıda sağlanan) VMware araçlarını yüklememenizi öneririz. VMware araçlarının yüklenmesi desteklenmeyen bir yapılandırmaya neden olabilir.

## <a name="start-the-virtual-device-and-get-the-ip"></a>Sanal cihazı başlatma ve IP adresini alma

Sanal cihazınızı başlatmak ve bağlantı kurmak için aşağıdaki adımları izleyin.

#### <a name="to-start-the-virtual-device"></a>Sanal cihazı başlatmak için
1. Sanal cihazı başlatın. Sağ bölmedeki VM listeden cihazınızı seçin ve sağ tıklayarak bağlam menüsünü açın. **Power** (Güç) ve ardından **Power on** (Aç) seçimini yapın. Bu işlemin ardından makinenizin açılması gerekir. Durumu web istemcisinin en altındaki bölmede görebilirsiniz.

    ![](./media/data-box-gateway-deploy-provision-vmware/image19.png)

2. VM'nizi tekrar seçin. Sağ tıklayıp **Console** (Konsol) ve **Open in a new window** (Yeni pencerede aç) seçimlerini yapın.

    ![](./media/data-box-gateway-deploy-provision-vmware/image20.png)

3. Sanal makine konsolu yeni bir pencerede açılır. 

    ![](./media/data-box-gateway-deploy-provision-vmware/image21.png)

4. Cihaz çalışmaya başladıktan sonra imleci konsol penceresinin ortasının üst kısmındaki sekmeye götürün ve tıklayın. **Guest OS > Send keys > Ctrl+Alt+Delete** (Konuk işletim sistemi > Tuş gönder > Ctrl+Alt+Delete) girişini seçin. Bu işlem VM'nin kilidini açar.

   ![](./media/data-box-gateway-deploy-provision-vmware/image22.png)

5. Makinede oturum açmak için parolayı girin. Varsayılan parola Password1 olarak belirlenmiştir.

   ![](./media/data-box-gateway-deploy-provision-vmware/image23.png)

6. Adım 5-7 yalnızca DHCP bulunmayan bir ortamdaki önyükleme süreci için geçerlidir. DHCP ortamındaysanız bu adımları atlayıp 8. adımla devam edebilirsiniz. Cihazınızı DHCP olmayan bir ortamda çalıştırdıysanız bunu belirten bir ileti açılacaktır: **Ağı yapılandırmak için Set-HcsIPAddress cmdlet'ini kullanın**. 
   
7. Ağı yapılandırmak için komut isteminde `Get-HcsIpAddress` komutunu kullanarak sanal cihazınızda etkinleştirilmiş olan ağ arabirimlerini listeleyin. Cihazınızda tek bir ağ arabirimi varsa `DATA1` varsayılan adı atanır.

8. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıda bir örnek gösterilmiştir:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

9. İlk kurulum işlemleri tamamlandıktan ve cihaz önyüklendikten sonra cihaz başlık metnini görürsünüz. Cihazı yönetmek için başlık metninde görüntülenen IP adresini ve URL'yi not edin. Bu IP adresini kullanarak sanal cihazınızın web arabirimine bağlanıp yerel kurulum ve etkinleştirme işlemlerini gerçekleştirebilirsiniz.

   ![](./media/data-box-gateway-deploy-provision-vmware/image24.png)

Cihazınız minimum yapılandırma gereksinimlerini karşılamıyorsa başlık metninde hata iletisi görüntülenir (aşağıda gösterilmiştir). Cihaz yapılandırmasını minimum gereksinimleri karşılayacak şekilde değiştirmeniz gerekir. Ardından cihazı yeniden başlatıp bağlantı kurabilirsiniz. Minimum yapılandırma gereksinimleri için bkz. [1. Adım: Ana bilgisayarın minimum cihaz gereksinimlerini karşıladığından emin olma](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

<!---If you face any other error during the initial configuration using the local web UI, refer to the following workflows:

* Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).-->

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Gateway konularını öğrendiniz:

> [!div class="checklist"]
> * Ana bilgisayarın minimum cihaz gereksinimlerini karşıladığından emin olma
> * Hiper yöneticide bir sanal cihaz sağlama
> * Sanal cihazı başlatma ve IP adresini alma

Sanal cihazınıza bağlanma, kurulumunu yapma ve etkinleştirme adımları için bir sonraki öğreticiye geçin.

* [Data Box Gateway'de paylaşımları ayarlama ve bağlantı kurma](data-box-gateway-deploy-connect-setup-activate.md)

