---
title: "Python ve Azure SDK - yükleyin"
description: "Python ve SDK'sı Azure ile kullanmak için nasıl yükleneceğini öğrenin."
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.openlocfilehash: e69fff29be5b12c3c0004b4101eba69c7da87d3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="installing-python-and-the-sdk"></a>Python ve SDK yükleme
Python Windows ayarlamak kolaydır ve Mac, Linux, önceden yüklenmiş olarak gelir ve [Windows için Bash](https://msdn.microsoft.com/commandline/wsl/about). Bu kılavuzda, yükleme ve makinenizi Azure ile kullanım için hazır hale anlatılmaktadır.

## <a name="whats-in-the-python-azure-sdk"></a>Python nedir Azure SDK'sı?
Python için Azure SDK'sı geliştirmek, dağıtmak ve Azure için Python uygulamaları yönetmenize olanak tanıyan bileşenleri içerir. Özellikle, Python için Azure SDK'sı aşağıdakileri içerir:

* **Yönetim kitaplıklarını**. Bu sınıf kitaplıkları depolama hesapları, sanal makineler gibi Azure kaynaklarını yönetme bir arabirim sağlar.
* **Çalışma zamanı kitaplıkları**. Bu sınıf kitaplıkları, depolama ve service bus gibi Azure özellikleri erişmek için bir arabirim sağlar.

## <a name="which-python-and-which-version-to-use"></a>Hangi Python ve hangi sürümün kullanılacağını
Python yorumlayıcılar çeşitli özellikleri kullanılabilir - örnekler şunlardır:

* CPython - standart ve en yaygın kullanılan Python yorumlayıcı
* PyPy - CPython için hızlı, uyumlu bir alternatif uygulama
* IronPython - .net/CLR üzerinde çalışan Python yorumlayıcı
* Jython - Java sanal makinede çalışan Python yorumlayıcı

**CPython** v2.7 veya v3.3 + ve PyPy 5.4.0 test edilmiş ve Python Azure SDK'sı için desteklenir.

## <a name="where-to-get-python"></a>Python alınacağı?
CPython almak için birkaç yolu vardır:

* Doğrudan [www.python.org][www.python.org]
* Tanınmış distro gibi gelen [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] veya [www.activestate.com][www.activestate.com]
* Kaynağından oluşturun!

Belirli bir gereksiniminiz yoksa, ilk iki seçeneği öneririz.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Windows, Linux ve MacOS (yalnızca istemci kitaplıkları) SDK yükleme
Yüklü Python zaten varsa, bir paketin tüm istemci kitaplıkları, varolan Python 2.7 veya Python 3.3 + ortamında yüklemek için PIP kullanabilirsiniz. Bu paketten indirir [Python paket dizinini] [ Python Package Index] (Pypı).

Yönetici haklarına ihtiyacınız:

* Linux ve MacOS, `sudo` komutu: `sudo pip install azure-mgmt-compute`.
* Windows: PowerShell/komut istemini yönetici olarak açın.

Her Azure hizmet için ayrı ayrı her kitaplığı yükleyebilirsiniz:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Önizleme paketleri kullanılarak yüklenebilir `--pre` bayrağı:

```console
   $ pip install --pre azure-mgmt-compute # installs only the latest Compute Management library
```

Tek satırlı kullanımında Azure kitaplıkları kümesi yükleyebilirsiniz `azure` meta paket. Bu meta paketteki tüm paketler henüz, kararlı yayımlandığı tarihten sonra `azure` meta paketi hala önizlemede değil.
Bununla birlikte, kod kalitesini/bütünlük açılardan çekirdek paketleri şu anda "kararlı" kabul edilebilir

* Bu resmi olarak şekilde eşitlenmiş diğer dilleri ile mümkün olan en kısa sürede etiketlenir.
  Biz hakkında daha fazla önemli değişikliklere o zamana kadar planladığınıza değil.

Önizleme sürümü olduğundan, kullanmanız gereken `--pre` bayrağı:

```console
   $ pip install --pre azure
```

veya doğrudan

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Daha fazla paket alma
[Python paket dizinini] [ Python Package Index] (Pypı) zengin Python kitaplıkları sahiptir.  Bir Distro yüklemeyi seçtiyseniz, çeşitli web geliştirme senaryolarını teknik bilgi işlem için ilginç BITS çoğunu zaten sahip olacaksınız.

## <a name="python-tools-for-visual-studio"></a>Visual Studio için Python Araçları
[Visual Studio için Python Araçları][Visual Studio için Python Araçları] (PTVS) olan bir ücretsiz OSS eklenti VS tam özellikli bir Python IDE içinde kapatır Microsoft:

![How-to-Install-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

PTVS kullanarak isteğe bağlıdır, ancak bu, Python ve Web Proje/çözüm desteği, hata ayıklama, profil oluşturma, etkileşimli pencere, şablonu düzenlemek ve IntelliSense sağlar önerilir.

PTVS de kolaylaştırır dağıtımına desteği ile Microsoft Azure'a dağıtmak [bulut Hizmetleri](cloud-services/cloud-services-python-ptvs.md) ve [Web siteleri](app-service/app-service-web-overview.md).

PTVS mevcut Visual Studio 2013, 2015 veya 2017 yüklemenizle birlikte çalışır.  Belgeler, indirme ve tartışma için bkz: [Visual Studio için Python Araçları].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Linux ve MacOS Azure senaryoları
Linux veya MacOS, desteklenen ana Azure senaryoları için:

1. Python için istemci kitaplıkları kullanarak Azure Hizmetleri kullanma
2. Bir Linux VM uygulamanızı çalıştırma
3. Geliştirme ve Azure Git kullanarak Web siteleri yayımlama

İlk senaryoda Azure PaaS yetenekleri gibi yararlanmak zengin web uygulamaları yazmak sağlar [blob depolama](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [kuyruk depolama](storage/queues/storage-python-how-to-use-queue-storage.md), [tablo depolama](cosmos-db/table-storage-how-to-use-python.md) aracılığıyla vb. Azure REST API'lerini için pythonic sarmalayıcıları. Bunlar aynı Windows, Mac ve Linux üzerinde çalışır.  Bu istemci kitaplıkları, yerel geliştirme makinenizde veya Azure üzerinde çalışan bir Linux VM de kullanabilirsiniz.

VM senaryosu için yalnızca bir Linux VM tercih ettiğiniz (Ubuntu, CentOS, Suse) Başlat ve Çalıştır/şeyleri yönetin.  Örnek olarak, çalıştırdığınız [IPython] [ IPython] REPL/Not Defteri, Windows/Mac/Linux tarayıcınız bir Linux veya IPython altyapısı Azure üzerinde çalışan Windows birden çok işlemci VM üzerine gelin ve makine.

Bir Linux VM ayarlama konusunda daha fazla bilgi için bkz: [çalıştıran bir sanal makine Linux oluşturmak](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Öğreticisi.

Git dağıtımı kullanarak, bir Python web uygulaması geliştirme ve herhangi bir işletim sisteminden bir Azure Web sitesine yayımlayın.  Azure'a deponuza göndermek, bir sanal ortam otomatik olarak oluşturur ve gerekli paketleri pip yükler.

Tüm WSGI uyumlu framework kullanma hakkında daha fazla bilgi için bkz: [yapılandırma Python Azure Web siteleri ile](app-service/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>Ek yazılım ve kaynaklar:
* [Python ReadTheDocs için Azure SDK'sı](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Python GitHub için Azure SDK'sı](https://github.com/Azure/azure-sdk-for-python)
* [Python için resmi Azure örnekleri](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Continuum Analytics Python dağıtımı][Continuum Analytics Python Distribution]
* [Enthought Python dağıtımı][Enthought Python Distribution]
* [ActiveState Python dağıtımı][ActiveState Python Distribution]
* [SciPy - suite bilimsel Python kitaplığı][SciPy - A suite of Scientific Python libraries]
* [NumPy - Python için bir sayı kitaplığı][NumPy - A numerics library for Python]
* [Django proje - bir yetişkin web framework/CMS][Django Project - A mature web framework/CMS]
* [IPython - Python için Gelişmiş bir REPL/dizüstü][IPython - an advanced REPL/Notebook for Python]
* [Github'da Visual Studio için Python araçları][Python Tools for Visual Studio on GitHub]
* [Python Geliştirici Merkezi](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[Visual Studio için Python Araçları]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
