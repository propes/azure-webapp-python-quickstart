# Azure App Service Web App quickstart using python

1. Set up a virtual environment and install Flask

```sh
python3 -m venv venv
source venv/bin/activate
pip install flask
```

2. Set up a project folder

```sh
mkdir ~/BestBikeApp
cd ~/BestBikeApp
```

3. Create the application file

```sh
code application.py
```

```py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "<html><body><h1>Hello Best Bike App!</h1></body></html>\n"
```

4. Create an application requirements file

```sh
pip freeze > requirements.txt
```

5. Test locally

```sh
cd ~/BestBikeApp
export FLASK_APP=application.py
flask run
```

From a second terminal:
```sh
curl http://127.0.0.1:5000/
```

6. Set environment variables from azure (assumes only one webapp)

```sh
export APPNAME=$(az webapp list --query [0].name --output tsv)
export APPRG=$(az webapp list --query [0].resourceGroup --output tsv)
export APPPLAN=$(az appservice plan list --query [0].name --output tsv)
export APPSKU=$(az appservice plan list --query [0].sku.name --output tsv)
export APPLOCATION=$(az appservice plan list --query [0].location --output tsv)
```

7. Deploy the app to azure app service

```sh
cd ~/BestBikeApp
az webapp up --name $APPNAME --resource-group $APPRG --plan $APPPLAN --sku $APPSKU --location "$APPLOCATION"
```