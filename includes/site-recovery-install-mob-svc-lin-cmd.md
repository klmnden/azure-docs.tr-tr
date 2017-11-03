1. Yükleyici, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, tmp) kopyalayın. Bir terminal, aşağıdaki komutları çalıştırın:
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Mobilite hizmetinin yüklenmesi için aşağıdaki komutu çalıştırın:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Yükleme tamamlandıktan sonra Mobility hizmetinin yapılandırma sunucusuna kayıtlı gerekiyor. Mobility hizmetinin yapılandırma sunucusu ile kaydetmek için aşağıdaki komutu çalıştırın.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Mobility hizmeti yükleyicisinin komut satırı

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|-r |Zorunlu|Mobility hizmetinin (MS) yüklü olmalıdır veya MasterTarget(MT) yüklenmesi gerektiğini belirtir|MS </br> MT|
|-d |İsteğe bağlı|Mobility hizmetinin yükleneceği konum|/usr/local/ASR|
|-v|Zorunlu|Mobility hizmetinin yüklendiği platformu belirtir </br> </br>- **VMware** : mobility hizmeti üzerinde çalışan bir VM yüklüyorsanız bu değeri kullanın *VMware vSphere ESXi konakları*, *Hyper-V konakları* ve *Phsyical sunucuları* </br> - **Azure** : bir Azure Iaas VM aracısı yüklüyorsanız, bu değeri kullanın| VMware </br> Azure|
|-q|İsteğe bağlı|Yükleyiciyi sessiz modda çalıştırmak için belirtir| Yok|


#### <a name="mobility-service-configuration-command-line"></a>Mobility hizmeti yapılandırma komut satırı

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|-i |Zorunlu|Yapılandırma sunucusu IP|Herhangi bir geçerli IP adresi|
|-P |Zorunlu|Bağlantı parola kaydedildiği dosyanın tam dosya yolu|Geçerli bir klasör|
