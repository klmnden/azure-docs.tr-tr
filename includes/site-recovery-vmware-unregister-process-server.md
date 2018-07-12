Bir işlem sunucusunun kaydını kaldırma adımları, Yapılandırma Sunucusu’ndaki bağlantı durumuna bağlı olarak farklılık gösterir.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Bağlı durumdaki işlem sunucusunun kaydını kaldırma

1. İşlem sunucusunda Yönetici olarak uzaktan oturum açın.
2. **Denetim Masası**’nı başlatıp **Programlar > Program kaldır**’ı açın
3. **Microsoft Azure Site Recovery Yapılandırması/İşlem Sunucusu** adlı programı kaldırın
4. Adım 3 tamamlandıktan sonra **Microsoft Azure Site Recovery Yapılandırması/İşlem Sunucusu Bağımlılıkları**’nı kaldırabilirsiniz

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Bağlantısı kesik durumdaki işlem sunucusunun kaydını kaldırma

> [!WARNING]
> İşlem Sunucusunun yüklü olduğu sanal makinenin canlandırılması mümkün değilse aşağıdaki adımlar kullanılmalıdır.

1. Yapılandırma sunucunuzda yönetici olarak oturum açın.
2. Bir Yönetici komut istemi açın ve `%ProgramData%\ASR\home\svsystems\bin` dizinine gidin.
3. Şimdi komutu çalıştırın.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Bunun yapılması, işlem sunucusunun ayrıntılarını sistemden temizler.
