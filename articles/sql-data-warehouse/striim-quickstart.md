---
title: Azure SQL veri ambarı ile Striim hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde Striim ve Azure SQL veri ambarı ile çalışmaya başlayın.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 8ed9936884a648d736942caecade2ac3c2980e67
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873405"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Striim Azure SQL DW Market teklifi Yükleme Kılavuzu

Bu hızlı başlangıçta, önceden mevcut olan bir SQL veri ambarı örneğini zaten sahip olduğunuzu varsayar.

Azure Marketi'nde Striim arayın ve Striim veri tümleştirme için SQL veri ambarı'nı (aşamalı) seçeneğini belirleyin. 

![Striim yükleyin][install]

Belirtilen özelliklerle Striim küme adı, parola ve yönetici parolası belirtmeye Striim VM yapılandırma

![Striim yapılandırın][configure]

Uygulama dağıtıldıktan sonra tıklayarak \<VM adı >-Azure portalında masternode Bağlan'a tıklayın ve VM yerel hesabı kullanarak oturum açma kopyalayın 

![Striim SQL Data Warehouse'a bağlanma][connect]

Gelen sqljdbc42.jar indirme <https://www.microsoft.com/en-us/download/details.aspx?id=54671> yerel makinenize. 

Bir komut satırı penceresi açın ve dizinleri JDBC jar dosyasını indirdiğiniz konuma değiştirin. SCP jar dosyasını Striim vm'nize, Azure portalından alma adresi ve parola

![Sanal makinenizde jar dosyasını kopyalamanız][copy-jar]

Başka bir komut satırı penceresi açın ve kullanmak bir ssh ssh Striim küme için yardımcı programı

![Kümenin içine SSH][ssh]

JDBC jar dosyasını Striim'ın LIB dizinine taşımak, başlangıç ve sunucuyu durdurmak için aşağıdaki komutları yürütün.

   1. sudo su
   2. CD/tmp
   3. mv sqljdbc42.jar /opt/striim/lib
   4. systemctl durdurma striim düğümü
   5. systemctl durdurma striim dbms
   6. systemctl başlangıç striim dbms
   7. systemctl başlangıç striim düğümü

![Striim kümeyi başlatın][start-striim]

Şimdi, sık kullandığınız tarayıcıyı açın ve gidin \<DNS adı >: 9080

![Oturum açma ekranına gidin][navigate]

Kullanıcı adı ve Azure portalında ayarlama ve kullanmaya başlayın ya da sürükle kullanmaya başlayın ve bırak kullanıcı Arabirimi için uygulamalar sayfasına gidin, tercih edilen Sihirbazı parolayla oturum açın

![Sunucu kimlik bilgileriyle oturum açın][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
