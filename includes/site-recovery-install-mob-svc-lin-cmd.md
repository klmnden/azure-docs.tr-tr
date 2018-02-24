1. Yükleyici, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, tmp) kopyalayın. Bir terminal, aşağıdaki komutları çalıştırın:
  ```
  cd /tmp ;

  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Mobilite hizmetinin yüklenmesi için aşağıdaki komutu çalıştırın:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Yükleme tamamlandıktan sonra Mobility hizmetinin yapılandırma sunucusuna kayıtlı gerekir. Mobility hizmeti ile yapılandırma sunucusunu kaydetmek için aşağıdaki komutu çalıştırın:

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Mobility Hizmeti Yükleyici komut satırı

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|-r |Zorunlu|Mobility hizmetinin (MS) yüklü olmalıdır veya MasterTarget (MT) yüklenmesi gerektiğini belirtir.|MS </br> MT|
|-d |İsteğe bağlı|Mobility Hizmeti'nin yüklendiği konumu.|/usr/local/ASR|
|-v|Zorunlu|Mobility hizmetinin yüklü olduğu platform belirtir. </br> </br>- **VMware**: Mobility hizmeti üzerinde çalışan bir VM'de yüklerseniz, bu değeri kullanın *VMware vSphere ESXi konakları*, *Hyper-V konakları*, ve *fiziksel sunucuları*. </br> - **Azure**: bir Azure Iaas sanal aracıyı yüklerseniz, bu değeri kullanın.| VMware </br> Azure|
|-q|İsteğe bağlı|Yükleyici sessiz modda çalıştırmak için belirtir.| Yok|


#### <a name="mobility-service-configuration-command-line"></a>Mobility hizmeti yapılandırma komut satırı

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|-i |Zorunlu|Yapılandırma sunucusu IP|Herhangi bir geçerli IP adresi|
|-P |Zorunlu|Burada bağlantısı geçebilmesi tümcecik dosyaları için tam dosya yolu kaydedilir|Geçerli bir klasör|
