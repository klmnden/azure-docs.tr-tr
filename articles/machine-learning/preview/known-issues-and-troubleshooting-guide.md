---
title: "Bilinen sorunlar ve sorun giderme kılavuzu | Microsoft Docs"
description: "Bilinen sorunların listesi ve gidermenize yardımcı olması için bir kılavuz"
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: f39faea6b7e0886d63085b752f9532a7010ea941
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning çalışma ekranı - bilinen sorunlar ve sorun giderme kılavuzu 
Bu makalede, bulma ve hataları düzeltin ya da Azure Machine Learning çalışma ekranı uygulamasını kullanarak bir parçası olarak karşılaşılan hataları yardımcı olur. 

> [!IMPORTANT]
> Destek Ekibi ile iletişim kurarken yapı numarası olması önemlidir. Tıklayarak uygulamayı yapı numarası bulabilirsiniz **yardımcı** menüsü. Yapı numarası tıklatarak panonuza kopyalar. Destek forumları rapor çözmeye yardımcı olması için ya da e-postalara yapıştırın.

![sürüm numarasını kontrol edin](media/known-issues-and-troubleshooting-guide/buildno.png)

## <a name="machine-learning-msdn-forum"></a>Machine Learning MSDN Forumu
Sorular nakledebilirsiniz bir MSDN Forumu sunuyoruz. Ürün ekibi forum etkin olarak izler. URL Forumu [https://aka.ms/azureml-forum](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Tanılama bilgileri toplayın
Bazen yardımını isterken tanı bilgilerini sağlarsanız, yararlı olabilir. Günlük dosyalarının nerede Canlı aşağıda verilmiştir:

### <a name="installer"></a>Yükleyici
Yükleme sırasında sorunu yaşayıp çalıştırırsanız, yükleyici günlük dosyaları şunlardır:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
Bu dizinlerin içerikleri zip ve tanılama için bize gönderin.

### <a name="workbench-desktop-app"></a>Çalışma ekranı masaüstü uygulaması
Çalışma ekranı Masaüstü çökerse günlük dosyaları burada bulabilirsiniz:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
Bu dizinlerin içerikleri zip ve tanılama için bize gönderin.

### <a name="experiment-execution"></a>Deneme yürütme
Masaüstü uygulaması gönderim sırasında belirli bir komut başarısız olursa, CLI kullanarak yeniden göndermeyi deneyin `az ml experiment submit` komutu. Bu, tam hata iletisi JSON biçiminde vermeniz gerekir ve en önemlisi içeren bir **işlem kimliği** değeri. JSON dosyası dahil bize gönderin **işlem kimliği** ve tanılamanıza yardımcı olabiliriz. 

Belirli bir komut dosyası gönderisine başarılı ancak yürütme başarısız olursa, yazdırabilirsiniz **Çalıştır kimliği** belirli çalıştıran tanımlamak için. Aşağıdaki komutu kullanarak ilgili günlük dosyaları paketleyebilir:

```azurecli
# Create a ZIP file that contains all the diagnostics information
$ az ml experiment diagnostics -r <run_id> -t <target_name>
```

`az ml experiment diagnostics` Komutu oluşturan bir `diagnostics.zip` proje kök klasörü içinde dosya. Bu, günlük bilgilerini yürütüldüğü zaman durumu tüm proje klasöründe ZIP paketini içerir. Bize tanılama dosya göndermeden önce dahil etmek istemediğiniz herhangi bir önemli bilgi kaldırdığınızdan emin olun.

## <a name="send-us-a-frown-or-a-smile"></a>Bize bir kaş çatma (veya bir gülümseme gönderme) gönderin

Azure ML çalışma ekranı içinde çalışırken, ayrıca bize bir kaş çatma (veya bir gülümseme gönderme) uygulama kabuk sol alt köşesindeki gülen yüz simgesine tıklayarak gönderebilirsiniz. İsteğe bağlı olarak (biz geri size sınıflandırıp), e-posta adresinizi dahil etmeyi seçebilirsiniz ve/veya geçerli durumunun ekran görüntüsü. 

## <a name="known-service-limits"></a>Bilinen hizmet sınırları
- İzin verilen maks. proje klasör boyutu: 25 MB.
    >[!NOTE]
    >Bu sınır uygulanmaz `.git`, `docs` ve `outputs` klasörler. Bu klasör adları büyük/küçük harfe duyarlıdır. Büyük dosyalarla çalışıyorsanız, başvurmak [kalıcı değişiklikler ve çok büyük dosyalarla anlaşma](how-to-read-write-files.md).

- İzin verilen maks. deneme yürütme süresi: yedi gün
- İzlenen dosyasının en büyük boyutu `outputs` sonra bir çalışma klasörü: 512 MB
  - Bu komut çıkış klasöründeki 512 MB daha büyük bir dosya oluşturur, var. toplanmaz anlamına gelir. Büyük dosyalarla çalışıyorsanız, başvurmak [kalıcı değişiklikler ve çok büyük dosyalarla anlaşma](how-to-read-write-files.md).

- SSH anahtarları bir uzak makine ya da Spark kümesi SSH üzerinden bağlanırken desteklenmez. Yalnızca kullanıcı adı/parola modu şu anda desteklenir.

- Metin kümeleme dönüşümler Mac üzerinde desteklenmez.

- RevoScalePy kitaplığı yalnızca Windows ve Linux (Docker kapsayıcılardaki) desteklenir. MacOS üzerinde desteklenmiyor.

## <a name="file-name-too-long-on-windows"></a>Windows dosya adı çok uzun
Windows çalışma ekranı kullanıyorsa, bir biraz "Sistem belirtilen yol bulunamıyor" hatası yanıltıcı olarak yüzey varsayılan en fazla 260 karakter dosya adı uzunluğu sınırı içinde çalıştırabilirsiniz. Çok uzun dosya yolu adı izin vermek için bir kayıt defteri anahtarı ayarı değiştirebilirsiniz. Gözden geçirme [bu makalede](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath) nasıl ayarlanacağı hakkında daha fazla ayrıntı için _MAX_PATH_ kayıt defteri anahtarı.

## <a name="docker-error-read-connection-refused"></a>Docker hata "okuma: bağlantı reddedildi"
Bazen yerel bir Docker kapsayıcısı karşı çalıştırırken şu hatayı görebilirsiniz: 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```

Docker DNS sunucusundan değiştirerek düzeltebilirsiniz `automatic` bir sabit değere `8.8.8.8`.

## <a name="remove-vm-execution-error-no-tty-present"></a>VM yürütme hatası "tty mevcut" Kaldır
Bir uzak Linux makinesinde Docker kapsayıcısı karşı yürütürken, aşağıdaki hata iletisini karşılaşabilirsiniz:
```
sudo: no tty present and no askpass program specified.
``` 
Bir Ubuntu Linux VM kök parolasını değiştirmek için Azure portal'ı kullanırsanız, bu durum oluşabilir. 

Azure Machine Learning çalışma ekranı uzak ana bilgisayarda çalıştırmak için parola daha az sudoers erişim gerektirir. Bunu yapmanın en kolay yolu kullanmaktır _visudo_ (oluşturduğunuz dosyayı henüz yoksa) aşağıdaki dosyayı düzenlemek için:

```
$ sudo visudo -f /etc/sudoers
```

>[!IMPORTANT]
>Dosyayı düzenlemek önemlidir _visudo_ ve değil başka bir komutu. _visudo_ sözdizimi tüm sudo yapılandırma dosyalarını otomatik olarak denetler ve başarısızlık sözdizimsel olarak doğru sudoers dosya oluşturmak için sudo dışında kilitleyebilir.

Aşağıdaki satırı dosyanın sonuna ekle:

```
username ALL=(ALL) NOPASSWD:ALL
```

Burada _kullanıcıadı_ Azure Machine Learning çalışma ekranı adını uzak ana bilgisayara oturum açmak için kullanacağınız değil.

Satır #includedir sonra yerleştirilmelidir "/ etc/sudoers.d", aksi takdirde, başka bir kural tarafından geçersiz kılınmış olabilir.

Daha karmaşık bir sudo yapılandırmanız varsa, sudo Ubuntu kullanılabilir burada belgelerine isteyebilirsiniz: https://help.ubuntu.com/community/Sudoers

Yukarıdaki hata, bir Ubuntu tabanlı Linux VM Azure'da bir yürütme hedef olarak kullanmıyorsanız de oluşabilir. Yalnızca Linux VM Ubuntu tabanlı uzak yürütme için destekliyoruz. 

## <a name="vm-disk-is-full"></a>VM disk dolu
Azure'da yeni bir Linux VM oluşturduğunuzda varsayılan olarak, 30 GB disk işletim sistemi için alırsınız. Docker altyapısına varsayılan olarak aynı disk görüntüleri çekmek ve çalışan kapsayıcılar için kullanır. Bu işletim sistemi diski doldurabilir ve bu durumda "VM Disk olan tam" hatasını görürsünüz.

Bir hızlı düzeltme artık kullanmadığınız tüm Docker görüntüleri kaldırmaktır. Aşağıdaki Docker komutu tam olarak bunu yapar. (Elbette, VM içine SSH bash Kabuğu'ndan Docker komut yürütmek için gerekir.)

```
$ docker system prune -a
```

Ayrıca, bir veri diski ekleyin ve görüntüleri saklamak için veri diski kullanmak için Docker altyapısına yapılandırın. Burada [bir veri diski ekleme](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk). Daha sonra [Docker görüntüleri depoladığı değişiklik](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Veya, işletim sistemi diski genişletebilirsiniz ve Docker altyapısı yapılandırması touch gerekmez. Burada [nasıl işletim sistemi diski genişletebilirsiniz](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk).

## <a name="sharing-c-drive-on-windows"></a>Windows C sürücüsünde paylaşımı
Windows yerel bir Docker kapsayıcısı içinde çalıştırıldığında, ayarı `sharedVolumes` için `true` içinde `docker.compute` altında dosya `aml_config` yürütme performansını iyileştirebilir. Ancak, bu C sürücüsünde paylaşmak gerektirir _Windows aracı için Docker_. C sürücüsünün paylaşmak mümkün değilse, aşağıdaki ipuçlarını deneyin:

* Dosya Gezgini'ni kullanarak C sürücüsünde paylaşımı denetleyin
* Ağ bağdaştırıcısı ayarları'nı açın ve kaldırma / "Microsoft Ağları için dosya ve Yazıcı Paylaşımı" vEthernet için yeniden yükleme
* Docker Ayarları'nı açın ve docker ayarları içinde C sürücüsünden paylaşma
* Windows parolası yapılan değişiklikler, paylaşım etkiler. Dosya Gezgini'ni açın, C sürücüsünün yeniden paylaşabilirsiniz ve yeni bir parola girin.
* Ayrıca, C sürücüsünün Docker ile paylaşmak çalışırken güvenlik duvarı sorunla karşılaşabilirsiniz. Bu [yığın taşması post](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) yararlı olabilir.
* Etki alanı kimlik bilgilerini kullanarak C sürücüsü paylaşırken paylaşımı burada etki alanı denetleyicisini (örneğin, ev ağı, ortak wifi vb. için) ulaşılabilir değil ağlarda çalışmayı durdurabilir. Daha fazla bilgi için bkz: [bu post](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

Ayrıca maliyet, ayarlayarak küçük bir performans adresindeki paylaşma sorun kaçının `sharedVolumne` için `false` içinde `docker.compute` dosya.

## <a name="some-useful-docker-commands"></a>Bazı yararlı Docker komutları

Bazı yararlı Docker komutlar şunlardır:

```sh
# display all running containers
$ docker ps

# dislplay all containers (running or stopped)
$ docke ps -a

# display all images
$ docker images

# show Docker logs of a container
$ docker logs <container_id>

# create a new container and launch into a bash shell
$ docker run <image_id> /bin/bash

# launch into a bash shell on a running container
$ docker exec -it <container_id> /bin/bash

# stop an running container
$ docker stop <container_id>

# delete a container
$ docker rm <container_id>

# delete an image
$ docker rmi <image_id>

# delete all unussed Docker images 
$ docker system prune -a

```
