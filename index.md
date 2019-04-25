# Table of contents

* [Useful Links](#useful-links)
* [Our Goals](#our-goals)
* [User Guide](#user-guide)
* [Installation](#installation)
* [Application design](#application-design)
  * [Directory structure](#directory-structure)
  * [Import conventions](#import-conventions)
  * [Naming conventions](#naming-conventions)
  * [Data model](#data-model)
  * [CSS](#css)
  * [Configuration](#configuration)
  * [Quality Assurance](#quality-assurance)
    * [ESLint](#eslint)
* [Our Milestones](#our-milestones)
* [FoodieCravings Website](#foodiecravings-website)
 * [Milestone development](#milestone-development)
  * [Milestone 1: Mockup development](#milestone-1-mockup-development)
  * [Milestone 2: Improve Functionality](#milestone-2)


## useful links
[FoodieCravings App](http://foodiecravings.meteorapp.com/#/)

[Current Milestone](https://github.com/foodiecravings/foodiecravings/projects/2)

[Project Board](https://github.com/foodiecravings/foodiecravings/projects)

[GitHub Organization](https://github.com/foodiecravings)

## Our Goals
We hope to create a website that can help everyone feed their cravings. Our website will be able to collect
information from campus restaurants. Some information that we plan to share with our users is the location, the 
hours of operation, some visual aids, the price of the food, and the ability to add ratings for others to see.

## User Guide
Navigate over to our [website](http://foodiecravings.meteorapp.com/#/) and you will arrive at our landing page.
![](images/landing.PNG)
From here you will need to set up an account. You can do this by clicking the login button at the very top right 
and then selecting sign up. 
![](images/sign_up.png)

After entering your information you will now have full access to our website! 

You can add new ratings by navigating to the "Add Food" tab on top. Here we will ask you for your review of the food. 
![](images/add_food.png)
There is also a page where you can search for food, this is under the "List Food" tab. 

The last special feature is your profile page. Here is where other reviews can get to know a little bit more about you! 
![](images/profile.png)

# Installation

First, [install Meteor](https://www.meteor.com/install).

Second, [download a copy of FoodieCravings](https://github.com/foodiecravings/foodiecravings.git), or clone it using git.
  
Third, cd into the app/ directory and install libraries with:

```
$ meteor npm install
```

Fourth, run the system with:

```
$ meteor npm run start
```

If all goes well, the application will appear at [http://localhost:3000](http://localhost:3000). 

# Application Design

## Directory structure

The top-level directory structure contains:

```
app/        # holds the Meteor application sources
config/     # holds configuration files, such as settings.development.json
.gitignore  # don't commit IntelliJ project files, node_modules, and settings.production.json
```

This structure separates configuration files (such as the settings files) in the config/ directory from the actual Meteor application in the app/ directory.

The app/ directory has this top-level structure:

```
client/
  lib/           # holds Semantic UI files.
  head.html      # the <head>
  main.js        # import all the client-side html and js files. 

imports/
  api/           # Define collection processing code (client + server side)
    base/
    interest/
    profile/
  startup/       # Define code to run when system starts up (client-only, server-only)
    client/        
    server/        
  ui/
    components/  # templates that appear inside a page template.
    layouts/     # Layouts contain common elements to all pages (i.e. menubar and footer)
    pages/       # Pages are navigated to by FlowRouter routes.
    stylesheets/ # CSS customizations, if any.

node_modules/    # managed by Meteor

private/
  database/      # holds the JSON file used to initialize the database on startup.

public/          
  images/        # holds static images for landing page and predefined sample users.
  
server/
   main.js       # import all the server-side js files.
```

## Import conventions

This system adheres to the Meteor 1.4 guideline of putting all application code in the imports/ directory, and using client/main.js and server/main.js to import the code appropriate for the client and server in an appropriate order.

This system accomplishes client and server-side importing in a different manner than most Meteor sample applications. In this system, every imports/ subdirectory containing any Javascript or HTML files has a top-level index.js file that is responsible for importing all files in its associated directory.   

Then, client/main.js and server/main.js are responsible for importing all the directories containing code they need. For example, here is the contents of client/main.js:

```
import '/imports/startup/client';
import '/imports/ui/components/form-controls';
import '/imports/ui/components/directory';
import '/imports/ui/components/user';
import '/imports/ui/components/landing';
import '/imports/ui/layouts/directory';
import '/imports/ui/layouts/landing';
import '/imports/ui/layouts/shared';
import '/imports/ui/layouts/user';
import '/imports/ui/pages/directory';
import '/imports/ui/pages/filter';
import '/imports/ui/pages/landing';
import '/imports/ui/pages/user';
import '/imports/api/base';
import '/imports/api/profile';
import '/imports/api/interest';
import '/imports/ui/stylesheets/style.css';
```

Apart from the last line that imports style.css directly, the other lines all invoke the index.js file in the specified directory.

We use this approach to make it more simple to understand what code is loaded and in what order, and to simplify debugging when some code or templates do not appear to be loaded.  In our approach, there are only two places to look for top-level imports: the main.js files in client/ and server/, and the index.js files in import subdirectories. 

Note that this two-level import structure ensures that all code and templates are loaded, but does not ensure that the symbols needed in a given file are accessible.  So, for example, a symbol bound to a collection still needs to be imported into any file that references it. 
 
## Naming conventions

This system adopts the following naming conventions:

  * Files and directories are named in all lowercase, with words separated by hyphens. Example: accounts-config.js
  * "Global" Javascript variables (such as collections) are capitalized. Example: Profiles.
  * Other Javascript variables are camel-case. Example: collectionList.
  * Templates representing pages are capitalized, with words separated by underscores. Example: Directory_Page. The files for this template are lower case, with hyphens rather than underscore. Example: directory-page.html, directory-page.js.
  * Routes to pages are named the same as their corresponding page. Example: Directory_Page.


## Data model

The FoodieCravings data model is implemented by two Javascript classes: [FoodCollection](https://github.com/foodiecravings/foodiecravings/blob/master/app/imports/api/food/food.js) and [ReportCollection](https://github.com/foodiecravings/foodiecravings/blob/master/app/imports/api/report/report.js).
* Food collection is the data of food where users are able to view in the system.
* Report collection added where users may provide feedback to the admins.

## CSS

The application uses the [Semantic UI](http://semantic-ui.com/) CSS framework. To learn more about the Semantic UI theme integration with Meteor, see [Semantic-UI-Meteor](https://github.com/Semantic-Org/Semantic-UI-Meteor).

The Semantic UI theme files are located in [app/client/lib/semantic-ui](https://github.com/ics-software-engineering/meteor-application-template/tree/master/app/client/lib/semantic-ui) directory. Because they are located in the client/ directory and not the imports/ directory, they do not need to be explicitly imported to be loaded. (Meteor automatically loads all files into the client that are located in the client/ directory). 

## Configuration

The [config](https://github.com/foodiecravings/foodiecravings/tree/master/config) directory is intended to hold settings files.  The repository contains one file: [config/settings.development.json](https://github.com/foodiecravings/foodiecravings/blob/master/config/settings.development.json).

The [.gitignore](https://github.com/foodiecravings/foodiecravings/blob/master/.gitignore) file prevents a file named settings.production.json from being committed to the repository. So, if you are deploying the application, you can put settings in a file named settings.production.json and it will not be committed.

## Quality Assurance

### ESLint

FoodieCravings includes a [.eslintrc](https://github.com/foodiecravings/foodiecravings/blob/master/app/.eslintrc) file to define the coding style adhered to in this application. You can invoke ESLint from the command line as follows:

```
meteor npm run lint
```

ESLint should run without generating any errors.  

It's significantly easier to do development with ESLint integrated directly into your IDE (such as IntelliJ).

## Our Milestones
[Milestone 1](https://github.com/foodiecravings/foodiecravings/projects/1)

[Milestone 2](https://github.com/foodiecravings/foodiecravings/projects/2)

[milestone 3](https://github.com/foodiecravings/foodiecravings/projects/3)

## FoodieCravings Website

[FoodieCravings App](http://foodiecravings.meteorapp.com/#/)

# Milestone Development
## Milestone 1: Mockup development
This milestone started on April 4, 2019 and ended on April 11, 2019.

# The goal of milestone 1:

## Pages
- Create a fully functional [landing page](http://foodiecravings.meteorapp.com/#/) with working navbar and footer and allows users to know what our website is about
![](images/landing.PNG)
- Create a [contact us](http://foodiecravings.meteorapp.com/#/contact) page where users can provide feedback where admins may use to fix the website
![](images/contact_us.png)
- Add functionality to search bar on [landing page](http://foodiecravings.meteorapp.com/#/)
![](images/functional_search_bar.png)
- Add [log in](http://foodiecravings.meteorapp.com/#/signin) page to allow user to log in and add in their food
![](images/login_page.png)

Milestone 1 consisted of 10 issues

Each issue was implemented in its own branch, and merged into master when completed

![](images/m1.PNG)

## Milestone 2:
Our task for [milestone 2](https://github.com/foodiecravings/foodiecravings/projects/2)
![](images/m2.png)

## Milestone 3:
Our Task for [milestone 3](https://github.com/foodiecravings/foodiecravings/projects/3)
