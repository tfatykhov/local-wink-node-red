Wink Node-RED Local Starter Application
====================================

### Local Wink Node-RED 
This repository is an modified  Node-RED application that can be deployed locally to use with Wink Node Red flows


#### Installation:
1. Install Node.JS version 4 from https://nodejs.org/download/release/v4.7.2/ (you can try it with latest version as well but I did not test it)

2. run following command to install proper version of node
* npm install local-wink-node-red@https://github.com/tfatykhov/local-wink-node-red.git

This should install all required dependencies.

3. Navigate to node_modules/local-wink-node-red and edit settings.js file. Replace XXX with your actual values
These are user name/password for node-red flow editor
var NODE_RED_USERNAME="XXXXX";
var NODE_RED_PASSWORD="XXXXX";


BlueMixUrlBase: "XXX",  - this is your externally avaliable DNS resolvable host name and port ex - http://external_name:externalport
forecastIoApiKey: "5XXX", forecast.io (darksky.io) key
FREEBOARD_TOKEN: "XXX", randomly generated string that you need to create, you can use https://www.random.org/strings/ 
IFTTT_TOKEN: "XXX", this is ifttt maker channel key.

Once this is done you can start your node-red by running npm start.

your flow editor will be accessible  http://external_name:externalport/red or http://your_local_ip:port/red


#### Securing Node-Red
Once you have that running I would strongly recommend to follow next section and get a proper signed SSL certificate. This is not free service but it costs 5 USD per year. 

1. You definately will need a static domain name. I am using noip.com since it is free and if you have dd-wrt enabled router it can automatically refresh your dynamic ip once it changed by your provider.
If you have an option to get a static IP from your provider - this works as well.

2. Install Open SSL:

For windows - use following article to install openSSL.
https://blog.didierstevens.com/2015/03/30/howto-make-your-own-cert-with-openssl-on-windows/ (use only installation steps)

For linux - openSSL should be already installed as part of the system

3. open following url and order a SSL certificate - https://www.ssls.com/ssl-certificates/comodo-positivessl
Follow instructions provided on the site to generate your private key using Open SSL:
http://helpdesk.ssls.com/hc/en-us/articles/203427502-How-to-generate-a-CSR-code-on-Apache-Nginx-using-OpenSSL (now you can generate your key)

Once you do that and submit your private key, you will be asked to verify that you actually own a web site. There will be multiple options provided. make sure you select one that allows you to verify your website by putting
a specific file to the web site root. You will get a file with some random string inside. Once you have it - stop your node-red instance, copy the file to the "node_modules/local-wink-node-red/public" folder and start node-red again.
It will take some time for certificate authority to verify your web site. Make sure it is accessible from outside your firewall router.
After verification is complete you will see that status of your certificate changed to active and you can download your copy by clicking on Cert #.
After download you will get an zip archive with 3 files inside:

xxxx.p7b
xxxx.crt
xxx.ca-bundle

All these are same certificate in different formats.
You need file with p7b extension and you need to covert it to a .pem format.
In order to do that copy that file as well as your .key file to node_modules/local-wink-node-red folder and then run following command:

    openssl pkcs7 -print_certs -in your_cert_name.p7b -out your_cer_name.pem

once it's complete you will get a xxx.pem file. You need .key and .pem files to make node-red run via https.
in order to do that you need to stop node-red, edit settings.js file, uncomment following section (delete //) and set proper file names:

       https: {
            key: fs.readFileSync('xxx.key'),
            cert: fs.readFileSync('xxx.pem')
       },

after that you need to change your bluemixUrlBase and make it https:// instead of http://
Start node-red again.
If everything is correct and you do not see any errors from now on you will always have to access your node-red instance via https://external_name:externalport or your browser will show you a warning saying that certificate is not correct.
you still can proceed and open the editor though.
