---
title: "Bir IDE ile çalışmak için Azure Machine Learning çalışma ilkesi nasıl yapılandırılır?  | Microsoft Belgeleri"
description: "IDE ile çalışmak için Azure Machine Learning çalışma yapılandırma için bir kılavuz."
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/05/2017
ms.openlocfilehash: 4e18a413a0559b1ddebecf1b29722d21ef35c337
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-azure-machine-learning-workbench-to-work-with-an-ide"></a>Azure Machine Learning çalışma'ekranı bir IDE ile çalışmak için yapılandırma 

Azure Machine Learning çalışma ekranı popüler Python IDE (tümleşik geliştirme ortamı) ile çalışmak için yapılandırılabilir. Veri hazırlama, kod yazma, çalıştırma izleme ve operationalization arasında taşıma bir kesintisiz veri bilimi geliştirme deneyimi sağlar. Şu anda desteklenen IDE şunlardır:
- Microsoft Visual Studio Code 
- JetBrain PyCharm 

## <a name="configure-workbench"></a>Çalışma ekranı yapılandırın
1. Tıklayın **dosya** üst menüde sol köşe uygulamanın. 
2. Seçin **yapılandırma proje IDE** çıkma seçeneği 
3. Yazın `VS Code` veya `PyCharm` içinde **adı** (adı olan rastgele) alanı
4. IDE yürütülebilir dosya (yürütülebilir dosya adı ve uzantısı ile tamamlanan) için konumu girin **yürütme yolu**

### <a name="default-install-path-for-visual-studio-code"></a>Visual Studio Code için varsayılan yükleme yolu  

* Windows 32-bit-`C:\Program Files (x86)\Microsoft VS Code\Code.exe`
* Windows 64-bit-`C:\Program Files\Microsoft VS Code\Code.exe`
* macOS - .app yol örneğin seçin `/Applications/Visual Studio Code.app`, ve yolun geri kalanı, uygulama ekler. Varsayılan yürütülebilir dosyanın tam yolu `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code`. Yürütülürse `Shell Command: Install 'code' command in PATH` VS Code komut başvurusu yapabilir sonra VS Code'da komutu`/usr/local/bin/code`

### <a name="default-install-path-for-pycharm"></a>PyCharm için varsayılan yükleme yolu 

* Windows 32-bit - `C:\Program Files (x86)\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm.exe`. 
* Windows 64-bit - `C:\Program Files\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm64.exe`.
* macOS - örneğin "/ uygulamalar/PyCharm CE.app".app yol seçin ve yolun geri kalanı, uygulama ekler. Varsayılan yürütülebilir dosyanın tam yolu `/Applications/PyCharm CE.app/Contents/MacOS/pycharm`. Ayrıca, bin klasörü adresindeki PyCharm bulabilirsiniz,`/usr/local/bin/charm`

## <a name="open-project-in-ide"></a>IDE içinde bir açık projeye 
Yapılandırma tamamlandıktan sonra Azure Machine Learning projesinde açarak açabilirsiniz **dosya** Azure Machine Learning çalışma ekranı, menüde ardından **Proje Aç (< IDE_Name >)**. Bu eylem geçerli etkin proje yapılandırılmış IDE'de açar. _Not: bir projede değilse **Proje Aç (< IDE_Name >)** devre dışı bırakılır._

## <a name="configuring-the-integrated-terminal-in-visual-studio-code"></a>Visual Studio kodda tümleşik terminal yapılandırma

### <a name="windows"></a>Windows 
Biz yerine PowerShell cmd olmasını varsayılan kabuğunu geçersiz. Tıklayarak üzerinde **Proje Aç (< IDE_Name >)**, bir uyarı görürsünüz: 

_Kabuk izin verme: `C:\windows\System32\cmd.exe` (çalışma ayarı olarak tanımlanan) terminale başlatılacak?_

Yanıt `yes` Azure ML çalışma ekranı komut satırı arabirimi ile sorunsuz çalışması için kabuk yapılandırma izin vermek için.

### <a name="mac"></a>Mac
Çalıştırmak için bir `az` Visual Studio kodun kullanarak komutu tümleşik Mac terminalde, el ile ayarlamanız gerekir `PATH` aynı değere olmasını `PATH` projenin `.vscode/settings.json` anahtarı altındaki dosya `terminal.integrated.env.osx`. Terminale aşağıdaki komutu çalıştırarak bunu yapabilirsiniz:`PATH=<PATH in .vscode/settings>`
