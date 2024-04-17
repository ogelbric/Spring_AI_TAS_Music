# Spring_AI_TAS_Music
```
This is a demo of Spring and AI libraries and TAS as the host.
The example is using a music album application.  
```

## Cloud Foundry Install (for Mac)
```
https://github.com/cloudfoundry/cli/wiki/V7-CLI-Installation-Guide
brew install cloudfoundry/tap/cf-cli@7
```

## Login to TAS demo environment
```
cf api https://api.sys.dhaka.cf-app.com
cf login --sso
```
![Version](https://github.com/ogelbric/Spring_AI_TAS_Music/blob/main/login2.png)

## Temporary Authentication Code 
```
https://login.sys.dhaka.cf-app.com/passcode
	login with Broadcom OKTA! (abc@br.xxx)
	Then use give code on the cf login -sso line 
```

## Create ORG and Space
```
export USER="abc.def@br.com"  #Since the display told to use the Broadcom dot com name not dot net
export ORG_NAME=$USER 
export SPACE_NAME="development"
cf create-org "$ORG_NAME"
cf target -o "$ORG_NAME"
cf create-space "$SPACE_NAME"
cf target -s "$SPACE_NAME"
```

## Java (need version 17)(Mac)
```
java -version
sdk install java 17.0.1.fx-librca
```

## Get App
```
mkdir kirti
cd kirti
git clone https://github.com/nkuhn-vmw/GenAI-for-TAS-Samples.git
cd GenAI-for-TAS-Samples 
cd spring-music-taylors-version
vi manifest.yml and -{Some unique name here} to the name field

Change from:
- name: spring-music-ai-taylors-version-tanzu-live
Change to:
- name: spring-music-ai-taylors-version-tanzu-live-{Some unique name here} i.e. in my case: - name: spring-music-ai-taylors-version-tanzu-live-orf

./gradlew build

( I had to run this cf push command several times to over come time outs and "deploy issues" - it also ran for some time)
cf push
```

## Market place apps 
```
cf marketplace
cf create-service postgres on-demand-postgres-small tanzu-gpt-postgres
cf create-service genai-service shared-ai-plan tanzu-gpt-genai-service
```
## See the apps deployed
```
cf services

name                      service         plan                       bound apps   last operation     broker          upgrade available
tanzu-gpt-genai-service   genai-service   shared-ai-plan                          create succeeded   genai-service   
tanzu-gpt-postgres        postgres        on-demand-postgres-small                create succeeded   postgres-odb    no
```

## Bind the apps to my app and re-stage
```
cf bind-service spring-music-ai-taylors-version-tanzu-live-orf tanzu-gpt-genai-service
cf bind-service spring-music-ai-taylors-version-tanzu-live-orf tanzu-gpt-postgres
cf restage spring-music-ai-taylors-version-tanzu-live-orf
cf restage spring-music-ai-taylors-version-tanzu-live-orf


cf services                                                                           

name                      service         plan                       bound apps                                       last operation     broker          upgrade available
tanzu-gpt-genai-service   genai-service   shared-ai-plan             spring-music-ai-taylors-version-tanzu-live-orf   create succeeded   genai-service   
tanzu-gpt-postgres        postgres        on-demand-postgres-small   spring-music-ai-taylors-version-tanzu-live-orf   create succeeded   postgres-odb    no
```

## Lets check things out in TAS Apps Manager the restults

```
https://apps.sys.dhaka.cf-app.com/
```

## Jump to application from TAS Apps Manager




## Delete old not working my app
```
it had routing issues since the name of the app conflicted with other users in the org!
cf delete spring-music-ai-taylors-version-tanzu-live
```
