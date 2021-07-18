# MoneySprouts

## Background 🦋
This full-stack app was realized during a four-week long team sprint during the immersive coding bootcamp at
[Code Chrysalis](https://www.codechrysalis.io)

## Summary

🪴🌱🪴🌱🪴🌱🪴🌱🪴🌱

**Get in the black, by going green!** 

🪴🌱🪴🌱🪴🌱🪴🌱🪴🌱

MoneySprouts is a progressive web app that helps its users to **save money** and **remind them to accomplish their personal environmental goals** at the same time.

### Current Features Include:
* Environmental facts reminder 🌲
* Monthly budget setting 💰
* Monthly savings target setting 💸
* Expense input 🙈
* Eco-action input 🏅

## Links
* This app was part of a public presentation streamed live [on Youtube](https://www.youtube.com/watch?v=522BJezfQ5Y) on June 17th 2021. (Presentation starts at 52:59)
* The app is released on AWS at the following link: [MoneySprouts](https://www.moneysprouts.net/)

![app demonstration](moneysproutsdemo.gif)

## Tech Stack 🧑‍🔬
### Front-end 📲
* The front-end was designed with [Vue.js](https://vuejs.org/) and [VueX](https://vuex.vuejs.org/)
* The charts are based on [Chart.js](https://www.chartjs.org/) and [VueChart.js](https://vue-chartjs.org/)

### Back-end 📂
* The back-end database is using [PostgreSQL](https://www.postgresql.org/)
* The back-end API server is based on **Python**, with [Flask](https://pypi.org/project/Flask/) framework for API, and [SQLAlchemy](https://www.sqlalchemy.org/) for DB interaction.
* In production, we are using the following tech:
    * [Gunicorn](https://gunicorn.org/) for WSGI server
    * [NGINX](https://nginx.org/en/) for reverse proxy load balancing and static files servicing
* The app was deployed on AWS EC2

## Team members 👱👱👩🧑‍🦱👱

| Staff name | Role |
| ---------- | -------- |
| [Michael](https://github.com/michael-metcalf) | Tech lead, front-end |
| [Alix](https://github.com/AlixFachin) | Full-stack | 
| [David](https://github.com/DavidofOrange) | Full-stack |
| [Julie](https://github.com/dawndarkness) | Front-end |
| [Russell](https://github.com/RussellPacheco) | Full-stack |

## Future Features
The future features are listed on the Github repo in the "Issues" section.


# Getting the app Up and Running on local environment 🏃
In order to run the app on a local machine, you need to:
1. download / clone the repository
1. run the back-end server with the proper Python configuration (more details below)
1. run the front-end development server after having installed the required libraries and dependencies

## The Backend Environment 

1. Clone Repo

1. Create a virtual environment

    ```
    python3 -m venv <FOLDERNAME>
    ```

1. Start the virtual environment

    <details><summary><b>For Mac Users</b></summary>

    ```
    source /<VENV FOLDER>/bin/activate
    ```

    If you are using the M1 chip, you may need to use python 3.7 to be able to run the environment.
    </details>

    <details><summary><b>For Windows Users</b></summary>

    Navigate to where your virtual environment folder is, and within that folder you should find another folder called Scripts, and within that a number of files. To start the virtual environment, you would need to run either the `activate.bat` or `activate.ps1`. To run, just type one of these files in your terminal, and press enter. If the start of the VM was successful, you should see `(<FOLDER NAME>)` printed in your terminal. This would be written before the PATH. 

    Run one of the following based on the following requirements:

    if using command prompt:

    ```
    \<VENV FOLDER>\Scripts\activate.bat
    ```

    or 

    if using powershell:

    ```
    Run \<VENV FOLDER>\Scripts\Activate.ps1
    ```
    </details>

1. Install all dependencies

    ```
    pip install -r requirements.txt
    ```

1. Ensure that your IDE's Python Interpretor is set to the interpretor located in your virtual environment folder. This 
would be either in the `Scripts` folder if you are a Windows user, or in the `bin` folder for Linux and Mac users. The interpretor
would be the `python.exe` file. Have this set with your IDE. 


1. Create config for the flask development server
Create a '.flaskenv' file in the be-server folder and set it's environment variables. 

    ```
    FLASK_APP=main.py
    FLASK_ENV=development
    ```

1. Start Flask
Make sure you are in the be-server directory!!! 

    ```
    flask run
    ```


1. Create Initial Migration File

    ```
    alembic revision --autogenerate -m "initial"
    ```

1. Seed

    ```
    alembic upgrade head
    ```

## The Frontend Environment

### fe-moneysprouts

The frontend environment must be set up from within the fe-moneysprouts folder.

### Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```
### Customize configuration
See Vue CLI [Configuration Reference](https://cli.vuejs.org/config/).

## About deployment on AWS

To deploy in prod, we need to have a proper HTTPS server, which will be `nginx`.
`Ngninx` will be serving the static files (e.g. the `Vue` app) and will communicate with Python for the API, 
enabling the front-end to discuss with the back-end.

However `Nginx` and Python don't know how to speak together, so we need to put a server in the middle, which 
will be `gunicorn`.

## How to setup and restart nginx
We need to restart nginx server each time we make some changes to the server config files or to the back-end.
* The server config file is in the `/etc/nginx/sites-enabled` folder.
* There is another config file in the `/etc/nginx/sites-available` folder. As this second file is the same than 
the file above, we created a symbolic link to, well, link both files so that only the file in `sites-enabled` have
to be modified.
* To restart `nginx` (after having modified the conf.d file, for example) we just need to run `sudo systemctl restart nginx`
* To see the current status of `nginx`, we can do `service nginx status`


## Credits

* Icons were created using [font awesome](https://fontawesome.com/)
* Image credit goes to [Leo Wieling](https://unsplash.com/photos/gaP0QDorAj8)