---
title: Bilinen sorunlar ve sorun giderme kılavuzu | Microsoft Docs
description: Bilinen sorunların listesi ve gidermeye yardımcı olmak için bir kılavuz
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: 8177808fd4d666ea04b1bc097f261c7643931704
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885059"
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Learning Workbench'i - bilinen sorunlar ve sorun giderme kılavuzu azure makine 
Bu makalede, bulma ve hataları düzeltin veya Azure Machine Learning Workbench uygulamasını kullanarak bir parçası olarak karşılaşılan hatalar yardımcı olur. 

## <a name="find-the-workbench-build-number"></a>Workbench yapı numarası bulun
Destek Ekibi ile iletişim kurarken, yapı numarası Workbench uygulamasının eklemek önemlidir. Windows üzerinde yapı numarasını tıklayarak bulabilirsiniz **yardımcı** menüsünü seçip **Azure ML Workbench hakkında**. MacOS üzerinde tıklayabilirsiniz **Azure ML Workbench** menüsünü seçip **Azure ML Workbench hakkında**.

## <a name="machine-learning-msdn-forum"></a>Machine Learning MSDN Forumu
Bir MSDN forumuna soru gönderebilir sahibiz. Ürün ekibine forumun etkin bir şekilde izler. URL forum [ https://aka.ms/aml-forum-service ](https://aka.ms/aml-forum-service). 

## <a name="gather-diagnostics-information"></a>Tanılama bilgilerini toplama
Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Günlük dosyaları burada Canlı aşağıda verilmiştir:

### <a name="installer-log"></a>Yükleyici günlük
Yükleme sırasında sorunla çalıştırırsanız, yükleyici günlük dosyaları şunlardır:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
Bu dizinin içeriğini zip ve tanılama için bize gönderin.

### <a name="workbench-desktop-app-log"></a>Workbench masaüstü uygulaması günlüğü
Oturum açma sorun yaşarsanız veya Workbench Masaüstü çökerse, günlük dosyaları burada bulabilirsiniz:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
Bu dizinin içeriğini zip ve tanılama için bize gönderin.

### <a name="experiment-execution-log"></a>Deneme yürütme günlüğü
Belirli bir betiğin Masaüstü uygulamasından gönderme sırasında başarısız olursa yeniden CLI kullanarak gönderin `az ml experiment submit` komutu. Bu, tüm hata iletilerini JSON biçiminde vermeniz gerekir ve en önemlisi, içerdiği bir **işlem kimliği** değeri. JSON dosyası dahil olmak üzere bize gönderin **işlem kimliği** ve tanılamanıza yardımcı olabiliriz. 

Belirli bir betiğin gönderisine başarılı olur, ancak yürütme başarısız olursa, yazdırabilirsiniz **çalıştırması kimliği** o belirli çalışma belirlemek için. Aşağıdaki komutu kullanarak ilgili günlük dosyaları paketleyebilir:

```azurecli
# Create a ZIP file that contains all the diagnostics information
$ az ml experiment diagnostics -r <run_id> -t <target_name>
```

`az ml experiment diagnostics` Komut oluşturur. bir `diagnostics.zip` proje kök klasöründeki dosya. ZIP paketini tüm proje klasörünün durumunda bu, günlük bilgilerini yürütülmesi zaman içerir. Bize tanılama dosya göndermeden önce dahil etmek istemediğiniz herhangi bir önemli bilgi kaldırdığınızdan emin olun.

## <a name="send-us-a-frown-or-a-smile"></a>Kaş çatma (veya bir gülümseme Gönder) bize gönderin

Azure ML Workbench'te çalışırken de bize kaş çatma (veya bir gülümseme Gönder) uygulama kabuk sol alt köşesindeki gülen yüz simgesine tıklayarak gönderebilirsiniz. İsteğe bağlı (biz size dönebilirsiniz şekilde), e-posta adresi eklemek seçebilirsiniz ve/veya ekran görüntüsü geçerli durumu. 

## <a name="known-service-limits"></a>Bilinen hizmet sınırları
- Proje klasör boyutu izin verilen Maks: 25 MB.
    >[!NOTE]
    >Bu limit uygulanmaz `.git`, `docs` ve `outputs` klasörleri. Bu klasör adları büyük/küçük harfe duyarlıdır. Büyük dosyaları ile çalışıyorsanız, başvurmak [değişiklikleri kalıcı hale getirmeniz ve büyük dosyaları kadarıyla](how-to-read-write-files.md).

- En fazla izin verilen deneme yürütme süresi: yedi gün

- İzlenen dosyasının en büyük boyutu `outputs` sonra bir çalışma klasörü: 512 MB
  - Bu komut çıktı klasöründe 512 MB değerinden daha büyük bir dosya oluşturur, burada toplanmaz anlamına gelir. Büyük dosyaları ile çalışıyorsanız, başvurmak [değişiklikleri kalıcı hale getirmeniz ve büyük dosyaları kadarıyla](how-to-read-write-files.md).

- SSH anahtarları, bir uzak makine veya Spark kümesi, SSH üzerinden bağlanırken desteklenmez. Yalnızca kullanıcı adı/parola modunda desteklenmektedir.

- Hedef işlem olarak HDInsight kümesi kullanırken, Azure kullanmalısınız blob birincil depolama olarak. Azure Data Lake depolamanın kullanılması desteklenmiyor.

- Mac üzerinde metin kümeleme dönüştürmeleri desteklenmez.

- RevoScalePy kitaplığı yalnızca, Windows ve Linux'ta (Docker kapsayıcıları içinde) desteklenir. MacOS üzerinde desteklenmiyor.

- Jupyter not defterleri, bunları Workbench uygulamasından açılırken 5 MB maksimum boyut sınırı sahip. Dosya boyutunu küçültmek için temiz bir hücre çıkarır ve büyük Not Defterleri 'az ml Not start' komutunu kullanarak CLI'dan açabilir.

## <a name="cant-update-workbench"></a>Workbench güncelleştirilemiyor
Workbench uygulama giriş sayfası, yeni bir güncelleştirme kullanılabilir olduğunda, yeni güncelleştirme hakkında bildiren bir ileti görüntüler. Zil simgesine uygulamasının sol alt köşedeki görünen bir güncelleştirme rozeti görmeniz gerekir. Rozeti oylayıp tıklayın ve güncelleştirmeyi yüklemek için yükleyici Sihirbazı izleyin. 

![Güncelleştirme resmi](../service/media/known-issues-and-troubleshooting-guide/update.png)

Bildirim görmüyorsanız, uygulamayı yeniden başlatmayı deneyin. Yeniden başlatma işleminden sonra güncelleştirme bildirimi hala görmüyorsanız, birkaç nedeni olabilir.

### <a name="you-are-launching-workbench-from-a-pinned-shortcut-on-the-task-bar"></a>Görev çubuğunda sabitlenmiş kısayol Workbench'i başlatma
Güncelleştirme zaten yüklü. Ancak, sabitlenmiş kısayol hala eski BITS diskte işaret. Bunu göz atarak doğrulayın `%localappdata%/AmlWorkbench` klasörü ve en son sürümü yüklü olan ve burada işaret görmek için sabitlenmiş kısayol özelliğini inceleyin bakın. Doğruladıysanız, eski kısayol kaldırmanız, Başlat Menüsü'nden Workbench'i başlatın ve isteğe bağlı olarak görev çubuğunda yeni sabitlenmiş bir kısayol oluşturun.

### <a name="you-installed-workbench-using-the-install-azure-ml-workbench-link-on-a-windows-dsvm"></a>"Azure ML Workbench'i yükleyin" bağlantısı üzerinde bir Windows DSVM'sini kullanma Workbench yüklü
Ne yazık ki bu bir kolayca düzeltme yoktur. Yüklü BITS kaldırın ve Workbench yeni yükleme için en son yükleyiciyi indirmek için aşağıdaki adımları gerçekleştirmeniz gerekir: 
   - klasörü Kaldır `C:\Users\<Username>\AppData\Local\amlworkbench`
   - betik Kaldır `C:\dsvm\tools\setup\InstallAMLFromLocal.ps1`
   - Yukarıdaki komut dosyasını başlatan bir masaüstü kısayolu Kaldır
   - yükleyiciyi indirin https://aka.ms/azureml-wb-msi ve yeniden yükleyin.

## <a name="stuck-at-checking-experimentation-account-screen-after-logging-in"></a>Oturum açtıktan sonra "deneme hesabı denetimi" ekranında takılı
Oturum açtıktan sonra Workbench uygulaması üzerinde boş bir ekran "Denetleme deneme hesabı" dönen tekerleğiyle gösteren bir iletiyle tıkanıp. Bu sorunu çözmek için aşağıdaki adımları uygulayın:
1. Uygulamayı kapatma
2. Aşağıdaki dosyayı silin:
  ```
  # on Windows
  %appdata%\AmlWorkbench\AmlWb.settings

  # on macOS
  ~/Library/Application Support/AmlWorkbench/AmlWb.settings
  ```
3. Uygulamayı yeniden başlatın.

## <a name="cant-delete-experimentation-account"></a>Deneme hesabı silinemiyor
Deneme hesabını silmek için CLI'yi kullanabilirsiniz, ancak alt çalışma alanları ve alt projeler bu alt çalışma önce silmeniz gerekir. Aksi halde, "iç içe kaynaklar silinmeden önce hata kaynak silinemiyor." bakın

```azure-cli
# delete a project
$ az ml project delete -g <resource group name> -a <experimentation account name> -w <workspace name> -n <project name>

# delete a workspace 
$ az ml workspace delete -g <resource group name> -a <experimentation account name> -n <workspace name>

# delete an experimentation account
$ az ml account experimentation delete -g <resource group name> -n <experimentation account name>
```

Workbench uygulama içinde çalışma alanlarından ve projeleri de silebilirsiniz.

## <a name="cant-open-file-if-project-is-in-onedrive"></a>Proje Onedrive'da ise dosyası açılamıyor
Windows 10 Fall Creators Update ve projeniz Onedrive'a eşlenmiş yerel bir klasörde oluşturulur, Workbench'te herhangi bir dosya açılamıyor bulabilirsiniz. Node.js kodu OneDrive klasöründe başarısız olmasına neden olan Fall Creators Update tarafından sunulan bir hata nedeniyle budur. Hata, Windows update tarafından ancak o zamana kadar olan en kısa sürede düzeltilecektir, lütfen projeleri OneDrive klasöründe oluşturmayın.

## <a name="file-name-too-long-on-windows"></a>Windows dosya adı çok uzun
Windows üzerinde Workbench kullanırsanız, bir "Sistem belirtilen yolu bulamıyor" hata olarak yüzey varsayılan en fazla 260 karakter dosya adı uzunluk sınırını karşılaşabileceğiniz. Çok uzun dosya yolu adı izin vermek için bir kayıt defteri anahtarı ayarı değiştirebilirsiniz. Gözden geçirme [bu makalede](https://msdn.microsoft.com/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath) nasıl ayarlanacağı hakkında daha fazla ayrıntı için _MAX_PATH_ kayıt defteri anahtarı.

## <a name="interrupt-cli-execution-output"></a>CLI yürütme çıkışı kesme
Kullanarak çalışan bir deneme yaslanıp, `az ml experiment submit` veya `az ml notebook start` ve çıkış kesmek istiyorsanız: 
- Windows üzerinde klavye Ctrl-Break tuş bileşimini kullanın
- MacOS üzerinde Ctrl-C'dir kullanın

Bu yalnızca çıkış akışına CLI penceresinde kesmeleri unutmayın. Yürütülmekte olan bir işi gerçekten durdurmaz. Devam eden bir işi iptal etmek istiyorsanız, kullanın `az ml experiment cancel -r <run_id> -t <target name>` komutu.

Break anahtar olmayan klavyeler birlikte Windows bilgisayarlarda Fn B, Ctrl Fn B veya Fn + Esc olası alternatifler içerir. Belirli bir tuş bileşimi için donanım satıcınızın belgelerine bakın.

## <a name="docker-error-read-connection-refused"></a>Docker hata "okuma: bağlantı reddedildi"
Bazen yerel bir Docker kapsayıcısı karşı yürütülürken, aşağıdaki hatayı görebilirsiniz: 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```

Docker DNS sunucusundan değiştirerek düzeltebilirsiniz `automatic` sabit değerini `8.8.8.8`.

## <a name="remove-vm-execution-error-no-tty-present"></a>VM yürütme hatası "tty mevcut" Kaldır
Uzak Linux makine üzerinde bir Docker kapsayıcısı karşı çalıştırılırken şu hata iletisini karşılaşabilirsiniz:
```
sudo: no tty present and no askpass program specified.
``` 
Ubuntu Linux sanal makinesi kök parolasını değiştirmek için Azure portalını kullanıyorsanız bu durum oluşabilir. 

Azure Machine Learning Workbench, uzak ana bilgisayarlarda çalıştırmak için parola olmadan sudoers erişim gerektirir. Bunu yapmanın en kolay yolu kullanmaktır _visudodan_ (oluşturma dosya henüz yoksa) aşağıdaki dosyayı düzenlemek için:

```
$ sudo visudo -f /etc/sudoers
```

>[!IMPORTANT]
>Dosyayı düzenlemek önemlidir _visudodan_ ve değil başka bir komutu. _visudodan_ söz dizimi tüm sudo yapılandırma dosyaları otomatik olarak denetler ve başarısızlık sözdizimsel olarak doğru sudoers dosyası üretmek için sudo dışında kilitleyebilir.

Dosyanın sonunda aşağıdaki satırı ekleyin:

```
username ALL=(ALL) NOPASSWD:ALL
```

Burada _kullanıcıadı_ olduğundan, uzak bir ana bilgisayara oturum açmak için Azure Machine Learning Workbench adını kullanır.

Satır #includedir sonra yerleştirilmelidir "/ etc/sudoers.d", aksi takdirde, başka bir kural tarafından geçersiz kılınmış olabilir.

Daha karmaşık bir sudo yapılandırması varsa, sudo Ubuntu için burada belgelerine isteyebilirsiniz: https://help.ubuntu.com/community/Sudoers

Ubuntu tabanlı bir Linux VM Azure'da bir yürütme hedefi kullanmıyorsanız, yukarıdaki hata de oluşabilir. Yalnızca Ubuntu tabanlı bir Linux VM için uzaktan yürütme destekliyoruz. 

## <a name="vm-disk-is-full"></a>VM disk dolu
Azure'da yeni bir Linux VM oluşturduğunuzda varsayılan olarak 30 GB disk işletim sistemi için sahip olursunuz. Varsayılan olarak docker altyapısı aynı disk görüntüleri çekerek ve kapsayıcıları çalıştırmak için kullanır. Bu işletim sistemi diski doldurabilir ve böyle durumlarda, bir "VM Disk olan tam" hatası görürsünüz.

Hızlı düzeltme artık kullanmadığınız tüm Docker görüntülerini kaldırmaktır. Aşağıdaki Docker komutu yalnızca yapar. (Kuşkusuz, VM ile SSH Docker komutu bir bash kabuğundan yürütmek için gerekir.)

```
$ docker system prune -a
```

Ayrıca, bir veri diski ekleme ve Docker altyapısı, görüntüleri depolamak için veri diski kullanacak şekilde yapılandırın. İşte [veri diski ekleme](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk). Daha sonra [Docker görüntüleri depoladığı değişiklik](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Veya, işletim sistemi diski genişletebilirsiniz ve dokunma Docker altyapısı yapılandırması gerekmez. İşte [nasıl işletim sistemi diski genişletebilirsiniz](https://docs.microsoft.com/azure/virtual-machines/linux/expand-disks).

```azure-cli
# Deallocate VM (stopping will not work)
$ az vm deallocate --resource-group myResourceGroup  --name myVM

# Get VM's Disc Name
az disk list --resource-group myResourceGroup --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table

# Update Disc Size using above name
$ az disk update --resource-group myResourceGroup --name myVMdisc --size-gb 250
    
# Start VM    
$ az vm start --resource-group myResourceGroup  --name myVM
```

## <a name="sharing-c-drive-on-windows"></a>Windows üzerinde C sürücüsü paylaşımı
Windows yerel bir Docker kapsayıcısında yürütme, ayarı `sharedVolumes` için `true` içinde `docker.compute` altında dosya `aml_config` yürütme performansını iyileştirebilir. Ancak, bu C sürücüsünde paylaştığınız gerektirir _Windows için Docker aracı_. C sürücüsünün paylaşabilmek emin değilseniz, aşağıdaki ipuçlarını deneyin:

* Dosya Gezgini'ni kullanarak C sürücüsünde paylaşımı denetleyin
* Ağ bağdaştırıcısı ayarlarını açın ve kaldırma / "Microsoft Ağları için dosya ve Yazıcı Paylaşımı" vEthernet için yeniden yükleme
* Docker Ayarları'nı açın ve docker ayarlarındaki C sürücüden paylaşın
* Windows parola değişiklikleri paylaşımı etkiler. Dosya Gezgini'ni açın ve sonra da C sürücüsü yeniden paylaşabilir, yeni bir parola girin.
* Docker ile C sürücüsünün paylaşmak çalışırken de güvenlik duvarı sorunu ile karşılaşabilirsiniz. Bu [Stack Overflow post](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) yararlı olabilir.
* Etki alanı kimlik bilgilerini kullanarak C sürücüsünü paylaşırken paylaşımı etki alanı denetleyicisi (örneğin, ev ağı, genel wifi vb. için) erişilebilir değil nereden ağlarda çalışmayı durdurabilir. Daha fazla bilgi için [yazıya](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

Ayrıca en küçük bir performans maliyetine, ayarlayarak paylaşımı sorunu önleyebilirsiniz `sharedVolumne` için `false` içinde `docker.compute` dosya.

## <a name="wipe-clean-workbench-installation"></a>Temiz Workbench'i yükleme silme
Genellikle bunu yapmanız gerekmez. Ancak, yüklemenin temiz silme durumunda, adımlar şunlardır:

- Windows'da:
  - İlk olarak kullandığınızdan emin olun _Program Ekle veya Kaldır_ uygulaması _Denetim Masası_ kaldırmak için _Azure Machine Learning Workbench_ uygulama girişi.  
  - Ardından indirin ve aşağıdaki komut dosyalarından birini çalıştırın:
    - [Windows komut satırı betik](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.cmd).
    - [Windows PowerShell Betiği](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.ps1). (Çalıştırmanız gerekebilir `Set-ExecutionPolicy Unrestricted` betiği çalıştırmadan önce bir yükseltilmiş ayrıcalık PowerShell penceresinde.)
- Macos'ta:
  - Yalnızca indirme ve çalıştırma [macOS bash Kabuk betiği](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_mac.sh).

## <a name="azure-ml-using-a-different-python-location-than-the-azure-ml-installed-python-environment"></a>Azure ML Azure ML farklı python yerleşimden kullanarak python ortamı yüklü
Azure Machine Learning Workbench yeni bir değişiklik nedeniyle, kullanıcılar artık Azure ML Workbench tarafından yüklenen python ortamı yerel çalıştırma işaret olduğunu fark edebilirsiniz. Bu, kullanıcının bilgisayarında yüklü başka bir python ortamı varsa ve söz konusu ortama işaret edecek şekilde "Python" yolunu ayarlayın ortaya çıkabilir. Azure ML Workbench'i kullanabilmeniz için Python ortamı yüklü, aşağıdaki adımları izleyin:
- Proje kökünüze altında aml_config klasörünüzün altında local.compute dosyasına gidin.
- Python ortamı Değiştir "pythonLocation" değişkenini Azure ML workbench fiziksel yolu işaret edecek şekilde yüklenir. Bu yol, iki yolla alabilirsiniz:
    - Azure ML python konumu %localappdata%\AmlWorkbench\python\python.exe bulunabilir
    - cmd Azure ML Workbench uygulamasını açın, python komut satırına yazın, sys.exe içeri aktarabilir, sys.executable çalıştırın ve buradan yolunu alın. 



## <a name="some-useful-docker-commands"></a>Bazı yararlı Docker komutları

Bazı yararlı Docker komutları şunlardır:

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
