<properties
   pageTitle="Azure RemoteApp ile herhangi bir cihazda aynı Office 365 deneyimini elde edin | Microsoft Azure"
   description="Azure RemoteApp kullanarak herhangi bir Office 365 uygulamasını kullanıcılarınızla nasıl paylaşacağınızı öğrenin."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>



# Azure RemoteApp ile herhangi bir cihazda aynı Office 365 deneyimini elde edin

> [AZURE.IMPORTANT]
> Azure RemoteApp kullanımdan kaldırılıyor. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.

Bu makalede, Office 365’i şirket içinde herhangi bir cihazda nasıl dağıtacağınız açıklanmaktadır. Kullanıcılarınız Android, Apple ve Windows’da aynı özellikler ve kullanıcı arabirimi deneyimine sahip olur.

Bunu Azure’da kullanıcıların bağlanabileceği ölçeklenebilir sanal makinelerde Office 365’i barındırmak için Azure RemoteApp kullanarak yapabiliriz. Bu sanal makineler kümesi "bulut koleksiyonu" olarak adlandırılır.

## Bulut koleksiyonu oluşturma

Azure hesabı oluşturduktan sonra, sol taraftaki bağlantıya tıklayarak **RemoteApp**’e gidin.
![Azure Portalı’nda Azure RemoteApp gösteriliyor](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

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

## Office 365’e bağlanın

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



<!--HONumber=Sep16_HO3-->


