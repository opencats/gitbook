# Screens In-Depth

[Using the software](README.md)

This page describes the main screens in OpenCATS 0.12.0. Bootstrap layouts adapt to screen width; use the control labels rather than older descriptions of colours or positions. Available actions also depend on your permissions.

### OpenCATS dashboard/home screen

Note

The Home dashboard groups recent activity, upcoming events, hires, Hiring Overview and Important Candidates in cards beneath the shared navigation.

* **The main OpenCATS module tabs:** The main navigation tabs.
* **Recent:** Your five most recently viewed candidates/contacts. Names are clickable for quick access.
* **Quick Search:** Search for candidate/contact name, job title, and Company name
* **My Recent Calls:** Most recent candidate/contact calls. Names are clickable for quick access.
* **My upcoming calls:** List of upcoming, scheduled calls, IF scheduled in OpenCATS.
* **My upcoming events:** Upcoming events scheduled in OpenCATS.
* **Recent Hires:** Short list of your company’s most recent hires
* **Hiring Overview:** Submission, interview and placement status-change events across four calendar weeks, months or years. Switch the period with the range controls. The current period is incomplete and repeat transitions may count more than once. The values table provides exact counts.
* **Important Candidates:** Small overview of some recent candidate activity. The columns in this section are adjustable by dragging the column title left or right.

### Activities Screen

The activities screen gives you an overview of recent candidate, company and contact activities.

Note

The rows at the top always stay the same in OpenCATS (Main navigation tabs, recent, and quick search). We won’t cover that again in the documentation.

* **Time-frame of results:** Click these to filter your results by day, week, month, etc.
* **Rows per page:** Click the drop-down bar to change the amount of results per screen. 15, 30, 50, or 100
* **Filter:** Options to filter results by Date, Regarding, Activity, Notes, Entered By
* **Activity columns:**
  * **Show Columns:** Select which columns to be visible in the activities grid
  * **Rearrange Columns:** Columns can be moved left and right by grabbing the column name and moving it left or right.
  * **Sort Columns:** Each column can also be sorted alphabetically by clicking the column title at the top of the column.

### Job Orders Screen

The Job Orders screen is job order specific. It is where all the job orders are.

Note

The job order screen is structured similarly to the Activities screen. So we won’t go through the same features again.

In the top, right, highlighted row, we have the following:

* **Active/On Hold/Full Drop-down box:** This allows you to filter your results based on the status of the job order. Options are: `Active/On Hold/Full`, `Active`, `On Hold/Full`, `Closed/Canceled`, `Upcoming/Lead`, `ALL`.
* **Only My Job Orders:** This will return results that are only your job orders.
* **Only Hot Job Orders:** This will return results that are only marked as Hot Jobs.

**Show Columns:**

There are quite a few options here to select or deselect. The checked options will include in the job order screen information. Use the column selector to inspect the available fields.

Note

As with the other screens, all columns can be moved left and right, as well as sorted alphabetically.

The last thing to note on the Job order screen is the action button in the bottom left corner.

Action:

* Clicking the action button will allow you to export results to a .csv sheet, or import results to a hot-list within the OpenCATS system. We will go into those further later in the documentation.

Add specified job orders to an OpenCATS Hot-list, or export them to a CSV:

* Select the checkbox (next to `action`), this will select all the boxes on this screen only.
* Manually select specified candidates
* Then `export` or `add to list` and click `selected`.

OR:

* select `action` then `add to list` or `export` and select `all` to include the entire database (in this case, it would include ALL of the job orders in your OpenCATS system) in your hot-list or CSV export.

### Candidates Screen

The main Candidates screen is laid out similarly to the others.

There are two checkboxes in upper right for filtering the results on this screen. `Only My Candidates` and `Only Hot Candidates`.

Again, there are additional options below that with the `Rows Per Page` dropdown and the `Filter` dropdown.

As before, the columns can be moved right and left by dragging the column title word at the top, and columns can be added or removed by the Show Columns (Image of a grid) in the upper left corner of the main section of the candidates screen. Use the column selector to inspect the available fields.

Depending on access level, the `Action` control offers different candidate actions. You can select specific candidates, the candidates showing on the screen, or all of the candidates in OpenCATS and `Add to List`, `Add To Pipeline`, `Send E-Mail`, or `Export`.

* **Add to List:** Add to a hotlist
* **Add To Pipeline:** Add to a job order pipeline
* **Send E-Mail:** Means type your own email to candidate(s) or use an OpenCATS template.
* **Export:** Export selected candidates to a CSV file. CSV exports through the export module require Site Administrator access in 0.12.0; ordinary recruiter accounts do not have this action.

CSV export actions described above use the administrator-only export module. Adding records to a list or pipeline remains subject to the relevant module permissions.

### Companies Screen

The main Companies page is very similar to the Candidates page. Everything functions the same, and it has the same options.

The only exception is that there are different columns available to choose in the `Show Columns` button. Use the column selector to inspect the available fields.

### Lists Screen

Hot lists, Lead Lists, tear sheets, call lists. Whatever you call them, most recruiters have used them at some point. OpenCATs has them here.

Use `Show Lists` to view lists. Membership is maintained manually; lists do not automatically refresh as a saved search would. Review older lists before using them for calls or mailings.

### Calendar Screen

The Calendar is a central hub for your OpenCATS usage. Every phone call and event, if scheduled through OpenCATS will appear here. The default Calendar screen will show a list of upcoming events in the left column and a Calendar overview on the right.

* `My Upcoming Events:` Shows all upcoming, scheduled events in the left column.
* `Add Event:` Add a new event to your calendar
* `Goto Today:` Shows you todays events
* Calendar date views can be changed by clicking: `Day`, `Week`, or `Month`.
* The Green Arrows can be clicked to move you back or forward on the Calendar.

Every Event box within the calendar is clickable for event information, which will appear in the left column.

Red Arrows:

* The first arrow points to a clickable icon that opens up the record of the Candidate included in the scheduled event.
* The middle red arrow points to an icon that indicates this is a “Public” event, which means it is viewable by any OpenCATS user.
* The third red arrow points to a clickable icon that opens up the record of the Contact included in the scheduled event.
* Lastly, every date number on the Calendar can be clicked on to view that date’s scheduled events.

Note

At this time, the OpenCATS calendar does not sync with outside calendars (Outlook, Google Calendar, etc.). However, that functionality CAN be added if you know a little coding, or a developer willing to do it.

### Reports Screen

The Reports screen displays counts for new Job Orders, Candidates, Companies, Contacts, Submissions and Placements across the available periods, plus totals to date. The submission and placement links open detailed reports.

Job-order pipeline and EEO charts use locally bundled Chart.js with tables of underlying values. EEO visibility depends on permissions and tracking configuration. Pipeline categories are cumulative and overlap, so they should not be added together as separate groups.

The job-order recruiting-summary PDF retains the report figures and notes, with a compact numeric summary replacing its former graph image.

### Settings Screen

There isn’t much to the settings screen.

In the upper left corner:

* `Administration` If you are the OpenCATS system administrator, this will take you to the administration screen.
* `My Profile` View your OpenCATS user profile or change your password.
* `Downloads` For now, this page serves no purpose. Disregard it completely.
