---
title: Desteklenen diller için veri bilimi sanal makinesi
titleSuffix: Azure
description: Program dil ve veri bilimi sanal makinesi üzerinde önceden yüklenmiş olan ilgili araçlar hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 586f37ff972a6102da351794365f719a185857fc
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877423"
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi üzerinde desteklenen diller 

Veri bilimi sanal makinesi (DSVM), birkaç önceden oluşturulmuş diller ve, yapay ZEKA uygulamaları oluşturmaya yönelik geliştirme araçları ile birlikte gelir. Dikkat çekici olanları bazıları aşağıda verilmiştir. 

## <a name="python-windows-server-2016-edition"></a>Python (Windows Server 2016 sürümü)

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | 2.7 ve 3.6 |
| Desteklenen DSVM sürümleri      | Windows Server 2016     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | İki genel `conda` ortamları oluşturulur. <br /> * `root` ortam konumu `/anaconda/` Python 3.6 olduğu. <br/> * `python2` ortam konumu `/anaconda/envs/python2`Python 2.7 olduğu       |
| Örneklere bağlantılar      | Python örnek Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | PySpark, R, Julia      |

> [!NOTE]
> Windows Server 2016'ın Mart 2018'den önce oluşturulan Python 3.5 ve Python 2.7 içerir. Python 2.7 conda Ayrıca, **kök** ortam ve **py35** Python 3.5 ortamıdır. 

### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?    

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
> Python 2.7 PTVS işaret etmek için PTVS içinde özel bir ortam oluşturmanız gerekir. Visual Studio Community Edition bu ortam yollarını ayarlamak için gidin **Araçları** -> **Python Araçları** -> **Python ortamları** ve ardından **+ özel**. Ardından konumu kümesine `c:\anaconda\envs\python2` ve ardından _otomatik algıla_. 

* Jupyter'de kullanma

Jupyter açın ve tıklayarak `New` yeni bir not defteri oluşturmak için. Bu noktada, çekirdek türü olarak seçebileceğiniz _Python [Conda kök]_ için Python 3.6 ve _Python [Conda env:python2]_ Python 2.7 ortamı için. 

* Python paketlerini yükleme

DSVM üzerinde varsayılan Python ortamları, genel ortam okunabilir ve tüm kullanıcılar ' dir. Ancak yalnızca Yöneticiler yazma / genel paketleri yükleyin. Genel ortama paket yüklemek için kök veya python2 ortamı kullanarak etkinleştirin `activate` yönetici olarak komutu. Paket Yöneticisi gibi kullanabilirsiniz `conda` veya `pip` yükleme veya güncelleştirme paketleri. 

## <a name="python-linux-and-windows-server-2012-edition"></a>Python (Linux ve Windows Server 2012 sürümü)

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | 2.7 ve 3.5 |
| Desteklenen DSVM sürümleri      | Linux, Windows Server 2012    |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | İki genel `conda` ortamları oluşturulur. <br /> * `root` ortam konumu `/anaconda/` olan Python 2.7. <br/> * `py35` ortam konumu `/anaconda/envs/py35`Python 3.5       |
| Örneklere bağlantılar      | Python örnek Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | PySpark, R, Julia      |
### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?    

**Linux**
* Terminal içinde çalışan

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

Jupyter açın ve tıklayarak `New` yeni bir not defteri oluşturmak için. Bu noktada, çekirdek türü olarak seçebileceğiniz _Python [Conda kök]_ Python 2.7 için ve _Python [Conda env:py35]_ Python 3.5 ortamı için. 

* Python paketlerini yükleme

DSVM üzerinde varsayılan Python ortamları, genel ortamları okunabilir ve tüm kullanıcılar ' dir. Ancak yalnızca Yöneticiler yazma / genel paketleri yükleyin. Genel ortama paket yüklemek için kök veya py35 ortamı kullanarak etkinleştirin `source activate` yönetici veya sudo izni olan bir kullanıcı olarak komutu. Bir paket Yöneticisi gibi kullanabilirsiniz `conda` veya `pip` yükleme veya güncelleştirme paketleri. 

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

Python araçları için Visual Studio (Visual Studio Community edition yüklü PTVS) kullanın. Python 2.7, PTVS otomatik olarak yalnızca ortam kurulumu. 
> [!NOTE]
> Python 3.5 PTVS işaret etmek için PTVS içinde özel bir ortam oluşturmanız gerekir. Visual Studio Community Edition bu ortam yollarını ayarlamak için gidin **Araçları** -> **Python Araçları** -> **Python ortamları** ve ardından **+ özel**. Ardından konumu kümesine `c:\anaconda\envs\py35` ve ardından _otomatik algıla_. 

* Jupyter'de kullanma

Jupyter açın ve tıklayarak `New` yeni bir not defteri oluşturmak için. Bu noktada, çekirdek türü olarak seçebileceğiniz _Python [Conda kök]_ Python 2.7 için ve _Python [Conda env:py35]_ Python 3.5 ortamı için. 

* Python paketlerini yükleme

DSVM üzerinde varsayılan Python ortamları, genel ortam okunabilir ve tüm kullanıcılar ' dir. Ancak yalnızca Yöneticiler yazma / genel paketleri yükleyin. Genel ortama paket yüklemek için kök veya py35 ortamı kullanarak etkinleştirin `activate` yönetici olarak komutu. Paket Yöneticisi gibi kullanabilirsiniz `conda` veya `pip` yükleme veya güncelleştirme paketleri. 

## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | Microsoft R Open 3.x (% 100'ün üzerinde CRAN R ile uyumlu<br /> Microsoft R Server 9.x Developer edition (bir ölçeklenebilir Kurumsal hazır R platform)|
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Windows: `C:\Program Files\Microsoft\ML Server\R_SERVER` <br />Linux: `/usr/lib64/microsoft-r/3.3/lib64/R`    |
| Örneklere bağlantılar      | Örnek R Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | SparkR, Python, Julia      |
### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?    

**Windows**:

* Komut isteminde çalışmasını

Komut istemi açın ve yazmanız yeterlidir `R`.

* IDE içinde kullanma

Kullanım RTools için Visual Studio (Visual Studio Community sürümü veya RStudio yüklü RTVS). Bunlar, Başlat menüsünde veya Masaüstü simgesi olarak kullanılabilir. 

* Jupyter'de kullanma

Jupyter açın ve tıklayarak `New` yeni bir not defteri oluşturmak için. Bu noktada, çekirdek türü olarak seçebileceğiniz _R_ Jupyter R çekirdek (IRKernel) kullanılacak. 

* R paketlerini yükleme

R tüm kullanıcılar tarafından okunabilir bir genel ortamında DSVM yüklenir. Ancak yalnızca Yöneticiler yazma / genel paketleri yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak R çalıştırın. R paketi Yöneticisi'ni çalıştırabilirsiniz `install.packages()` yükleme veya güncelleştirme paketleri. 

**Linux**:

* Terminal içinde çalışan

Açık terminal ve sadece çalışma `R`.  

* IDE içinde kullanma

Linux DSVM'sini üzerinde yüklü RStudio kullanın.  

* Jupyter'de kullanma

Jupyter açın ve tıklayarak `New` yeni bir not defteri oluşturmak için. Bu noktada, çekirdek türü olarak seçebileceğiniz _R_ Jupyter R çekirdek (IRKernel) kullanılacak. 

* R paketlerini yükleme

R tüm kullanıcılar tarafından okunabilir bir genel ortamında DSVM yüklenir. Ancak yalnızca Yöneticiler yazma / genel paketleri yükleyin. Genel ortama paket yüklemek için yukarıdaki yöntemlerden birini kullanarak R çalıştırın. R paketi Yöneticisi'ni çalıştırabilirsiniz `install.packages()` yükleme veya güncelleştirme paketleri. 


## <a name="julia"></a>Julia

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | 0,6 |
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Windows: Yüklü `C:\JuliaPro-VERSION`<br /> Linux: Yüklü `/opt/JuliaPro-VERSION`    |
| Örneklere bağlantılar      | Julia örnek Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | Python, R      |
### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?    

**Windows**:

* Komut isteminde çalışmasını

Komut istemi açın ve yalnızca `julia`. 
* IDE içinde kullanma

Kullanım `Juno` DSVM yüklü ve bir masaüstü kısayolu Julia IDE.

* Jupyter'de kullanma

Jupyter açın ve tıklayarak `New` yeni bir not defteri oluşturmak için. Bu noktada, çekirdek türü olarak seçebilirsiniz. `Julia VERSION` 

* Julia paketlerini yükleme

Julia konumu tüm kullanıcılar tarafından okunabilir ortam bir genel varsayılandır. Ancak yalnızca Yöneticiler yazma / genel paketleri yükleyin. Genel ortama paket yüklemek için Julia yukarıdaki yöntemlerden birini kullanarak çalıştırın. Julia'nın Paket Yöneticisi gibi komutlar çalıştırırsanız `Pkg.add()` yükleme veya güncelleştirme paketleri. 


**Linux**:
* Terminalde çalışıyor.

Açık terminal ve sadece çalışma `julia`. 
* IDE içinde kullanma

Kullanım `Juno` Julia IDE DSVM yüklü ve bir uygulama menüsü kısayol olarak kullanılabilir.

* Jupyter'de kullanma

Jupyter açın ve tıklayarak `New` yeni bir not defteri oluşturmak için. Bu noktada, çekirdek türü olarak seçebilirsiniz. `Julia VERSION` 

* Julia paketlerini yükleme

Julia konumu tüm kullanıcılar tarafından okunabilir ortam bir genel varsayılandır. Ancak yalnızca Yöneticiler yazma / genel paketleri yükleyin. Genel ortama paket yüklemek için Julia yukarıdaki yöntemlerden birini kullanarak çalıştırın. Julia'nın Paket Yöneticisi gibi komutlar çalıştırırsanız `Pkg.add()` yükleme veya güncelleştirme paketleri. 

## <a name="other-languages"></a>Diğer diller

**C#**: Windows üzerinde kullanılabilen ve erişilebilen veya Visual Studio Community sürümü üzerinden bir `Developer Command Prompt for Visual Studio` az önce çalıştırdığınız `csc` komutu. 

**Java**: OpenJDK hem Linux hem de Windows sürümü DSVM ve yolun kümesinde kullanılabilir. Yazabilirsiniz `javac` veya `java` Windows komut istemi veya bash Kabuğu'nda Linux Java kullanma komutu. 

**node.js**: node.js DSVM ve yolun kümesinde hem Linux hem de Windows Edition'da kullanılabilir. Yazabilirsiniz `node` veya `npm` Windows komut istemi veya bash Kabuğu'nda node.js erişmek için Linux komutu. Windows üzerinde Visual Studio uzantısı için Node.js araçları, node.js uygulaması geliştirmeye yönelik bir grafik IDE sağlamak için yüklenir. 

**F#**: Windows üzerinde kullanılabilen ve erişilebilen veya Visual Studio Community sürümü üzerinden bir `Developer Command Prompt for Visual Studio` az önce çalıştırdığınız `fsc` komutu. 


