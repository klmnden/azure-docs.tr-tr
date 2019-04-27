---
title: Microsoft Azure tabanlı VM görüntünüz için paylaşılan erişim imzası URI'si alın | Microsoft Docs
description: Paylaşılan erişim imzası (SAS) URI VM görüntünüz için alınacağı açıklanmaktadır.
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
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: c21fa3cf819f48dcda46f2d444ed52bc2eb9ae3d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60744039"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>VM görüntünüz için paylaşılan erişim imzası URI'si Al

Yayımlama işlemi sırasında SKU ile ilişkili her sanal sabit disk (VHD) için bir Tekdüzen Kaynak Tanımlayıcısı (URI) sağlamanız gerekir. Sertifika işlemi sırasında Microsoft'un bu VHD'lere erişmesi gerekir. Bu makalede, bir paylaşılan erişim imzası (SAS) URI için her bir VHD oluşturma adımları açıklanmaktadır. Bu URI girer **SKU'ları** bulut iş ortağı portalı sekmesindedir. 

Vhd'leriniz için SAS URI'leri oluştururken aşağıdaki gereksinimlere uyması:

- Yalnızca yönetilmeyen VHD'ler desteklenir.
- `List` ve `Read` izinler yeterli değil. Yapmak *değil* sağlamak `Write` veya `Delete` erişim.
- Erişim süresi (*sona erme tarihi*) en az üç SAS URI'sini oluşturulduğunda gelen haftalık olmalıdır.
- UTC saat farklılıklara karşı korumak için geçerli tarihten bir gün için başlangıç tarihini ayarlayın. Örneğin, geçerli tarihi 6 Ekim 2014 ise 10/5/2014'ı seçin.

## <a name="generate-the-sas-url"></a>SAS URL'sini oluşturun

SAS URL'si aşağıdaki araçları kullanarak iki yaygın şekilde oluşturulabilir:

-   Microsoft Storage Gezgini - Windows, macOS ve Linux'ta kullanılabilen grafik aracı
-   Microsoft Azure CLI - Windows - OSs ve otomatik olarak veya sürekli tümleştirme ortamları için önerilen


### <a name="azure-cli"></a>Azure CLI

Azure CLI ile bir SAS URI'si oluşturmak için aşağıdaki adımları kullanın.

1. İndirme ve yükleme [Microsoft Azure CLI'yı](https://azure.microsoft.com/documentation/articles/xplat-cli-install/).  Sürümleri, Windows, macOS ve Linux çeşitli işlemleri için kullanılabilir. 
2. Bir PowerShell dosyası oluşturun (`.ps1` dosya uzantısı), aşağıdaki kodu kopyalayın ve ardından yerel olarak kaydedin.

   ``` powershell
   az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
   ```
    
3. Aşağıdaki parametre değerlerini sağlamak için dosyayı düzenleyin.  Tarihleri sağlanmalıdır UTC tarih saat biçiminde örneğin `10-25-2016T00:00:00Z`.
   - `<account-name>` -Azure depolama hesabı adı
   - `<account-key>` -Azure depolama hesabı anahtarınızı
   - `<vhd-name>` -VHD adı
   - `<start-date>` -Permission başlangıç tarihi VHD erişim için. Bir tarihi geçerli tarihten bir gün belirtin. 
   - `<expiry-date>` -VHD erişim izni sona erme tarihi.  En az üç hafta ötesinde geçerli tarihi bir tarih belirtin. 
 
   Aşağıdaki örnek, uygun parametre değerleri (Bu makalenin yazıldığı sırada) gösterir.

   ``` powershell
       az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2017-11-06T00:00:00Z' --expiry '2018-08-20T00:00:00Z'
   ```
 
4. Bu PowerShell Betiği için değişiklikleri kaydedin.
5. Bu komut dosyasını oluşturmak için yönetici ayrıcalıkları kullanarak çalıştırın bir *SAS bağlantı dizesi* kapsayıcı düzeyinde erişim için.  İki temel yaklaşım kullanabilirsiniz:
   - Konsoldan betiği çalıştırın.  Örneğin, Windows yazma betik ve select tıklamayla **yönetici olarak çalıştır**.
   - Betik gibi bir PowerShell komut dosyası Düzenleyicisi'nden çalıştırmak [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise), yönetici ayrıcalıklarını kullanarak. 
     Bu Düzenleyici içinde oluşturulan bir SAS bağlantı dizesi gösterir. 

     ![PowerShell ıse'de SAS URI'si oluşturma](./media/publishvm_032.png)

6. Oluşturulan SAS bağlantı dizesini kopyalayın ve güvenli bir konumda bir metin dosyasına kaydedin.  İlişkili VHD'yi konum bilgileri son SAS URI'sini oluşturmak için eklemek için bu dize düzenleyeceksiniz. 
7. Azure portalında yeni oluşturulan URI ile ilişkilendirilen VHD içeren blob depolama birimine gidin.
8. URL değerini kopyalayın **Blob Hizmeti uç noktası**, aşağıda gösterildiği gibi.

    ![BLOB Hizmeti uç noktası Azure portalında](./media/publishvm_033.png)

9. 6. adım SAS bağlantı dizesi ile metin dosyasını düzenleyin.  Aşağıdaki biçimi kullanarak tam SAS URI'sini oluşturmak:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Örneğin, VDH adını ise `TestRGVM2.vhd`, sonuçta elde edilen SAS URI şu şekilde olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

SKU'ları dağıtmayı planladığınız her VHD için bu adımları yineleyin.


### <a name="microsoft-storage-explorer"></a>Microsoft Depolama Gezgini

Microsoft Azure Depolama Gezgini ile SAS URI'si oluşturmak için aşağıdaki adımları kullanın.

1. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)'ni indirip yükleme.
2. Gezgini'ni açın ve sol menü çubuğunda, tıklayarak **hesabı Ekle** simgesi.  **Azure Storage'a Bağlan** iletişim kutusu görüntülenir.
3. **Azure Hesabı Ekle**'yi seçip **Oturum açın**'a tıklayın.  Azure hesabınızda oturum açmak için gerekli adımlara devam edin.
4. Sol taraftaki içinde **Gezgini** bölmesinde gidin, **depolama hesapları** bu düğümünü genişletin.
5. VHD sağ tıklatın **paylaşım erişim imzası Al** bağlam menüsünden. 

    ![Azure Gezgini'nde SAS öğesi Al](./media/publishvm_034.png)

6. **Paylaşılan erişim imzası** iletişim kutusu görüntülenir. Aşağıdaki alanlar için değerleri girin:
   - **Başlangıç saati** -izni başlangıç tarihi VHD erişim için. Bir tarihi geçerli tarihten bir gün belirtin.
   - **Süre sonu** -VHD erişim izni sona erme tarihi.  En az üç hafta ötesinde geçerli tarihi bir tarih belirtin.
   - **İzinleri** - seçin `Read` ve `List` izinleri. 

     ![Azure Gezgini'nde SAS iletişim kutusu](./media/publishvm_035.png)

7. Tıklayın **Oluştur** bu VHD ilişkili SAS URI'sini oluşturmak için.  İletişim kutusu, artık bu işlem ayrıntılarını görüntüler. 
8. Kopyalama **URL** değeri ve güvenli bir konumda bir metin dosyasına kaydedin. 

    ![Azure Gezgini'nde SAS URI'si oluşturma](./media/publishvm_036.png)

    Bu oluşturulan SAS URL'si için kapsayıcı düzeyi erişimdir.  Belirgin hale getirmek için ilişkili olarak VHD adını kendisine eklenmelidir.

9. Metin dosyasını düzenleyin. VHD adından sonra Ekle `vhds` SAS URL dizesinde (önde gelen eğik çizgi dahil).  Son SAS URI biçiminde olması:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Örneğin, VDH adını ise `TestRGVM2.vhd`, sonuçta elde edilen SAS URI şu şekilde olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

SKU'ları dağıtmayı planladığınız her VHD için bu adımları yineleyin.


## <a name="verify-the-sas-uri"></a>SAS URI'sini doğrulayın

Gözden geçirin ve SAS URI'si aşağıdaki denetim listesini kullanarak oluşturulan her doğrulayın.  Aşağıdakileri doğrulayın:
- URI biçimi şöyledir:       `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`
- URI ".vhd" dosya adı uzantısı dahil olmak üzere, VHD görüntüsü filename içeriyor.
- URI ortasına doğru `sp=rl` görünür. Bu dize belirten `Read` ve `List` erişim belirtildi.
- Bu noktadan sonra `sr=c` de görünür. Bu dize, kapsayıcı düzeyinde erişim belirtilip belirtilmediğini gösterir.
- Kopyalayıp URI ilişkili blob yüklemeye başlamak için bir tarayıcıya yapıştırın.  (Yükleme tamamlanmadan önce işlemi iptal edebilirsiniz.)


## <a name="next-steps"></a>Sonraki adımlar

SAS URI'si oluşturma sorunlarla karşılaşıyorsanız bkz [ortak bir SAS URL'si sorunları](./cpp-common-sas-url-issues.md).  Aksi takdirde, SAS URI(s) daha sonra kullanmak için güvenli bir konuma kaydedin. İçin gerekli [, VM teklifi yayımlama](./cpp-publish-offer.md) bulut iş ortağı Portalı'nda.
