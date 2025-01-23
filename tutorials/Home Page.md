# Home Page
The Home page is designed to provide a snapshot overview of Key Performance Indicators for based on the user type. The Home page is customizable to set different Home Pages for each user type in order to dispay the most relevant KPIs. 

## Accessing the Home Page
1. Go to the Assure Fintech portal and log in to your account
2. The default landing page upon logging in will be the Home Page.
3. Alternatively, select the *Home* page from the main menu bar.

## Types of Home Pages
The system offers 3 types of home pages for the primary user types. The type of Home Page is assigned to each account group using the [Account Groups feature](https://github.com/LatroServices/test.github.io/blob/Tutorials-Web-App/Admin.md#admin-account-groups)
1. Executive - Core mobile money business KPIs 
2. Analyst - Control & Rule based KPIs
3. IT - System health monitoring KPIs

### Executive Home Page
The following section includes the definitions and calculations for all figures:
**Tiles:**
- *Total Cash Flow* = (Total Inward + Total Outward) for the period from [(current date) to (current date - 7 days)]
- *Total Inward Flow* = (Total Inward direction) for the period from [(current date) to (current date - 7 days)]
- *Total Outward Flow* = (Total Outward direction) for the period from [(current date) to (current date - 7 days)]

**Graphs:**
- *Float Balance Over Time* = (Total Cash Flow = Total Inward + Total Outward )for each day.
- *Inward vs Outward Float* = (Total Inward and Total Outward)for each day
- *Transaction Amount Breakdown* = [(Total Inward Amount + Total Outward Amount) Grouped by Transaction Type]/ [(Total Inward + Total Outward) for all transaction types] x 100

**Current Alarm Status:**
These counters are updated in real-time as the Alarm Status is changed. 
- *Open* = Corresponds to count of all alarms with *Open Status* on the *Alarms Event List Page*
- *In-Progress* = Corresponds to count of all alarms with *In Progress Status* on the *Alarms Event List Page*
- *Closed* = Corresponds to count of all alarms with *Closed Status* on the *Alarms Event List Page*

### Analyst Home Page
*Coming Soon*

### IT Home Page
*Coming Soon*
