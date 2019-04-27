---
title: Azure Market VM görüntünüzü sertifika | Microsoft Docs
description: Azure Marketi sertifikası için bir VM görüntüsü gönderme ve test açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/26/2018
ms.author: pbutlerm
ms.openlocfilehash: 24430b1b785a24da06a8ea51594147040e6d5bd6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60638394"
---
# <a name="certify-your-vm-image"></a>VM görüntünüzdeki Onayla

Sanal makinenize (VM) oluşturup sonra test ve sanal makine görüntüsünü Azure Marketi sertifikası için gönderin. Bu makalede alınacağı açıklanmaktadır *Azure sertifikası için sertifika Test aracı*, VM görüntünüz onaylamak için bu aracı kullanma ve doğrulama sonuçlarını Vhd'lerinizin bulunduğu Azure kapsayıcısına karşıya yükleme. 


## <a name="download-and-run-the-certification-test-tool"></a>Sertifika sınama aracını indirme ve çalıştırma

Azure sertifikası için sertifika Test aracı yerel bir Windows makinede çalışır, ancak bir Windows Azure tabanlı veya Linux VM'i sınar.  Kullanıcı VM görüntüsü, Microsoft Azure ile uyumlu olduğunu doğrular; yönelik yönerge ve VHD'nizi hazırlamaya etrafında gereksinimlerin karşılanmış olduğunu. Aracın çıktısı, karşıya yükleyecek bir uyumluluk raporudur [bulut iş ortağı portalı](https://cloudpartner.azure.com) VM sertifika istemek için.

1. İndirme ve yükleme son [Azure sertifikası için sertifika Test aracı](https://www.microsoft.com/download/details.aspx?id=44299). 
2. Sertifika Aracı'nı açın ve ardından **teni Test Başlat**.
3. Gelen **Test bilgileri** ekranında, girin bir **Test adı** test çalıştırması için.
4. Seçin **Platform** , VM'niz için ya da `Windows Server` veya `Linux`. Diğer seçenekleri, platform seçimi etkiler.
5. Bu veritabanı hizmeti, sanal Makinenizin kullanıyorsanız seçin **Azure SQL veritabanı için Test** onay kutusu.

   ![Sertifika test aracı başlangıç sayfası](./media/publishvm_025.png)


## <a name="connect-the-certification-tool-to-a-vm-image"></a>Sertifika aracı bir VM görüntüsüne bağlama

  Aracı ile Windows tabanlı Vm'leri bağlanır [PowerShell](https://docs.microsoft.com/powershell/) ve Linux Vm'leri ile bağlandığı [SSH.Net](https://www.ssh.com/ssh/protocol/).

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>Sertifika Aracı'nı bir Linux VM görüntüsüne bağlama

1. Seçin **SSH kimlik doğrulaması** modu: `Password Authentication` veya `key File Authentication`.
2. Parola tabanlı kimlik doğrulama kullanılıyorsa için değerleri girin **VM DNS adı**, **kullanıcı adı**, ve **parola**.  İsteğe bağlı olarak, varsayılan değiştirebilirsiniz **SSH bağlantı noktası** sayı.

     ![Linux VM görüntüsü parola kimlik doğrulaması](./media/publishvm_026.png)

3. Anahtar dosya tabanlı kimlik doğrulaması kullanıyorsanız, için değerleri girin **VM DNS adı**, **kullanıcı adı**, ve **özel anahtarı** konumu.  İsteğe bağlı olarak, kaynağı bir **parola** veya varsayılan değeri değiştirmek **SSH bağlantı noktası** sayı.

     ![Linux VM görüntüsü dosyası kimlik doğrulaması](./media/publishvm_027.png)

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Sertifika aracı bir Windows tabanlı VM görüntüsüne bağlama**
1. Tam girin **VM DNS adı** (örneğin, `MyVMName.Cloudapp.net`).
2. İçin değerler girin **kullanıcı adı** ve **parola**.

   ![Windows VM görüntüsünün parola kimlik doğrulaması](./media/publishvm_028.png)


## <a name="run-a-certification-test"></a>Bir sertifika testi Çalıştır

Parametre değerlerini, sertifika aracı, VM görüntünüz için sağladığınız sonra seçin **Test Bağlantısı** sanal makinenize yönelik geçerli bir bağlantı sağlamak için. Bağlantı doğrulandıktan sonra seçin **sonraki** testi başlatmak için.  Test tamamlandıktan sonra test sonuçları (başarılı/başarısız/uyarı) ile bir tablo görüntülenir.  Aşağıdaki örnek, bir Linux sanal makinesi test için test sonuçlarını gösterir. 

![Linux VM görüntüsü için sertifika test sonuçları](./media/publishvm_029.png)

Testler başarısız olursa, görüntüsüdür *değil* sertifikalı. Bu durumda, hata iletileri ve gereksinimleri gözden geçirin, istenen değişiklikleri yapmak ve testi yeniden çalıştırın. 

VM görüntünüzdeki hakkında ek bilgi sağlamak için gereklidir otomatik test sonra **anketi** ekran.  Bu, tamamlaması gereken iki sekme içerir.  **Genel değerlendirmesi** sekmesini içeren **True/False** soru ise **çekirdek özelleştirme** birden fazla seçimi ve serbest biçimli sorular içerir.  Her iki sekmede hakkında sorular tamamlamanız ve ardından seçin **sonraki**.

![Sertifika aracı anketi](./media/publishvm_030.png)

Son ekran bir Linux VM görüntüsü için SSH erişim bilgileri ve özel durumlar arıyorsanız başarısız tüm değerlendirmesi için bir açıklama gibi ek bilgileri girmenize olanak tanır. 

Son olarak, tıklayın **rapor oluştur** test sonuçlarını indirerek ve anketi verdiğiniz yanıtlar için ayrıca günlük dosyaları yürütülen test çalışmalarını. Vhd'leriniz ile aynı kapsayıcıda sonuçları kaydedin.

![Sertifika, test sonuçları Kaydet](./media/publishvm_031.png)


## <a name="next-steps"></a>Sonraki adımlar

Sonra [Tekdüzen Kaynak Tanımlayıcıları (URI) her bir VHD oluşturmak](./cpp-get-sas-uri.md) Market'te gönderdiğiniz. 
