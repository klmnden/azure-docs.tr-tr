---
title: IOT Edge cihazı - Machine Learning, Azure IOT Edge üzerinde yapılandırma | Microsoft Docs
description: Saydam bir ağ geçidi olarak davranır bir Azure IOT Edge cihazı olarak Linux çalıştıran bir Azure sanal makineyi yapılandırın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a2096004a7b389f627c528a8dfb4768ac001f390
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155621"
---
# <a name="tutorial-configure-an-iot-edge-device"></a>Öğretici: IOT Edge cihazı yapılandırma

> [!NOTE]
> Bu makale bir serinin IOT Edge üzerinde Azure Machine Learning'i kullanma hakkında bir öğretici için parçasıdır. Bu makalede doğrudan gelmiş, başlangıç öneriyoruz [makaleyi](tutorial-machine-learning-edge-01-intro.md) serisindeki en iyi sonuçlar için.

Bu makalede, saydam bir ağ geçidi olarak davranır bir Azure IOT Edge cihazı olarak Linux çalıştıran bir Azure sanal makinesi yapılandırıyoruz. Saydam bir ağ geçidi yapılandırması cihazların Azure IOT ağ geçidi var olan Hub'ına bilerek olmadan ağ geçidi üzerinden bağlanmasına izin verir. Aynı zamanda, cihazlar IOT hub ile etkileşim kurma kullanıcı ara ağ geçidi cihazı habersizdir. Sonuç olarak, saydam bir ağ geçidi için ağ geçidini IOT Edge modülleri ekleyerek sistemimize uç analizi için kullanırız.

Bu makaledeki adımlarda, genellikle bir bulut geliştiricisi tarafından gerçekleştirilir.

## <a name="generate-certificates"></a>Sertifika oluşturma

Ağ geçidi olarak işlevi için bir cihaz için aşağı akış cihazlar için güvenli bir şekilde bağlanabilir olması gerekir. Azure IOT Edge cihazları arasında güvenli bağlantılar kurmak için bir ortak anahtar altyapısı (PKI) kullanmanıza olanak tanır. Bu durumda, biz saydam bir ağ geçidi olarak görev yapan bir IOT Edge cihazına bağlamak için bir aşağı akış cihazı vermiş olursunuz. Makul güvenliğini sağlamak için aşağı akış cihazın IOT Edge cihaz kimliğini onaylamalıdır. IOT Edge cihazları sertifikaları kullanma hakkında daha fazla bilgi için bkz. [Azure IOT Edge sertifika kullanım ayrıntılarını](iot-edge-certs.md).

Bu bölümde, ardından kurduğumuz ve çalıştıran bir Docker görüntüsü kullanılarak otomatik olarak imzalanan sertifikaları oluşturacağız. Windows geliştirme makinenizde sertifikaları oluşturmak için gereken adım sayısını önemli ölçüde azaldığından gibi bu adımı tamamlamak için bir Docker görüntüsü kullanma seçtik. Bkz: [oluşturmak Windows sertifikalarla](how-to-create-transparent-gateway.md#generate-certificates-with-windows) kapsayıcı kullanmadan sertifikaları oluşturmak hakkında ayrıntılar için. [Linux sertifikalarını oluşturmak](how-to-create-transparent-gateway.md#generate-certificates-with-linux) biz Docker görüntüsünü otomatik yönergeleri kümesi vardır.

1. Geliştirme sanal makinenizde oturum açın.

2. Bir komut satırı istemi açın ve sanal makinede bir dizin oluşturmak için aşağıdaki komutu çalıştırın.

    ```cmd
    mkdir c:\edgeCertificates
    ```

3. Başlangıç **için Docker Windows** Windows Başlat menüsünde.

4. Visual Studio Code'u açın.

5. Seçin **dosya** > **Klasör Aç...**  ve **C:\\kaynak\\IoTEdgeAndMlSample\\CreateCertificates**.

6. Dockerfile üzerinde sağ tıklatın ve seçin **derleme yansıma**.

7. İletişim kutusunda, görüntü adı ve etiket için varsayılan değeri kabul edin: **createcertificates:latest**.

8. Derlemenin tamamlanmasını bekleyin.

    > [!NOTE]
    > Eksik bir ortak anahtar için bir uyarı görebilirsiniz. Bu uyarıyı yoksaymak güvenlidir. Benzer şekilde, öneren bir güvenlik uyarısı onay/bu görüntü için yoksaymak güvenlidir görüntünüzü izinlerini sıfırlama görürsünüz.

9. Visual Studio Code terminal penceresinde createcertificates kapsayıcı çalıştırın.

    ```cmd
    docker run --name createcertificates --rm -v c:\edgeCertificates:/edgeCertificates createcertificates /edgeCertificates
    ```

10. Docker, erişim için ister **c:\\**  sürücü. Seçin **paylaşın**.

11. İstendiğinde kimlik bilgilerinizi sağlayın.

12. Çalıştıran, bir kez kapsayıcı tamamlandığında aşağıdaki dosyaları onay **c:\\edgeCertificates**:

    * C:\\edgeCertificates\\sertifikaları\\azure-IOT-test-only.root.ca.cert.pem
    * C:\\edgeCertificates\\sertifikaları\\yeni-edge-cihaz-tam-chain.cert.pem
    * C:\\edgeCertificates\\sertifikaları\\edge device.cert.pem yeni
    * C:\\edgeCertificates\\sertifikaları\\edge device.cert.pfx yeni
    * C:\\edgeCertificates\\özel\\edge device.key.pem yeni

## <a name="upload-certificates-to-azure-key-vault"></a>Azure anahtar Kasası'na sertifikaları karşıya yükle

Bizim sertifikaları güvenli bir şekilde saklayın ve birden çok cihaz erişilebilir hale getirmek için sertifikaları Azure Key Vault'a yükleyeceksiniz. Yukarıdaki listeden görebileceğiniz gibi iki sertifika dosya türlerini sunuyoruz: PFX ve PEM. Key Vault Certificates, anahtar Kasası'na yüklenecek olarak PFX koyacağız. PEM dosyaları olan düz metin ve bunları Key Vault gizli dizileri koyacağız. Oluşturduğumuz çalıştırarak Azure Machine Learning hizmeti çalışma alanı ile ilişkilendirilmiş Key Vault kullanacağız [Azure not defterleri](tutorial-machine-learning-edge-04-train-model.md#run-azure-notebooks).

1. Gelen [Azure portalında](https://portal.azure.com), Azure Machine Learning hizmeti çalışma alanınıza gidin.

2. Azure Machine Learning hizmeti çalışma alanında genel bakış sayfasından adını bulun **Key Vault**.

    ![Anahtar kasası adı Kopyala](media/tutorial-machine-learning-edge-05-configure-edge-device/find-key-vault-name.png)

3. Geliştirme makinenizde, anahtar Kasası'na sertifikaları karşıya yükleyin. Değiştirin **\<Subscriptionıd\>** ve **\<keyvaultname\>** kaynak bilgilerinizle.

    ```powershell
    c:\source\IoTEdgeAndMlSample\CreateCertificates\upload-keyvaultcerts.ps1 -SubscriptionId <subscriptionId> -KeyVaultName <keyvaultname>
    ```

4. İstenirse, Azure'da oturum açın.

5. Komut dosyası için birkaç dakika yeni Key Vault girişleri listeler çıkış ile çalışır.

    ![Key Vault betik çıktısı](media/tutorial-machine-learning-edge-05-configure-edge-device/key-vault-entries-output.png)

## <a name="create-iot-edge-device"></a>IoT Edge cihazı oluşturma

Azure IOT Edge cihazı IOT hub'a bağlanmak için öncelikle bir cihaz için bir kimlik hub'ı oluştururuz. Biz bulutta cihaz kimliğini bağlantı dizesini alın ve çalışma zamanı, IOT Edge cihazında yapılandırmak için kullanın. Cihaz yapılandırıldı ve hub'a bağlanan sonra modülleri dağıtma ve iletiler gönderebilirsiniz duyuyoruz. Ayrıca fiziksel IOT Edge cihazı yapılandırmasına karşılık gelen bir cihaz kimliği IOT hub'ında yapılandırmasını değiştirerek Değiştirebiliriz.

Bu öğreticide, Visual Studio Code kullanarak yeni bir cihaz kimliği oluştururuz. Ayrıca, kullanarak aşağıdaki adımları tamamlayabilirsiniz [Azure portalında](how-to-register-device-portal.md), veya [Azure CLI](how-to-register-device-cli.md).

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. Açık **Azure IOT Hub cihazları** Visual Studio Code Gezgini görünümünde çerçeve.

3. Seçin ve üç nokta simgesine tıklayarak **IOT Edge cihazı oluşturma**.

4. Cihaz, bir ad verin. Kolaylık olması için kullandığımız **aaTurbofanEdgeDevice** önceden oluşturduğumuz önceki test verileri göndermek için cihaz bandı tüm istemci aygıtları sıralar için.

5. Yeni cihaz, cihaz listesinde görünür.

    ![VS Code Gezgini'nde yeni aaTurbofanEdgeDevice görüntüle](media/tutorial-machine-learning-edge-05-configure-edge-device/iot-hub-devices-list.png)

## <a name="deploy-azure-virtual-machine"></a>Azure sanal makine dağıtma

Kullandığımız [Azure IOT Edge üzerinde Ubuntu'da](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) Bu öğreticide, IOT Edge cihazı oluşturmak için Azure Market görüntüsünden. Ubuntu görüntüsünü Azure IOT Edge, başlangıçta en son Azure IOT Edge çalışma zamanı ve onun bağımlılıklarını yükler. Biz bir PowerShell betiğini kullanarak VM'yi dağıtın `Create-EdgeVM.ps1`; bir Resource Manager şablonu `IoTEdgeVMTemplate.json`; ve bir kabuk betiği `install packages.sh`.

### <a name="enable-programmatic-deployment"></a>Programlamalı dağıtımı etkinleştir

Komut dosyası kullanan bir dağıtımda Market görüntüsü kullanmak için görüntünün programlamalı dağıtımını etkinleştirmek ihtiyacımız var.

1. Azure Portal’da oturum açın.

1. **Tüm Hizmetler**’i seçin.

1. Arama çubuğunda girin ve seçin **Market**.

1. Arama çubuğunda girin ve seçin **Azure IOT Edge üzerinde Ubuntu'da**.

1. Seçin **programlı bir şekilde dağıtmak istiyorsunuz? Başlama** köprü.

1. Seçin **etkinleştirme** düğmesine **Kaydet**.

    ![VM için programlamalı dağıtımı etkinleştir](media/tutorial-machine-learning-edge-05-configure-edge-device/deploy-ubuntu-vm.png)

1. Başarı bildirimini görürsünüz.

### <a name="create-virtual-machine"></a>Sanal makine oluşturma

Ardından, IOT Edge cihazınız için sanal makine oluşturmak için betiği çalıştırın.

1. Bir PowerShell penceresi açın ve gidin **EdgeVM** dizin.

    ```powershell
    cd c:\source\IoTEdgeAndMlSample\EdgeVM
    ```

2. Sanal makine oluşturmak için betiği çalıştırın.

    ```powershell
    .\Create-EdgeVm.ps1
    ```

3. İstendiğinde, her parametre için değerler sağlayın. Bu öğretici boyunca tüm kaynaklar için sahip olduğunuz abonelik için kaynak grubu ve konumda öneririz aynı kullanın.

    * **Azure abonelik kimliği**: Azure portalında bulunamadı
    * **Kaynak grubu adı**: Bu öğretici için kaynakları gruplandırma için akılda kalıcı adı
    * **Konum**: Sanal makine oluşturulacağı azure konumu. Örneğin, westus2 veya northeurope. Daha fazla bilgi için tüm görmek [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).
    * **AdminUsername**: yönetici hesabı adı sanal makineye oturum açmak için kullanacaksınız
    * **AdminPassword**: sanal makinede AdminUsername için ayarlanacak parola

4. Betik VM ayarlama yapmak için kullandığınız Azure aboneliği ile ilişkili kimlik bilgileri ile Azure'da oturum açmanız gerekir.

5. Betik bilgileri, sanal Makinenizin oluşturulması için onaylar. Seçin **y** veya **Enter** devam etmek için.

6. Aşağıdaki adımları yürütme sırasında birkaç dakikalığına komut dosyasını çalıştırır:

    * Zaten yoksa kaynak grubu oluşturun
    * Sanal makineyi oluşturma
    * VM için 22 (SSH) (AMQP), 5671, 5672 (AMPQ) noktaları için NSG özel durumlarını ekleyin ve 443 (SSL)
    * Yükleme [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest))

7. Betik VM bağlanmak için SSH bağlantı dizesi çıkarır. Sonraki adım için bağlantı dizesini kopyalayın.

    ![VM için SSH bağlantı dizesini kopyalayın](media/tutorial-machine-learning-edge-05-configure-edge-device/vm-ssh-connection-string.png)

## <a name="connect-to-your-iot-edge-device"></a>IOT Edge Cihazınızı bağlama

Sonraki birkaç bölümlerde oluşturduğumuz Azure sanal makineyi yapılandırın. İlk adım, sanal makineye bağlamaktır.

1. Bir komut istemi açın ve komut dosyası çıktısından kopyaladığınız SSH bağlantı dizesini yapıştırın. Önceki bölümdeki PowerShell Betiği için kullanıcı adı ve soneki bölgeye göre sağlanan değerler için kendi bilgilerinizi girin.

    ```cmd
    ssh -l <username> iotedge-<suffix>.<region>.cloudapp.azure.com
    ```

2. Konak özgünlüğünü doğrulamak için istendiğinde yazın **Evet** seçip **Enter**.

3. İstendiğinde parolanızı girin.

4. Ubuntu Hoş Geldiniz iletisi görüntüler ve ardından bir komut istemi benzer görmelisiniz. `<username>@<machinename>:~$`.

## <a name="download-key-vault-certificates"></a>Key Vault sertifikaları indir

Bu makalenin önceki bölümlerinde Key Vault, bizim IOT Edge cihazı ve IOT Edge cihazı IOT Hub ile iletişim kurmak için bir ağ geçidi olarak kullanan bir aşağı akış cihaz bizim yaprak cihaz için kullanılabilir hale getirmek için sertifikaları karşıya. Biz, öğreticinin ilerleyen bölümlerinde yaprak cihazı ile ilgilenecektir. Bu bölümde, IOT Edge cihazına bir sertifika indirin.

1. Linux sanal makinesinde SSH oturumunda, Azure CLI ile azure'da oturum açın.

    ```bash
    az login
    ```

1. Bir tarayıcıda istenir <https://microsoft.com/devicelogin> ve benzersiz bir kod sağlar. Yerel makinenizde aşağıdaki adımları gerçekleştirebilirsiniz. İşiniz bittiğinde tarayıcı penceresini kapatın. kimlik doğrulama.

1. Başarıyla doğrulandığında, Linux VM oturum açın ve Azure aboneliklerinizi listeler.

1. Azure CLI komutları için kullanmak istediğiniz Azure aboneliğini ASet.

    ```bash
    az account set --subscription <subscriptionId>
    ```

1. Sertifikalar için VM üzerinde bir dizin oluşturun.

    ```bash
    sudo mkdir /edgeMlCertificates
    ```

1. Anahtar Kasası'nda depolanan sertifikaları indirin: azure-IOT-test-only.root.ca.cert.pem yeni-edge-cihaz-tam-chain.cert.pem ve edge device.key.pem yeni

    ```bash
    key_vault_name="<key vault name>"
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-full-chain-cert-pem -f /edgeMlCertificates/new-edge-device-full-chain.cert.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-key-pem -f /edgeMlCertificates/new-edge-device.key.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name azure-iot-test-only-root-ca-cert-pem -f /edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem
    ```

## <a name="update-the-iot-edge-device-configuration"></a>IOT Edge cihaz yapılandırmasını güncelleştirme

IOT Edge çalışma zamanı dosya /etc/iotedge/config.yaml yapılandırmasını kalıcı hale getirmek için kullanır. Üç parça bilgi bu dosyadaki güncelleştirmeniz gerekir:

* **Cihaz bağlantı dizesini**: IOT hub'ında bu cihazın kimliğini alınan bağlantı dizesi
* **Sertifikaları:** aşağı akış cihazlarla yapılan bağlantılar için sertifikaları
* **Ana bilgisayar adı:** VM IOT Edge cihazı tam etki alanı adını (FQDN).

*Azure IOT Edge üzerinde Ubuntu'da* IOT Edge VM oluşturmak için kullanılan görüntü config.yaml bağlantı dizesiyle güncelleştiren bir kabuk betiği ile birlikte sunulur.

1. Visual Studio code'da IOT Edge cihazında sağ tıklayın ve ardından **kopyalama cihaz bağlantı dizesini**.

    ![Visual Studio Code'dan bağlantı dizesini kopyalayın](media/tutorial-machine-learning-edge-05-configure-edge-device/copy-device-connection-string-command.png)

2. SSH oturumunuzda, cihaz bağlantısı dizeniz ile config.yaml dosyayı güncelleştirmek için komutu çalıştırın.

    ```bash
    sudo /etc/iotedge/configedge.sh "<your_iothub_edge_device_connection_string>"
    ```

Sonra sertifikaları ve ana bilgisayar adı config.yaml doğrudan düzenleyerek güncelleştireceğiz.

1. Config.yaml dosyasını açın.

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

2. Önde gelen kaldırarak config.yaml sertifikaları bölümünü güncelleştirme `#` ve dosyayı aşağıdaki örnekteki gibi görünür yol ayarlama:

    ```yaml
    certificates:
      device_ca_cert: "/edgeMlCertificates/new-edge-device-full-chain.cert.pem"
      device_ca_pk: "/edgeMlCertificates/new-edge-device.key.pem"
      trusted_ca_certs: "/edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem"
    ```

    Emin olun "sertifikaları:" hiçbir önceki boşluk ve her sertifikaların tarafından iki boşluk önünde.

    Nano sağ, geçerli imleç konumu panonuza içeriğini yapıştırın. Dize değiştirmek için değiştirmek için dize silmek istediğiniz dizeyi gidin, klavye oklarını kullanın ve arabellekteki yapıştırmak için sağ tıklatın.

3. Azure portalında, sanal makineye gidin. DNS adı (FQDN makinenin) kopyalama **genel bakış** bölümü.

4. FQDN config.yml hostname bölümüne yapıştırın. Adın tamamı küçük harf olduğundan emin olun.

    ```yaml
    hostname: '<machinename>.<region>.cloudapp.azure.com'
    ```

5. Dosyasını kaydedin ve kapatın (`Ctrl + X`, `Y`, `Enter`).

6. İotedge Daemon programını yeniden başlatın.

    ```bash
    sudo systemctl restart iotedge
    ```

7. IOT Edge Daemon durumunu denetleyin (komuttan sonra girin ": q" çıkmak için).

    ```bash
    systemctl status iotedge
    ```

8. Hatalar görürseniz (metin ön ekine sahip renkli "\[hata\]") durumu inceleyin arka plan programı günlüklerinde ayrıntılı hata bilgileri.

    ```bash
    journalctl -u iotedge --no-pager --no-full
    ```

## <a name="next-steps"></a>Sonraki adımlar

Yalnızca bir Azure sanal makinesi Azure IOT Edge ağ geçidi saydam yapılandırma tamamlandı. Azure anahtar Kasası'na dosyamızı test sertifikaları oluşturarak Başladık. Ardından, bir komut dosyası ve Resource Manager şablonu "Ubuntu Server 16.04 LTS + Azure IOT Edge çalışma zamanı" ile VM dağıtma için kullandığımız Azure Market görüntüsünden. Azure CLI Yükleme ek adıma betiğin ([apt ile Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli-apt)). Çalışan ve VM'yi biz SSH yoluyla bağlı, Azure'da oturum açmış, Key Vault'tan sertifikalar indirilen ve çeşitli güncelleştirmeler config.yaml dosyasını güncelleştirerek IOT Edge çalışma zamanı yapılandırması için yapılan. IOT Edge ağ geçidi olarak kullanma hakkında daha fazla bilgi için bkz. [nasıl bir IOT Edge cihazı ağ geçidi olarak kullanılabilir](iot-edge-as-gateway.md). Saydam bir ağ geçidi olarak IOT Edge cihazı yapılandırma hakkında daha fazla bilgi için bkz. [saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md).

IOT Edge modüllerini oluşturmak için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Oluşturma ve dağıtma özel IOT Edge modülleri](tutorial-machine-learning-edge-06-custom-modules.md)
