---
title: "Azure RemoteApp ile herhangi bir cihazda aynı Office 365 deneyimini elde edin | Microsoft Belgeleri"
description: "Azure RemoteApp kullanarak herhangi bir Office 365 uygulamasını kullanıcılarınızla nasıl paylaşacağınızı öğrenin."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 584c781c97097cda3c1455ade05cba8659f11073
ms.lasthandoff: 03/31/2017


---
# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Azure RemoteApp ile herhangi bir cihazda aynı Office 365 deneyimini elde edin
> [!IMPORTANT]
> Azure RemoteApp 31 Ağustos 2017’de kullanımdan kaldırılacaktır. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.
> 
> 

Bu makalede, Office 365’i şirket içinde herhangi bir cihazda nasıl dağıtacağınız açıklanmaktadır. Kullanıcılarınız Android, Apple ve Windows’da aynı özellikler ve kullanıcı arabirimi deneyimine sahip olur.

Bunu Azure’da kullanıcıların bağlanabileceği ölçeklenebilir sanal makinelerde Office 365’i barındırmak için Azure RemoteApp kullanarak yapabiliriz. Bu sanal makineler kümesi "bulut koleksiyonu" olarak adlandırılır.

## <a name="create-a-cloud-collection"></a>Bulut koleksiyonu oluşturma
Azure hesabı oluşturduktan sonra, sol taraftaki bağlantıya tıklayarak **RemoteApp**’e gidin.
![Azure Portal’da Azure RemoteApp gösteriliyor](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Ardından, bir koleksiyon oluşturmak için aşağıda **Yeni**’ye ve “Hızlı oluşturma”ya tıklayın. Bir ad, bölge, abonelik, plan ve sağlamış olduğumuz “Office Proffesional 2013” görüntüsünü sağlayın.
![Oluştur İletişim Kutusu](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Formu tamamladığınızda koleksiyon oluşturma süreci başlamalıdır. Bu işlem yaklaşık bir saat sürer.

![Bekleniyor](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

İşlem tamamlandığında, aşağıdakine benzer görünecektir. **Yayımlama**’ya tıkladığımızda, çoğu Office uygulamasının bizim için zaten yayımlandığını görebiliriz.
![Oluşturulan koleksiyon](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Yayımlanan uygulamalar](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Bu noktada **Kullanıcı Erişimi**’ne tıklayarak bu koleksiyona erişimi olan daha fazla kullanıcı ekleyebilirsiniz.
![Kullanıcı erişimini yapılandırma](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Şimdi Office 365’e bağlanmayı deneyelim!

## <a name="connect-to-office-365"></a>Office 365’e bağlanın
Kullandığınız cihaza Azure RemoteApp istemcisini yüklemek için [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/) adresine giderek aşağı doğru ilerleyecek ve **İstemcileri indir**’e tıklayacağız. Aşağıdaki ekran görüntüleri Windows içindir.

Uygulama başlatıldıktan sonra Microsoft hesabınızda (eski adıyla “Live ID”) oturum açmanız istenir, şimdilik Azure hesabınızla aynı hesabı kullanın. Oturum açtığınızda yeni davetlerle ilgili bir bildirim görürsünüz, buna tıkladığınızda aşağıdakine benzer bir liste görüntülenir. Azure hesabı sahibinin e-postasıyla ile eşleşen daveti kabul edin.

![Yeni davet](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Yeni davetler olduğunda bu şekilde görünür.

![Uygulama kabul etme](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Daveti kabul ettikten sonra Azure RemoteApp istemcisindeki tüm Office uygulamalarını görmeniz gerekir.

![Uygulama listesi](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Bu uygulamalardan herhangi birine tıkladığınızda, uygulama Azure sanal makinesinde başlatılır. Hepsi bu kadar! Keyfini çıkarın!

![başlatma](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)


