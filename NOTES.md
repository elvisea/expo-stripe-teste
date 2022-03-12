Acessar a pasta `android`

```bash
cd android
``` 

Rodar o seguinte comando:

```bash
keytool -genkeypair -v -storetype PKCS12 -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```
Insira uma senha `123456`.

Responda `sim` quando precisar.

Foi gerado a chave `my-upload-key.keystore` na pasta `android`.

Dentro da pasta `android` no arquivo `gradle.properties` inserir as seguintes credenciais:

```bash
MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
MYAPP_UPLOAD_STORE_PASSWORD=123456
MYAPP_UPLOAD_KEY_PASSWORD=123456
```

Agora acessar a seguinte pasta:

```bash
cd android/app/
```

Acessar o arquivo `build.gradle` e dentro da chave `signingConfigs` inserir o seguinte: 

```gradle
release {
          if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
              storeFile file(MYAPP_UPLOAD_STORE_FILE)
              storePassword MYAPP_UPLOAD_STORE_PASSWORD
              keyAlias MYAPP_UPLOAD_KEY_ALIAS
              keyPassword MYAPP_UPLOAD_KEY_PASSWORD
          }
      }
```

Alterar também dentro a chave `buildTypes` dentro de `release` a seguinte informação:

```gradle
signingConfig signingConfigs.debug
``` 

para:

```gradle
signingConfig signingConfigs.release
```

## Generating the release AAB

Execute o seguinte em um terminal:

```bash
cd android
./gradlew bundleRelease
```

## Testando a versão de lançamento do seu aplicativo
Antes de enviar a versão de lançamento para a Play Store, certifique-se de testá-la completamente. Primeiro desinstale qualquer versão anterior do aplicativo que você já instalou. Instale-o no dispositivo usando o seguinte comando na raiz do projeto:

```bash
npx react-native run-android --variant=release
```

