# Spring_AI_TAS_Music

This is a demo of Spring and AI libraries and TAS as the host.  

Cloud Foundry Install 
======================
https://github.com/cloudfoundry/cli/wiki/V7-CLI-Installation-Guide
brew install cloudfoundry/tap/cf-cli@7


Login to TAS demo environment
=============================
cf api https://api.sys.dhaka.cf-app.com
cf login --sso

Temporary Authentication Code 
==============================
https://login.sys.dhaka.cf-app.com/passcode
	login with Broadcom OKTA! (og010490@broadcom.net)
	Then use give code on the cf login -sso line 


Create ORG and Space
======================
export USER="orf.gelbrich@broadcom.com"  #Since the display told to use the Broadcom dot com name not dot net
export ORG_NAME=$USER # defaults to your computers username change this value to match your VMWare username if needed
export SPACE_NAME="development"
cf create-org "$ORG_NAME"
cf target -o "$ORG_NAME"
cf create-space "$SPACE_NAME"
cf target -s "$SPACE_NAME"


Java (need version 17)
====
java -version
sdk install java 17.0.1.fx-librca


Get App
========
mkdir kirti
cd kirti
git clone https://github.com/nkuhn-vmw/GenAI-for-TAS-Samples.git
cd GenAI-for-TAS-Samples 
cd spring-music-taylors-version
vi manifest.yml and -orf to the name

./gradlew build
cf push

Market place apps 
=================
cf marketplace
cf create-service postgres on-demand-postgres-small tanzu-gpt-postgres
cf create-service genai-service shared-ai-plan tanzu-gpt-genai-service

See the apps deployed
=====================
cf services
Getting services in org orf.gelbrich@broadcom.com / space development as orf.gelbrich@broadcom.com...

name                      service         plan                       bound apps   last operation     broker          upgrade available
tanzu-gpt-genai-service   genai-service   shared-ai-plan                          create succeeded   genai-service   
tanzu-gpt-postgres        postgres        on-demand-postgres-small                create succeeded   postgres-odb    no


Bind the apps to my app and re-stage
======================================

cf bind-service spring-music-ai-taylors-version-tanzu-live-orf tanzu-gpt-genai-service
cf bind-service spring-music-ai-taylors-version-tanzu-live-orf tanzu-gpt-postgres
cf restage spring-music-ai-taylors-version-tanzu-live-orf
cf restage spring-music-ai-taylors-version-tanzu-live-orf


cf services                                                                           
Getting services in org orf.gelbrich@broadcom.com / space development as orf.gelbrich@broadcom.com...

name                      service         plan                       bound apps                                       last operation     broker          upgrade available
tanzu-gpt-genai-service   genai-service   shared-ai-plan             spring-music-ai-taylors-version-tanzu-live-orf   create succeeded   genai-service   
tanzu-gpt-postgres        postgres        on-demand-postgres-small   spring-music-ai-taylors-version-tanzu-live-orf   create succeeded   postgres-odb    no




Delete old not working my app
=============================

cf delete spring-music-ai-taylors-version-tanzu-live

