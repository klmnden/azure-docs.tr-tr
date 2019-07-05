---
title: Azure veri kutusu Blob Depolama REST API'leri aracılığıyla veri kopyalamak için öğretici | Microsoft Docs
description: Azure veri kutusu Blob depolamanızın REST API'leri aracılığıyla veri kopyalama hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 2c66b94cbcfa4688d9dc45d99688abe76fa55d17
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595745"
---
# <a name="tutorial-copy-data-to-azure-data-box-blob-storage-via-rest-apis"></a>Öğretici: Azure veri kutusu Blob Depolama REST API'leri aracılığıyla veri kopyalama  

Bu öğreticide, Azure veri kutusu Blob Depolama REST API'leri aracılığıyla üzerinden bağlanmak için gerekli yordamlar açıklanmaktadır *http* veya *https*. Bağlantı kurulduktan sonra veri kutusu Blob depolama alanına veri kopyalamak için gereken adımlar açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Veri kutusu Blob Depolama bağlanma *http* veya *https*
> * Veri kutusu ağır için veri kopyalama

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladınız [Öğreticisi: Azure veri kutusu ağır kümesi](data-box-heavy-deploy-set-up.md).
2. Aldığınız, veri kutusu yoğun ve sipariş durumu Portalı'nda **teslim edildi**.
3. Gözden geçirdim [veri kutusu Blob Depolama için sistem gereksinimleri](data-box-system-requirements-rest.md) ve API'leri, SDK'lar ve Araçlar'ın desteklenen sürümleriyle ilgili bilgi sahibi olduğunuz.
4. Erişim üzerinden veri kutusu ağır kopyalamak istediğiniz verileri içeren bir ana bilgisayara göre. Ana bilgisayarınız:
    - [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı bir ağa bağlı olmalıdır. Hızlı kopya hızları için iki 40 GbE bağlantılar (düğüm başına bir adet) paralel olarak yararlanılabilir. 40 GbE bağlantı kullanılabilir değilse, en az iki 10 GbE bağlantılar (düğüm başına bir tane) sahip olmasını öneririz. 
5. [AzCopy 7.1.0 indirme](https://aka.ms/azcopyforazurestack20170417) ana bilgisayarınızda. Ana bilgisayarınızdan Azure veri kutusu Blob depolama alanına veri kopyalamak için AzCopy kullanacaksınız.


## <a name="connect-via-http-or-https"></a>HTTP veya https aracılığıyla bağlanma

Veri kutusu Blob depolama alanına üzerinden bağlanabilir *http* veya *https*.

- *HTTPS* veri kutusu Blob depolamaya bağlanmak için güvenli ve önerilen yoludur.
- *HTTP* ağlar üzerinden bağlanma güvenilen kullanılır.

Üzerinde veri kutusu Blob depolamaya bağlanırken bağlanma adımları farklı *http* veya *https*.

## <a name="connect-via-http"></a>HTTP üzerinden bağlanma

Veri kutusu Blob Depolama REST API'leri için bir bağlantı üzerinden *http* aşağıdaki adımları gerektirir:

- Cihaz IP ekleyin ve uzak ana bilgisayar için hizmet uç noktası blob
- Üçüncü taraf yazılım yapılandırma ve bağlantıyı doğrulama

Bu adımların her biri, aşağıdaki bölümlerde açıklanmıştır.

> [!IMPORTANT]
> Veri kutusu yoğun için ikinci düğüme bağlanmak için tüm bağlantı yönergeleri tekrarlamanız gerekir.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Blob Hizmeti uç noktası ve cihaz IP adresi Ekle

[!INCLUDE [data-box-add-device-ip](../../includes/data-box-add-device-ip.md)]



### <a name="configure-partner-software-and-verify-connection"></a>İş ortağı yazılım yapılandırma ve bağlantısını doğrulama

[!INCLUDE [data-box-configure-partner-software](../../includes/data-box-configure-partner-software.md)]

[!INCLUDE [data-box-verify-connection](../../includes/data-box-verify-connection.md)]

## <a name="connect-via-https"></a>HTTPS bağlanma

Bağlantı https üzerinden Azure Blob Depolama REST API'leri için aşağıdaki adımları gerektirir:

- Azure Portalı'ndan sertifikayı indirin
- Uzak ana bilgisayarda veya istemci sertifikasını içeri aktarın
- Cihaz IP ekleyin ve hizmet uç noktası istemci veya uzak bir ana bilgisayar için blob
- Üçüncü taraf yazılım yapılandırma ve bağlantıyı doğrulama

Bu adımların her biri, aşağıdaki bölümlerde açıklanmıştır.

> [!IMPORTANT]
> Veri kutusu yoğun için ikinci düğüme bağlanmak için tüm bağlantı yönergeleri tekrarlamanız gerekir.

### <a name="download-certificate"></a>Sertifikayı indirin

Sertifika indirmek için Azure portalını kullanın.

1. Azure portalında oturum açın.
2. Data Box Siparişiniz gidip için **genel > cihaz ayrıntıları**.
3. Altında **cihaz kimlik bilgilerini**Git **API erişimi** aygıt için. **İndir**'e tıklayın. Bu eylem indiren bir  **\<sipariş adınız > .cer** sertifika dosyası. **Kaydet** bu dosya. Bu sertifika, cihaza bağlanmak için kullanacağınız istemci veya konak bilgisayara yükler.

    ![Azure portalında sertifikasını indirin](media/data-box-deploy-copy-data-via-rest/download-cert-1.png)
 
### <a name="import-certificate"></a>Sertifika İçeri Aktar 

HTTPS üzerinden veri kutusu Blob depolama alanına erişim, cihaz için bir SSL sertifikası gerektirir. Bu sertifika bir istemci uygulama için kullanılabilir yapılır biçimi, uygulama başka bir uygulama ve işletim sistemleri ve dağıtımlar arasında değişir. Bazı uygulamalar diğer uygulamaları değil yaparken sistemin sertifika deposuna aktardıktan sonra sertifikayı erişebilir o mekanizmasını kullanın.

Bazı uygulamalar için belirli bilgiler, bu bölümde açıklanan. Diğer uygulamalar hakkında daha fazla bilgi için uygulama ve kullanılan işletim sistemi belgelerine bakın.

İçeri aktarmak için aşağıdaki adımları izleyin `.cer` dosyası bir Windows veya Linux istemcisi kök deposuna. Bir Windows sisteminde almak ve sisteminizde sertifikayı yüklemek için Windows PowerShell veya Windows Server kullanıcı Arabirimi kullanabilirsiniz.

#### <a name="use-windows-powershell"></a>Windows PowerShell kullanma

1. Bir Windows PowerShell oturumu yönetici olarak başlatın.
2. Komut istemine şunları yazın:

    ```
    Import-Certificate -FilePath C:\temp\localuihttps.cer -CertStoreLocation Cert:\LocalMachine\Root
    ```

#### <a name="use-windows-server-ui"></a>Windows sunucusu kullanıcı arabirimini kullanın

1.  Sağ `.cer` seçin ve dosya **yükleme sertifika**. Bu eylem, Sertifika Alma Sihirbazı'nı başlatır.
2.  İçin **Store konumu**seçin **yerel makine**ve ardından **sonraki**.

    ![PowerShell kullanarak sertifikayı içeri aktarma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-1.png)

3.  Seçin **tüm sertifikaları aşağıdaki depolama alanına yerleştir**ve ardından **Gözat**. Uzak ana kök deposuna gidin ve ardından **sonraki**.

    ![PowerShell kullanarak sertifikayı içeri aktarma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-2.png)

4.  **Son**'a tıklayın. İçeri aktarmanın başarılı olduğunu bildiren bir ileti görüntülenir.

    ![PowerShell kullanarak sertifikayı içeri aktarma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-3.png)

#### <a name="use-a-linux-system"></a>Bir Linux sistem kullanın

Yöntem bir sertifikayı içeri aktarmak için dağıtım göre değişir.

> [!IMPORTANT]
> Veri kutusu yoğun için ikinci düğüme bağlanmak için tüm bağlantı yönergeleri tekrarlamanız gerekir.

Ubuntu ve Debian, gibi birçok `update-ca-certificates` komutu.  

- İçin Base64 kodlamalı sertifika dosyasını yeniden adlandırma bir `.crt` uzantısı ve içine kopyalayın `/usr/local/share/ca-certificates directory`.
- Komutunu çalıştırın `update-ca-certificates`.

RHEL, Fedora ve en son sürümlerini kullanan `update-ca-trust` komutu.

- Sertifika dosyasına kopyalayın `/etc/pki/ca-trust/source/anchors` dizin.
- `update-ca-trust` öğesini çalıştırın.

Dağıtımınız için belirli Ayrıntılar için belgelere bakın.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Blob Hizmeti uç noktası ve cihaz IP adresi Ekle 

Aynı adımları [aygıt IP adresi ekleme ve hizmet uç noktası üzerinden bağlanırken blob *http*](#add-device-ip-address-and-blob-service-endpoint).

### <a name="configure-partner-software-and-verify-connection"></a>İş ortağı yazılım yapılandırma ve bağlantısını doğrulama

Adımlarını izleyin [üzerinden bağlanırken kullanılan iş ortağı yazılım yapılandırma *http*](#configure-partner-software-and-verify-connection). Tek fark, bırakmanız gerekir *http seçeneğini* seçeneği işaretli değil.

## <a name="copy-data-to-data-box-heavy"></a>Veri kutusu ağır için veri kopyalama

Veri kutusu Blob depolama alanına bağlandıktan sonra sonraki adıma veri kopyalamaktır. Veri kopyalama önce aşağıdaki konuları gözden geçirin:

-  Veri kopyalama sırasında veri boyutu, açıklanan boyutu sınırları uymasını sağlamak [Azure depolama ve veri kutusu ağır sınırları](data-box-limits.md).
- Veri kutusu ağır tarafından karşıya yükleniyor, verileri, eşzamanlı olarak veri kutusu ağır dışında başka uygulamalar tarafından yüklenirse bu yükleme işi hataları ve veri bozulmasına neden olabilir.

Bu öğreticide, veri kutusu Blob depolama alanına veri kopyalamak için AzCopy kullanılır. Verileri kopyalamak için Azure Depolama Gezgini (GUI tabanlı bir araç tercih ederseniz) veya bir iş ortağı yazılım kullanabilirsiniz.

Kopyalama yordam aşağıdaki adımı vardır:

- Bir kapsayıcı oluşturma
- Bir klasörün içeriğini veri kutusu Blob depolamaya yükleme
- Değiştirilmiş dosyalar veri kutusu Blob depolamaya yükleme


Bu adımların her biri, aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

> [!IMPORTANT]
> Veri kutusu yoğun için ikinci düğüme veri kopyalamak için tüm kopyalama yönergeleri tekrarlamanız gerekir.

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir çünkü ilk adımı bir kapsayıcı oluşturmaktır. Kapsayıcıları BLOB gruplarını, bilgisayarınızda dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi. Bir blob kapsayıcısı oluşturmak için aşağıdaki adımları izleyin.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, blob kapsayıcısını oluşturmak istediğiniz depolama hesabını genişletin.
3. Sağ **Blob kapsayıcıları**, bağlam menüsünden seçip **Blob kapsayıcısı Oluştur**.

   ![BLOB kapsayıcıları bağlam menüsü oluşturma](media/data-box-deploy-copy-data-via-rest/create-blob-container-1.png)

4. Bir metin kutusu altında görünen **Blob kapsayıcıları** klasör. Blob kapsayıcınızın adını girin. Bkz: [kapsayıcı oluşturma ve izinleri ayarlama](../storage/blobs/storage-quickstart-blobs-dotnet.md) blob kapsayıcılarını adlandırmayla ilgili kural ve kısıtlamaların hakkında bilgi için.
5. Tuşuna **Enter** blob kapsayıcısı oluşturma işlemi tamamlandığında veya **Esc** iptal etmek için. Blob kapsayıcısı başarıyla oluşturulduktan sonra altında gösterilir **Blob kapsayıcıları** seçili depolama hesabı için bir klasör.

   ![Oluşturulan blob kapsayıcısı](media/data-box-deploy-copy-data-via-rest/create-blob-container-2.png)

### <a name="upload-contents-of-a-folder-to-data-box-blob-storage"></a>Bir klasörün içeriğini veri kutusu Blob depolamaya yükleme

Blob Depolama Windows veya Linux üzerinde bir klasördeki tüm dosyaları yüklemek için AzCopy kullanın. Bir klasördeki tüm blobları karşıya yüklemek için aşağıdaki AzCopy komutunu girin:

#### <a name="linux"></a>Linux

    azcopy \
        --source /mnt/myfolder \
        --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
        --dest-key <key> \
        --recursive

#### <a name="windows"></a>Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S


Değiştirin `<key>` hesabınızın anahtarıyla. Azure portalında hesap anahtarınızı almak için depolama hesabınıza gidin. Git **ayarlar > erişim anahtarları**, bir anahtar seçip AzCopy komutuna yapıştırın.

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy bu kapsayıcıyı oluşturur ve dosyayı kapsayıcıya yükler. Veri dizininizin kaynak yolunu güncelleştirin ve `data-box-storage-account-name` hedef depolama hesabı adı ile URL Data Box'ınızı ile ilişkili.

Belirtilen dizinin içeriklerini Blob depolama alanına yinelemeli olarak yüklemek için `--recursive` (Linux) veya `/S` (Windows) seçeneğini belirtin. AzCopy komutunu şu seçeneklerden biriyle çalıştırdığınızda tüm alt klasörler ve bu klasörlerin dosyaları da karşıya yüklenir.

### <a name="upload-modified-files-to-data-box-blob-storage"></a>Değiştirilmiş dosyalar veri kutusu Blob depolamaya yükleme

AzCopy, son değiştirilme zamanına göre dosyaları karşıya yüklemek için kullanın. Bunu denemek için, test amacıyla kaynak dizininizde yeni dosyalar oluşturun veya değiştirin. Yalnızca güncelleştirilmiş veya yeni dosyaları karşıya yüklemek için, AzCopy komutuna `--exclude-older` (Linux) veya `/XO` (Windows) parametresini ekleyin.

Yalnızca hedefte bulunmayan çıkış kaynaklarını kopyalamak istiyorsanız, AzCopy komutunda `--exclude-older` ve `--exclude-newer` (Linux) veya `/XO` ve `/XN` (Windows) parametrelerini belirtin. AzCopy, yalnızca güncelleştirilmiş verileri zaman damgasına göre karşıya yükler.

#### <a name="linux"></a>Linux
    azcopy \
    --source /mnt/myfolder \
    --destination https://data-box-heavy-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
    --dest-key <key> \
    --recursive \
    --exclude-older

#### <a name="windows"></a>Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-heavy-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S /XO

Connect ya da kopyalama işlemi sırasında herhangi bir hata varsa, bkz. [veri kutusu Blob Depolama ile sorun giderme](data-box-troubleshoot-rest.md).

Sonraki adım, cihazı göndermeye hazırlayın sağlamaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Veri kutusu Blob Depolama bağlanma *http* veya *https*
> * Veri kutusu ağır için veri kopyalama


Data Box'ınızı Microsoft'a göndermeye hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Microsoft, Azure veri kutusu ağır gönderin](./data-box-heavy-deploy-picked-up.md)
