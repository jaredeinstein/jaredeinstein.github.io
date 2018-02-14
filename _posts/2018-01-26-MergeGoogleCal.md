---
title: Merging Google Calendars
---

Original article about [Merging Google Calendars](http://techingthetech.blogspot.com/2015/04/merging-google-calendars.html)

This references and based off this original [Article](http://blog.debsankha.net/2011/01/merging-two-google-calendars.html?showComment=1427776531017#c1574343446529575575)


Here is the transcript of each page in case the links break:


## Merging Google Calendars

I have been using Google Calendar for many years now. At first it was just a replacement for my Palm PDA. Overtime I have grown to love the features it offers. It syncs with all of my devices, it has very flexible recurring event rules, I can coordinate a time to meet with a group of people, and my wife and I can see each others schedules automatically.

The last two features are problematic since they only work with your main calendar. Secondary calendars are not included when looking at other's schedules. This always seemed like a huge oversight but going on four years now that Google hasn't fixed it and I am loosing hope that they will.

I have three calendars that I primarily use for different types of events. I prefer to keep them separate. I tried combining them and only using one but I missed the color coding that separate calendars provides. Plus if I needed to give someone my schedule I could choose what events to show them. My wife however needed to see all of them. Instead of sharing each individual calendar I wanted them all on one.

In addition I couldn't use the automatic meeting time recommendation feature that Google has unless all my events were on one calendar and that calendar was the primary calendar. (The primary calendar is the one that you have when you first create the account and can't be deleted.)

Fortunately Google gives us the tools to solve a lot of our own problems. It is amazing what can be done when you have API access to an online service.

Google offers a feature called Google Apps Script. It is essentially a way of running JavaScript code on Google's servers with access to all of your Google services and other third party services that you connect to. Since it runs on Google's servers you aren't depending on your own computer to be connected or have battery power.

I used to use an app on my phone to merge my secondary calendars into my main one. This worked very well, except that when I reformatted my phone I had to resetup the app. This was a pain. In addition it used my phone's resources which are limited. It didn't use much, but it did use some. It wasn't the most elegant solution. An online script seems like the best way to do this.

I found a blog post from 2011 by Debsankha called "Merging Two Google Calendars". He provided some code that would merge a secondary calendar into the primary. This was a start to what I needed. It was a simple matter to modify the code to accept multiple calendars. I also added the ability to delete events that you delete.

The code is shown below. Instructions are found below the code.

```javascript
 // This code was found at  
 // http://blog.debsankha.net/2011/01/merging-two-google-calendars.html?showComment=1427776531017#c1574343446529575575  
 // I modified it to work off an array of calendar ids.  
   
   
 function sync() {  
   
  var ids = [  
       "fjklk930acs020fm9f8sadvjk2@group.calendar.google.com",  
       "ppei902cnkla229vkdoiwoq90d@group.calendar.google.com",  
       "fji390zlkvrise782njkdklv94@group.calendar.google.com"  
       ]; //ids of the secondary calendars  
   
  for (i in ids)  
  {  
   var id = ids[i];  
     
   var cal=CalendarApp.getCalendarById(id);  
   var today=new Date();  
   var enddate=new Date();  
   enddate.setDate(today.getDate()+30); // only merges out 30 days, and only future events (makes sense)  
   var events=cal.getEvents(today,enddate); // holds the events to be merged  
    
   var mycal=CalendarApp.getDefaultCalendar();  
     
   var myevs=mycal.getEvents(today,enddate);  
     
   var stat=1;  
   var evi, myev;  
   var ti;  
     
     
   for (ev in events)  
   {  
    stat=1;  
    evi=events[ev];  
      
    // To remove events deleted from the primary calendar do this in reverse  
    // this loop checks each event in myevs against evi to see if they are the same  
    // if evi exists in myevs it does nothing ("continue")  
    // have a second loop that checks each event in events against myev to see if they are the same  
    // if myev does not exist in events delete it  
    // will have to have myevs rotate through the array of ids to check all calendars  
    for (myev in myevs)  
     {  
            
      if ((myevs[myev].getStartTime().getTime()==evi.getStartTime().getTime()) && (myevs[myev].getEndTime().getTime()==evi.getEndTime().getTime()))  
      {  
      // Browser.msgBox("ignoring common");  
       stat=0;  
       break;  
      }  
     }  
      
    if (stat==0) continue;      
          
    if (evi.isAllDayEvent())  
    {  
     ti=evi.getStartTime();  
     ti.setDate(ti.getDate()+2);  
       
     mycal.createAllDayEvent(evi.getTitle(), ti, {location: evi.getLocation(), description: evi.getDescription()});   
        
    }  
    else  
    {  
     mycal.createEvent(evi.getTitle(),evi.getStartTime(),evi.getEndTime(), {location: evi.getLocation(), description: evi.getDescription()});  
       
    }  
    
   }  
  }  
    
  var calendars = new Array();  
    
  for (i in ids)  
  {  
   calendars[calendars.length] = CalendarApp.getCalendarById(ids[i]).getEvents(today, enddate);  
  }  
    
  for (myev in myevs)  
  {  
   var count = 0;  
   Utilities.sleep(1000);  
   stat = 1;  
   for (eventList in calendars)  
   {  
    for (ev in calendars[eventList])  
    {  
     count++;  
     if ((myevs[myev].getStartTime().getTime()==calendars[eventList][ev].getStartTime().getTime()) && (myevs[myev].getEndTime().getTime()==calendars[eventList][ev].getEndTime().getTime()))  
     {  
      stat = 0;  
     }  
    }  
   }  
     
   if (stat == 1)  
   {  
    myevs[myev].deleteEvent();  
   }  
  }  
    
  Logger.log("Last ran " + today);  
  //Browser.msgBox("added all");   
 }​  
```

How to use this code
First go to Google Drive and create a new Google Apps Script (it will be under New > More). Copy the code from above into this new script. 

On the eighth line you will see "vars ids = [". This identifies an array of ids for the calendars you want to merge into your default calendar. These are the source calendars, while the default is the destination. Below this line you will see three lines that hold what looks like gibberish ending in "@group.calendar.google.com" highlighted in blue. These are the addresses of those calendars. You can have as many as you want, just put them in a separate lines surrounded by quotation marks with a comma at the end of each one except the last.


To get the calendar addresses go to Google Calendar and click the arrow next to the calendar you want. Select Calendar Settings.

In the settings there is a section called Calendar Address. The Calendar ID is what you want (I blacked mine out just because.) Copy this to the clipboard.

If you accidentally clicked on your primary calendar there will not be a calendar ID available.


 Now paste the ID over the one in the code. You can delete the other calendars if you want or add more, just be sure to have the Calendar ID in quotation marks and a comma after all but the last one.

For the last step, in the Google Apps Script page go to Resources > Current Project's Triggers. Click the link to add a new trigger. Select "sync", "Time-driven", then the time period you want it to run (I chose every hour). Save the triggers then save the file and you are set. From now on your default calendar will be updated with your secondary calendars you have selected every hour.



##Merging Two Google Calendars

I regularly use Mail for Exchange to sync my phone with my Google Calendar account. It's awfully useful but there's one catch : you can't set it up to sync with two google calendars. It'll always sync only your primary calendar.
Now, I have a personal calendar with entries for my classes etc. Apart from that, I have also subscribed to another public calendar for keeping track of the activities organized by various clubs of IISER Kolkata. But the default-calendar-only policy of Mail for Exchange meant I was unable to get the second calendar to sync with my phone.

I googled for finding some solution for the issue, but it didn't yield any useful information. I had almost given up when I came across a not so well-known service from google called Google Apps Script. As the name suggests, It lets you run scripts written in JavaScript on google's server and the script can access many of google's popular services, including calendar.

That solved my problem completely. All I needed was a script which will add all the events of the second calendar to the primary calendar, with some background checks to avoid adding duplicate entries. That was pretty easy: as per google's standards, the documentation was excellent, with ample examples too. So I had a working version of the script ready within very little time.

```javascript
function sync() {

  var id="i0oa77ome1sh58gf5tq0ke9teg@group.calendar.google.com"; //id of the secondary calendar
      
  var cal=CalendarApp.getCalendarById(id);
  var today=new Date();
  var enddate=new Date();
  enddate.setDate(today.getDate()+30);
  var events=cal.getEvents(today,enddate);

  var mycal=CalendarApp.getDefaultCalendar();
  
  var myevs=mycal.getEvents(today,enddate);
  
  var stat=1;
  var evi, myev;
  var ti;
  
 
  for (ev in events)
  {
    stat=1;
    evi=events[ev];
    
    for (myev in myevs)
      {
                
        if ((myevs[myev].getStartTime().getTime()==evi.getStartTime().getTime()) && (myevs[myev].getEndTime().getTime()==evi.getEndTime().getTime()))
        {
        //  Browser.msgBox("ignoring common");
          stat=0;
          break;
        }
      }
    
    if (stat==0) continue;        
            
    if (evi.isAllDayEvent())
    {
      ti=evi.getStartTime();
      ti.setDate(ti.getDate()+2);
      
      mycal.createAllDayEvent(evi.getTitle(), ti, {location: evi.getLocation(), description: evi.getDescription()}); 
        
    }
    else
    {
      mycal.createEvent(evi.getTitle(),evi.getStartTime(),evi.getEndTime(), {location: evi.getLocation(), description: evi.getDescription()});
      
    }

  }
  Browser.msgBox("added all"); 
}​
```

You can add triggers for the script as well. I added a daily trigger, so it will run once a day and sync the secondary calendar. Problem solved!
