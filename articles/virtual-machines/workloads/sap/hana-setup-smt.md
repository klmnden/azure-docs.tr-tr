---
title: Kurulum SMT sunucusu (büyük örnekler) Azure üzerinde SAP HANA için nasıl | Microsoft Docs
description: SMT sunucusu (büyük örnek) Azure üzerinde SAP HANA için ayarlama.
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
ms.openlocfilehash: f387c1afe88f2bba476309b2e2e01942d2b7ae5b
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982634"
---
# <a name="setting-up-smt-server-for-suse-linux"></a>SUSE Linux için SMT sunucusu ayarlama
SAP HANA büyük örnekleri, doğrudan Internet bağlantısı yok. Bu nedenle, böyle bir birim OS sağlayıcıya Kaydol indirin ve düzeltme ekleri uygulama için basit bir işlem değil. SUSE Linux Azure VM'deki bir SMT sunucusunu ayarlamak için tek bir çözüm olabilecek durumunda. Azure VM, HANA büyük örneği için bağlı olan bir Azure VNet, barındırılması gerekiyor ancak. Böyle bir SMT sunucusu ile HANA büyük örneği birim kaydedin ve düzeltme eklerini indirmek. 

SUSE üzerinde daha büyük bir kılavuz sağlar, [SLES 12 SP2 için abonelik yönetimini](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

HANA büyük örneği için görev karşılayan bir SMT sunucusunun yüklenmesi için önkoşul ihtiyacınız:

- Bir Azure HANA büyük örneği ER bağlantı hattına bağlı sanal ağ.
- Bir kuruluşla ilişkili olan bir SUSE hesabı. Kuruluş bazı geçerli SUSE aboneliğinizin olması gerekir ancak.

## <a name="installation-of-smt-server-on-azure-vm"></a>Azure sanal makinesinde SMT server yüklemesi

Bu adımda, Azure VM'deki SMT sunucusu yükleyin. Oturum açmak için ilk ölçüdür [SUSE Müşteri merkezi](https://scc.suse.com/).

Oturum açtığınız gibi kuruluş Git kuruluş kimlik-->. Bu bölümde SMT sunucusu kurmak gerekli kimlik bilgilerini bulmanız gerekir.

Üçüncü adım SUSE Linux VM Azure Vnet'te yüklemektir. VM dağıtmak için Azure (BYOS SUSE görüntüyü seçin) SLES 12 SP2 galeri görüntüsü alın. Dağıtım işlemi, bir DNS adı tanımlayın yok ve bu ekran görüntüsünde görüldüğü gibi statik IP adresleri kullanmayın

![SMT sunucusu için VM dağıtımı](./media/hana-installation/image3_vm_deployment.png)

Dağıtılan VM, daha küçük bir VM olan ve iç IP adresi 10.34.1.4 Azure Vnet'te aldı. VM'nin adı smtserver oluştu. Yükleme sonrasında, HANA büyük örneği birim bağlantısını denetlendi. Ad çözümlemesinin nasıl düzenlediğiniz bağımlı, HANA büyük örneği birimlerinin çözümlemesi vb./konakları Azure VM'nin yapılandırmanız gerekebilir. Düzeltme ekleri tutmak için kullanılacak giden VM'ye ek bir disk ekleyin. Önyükleme diski çok küçük olabilir. Gösterilen durumda da, aşağıdaki ekran görüntüsünde gösterildiği gibi /srv/www/htdocs disk takılı. 100 GB boyutlu disk yeterli olacaktır.

![SMT sunucusu için VM dağıtımı](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

HANA büyük örneği birimi için oturum açın, / etc/hosts korumak ve SMT sunucunun ağ üzerinde çalıştırmak için gereken Azure VM ulaşıp ulaşamadığını denetleyin.

Bu denetimi başarıyla tamamlandıktan sonra SMT sunucunun çalışması gereken Azure VM ile oturum açmanız gerekir. VM'de oturum açmak için putty kullanıyorsanız, bu komutları dizisini bash pencerenizde yürütülecek gerekir:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Bu komutlar yürütüldükten sonra ayarları etkinleştirmek için bash yeniden başlatın. Daha sonra YAST başlatın.

Sanal makinenize (smtserver) SUSE siteye bağlanın.

```
smtserver:~ # SUSEConnect -r <registration code> -e s<email address> --url https://scc.suse.com
Registered SLES_SAP 12.2 x86_64
To server: https://scc.suse.com
Using E-Mail: email address
Successfully registered system.
```

VM SUSE siteye bağlandıktan sonra smt paketleri yükleyin. Smt paketleri yüklemek için putty aşağıdaki komutu kullanın.

```
smtserver:~ # zypper in smt
Refreshing service 'SUSE_Linux_Enterprise_Server_for_SAP_Applications_12_SP2_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...
```


Smt paketleri yüklemek için YAST aracını da kullanabilirsiniz. Yazılım bakımı ve smt araması YAST içinde gidin. Aşağıda gösterildiği gibi yast2 smt için otomatik olarak geçer smt, seçin

![SMT yast içinde](./media/hana-installation/image5_smt_in_yast.PNG)


Smtserver yüklemesinde seçimini kabul edin. Yüklendikten sonra SMT Sunucu Yapılandırması'na gidin ve daha önce aldığınız SUSE müşteri Merkezi'nden Kurumsal kimlik bilgilerinizi girin. Ayrıca, Azure VM konak adı SMT sunucu URL'sini girin. Bu örnekte olduğu https://smtserver sonraki grafikleri görüntülendiği gibi.

![SMT sunucusunun yapılandırması](./media/hana-installation/image6_configuration_of_smtserver1.png)

Sonraki adım olarak, SUSE müşteri Merkezi'ne bağlantı çalışıp çalışmadığını test etmelisiniz. Tanıtım durumda, aşağıdaki grafiklerde gördüğünüz gibi çalıştı.

![Test SUSE müşteri Merkezi'ne bağlanma](./media/hana-installation/image7_test_connect.png)

Bir kez SMT Kurulum başladığında, veritabanı parolasını sağlamanız gerekir. Yeni bir yüklemesi olduğundan, bu parolayı sonraki grafikleri gösterildiği tanımlamanız gerekir.

![Veritabanı parolasını tanımlayın](./media/hana-installation/image8_define_db_passwd.PNG)

Bir sertifika oluşturulur sonraki etkileşim sahip olduğunuz durumdur. Aşağıda gösterildiği iletişim kutusundan gidin ve adım devam etmelisiniz.

![SMT sunucusu için sertifika oluşturma](./media/hana-installation/image9_certificate_creation.PNG)

Yapılandırmanın sonunda 'Eşitleme denetimi Çalıştır' adımı harcanan bazı dakika olabilir. Yükleme ve yapılandırma SMT sunucusunun sonra bağlama noktası /srv/www/htdocs altında dizin deposu bulmalısınız/artı depo bazı dizinler. 

SMT sunucunun ve ilgili hizmetleri şu komutlarla yeniden başlatın.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-of-packages-onto-smt-server"></a>SMT sunucuya paketleri indirin

Tüm hizmetleri yeniden başlatıldıktan sonra SMT Yast kullanarak Yönetimi'nde uygun paketleri seçin. Paket seçimi, HANA büyük örneği sunucu işletim sistemi görüntüsü ve SLES sürüm veya sürüm SMT server çalıştıran sanal makinenin üzerinde bağlıdır. Seçimi ekran örneği aşağıda gösterilmiştir.

![Paketleri seçin](./media/hana-installation/image10_select_packages.PNG)

Paket seçimi tamamladıktan sonra ayarladığınız SMT sunucuya paket seçmek ilk kopyasını başlatmanız gerekir. Bu kopya komut smt-aşağıda gösterildiği gibi yansıtma kullanarak Kabuğu'nda tetiklendi


![SMT sunucuya paketleri indirin](./media/hana-installation/image11_download_packages.PNG)

Yukarıda gördüğünüz gibi bağlama noktası /srv/www/htdocs altında oluşturulan dizinleri halinde paketler kopyalanmasını. Bu işlem biraz sürebilir. Seçtiğiniz kaç paketin bağımlı, bunu bir saat veya daha fazla sürebilir.
Bu işlem bittikçe SMT istemci kurulum taşımanız gerekir. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>HANA büyük örneği birimleri SMT istemcide ayarlama

İstemci bu durumda HANA büyük örneği birimleridir. SMT Sunucusu Kurulumu betik clientSetup4SMT.sh Azure VM'de oturum kopyalanır. Üzerinde HANA büyük örneği birimi komut dosyası kopyalama SMT sunucunuza bağlanmak istediğiniz. -H seçeneğiyle komut dosyasını başlatmak ve parametre olarak SMT sunucunuzun adını verin. Bu örnek smtserver.

![SMT istemciyi Yapılandırma](./media/hana-installation/image12_configure_client.PNG)

Bir senaryo burada sunucu tarafından istemci sertifikası yükünü başarılı oldu, ancak aşağıda gösterildiği gibi kayıt başarısız olabilir.

![İstemci kaydı başarısız](./media/hana-installation/image13_registration_failed.PNG)

Kayıt başarısız olursa bu okuma [SUSE belge desteği](https://www.suse.com/de-de/support/kb/doc/?id=7006024) ve burada açıklanan adımları yürütün.

> [!IMPORTANT] 
> Sunucu adı olarak tam etki alanı adı olmadan bu büyük/küçük harf smtserver, VM'nin adı sağlamanız gerekir. Yalnızca VM adı çalışır. 

Adımları yürütülen sonra aşağıdaki komutu yürütün HANA büyük örneği biriminde gerekir

```
SUSEConnect –cleanup
```

> [!Note] 
> Bizim testleri her zaman bu adımdan sonra birkaç dakika beklemeniz vardı. Hemen yürütme clientSetup4SMT.sh SUSE makalesinde açıklanan düzeltici önlemleri sonra sertifika henüz geçerli olmaz iletileri ile sona erdi. Başarılı istemci yapılandırmasında, genellikle 5-10 dakika bekleyen ve clientSetup4SMT.sh yürütme sona erdi.

Sorunu düzeltmek için SUSE makalenin adımlara göre gerekli olan çalıştırdıysanız, HANA büyük örneği birimi clientSetup4SMT.sh yeniden yeniden başlatmanız gerekir. Şimdi, aşağıda gösterildiği gibi başarıyla tamamlanmalıdır.

![İstemci kaydı başarılı](./media/hana-installation/image14_finish_client_config.PNG)

Bu adımda, Azure sanal Makinesinde yüklü SMT sunucusuna karşı bağlanmak için HANA büyük örneği biriminin SMT istemci yapılandırılmış. Şimdi, HANA büyük örnekler için işletim sistemi düzeltme ekleri yüklemek veya ek paketler yüklemek için 'kurmak zypper' veya 'ı zypper içinde' alabilir. SMT sunucuda önceden indirdiğiniz eklerini yalnızca alabilir anlaşılır.

**Sonraki adımlar**
- Başvuru [HLI HANA yüklemesi](hana-example-installation.md).











