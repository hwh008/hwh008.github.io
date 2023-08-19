---

layout: post
title: 'Design of nocode/low-code framework'
date: 2023-08-19
author: huang
cover: 'http://placeimg.com/1300/400'
tags: nocode lowcode

---

Nocode, in my opinion, is a user interface gateway that provides the ability to create pages for manipulating data tables, to establish workflow processes, and to trigger true business functions in the backend, all without the need for coding.


Let's imagine a scenario: It starts with a page containing a grid that reflects a table in the backend database. You input data into some rows and click a submit button to send the data to the next person. The person reviews the data, checks it, and clicks a run button, which calls an API on another backend server. The API performs some calculations and generates something new.

**i.**

In common project development, we often bundle various components together into what we call an application. The Nocode framework supports applications, which are collections that include pages, menu items with functions, table resources, configure strings, and schedules.I would categorize them into three types:

1. modules.
2. resources.
3. schedules.

**ii.**

A module in the nocode framework is represented as a menu item in the sidebar menu tree. When you click on a menu item, it opens a corresponding page. On that page,  when you click a button, for example, it sends a POST request to another module, which performs a specific action.


Both activities are defined as calling a module. Modules can be displayed as menu items or hidden behide UI event, and they are compatible with an authorization mechanism. Before calling a module, you need to assign it to specific roles.


modules do the hard work using various components, each component handling a different job such as inserting data or displaying a page.

**iii.**

A resource is a part of an application that provides the essential configuration that the application requires to run. It includes table definitions and string maps. When deploying an application from development to production, it is necessary to rebuild the tables and configure options specific to the target environment.

1. In table definition, you set column names, types, visibility, editability, importability, and value ranges. This definition is used to create a table in the database or to create a properly functioning grid on the page.

2. The string map is a set of options that can be used for different deployments, such as URLs or other configuration settings.

**iv.**

A scheduler is simply a timer that defines background tasks that are requested to run at a particular time. As we mentioned above, the tasks are modules.


Therefore,  modules in an application can be divided into three parts:  pages(to menu items), APIs, and  schedules.

**v.**

Next, we will step into the design of pages and components, which handle the actual work.


A page is a specific component in a module that generates a user interface. To distinguish components from modules, I refer to the components on a page as elements. A page commonly includes these elements: windows, form inputs, grids, toolbars, and buttons.


The grid is a functioning data table that is bound to a table definition resource. It is used to display data, insert, update, and delete rows, import and export data, and relatively connect to different modules that utilize components to complete these tasks.


Buttons are the most flexible part of the user interface, and you can configure button options to make them call different modules with different data, which can be obtained from window form inputs, selected rows in the grid, and toolbar inputs. Button options require three parameters:

1. Operation: Select a value from a predefined list of functions that manage UI elements.
2. Elements: Assign elements to be handled and draw data from them.
3. Module ID: Set the module that will be called with the data.

**vi.**

A component is a predefined code block that is run on the server side and is a significant part of nocode. The framework includes these basic components:

1. Executing a SQL query and providing the resulting data to grids, combo inputs, and other elements.
2. Updating a table in the database when the grid has been modified or a form has been submitted.
3. File upload and download.
4. Grid data import and export.
5. Calling remote APIs, with all data sent from the UI. This is an excellent component, as it is the key point that enables our nocode framework to function as an API gateway and extend its functionality widely.

**vii.**

When deploying an application on this framework, which has been packaged from another environment, you need to complete the following three steps:

1. Set configuration options in the string map.
2. Create tables in the database from the table definitions, if necessary.
3. Authorize access, assigning modules to roles/RBAC.

This is my main concept for the design of the NoCode framework, based on my understanding and deeply influenced by [webbuilder](putdb.com).
