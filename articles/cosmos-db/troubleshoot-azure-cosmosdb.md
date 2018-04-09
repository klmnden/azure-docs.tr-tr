---
title: Azure Depolama Gezgini’nde Azure Cosmos DB’yi Yönetme
description: Azure Depolama Gezgini’nde Azure Cosmos DB’yi yönetmeyi öğrenin.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: ''
author: Jejiang
manager: omafnan
editor: ''
tags: Azure Cosmos DB
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2018
ms.author: jejiang
ms.openlocfilehash: c593eb2b49d15d20a26ef735d1032d5dea45f125
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cosmos-db-in-storage-explorer-troubleshooting-guide-overview"></a>Depolama Gezgini’ndeki Azure Cosmos DB ile ilgili sorun giderme kılavuzuna genel bakış

[Azure Depolama Gezgini’ndeki Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer), Windows, macOS veya Linux’tan Sovereign Clouds ve Azure’da barındırılan Azure Cosmos DB hesaplarına bağlanmanıza olanak sağlayan tek başına bir uygulamadır. Azure Cosmos DB varlıklarını yönetmenize, verileri işlemenize, saklı yordamların ve tetikleyicilerin yanı sıra Depolama blobları ve kuyrukları gibi diğer Azure varlıklarını güncelleştirmesine olanak sağlar.

Bu kılavuzda, Depolama Gezgini’nde Azure Cosmos DB için görülen yaygın sorunların çözümleri özetlenmektedir.

- [Oturum açma sorunları](#Sign-in-issues)
  - [Sertifika zincirindeki otomatik olarak imzalanan sertifika](#Self-signed-certificate-in-certificate-chain)
  - [Abonelikler alınamıyor](#Unable-to-retrieve-subscriptions)
  - [Kimlik doğrulaması sayfası görülemiyor](#Unable-to-see-the-authentication-page)
  - [Hesap kaldırılamıyor](#Cannot-remove-account)
- [Http/Https ara sunucu sorunu](#Http/Https-proxy-issue)
- ["Yerel ve Bağlı" düğümünün altındaki "Geliştirme" düğümü sorunu](#Development-node-under-Local-and-Attached-node-issue)
- ["Yerel ve Bağlı" düğümünde Azure Cosmos DB hesabını ekleme hatası](#Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error)
- [Azure Cosmos DB düğümünü genişletme hatası](#Expand-Azure-Cosmos-DB-node-error)
- [Sonraki adımlar](#Next-steps)

<h2 id="Sign-in-issues">Oturum açma sorunları</h2>

Devam etmeden önce uygulamanızı yeniden başlatmayı deneyin ve sorunların düzeltilip düzeltilmediğine bakın.

<h2 id="Self-signed-certificate-in-certificate-chain">Sertifika zincirindeki otomatik olarak imzalanan sertifika</h2>

Bu hatayı görmenizin birkaç nedeni vardır. En yaygın iki neden şudur:

1. Bir “saydam ara sunucunun” ardındasınızdır; başka bir deyişle birisi (örn. BT departmanınız) HTTPS trafiğini kesintiye uğratıyor, trafiğin şifresini çözüyor ve sonra otomatik olarak imzalanan bir sertifika kullanarak trafiği şifreliyordur.

2. Aldığınız HTTPS iletilerine otomatik olarak imzalanan SSL sertifikaları ekleyen antivirüs yazılımı gibi bir yazılım çalıştırıyorsunuzdur.

Depolama Gezgini bu "otomatik olarak imzalanan sertifikalardan" biriyle karşılaştığında artık aldığı HTTPS iletisinin kurcalanıp kurcalanmadığını bilemez. Otomatik olarak imzalanan sertifikanın bir kopyası varsa, Depolama Gezgini’ne buna güvenmesi gerektiğini bildirebilirsiniz. Sertifikayı kimin eklediğinden emin değilseniz, aşağıdaki adımları uygulayarak kendiniz bulmaya çalışabilirsiniz:

1. Açık SSL yükleme
     - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (basit sürümlerden herhangi biri olabilir)
     - Mac ve Linux: İşletim sisteminize eklenmelidir
2. Açık SSL çalıştırma
    - Windows: Yükleme dizinine gidin, ardından **/bin/** konumuna gidip **openssl.exe** dosyasına çift tıklayın.
    - Mac ve Linux: Bir terminalden **openssl** komutunu yürütün
3. `s_client -showcerts -connect microsoft.com:443` yürütme
4. Otomatik olarak imzalanan sertifikaları bulun. Hangisinin otomatik olarak imzalandığından emin değilseniz, konu ("s:") ve veren ("i:") aynı olan sertifikaları bulun.
5.  Otomatik olarak imzalanan bir sertifika bulduktan sonra, **-----BEGIN CERTIFICATE-----** ile **-----END CERTIFICATE-----** arasındaki (bu kısımlar da dahil) her şeyi kopyalayıp yeni bir .cer dosyasına yapıştırın.
6.  Depolama Gezgini’ni açın ve **Düzenle** > **SSL Sertifikaları** > **Sertifikaları İçeri Aktar** bölümüne gidin. Dosya seçicisini kullanarak, oluşturduğunuz .cer dosyalarını bulun, seçin ve açın.

Yukarıdaki adımları kullanarak otomatik olarak imzalanan bir sertifika bulamazsanız daha fazla yardım için geri bildirim gönderebilirsiniz.

<h2 id="Unable-to-retrieve-subscriptions">Abonelikler alınamıyor</h2>

Başarıyla oturum açtıktan sonra aboneliklerinizi alamıyorsanız:

- [Azure Portal](http://portal.azure.com/)’da oturum açarak, hesabınızın aboneliklerinize erişiminin olduğunu doğrulayın
- Doğru ortamı kullanarak oturum açtığınızdan emin olun ([Azure](http://portal.azure.com/), [Azure Çin](https://portal.azure.cn/), [Azure Almanya](https://portal.microsoftazure.de/), [Azure ABD Kamu](http://portal.azure.us/) veya Özel Ortam/Azure Stack)
- Bir ara sunucunun ardından değilseniz, Depolama Gezgini ara sunucusunu düzgün şekilde yapılandırdığınızdan emin olun
- Hesabı kaldırıp yeniden eklemeyi deneyin
- Aşağıdaki dosyaları giriş dizininizden (örn: C:\Users\ContosoUser) silmeyi ve sonra hesabı yeniden eklemeyi deneyin:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Oturum açarken geliştirici araçları konsolunda (f12) herhangi bir hata iletisi olup olmadığını gözlemleyin

![console](./media/troubleshoot-cosmosdb/console.png)

<h2 id="Unable-to-see-the-authentication-page">Kimlik doğrulaması sayfası görülemiyor</h2>  

Kimlik doğrulaması sayfasını göremiyorsanız:

- Bağlantınızın hızına bağlı olarak, oturum açma sayfasının yüklenmesi biraz zaman alabilir, kimlik doğrulaması iletişim kutusunu kapatmadan önce en az bir dakika bekleyin
- Bir ara sunucunun ardından değilseniz, Depolama Gezgini ara sunucusunu düzgün şekilde yapılandırdığınızdan emin olun
- F12 tuşuna basarak geliştirici konsolunu getirin. Geliştirici konsolundan gelen yanıtları izleyin ve kimlik doğrulamasının neden çalışmadığına dair bir ipucu bulup bulamayacağınızı görün

<h2 id="Cannot-remove-account">Hesap kaldırılamıyor</h2>

Bir hesabı kaldıramıyorsanız veya yeniden kimlik doğrulama bağlantısı bir işlem yapmazsa

- Aşağıdaki dosyaları giriş dizininizden silmeyi ve sonra hesabı yeniden eklemeyi deneyin:
  - .adalcache
  - .devaccounts
  - .extaccounts
- SAS bağlı Depolama kaynaklarını kaldırmak istiyorsanız şunları silin:
  - Windows için %AppData%/StorageExplorer klasörü
  - Mac için /Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer
  - Linux için ~/.config/StorageExplorer
  - Bu dosyaları silerseniz **tüm kimlik bilgilerinizi yeniden girmeniz gerekir**


<h2 id="Http/Https-proxy-issue">Http/Https ara sunucu sorunu</h2>

ASE’de http/https ara sunucusunu yapılandırırken sol ağaçta Azure Cosmos DB düğümlerini listeleyemezsiniz. Bu bilinen bir sorundur ve sonraki yayında düzeltilecektir. Şimdilik geçici bir çözüm olarak Azure portalında Azure Cosmos DB veri gezginini kullanabilirsiniz. 

<h2 id="Development-node-under-Local-and-Attached-node-issue">"Yerel ve Bağlı" düğümünün altındaki "Geliştirme" düğümü sorunu</h2>

Soldaki ağaçta "Yerel ve Bağlı" düğümünün altındaki "Geliştirme" düğümüne tıklandıktan sonra bir yanıt yok.  Bu beklenen bir davranıştır. Azure Cosmos DB yerel öykünücüsü, sonraki yayında desteklenecektir.

![Geliştirme düğümü](./media/troubleshoot-cosmosdb/development.png)

<h2 id="Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error">"Yerel ve Bağlı" düğümünde Azure Cosmos DB hesabını ekleme hatası</h2>

"Yerel ve Bağlı" düğümünde Azure Cosmos DB hesabını ekledikten sonra aşağıdaki hatayı görürseniz, doğru bağlantı dizesini kullanıp kullanmadığınızı denetleyin.

![Yerel ve Bağlı düğümünde Azure Cosmos DB ekleme hatası](./media/troubleshoot-cosmosdb/attached-error.png)

<h2 id="Expand-Azure-Cosmos-DB-node-error">Azure Cosmos DB düğümünü genişletme hatası</h2>

Soldaki ağaç düğümlerini genişletmeye çalışırken aşağıdaki hatayı görebilirsiniz. 

![Genişletme Hatası](./media/troubleshoot-cosmosdb/expand-error.png)

Aşağıdaki önerileri deneyin:

- Azure Cosmos DB hesabının, sağlama durumunda olup olmadığını denetleyin ve hesap başarıyla oluşturulduğunda yeniden deneyin.
- Hesap, "Hızlı Erişim" düğümünün veya "Yerel ve Bağlı" düğümlerinin altındaysa, hesabın silinip silinmediğini denetleyin. Silindiyse, düğümü sizin kaldırmanız gerekir.

<h2 id="Next-steps">Sonraki adımlar</h2>

Hiçbir çözüm işinize yaramazsa, sorunu düzeltmek için Azure Cosmos DB Geliştirme Araçları Ekibine ([cosmosdbtooling@microsoft.com](mailto:cosmosdbtooling@microsoft.com)) sorunun ayrıntılarını içeren bir e-posta gönderin.





