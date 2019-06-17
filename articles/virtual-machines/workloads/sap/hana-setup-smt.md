---
title: SAP hana (büyük örnekler) azure'da SMT sunucusu kurmak nasıl | Microsoft Docs
description: SMT sunucusu (büyük örnekler) Azure üzerinde SAP HANA için ayarlama yapma.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 690f41e941f2d1db8fc92d225a54d07570299222
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60478115"
---
# <a name="set-up-smt-server-for-suse-linux"></a>SUSE Linux için SMT sunucusu ayarlama
Büyük örnekler, SAP HANA doğrudan internet bağlantısı yok. Böyle bir birim işletim sistemi sağlayıcısı ile kaydolmak için indirme ve güncelleştirmeleri uygulamak için basit bir işlem değil. Bir SUSE Linux için bir Azure sanal makine'de SMT sunucusunu ayarlamak için çözümüdür. HANA büyük örneği için bağlı olan bir Azure sanal ağında sanal makine konağının. Böyle bir SMT sunucusu ile HANA büyük örneği birim kaydedin ve güncelleştirmeleri karşıdan. 

SUSE üzerinde daha fazla bilgi için bkz, [SLES 12 SP2 için abonelik yönetimini](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

HANA büyük örnekler için görev karşılayan bir SMT sunucusu yüklemek için Önkoşullar şunlardır:

- HANA büyük örneği ExpressRoute işlem hattına bağlı bir Azure sanal ağı.
- Bir kuruluşla ilişkili olan bir SUSE hesabı. Kuruluş geçerli SUSE aboneliğinizin olması.

## <a name="install-smt-server-on-an-azure-virtual-machine"></a>Azure sanal makinesinde SMT sunucusu yükleme

İlk olarak oturum açın [SUSE Müşteri merkezi](https://scc.suse.com/).

Git **kuruluş** > **kuruluş kimlik**. Bu bölümde, SMT sunucusu kurmak gerekli kimlik bilgilerini bulmanız gerekir.

Ardından, SUSE Linux VM, Azure sanal ağında yükleyin. Sanal makineyi dağıtmak için Azure (BYOS SUSE görüntüyü seçin) SLES 12 SP2 galeri görüntüsü alın. Dağıtım işlemi, bir DNS adı yok tanımlayın ve statik IP adresini kullanmayın.

![Sanal makine dağıtımı SMT sunucusu için ekran görüntüsü](./media/hana-installation/image3_vm_deployment.png)

Dağıtılan sanal makine daha küçüktür ve iç IP adresi 10.34.1.4 Azure sanal ağında alındı. Sanal makine adı *smtserver*. Yükleme sonrasında, HANA büyük örneği birim veya birim bağlantısını denetlenir. Ad çözümlemesinin nasıl düzenlendiği bağlı olarak, vb./ana bilgisayarları Azure sanal makinesinin HANA büyük örneği birimlerinin çözüm yapılandırmanız gerekebilir. 

Bir disk sanal makineye ekleyin. Güncelleştirmeleri tutmak için bu diski kullanan ve önyükleme diski çok küçük olabilir. Burada, disk /srv/www/htdocs için aşağıdaki ekran görüntüsünde gösterildiği gibi bağlı. 100 GB boyutlu disk yeterli olacaktır.

![Sanal makine dağıtımı SMT sunucusu için ekran görüntüsü](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

HANA büyük örneği birim veya birim oturum/etc/hosts korumak ve SMT sunucunun ağ üzerinde çalıştırmak için gereken Azure sanal makinesini ulaşıp ulaşamadığını denetleyin.

Bu denetimden sonra SMT sunucunun çalışması gereken Azure sanal makine için oturum açın. Sanal makineye oturum açmak için putty kullanıyorsanız, bu bir dizi komut, bash penceresinde çalıştırın:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Ayarları etkinleştirmek için bash yeniden başlatın. Daha sonra YAST başlatın.

Sanal makinenize (smtserver) SUSE siteye bağlanın.

```
smtserver:~ # SUSEConnect -r <registration code> -e s<email address> --url https://scc.suse.com
Registered SLES_SAP 12.2 x86_64
To server: https://scc.suse.com
Using E-Mail: email address
Successfully registered system.
```

Sanal makine SUSE siteye bağlandıktan sonra smt paketleri yükleyin. Smt paketleri yüklemek için putty aşağıdaki komutu kullanın.

```
smtserver:~ # zypper in smt
Refreshing service 'SUSE_Linux_Enterprise_Server_for_SAP_Applications_12_SP2_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...
```


Smt paketleri yüklemek için YAST Aracı'nı da kullanabilirsiniz. YAST içinde Git **yazılım bakımı**, smt arayın. Seçin **smt**, hangi anahtarları otomatik olarak yast2 smt için.

![SMT YAST içinde ekran görüntüsü](./media/hana-installation/image5_smt_in_yast.PNG)


Smtserver yüklemesinde seçimini kabul edin. Yükleme tamamlandıktan sonra SMT sunucu yapılandırmasına geçin. Daha önce aldığınız SUSE müşteri Merkezi'nden Kurumsal kimlik bilgilerinizi girin. Ayrıca, Azure sanal makine ana bilgisayar adı SMT sunucu URL'sini girin. Bu örnekte, bu uygulamanın https:\//smtserver.

![Ekran görüntüsü, SMT sunucu yapılandırması](./media/hana-installation/image6_configuration_of_smtserver1.png)

Şimdi SUSE müşteri Merkezi'ne bağlantı çalışıp çalışmadığını test edin. Bu sunum durumda, aşağıdaki ekran görüntüsünde gördüğünüz gibi çalıştı.

![SUSE müşteri Merkezi'ne bağlantı test ediliyor ekran görüntüsü](./media/hana-installation/image7_test_connect.png)

SMT Kurulum başlatıldıktan sonra bir veritabanı parolasını belirtin. Yeni bir yüklemesi olduğundan, aşağıdaki ekran görüntüsünde gösterildiği gibi bu parolayı tanımlamanız gerekir.

![Veritabanı parolasını tanımlama ekran görüntüsü](./media/hana-installation/image8_define_db_passwd.PNG)

Sonraki adım, bir sertifika oluşturmaktır.

![SMT sunucusu için bir sertifika oluşturma işleminin ekran görüntüsü](./media/hana-installation/image9_certificate_creation.PNG)

Yapılandırmanın sonunda, eşitleme denetimini çalıştırmak için birkaç dakika sürebilir. Yükleme ve yapılandırma SMT sunucusunun sonra bağlama noktası /srv/www/htdocs altında dizin deposu bulmalısınız /. Depo bazı alt dizinler de vardır. 

SMT sunucunun ve ilgili hizmetleri şu komutlarla yeniden başlatın.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-packages-onto-smt-server"></a>SMT sunucuya paketlerini indirin

Tüm hizmetleri yeniden başlatıldıktan sonra uygun paketleri SMT yönetiminde YAST kullanarak seçin. Paket seçimi, HANA büyük örneği sunucu işletim sistemi görüntüsü üzerinde bağlıdır. Paket seçimi SLES sürüm veya sürüm SMT server çalıştıran sanal makinenin bağımlı değildir. Aşağıdaki ekran görüntüsünde seçimi ekran örneği gösterilmektedir.

![Paket seçme işleminin ekran görüntüsü](./media/hana-installation/image10_select_packages.PNG)

Ardından, paket seçmek için ayarladığınız SMT sunucu ilk kopyasını başlatın. Bu kopya komut smt yansıtma kullanarak Kabuğu'nda tetiklenir.

![SMT sunucuya paketleri indirme işleminin ekran görüntüsü](./media/hana-installation/image11_download_packages.PNG)

Bağlama noktası /srv/www/htdocs altında oluşturulan dizinleri halinde paketler kopyalanmasını. Bu işlem bir saat veya seçtiğiniz kaç paketin bağlı olarak daha fazla sürebilir. Bu işlemi tamamlanır, SMT istemci kurulum taşıyabilirsiniz. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>HANA büyük örneği birimleri SMT istemcide ayarlama

Bu durumda istemci veya istemciler HANA büyük örneği birimler verilir. SMT Sunucusu Kurulumu betik clientSetup4SMT.sh Azure sanal makine kopyalar. Üzerinde HANA büyük örneği birimi komut dosyası kopyalama SMT sunucunuza bağlanmak istediğiniz. Betik -h seçeneğiyle başlatın ve bir parametre olarak SMT sunucunuzun adını verin. Bu örnekte, addır *smtserver*.

![SMT istemci yapılandırma ekran görüntüsü](./media/hana-installation/image12_configure_client.PNG)

Aşağıdaki ekran görüntüsünde gösterildiği gibi sunucu tarafından istemci sertifikası yükünü başarılı, ancak kayıt başarısız olursa mümkündür.

![İstemci kayıt hatası görüntüsü](./media/hana-installation/image13_registration_failed.PNG)

Kayıt başarısız olursa bkz [SUSE belge desteği](https://www.suse.com/de-de/support/kb/doc/?id=7006024), ve burada açıklanan adımları çalıştırın.

> [!IMPORTANT] 
> Sunucu adı için sanal makine adını belirtin (Bu durumda, *smtserver*), tam etki alanı adı. 

Bu adımları çalıştırdıktan sonra HANA büyük örneği biriminde aşağıdaki komutu çalıştırın:

```
SUSEConnect –cleanup
```

> [!Note] 
> Bu adımdan sonra birkaç dakika bekleyin. ClientSetup4SMT.sh hemen çalıştırırsanız, bir hata alabilirsiniz.

Düzeltme SUSE makalenin adımlara göre gereken bir sorunla karşılaşırsanız, HANA büyük örneği birimi clientSetup4SMT.sh yeniden başlatın. Şimdi, başarıyla tamamlanmalıdır.

![İstemci kayıt başarı ekran görüntüsü](./media/hana-installation/image14_finish_client_config.PNG)

Azure sanal makinesinde yüklü SMT sunucusuna bağlanmak için HANA büyük örneği biriminin SMT istemci yapılandırdığınız. Artık yapabilir HANA büyük örnekler için işletim sistemi güncelleştirmeleri yüklemek için 'kurmak zypper' veya 'ı zypper içinde' olması veya ek paketler yüklemek. Yalnızca SMT sunucuda önceden indirdiğiniz güncelleştirmeleri alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [HANA yüklemesi HLI](hana-example-installation.md).











