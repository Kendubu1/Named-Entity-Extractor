## Azure Deployment Steps using Start-Up Script
1. Deploy locally & verify working on localhost
- You will find that "en-core-web-md" will be missing & you will need to install this manually using "python -m spacy download en_core_web_md" while on your local machine. 
2. Initalize a Linux App Service running Python 
3. We will need to create a custom startup script to install the same packages during startup on Azure. | https://docs.microsoft.com/en-us/azure/app-service/containers/how-to-configure-python#customize-startup-command
- I found it easiest to SSH into your container & run the following
```
...Commands...
cd site/
touch startup.sh
vi startup.sh

...SCRIPT...
bash
#!/bin/sh
python -m spacy download en_core_web_md en
gunicorn --bind=0.0.0.0 --timeout 600 app:app

```
4. Add your startup command path from your configuration blade in Azure portal. "/home/site/startup.sh"
- Once saved the app service is expected to fail since spacy is a missing dependency & there is no app.py for gunicorn to reference.
5. Deploy using Local Git | https://docs.microsoft.com/en-us/azure/app-service/deploy-local-git
6. Test your site! | Example: https://named-entity-extractor.azurewebsites.net
### Repository Modifications 

| Files             |  Content                                   |
|----------------------|--------------------------------------------|
| `requirements.txt`           | Added requirement.txt in order for Azure to build the app with Oryx since it looks for a requirements.txt to build all dependencies              |



# Named-Entity-Extractor
A simple Flask API for named entity extraction using spaCy Model
