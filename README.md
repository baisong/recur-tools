# recur-tools
Generate ICS files for calendar import using advanced recurrence rules unavailable in Thunderbird or Google Calendar

## purpose

Consider the following excerpt from [Internet Calendaring and Scheduling Core Object Specification (iCalendar) - Section 3.8.5.3 (Recurrence Rule)](https://datatracker.ietf.org/doc/html/rfc5545#section-3.8.5.3)

      The first Saturday that follows the first Sunday of the month,
      forever:

       DTSTART;TZID=America/New_York:19970913T090000
       RRULE:FREQ=MONTHLY;BYDAY=SA;BYMONTHDAY=7,8,9,10,11,12,13

       ==> (1997 9:00 AM EDT) September 13;October 11
           (1997 9:00 AM EST) November 8;December 13
           (1998 9:00 AM EST) January 10;February 7;March 7
           (1998 9:00 AM EDT) April 11;May 9;June 13...
           ...

      Every 4 years, the first Tuesday after a Monday in November,
      forever (U.S. Presidential Election day):

       DTSTART;TZID=America/New_York:19961105T090000
       RRULE:FREQ=YEARLY;INTERVAL=4;BYMONTH=11;BYDAY=TU;
        BYMONTHDAY=2,3,4,5,6,7,8

        ==> (1996 9:00 AM EST) November 5
            (2000 9:00 AM EST) November 7
            (2004 9:00 AM EST) November 2
            ...

There are even more examples below, but the two above are both useful and can't be accomplished in the Thunderbird or Google calendar event edit forms as recurrence rules.

Another simple (but usually impossible to create) example is an event to occur "the day before the 3rd Tuesday of the month". The closest thing would be "The third Monday", but this will be one week late for about one in every 7 months (in those months that begin on a Tuesday, the third Monday occurs 6 days after the third Tuesday).

The idea would be to create a web interface that generates an ICS file that can be imported into any calendar that accepts the ICAL format, supporting at least these kinds of annual and monthly advanced recurrence rules.

## more

Additional even more advanced recurrence rules:

      The third instance into the month of one of Tuesday, Wednesday, or
      Thursday, for the next 3 months:

       DTSTART;TZID=America/New_York:19970904T090000
       RRULE:FREQ=MONTHLY;COUNT=3;BYDAY=TU,WE,TH;BYSETPOS=3

       ==> (1997 9:00 AM EDT) September 4;October 7
           (1997 9:00 AM EST) November 6

      The second-to-last weekday of the month:

       DTSTART;TZID=America/New_York:19970929T090000
       RRULE:FREQ=MONTHLY;BYDAY=MO,TU,WE,TH,FR;BYSETPOS=-2

       ==> (1997 9:00 AM EDT) September 29
           (1997 9:00 AM EST) October 30;November 27;December 30
           (1998 9:00 AM EST) January 29;February 26;March 30
           ...

      Every 3 hours from 9:00 AM to 5:00 PM on a specific day:

       DTSTART;TZID=America/New_York:19970902T090000
       RRULE:FREQ=HOURLY;INTERVAL=3;UNTIL=19970902T170000Z

       ==> (September 2, 1997 EDT) 09:00,12:00,15:00

      Every 15 minutes for 6 occurrences:

       DTSTART;TZID=America/New_York:19970902T090000
       RRULE:FREQ=MINUTELY;INTERVAL=15;COUNT=6

       ==> (September 2, 1997 EDT) 09:00,09:15,09:30,09:45,10:00,10:15

      Every hour and a half for 4 occurrences:

       DTSTART;TZID=America/New_York:19970902T090000
       RRULE:FREQ=MINUTELY;INTERVAL=90;COUNT=4

       ==> (September 2, 1997 EDT) 09:00,10:30;12:00;13:30

      Every 20 minutes from 9:00 AM to 4:40 PM every day:

       DTSTART;TZID=America/New_York:19970902T090000
       RRULE:FREQ=DAILY;BYHOUR=9,10,11,12,13,14,15,16;BYMINUTE=0,20,40
       or
       RRULE:FREQ=MINUTELY;INTERVAL=20;BYHOUR=9,10,11,12,13,14,15,16

       ==> (September 2, 1997 EDT) 9:00,9:20,9:40,10:00,10:20,
                                   ... 16:00,16:20,16:40
           (September 3, 1997 EDT) 9:00,9:20,9:40,10:00,10:20,
                                   ...16:00,16:20,16:40
           ...

      An example where the days generated makes a difference because of
      WKST:

       DTSTART;TZID=America/New_York:19970805T090000
       RRULE:FREQ=WEEKLY;INTERVAL=2;COUNT=4;BYDAY=TU,SU;WKST=MO

       ==> (1997 EDT) August 5,10,19,24

      changing only WKST from MO to SU, yields different results...

       DTSTART;TZID=America/New_York:19970805T090000
       RRULE:FREQ=WEEKLY;INTERVAL=2;COUNT=4;BYDAY=TU,SU;WKST=SU

       ==> (1997 EDT) August 5,17,19,31

      An example where an invalid date (i.e., February 30) is ignored.

       DTSTART;TZID=America/New_York:20070115T090000
       RRULE:FREQ=MONTHLY;BYMONTHDAY=15,30;COUNT=5

       ==> (2007 EST) January 15,30
           (2007 EST) February 15
           (2007 EDT) March 15,30
