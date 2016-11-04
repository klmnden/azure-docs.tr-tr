Bazı paketler, Azure üzerinde çalıştığında pip kullanılarak yüklenemez.  En basit haliyle, paket Python Paket Dizini’nde bulunmayabilir.  Derleyici gerekiyor olabilir (Azure App Service’te web uygulaması çalıştıran makinede derleyici olmayabilir).

Bu bölümde, bu sorunu düzeltmenin yollarını inceleyeceğiz.

### Tekerlek isteği
Paket yüklemesine bir derleyici gerekiyorsa, paket sahibinden pakete tekerleri de katmasını istemek için görüşmeye çalışmalısınız.

[Python 2.7 için Microsoft Visual C++ Derleyicisi][Python 2.7 için Microsoft Visual C++ Derleyicisi] uygulamasının yakın zamanda piyasaya çıkmasıyla, Python 2.7 için yerel koda sahip paketleri oluşturmak artık daha kolay.

### Tekerlek derleme (Windows gerekir)
Not: Bu seçenek kullanıldığında, Azure App Service’in web uygulamasında kullanılan platform/mimari/sürümle eşleşen Python ortamı kullanılarak paketin derlendiğinden emin olun (Windows/32-bit/2.7 veya 3.4).

Tekerlek gerektiğinden paket yüklenmediyse, derleyiciyi yerel makinenize yükleyip paket için bir tekerlek derleyebilirsiniz; bundan sonra depoda yerini alacaktır.

Mac/Linux Kullanıcıları: Windows makinesine erişiminiz yoksa Azure’da VM oluşturmak için bkz. [Windows çalıştıran bir sanal makine oluşturma][Windows çalıştıran bir sanal makine oluşturma].  Tekerlekleri derlemek için bunu kullanabilir, bunları depoya ekleyebilir ve isterseniz VM’yi atabilirsiniz. 

Python 2.7 için [Python 2.7 için Microsoft Visual C++ Derleyicisi][Python 2.7 için Microsoft Visual C++ Derleyicisi] uygulamasını yükleyebilirsiniz.

Python 3.4 için [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express] uygulamasını yükleyebilirsiniz.

Tekerlekleri derlemek için tekerlek paketi gerekir:

    env\scripts\pip install wheel

Bağımlılığı derlemek için `pip wheel` öğesini kullanacaksınız:

    env\scripts\pip wheel azure==0.8.4

Bu \wheelhouse klasöründe bir .whl dosyası oluşturur.  \Wheelhouse klasörünü ve tekerlek dosyalarını deponuza ekleyin.

`--find-links` seçeneğini en üstüne eklemek amacıyla requirements.txt dosyanızı düzenleyin. Bu, pip’ye python paket dizinine gitmeden önce yerel klasörde tam bir eşleşme aramasını bildirir.

    --find-links wheelhouse
    azure==0.8.4

Tüm bağımlılıklarınızı \Wheelhouse klasörüne eklemek ve python paket dizinini hiçbir şekilde kullanmamak isterseniz, requirements.txt dosyanızın en üstüne `--no-index` ekleyerek pip’yi paket dizinini yok sayması için zorlayabilirsiniz.

    --no-index

### Yüklemeyi özelleştirme
easy\_install gibi alternatif bir yükleyici kullanarak sanal ortama paket yüklemek için dağıtım betiğini özelleştirebilirsiniz.  Açıklama satırı yapılan bir örnek için deploy.cmd dosyasına bakın.  pip’in bunları yüklenmesini önlemek için requirements.txt dosyasında bu gibi paketlerin listelenmediğinden emin olun.

Bunu dağıtım betiğine ekleyin:

    env\scripts\easy_install somepackage

Bir exe yükleyiciden yükleme yapacak easy\_install öğesini de kullanabilirsiniz (bunlardan bazıları zip uyumludur; bu nedenle easy\_install bunları destekler).  Yükleyiciyi depoya ekleyin ve yolu yürütülebilir dosyaya geçirerek easy\_install öğesini çalıştırın.

Bunu dağıtım betiğine ekleyin:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### Sanal ortamı depoya ekleme (Windows gerekir)
Not: Bu seçenek kullanıldığında, Azure App Service’in web uygulamasında kullanılan platform/mimari/sürümle eşleşen sanal ortam kullanıldığından emin olun (Windows/32-bit/2.7 veya 3.4).

Sanal ortamı depoya eklerseniz, dağıtım betiğinin boş bir dosya oluşturarak Azure’da sanal ortamı yönetimi gerçekleştirmesine engel olabilirsiniz:

    .skipPythonDeployment

Sanal ortam otomatik olarak yönetildiği sırada kalan dosyaları engellemek için uygulamada var olan sanal ortamı silmenizi öneririz.

[Windows çalıştıran bir sanal makine oluşturma]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Python 2.7 için Microsoft Visual C++ Derleyicisi]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949


<!--HONumber=Sep16_HO3-->


