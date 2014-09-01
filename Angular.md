Angular Best Practices
======================

Course Introduction
-------------------

### Course Agenda
* Introduction
* Organizing Your Code
* Writing Controllers and Services
* Writing Directives and Views
* Dealing with Scope
* Communication Between Components
* Breaking Down a Page Into Components
* Communication with Server
* Dealing with Models

###Prerequisites
* AngularJS Foundation
* node

### Course Conventions
* Best Practices
* Deep Dives
* Quizzes

### Demos
* node web-server.js
* http://localhost:8000/
* github.com/joeeames/AngularBestPracticesFiles

### Tools
* WebStorm
* Chrome

Organization your code
------------

### Module Agenda
* File & Folder Organization
* Module Organization
* Naming

### File Organization

* Angular Seed
    
    * App
    * App/js
    * App/js/controllers.js
    * App/js/services.js
    * App/js/filters.js
    * App/js/directives.js
    * App/css
    * App/partials
    * App/img
    * App/lib
    
    
    * App
    * App/js
    * App/js/controller1.js
    * App/js/controller2.js
    * App/js/service1.js
    * App/js/service2.js
    * etc ...
    * App/css
    * App/partials
    * App/img
    * App/lib
    
    * App
    * App/js
    * App/js/controllers/controller1.js
    * App/js/controllers/controller2.js
    * App/js/services/service1.js
    * App/js/services/service2.js
    * App/filters/filter1.js
    * App/directives/directive1.js
    * App/directives/directive2.js
    * App/css
    * App/partials
    * App/partials/view1.html
    * App/partials/directive2View.html
    * App/img
    * App/lib
        
    * App
    * App/authentication
    * App/authentication/authCtrl.js
    * App/authentication/authSvc.js
    * App/authentication/authDirective.js
    * App/authentication/authDirective.html
    * App/authentication/auth.html
    * App/authentication/authFilter.js
    * App/schedule
    * App/schedule/scheduleCtrl.js
    * App/schedule/scheduleSvc.js
    * App/schedule/schedule.html
    * App/css ...
    