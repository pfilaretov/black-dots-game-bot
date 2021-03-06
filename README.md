# BlackDotsGameBot
This Telegram bot provides some simple games with notes. These small black dots, you know.

## How to run
* set `BLACK_DOTS_GAME_BOT_CREATOR_ID` environment variable to your Telegram ID
* set `BLACK_DOTS_GAME_BOT_TOKEN` environment variable to the token value generated by BotFather
* set `BLACK_DOTS_GAME_BOT_BASE_URL` environment variable to the base URL of the game
* generate PKCS12 keystore with a certificate or convert PEM files generated by Let's Encrypt Certbot using 
  ```
  openssl pkcs12 -export -in fullchain.pem \
                 -inkey privkey.pem \
                 -out keystore.p12 \
                 -name <BLACK_DOTS_GAME_BOT_KEY_ALIAS> \
                 -CAfile chain.pem \
                 -caname root
  ```
  Set keystore password when requested.
* set `BLACK_DOTS_GAME_BOT_KEYSTORE` environment variable to the PKCS12 keystore path.
* set `BLACK_DOTS_GAME_BOT_KEYSTORE_PASSWORD` environment variable to the keystore password.
* set `BLACK_DOTS_GAME_BOT_KEY_ALIAS` to the key alias.
* build:
  ```
  mvn clean package
  ```
  Output is in the `target` folder.
* run:
  * locally for development:
    ```
    java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar -Dspring.profiles.active=local black-dots-game-bot-1.0.0.war
    ```
    Open browser at https://localhost:7443/index.html
  * on prod env:
    ```
    sudo java -jar -Dspring.profiles.active=prod black-dots-game-bot-1.0.0.war &
    ```

## How to renew Let's Encrypt certificate
**TODO - automate (using certbot-renew.timer?)**

* get new certificate 
  ```
  sudo systemctl start certbot-renew
  ```
* stop application (kill java process)
* login as root (or use full paths for files) and update PKCS12 keystore:
  ```
  openssl pkcs12 -export -in fullchain.pem \
                 -inkey privkey.pem \
                 -out keystore.p12 \
                 -name <BLACK_DOTS_GAME_BOT_KEY_ALIAS> \
                 -CAfile chain.pem \
                 -caname root
  ```
* start application
  ```
  sudo java -jar -Dspring.profiles.active=prod black-dots-game-bot-1.0.0.war &
  ```