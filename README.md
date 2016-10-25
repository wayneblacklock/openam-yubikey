## OpenAM Yubikey Authentication Module

(With much thanks to https://github.com/ronalders)

Authentication module for use with ForgeRock OpenAM (http://forgerock.com/)

Currently being updated to work with OpenAM 13.5 

## Yubikeys

The module validated Yubikey OTP agains Yubico YubiCloud validation platform
See: http://www.yubico.com for more details

For valdition of OTP is depends onYubico Java client v2 (currently 2.0.1). 
See: https://github.com/Yubico/yubico-java-client

By default all parts use the Yubico YubiCloud validation platform, but can
be configured for another validaiton server. To use YubiCloud you need a
client id and an API key that can be fetched from
https://upgrade.yubico.com/getapikey/. 

The first 12 characters of a Yubikey is it's identifier. This is what should be entered in the Yubikey attribute below and is used to map to users in OpeAM.

A Yubikey ID value might look like:

ccccdferttrsdsfjsdoufsdfdf

Yubikey ID: ccccdferttrs 
OTP: dsfjsdoufsdfdf

## Installation Instructions

* mvn install
* /usr/local/env/box/tools/ssoadm/openam/bin/ssoadm create-svc -u amadmin -f passwd.txt --xmlfile 'src/main/resources/amAuthYubikeyModule.xml'
* /usr/local/env/box/tools/ssoadm/openam/bin/ssoadm register-auth-module -u amadmin -f passwd.txt --authmodule nl.alders.openam.YubikeyModule
* cp target/openam-yubikey-1.0.0-SNAPSHOT-jar-with-dependencies.jar /usr/local/env/box/tomcatam/webapps/openam/WEB-INF/lib
* Get an Yubikey API key: https://upgrade.yubico.com/getapikey/
* Configure module: Realm -> Authentication -> Modules: Add the Yubikey module
Yubikey Client ID: Client ID from Yubikey API
Yubikey API key (Secret): Secret API key from Yubikey API
Yubikey attribute name: The OpenAM attribute that contains the YubikeyID, used to match the Yubikey to users in OpenAM e.g. demo should have an attribute like "ccccdferttrs
Yubikey validation server(s): Should be pre-configured
Authentication Level: Standard auth level ( 0 is fine )
* Create a chain with the Datastore first and the Yubikey module second