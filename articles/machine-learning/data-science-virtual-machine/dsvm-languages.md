---
title: Azure veri bilimi sanal makine için dilleri | Microsoft Docs
description: Azure veri bilimi sanal makine için dilleri
keywords: Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 411729155f5135c7e45588b69995274c9cac1315
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31418324"
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinede desteklenen diller 

Veri bilimi sanal makine (DSVM) birkaç önceden derlenmiş diller ve AI uygulamalarınızı oluşturmak için geliştirme araçları ile birlikte gelir. Belirgin olanları bazıları aşağıda verilmiştir. 

## <a name="python-windows-server-2016-edition"></a>Python (Windows Server 2016 Edition)

|    |           |
| ------------- | ------------- |
| Dil sürümleri destekleniyor | 2.7 ve 3.6 |
| Desteklenen DSVM sürümleri      | Windows Server 2016     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | İki genel `conda` ortamları oluşturulur. <br /> * `root` ortamında bulunan `/anaconda/` Python 3.6 değil. <br/> * `python2` ortamında bulunan `/anaconda/envs/python2`Python 2.7 olduğu       |
| Örnekleri bağlantılar      | Python için örnek Jupyter not defterleri dahil edilir     |
| DSVM ilgili araçları      | PySpark, R, Jale      |

> [!NOTE]
> Windows Server 2016 Mart 2018 önce oluşturulan Python 3.5 ve Python 2.7 içerir. Aynı zamanda Python 2.7 conda olan **kök** ortamı ve **py35** Python 3.5 ortamıdır. 

### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?    

* Komut isteminde çalışmasını

Komut istemi açın ve Python çalıştırmak istediğiniz sürümüne bağlı olarak aşağıdakileri yapın. 

```
# To run Python 2.7
activate python2
python --version

# To run Python 3.6
activate 
python --version

```
* IDE içinde kullanma

Python araçları için Visual Studio (Visual Studio Community edition yüklü PTVS) kullanın. Python 3.6 PTVS varsayılan olarak otomatik olarak yalnızca ortam kurulur. 

> [!NOTE]
> Python 2.7 PTVS işaret etmek için özel bir ortam içinde PTVS oluşturmanız gerekir. Visual Studio Community Edition bu ortam yolları ayarlamak için gidin **Araçları** -> **Python Araçları** -> **Python ortamları** ve ardından **+ özel**. Konum ayarlamak `c:\anaconda\envs\python2` ve ardından _otomatik algıla_. 

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebileceğiniz _Python [Conda kök]_ Python 3.6 için ve _Python [Conda env:python2]_ Python 2.7 ortamı için. 

* Python paketlerini yükleme

Varsayılan Python ortamları DSVM üzerinde genel ortam okunabilir ve tüm kullanıcılar ' dir. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için kök veya python2 Ortamı'nı kullanarak etkinleştirin `activate` yönetici olarak komutu. Paket Yöneticisi gibi kullanabilirsiniz `conda` veya `pip` yükleme veya güncelleştirme paketleri. 

## <a name="python-linux-and-windows-server-2012-edition"></a>Python (Linux ve Windows Server 2012 Edition)

|    |           |
| ------------- | ------------- |
| Dil sürümleri destekleniyor | 2.7 ve 3.5 |
| Desteklenen DSVM sürümleri      | Linux, Windows Server 2012    |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | İki genel `conda` ortamları oluşturulur. <br /> * `root` ortamında bulunan `/anaconda/` Python 2.7 değil. <br/> * `py35` ortamında bulunan `/anaconda/envs/py35`Python 3.5       |
| Örnekleri bağlantılar      | Python için örnek Jupyter not defterleri dahil edilir     |
| DSVM ilgili araçları      | PySpark, R, Jale      |
### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?    

**Linux**
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

**Windows 2012**
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

## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Dil sürümleri destekleniyor | Microsoft R açık 3.x (% 100 CRAN R ile uyumlu<br /> Microsoft R Server 9.x Geliştirici sürümü (A ölçeklenebilir kuruluş hazır R platformu)|
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Windows: `C:\Program Files\Microsoft\ML Server\R_SERVER` <br />Linux: ` /usr/lib64/microsoft-r/3.3/lib64/R`    |
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
| Dil sürümleri destekleniyor | 0,6 |
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Windows: yüklü `C:\JuliaPro-VERSION`<br /> Linux: yüklü `/opt/JuliaPro-VERSION`    |
| Örnekleri bağlantılar      | Jale için örnek Jupyter not defterleri dahil edilir     |
| DSVM ilgili araçları      | Python, R      |
### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?    

**Windows**:

* Komut isteminde çalışmasını

Komut istemi açın ve yalnızca çalıştırın `julia`. 
* IDE içinde kullanma

Kullanım `Juno` Jale IDE DSVM yüklü ve masaüstü kısayolu olarak kullanılabilir.

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebilirsiniz. `Julia VERSION` 

* Jale paketleri yükleniyor

Jale konumu tüm kullanıcılar tarafından okunabilir ortam bir genel varsayılandır. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak Jale çalıştırın. Jale Paket Yöneticisi komutları gibi çalıştırın. ardından, `Pkg.add()` yükleme veya güncelleştirme paketleri. 


**Linux**:
* Terminal çalışır.

Açık terminal ve yalnızca çalışma `julia`. 
* IDE içinde kullanma

Kullanım `Juno` Jale IDE DSVM yüklü ve uygulama menü kısayolu olarak kullanılabilir.

* Jupyter'de kullanma

Jupyter açın ve tıklayın `New` düğmesi yeni bir not defteri oluşturun. Bu noktada, çekirdek türü olarak seçebilirsiniz. `Julia VERSION` 

* Jale paketleri yükleniyor

Jale konumu tüm kullanıcılar tarafından okunabilir ortam bir genel varsayılandır. Ancak yalnızca Yöneticiler yazma / genel paketlerini yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak Jale çalıştırın. Jale Paket Yöneticisi komutları gibi çalıştırın. ardından, `Pkg.add()` yükleme veya güncelleştirme paketleri. 

## <a name="other-languages"></a>Diğer diller

**C#**: Windows kullanılabilir ve veya Visual Studio Community edition üzerinden erişilebilir bir `Developer Command Prompt for Visual Studio` yalnızca çalıştırabileceğiniz `csc` komutu. 

**Java**: OpenJDK DSVM ve yolun kümesinde hem Linux hem de Windows Edition'da kullanılabilir. Yazabilirsiniz `javac` veya `java` Windows komut isteminde veya Java kullanma Linux bash kabuğunda komutu. 

**node.js**: node.js DSVM ve yolun kümesinde hem Linux hem de Windows Edition'da kullanılabilir. Yazabilirsiniz `node` veya `npm` Windows komut isteminde veya node.js erişmek için Linux bash kabuğunda komutu. Windows, Visual Studio uzantısı için Node.js araçları node.js uygulamanızı geliştirmek için bir grafik IDE sağlamak için yüklenir. 

**F #**: Windows kullanılabilir ve veya Visual Studio Community edition üzerinden erişilebilir bir `Developer Command Prompt for Visual Studio` yalnızca çalıştırabileceğiniz `fsc` komutu. 


