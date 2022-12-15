# Create-User-in-Openshift-by-Htpasswd-IdentityProvider

Before jumping on the process, create some backup file to restore if somthing no works properly 
so we can restore the setting later.

[ ]# oc get oauth cluster -o yaml > oauth.yaml

------------------------------------------------------------------------------------------------------
# Step1: Create Htpasswd file in system

[ ]# htpasswd -cb htpasswd username1 password1

htpasswd <--- first one is command
              second "htpasswd" just a file name you can give any name to file 

------------------------------------------------------------------------------------------------------
# Step2: Add new multiple user inside "htpasswd" file after creating htpasswd file

[ ]# htpaswwd -b htpasswd username2 password2

------------------------------------------------------------------------------------------------------
# Step3: Create a secret in "openshift-config" project/namespace

[ ]# oc create secret generic demo_secret --from-file htpasswd=/root/demouser/htpasswd -n openshift-config

------------------------------------------------------------------------------------------------------
# Step4: Edit Oauth resource by yml file and update the oauth settings

[ ]# oc get oauth cluster -o yaml > oauth.yml
[ ]# vim oauth.yml
apiVersion: config.openshift.io/v1
kind: OAuth
|
|
|
|
|
spec:
  identityProviders:
  - name: give_any_name_to_identity_provider
    mappingMethod: claim
    type: HTPasswd
    htpasswd:
      fileData:
        name: demo_secret

[ ]# oc replace -f oauth.yml

Note:
now wait for some time or you can check Pods from "openshift-authentication" namespace they restart automatically, 
after these Pods restart completely you can able to login with your new user credential in the opnshift cluster. 

------------------------------------------------------------------------------------------------------
# Login with new user

[ ]# oc login -u username -p password
Login Successfull

That's all...
