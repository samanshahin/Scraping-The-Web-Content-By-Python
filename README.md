# Scraping-The-Web-Content-By-Python
Hello Everyone!

This is Saman Shahinpour, and we're here for some Real Data Science Projects that would be introduced in several posts.

So let's start one project at this time.

This project, I call it "Brokerage-ir", is working on Tehran Stock Exchange website and gets its data like general index, and so on.

We use web scraping techniques to build our web application on [Flask](https://flask.palletsprojects.com/) framework.

Collecting Data as one of the earliest steps in Data Science process:

We used web scraping techniques in this application to get the data from other sources, named other websites.

You can find a more detailed explanation of steps in Data Science process and collection data in another post.

As I just mentioned, I built the application on Flask framework so a short all-in-one guide for this framework can be found in another post, so I pass the details about coding (just a little for running our project) and go straight to the DS part of that.

To create a website in Flask just follow these steps:

   0. after installing latest version of python

   1. create an virtual environment (venv) using command prompt (cmd):

    python -m venv venv_name

    venv_name is your project name

   2. activate the venv:

    venv_name\Scripts\activate

   3. Install the flask within the venv area:

    pip install Flask

   4. running the flask app by:

    flask run

   5. enter this line in address bar at your browser:

    http://127.0.0.1:5000/

Now you have a simple web page (i.e. web application) in Flask running on your Browser. Go to the venv_name folder you just created.

As you can see there are some files and folders, but from no on, we're just working on two files: app.py and index.html

We started with a folder named template (this name "template" is mandatory as Flask is looking for some folder like this) and simple html file named "index.html" in that folder. We just used bootstrap templates, found here, with a little bit modification to get what we want.

It's up to you to choose an appropriate template or use our code and put it somewhere you like.

We just need this part in navbar div (consider {{ g_idx }} part in the code below that we scrap its value from Tehran Stock Exchange website):

    <div class="navbar navbar-expand-lg navbar-dark text-center">

        <div class="container">

            <div class="container text-center">

                <a class="navbar-brand" href="#">BROKERAGE-IR</a>

            </div>

            <div class="container bg-top-1 text-center">

                <table>

                    <tbody>

                        <tr><td class="top-title">General Index</td></tr>

                        <tr><td id="general-idx" class="down-title">{{ g_idx }}</td></tr>

                    </tbody>

                </table>

            </div>

        </div>

    </div>

Now we start working on app.py as the core logic center of the application. (like Controller in MVC data flow or js script file in web application)

We're starting by importing some packages that we need.

    from flask import Flask, render_template

    import requests

    from bs4 import BeautifulSoup

    import time

    import threading

Notes:

We use render_template package to read our index.html file and run it as our home page.

Because we used Beautiful Soup package for scrapig data from web, (for other packages see another post). You should first install the package on your project folder (venv_name):

    pip install requests bs4

We use time and threading to repeat the application (scraping data from the web) to make our web application dynamic.

we keep the rest but for routing, replace these lines as needed:

I put comments between codes so it's self explanatory.

    app = Flask(__name__)

    Each html page must have a function (def...) in app.py, that we called check_price() function from there.

    @app.route('/index')

    @app.route('/index/g_idx')

    def index(title=None):

        #calling check_price() function from here
    
        return check_price()

    def check_price():

We set the timer to start (precisely saying, to restart) the function

        threading.Timer(5.0, check_price).start()

or threading.Timer(5.0, app).run()

setting the source (URL)

        URL = 'http://www.tsetmc.com/Loader.aspx?ParTree=15'

search "my user-agent" on google and click on first link, then copy the information here as headers

    headers={"user-agent": 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0'}

    page = requests.get(URL, headers=headers)

    soup = BeautifulSoup(page.content, 'html.parser')

press F12 in your source URL and find the css or html tag that you're looking for

in my case the third td tag of the id="TseTab1Elm" is that we're looking for

    g_idx = soup.find(id="TseTab1Elm")

    g_idx = g_idx.find_all('td')

    # third td tag of id="TseTab1Elm"

    g_idx = g_idx[3].get_text()

    # calling the html file from template folder and returning the data to it

    return render_template('index.html', g_idx=g_idx)

And finally:

    if __name__ == '__main__':

    app.run()

So you'll have a div that each 5 seconds, being refreshed and shows the last general index of Tehran Stock Exchange.

Have any question or project to do, Contact me via email (saman_shahin@yahoo.com).

# A Newس Agency project

Continued from previous post called "Scraping The Web Content By Python"...

In this project, we're going to scrap economy-related news headlines from [a news website](https://www.mehrnews.com/service/Economy).

This project is a little tricky! Because we're looking for tags with a specific class name (not id as discussed in previous post):

We used this function as our scraping method (for more details and comments on the code, please refer to precious post.) :

    def get_news_from_eq():
    URL = 'https://www.mehrnews.com/service/Economy'
    headers={"user-agent": 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0'}
    page = requests.get(URL, headers=headers)
    soup = BeautifulSoup(page.content, 'html.parser')
    m_news = soup.findAll("li", { "class" : "news" })
    return m_news
    
Have Fun!
