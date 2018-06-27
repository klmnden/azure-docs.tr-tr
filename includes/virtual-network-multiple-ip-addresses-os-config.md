---
title: include dosyası
description: include dosyası
services: virtual-network
author: jimdial
ms.service: virtual-network
ms.topic: include
ms.date: 04/09/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: ec1727926f6dbfeead9932004715a8bb1dfbb0cd
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36964544"
---
## <a name="os-config"></a>Bir VM işletim sistemine IP adresleri ekleme

Bağlanma ve birden çok özel IP adresleri ile oluşturduğunuz bir VM için oturum açın. Sanal makineye eklediğiniz tüm özel IP adreslerini (birincil adres dahil) el ile eklemeniz gerekir. Adımları aşağıdaki VM işletim sisteminiz için.

### <a name="windows"></a>Windows

1. Bir komut isteminde *ipconfig/all* yazın.  Yalnızca *Birincil* özel IP adresini (DHCP üzerinden) görürsünüz.
2. Komut istemine *ncpa.cpl* yazarak **Ağ bağlantıları** penceresini açın.
3. Uygun bağdaştırıcının özelliklerini açın: **Yerel Ağ Bağlantısı**.
4. İnternet Protokolü sürüm 4 (IPv4) öğesine çift tıklayın.
5. **Aşağıdaki IP adresini kullan**’ı seçip aşağıdaki değerleri girin:

    * **IP adresi**: *Birincil* özel IP adresini girin
    * **Alt ağ maskesi**: Alt ağınıza göre ayarlanır. Örneğin, alt ağ bir /24 alt ağı ise alt ağ maskesi 255.255.255.0 şeklindedir.
    * **Varsayılan ağ geçidi**: Alt ağdaki ilk IP adresi. Alt ağınız 10.0.0.0/24 ise ağ geçidi IP adresi 10.0.0.1 şeklindedir.
    * Seçin **aşağıdaki DNS sunucu adreslerini kullan** ve aşağıdaki değerleri girin:
        * **Tercih edilen DNS sunucusu**: Kendi DNS sunucunuzu kullanmıyorsanız 168.63.129.16 girin.  Kendi DNS sunucunuzu kullanıyorsanız kendi sunucunuzun IP adresini girin.
    * Seçin **Gelişmiş** düğmesini tıklatın ve ek IP adreslerini ekleyin. Her Azure ağ arabirimi bir önceki adımda eklenen ikincil özel IP adresleri, Azure ağ arabirimine atanmış birincil IP adresi atanır Windows ağ arabirimi ekleyin.

        Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir. İşletim sistemi içinde IP adresini el ile ayarladığınızda, Azure için atanan özel IP adresi aynı adresi olduğundan emin olun [ağ arabirimi](../articles/virtual-network/virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantısını kaybedebilir. Daha fazla bilgi edinmek [özel IP adresi](../articles/virtual-network/virtual-network-network-interface-addresses.md#private) ayarlar. Hiçbir işletim sistemi içinde Azure genel IP adresi atamanız gerekir.

    * **Tamam**’a tıklayarak TCP/IP ayarlarını kapatın ve ardından tekrar **Tamam**’a tıklayarak bağdaştırıcı ayarlarını kapatın. RDP bağlantınız yeniden kurulur.

6. Bir komut isteminde *ipconfig/all* yazın. Eklediğiniz tüm IP adresleri gösterilir ve DHCP kapatılır.
7. Windows özel IP adresini birincil IP yapılandırmasının Azure'da Windows için birincil IP adresi olarak kullanacak şekilde yapılandırın. Bkz: [birden çok IP adresi varsa Azure Windows VM Hayır Internet erişimden](https://support.microsoft.com/help/4040882/no-internet-access-from-azure-windows-vm-that-has-multiple-ip-addresse) Ayrıntılar için. 

### <a name="validation-windows"></a>Doğrulama (Windows)

İkincil IP yapılandırmanızdan, ona bağlı genel IP ile internet’e bağlanabildiğinizden emin olmak için, yukarıdaki adımları doğru şekilde kullanarak ekledikten sonra aşağıdaki komutu kullanın:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>Yapılandırma ile ilgili genel bir IP adresi varsa, ikincil IP yapılandırmaları için yalnızca Internet'e ping işlemi yapabilirsiniz. Birincil IP yapılandırmaları için bir ortak IP adresi, Internet'e ping işlemi yapmak için gerekli değildir.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. Bir terminal penceresi açın.
2. Kök kullanıcı olduğunuzdan emin olun. Kök kullanıcı değilseniz aşağıdaki komutu girin:

    ```bash
    sudo -i
    ```

3. Ağ arabiriminin yapılandırma dosyasını güncelleştirin (‘eth0’ olduğu varsayılır).

    * Dhcp için var olan satır öğesini tutun. Birincil IP adresi daha önce olduğu gibi yapılandırılmış olarak kalır.
    * Aşağıdaki komutları kullanarak başka bir statik IP adresi için yapılandırma ekleyin:

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    Bir .cfg dosyası görmeniz gerekir.
4. Dosyayı açın. Dosyanın sonunda aşağıdaki satırları görmeniz gerekir:

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. Bu dosyada mevcut satırların sonuna aşağıdaki satırları ekleyin:

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. Aşağıdaki komutu kullanarak dosyayı kaydedin:

    ```bash
    :wq
    ```

7. Aşağıdaki komutu kullanarak ağ arabirimini sıfırlayın:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > Uzak bağlantı kullanıyorsanız aynı satırda hem ifdown hem de ifup komutunu çalıştırın.
    >

8. IP adresinin ağ arabirimine eklendiğini aşağıdaki komutla doğrulayın:

    ```bash
    ip addr list eth0
    ```

    Listenin bir parçası olarak eklediğiniz IP adresini görmeniz gerekir.

### <a name="linux-red-hat-centos-and-others"></a>Linux (Red Hat, CentOS ve diğerleri)

1. Bir terminal penceresi açın.
2. Kök kullanıcı olduğunuzdan emin olun. Kök kullanıcı değilseniz aşağıdaki komutu girin:

    ```bash
    sudo -i
    ```

3. Parolanızı girin ve istenen yönergeleri izleyin. Kök kullanıcı olduktan sonra aşağıdaki komutla ağ betikleri klasörüne gidin:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Aşağıdaki komutu kullanarak ilgili ifcfg dosyalarını listeleyin:

    ```bash
    ls ifcfg-*
    ```

    Gördüğünüz dosyalardan biri *ifcfg-eth0* olmalıdır.

5. Bir IP adresi eklemek için aşağıda gösterildiği gibi bir yapılandırma dosyası oluşturun. Her IP yapılandırması için bir dosya oluşturulmalıdır.

    ```bash
    touch ifcfg-eth0:0
    ```

6. *ifcfg-eth0:0* dosyasını aşağıdaki komutla açın:

    ```bash
    vi ifcfg-eth0:0
    ```

7. Dosyaya (bu örnekte *eth0:0*) aşağıdaki komutla içerik ekleyin. Bilgileri IP adresine göre güncelleştirdiğinizden emin olun.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. Aşağıdaki komutla dosyayı kaydedin:

    ```bash
    :wq
    ```

9. Ağ hizmetlerini yeniden başlatın ve aşağıdaki komutları kullanarak değişikliklerin başarılı olduğundan emin olun:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    Döndürülen listede eklemiş olduğunuz *eth0:0* IP adresini görmeniz gerekir.

### <a name="validation-linux"></a>Doğrulama (Linux)

İkincil IP yapılandırmanızla ilişkili genel IP aracılığıyla İnternet’e bağlanabildiğinizden emin olmak için aşağıdaki komutu kullanın:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>Yapılandırma ile ilgili genel bir IP adresi varsa, ikincil IP yapılandırmaları için yalnızca Internet'e ping işlemi yapabilirsiniz. Birincil IP yapılandırmaları için bir ortak IP adresi, Internet'e ping işlemi yapmak için gerekli değildir.

Linux sanal makineleri için, ikincil bir NIC’den giden bağlantıyı doğrulamaya çalışırken uygun yolları eklemeniz gerekebilir. Bunu yapmanın çok sayıda yolu vardır. Lütfen Linux dağıtımınız için uygun belgelere bakın. Bunu gerçekleştirmeye yönelik bir yöntem aşağıdaki gibidir:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Şunu değiştirdiğinizden emin olun:
    - **10.0.0.5** adresini ilişkili bir genel IP adresi olan özel IP adresiyle
    - **10.0.0.1** adresini varsayılan ağ geçidiniz ile
    - **eth2** değerini ikincil NIC’nin adı ile
