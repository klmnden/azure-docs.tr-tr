---
title: İndirmeyi ve ayıklamayı Azure yığın Geliştirme Seti (ASDK) | Microsoft Docs
description: İndirmeyi ve ayıklamayı Azure yığın Geliştirme Seti (ASDK) açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0cf389c9443a9cff477b884c277d72de27769afc
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
ms.locfileid: "29975884"
---
# <a name="download-and-extract-the-azure-stack-development-kit-asdk"></a>İndirmeyi ve ayıklamayı Azure yığın Geliştirme Seti (ASDK)
Geliştirme Seti ana bilgisayarınız ASDK yüklemek için temel gereksinimleri karşıladığından emin olduktan sonra yüklemek ve Cloudbuilder.vhdx almak için ASDK dağıtım paketi ayıklamak için sonraki adım olacaktır.

## <a name="download-the-asdk"></a>ASDK indirin
1. Yükleme başlamadan önce bilgisayarınızın aşağıdaki önkoşulları karşıladığından emin olun:

  - Bilgisayarda en az 60 Ayrıca işletim sistemi diski boş dört ayrı, aynı mantıksal sabit sürücülerde disk alanı GB olmalıdır.
  - [.NET framework 4.6 (veya sonraki bir sürümünü)](https://aka.ms/r6mkiy) yüklü olması gerekir.

2. [Gidilecek Başlarken sayfa](https://azure.microsoft.com/overview/azure-stack/try/?v=try) burada Azure yığın Geliştirme Seti indirebilir, bilgilerinizi verin ve ardından **gönderme**.
3. İndirme ve çalıştırma [Azure yığın Geliştirme Seti için dağıtım denetleyicisi](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) önkoşul denetleyicisini komut dosyası. Bu tek başına komut dosyası Azure yığın Geliştirme Seti için kurulumu tarafından gerçekleştirilen bir ön koşul denetimleri geçer. Azure yığın Geliştirme Seti için büyük paketini yüklemeden önce donanım ve yazılım gereksinimleri karşılayıp onaylamak için bir yol sağlar.
4. Altında **yazılımı yüklemek**, tıklatın **Azure yığın Geliştirme Seti**.

  > [!NOTE]
  > ASDK yükleme (AzureStackDevelopmentKit.exe) yaklaşık olarak 10 GB'tır.

## <a name="extract-the-asdk"></a>ASDK Ayıkla
1. Yükleme tamamlandıktan sonra **çalıştırmak** ASDK ayıklayıcısı (AzureStackDevelopmentKit.exe) başlatın.
2. Gözden geçirin ve görüntülenen Lisans Sözleşmesi'nden kabul **Lisans Sözleşmesi'ni** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**.
3. Görüntülenen gizlilik bildirimi bilgilerini gözden **önemli duyuru** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**.
4. Göre ayıklanacak Azure yığın kurulum dosyalarının konumunu seçin **hedef konum seçin** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**. Varsayılan konum *geçerli klasörde*\Azure yığın Geliştirme Seti. 
5. Hedef konumun özetini gözden **Extract hazır** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **ayıklamak** CloudBuilder.vhdx (yaklaşık 25 GB) ayıklayın ve ThirdPartyLicenses.rtf dosyaları. Bu işlemin tamamlanması biraz zaman alır.
6. Kopyalama veya CloudBuilder.vhdx dosyayı C:\ sürücüsü (C:\CloudBuilder.vhdx) kök dizinine ASDK ana bilgisayara taşıyın.

> [!NOTE]
> Dosyaları ayıkla sonra silebilirsiniz. EXE ve. Sabit disk alanını kurtarmak için DEPO dosyaları. Veya, böylece ASDK dağıtmanız gerekiyorsa dosyaları yeniden karşıdan yüklemeniz gerekmez, bu dosyaları yedekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
[ASDK ana bilgisayarı hazırla](asdk-prepare-host.md)