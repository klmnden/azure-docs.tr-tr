---
title: "Azure veri bilimi sanal makine için dilleri | Microsoft Docs"
description: "Azure veri bilimi sanal makine için dilleri"
keywords: "Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 2f2125e739b738847e03ce429d65801969611685
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinede desteklenen diller 

Veri bilimi sanal makine (DSVM) birkaç önceden derlenmiş diller ve AI uygulamalarınızı oluşturmak için geliştirme araçları ile birlikte gelir. Belirgin olanları bazıları aşağıda verilmiştir. 

## <a name="python"></a>Python

|    |           |
| ------------- | ------------- |
| Dil sürümleri destekleniyor | 2.7 ve 3.5 |
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | İki genel `conda` ortamları oluşturulur. <br /> * `root`ortamında bulunan `/anaconda/` Python 2.7 değil. <br/> * `py35`ortamında bulunan `/anaconda/envs/py35`Python 3.5       |
| Örnekleri bağlantılar      | Python için örnek Jupyter not defterleri dahil edilir     |
| DSVM ilgili araçları      | PySpark, R, Jale      |
### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?    

**Windows**:

* Komut isteminde çalışmasını

Komut istemi açın ve Python çalıştırmak istediğiniz sürümüne bağlı olarak aşağıdakileri yapın. 

```
# To run Python 2.7
activate 
python --version

# To run Python 3.5
activate py35
python --version

```
* IDE içinde kullanma

Python araçları için Visual Studio (Visual Studio Community edition yüklü PTVS) kullanın. Python 2.7 PTVS otomatik olarak yalnızca ortamı kurulumu. 
> [!NOTE]
> PTVS Python 3.5 işaret etmek için özel bir ortam içinde PTVS oluşturmanız gerekir. Visual Studio Community Edition bu ortam yolları ayarlamak için gidin **Araçları** -> **Python Araçları** -> **Python ortamları** ve ardından **+ özel**. Konum ayarlamak `c:\anaconda\envs\py35` ve ardından _otomatik algıla_. 

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebileceğiniz _Python [Conda kök]_ Python 2.7 için ve _Python [Conda env:py35]_ Python 3.5 ortamı için. 

* Python paketlerini yükleme

Varsayılan Python ortamları DSVM üzerinde genel ortam okunabilir ve tüm kullanıcılar ' dir. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için kök veya py35 Ortamı'nı kullanarak etkinleştirin `activate` yönetici olarak komutu. Paket Yöneticisi gibi kullanabilirsiniz `conda` veya `pip` yükleme veya güncelleştirme paketleri. 


**Linux**:

* Terminal çalışır

Terminali açın ve Python çalıştırmak istediğiniz sürümüne bağlı olarak aşağıdakileri yapın. 

```
# To run Python 2.7
source activate 
python --version

# To run Python 3.5
source activate py35
python --version

```
* IDE içinde kullanma

Visual Studio Community edition yüklü PyCharm kullanın. 

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebileceğiniz _Python [Conda kök]_ Python 2.7 için ve _Python [Conda env:py35]_ Python 3.5 ortamı için. 

* Python paketlerini yükleme

Varsayılan Python ortamları DSVM üzerinde genel ortamları okunabilir ve tüm kullanıcılar ' dir. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için kök veya py35 Ortamı'nı kullanarak etkinleştirin `source activate` bir yönetici veya sudo izni olan bir kullanıcı olarak komutu. Paket Yöneticisi gibi kullanabilirsiniz `conda` veya `pip` yükleme veya güncelleştirme paketleri. 

## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Dil sürümleri destekleniyor | Microsoft R açık 3.x (% 100 CRAN R ile uyumlu<br /> Microsoft R Server 9.x Geliştirici sürümü (A ölçeklenebilir kuruluş hazır R platformu)|
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Windows:`C:\Program Files\Microsoft\R Server\R_SERVER` <br />Linux:` /usr/lib64/microsoft-r/3.3/lib64/R`    |
| Örnekleri bağlantılar      | R örnek Jupyter not defterleri dahil edilir     |
| DSVM ilgili araçları      | SparkR, Python, Jale      |
### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?    

**Windows**:

* Komut isteminde çalışmasını

Komut istemi açın ve yalnızca yazın `R`.

* IDE içinde kullanma

Kullanım RTools için Visual Studio (Visual Studio Community edition veya Rstudio'dan yüklü RTVS). Bunlar, Başlat menüsünde veya Masaüstü simgesi olarak kullanılabilir. 

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebileceğiniz _R_ Jupyter R çekirdek (IRKernel) kullanmak için. 

* R paketlerini yükleme

R tüm kullanıcılar tarafından okunabilir genel ortamında DSVM yüklenir. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak R çalıştırın. R paketi Yöneticisi'ni çalıştırın. ardından, `install.packages()` yükleme veya güncelleştirme paketleri. 

**Linux**:

* Terminal çalışır

Açık terminal ve yalnızca çalışma `R`.  

* IDE içinde kullanma

Linux DSVM üzerinde yüklü Rstudio'dan kullanın.  

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebileceğiniz _R_ Jupyter R çekirdek (IRKernel) kullanmak için. 

* R paketlerini yükleme

R tüm kullanıcılar tarafından okunabilir genel ortamında DSVM yüklenir. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak R çalıştırın. R paketi Yöneticisi'ni çalıştırın. ardından, `install.packages()` yükleme veya güncelleştirme paketleri. 


## <a name="julia"></a>Jale

|    |           |
| ------------- | ------------- |
| Dil sürümleri destekleniyor | 0.5 |
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Windows: yüklü`C:\JuliaPro-VERSION`<br /> Linux: yüklü`/opt/JuliaPro-VERSION`    |
| Örnekleri bağlantılar      | Jale için örnek Jupyter not defterleri dahil edilir     |
| DSVM ilgili araçları      | Python, R      |
### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?    

**Windows**:

* Komut isteminde çalışmasını

Komut istemi açın ve yalnızca çalıştırın `julia`. 
* IDE içinde kullanma

Kullanım `Juno` Jale IDE DSVM yüklü ve masaüstü kısayolu olarak kullanılabilir.

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebilirsiniz.`Julia VERSION` 

* Jale paketleri yükleniyor

Jale konumu tüm kullanıcılar tarafından okunabilir ortam bir genel varsayılandır. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak Jale çalıştırın. Jale Paket Yöneticisi komutları gibi çalıştırın. ardından, `Pkg.add()` yükleme veya güncelleştirme paketleri. 


**Linux**:
* Terminal çalışır.

Açık terminal ve yalnızca çalışma `julia`. 
* IDE içinde kullanma

Kullanım `Juno` Jale IDE DSVM yüklü ve uygulama menü kısayolu olarak kullanılabilir.

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebilirsiniz.`Julia VERSION` 

* Jale paketleri yükleniyor

Jale konumu tüm kullanıcılar tarafından okunabilir ortam bir genel varsayılandır. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak Jale çalıştırın. Jale Paket Yöneticisi komutları gibi çalıştırın. ardından, `Pkg.add()` yükleme veya güncelleştirme paketleri. 

## <a name="other-languages"></a>Diğer diller

**C#**: Windows kullanılabilir ve veya Visual Studio Community edition üzerinden erişilebilir bir `Developer Command Prompt for Visual Studio` yalnızca çalıştırabileceğiniz `csc` komutu. 

**Java**: OpenJDK DSVM ve yolun kümesinde hem Linux hem de Windows Edition'da kullanılabilir. Yazabilirsiniz `javac` veya `java` Windows komut isteminde veya Java kullanma Linux bash kabuğunda komutu. 

**node.js**: node.js DSVM ve yolun kümesinde hem Linux hem de Windows Edition'da kullanılabilir. Yazabilirsiniz `node` veya `npm` Windows komut isteminde veya node.js erişmek için Linux bash kabuğunda komutu. Windows, Visual Studio uzantısı için Node.js araçları node.js uygulamanızı geliştirmek için bir grafik IDE sağlamak için yüklenir. 

**F #**: Windows kullanılabilir ve veya Visual Studio Community edition üzerinden erişilebilir bir `Developer Command Prompt for Visual Studio` yalnızca çalıştırabileceğiniz `fsc` komutu. 


