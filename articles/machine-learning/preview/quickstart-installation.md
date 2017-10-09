---
title: "Azure Machine Learning hizmetleri Yükleme Hızlı Başlangıcı | Microsoft Docs"
description: "Bu Hızlı Başlangıç, Azure Machine Learning kaynakları oluşturma ve Azure Machine Learning Workbench’i yükleme işlemini göstermektedir."
services: machine-learning
author: hning86
ms.author: haining, raymondl, chhavib
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: hero-article
ms.date: 09/20/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 91d2f47a528050f644973044f96c0354b91dba25
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---

# <a name="create-azure-machine-learning-preview-accounts-and-install-azure-machine-learning-workbench"></a>Azure Machine Learning önizleme hesapları oluşturma ve Azure Machine Learning Workbench’i yükleme
Azure Machine Learning, uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu Hızlı Başlangıç, Azure Machine Learning hizmetleri önizlemesinde deneme ve model yönetim hesapları oluşturma işlemini göstermektedir. Ayrıca, Azure Machine Learning Workbench masaüstü uygulamasını ve CLI araçlarını yükleme adımları gösterilmektedir. Ardından, bazı fiziksel özelliklerine göre iris türünü tahmin eden bir model oluşturmak üzere zamansız [Iris çiçek veri kümesini](https://en.wikipedia.org/wiki/iris_flower_data_set) kullanarak Azure Machine Learning önizleme özelliklerinde hızlı bir tura çıkacaksınız.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Şu anda Azure Machine Learning Workbench yalnızca şu işletim sistemlerine yüklenebilir: Windows 10, Windows Server 2016 ve macOS Sierra.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-azure-machine-learning-accounts"></a>Azure Machine Learning hesapları oluşturma
Azure Machine Learning hesapları sağlamak için Azure portalını kullanın. 
1. Portalın sol üst köşesinde bulunan **Yeni** düğmesini (+) seçin.

2. Arama çubuğuna "Machine Learning" yazın. **Machine Learning Denemesi (önizleme)** adlı arama sonucunu seçin.  Bu seçimi Azure portalında sık kullanılanlara eklemek için yıldız simgesine tıklayın.

   ![Azure Machine Learning Araması](media/quickstart-installation/portal-more-services.png)

3. Yeni bir Machine Learning Denemesi hesabı yapılandırmak için **+ Ekle** öğesine tıklayın. Ayrıntılı form açılır.

   ![Machine Learning Denemesi Hesabı](media/quickstart-installation/portal-create-experimentation.png)

4. Machine Learning Denemesi formunu aşağıdaki bilgilerle doldurun:

   Ayar|Önerilen değer|Açıklama
   ---|---|---
   Deneme hesabı adı | _Benzersiz ad_ |Hesabınızı tanımlayan benzersiz bir ad seçin. Kendi adınızı veya denemeyi en iyi şekilde tanımlayan departman ya da proje adını kullanabilirsiniz. Ad 2 ile 32 karakter arasında olmalı ve yalnızca alfasayısal karakterler ile '-' tire karakteri içermelidir. 
   Abonelik | _Aboneliğiniz_ |Denemeniz için kullanmak istediğiniz Azure aboneliği. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin.
   Kaynak Grubu | _Kaynak grubunuz_ | Yeni bir kaynak grubu adı oluşturabilir veya mevcut bir aboneliğinizi kullanabilirsiniz.
   Konum | _Kullanıcılarınıza en yakın bölge_ | Kullanıcılarınız ve veri kaynaklarınız için en yakın konumu seçin.
   Bilgisayar lisansı sayısı | 2 | Bilgisayar lisansı sayısını yazın. Bu seçim [fiyatlandırmayı](https://azure.microsoft.com/pricing/details/machine-learning/) etkiler. İlk iki bilgisayar lisansı ücretsizdir. Bu Hızlı Başlangıç için iki bilgisayar lisansı kullanın. Bilgisayar lisansı sayısını daha sonra Azure portalında gereken şekilde güncelleştirebilirsiniz.
   Depolama Hesabı | _Benzersiz ad_ | **Yeni oluştur**’u seçin ve bir ad girerek yeni bir Azure depolama hesabı oluşturun veya **Var olanı kullan**’ı seçip açılan listeden var olan depolama hesabınızı belirleyin. Depolama hesabı gereklidir ve proje yapıtlarını tutmak ve geçmiş verileri çalıştırmak için kullanılır. 
   Deneme hesabı için çalışma alanı | _Benzersiz ad_ | Yeni çalışma alanı için bir ad belirtin. Ad 2 ile 32 karakter arasında olmalı ve yalnızca alfasayısal karakterler ile '-' tire karakteri içermelidir.
   Çalışma alanının sahibini atama | _Hesabınız_ | Çalışma alanı sahibi olarak kendi hesabınızı seçin.
   Model Yönetimi Hesabı Oluşturma | *işaretle* | Deneme hesabı oluşturma deneyiminin bir parçası olarak, aynı zamanda Machine Learning Model Yönetimi hesabını oluşturma seçeneğine sahipsiniz. Bu kaynak, modellerinizi gerçek zamanlı web hizmetleri olarak dağıtıp yönetmeye hazır olduğunuzda kullanılır. Model Yönetimi hesabını Deneme hesabı ile aynı zamanda oluşturmanız önerilir.
   Hesap Adı | _Benzersiz ad_ | Model Yönetimi hesabınızı tanımlayan benzersiz bir ad seçin. Kendi adınızı veya denemeyi en iyi şekilde tanımlayan departman ya da proje adını kullanabilirsiniz. Ad 2 ile 32 karakter arasında olmalı ve yalnızca alfasayısal karakterler ile '-' tire karakteri içermelidir. 
   Model Yönetimi fiyatlandırma katmanı | **DEVTEST** | Yeni Model Yönetimi hesabınıza ait fiyatlandırma katmanını belirtmek için **Fiyatlandırma katmanı seçilmedi** öğesine tıklayın. Maliyet tasarrufu için, aboneliğinizde varsa **DEVTEST** fiyatlandırma katmanını (sınırlı kullanılabilirlik), yoksa S1 fiyatlandırma katmanını seçin. Fiyatlandırma katmanı seçimini kaydetmek için **Seç**’e tıklayın. 
   Panoya sabitle | _işaretle_ | Azure Portal'ın ön pano sayfasında Machine Learning Deneme hesabınızın kolayca izlenmesine izin vermek için **Panoya Sabitle** seçeneğini işaretleyin.

5. Oluşturma işlemini başlatmak için **Oluştur** öğesine tıklayın.

6. Azure portalı araç çubuğunun sağ üst köşesinde **Bildirimler**’e (zil simgesi) tıklayarak dağıtım işlemini izleyin. 

   Bildirim "Dağıtım devam ediyor..." ifadesini gösterir. İşlem tamamlandıktan sonra durum "Dağıtım başarılı" olarak değişir. Başarılı olduktan sonra Machine Learning Denemesi hesap sayfanız açılır.
   
   ![Azure portalı bildirimleri](media/quickstart-installation/portal-notification.png)

Yerel bilgisayarınızda kullandığınız işletim sistemine bağlı olarak, sonraki iki bölümden birini izleyerek Azure Machine Learning Workbench’i bilgisayarınıza yükleyin. 

## <a name="install-azure-machine-learning-workbench-on-windows"></a>Windows’da Azure Machine Learning Workbench Yükleme
Azure Machine Learning Workbench’i Windows 10, Windows Server 2016 veya daha yeni bir sürümü çalıştıran bilgisayarınıza yükleyin.

1. En son Azure Machine Learning Workbench yükleyicisi **[AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi)** dosyasını indirin.

2. İndirilen _AmlWorkbenchSetup.msi_ yükleyicisine Dosya Gezgini’nde çift tıklayın.

   >[!IMPORTANT]
   >Yükleyiciyi diske tam olarak indirip oradan başlatın. Doğrudan tarayıcınızın indirme penceresinden başlatmayın.

3. Ekrandaki yönergeleri izleyerek yüklemeyi tamamlayın.

   Yükleyici Python, Miniconda ve diğer ilgili kitaplıklar gibi gereken tüm bağımlı bileşenleri indirir. Yüklemenin tüm bileşenleri tamamlaması yaklaşık yarım saat sürebilir. 

4. Azure Machine Learning Workbench aşağıdaki dizine yüklenir:
   
   `C:\Users\<user>\AppData\Local\AmlWorkbench`

## <a name="install-azure-machine-learning-workbench-on-macos"></a>macOS’ta Azure Machine Learning Workbench Yükleme
Azure Machine Learning Workbench’i macOS Sierra çalıştıran bilgisayarınıza yükleyin.

1. [Homebrew](http://brew.sh) kullanarak openssl kitaplığını yükleyin. Daha fazla bilgi için bkz. [Mac üzerinde .NET Core Önkoşulları](https://docs.microsoft.com/dotnet/core/macos-prerequisites).
   ```
   # install Homebrew first if you don't have it already
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   # install latest openssl needed for .NET Core 1.x
   brew update
   brew install openssl
   mkdir -p /usr/local/lib
   ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
   ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
   ```

2. En son Azure Machine Learning Workbench yükleyicisi **[AmlWorkbench.dmg](https://aka.ms/azureml-wb-dmg)** dosyasını indirin.

   >[!IMPORTANT]
   >Yükleyiciyi diske tam olarak indirip oradan başlatın. Doğrudan tarayıcınızın indirme penceresinden başlatmayın.

3. İndirilen _AmlWorkbench.dmg_ yükleyicisine Finder’da çift tıklayın.

4. Ekrandaki yönergeleri izleyerek yüklemeyi tamamlayın.

   Yükleyici Python, Miniconda ve diğer ilgili kitaplıklar gibi gereken tüm bağımlı bileşenleri indirir. Yüklemenin tüm bileşenleri tamamlaması yaklaşık yarım saat sürebilir. 

5. Azure Machine Learning Workbench aşağıdaki dizine yüklenir: 

   _/Applications/AmlWorkbench.app_

## <a name="run-azure-machine-learning-workbench-to-log-in-the-first-time"></a>İlk kez oturum açmak üzere Azure Machine Learning Workbench’i çalıştırma
1. Yükleme işlemi tamamlandıktan sonra, yükleyicinin son ekranındaki **Workbench’i Başlat** düğmesine tıklayın. Yükleyiciyi kapattıysanız, masaüstü ve başlat menüsünde **Azure Machine Learning Workbench** adlı Machine Learning Workbench kısayolunu bularak uygulamayı başlatın.

2. Daha önce Azure kaynaklarınızı sağlamak için kullandığınız hesabı kullanarak Workbench oturumunu açın. 

3. Oturum açma işlemi başarılı olduğunda, Workbench daha önce oluşturduğunuz Machine Learning Denemesi hesaplarını bulmaya çalışır. Kimlik bilginizin erişimi olan tüm Azure aboneliklerini arar. En az bir Deneme Hesabı bulunduğunda, Workbench bu hesapla açılır. Daha sonra bu hesapta bulunan Çalışma Alanları ve Projeler listelenir. 

   >[!TIP]
   > Birden fazla Deneme Hesabına erişiminiz varsa, Workbench uygulamasının sol alt köşesindeki avatar simgesine tıklayarak farklı bir hesaba geçiş yapabilirsiniz.

Web hizmetlerinizi dağıtmaya yönelik bir ortam oluşturmak için bkz. [Dağıtım Ortamı Kurulumu](deployment-setup-configuration.md).

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma
1. Azure ML Workbench uygulamasını başlatıp oturum açın. 

2. **Dosya** --> **Yeni Proje**’e tıklayın (veya **+** öğesine tıklayıp **PROJELER** bölmesinde oturum açın). 

3. **Proje adı** ve **Proje dizini** alanlarını doldurun. **Proje açıklaması** isteğe bağlıdır, ancak faydalıdır. **Visualstudio.com GIT Deposu URL’si** alanını şimdilik boş bırakın. Bir çalışma alanı seçin ve proje şablonu olarak **Iris Sınıflandırma** seçeneğini belirleyin.

   >[!TIP]
   >İsteğe bağlı olarak, [VSTS (Visual Studio Team Service)](https://www.visualstudio.com) projesinde barındırılan bir Git deposunun URL’si ile Git deposu metin alanını doldurabilirsiniz. Bu Git deposu önceden var olmalıdır ve ana dal içermeksizin boş olmalıdır. Ayrıca bu depoya yazma erişiminiz olmalıdır. Şu anda bir Git deposunun eklenmesi, dolaşıma ve daha sonra senaryoları paylaşmanıza olanak tanır. [Daha fazla bilgi edinin](using-git-ml-project.md).

4. Projeyi oluşturmak için **Oluştur** düğmesine tıklayın. Yeni bir proje oluşturulup açılır. Bu noktada proje giriş sayfası, veri kaynakları, dizüstü bilgisayarlar, kaynak kodu dosyalarını keşfedebilirsiniz. 

    >[!TIP]
    >Ayrıca, projeyi VS Code veya diğer düzenleyicilerde bir IDE (Tümleşik Geliştirme Ortamı) bağlantısı yapılandırarak açabilir ve sonra içindeki proje dizinini açabilirsiniz. [Daha fazla bilgi edinin](how-to-configure-your-IDE.md). 

## <a name="run-a-python-script"></a>Python betiği çalıştırma
Şimdi yerel bilgisayarınızda bir betik yürütelim. 

1. Her proje kendi **Proje Panosu** sayfasında açılır. Uygulamanın üst kısmındaki çalıştır düğmesinin solunda bulunan komut çubuğundan yürütme hedefi olarak `local` öğesini ve çalıştırılacak betik olarak `iris_sklearn.py` öğesini seçin.  Örnekte daha sonra bakabileceğiniz birkaç diğer dosya da mevcuttur. 

   ![görüntü](media/quickstart-installation/run_control.png)

2. **Bağımsız değişkenler** metni alanına `0.01` girin. Bu sayı kodda doğrusal regresyon modelinin eğitimini yapılandırmak için kullanılan bir değer olan düzenleme hızını ayarlamak için kullanılır. 

3. Bilgisayarınızda `iris_sklearn.py` yürütmeye başlamak için **Çalıştır** düğmesine tıklayın. 

   Bu kod, modeli oluşturmak için popüler Python [scikit-learn](http://scikit-learn.org/stable/index.html) kitaplığındaki [mantıksal regresyon](https://en.wikipedia.org/wiki/logistic_regression) algoritmasını kullanır.

4. **İşler** paneli henüz görünmüyorsa sağ taraftan kaydırılır ve panele bir `iris_sklearn` işi eklenir. İş çalıştırılmaya başladığında **Gönderiliyor** olan durum **Çalıştırılıyor** olarak değişir, birkaç saniye sonra ise **Tamamlandı** olur. 

5. Tebrikler. Azure ML Workbench uygulamasında Python betiğini başarıyla yürüttünüz.

6. Adım 2-4’ü birkaç kez yineleyin. Her defasında `10` ila `0.001` aralığındaki farklı bağımsız değişken değerlerini kullanın.

## <a name="view-run-history"></a>Çalıştırma geçmişini görüntüleme
1. **Çalıştırmalar** görünümüne gidin ve çalıştırma listesindeki **iris_sklearn.py** öğesine tıklayın. `iris_sklearn.py` için çalıştırma geçmişi panosu açılır. `iris_sklearn.py` üzerinde yürütülen her çalıştırmayı gösterir. 

   ![görüntü](media/quickstart-installation/run_view.png)

2. Çalıştırma geçmişi panosu ayrıca her çalışma için başlıca ölçümleri, bir dizi varsayılan grafiği ve ölçüm listesini görüntüler. Yapılandırma simgesine veya filtre simgesine tıklayarak yapılandırmaları sıralama, filtreleme ve ayarlama yoluyla bu görünümü özelleştirebilirsiniz.

   ![görüntü](media/quickstart-installation/run_dashboard.png)

3. Tamamlanmış bir çalıştırmaya tıkladığınızda, ek ölçümler, oluşturulan dosyalar ve faydalı olabilecek diğer günlükler dahil olmak üzere o yürütmenin ayrıntılı bir görünümünü görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bir Azure Machine Learning Denemesi hesabını ve Azure Machine Learning Model Yönetimi hesabını başarıyla oluşturdunuz. Azure Machine Learning Workbench masaüstü uygulamasını ve komut satırı arabirimini yüklediniz. Yeni bir proje oluşturdunuz, betik yürüterek bir model oluşturdunuz ve betiğin çalıştırma geçmişini incelediniz.

Iris modelinizi web hizmeti olarak dağıtma dahil olmak üzere bu iş akışı hakkında daha ayrıntılı bir deneyim için [veri hazırlığı](tutorial-classifying-iris-part-1.md), [ deneme](tutorial-classifying-iris-part-2.md) ve [model yönetimi](tutorial-classifying-iris-part-3.md) hakkında ayrıntılı adımları içeren Iris Sınıflandırma Öğreticisinin tamamını izleyin. 

> [!div class="nextstepaction"]
> [Iris Sınıflandırma Öğreticisi](tutorial-classifying-iris-part-1.md)

