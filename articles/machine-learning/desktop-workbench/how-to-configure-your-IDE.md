---
title: Azure Machine Learning Workbench bir IDE ile çalışacak şekilde yapılandırmak nasıl?  | Microsoft Docs
description: Azure Machine Learning Workbench, IDE ile çalışacak şekilde yapılandırmak için bir kılavuz.
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 8158d6faee5ec4d28f0c7e16963fc3d78392b857
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978973"
---
# <a name="how-to-configure-azure-machine-learning-workbench-to-work-with-an-ide"></a>Azure Machine Learning Workbench bir IDE ile çalışacak şekilde yapılandırma 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Azure Machine Learning Workbench, popüler Python IDE (tümleşik geliştirme ortamı) ile çalışacak şekilde yapılandırılabilir. Bu veri hazırlama, kod yazma, çalıştırma, izleme ve kullanıma hazır hale getirme arasında taşıma sorunsuz veri bilimi geliştirme deneyimi sağlar. Şu anda desteklenen IDE'ler şunlardır:
- Microsoft Visual Studio kodu 
- JetBrain PyCharm 

## <a name="configure-workbench"></a>Workbench yapılandırın
1. Tıklayarak **dosya** üst menü sol üst köşedeki uygulama. 
2. Seçin **yapılandırma proje IDE** seçeneğinden açılır öğesi 
3. Yazın `VS Code` veya `PyCharm` içinde **adı** (adı: isteğe bağlı) alanı
4. IDE yürütülebilir dosya (yürütülebilir adını ve uzantısını tam) için konumu girin **yürütme yolu**

### <a name="default-install-path-for-visual-studio-code"></a>Visual Studio Code için varsayılan yükleme yolu  

* Windows 32-bit- `C:\Program Files (x86)\Microsoft VS Code\Code.exe`
* Windows 64-bit- `C:\Program Files\Microsoft VS Code\Code.exe`
* macOS - seçin .app yol, örneğin `/Applications/Visual Studio Code.app`, ve yolun geri kalanı, uygulama ekler. Varsayılan olarak yürütülebilir dosyanın tam yolu `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code`. Çalıştırıldığında `Shell Command: Install 'code' command in PATH` VS Code komut de başvurabilirsiniz sonra VS Code'da komutu `/usr/local/bin/code`

### <a name="default-install-path-for-pycharm"></a>Varsayılan yükleme yolunu PyCharm 

* Windows 32-bit - `C:\Program Files (x86)\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm.exe`. 
* Windows 64-bit - `C:\Program Files\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm64.exe`.
* macOS - örneğin "/ Applications/PyCharm CE.app".app yolunu seçin ve yolun geri kalanı, uygulama ekler. Varsayılan olarak yürütülebilir dosyanın tam yolu `/Applications/PyCharm CE.app/Contents/MacOS/pycharm`. Ayrıca, PyCharm bin klasöründe bulabilirsiniz, `/usr/local/bin/charm`

## <a name="open-project-in-ide"></a>IDE projeyi açma 
Yapılandırma tamamlandıktan sonra bir Azure Machine Learning projesi açarak açabilirsiniz **dosya** Azure Machine Learning Workbench, menüde ardından **Proje Aç (< IDE_Name >)**. Bu eylem geçerli etkin proje yapılandırılmış IDE içinde açılır. _Not: bir projede değilse **Proje Aç (< IDE_Name >)** devre dışı bırakılır._

## <a name="configuring-the-integrated-terminal-in-visual-studio-code"></a>Visual Studio Code'da tümleşik Terminalini yapılandırma

### <a name="windows"></a>Windows 
Biz yerine PowerShell cmd olmasını varsayılan kabuğunu silmiş. Tıklayarak üzerinde **Proje Aç (< IDE_Name >)**, bir istem göreceksiniz: 

_Kabuk izin: `C:\windows\System32\cmd.exe` (bir çalışma ayarı olarak tanımlanır) terminalde başlatılacak?_

Yanıt `yes` Kabuk Azure ML Workbench komut satırı arabirimi ile sorunsuz çalışacak şekilde yapılandırma izin vermek için.

### <a name="mac"></a>Mac
Çalıştırılacak bir `az` komutu kullanarak Visual Studio Code'nın tümleşik terminal Mac üzerinde el ile ayarlamanız gerekir `PATH` aynı değere sahip için `PATH` projenin `.vscode/settings.json` anahtarı altındaki dosya `terminal.integrated.env.osx`. Terminalde aşağıdaki komutu çalıştırarak bunu yapabilirsiniz: `PATH=<PATH in .vscode/settings>`
