# Create-User-in-Openshift-by-Htpasswd-IdentityProvider

------------------------------------------------------------------------------------------------------
# Step1: Retrive Htpasswd File from secret (i.e htpasswd-secret) located in "openshift-config" project

[ ]# oc extract secret/htpasswd-secret -n openshift-config --to /root/userdemo/htpasswd 

Note:
htpasswd <--- just a file name you can give any name to file while extracting 

------------------------------------------------------------------------------------------------------
# Step2: Add new user in retrived "htpasswd" file 

[ ]# htpasswd -bB /root/userdemo/htpasswd username password

------------------------------------------------------------------------------------------------------
# Step3: Update the original secret (i.e. htpasswd-secret) with new user detail

[ ]# oc set data secret/htpasswd-secret --from-file htpasswd=/root/demouser/htpasswd -n openshift-config

------------------------------------------------------------------------------------------------------
# Login with new user

[ ]# oc login -u username -p password
Login Successfull

That's all...
