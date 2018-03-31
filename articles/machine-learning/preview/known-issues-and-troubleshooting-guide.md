---
title: Bilinen sorunlar ve sorun giderme kılavuzu | Microsoft Docs
description: Bilinen sorunların listesi ve gidermenize yardımcı olması için bir kılavuz
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: 3699e2a59061d8a2870a263588917268ca504866
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning çalışma ekranı - bilinen sorunlar ve sorun giderme kılavuzu 
Bu makalede, bulma ve hataları düzeltin ya da Azure Machine Learning çalışma ekranı uygulamasını kullanarak bir parçası olarak karşılaşılan hataları yardımcı olur. 

## <a name="find-the-workbench-build-number"></a>Çalışma ekranı yapı numarası Bul
Destek Ekibi ile iletişim kurarken, çalışma ekranı uygulamanın yapı numarası eklemek önemlidir. Windows üzerinde tıklayarak yapı numarası bulabilirsiniz **yardımcı** menü ve **hakkında Azure ML çalışma ekranı**. MacOS üzerinde tıklatabilirsiniz **Azure ML çalışma ekranı** menü ve **hakkında Azure ML çalışma ekranı**.

## <a name="machine-learning-msdn-forum"></a>Machine Learning MSDN Forumu
Sorular nakledebilirsiniz bir MSDN Forumu sunuyoruz. Ürün ekibi forum etkin olarak izler. URL Forumu [ https://aka.ms/azureml-forum ](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Tanılama bilgileri toplayın
Bazen yardımını isterken tanı bilgilerini sağlarsanız, yararlı olabilir. Günlük dosyalarının nerede Canlı aşağıda verilmiştir:

### <a name="installer-log"></a>Yükleyici günlüğü
Yükleme sırasında sorunu yaşayıp çalıştırırsanız, yükleyici günlük dosyaları şunlardır:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
Bu dizinlerin içerikleri zip ve tanılama için bize gönderin.

### <a name="workbench-desktop-app-log"></a>Çalışma ekranı Masaüstü uygulama günlüğü
Oturum açma konusunda sorun yaşıyorsanız veya çalışma ekranı Masaüstü çökerse, günlük dosyalarını burada bulabilirsiniz:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
Bu dizinlerin içerikleri zip ve tanılama için bize gönderin.

### <a name="experiment-execution-log"></a>Deneme yürütme günlüğü
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

- Hedef işlem olarak Hdınsight kümesi kullanırken, Azure kullanmalısınız blob birincil depolama olarak. Azure Data Lake Storage kullanılması desteklenmez.

- Metin kümeleme dönüşümler Mac üzerinde desteklenmez.

- RevoScalePy kitaplığı yalnızca Windows ve Linux (Docker kapsayıcılardaki) desteklenir. MacOS üzerinde desteklenmiyor.

- Jupyter not defterleri çalışma ekranı uygulamadan açmadan 5 MB maksimum boyut sınırı vardır. 'Az ml Not start' komutunu kullanarak CLI büyük not defterlerini açabilir ve dosya boyutunu azaltmak için temiz hücre çıkarır.

## <a name="cant-update-workbench"></a>Çalışma ekranı güncelleştirilemiyor
Yeni bir güncelleştirme kullanılabilir olduğunda, çalışma ekranı uygulama giriş sayfası yeni güncelleştirme hakkında bildiren bir ileti görüntüler. Sol alt köşesindeki uygulama zil simgesine görünen bir güncelleştirme rozet görmeniz gerekir. Gösterge üzerinde tıklatın ve güncelleştirmeyi yüklemek için yükleyici Sihirbazı izleyin. 

![Güncelleştirme resmi](./media/known-issues-and-troubleshooting-guide/update.png)

Bildirim görmüyorsanız, uygulamanın yeniden başlatmayı deneyin. Yeniden başlatma işleminden sonra güncelleştirme bildirimi hala görmüyorsanız, birkaç nedeni olabilir.

### <a name="you-are-launching-workbench-from-a-pinned-shortcut-on-the-task-bar"></a>Görev çubuğunda sabitlenmiş kısayoldan çalışma ekranı başlatma
Güncelleştirme zaten yüklenmiş. Ancak sabitlenmiş kısayolu hala eski BITS diskteki işaret. Bunu göz atarak doğrulamak `%localappdata%/AmlWorkbench` klasörü ve en son sürümü yüklü varsa ve burada işaret ettiğinden görmek için sabitlenmiş kısayol özelliğini inceleyin bakın. Doğrulandı, yalnızca eski kısayol kaldırın, çalışma ekranı Başlat menüsünden başlatın ve isteğe bağlı olarak görev çubuğunda yeni sabitlenmiş bir kısayol oluşturun.

### <a name="you-installed-workbench-using-the-install-azure-ml-workbench-link-on-a-windows-dsvm"></a>Bir Windows DSVM "Azure ML çalışma ekranı Yükle" bağlantıyı kullanarak çalışma ekranı yüklü
Ne yazık ki bu bir kolay düzeltme yoktur. Yüklü BITS kaldırın ve çalışma ekranı yeni yükleme için en son yükleyici indirmek için aşağıdaki adımları uygulamanız gerekir: 
   - klasörü kaldırın `C:\Users\<Username>\AppData\Local\amlworkbench`
   - komut dosyasını kaldırma `C:\dsvm\tools\setup\InstallAMLFromLocal.ps1`
   - Yukarıdaki komut dosyasını başlatır masaüstü kısayolu
   - Yükleyici indirmek https://aka.ms/azureml-wb-msi ve yeniden yükleyin.

## <a name="stuck-at-checking-experimentation-account-screen-after-logging-in"></a>Oturum açtıktan sonra "deneme hesabı denetim" ekranında takılmış
Oturum açtıktan sonra çalışma ekranı uygulama boş bir ekranda dönen Tekerlek "denetleme deneme hesabı" gösteren bir iletiyle takılı. Bu sorunu çözmek için aşağıdaki adımları uygulayın:
1. Uygulama kapatma
2. Aşağıdaki dosya sil:
  ```
  # on Windows
  %appdata%\AmlWorkbench\AmlWb.settings

  # on macOS
  ~/Library/Application Support/AmlWorkbench/AmlWb.settings
  ```
3. Uygulamayı yeniden başlatın.

## <a name="cant-delete-experimentation-account"></a>Deneme hesabı silinemiyor
Bir deneme hesabı silmek için CLI kullanabilirsiniz, ancak alt çalışma alanları ve bu alt çalışma içinde alt projeleri silmeniz gerekir. Aksi takdirde, "iç içe kaynaklar silinmeden önce hata kaynak silinemiyor." konusuna bakın

```azure-cli
# delete a project
$ az ml project delete -g <resource group name> -a <experimentation account name> -w <worksapce name> -n <project name>

# delete a workspace 
$ az ml workspace delete -g <resource group name> -a <experimentation account name> -n <worksapce name>

# delete an experimentation account
$ az ml account experimentation delete -g <resource group name> -n <experimentation account name>
```

Ayrıca, projeler ve çalışma ekranı uygulama içinde çalışma alanlarından silebilirsiniz.

## <a name="cant-open-file-if-project-is-in-onedrive"></a>Onedrive'da projesiyse dosyası açılamıyor
Windows 10 sonbaharda oluşturucuları güncelleştirme varsa ve projenizin OneDrive eşlenmiş yerel bir klasörde oluşturulur, çalışma ekranı içinde herhangi bir dosyayı açamıyor bulabilirsiniz. Bu node.js kodu OneDrive klasöründe başarısız olmasına neden olan sonbaharda oluşturucuları Update tarafından sunulan bir hatadan kaynaklanır. Hatanın en kısa sürede Windows update tarafından ancak o kadar düzeltilecektir, lütfen projeleri OneDrive klasöründe oluşturmayın.

## <a name="file-name-too-long-on-windows"></a>Windows dosya adı çok uzun
Windows çalışma ekranı kullanıyorsanız, "Sistem belirtilen yol bulunamıyor" hata olarak yüzey varsayılan en fazla 260 karakter dosya adı uzunluğu sınırı içinde çalıştırabilirsiniz. Çok uzun dosya yolu adı izin vermek için bir kayıt defteri anahtarı ayarı değiştirebilirsiniz. Gözden geçirme [bu makalede](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath) nasıl ayarlanacağı hakkında daha fazla ayrıntı için _MAX_PATH_ kayıt defteri anahtarı.

## <a name="interrupt-cli-execution-output"></a>CLI yürütme çıktısını kesme
Kullanarak çalışan bir deneme kazandırın varsa `az ml experiment submit` veya `az ml notebook start` ve çıktı kesmek istiyor musunuz: 
- Windows üzerinde klavyeden Ctrl-Break tuş bileşimini kullanın
- Ctrl + c macOS üzerinde kullanın

Bu yalnızca CLI penceresinde çıkış akışı keser unutmayın. Yürütülmekte olan bir işi aslında durdurmaz. Devam eden işi iptal etmek istiyorsanız, kullanmak `az ml experiment cancel -r <run_id> -t <target name>` komutu.

Break anahtar olmayan klavye Windows bilgisayarlarda, olası alternatifler Fn B, Ctrl Fn B veya Fn + Esc içerir. Belirli bir tuş bileşimini donanım satıcınızın belgelerine bakın.

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

Ayrıca, bir veri diski ekleyin ve görüntüleri saklamak için veri diski kullanmak için Docker altyapısına yapılandırın. Burada [bir veri diski ekleme](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk). Daha sonra [Docker görüntüleri depoladığı değişiklik](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Veya, işletim sistemi diski genişletebilirsiniz ve Docker altyapısı yapılandırması touch gerekmez. Burada [nasıl işletim sistemi diski genişletebilirsiniz](https://docs.microsoft.com/azure/virtual-machines/linux/expand-disks).

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

## <a name="sharing-c-drive-on-windows"></a>Windows C sürücüsünde paylaşımı
Windows yerel bir Docker kapsayıcısı içinde çalıştırıldığında, ayarı `sharedVolumes` için `true` içinde `docker.compute` altında dosya `aml_config` yürütme performansını iyileştirebilir. Ancak, bu C sürücüsünde paylaşmak gerektirir _Windows aracı için Docker_. C sürücüsünün paylaşmak mümkün değilse, aşağıdaki ipuçlarını deneyin:

* Dosya Gezgini'ni kullanarak C sürücüsünde paylaşımı denetleyin
* Ağ bağdaştırıcısı ayarları'nı açın ve kaldırma / "Microsoft Ağları için dosya ve Yazıcı Paylaşımı" vEthernet için yeniden yükleme
* Docker Ayarları'nı açın ve docker ayarları içinde C sürücüsünden paylaşma
* Windows parolası yapılan değişiklikler, paylaşım etkiler. Dosya Gezgini'ni açın, C sürücüsünün yeniden paylaşabilirsiniz ve yeni bir parola girin.
* Ayrıca, C sürücüsünün Docker ile paylaşmak çalışırken güvenlik duvarı sorunla karşılaşabilirsiniz. Bu [yığın taşması post](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) yararlı olabilir.
* Etki alanı kimlik bilgilerini kullanarak C sürücüsü paylaşırken paylaşımı burada etki alanı denetleyicisini (örneğin, ev ağı, ortak wifi vb. için) ulaşılabilir değil ağlarda çalışmayı durdurabilir. Daha fazla bilgi için bkz: [bu post](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

Ayrıca maliyet, ayarlayarak küçük bir performans adresindeki paylaşma sorun kaçının `sharedVolumne` için `false` içinde `docker.compute` dosya.

## <a name="wipe-clean-workbench-installation"></a>Temiz çalışma ekranı yükleme silme
Genellikle bu yapmanız gerekmez. Ancak, yüklemenin tamamen temizlerler durumda adımlar şunlardır:

- Windows'da:
  - Öncelikle, kullandığınızdan emin olun _Program Ekle veya Kaldır_ uygulaması _Denetim Masası_ kaldırmak için _Azure Machine Learning çalışma ekranı_ uygulama girişi.  
  - Ardından yükleyin ve aşağıdaki komut dosyalarından birini çalıştırın:
    - [Windows komut satırı komut dosyası](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.cmd).
    - [Windows PowerShell komut dosyası](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.ps1). (Çalıştırmanız gerekebilir `Set-ExecutionPolicy Unrestricted` komut dosyasını çalıştırmadan önce bir ayrıcalık yükseltilmiş PowerShell penceresinde.)
- On macOS:
  - Yalnızca indirme ve çalıştırma [macOS bash Kabuk betiği](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_mac.sh).

## <a name="azure-ml-using-a-different-python-location-than-the-azure-ml-installed-python-environment"></a>Azure ML yüklü python ortamı farklı python konumundan kullanarak azure ML
Azure Machine Learning çalışma ekranı son zamanlarda bir değişiklik nedeniyle, kullanıcılar yerel çalıştırmalar Azure ML çalışma ekranı tarafından artık yüklü python ortamı göstermiyor olabilir fark edebilirsiniz. Bu kullanıcı bilgisayarlarında yüklü başka bir python ortamı varsa ve bu ortama işaret edecek şekilde "Python" yolunu ayarlama ortaya çıkabilir. Azure ML çalışma ekranı kullanılabilmesi için Python ortamı yüklü, şu adımları izleyin:
- Proje kökü altındaki aml_config klasörünüze altında local.compute dosyasına gidin.
- Değişiklik Azure ML ekranının fiziksel yolunu işaret edecek şekilde "pythonLocation" değişkeni python ortamı yüklü. Bu yol iki yolla alabilirsiniz:
    - Azure ML python konumu %localappdata%\AmlWorkbench\python\python.exe bulunabilir
    - Azure ML çalışma ekranından cmd açın, python komut satırına yazın, sys.exe içeri aktarabilir, sys.executable çalıştırın ve buradan yolu. 



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
